# Beacon Chain Light Client

## My current understanding

#### Protocol specs

1. Consensus protocol and execution

- The consensus protocol as of Altair already supports light client state updates. See [sync-protocol](https://github.com/ethereum/consensus-specs/blob/dev/specs/altair/sync-protocol.md) and [beacon-chain](https://github.com/ethereum/consensus-specs/blob/dev/specs/altair/beacon-chain.md)
- There are some discussions on how light clients use state root and merkle proofs to create transactions, but they are not in the specs.

2. Network protocol

- It doesn't look like that there is any design decisions made on how light client fetch block headers, committee signatures, and merkle proofs (witnesses).
- Two basic approaches: talk to full notes, talk to a p2p light client subnet
- There seems to be an overlap with **portal network** on how to allow light weight clients to get the data that they need.

#### Client Implementations

- Most Beacon chain implementations have not implemented the "state update" logic.

- A note on some implementation details on the Lodestar Beacon Chain: https://hackmd.io/hsCz1G3BTyiwwJtjT4pe2Q

- The v2 release of Prysm, which is compatible with Altair hard fork, only implements sync committee logic that is required by the consensus protocol. It has not implemented any state update logic. This could be a good POC project to work on.

## My next steps

- Find a couple of people who is actively thinking about this topic. I want to have a chat with them with respect to the following questions.
  - Is there a plan to motivate client implementations to have a light client option? What is the timing of that plan?
  - With regard to Beacon Chain light clients, what is the "spec plan" and "implementation plan"?
  - What is the relationship between portal network and light client? Should light client be participating on the portal network? Is light client equivalent to stateless client? Will they co-exist or there going to be only one of them?
  - People:
    - Alex Stoke
    - Piper Merriam
- Understand how the portal network will impact Beacon Chain light clients
- Understand what is missing on the existing light client spec
- Implement light client "sync logic" in Prysm beacon-chain

## Open questions and comments

- More light client readings:

  - Light client reorg mechanism https://github.com/ethereum/consensus-specs/issues/2182
  - Light client specs checklist https://github.com/ethereum/consensus-specs/issues/2315
  - Light-client feedback https://github.com/ethereum/consensus-specs/issues/2429

- celo "ultralight client" design: https://diyhpl.us/wiki/transcripts/stanford-blockchain-conference/2020/celo-ultralight-client/

- portal network

  - https://ethresear.ch/t/scalable-transaction-gossip/8660
  - https://ethresear.ch/t/scalable-gossip-for-state-network/8958

- https://github.com/ethereum/devp2p

## Links that give overviews of Beacon Chain light client

- 2021/06/07 Vitalik gave a short description on the current design of how light clients would perform block header updates. It also described what is missing in the design of light client protocol. https://www.youtube.com/watch?v=iaAEGs1DMgQ&list=LL&index=62&t=2200s
- 2021/09/01 Alex Stokes talked about eth2 light client. He gave an overview of light client background. https://www.youtube.com/watch?v=ysW-Bq05pJQ
- 2021/07/21 A very high level background on light client, stateless clients, and lightweight clients by way of portal network https://www.youtube.com/watch?v=jAX_bgcESoc
