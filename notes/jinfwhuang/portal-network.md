# Portal Network

## Links
- The specs repo contains most of the latest thinking with regard to portal network: https://github.com/ethereum/portal-network-specs

- Why stateless is so important: https://dankradfeist.de/ethereum/2021/02/14/why-stateless.html

- devp2p basics: https://www.youtube.com/watch?v=hnw59hmk6rk

- libp2p basics: https://www.youtube.com/watch?v=69h1zhIdCN0


## My Notes

- The key feature of the portal network is that it allows each node to choose a radius `r` that controls how much data the node is interested in the network.

- I would categorize that there three types of data propagation pattern in the portal netowrk:
  - Rep/Resp domain; Nodes know what they are looking for. The DHT  E.g.
    - State data (root hash + branch + merkle proof), both execution layer and beacon chain
    - BlockBody, both execution layer and beacon chain

  -  `O(1)` Gossip domain
    Both small radius and large radius nodes want to have quick access to this data. The amount of data that need to be gossiped is relatively small. We might not need to distinguish between small radius and large radius nodes. E.g.
    - block headers
    - beach chain block headers and sync_aggregate

  - `O(n)` gossip domain
    Large radius nodes want to all the new data to reach them eventually, but for small radius nodes, they only have resources to store a subset. Small radius nodes still contribute to network reliability. E.g.
    - new transactions
    - block bodies


## Gtihub issue discussion
- https://github.com/ethereum/portal-network-specs/issues/89
- https://github.com/ethereum/portal-network-specs/issues/90
- https://github.com/ethereum/portal-network-specs/issues/91
- https://github.com/ethereum/portal-network-specs/issues/92
- https://github.com/ethereum/portal-network-specs/issues/93
