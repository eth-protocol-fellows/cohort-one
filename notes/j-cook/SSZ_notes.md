# SSZ Serialization, generalized indiced and multiproofs
This page contains a conceptual outline for these concepts as they relate to the development of the Rust light client.

## Generalized indices

A generalized index is an integer that represents a node in a binary merkle tree where each node has a generalized index `2 ** depth + index in row`. 

```
        1           --depth = 0  2**0 + 0 = 1
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

So the actual values for variable-length types are stored in a heap at the end of the serialized object with their offsets stored in their correct positions in the ordered list of fields. To deserialize this object requires the <b>schema</b>. The schema defines the precise layout of the serialized data so that each specific element can be deserialized from a blob of bytes into some meaningful object with the elements having the right type, value, size and position. It is the schema that tells the deserializer which values are actual values and which ones are offsets. All field names disappear when an object is serialized, but reinstantiated on deserialization according to the schema. 

It may also be unclear how the deserialization knows what is data and what is padding for a variable length type such as a vector or list. For these data types an additional "01" bit added after the last value. This 01 bit signifies the end of the data and beginning of the padding.

e.g. a 16-element vector of uint8's would serialize to look something like:

```
                           32 bytes in total
                          (not indcluding 0x)
                                  |
------------------------------------------------------------------
0x76e996cb952c162f94e5bb10ea57126e01000000000000000000000000000000
--|-------------------------------|-|-----------------------------
|                 |                |                 |
signifies        data         01 length bit         padding
hex encoding

```

Note that this is 64 elements long. This is because it is hex-encoded. In hex, each 8 bits of data is represented by 2 characters. Therefore, 64 hex characters = 32 elements each representing 8 bits = 32 bytes.

NB See [ssz.dev](https://www.ssz.dev/overview) for an interactive explainer on this.

## SSZ Types

There are three SSZ type categories: basic, composite and special. Distinguishing which categoriy a particular element is in is important because it determines how it is serialized. The table below shows which types fal into which category:

```
Category   |   Type             |            Definition                |  Illegal properties
-----------------------------------------------------------------------------------
BASIC      |  uintN             | N can be [8, 16, 32, 64, 128, 256]   |
           |  Bool              | true or false                        | 
           |                    |                                      |
COMPOSITE  | Vector[type, N]    | fixed length (N), homogenous type    | empty
           | List[type, N]      | variable length (<N), homogenous type| 
           | Container[         | heterogeneous types organised in     | no fields
           |  fieldA: [type],   |   key-value pairs                    |
           |  fieldN: [type],   |                                      |
           | ]                  |                                      |
           | Union[type1, type2]| Union (either/or) of valid SSZtypes  | null type at idx 0                 
           |                    |                                      |
SPECIAL    | Bitvector[N]       | Vector of Bools,length N             | empty
           | Bitlist            | List of Bools, length <N             |
           | Root               | Merkle root of composite SSZ type    |
           |                    |                                      |
           
```



<br>
<br>

![Merkleization schematic](/Assets/eth2-ssz.png)

<i>SSZ schematic from Protolambda github: https://github.com/protolambda/eth2-docs#ssz-encoding </i>
<br>
<br>

## Merkleization

This serialized object can then be merkleized - that is transformed into a Merkle-tree representation of the same data. First, the number of 32-byte chunks in the serialized object is determined. These are the "leaves" of the tree. Basic types are serialized directly (i.e. they are simply converted to bytes objects and, where necessary, right-padded to 32 bytes). For example, in the `beacon_state` the `genesis_time` is a single leaf chunk because it is a basic type that gets serialized directly to one 32 byte chunk. Same goes for the `genesis_validators_root` which is serialized directly as 32 bytes. and the same for `slot` which is a uint64 and serialized to 32 bytes (8 bytes of data and 24 bytes of zero-padding). However, the next field in `beacon_state` is `Fork` which is a container. `Fork` contains 3 fields (`current_version`,`epoch`,`previous_version`). This means the "tree" of objects has some additional depth here compared to the simple types. The Merkle tree representing the `beacon_state` object will contain a root in the node immediately right of the leaf representing `slot` and this node will have two children, ech of which will have two children. The reason for this is because there are three fields that must converge to a single root at the depth of the `slot` leaf. This means each field must hash with another. The number of leaves must be a power of two so that each leaf has a hash partner, so at the bottom level we need four values to represent the three fields. The fourth is 32 bytes of zeros. Then the two pairs of values hash together to produce two hashes that themselves hash together to provide the root representing the `Fork` object. Diagramatically, for this subset of the `beacon_state`:

```
                __root__
             /            \
            /              \ 
           /                \
          /                  \
         node1                node2
         hash                  hash
       /       \                 / \
      /         \               /   \
     /           \             |     \
   leaf1          leaf2       leaf3   node3 
  genesis_time  gen_val_root  slot    hash
  as bytes       as bytes    as bytes  /   \
                                      /     \    
                                     /       \    
                          node4 hash          node5 hash
                         /      \            /         \    
                        /        \          /           \            
                    leaf4       leaf5      leaf6           leaf7
             current_version    epoch     previous_version    32-byte zeros
               as bytes         as bytes     as bytes           
