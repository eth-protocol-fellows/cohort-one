# Witness

A witness provides all the necessary hashes in a state trie, along with some structural information about where in the trie those hashes belong. This allows a stateless node to include new transactions in its state and to compute a new root hash locally. It would not be required for the stateless node to download an entire copy of the state trie.

## Link Tree

### 2020
- [The curious case of BLOCKHASH and Stateless Ethereum, Alex Beregszaszi](https://ethresear.ch/t/the-curious-case-of-blockhash-and-stateless-ethereum/7304)
- [Block Witness Formal Specification, Igor Mandrigin](https://github.com/ethereum/portal-network-specs/blob/change-goals-structure/witness.md)
- [KV-Witness	, Igor Mandrigin](https://medium.com/@mandrigin/kv-witness-8985168537f9)
- [Eth Witness Experiments, Paul D](https://github.com/poemm/eth_witness_experiments)
- [Ethereum block witness format (outdated), Paul D](https://notes.ethereum.org/dfeV_C-pSI2h378AadR9DQ)
- [The Formal Witness Spec: An Illustrated Primer, Griffin Ichiba Hotchkiss](https://notes.ethereum.org/bjRBM5IxQQOgqp7bez15xQ)
- [Adapting Pointproofs for Witness Compression, Raghavendra](	https://hackmd.io/jYZoAiK2THq50EvI6lGhMw?view)
- [Splitting Witnesses into Two Parts, Viktor Kirilov](https://ethresear.ch/t/splitting-witnesses-into-2-parts/8357)

### 2019
- [Protocol Changes to Bound Witness Size, Vitalik Buterin](https://ethereum-magicians.org/t/protocol-changes-to-bound-witness-size/3885)

