# Alex Chen Notes

## Objective
The purpose to join this program is to learn both in-depth knowledge of Ethererum core protocol and how to collaborate with other contributors in this community, so that I could have enough prior knowledge to particiapte in the developement and research activities of Ethereum core protocols in future.

## Areas of Interest

- EVM
- Statelessness & State Expiry

## EVM

### Resource

- [CoinCulture's Guide to the EVM](https://github.com/CoinCulture/evm-tools/blob/master/analysis/guide.md)
- Diving Into The Ethereum EVM [1](https://blog.qtum.org/diving-into-the-ethereum-vm-6e8d5d2f3c30), [2](https://medium.com/@hayeah/diving-into-the-ethereum-vm-part-2-storage-layout-bc5349cb11b7), [3](https://medium.com/@hayeah/diving-into-the-ethereum-vm-the-hidden-costs-of-arrays-28e119f04a9b), [4](https://medium.com/@hayeah/how-to-decipher-a-smart-contract-method-call-8ee980311603), [5](https://medium.com/@hayeah/diving-into-the-ethereum-vm-part-5-the-smart-contract-creation-process-cb7b6133b855), [6](https://blog.qtum.org/how-solidity-events-are-implemented-diving-into-the-ethereum-vm-part-6-30e07b3037b9)
- Understand EVM bytecode [1](https://blog.trustlook.com/understand-evm-bytecode-part-1/), [2](https://blog.trustlook.com/understand-evm-bytecode-part-2/), [3](https://blog.trustlook.com/understand-evm-bytecode-part-3/), [4](https://blog.trustlook.com/understand-evm-bytecode-part-4/)
- Executable specification for the ETH 1.0
  - [The proposal](https://ethereum-magicians.org/t/replace-the-yellow-paper-with-executable-markdown-specification/6430/6)
  - [The repository](https://github.com/ethereum/execution-specs)
### Potential Projects
- [List of EVM features we would like to remove](https://hackmd.io/@vbuterin/evm_feature_removing)
- [Static Analysis to predict access list](https://github.com/ethereum-cdap/cohort-one/issues/26)
- Help to improve [the assembly / disassembly tool etk](https://github.com/quilt/etk)
- Participate in the development of some EVM projects, such as [evm-rs](https://github.com/rust-blockchain/evm), [evmodin](https://github.com/vorot93/evmodin) and [nanoeth](https://github.com/norswap/nanoeth)
  

## Statelessness & State Expiry

### Resource
- Thanks to [@Norswap](https://github.com/norswap)’s  [State Expiry & Statelessness in Review](https://www.notion.so/State-Expiry-Statelessness-in-Review-8d531abcc2984babb9bf76a44459e611), helped to save a lot of time
- [The Winding Road to Functional Light Clients](https://snakecharmers.ethereum.org/the-winding-road-to-functional-light-clients/)
- [Why it's so important to go stateless](https://dankradfeist.de/ethereum/2021/02/14/why-stateless.html)
- [A few paths to statelessness and state expiry](https://hackmd.io/@vbuterin/state_expiry_paths)
- [A state expiry and statelessness roadmap](https://notes.ethereum.org/@vbuterin/verkle_and_state_expiry_proposal)
- [An Updated Roadmap for Stateless Ethereum](https://ethresear.ch/t/an-updated-roadmap-for-stateless-ethereum/9046)
- [State Expiry EIP](https://notes.ethereum.org/@vbuterin/state_expiry_eip)
- [Verkle Trie EIP](https://notes.ethereum.org/@vbuterin/verkle_tree_eip)
- [Address Space Expansion (ASE)](https://ethereum-magicians.org/t/increasing-address-size-from-20-to-32-bytes/5485)
- [ASE with Translation Map](https://notes.ethereum.org/@ipsilon/address-space-extension-exploration)
- [Kate commitments in ETH](https://hackmd.io/yqfI6OPlRZizv9yPaD-8IQ)
- [The portal network specs](https://github.com/ethereum/portal-network-specs)

### Potential Projects
- The rust portal client [trin](https://github.com/ethereum/trin),  but it's reasonably saturated with contributor
- [Portal client implementation in Geth](https://github.com/ethereum-cdap/cohort-one/issues/24)
- [Introducing Fluffy - an ultra-light client for Ethereum](https://our.status.im/nimbus-fluffly/)
  - [Todo List](https://github.com/status-im/nimbus-eth1/issues/830)
- JS implementation of portal client, there’s already a [PoC](https://github.com/acolytec3/ultralight)


## Other resource

These resource should help us understand why the CDAP comes out and how we contibute to the Ethererum core protocol development.

- [What is the Core Developer Apprenticeship Program](https://snakecharmers.ethereum.org/the-core-developer-apprenticeship-program/)
- [How to actuall be helpful](https://github.com/ethereum-cdap/cohort-zero/issues/78)