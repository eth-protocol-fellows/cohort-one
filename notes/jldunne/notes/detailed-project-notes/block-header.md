# Block Headers - General Concepts

The block in Ethereum is the collection of relevant pieces of information (known as the block header) H, together with information corresponding to the comprised transactions, T, and a set of other block headers U that are known to have a parent equal to the present block's parent's parent (ommer blocks). 

The block header contains several pieces of information.

- parentHash: The Keccak 256-bit hash of the parent block's header, formally Hp
- ommersHash: The Keccak 256-bit hash of the ommers list portion of this block, formally Ho
- beneficiary: The 160-bit address to which all fees collected from the successful mining of this block should be transferred, Hc
- stateRoot: The Keccak 256-bit hash of the root node of the state trie, after all transactions in this block are executed and finalisations applied, Hr
- transactionsRoot: The Keccak 256-bit hash of the root node of the trie structure populated with each transaction in the transactions list portion of the block, formally Ht
- receiptsRoot: The Keccak 256-bit hash of the root node of the trie structure populated with the receipts of each transaction in the transactions list portion of the block, formally He
- logsBloom: the Bloom filter composed from indexable information (logger address and log topics) contained in each log entry from the receipt of each transaction in the transactions list, formally Hb
- difficulty: a scalar value corresponding to the difficulty level of this block. This can be calculated from the previous block's difficulty level and timestamp, formally Hd
- number: A scalar value equal to the number of ancestor blocks. The genesis block has a number of zero, formally Hi
- gasLimit: A scalar value equal to the current limit of gas expenditure per block; formally Hl
- gasUsed: A scalar value equal to the total gas used in transactions in this block, formally Hg
- timestamp: A scalar value equal to the reasonable output of Unix's time() at this block's inception; formally Hs
- extraData: An arbitrary byte array containing data relevant to this block. This must be 32 bytes or fewer, formally Hx.
- mixHash: A 256-bit hash which, combined with the nonce, proves that a sufficient amount of computation has been carried out on this block, formally Hm
- nonce: A 64-bit value which, combined with the mix hash, proves that a sufficient amount of computation has been carried out on this block, formally Hn.

The other two components in the block are simply a list of ommer block headers (Bu) and a series of the transactions (Bt).

Formally, we can refer to a block B as (Bh, Bt, Bu)

