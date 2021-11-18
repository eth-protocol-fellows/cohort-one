


https://github.com/ethereum/consensus-specs/pull/2146/files/de459bbbf26c9948fbb8bcbf457d6dfbcd91833c


https://hackmd.io/@vbuterin/das




# DAS scheme

- Each node in the committee get the full block. The block is cut into points (binary field). The points are sequenced and to be considered as evaluations of a polynomial. p(i) = point_i.

Having access to the polynomial allows us to recover the full block.

- Points -> polynomial -> extend the points. Points are divided into samples. Each node construct 4096 samples. Each sample has 8 points. 2048*8 samples is required to recover the polynomial.

- The polynomial commitment is on the horizontal, shard subset. Each sample goes to 64 vertical subset.

- The replication factor is 64. There is also erasure coding guarantee.



## some math on how much data a node gets.
- A shard is 1 unit of data.
- Say that there are 1000 vertical subsets. Replication is 64 and also 64 shards. This two do not have to the same.
- Each vertical subset gets 1/1000 * 64 = 0.064 =6% of the data. Plus expansion, so more like 10% of the shard data.

- Another scenario:  4000 vertical subset, 1000 shards, 100x replication --> 1/4000 * 100 = 2% of a full shard.

### Q:
- What happen to the data in the long run? Should they exist?





