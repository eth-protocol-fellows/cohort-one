## Updates

|     | Date       | Description                                                                                                                                                                                 |
| --- | ---------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 0   | 2021-09-28 | <ul><li>Organized research [resources](#Resources-Categories)</li><li>Started a high level overview of the current state of [Eth2 light client](#jinfwhuang/eth2-light-client.md)</li></ul> |
| 1   | ...        | ...                                                                                                                                                                                         |

## Resources Categories

I put all research materials into three general categories in order of completeness. The third category of **specs and implementation** are the materialized outputs that directly impact the networks. The second categories are intermediate notes, but they are well researched and written to explain what went or will go into specs and implementations. The first category documents live conversations. These contents might be hard to digest, but they contain a great deal of details that are important for ongoing research.

I expect that our outputs will also fall into these categories.

### Category 1: Realtime, unstructured discussions: messages, comments, forum posts

1. [R&D discord server](https://discord.gg/yZmSRBr4)
1. [Eth Research forum](https://ethresear.ch)
1. [EIP forum](https://ethereum-magicians.org/)
1. [Stack Exchange](https://ethereum.stackexchange.com/)
1. Github issues
1. Various third party forums. For example, an [example](https://www.reddit.com/r/ethfinance/comments/pk57n7/why_rollups_data_shards_are_the_only_sustainable/) in reddit.

### Category 2: Research notes: posts, talks, slides

Resources in this category tend to be the most accessible. They are written to be used as tools to communicate.

1. Hackmd documents. E.g.

   - https://hackmd.io/hsCz1G3BTyiwwJtjT4pe2Q
   - https://notes.ethereum.org/@vbuterin/SkeyEI3xv
   - https://notes.ethereum.org/@vbuterin/HF1_proposal

2. Talks. E.g.

   - https://www.youtube.com/watch?v=iaAEGs1DMgQ&list=LL&index=62&t=2200s
   - https://drive.google.com/file/d/1NJUTaiNsN5sP9qyeNTY7O8qrWQezedGa/view
   - https://www.youtube.com/watch?v=ysW-Bq05pJQ

3. Github repos that are not intended as software. E.g.
   - https://github.com/ethereum/annotated-spec
   - https://benjaminion.xyz/eth2-annotated-spec/

### Category 3: Specs and implementations

These codes govern the behaviors of the clients that make up of the networks.

- Eth2 [specs](https://github.com/ethereum/consensus-specs). All client implementations follow this spec. The
- Client Implementations. The client implementations contain what is currently running and also what is happening to the networks in the near future.
  - Eth1
    - Geth: https://github.com/ethereum/go-ethereum
    - Parity: https://github.com/openethereum/parity-ethereum
    - Nimbus Eth2: https://github.com/status-im/nimbus-eth1
  - Eth2
    - Prysm: https://github.com/prysmaticlabs/prysm
    - Lighthouse: https://github.com/sigp/lighthouse
    - Lodestar: https://github.com/ChainSafe/lodestar
    - Nimbus Eth2: https://github.com/status-im/nimbus-eth2

## High level Eth2 resources:

- 2021/08/01 Relative recent summary of a lot of eth2 resources https://notes.ethereum.org/@casparschwa/HyGvlvkfK
- Regular updates on all things Ethereum 2.0 by Ben Edgington https://hackmd.io/@benjaminion/eth2_news/https%3A%2F%2Fhackmd.io%2F%40benjaminion%2Fwnie2_210910
- 20210 A relatively old summary of eth2 resources: https://hackmd.io/@benjaminion/eth2_info
- https://github.com/eth2-clients/eth2-networks
