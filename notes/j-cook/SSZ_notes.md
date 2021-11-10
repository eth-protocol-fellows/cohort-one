# SSZ Serialization, generalized indiced and multiproofs
This page contains a conceptual outline for these concepts as they relate to the development of the Rust light client.

## Generalized indices

A generalized index is an integer that represents a node in a binary merkle tree where each node has a generalized index `2 ** depth + index in row`. 

```
        1           --depth = 0  2**0 + 1 = 1
    2       3       --depth = 1  2**1 + 0 = 2, 2**1+1 = 3
  4   5   6   7     --depth = 2  2**2 + 0 = 4, 2**2 + 1 = 5, 2**2 + 2 = 6, 2**2 + 3 = 7

```
Each generalized index is a uint64, so the path can be represented as a Vec<u64>.

This representation yields a node index for each piece of data in the merkle tree. We need to find the indices for the beacon state root and a) the sync_commitee object, and b) the finalized root in the merklized beacon state object. This is all that is needed for the update object. Downstream, these indices will be used for verification by identifying the hash partners of each node in the branch between the sync_committee/finalized root and the state root. Then, sequentially hashing each node with its partner should yield a hash equal to the state root.


So to implement this in the Rust light server, we first need to download the `beacon_state` json object and make an instance, `state`, of the `BeaconState` struct from it. Then, this struct must be serialized using SSZ (simple serialize). 


## ssz serialization

This takes each element in the object and returns a bytestring. Each value in the `state` object will be represented with fixed size in the SSZ encoded object. This is achieved by taking each element in turn and converting it into a little-endian value of fixed length (i.e. it is right-padded with zeros until it is the right size). For basic types (Boolean, uint) this is straightforward, but for variable length types we need to introduce the concept of offsets. In place of the actaul value int he serialization, we instead insert a number representing how many bits in the serialized data structure to skip in order to find the value, like a pointer to the actual data for the value relative to the position in the serialized object where it belongs. e.g. below I define a struct and instantiate it, then serialize using SSZ:



```
    struct Dummy {

        number: u64,
        number2: u64,
        vector: Vec<u8>
        number3: u64
    }

    dummy = Dummy{

        number1: 37,
        number2: 55,
        vector: vec![1,2,3,4],
        number3: 22,
    }

    serialized = ssz.serialize(dummy)

```

serialized would have the following structure (only padded to 4 bits here, padded to 32 bits in reality):

```
[37, 0, 0, 0, 55, 0, 0, 0, 16, 0, 0, 0, 22, 0, 0, 0, 1, 2, 3, 4]
  ----------   ----------   ----------  -----------  ----------
      |             |            |           |            |
   number1       number2    offset for    number 3    value for 
                              vector                   vector

```

divided over lines for clarity:

```
[
  37, 0, 0, 0,  # little-endian encoding of `number1`.
  55, 0, 0, 0,  # little-endian encoding of `number2`.  
  16, 0, 0, 0,  # The "offset" that indicates where the value of `vector` starts (little-endian 16).
  22, 0, 0, 0,  # little-endian encoding of `number3`.
  1, 2, 3, 4,   # The actual value of the `bytes` field.          
]
```
This is still a simplification - the integers and zeros in the schematics above would actually be stored as 8bit binary objects, like this:

```
[
  10100101000000000000000000000000  # little-endian encoding of `number1`
  10110111000000000000000000000000  # little-endian encoding of `number2`. 
  10010000000000000000000000000000  # The "offset" that indicates where the value of `vector` starts (little-endian 16). 
  10010110000000000000000000000000  # little-endian encoding of `number3`.
  10000001100000101000001110000100   # The actual value of the `bytes` field.          
]
```

So the actual values for variable-length types are stored in a heap at the end of the serialized object with their offsets stored in their correct positions in the ordered list of fields.

<br>
<br>

![Merkleization schematic](/Assets/eth2-ssz.png)

<i>SSZ schematic from Protolambda github: https://github.com/protolambda/eth2-docs#ssz-encoding </i>
<br>
<br>

## Merkleization

This serialized object can then be merkleized - that is transformed into a Merkle-tree representation of the same data. First, the number of 32 bit chunks in the serialized object is determined. These are the "leaves" of the tree. With 8 leaves there will be 3 levels to the tree (i.e. its depth is 3). This is because each element is hashed with one partner in each level, causing the number of elements to halve each time./ Three halvings of eight equals one, and this one is the root hash. In our exampel above, the first leaf will be the value of `number1` padded to 32 bits:

[37, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0] = leaf 1
<br>
[55, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0] = leaf 2

