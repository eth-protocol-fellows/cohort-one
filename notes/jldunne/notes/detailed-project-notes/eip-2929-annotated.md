# EIP-2929: Annotated

## Investigate the access list generation that clients are doing for EIP-2929

**EIP-2929: Gas cost increases for state access opcodes**

### Summary

The summary of this EIP is that gas costs will be increased for `SLOAD`, `*CALL`, `BALANCE`, `EXT*` and `SELFDESTRUCT`, when used for the first time in a transaction.
___
**How exactly does gas work?**

~ From Yellow Paper - https://ethereum.github.io/yellowpaper/paper.pdf~

Any given fragment of computation has a universally agreed upon cost in terms of gas. Every transaction has a specific amount of gas associated with it: the Gas Limit.

The gas limit of a transaction is the amount of gas that is implicitly purchased from the sender's account balance.

The purchase happens at the according gas price - also specified in the transaction. The transaction cannot be valid if the account balance cannot support such a purchase.

Transactors are free to specify any gas price that they wish, however miners are also free to ignore transactions. A higher gas price will be more likely to be selected for inclusion by more miners.

**What are SLOAD, CALL etc? Opcodes? What are opcodes? How do they work?**

See Page 30 of Ethereum Yellow Paper for list of Stop and Arithmetic Operations

SLOAD - Loads a word from storage
CALL - Message call into an account
DELEGATECALL - Message-call into this account with an alternative account's code, but persisting the current values for sender and value
STATICCALL - Static message-call into account
BALANCE - Get the balance of a given account
EXTCODESIZE - Get size of an account's code
EXTCODECOPY - Copy an account's code to memory
EXTCODEHASH - Get hash of account's code
SELFDESTRUCT - Halt execution and register the account for later deletion

The word is the mnemonic. The opcode is the number that specifies which operation should take place.

An opcode is the portion of the machine language instruction that specifies the operation to perform. Besides the opcode itself, most instructions also specify the data they will process  in the form of operands.

**What does the asterisk before or after the opcode mean?**

If it comes before, it means wildcard match before this mnemonic. After, then we wildcard match after. I think I have captured this pretty well above.
___
### Abstract

This EIP proposes to increase the gas cost of SLOAD to 2100, and \*CALL, BALANCE, and EXT* to 2600. Precompiles and addresses/storage slots that have already been accessed in the same transactions would be exempt. SSTORE and SELFDESTRUCT would be reformed to ensure that de-facto storage loads inherent in these operations are priced correctly.
___
**How much gas do these operations currently cost?**

SLOAD currently costs 800, and will now cost 2100. The call family and other operations currently cost 700 and will be increased to 2600.

**What is a pre-compile?**

A cryptographic primitive that is useful enough to be partially optimised, but not so useful or guaranteed to be there forever that it would be made into an opcode. These gas upgrades won't affect pre-compiles.

**What is a storage slot?**

A storage slot refers to the array that acts as a smart contract's storage.
___
### Motivation

The main function of gas costs of opcodes is to be an estimate of the time required to process that opcode. The gas limit should correspond to a limit on the time required to process a block. Howver, storage-accessing opcodes (above) have historically been underpriced. One of the most durably successful strategies used by the attacker in the Shanghai DoS attacks was to simply send tranasctions that access or call a large number of accounts.

In the past, gas costs were increased to mitigate this, but recent analysis suggests they weren't increased enough.

This EIP increases the cost of these opcodes by a factor of ~3, reducing the worst-case processing time to 7-27ish seconds.

Another benefit of this EIP is that it performs most of the work needed to make stateless witness sizes in Ethereum acceptable.

Assuming a switch to binary trees, the theoretical maximum witness size not including the code size would decrease from 14.3mb to 3.85mb.

In the future, there are similar benefits in the case of SNARK/STARK witnesses.
___
**How does increasing the cost of the opcode reduce the processing time?**
I think it is because a higher gas price will ensure that more miners will include this transaction for processing.

**What does a stateless witness size refer to?**
The size of the Merkle proofs that must be provided to help a stateless node verify a block

**What are STARK/SNARK witnesses? What is the difference between these and our current witnesses?**
This is just a different type of proof, and probably a rabbit hole that is not worth our while to go down right now.
___
### Specification

First, we must specify a number of parameters. 

- `FORK_BLOCK` is TBD
- `COLD_SLOAD_COST` is 2100
- `COLD_ACCOUNT_ACCESS_COST` is 2600
- `WARM_STORAGE_READ_COST` is 100

___

**Are the cold parameters existing already?**
No, it seems like these are introduced by this EIP

**What does the WARM\_STORAGE\_READ_COST refer to?**
This is the cost for accessing something that has already been accessed i.e. is in the access list. So, the high gas cost is paid once, and then it just costs 100 gas to access after that.
____
After the fork block, the following changes apply.

When executing a transaction, maintain a set of `accessed_addresses: Set[Address]` and `accessed_storage_keys: Set[Tuple[Address, Bytes32]]`
___
**What do these refer to?**
`accessed_addresses` refers to a set of addresses that has been the target of one of the named operations here (ext*, balance, call etc).

`accessed_storage_keys` refers to a set of pairs, each pair made up of the address of the contract whose storage is being read, and the storage key (which I think refers to the index being accessed in the contract storage)
____

