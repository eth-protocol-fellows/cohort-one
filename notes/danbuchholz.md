# Dan Buchholz

## Areas of Interest

- Light clients, Beacon chain, & PoS
- The Merge, execution layer, statelessness
- Sharding; zk & rollups
- MEV (within the scope of the roadmap)

## Cohort Goals

- To understand how low-level Ethereum client implementations are built, allowing me to better understand protocol-level interactions and creatively contribute to the Ethereum protocol's R&D through EIPs / specifications
- Protoype new functionality (defined in Ethereum specifications but yet to be implemented) using an existing client's codebase

## Project

- _Based on the project idea [Beacon chain light clients](https://github.com/ethereum-cdap/cohort-one/issues/9)_
- The goal is to create a very basic client/server of a light client, defined by the [Sync Protocol](https://github.com/ethereum/consensus-specs/blob/dev/specs/altair/sync-protocol.md)
- It is similar to what ChainSafe's Lodestar [has already created](https://github.com/ChainSafe/lodestar/blob/master/packages/light-client) and uses Sigma Prime's [Lighthouse codebase](https://github.com/sigp/lighthouse)
- The general design uses a server that makes HTTP GET requests (adhering to [Beacon APIs](https://ethereum.github.io/beacon-APIs)) to a node at every slot for the tip of the chain, allowing the server to create `LightClientUpdate`s and serve them to a requesting client (which then locally tracks/saves the latest block headers)
- Additional context can be found in [Development Update #2](https://hackmd.io/@dtb/By94YjF_F) and exists in the following repo: [here](https://github.com/buchhlz2/beacon-light-client)

## Notes

Notes repository located in the [`danbuchholz`](./danbuchholz) directory.

### Research Resources / Links

- Light clients
  - [Consensus Specifications (Beacon Chain, Sync Protocol, etc.)](https://github.com/ethereum/consensus-specs)
  - [ChainSafe's Consensus Light Client Server Implementation Notes](https://hackmd.io/hsCz1G3BTyiwwJtjT4pe2Q)
- EVM code safety & validation
  - [Safer control flow for EVM (EIP-3779)](https://eips.ethereum.org/EIPS/eip-3779)
  - [Everything about EVM Object Format ("EOF")](https://notes.ethereum.org/@ipsilon/evm-object-format-overview)
  - [EVM object format (EIP-3540)](https://eips.ethereum.org/EIPS/eip-3540)
  - [EOF code validation (EIP-3670)](https://eips.ethereum.org/EIPS/eip-3670)
- Neutering/removing SELFDESTRUCT
  - [Pragmatic destruction of SELFDESTRUCT](https://hackmd.io/@vbuterin/selfdestruct)
  - [Analysis of neaturing SELFDESTRUCT](https://github.com/adompeldorius/selfdestruct-analysis) (data [here](https://nbviewer.jupyter.org/github/adompeldorius/selfdestruct-analysis/blob/main/analysis.ipynb))
- MEV
  - [Targeting zero MEV](https://ethresear.ch/t/targeting-zero-mev-a-content-layer-solution/9224)
  - [Mitigating sandwich attacks](https://hackmd.io/@norswap/mev)
  - [MEV & Me](https://www.paradigm.xyz/2021/02/mev-and-me/)
  - [MEV in ETH2](https://hackmd.io/@flashbots/mev-in-eth2)
  - [Ethereum blockspace -- who gets what & why](https://www.paradigm.xyz/2021/03/ethereum-blockspace-who-gets-what-and-why/)
  - [Quantifiying REV](https://hackmd.io/@flashbots/quantifying-REV)
  - [MEV (data)](https://explore.flashbots.net/)
- Statelessness & state expiry
  - [A theory of state size mgt.](https://hackmd.io/@vbuterin/state_size_management)
  - [Why it's important to go stateless](https://dankradfeist.de/ethereum/2021/02/14/why-stateless.html)
  - [A few paths to statelessness & state expiry](https://hackmd.io/@vbuterin/state_expiry_paths)
  - [Stateless Ethereum tech tree](https://blog.ethereum.org/2020/01/28/eth1x-files-the-stateless-ethereum-tech-tree/)
  - [PCS multiproofs (for weak statelessness w/Verkle tries)](https://dankradfeist.de/ethereum/2021/06/18/pcs-multiproofs.html)
  - [Stateless Ethereum roadmap](https://ethresear.ch/t/an-updated-roadmap-for-stateless-ethereum/9046)
  - [State expiry resource list (via Discord channel)](https://github.com/tvanepps/EthereumDiscordGuidebook/blob/main/README.md#state-expiry)
  - [Stateless Ethereum & the Portal Network](https://www.youtube.com/watch?v=jAX_bgcESoc)
  - [Light Client Design in Eth2](https://www.youtube.com/watch?v=ysW-Bq05pJQ)
- Beacon chain, sharding, & the merge:
  - [Why sharding is great](https://vitalik.ca/general/2021/04/07/sharding.html)
  - [Beacon chain explainer](https://ethos.dev/beacon-chain/)
  - [Vitalik's Annotated Ethereum 2.0 Spec](https://notes.ethereum.org/@vbuterin/SkeyEI3xv)
  - [Ethereum reorgs after the merge](https://www.paradigm.xyz/2021/07/ethereum-reorgs-after-the-merge/)
  - [Ethereum 2.0 Knowledge Base](https://kb.beaconcha.in/glossary#beacon-chain)
  - [Ethereum 2.0 Phase 0 -- The Beacon Chain](https://benjaminion.xyz/eth2-annotated-spec/phase0/beacon-chain/)
  - [Combining GHOST & Casper](https://arxiv.org/pdf/2003.03052.pdf)
  - [Defining Attestation Inclusion](https://www.attestant.io/posts/defining-attestation-effectiveness/)
  - [Attestation Inclusion](https://www.youtube.com/watch?v=SPcgevcDqDE)
- zk
  - [ZK-SNARKs under the hood](https://medium.com/@VitalikButerin/zk-snarks-under-the-hood-b33151a013f6)
  - [Exploring elliptic curve pairings](https://medium.com/@VitalikButerin/exploring-elliptic-curve-pairings-c73c1864e627)
  - [Quadratic arithmetic](https://medium.com/@VitalikButerin/quadratic-arithmetic-programs-from-zero-to-hero-f6d558cea649)
  - [Zcash](https://z.cash/technology/zksnarks/)
  - [Explaining SNARKs](https://electriccoin.co/blog/snark-explain/)
- Misc.
  - [Verkle tries for ETH1 state](https://dankradfeist.de/ethereum/2021/06/18/verkle-trie-for-eth1.html)
  - [Simple Serialize (SSZ)](https://www.ssz.dev/)
  - [Increasing address size to 32 bytes](https://ethereum-magicians.org/t/increasing-address-size-from-20-to-32-bytes/5485)
