# Sharding

Sharding is very important to the future of Ethereum - by enabling Ethereum to scale and support thousands of transactions per second.

The blockchain scalability trilemma is important here - the idea that there are three properties that a blockchain can have, and that at best you can only have two of three. The three properties are scalable, secure or decentralised.

The basic idea of blockchain sharding is as follows (note that this is a much simpler technique than what Ethereum will eventually use) - the work of doing verification on a single block is randomly assigned to a group of validators. This group is called a commitee. When a block is validated, the validator publishes a signature. This means that the other validators are now verifying signatures instead of blocks, a much simpler task.

There are two main techniques for scalably validating computing - fraud proofs and ZK-SNARKs. Fraud proofs are a system where you require someone to stake a deposit in order to sign a message verifying computation. If someone else wants to dispute that message, they will make a challenge, and also have to stake a deposit. In the case of a challenge, all nodes will recompute the calculation and whoever was wrong will lose their staked deposit.

ZK-SNARKs are a form of cryptographic proof that directly proves the claim "performing computation C on input X gives output Y". This proof is also quick to verify.

## Link Tree

### 2021
- [Sharding FAQs](https://eth.wiki/sharding/Sharding-FAQs)
- [An explanation of the sharding + DAS proposal, Vitalik Buterin](https://hackmd.io/@vbuterin/sharding_proposal)
- [Quick merge via fork choice change, Vitalik Buterin](https://notes.ethereum.org/m9IX3OkkTveXCCOSzGkUiw)
- [Ethereum Annotated Spec, Vitalik Buterin](https://github.com/ethereum/annotated-spec)
- [Data Availability Sampling Phase 1 Proposal, Vitalik Buterin](https://hackmd.io/G-Iy5jqyT7CXWEz8Ssos8g)
- [Consensus Specs: Sharding, Anton Nashatyrev](https://github.com/ethereum/consensus-specs/tree/dev/specs/sharding)
- [Why sharding is great: demystifying the technical properties, Vitalik Buterin](https://vitalik.ca/general/2021/04/07/sharding.html)

### 2020
- [State Provider Models in Ethereum 2.0, Ansgar Dietrichs](https://ethresear.ch/t/state-provider-models-in-ethereum-2-0/6750)
- [A 0.001 bit proof of custody, Dankrad Feist](https://ethresear.ch/t/a-0-001-bit-proof-of-custody/7409)
- [A rollup-centric Ethereum roadmap, Vitalik Buterin](https://ethereum-magicians.org/t/a-rollup-centric-ethereum-roadmap/4698)
- [A fee market contract for eth2 shards in eth1, Vitalik Buterin](https://ethresear.ch/t/a-fee-market-contract-for-eth2-shards-in-eth1/8124)

### 2019
- [Fraud and Data Availability Proofs: Maximising Light Client Security and Scaling Blockchains with Dishonest Majorities, Mustafa Al-Bassam, Alberto Sonnino, Vitalik Buterin](https://arxiv.org/abs/1809.09044)
- [LazyLedger: A Distributed Data Availability Ledger with Client-Side Smart Contracts,	Mustafa Al-Bassam](https://arxiv.org/abs/1905.09274)
- [ETH2 Shard Chain Simplification Proposal, Vitalik Buterin](https://notes.ethereum.org/@vbuterin/HkiULaluS)
- [The Eth1 -> Eth2 Transition, Vitalik Buterin](https://ethresear.ch/t/the-eth1-eth2-transition/6265)
- [Alternative proposal for early eth1 <-> eth2 merge, Vitalik Buterin]	(https://ethresear.ch/t/alternative-proposal-for-early-eth1-eth2-merge/6666)

### 2018
- [1-bit aggregation-friendly custody bonds, Justin Drake](https://ethresear.ch/t/1-bit-aggregation-friendly-custody-bonds/2236)

### 2017
- [Exploring Elliptic Curve Pairings, Vitalik Buterin](https://vitalik.ca/general/2017/01/14/exploring_ecp.html)
