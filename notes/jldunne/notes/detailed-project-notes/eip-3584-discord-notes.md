Q1. After going through EIP-2929, is our expection with the new EIP to mandate clients (miners) to compute `accessed_addresses_witness: Set[Address,EncodedWitnessTrieRoot]` and include its merkle/verkle root in the block header?

Or do we just want to make `accessed_addresses` trie root part of the block header?

A1. Right now, `accessed_addresses` is just a transaction-level thing.

For Verkle/Merkle trees, the goal would be to compute a full access list and include some kind of hash of it in the block header.

____

EIP-3548 will prove that the witness data for the block (as fetched using wit/0) is complete (i.e. that witness data has all of the accessed addresses and nothing is left out).

For the purposes of light clients sending and receiving, batching/requests for the witness data can be done using headers. 

We hope to evolve this into proof of full data completeness later.

This will also help clients preload these addresses for optimal computation of the block.

The general json-rpc spec can be found (here)[https://eth.wiki/json-rpc/API] - note to lene - find the new access list endpoints by checking in #json-rpc channel.

___

Q2. On the block level we could have `accessed_block_addresses: Set[Address,[AccessedInTransactionNumberList] ]`

This can be deterministically computed as a trie while serially builing the transactions, whose root can be embedded in the block header.

A2. Sets tend to be unordered. We need something to denote canonical ordering if we want to be able to produce a consistent 32.byte hash reference to the access list.

The concept of a set is probably good here, but the EIP will need to explicitly define the rules of "no item may appear in the list multiple times"

The EIP will need to define how a 32-byte hash is derived from the structure. Using an SSZ list would be a suggested starting point but I recommend exploring different options.

___

Q3. That's why I mentioned that a Merkle/Verkle Trie be built for each block starting from an empty root node - while transactions are executed one by one, the first access will in that particular transaction lead to it being added to the addresses leaf node, leaf node being the tuple of addresses to the ordered list of the tranasction number of this transaction in the block.

A3. You are proposing to use the Merkle/Verkle trie as the 32-byte hash generation mechanism, stick the witness pieces into a trie and use the root hash as the 32-byte reference in the header?
___

This ties the access list to our trie format and there is benefit of it being independent from that format.

Also, tries are complex data structures. It is probably much simpler to define the protocol rules around a list of things with a prescribed ordering and rules around not allowing dupllicates. Implementations can use "set" type data structures, but the protocol can stay agnostic as to how it is modelled internally,

Also, if we use a merkle trie, then later when we migrate to verkle, we also have to migrate header.access_list_root as well.

This conversation may also be relevant to beam sync work - working towards getting a field in the header that would allow you to verify the data before you download it.

___
Other people are also hesitant to make the access order of the trie node to be consensus sensitive. It is much simpler as a client to just collect all accessed nodes and sort them some other way, rather than always precisely agree on access order. 

For ordering, I see two choices - A: lexicographical (sorted by account -> storage slot -> code chunk index), B: order of access within a block.
___
On ordering of the list:

Whether the access list is valid or complete can only be known by fully executing the block (and its match with the root/hash in the block will become another constraint on the validity of the block). WRT griefing, is an easy root check.
___
Afer going over the implemented access lists, my proposed construction is:
`access_list = 
Set[Address, 
	List[AccessedStorageSlots],
	Set[	AccessedInBlockTransactionNumber, 			List[AccessedStorageSlots]
	]
]`

In order to include witness data/proofs, this could easily be extended to: 
`access_list = 
Set[Address, 
	List[AccessedStorageSlots], 
	List[AccessedWitnesses],
	Set[ 	AccessedInBlockTransactionNumber, 			List[AccessedStorageSlots], 				List[AccessedWitness]
	] 
]`

There is some data denormalization here for the purposes of building the canonical access document whose commitment/hash can be included in the block header.

Proposed sorting - 
`Set<SortedByAddress>[Address, 		List<Sorted>[AccessedStorageSlots],
Set<SortedByTransactionNumber>[AccessedInBlockTransactionNumber,
List<Sorted>[AccessedStorageSlots] ] ]`

This can easily be constructed from the transaction access lists that have already been generated on the miner side, and furthermore, those transaction access lists can easily be reconstructed from the above construction (on client side).

The above construction can also be used to construct a partial order (full accessed-by order is not needed) on the transactions (DFS) enabling (optional and client-dependent) parallel tranasction computation/optimizations/sharding, and an easy index on to the transactions in the block affecting the current address, as well as witness bundling in the future.

A merkle/verkle trie can be built for hashes/commitments with chunkified addresses in the leaf and the root embedded in the blockheader for integrity checks. If trie computation is too heavy, then a simple hash/commitment