
## Development updates 2 (November-December 2021)

### Progress
- Deep dive into SSZ and generalized indices (conceptual) - notes added [here](/notes/j-cook/SSZ_notes.md).
- Prototype light client development [here](https://github.com/jmcook1186/../../../beacon-light-client.md):
  - beacon state downloading beacon state and parsing it to correct types
  - correctly building snapshot object
  - downloading beacon block and extracting body, parsed into correct types
  - downloading beacon block header parsed to correct type
  - serializing state object using SSZ

## Next steps
- Client development:
  - Merklize state object, built branches from generalized indices
  - calculate generalized index for sync_committee_index and finalized_root fields
  - finish update object
  - create store object
  - serve objects to light client over http

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