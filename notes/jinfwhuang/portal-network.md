# Core Portal network 

## Writings, Discussions, Issues, and Specs PRs
- Core Portal Network
  - [Reliable Transaction Gossip to Full Radius Nodes](https://github.com/ethereum/portal-network-specs/issues/89)
  - [The distance metric is of ring geometry instead of XOR](https://github.com/ethereum/portal-network-specs/issues/90)
  - [FindContent message might not reach large radius node](https://github.com/ethereum/portal-network-specs/issues/91)
  - [Truthful revelation of radius value](https://github.com/ethereum/portal-network-specs/issues/92)
  - [Secondary routing table](https://github.com/ethereum/portal-network-specs/pull/105)

- Beacon Chain Portal Network
  - [Spec out what are the sub-DHT and gossip topics needed for beacon-chain](https://github.com/ethereum/portal-network-specs/issues/93)
  - [Initial beacon chain spec](https://github.com/ethereum/portal-network-specs/pull/99)
  - [Skip sync and initial sync](https://github.com/ethereum/portal-network-specs/pull/102)
  - [Light client classifications](https://ethresear.ch/t/beacon-chain-light-client-classification/11061)
  - [Light client networking](https://ethresear.ch/t/beacon-chain-light-client-networking/11063)


## Planning
I am spearheading the effort to jump start the necessary beacon chain sub-networks on the portal network.

1. Working out the specs for [beacon chain sub-networks](https://github.com/ethereum/portal-network-specs/tree/master/beacon-chain) to the point where it could be implemented as a POC
1. Build the core networking layer in the Prysm codebase
1. Implement the appropriate sub-networks
1. Incorporate this networking layer in the beacon chain light client.

## Execution

#### Design
The [specs](https://github.com/ethereum/portal-network-specs/tree/master/beacon-chain) that is a work in progress.

#### Implementation
- I am starting with the implementation of beacon chain light client without the networking layer. See [light-client](./beacon-chain-light-client.md)
- The work on the beacon chain portal networks has not yet starsted.


## Unsorted notes on the topic
- See [notes](unsorted-notes/portal-network.md)

