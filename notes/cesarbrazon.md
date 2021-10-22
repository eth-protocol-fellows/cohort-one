# Cesar Brazon Notes

Understanding ethereum it's complex when you come at the technology for the first time, even tho there are a lot of resources, I feel that the Ethereum Protocol can be taughted the same way as the OSI model. This means that, we can separate the protocol into different layers in order to make it simpler.

The current way Ethereum nodes interact between them it's not the most efficient for different reasons:
- The nodes need to download the entire chain in order to become a validator
- To mine a block, all the nodes need to confirm the transactions

This makes ethereum not the fastest blockchain - but this will be improved 

## Objectives
- Get a better understanding of EVM mechanisms
- Understand the improvements that the community wants to achieve with ETH 2.0 and how do they work from a technical POV
- Improve knowledge on layer two solutions and eventually help on this development (Plasma, state channels, rollups)

## Areas of Interest
- EVM
- Sharding
- Roll ups
- zkSnarks

### Resources

- Understanding the Ethereum Protocol - https://www.youtube.com/watch?v=gjwr-7PgpN8
- Ethereum Illustrated - https://takenobu-hs.github.io/downloads/ethereum_evm_illustrated.pdf
- The limits to blockchain scalability - https://vitalik.ca/general/2021/05/23/scaling.html
- Shard chains - https://ethereum.org/en/eth2/shard-chains/
- EVM - https://ethereum.org/en/developers/docs/evm/
- Beacon chain - https://ethereum.org/en/eth2/beacon-chain/
- Rollup-centric roadmap - https://ethereum-magicians.org/t/a-rollup-centric-ethereum-roadmap/4698


- Validation focused Smart Contract Wallets - https://ethereum-magicians.org/t/validation-focused-smart-contract-wallets/6603
- EIP 3074 - https://eips.ethereum.org/EIPS/eip-3074
- Native meta transactions - https://ethresear.ch/t/native-meta-transaction-proposal-roundup/7525/5

#### AA
- https://blog.ethereum.org/2015/07/05/on-abstraction/
- EIP 101 - https://github.com/ethereum/EIPs/issues/28
- Implementing AA on Eth1.X https://ethereum-magicians.org/t/implementing-account-abstraction-as-part-of-eth1-x/4020/8  

## Questions

- What's the difference between precompilas and opcode?
- How can we interact with WASM through roll ups?


### Notes on AA

- With this EIP we want to add multiple improvements to the entire ethereum stack, which I think the two more importants are:
  - Native meta transactions, allowing the user to batch multiple send transactions at the protocol level (instead of using something like multisend for example)
  - The capicity of a contract to pay for gas, instead of having the EOA to pay for it every time