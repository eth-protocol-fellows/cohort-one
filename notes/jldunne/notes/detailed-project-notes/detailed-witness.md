# Detailed Witness Notes

## What is a witness?
Block processing can be modelled as a state transition function - 

`stf(state, block) = state'`

State is stored in a merkle tree, however only a small portion of the state is read/written each block.

The basic proposition of stateless, is that instead of the client holding the state, it would hold the state root. Therefore the new state transition function becomes:

`stf'(state_root, block, witness) = state_root'`

A witness is therefore the portions of the state that are read and modified, along with the proofs.

Witnesses are the pieces of merkle proofs that we would like to attach to blocks or transactions

## What is a Merkle Proof?

A Merkle proof is a subsection of a Merkle tree that can be used to prove that a given piece of data is part of a Merkle tree.

From my diagram, we would need the root hash, then some other pieces of the tree that we could hash with our piece of data. We would then compare that the hash we got is the same as the provided root hash i.e. the data is valid.

## How does this tie in with wit/0?
Wit/0 currently provides us with the following - 

Using the wit protocol, peers ask each other for the list of trie node hashes read during the execution of a particular block. This includes the following data:

- Storage nodes
- Bytecodes
- Account nodes
	- Read during EVM execution
	- Read during transaction validation
	- Read during block reward calculation
- Nodes read when generating the final state root (i.e. sometimes deleting data requires a trie refactor that reads nearby trie nodes)
- The trie node hashes which are generated at the end of the block from existing data are not included. For example, the final state root hash is not included.

## What does the py-evm version of `getBlockWitness` look like?

```
class MetaWitness(MetaWitnessAPI):
    def __init__(
            self,
            witness_hashes: Set[Hash32],
            accounts_metadata_queried: Dict[Address, AccountQueryTracker]) -> None:

        self._trie_node_hashes = frozenset(witness_hashes)
        self._accounts_metadata_queried = accounts_metadata_queried

    @property
    def hashes(self) -> FrozenSet[Hash32]:
        return self._trie_node_hashes

    @property
    def accounts_queried(self) -> FrozenSet[Address]:
        return frozenset(self._accounts_metadata_queried.keys())

    @property
    def account_bytecodes_queried(self) -> FrozenSet[Address]:
        return frozenset(
            address
            for address, query_tracker in self._accounts_metadata_queried.items()
            if query_tracker.did_query_bytecode
        )

    def get_slots_queried(self, address: Address) -> FrozenSet[int]:
        try:
            query_tracker = self._accounts_metadata_queried[address]
        except KeyError:
            return frozenset()
        else:
            return query_tracker.slots_queried

    @property
    def total_slots_queried(self) -> int:
        """
        Summed across all accounts, how many storage slots were queried?
        """
        return sum(
            len(query_tracker.slots_queried)
            for query_tracker in self._accounts_metadata_queried.values()
        )
```

## How would one prove a witness against an access list?

Stage 1.
Extend py-evm so that access lists as defined in EIP-3584 work, include tests

Stage 2.
How do we validate a witness against the accesslistroot? The accesslistroot is a hash of all the access lists. Given a block witness, we want to construct the accesss list root, and verify that they match.

Access list entry (from EIP) - [address, storage_keys, accesses_by_txn_index]

Extend witness in py-evm so that it captures information required to generate an access list, e.g. block transaction number, storage keys per block tx.

Stage 3.
Given a block number, call GetBlockWitness on it. This will return the updated witness. Figure out a reference implementation to verify that this witness corresponds to what is in the header for said block number.

Stage 4.
Extend access list construction to use whatever additional metadata is required by the witness proof, document.

Stage 5+. Repeat this process for Verkle trees if time allows.



### Useful Links
- [Terminology: What do we call "witness" in stateless Ethereum and why?](https://ethresear.ch/t/terminology-what-do-we-call-witness-in-stateless-ethereum-and-why-it-is-appropriate/7602)
- [Stateless Client Witnesses](https://vitalik.ca/files/misc_files/stateless_client_witnesses.pdf)
- [Primer on Merkle Trees](https://www.radixdlt.com/post/primer-on-merkle-trees)
- [What is a Merkle Tree? (nb very useful)](https://decentralizedthoughts.github.io/2020-12-22-what-is-a-merkle-tree/)
- [Witness Protocol](https://github.com/ethereum/devp2p/blob/master/caps/wit.md)
- [py-evm witness.py](https://github.com/ethereum/py-evm/blob/master/eth/db/witness.py)