The sets are transaction-context-wide, implemented identically to other transaction-scoped constructs such as the self-destruct-list and global refund counter. If a scope reverts, access lists should be in the state they were in before that scope was entered.

When a transaction execution begins, `accessed_storage_keys` is initialised to empty, and `accessed_addresses` is initialised to include the `tx.sender`, `tx.to` (or address being created) and the set of all precompiles.
___
**How are these two implemented?**
The self-destruct list is just an in-memory list in `computation.py` on `py-evm`.

**What does it mean for a scope to revert?**
I think this means if a transaction reverts, then the lists should be the same as they were prior to the transaction starting.

**What does it mean for a transaction execution to begin?**
If the transaction is deemed to be valid, it will be executed. This means that the gas is paid, sender balance is reduced, and the operations within the transaction are executed. This probably means that these lists will be done per transaction, rather than per block.
___
#### Storage read changes

When the address is the target of one of these opcodes, the gas costs are computed as follows - 

If the target is not in accessed addresses, charge COLD\_ACCOUNT\_ACCESS_COST and add the address to `accessed_addresses`

Otherwise, charge WARM\_STORAGE\_READ_COST

The idea is for the gas cost to be charged, and the map updated at the time the opcode is being called. 

When `CREATE*` is called, the address should also be added to `accessed_addresses`.

Note on `CREATE` - if one of these operations fails later on, or has insufficient gas to store the code in state, the address of the contract itself will remain in `access_addresses`

For SLOAD, if the (address, `storage_key`) pair (where address is the address of the contract whose storage is being read) is not yet in `accessed_storage_keys`, charge `COLD_SLOAD_COST` gas and add the pair to `accessed_storage_keys`. If the pair is already in `accessed_storage_keys`, charge `WARM_STORAGE_READ_COST` gas.

Note 2: There is currently no way to perform a ‘cold sload read/write’ on a ‘cold account’, simply because in order to read/write a slot, the execution must already be inside the account. Therefore, the behaviour of cold storage reads/writes on cold accounts is undefined as of this EIP.

#### SSTORE Changes

When calling SSTORE, check if the (address, storage_key) pair is in accessed_storage_keys. If it is not, charge an additional `COLD_SLOAD_COST` gas, and add the pair to accessed_storage_keys. 

Additionally modify the parameters defined in EIP-2200 as follows:
- SLOAD_GAS goes from 800 to `WARM_STORAGE_READ_COST`
- `SSTORE_RESET_GAS` goes from 5000 to 5000-`COLD_SLOAD_COST`

#### SELFDESTRUCT Changes

If the ETH recipient of a SELFDESTRUCT is not in `accessed_addresses` (regardless of whether or not the amount sent is nonzero), charge an additional `COLD_ACCOUNT_ACCESS_COST` on top of the existing gas costs, and add the ETH recipient to the set.

Note: SELFDESTRUCT does not charge a `WARM_STORAGE_READ_COST` in case the recipient is already warm, which differs from how the other call-variants work. The reasoning behind this is to keep the changes small, a SELFDESTRUCT already costs 5K and is a no-op if invoked more than once.

### Rationale

The the natural alternative path to changing gas costs to reflect witness sizes is to charge per byte of witness data. However, this would take a long time to implement.

Following this path faithfully would also lead to extremely high gas costs to transactions that touch contract code.

It is better to wait for code merklization to begin to try to properly account for gas costs.
___
**Why would this path lead to unacceptably high gas costs?**

See this article - https://medium.com/ewasm/evm-bytecode-merklization-2a8366ab0c90

If a contract touches contract code, it would require a witness for all of the chunks. We would therefore be charging for all of the chunks available - 24576.
___

The sets of already accessed accounts and storage slots are added to avoid needlessly charging for things that can be cached (and in all performant implementations already are cached). It also removes the undesirable current status quo where it is unaffordable to do self calls or call precompiles.

The change to SSTORE is needed to avoid the possibility of a DoS attack that "pokes" a randomly chosen zero storage slot, changing it from 0 to 0 at a cost of 800 gas, but requiring a de-facto storage load. The SSTORE_RESET_GAS reduction ensures that the total cost of SSTORE remains unchanged.

## Helpful Links
- [Merklization from EWASM team](https://medium.com/ewasm/evm-bytecode-merklization-2a8366ab0c90)
- [EIP-2929 Quick Explainer,Reddit](https://www.reddit.com/r/ethereum/comments/mrl5wg/a_quick_explanation_of_what_the_point_of_the_eip/)
- [Empirically Analyzing Ethereum's Gas Mechanism](https://arxiv.org/pdf/1905.00553.pdf)
- [Broken Metre - Attacking Resource Metering in EVM](https://arxiv.org/pdf/1909.07220.pdf)
- [Using zkSNARKs](https://cs251.stanford.edu/lectures/lecture16.pdf)
- [EIP-2929 Ethereum Magicians Thread](https://ethereum-magicians.org/t/eip-2929-gas-cost-increases-for-state-access-opcodes/4558/54)
- [Ethereum Stack Exchange post on storage keys](https://ethereum.stackexchange.com/questions/6397/how-do-i-get-the-storage-indices-keys)
- [Transaction Execution Walkthrough](https://dzone.com/articles/transaction-execution-ethereum-yellow-paper-walkth)
- [geth access lists prototype fork](https://github.com/holiman/go-ethereum/tree/access_lists)



