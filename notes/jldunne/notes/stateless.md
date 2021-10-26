# Stateless

The state of Ethereum describes the current status of all accounts and balances, as well all the transactions associated with all smart contracts deployed and running in the EVM.

## State Expiry

State expiry refers to the idea that the Ethereum state size is growing, and that old state should be expired so that nodes in the network are not forced to hold that state forever.

There are two approaches to this - the state can be explicitly deleted, maybe moved to a different Merkle tree so that someone could use it to revive the state object at a later date. The other is that the position in the tree is flagged as expired, so that nodes know not to store before this point.

In the first case, there are a few edge cases whereby a contract can be expired, revived, maybe modified or something else is deployed in its place, then a subsequent expiry/revival would fail.

## Weak Statelessness

Requires block proposers to store state, and allows all other nodes to verify blocks without holding state. This would practically require a switch to verkle trees to reduce witness sizes.


## Link Tree

### 2021
- [Complete revamp of the "Stateless Ethereum" roadmap, Piper Merriam](https://ethresear.ch/t/complete-revamp-of-the-stateless-ethereum-roadmap/8592)
- [Doing Stateless Ethereum Right, Piper Merriam](https://notes.ethereum.org/mSOAdx_XT02MEqrt0f2CPA)
- [A Theory of Ethereum State Size Management, Vitalik Buterin](https://hackmd.io/@vbuterin/state_size_management)
- [Why it's so important to go stateless, Dankrad Feist](https://dankradfeist.de/ethereum/2021/02/14/why-stateless.html)
- [Resurrection-conflict-minimized state bounding, take 2, Vitalik Buterin	](https://ethresear.ch/t/resurrection-conflict-minimized-state-bounding-take-2/8739)
- [A few paths to statelessness and state expiry, Vitalik Buterin](https://hackmd.io/@vbuterin/state_expiry_paths)
- [Increasing address size from 20 to 32 bytes, Vitalik Buterin](https://ethereum-magicians.org/t/increasing-address-size-from-20-to-32-bytes/5485)
- [An updated roadmap for Stateless Ethereum, Piper Merriam](https://ethresear.ch/t/an-updated-roadmap-for-stateless-ethereum/9046)
- [Light Ethereum Subprotocol, Felix Lange](https://github.com/ethereum/devp2p/blob/master/caps/les.md)
- [A state expiry and statelessness roadmap, Piper Merriam](https://notes.ethereum.org/@vbuterin/verkle_and_state_expiry_proposal)
- [State Expiry EIP, Vitalik Buterin](https://notes.ethereum.org/@vbuterin/state_expiry_eip)
- [State Expiry and Statelessness in Review, Nicolas Laurent](https://github.com/tvanepps/)
- [ASE (Address Space Extension) with Translation Map, Alex Beregszaszi](https://notes.ethereum.org/@ipsilon/address-space-extension-exploration)

### 2020
- [The Data Availability Problem under Stateless Ethereum, Piper Merriam](https://ethresear.ch/t/the-data-availability-problem-under-stateless-ethereum/6973)
- [ReGenesis - Resetting Ethereum to reduce the burden of large blockchain and state, Alexey Akhunov](https://ethresear.ch/t/regenesis-resetting-ethereum-to-reduce-the-burden-of-large-blockchain-and-state/7582)
- [ReGenesis Explained, Igor Mandrigin](https://medium.com/@mandrigin/regenesis-explained-97540f457807)


### 2019
- [Intro To Beam Sync, Jason Carver](https://medium.com/@jason.carver/intro-to-beam-sync-a0fd168be14a)
- [The 1.x Files: A Fast Sync, Griffin Ichiba Hotchkiss](https://blog.ethereum.org/2019/12/10/eth1x-files-fast-sync/)
- [The 1.x Files: The State of Stateless Ethereum, Griffin Ichiba Hotchkiss](https://blog.ethereum.org/2019/12/30/eth1x-files-state-of-stateless-ethereum/)

### 2017
- [The Stateless Client Concept, Vitalik Buterin](https://ethresear.ch/t/the-stateless-client-concept/172)

### 2015
- [EIP-103: Blockchain Rent, Vitalik Buterin](https://github.com/ethereum/EIPs/issues/35)

