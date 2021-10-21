## Links
- https://hackmd.io/@n0ble/ethereum_consensus_upgrade_mainnet_perspective

- Eth1+eth2 client relationship: https://ethresear.ch/t/eth1-eth2-client-relationship/7248

- The scope of Eth1-Eth2 merger: https://ethresear.ch/t/the-scope-of-eth1-eth2-merger/7362

- PM notes; where things stand: https://github.com/ethereum/pm/blob/master/Merge/mainnet-readiness.md

- Alternative proposal for eth1<-> eth2 merge: https://ethresear.ch/t/alternative-proposal-for-early-eth1-eth2-merge/6666/2

- eth1 -> eth2 transition: https://ethresear.ch/t/the-eth1-eth2-transition/6265

- good overview of key documentations: https://blog.ethereum.org/2020/07/23/eth2-quick-update-no-13/

- Architecture of a geth-based eth1 engine: https://ethresear.ch/t/architecture-of-a-geth-based-eth1-engine/7574


## My notes
- Eth1 client no longer performs "consensus" work.
- Beacon chain state will contain pointers to the Eth1/execution shard block headers.
- Eth2 validators perform attesting/validating. A validator committee is selected to validate execution shard. Those validators use the eth1 clients' processing to determine whether the blocks are properly proposed, but the voting is done by the eth2/consensus client.
```