These leaves sit at the lowest level of the tree - level 3 in this case - and they are hashed together to form a node in level 2. In the diagram below leaf1 and leaf2 hash to form node 1. Leaf3 and leaf4 hash to form node2. Node1 and node2 hash to form the root. If there are an odd number of leaves, the last leaf is hashed with 32 zeros.
<br>
<br>
```
      __root__
     /        \
    node1      node2
   /   \     /    \
leaf1 leaf2 leaf3  leaf4
```
<br>
<br>
Instead of referring to these tree lements as leaf X, node X etc, we can give them generalized indices, starting with root = 1 and counting from left to right along each level. This is the generalized index explained above. Each element in the serialized list has a generalized index equal to `2**depth + idx` where idx is its zero-indexed position in the serialized object and the depth is the number of levels in the Merkle tree, which can be determined as the square root of the number of elements (leaves). 

<br>
<br>

![Merkleization schematic](/Assets/eth2-htr.png)
<i>Merkleization schematic from Protolambda github: https://github.com/protolambda/eth2-docs#ssz-hash-tree-root-and-merkleization</i>

<br>
<br>
## Multiproofs

Providing the list of generalized indices representing a specific element allows us to verify it against the hash-tree-root. This root is our accepted version of reality. Any data we are provided can be verified against that reality by inserting it into the right place in the Merkle tree (determined by its generalized index) and observing that the root remains constant. There are functions in the spec [here](https://github.com/ethereum/consensus-specs/blob/dev/ssz/merkle-proofs.md#merkle-multiproofs) that show how to compute the minimal set of nodes required to verify the contents of a particular set of generalized indices.

For example, to verify data in index 9 in the tree below, we need the hash of the data at indices 8, 9, 5, 3, 1. 
The hash of (8,9) should equal hash (4), which hashes with 5 to produce 2, which hashes with 3 to produce the tree root 1. If incorrect data was provided for 9, the root would change - we would detect this and fail to verify the branch.

```
                    1 
          2                     3                  
    4           5          6          7               
8      9    10    11   12    13    14    15   
        
```


## Implementation in Rust light-client

For the light client server, we do not need to conduct any actual proofs, but we do need to serialize the `state` object and determine the generalized indices for the `next_sync_committee` and `finalized_root` fields. [ssz_rs](https://github.com/ralexstokes/ssz_rs) has almost all the required functionality, but probably requires some extending to cope with nested structs.

<br>
<br>

## The beacon_state object

The state obect to be serialized has the following fields. This information is useful for conceptualizing the serialization.

```

key                               |    type         |   fixed/var size   
-------------------------------------------------------------------------
balances                          |   vec<u64>      |     fixed         
block_roots                       |   vec<String>   |     variable     
current_epoch_participation       |   vec<u8>       |     fixed        
current_justified_checkpoint      |   Checkpoint    |     fixed        
    epoch                         |   u32           |     fixed           
    root                          |   String        |     fixed         
current_sync_committee            |   SyncCommittee |     fixed               
    pubkeys                       |   String        |     fixed          
    aggregate_pubkey              |   String        |     fixed         
eth1_data                         |   Eth1Data      |     fixed         
    deposit_root                  |   String        |     fixed         
    deposit_count                 |   u64           |     fixed         
    block_hash                    |   32bit hash    |     fixed        
eth1_data_votes                   |   Container?    |     ??            
eth1_deposit_index                |   u64           |     fixed         
finalized_checkpoint              |   Checkpoint    |     fixed          
    epoch                         |   u64           |     fixed
    root                          |   String        |     fixed
fork                              |   Fork          |     fixed           
    current_version               |   String        |     fixed
    epoch                         |   u64           |     fixed
    previous_version              |   String        |     fixed
genesis_time                      |   u64           |     fixed
genesis_validators_root           |   String        |     fixed
historical_roots                  |   vec<String>   |     ??
inactivity_scores                 |   vec<u64>      |     fixed
justification_bits                |   vec<u8>       |     fixed
latest_block_header               |   BlockHeader   |     fixed
next_sync_committee               |   SyncCommittee |     fixed
    pubkeys                       |   String        |     fixed          
    aggregate_pubkey              |   String        |     fixed 
previous_epoch_participation      |   vec<u8>       |     fixed
previous_justified_checkpoint     |   Checkpoint    |     fixed         
    epoch                         |   u64           |     fixed
    root                          |   String        |     fixed
randao_mixes                      |   vec<u32>      |     fixed     
slashings                         |   vec<u64>      |     ??
slot                              |   u64           |     fixed
state_roots                       |   vec<String>   |     ??
validators                        |   vec<u64>      |     fixed

```
<br>
<br>

## Outstanding Questions:

<b>QUESTION</b> How does the deserialization then know where the end of each fixed-size value is in the "heap" at the end?
<br>
<b>QUESTION</b> How does the deserialization know which elements are values and which are offsets?
<br>
<b>QUESTION</b> What happens to the field names?
<br>
<b>QUESTION</b> Should the generalized indices in the light client object just be the specific indices for the data, or the full "path" through the
hash-tree? i.e. for gen_idx = 10 in the following tree
<br>
<br>
```
                    1 
          2                     3                  
    4           5          6          7               
8      9    10    11   12    13    14    15   
        
```

does the update object require [10] or [1, 2, 5, 10]?