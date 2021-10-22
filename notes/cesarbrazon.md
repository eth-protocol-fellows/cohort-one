# Cesar Brazon Notes

## Understanding ethereum architecture

As in any complex software, the Ethereum Protocol is divided into different layers. In https://ethereum.org/en/developers/docs/ethereum-stack/ we can see an overview of the different pieces of the stack.

### EVM

### Consensus

- Nodes that wants to be miners needs to be have the entire history of Ethereum stored. Meaning that, when a new block is mined, **all** the nodes are updated with the new information.
This makes the Proof of Work consensus not the best option if we want Ethereum to be scalable. With Serenity (Eth 2.0) this problem will be solved with Casper

- **MEV** (Miner Extractable Value): When a miner gets the nonce to produce a new block into the chain, it has the ability to reorder the transactions.
This allow the miner to arbitrarily reorder transactions, insert its own transactions before or after other transactions, and delay transactions outright until the next block, and it turns out that there are a lot of ways that one can earn money from this; hence, making it a consensus-layer risk

## Objectives

- Give an overview of the ethereum architecture.
- Understand what are the benefits of the improvements in Eth2.0
- Describe the EVM in depth

## Areas of Interest

- EVM
- Sharding
- Roll ups
- zkSnarks

### Resources

- Understanding the Ethereum Protocol - https://www.youtube.com/watch?v=gjwr-7PgpN8
- Ethereum Illustrated - https://takenobu-hs.github.io/downloads/ethereum_evm_illustrated.pdf
- The limits to blockchain scalability - https://vitalik.ca/general/2021/05/23/scaling.html
- Ethereum stack - https://ethereum.org/en/developers/docs/ethereum-stack/
- Shard chains - https://ethereum.org/en/eth2/shard-chains/
- EVM - https://ethereum.org/en/developers/docs/evm/
- Beacon chain - https://ethereum.org/en/eth2/beacon-chain/
- Rollup-centric roadmap - https://ethereum-magicians.org/t/a-rollup-centric-ethereum-roadmap/4698
- Validation focused Smart Contract Wallets - https://ethereum-magicians.org/t/validation-focused-smart-contract-wallets/6603
- EIP 3074 - https://eips.ethereum.org/EIPS/eip-3074
- Native meta transactions - https://ethresear.ch/t/native-meta-transaction-proposal-roundup/7525/5

## General notes

- RLP is the main encoding method used to serialize objects in Ethereum

### Notes on AA

With this EIP we want to add multiple improvements to the entire ethereum stack, which I think the two more importants are:

- Native meta transactions, allowing the user to batch multiple send transactions at the protocol level (instead of using something like multisend for example)
- The capacity of a contract to pay for gas, instead of having the EOA to pay for it every time

Also, with the introduction of this EIP, we can add signature verification other than ECSDA which allows to make ethereum post-quantum resistant

In order to implement AA, we require merging EIP 2937 or preserve nonce on self destruct

A new type of transaction would be added, they are called "AA transactions" and the payload should be `rlp([nonce, target, data])`

The transaction would consists on two process:

1. Authentication, is the process where the caller can indeed call this contract. This will happen in an isolated environment, throwing if the authentication process try to access memory, or write storage. This process should be a pure function, or only interact with precompiled contracts
2. Execution, here, the opcode `PAYGAS` will be triggered at first, and will calculate the gas price and gas limit for the transaction. Note that if the transaction fails, the contract will not be refunded

`NONCE` and `PAYGAS` opcodes will be introduced with this EIP.

#### Resources on AA

- https://blog.ethereum.org/2015/07/05/on-abstraction/
- EIP 101 - https://github.com/ethereum/EIPs/issues/28
- Implementing AA on Eth1.X https://ethereum-magicians.org/t/implementing-account-abstraction-as-part-of-eth1-x/4020/8
- EIP 2938 - https://github.com/ethereum/EIPs/blob/master/EIPS/eip-2938.md
- Spending gas from contract's balance - https://github.com/ethereum/EIPs/issues/61

## Questions

Q: What's the difference between precompiles and opcode?
A: Precompiles are contracts which are embedded into clients with default addresses. Opcodes are instructions stored in the storage of the EVM, and it's what allow us to interact with the stack of the EVM.
We can say that precompiles are predefined bytecode, which allow us to create more complex logic (like `ecrecover`) and opcodes are native functions that allow us to interact directly with the EVM

Q: How can we interact with WASM through roll ups?
