# Project Title - Removal of gas stipend for CALL opcodes

Whenever the caller calls an external function, it automatically receives 2300 gas to perform limited computations such as issuing a log. The fixed quantity of gas prevents malicious smart contracts from conducting a reentrancy attack that occurred in 2016. A reentrancy attack happens when an external function is called recursively by another smart contract. Such an attack was only possible until a gas limit of 2300 was introduced on all CALL opcodes, effectively preventing the attacker from draining all ETH from the smart contract. Changing the stipend value will require a hardfork because it is hardcoded in the EVM.

## 2300 Gas Stipend Usage
When either of the functions receive() or fallback() is executed, the contract relies on 2300 gas for the execution. The following operations will consume more gas than the 2300 gas stipend:
- Writing to storage
- Creating a contract
- Calling an external function which consumes a large amount of gas
- Sending Ether

## List of relevant opcodes
### CALL
- CALL(gas, addr, value, argsOffset, argsLength, retOffset, retLength) // calls a method in another contract
- CALLCODE(gas, addr, value, argsOffset, argsLength, retOffset, retLength) 
- DELEGATECALL(gas, addr, argsOffset, argsLength, retOffset, retLength) // EIP-7: calls a method in another contract, using the storage of the current contract
- STATICCALL(gas, addr, argsOffset, argsLength, retOffset, retLength) // EIP-214: calls a method in another contract with state changes such as contract creation, event emission, storage modification and contract destruction disallowed

### LOGS
- LOG0(offset, length) // fires an event
- LOG1(offset, length, topic0) // fires an event
- LOG2(offset, length, topic0, topic1) // fires an event
- LOG3(offset, length, topic0, topic1, topic2) // fires an event
- LOG4(offset, length, topic0, topic1, topic2, topic3) // fires an event

## Reasons for removing the gas stipend
![](https://i.imgur.com/BfpHnSw.png)


## Community Feedback
1) "Changing the EVM stipend is much harder than changing Solidity's bytecode generation because of backward compatibility. Even with the EVM having a 
stipend, the .transfer and .send recommendations are still bad ideas." - Micah (https://github.com/ethereum/solidity/issues/7455)

This provides confirmation that reducing the EVM stipend would be a bad idea because it would break backwards compatibility for legacy contracts. Hence, it would be optimal to increase the stipend or set its value to infinity.

2) "This limitation shall allow some basic logic to be executed like using LOG. However, when using the proxy pattern, there are only about 1,300 gas units left for the actual logic execution. This is barely enough for a simple LOG, and from my own experience, it was enough for one LOG with a single, non-indexed parameter (it might allow a bit more in some cases, but still not enough for proper logic)." - Ben Kaufman (https://ethereum-magicians.org/t/eip-1285-increase-gcallstipend-gas-in-the-call-opcode/941)

This EIP also signals why it would be a good idea to increase the gas stipend. 

3) "Add a condition to SSTORE that causes it to revert if less than 2300 gas remains.
- Straightforward.
- Makes the violated invariant (‘the gas stipend is not enough to change state’) explicit.
- Can be applied without the need to enable it only on a hardfork, since all past transactions meet this requirement (on mainnet, at least)." - Arachnid (https://ethereum-magicians.org/t/remediations-for-eip-1283-reentrancy-bug/2434)

Based on the positive feedback for the change described above, implementing such change would be optimal because it would not require a hardfork. By adding a condition to SSTORE and setting the gas stipend to infinity, reentrancy attacks can be prevented, backwards compatibility will not be impacted, and effectively the feature can be removed. 

4) "This would be a change in solidity? We can’t change contracts which are on the chain already, so we cannot simply change that any contract which forwards 2300 gas now suddenly forwards less." - Jochem Brouwer (https://ethereum-magicians.org/t/remediations-for-eip-1283-reentrancy-bug/2434/12)

This provides confirmation that setting the gas stipend to 0 will create errors in legacy contracts.

5) " I really like the proposal of adding a callback-safe transfer opcode because it allows the developer an additional option to explicitly reduce their attack surface so they can protect themselves if a particular protocol would have safety issues that need to be protected. Re-entrancy is probably one of the most complex bugs possible with smart contracts, and I think giving protocol-level tools to protect against unintended behaviors is important to provide as it will actually mitigate the problem instead of band-aiding it as the 2300 gas stipend does." - fubuloubu (https://ethereum-magicians.org/t/immutables-invariants-and-upgradability/2440/21)

Adding a new opcode in relation to the gas stipend could be a potential pathway as it removes the risk of incompatibility for legacy contracts.

## Todo tasks
- Revisit the Gas and Payment section in the yellow paper.
- Understand how the fallback function works in relation to the CALL opcodes.
- Understand how the stipend amount is calculated.

## Thoughts
Learn all CALL opcodes and conduct an analysis on their usage. 

## Resources
1) https://gitter.im/ethereum/AllCoreDevs?at=5d667f5ee403470ab6ef89ce
2) https://hackmd.io/@vbuterin/evm_feature_removing#The-2300-gas-stipend
3) https://medium.com/coinmonks/protect-your-solidity-smart-contracts-from-reentrancy-attacks-9972c3af7c21
4) https://ethereum.stackexchange.com/questions/70208/gas-is-0-when-executing-call-opcode
5) https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1706.md
6) https://eips.ethereum.org/EIPS/eip-1285
7) https://ethereum.stackexchange.com/questions/11237/execution-of-fallback-function-with-more-2300-gas
8) https://ethereum.stackexchange.com/questions/1686/ethereum-event-log-maximum-size
9) https://ethereum.stackexchange.com/questions/7978/whats-cheaper-contract-storage-log-data-transaction-input
10) https://ethereum.stackexchange.com/questions/3667/difference-between-call-callcode-and-delegatecall
11) https://ethereum-magicians.org/t/eip-1285-increase-gcallstipend-gas-in-the-call-opcode/941
12) https://ethereum-magicians.org/t/suggested-1884-issue-solution-stipend-context/3607
13) https://ethereum-magicians.org/t/immutables-invariants-and-upgradability/2440/21
