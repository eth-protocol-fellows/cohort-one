# Portal Network

**What is the portal network?**

It is an effort to ensure lightweight protocol access by resource-constrained devices. It is made up of multiple P2P networks that expose the JSON-RPC API. These networks are designed so that clients can participate using minimal bandwidth, CPU, RAM and other resources.

A portal client is a piece of software that participates on these networks.

**What is the JSON-RPC API?**

It is a standard API that each Ethereum client must implement. It contains a set of methods that can return information about blocks/addresses etc. The specification can be found [here](https://playground.open-rpc.org/?schemaUrl=https://raw.githubusercontent.com/ethereum/eth1.0-apis/assembled-spec/openrpc.json&uiSchema%5BappBar%5D%5Bui:splitView%5D=true&uiSchema%5BappBar%5D%5Bui:input%5D=false&uiSchema%5BappBar%5D%5Bui:examplesDropdown%5D=false)

An application can interact directly with the JSON-RPC API, or there are many libraries that provide wrappers.

**Why is the portal network necessary?**

It is desirable to have the Ethereum protocol be available to as many devices as possible - no matter how few resources they have - as this increases decentralisation.

**What is the portal network made up of?**
It is built on the Discover V5 protocol and operates over UDP. This protocol allows building custom sub-protocols. Portal is therefore divided into the following sub-protocols - state network, history network, transaction gossip network, header gossip network, canonical indices network. 

https://github.com/ethereum/portal-network-specs

**Potential Projects**

- PoC for the header accumulator - https://notes.ethereum.org/KaMqlqxiQLCWyDoXCUCC4Q
- PoC for Chain History Storage Network - https://hackmd.io/ctTNH9xsSu2ci9DeGidUsQ?view

## Link Tree

### 2021
- [The Eth1x Glossary, Piper Merriam](https://github.com/ethereum/portal-network-specs/wiki/Glossary)
- [Bridge Node JSON-RPC Interface	, Brian Cloutier](https://notes.ethereum.org/@lithp/r1NB6M4Gu)
- [Gossip for DHT State Network, Piper Merriam](https://notes.ethereum.org/Fhh0MKOCRgyDdNcw6a9h6w)
- [Scalable Gossip for state network,	Piper Merriam](https://ethresear.ch/t/scalable-gossip-for-state-network/8958)
- [State Network DHT - Development Update #2, Piper Merriam	](https://ethresear.ch/t/state-network-dht-development-update-2/9005)
- [Beam Sync in 80 Seconds, Using Meta-Witnesses, Jason Carver](https://snakecharmers.ethereum.org/beam-sync-in-80-seconds-using-meta-witnesses/)
- [Ethereum Consensus Upgrade (Execution-layer perspective), Mikhail Kalinin](https://hackmd.io/@n0ble/ethereum_consensus_upgrade_mainnet_perspective)
- [Notes on Header Accumulation and the Portal Network, Piper Merriam](https://notes.ethereum.org/KaMqlqxiQLCWyDoXCUCC4Q)
- [Extensible Discover V5 Sub-Protocol Architecture, Piper Merriam	](https://notes.ethereum.org/tPzmxQD_S3S3uvtpUSA0-g)
- [Chain History Storage Network, Jacob Kaufmann](https://hackmd.io/ctTNH9xsSu2ci9DeGidUsQ?view)
- [Portal Network Specs](https://github.com/ethereum/portal-network-specs)
- [Minimal Light Client, Hsiao-Wei Wang](https://github.com/ethereum/consensus-specs/blob/a465f21deb5df945ff629b87d3b1de229cbceb3f/specs/altair/sync-protocol.md)
- [Introducing Fluffy - an ultra-light client for Ethereum, Sacha From Status](https://our.status.im/nimbus-fluffly/)


### 2020
- [Some cold storage numbers, Brian Cloutier](https://notes.ethereum.org/@lithp/HymFXB8gu)