```


Importantly, the values hashed together in the Merkle tree respresentation of the object ARE the serialized representations of the data - they are serialized into 32 byte chunks specifically to enable hashing and Merkleization.

Instead of referring to these tree elements as leaf X, node X etc, we can give them generalized indices, starting with root = 1 and counting from left to right along each level. This is the generalized index explained above. Each element in the serialized list has a generalized index equal to `2**depth + idx` where idx is its zero-indexed position in the serialized object and the depth is the number of levels in the Merkle tree, which can be determined as the square root of the number of elements (leaves). 

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

For the light client server, we do not need to conduct any actual proofs, but we do need to serialize the `state` object and determine the generalized indices for the `next_sync_committee` and `finalized_root` fields, so that the multiproofs can be achieved by the light client after receiving the update object from the light server. [ssz_rs](https://github.com/ralexstokes/ssz_rs) appears to have the required functionality to SSZ serialize the `state` object. Below I have listed each of the keys in the `state` object and their types.


<br>
<br>

## The beacon_state object

The state obect to be serialized has the following fields. This information is useful for conceptualizing the serialization.
<b>(TODO: reorder according to spec )</b>

```

key                               |    type         |   fixed/var size   | 
-------------------------------------------------------------------------------------
balances                          |   vec<u64>      |     fixed          |  
block_roots                       |   vec<vec<u8>>  |     variable       |  
current_epoch_participation       |   vec<bool>     |     fixed          |  
current_justified_checkpoint      |   Checkpoint    |     fixed          |  
    epoch                         |   u32           |     fixed          |  
    root                          |   vec<u8>       |     fixed          |  
current_sync_committee            |   SyncCommittee |     fixed          |     
    pubkeys                       |   vec<u8>       |     fixed          |  
    aggregate_pubkey              |   vec<u8>       |     fixed          |  
eth1_data                         |   Eth1Data      |     fixed          |
    deposit_root                  |   vec<u8>       |     fixed          |  
    deposit_count                 |   u64           |     fixed          |  
    block_hash                    |   vec<u32>      |     fixed          |  
eth1_data_votes                 |vec<u8,u64,vec<u32>>|     ??            |  
eth1_deposit_index                |   u64           |     fixed          |  
finalized_checkpoint              |   Checkpoint    |     fixed          |  
    epoch                         |   u64           |     fixed          |  
    root                          |   vec<u8>       |     fixed          |  
fork                              |   Fork          |     fixed          |
    current_version               |   vec<u8>       |     fixed          |  
    epoch                         |   u64           |     fixed          |  
    previous_version              |   vec<u8>       |     fixed          |  
genesis_time                      |   u64           |     fixed          |  
genesis_validators_root           |   vec<u8>       |     fixed          |  
historical_roots                  |   vec<vec<u8>>  |     ??             |  
inactivity_scores                 |   vec<u64>      |     fixed          |  
justification_bits                |   vec<u8>       |     fixed          |  
latest_block_header               |   BlockHeader   |     fixed          |  
   Slot                           |   u64           |     fixed          |  
   proposer_index                 |   u64           |     fixed          |  
   parent_root                    |   vec<u8>       |     fixed          |  
   state_root                     |   vec<u8>       |     fixed          |  
   body_root                      |   vec<u8>       |     fixed          |  
next_sync_committee               |   SyncCommittee |     fixed          |  
    pubkeys                       |   String        |     fixed          |  
    aggregate_pubkey              |   String        |     fixed          |  
previous_epoch_participation      |   vec<bool>     |     fixed          |  
previous_justified_checkpoint     |   Checkpoint    |     fixed          |  
    epoch                         |   u64           |     fixed          |  
    root                          |   String        |     fixed          |  
randao_mixes                      |   vec<u32>      |     fixed          |  
slashings                         |   vec<u64>      |     ??             |  
slot                              |   u64           |     fixed          |  
state_roots                       |   vec<String>   |     ??             |  
validators                        |   vec<u64>      |     fixed          |  

```
<br>
<br>

## Outstanding Questions:

