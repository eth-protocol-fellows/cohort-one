# Dan Buchholz

## Areas of Interest

- Statelessness & light clients
- Sharding / Beacon chain & PoS
- MEV
- EVM code safety
- zk & privacy / rollups

## Cohort Goals

### Phase 0

To better understand the innerworkings of Ethereum clients (e.g., geth), bolstering my Ethereum knowledge such that I can creatively contribute to the community through EIPs, open issues/bugs, etc.

## Project(s) of Interest

- [Trace API analysis](https://github.com/ethereum-cdap/cohort-one/issues/43) -- provides hands-on approach toward better understanding clients (Phase 0 goal); this project has been committed to
- [Beacon chain light clients](https://github.com/ethereum-cdap/cohort-one/issues/9) -- provides hands-on approach for contributing to specifications and a potential implementation (in only a prototype); this project has been committed to
- [Low-level Rust tooling for EVM](https://github.com/ethereum-cdap/cohort-one/issues/30) -- provides additional insight into EVM innerworkings (Phase 0 goal, plus, selfishly learning Rust); this has not been committed to but a potential option

## Notes

Notes repository located in the [`danbuchholz`](./danbuchholz) directory.

### Research Resources / Links

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
