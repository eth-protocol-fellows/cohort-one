# Ethereum Capacity Improvents:

## Interests

The following topics have particularly piqued my interest:
- PoS
- Beacon light client development
- Beacon fuzzing/testing

I'm pretty convinced Beacon-chain light-client development is my top choice of topic and this is where I intend to focus my attention initially.
  
## Background

I'm working on this part-time and would be very open to collaborating with others on Beacon chain light client development. I am especially interested in this because light-clients are fundamental to Ethereum being widely decentralised and minimizing reliance on centralised operators such as Infura. On a personal level this feels like a gap where I might be able to make some tangible contributions and a good way to develop a really deep understanding of PoS consensus in advance of the merge. Strategically, this seems like a potentally good foothold into core development on post-merge Ethereum. I am arriving with diverse software development experience including dapps and pretty good conceptual knowledge of Ethereum, PoS, Beacon etc, but no hands-on experience at all with p2p or client building.

## Current progress

- Deep dive into PoS, merge and rollup-centric roadmap
- Deep dive into Altair incl reading EIP and specs, listening to core dev calls, reading EF blog posts
- Examination of geth and lodestar source code
- Initial examination of beacon-fuzz code
- Reading existing literature on client development (most useful linked below)
- Spin up Geth node and interact using json RPC on testnet
- PR on ethereum.org contributing PoS explainer page (https://github.com/ethereum/ethereum-org-website/pull/3873)
- Deep dive into libp2p and build basic p2p project in Rust

## Next steps

- Continue to explore light client development, migrating from exploration to defining specific objectives
- Reach out to potential collaborators/mentors - ralexstokes has created very interesting issue on cohort-one repo
- Spin up beacon node (likely Lodestar) - if I get time and unbrick spare laptop.
- Continue exploring client architectures, specs - probably focus on Rust implementations but also explore GO/js
  
## Outstanding questions

- What is the status of the various python implementations? They seem out of vogue - Python just too slow? More secure to use a static typed language?
- rust vs go vs js for new light client? Rust feels good atm...!
- still haven't really identified and understood the the p2p networking code/control flow in existing clients - geth, lodestar...
- what are whisper and swarm? Are they fundamental to ethereum? Will they be persisted in Beacon clients?

## Useful Links
- What is a light client and why should you care? https://www.parity.io/blog/what-is-a-light-client/
- Ethereum clients 101 https://medium.com/@eth.anBennett/ethereum-clients-101-beginner-geth-parity-full-node-light-client-4bbd87bf1dee
- Micah Zoltu comments on client incentivization https://ethresear.ch/t/incentives-for-running-full-ethereum-nodes/1239/4
- Primer on light clients https://medium.com/@rauljordan/a-primer-on-ethereum-blockchain-light-clients-f3cadde49137
- Piper's light clint blog pt1,2,3 https://snakecharmers.ethereum.org/the-winding-road-to-functional-light-clients/
- Lodestar: https://lodestar.chainsafe.io/
- Lodestar (Command-Lines): https://chainsafe.github.io/lodestar/reference/cli/
- Lodestar architecture: https://chainsafe.github.io/lodestar/design/architecture/
- Beacon Api Direct Link: https://ethereum.github.io/beacon-APIs/
- Bootstrapping Beacon Light Chain Client: https://notes.ethereum.org/@ralexstokes/S1RSe1JlF
- Altair: https://github.com/ethereum/consensus-specs/blob/dev/specs/altair/sync-protocol.md
- Validator Life Cycle: https://notes.ethereum.org/7CFxjwMgQSWOHIxLgJP2Bw#A-note-on-Ethereum-20-phase-0-validator-lifecycle
- Validator node in raspberry pi https://dankradfeist.de/ethereum/2020/11/20/staking-on-raspi.html
- Schematic of Ethereum blocks https://i.stack.imgur.com/afWDt.jpg
- Basic json-rpc client in GO https://golangrepo.com/repo/ybbus-jsonrpc-go-distributed-systems
- DevP2P repository for connecting to network peers (will be base layer of light client) https://github.com/ethereum/devp2p
- DevP2P theory https://github.com/ethereum/devp2p/blob/master/discv5/discv5-theory.md
- Altair specs https://github.com/ethereum/consensus-specs/blob/dev/specs/altair/p2p-interface.md
- Alex Stokes light client EDCON presentation https://www.youtube.com/watch?v=ysW-Bq05pJQ
- Ethereum client architecture video https://www.youtube.com/watch?v=VH0obZ2A0Yg&list=PLNLh1EyDzSGP-lkNCBhCptoJ-NMu_BYfS&index=2
- Ethereum networking video https://www.youtube.com/watch?v=hnw59hmk6rk&list=PLNLh1EyDzSGP-lkNCBhCptoJ-NMu_BYfS&index=3
- Kademlia to Discv5 in Eth2 https://vac.dev/kademlia-to-discv5
- Piper's light client presentation https://www.youtube.com/watch?v=MZxqRs_tLNs