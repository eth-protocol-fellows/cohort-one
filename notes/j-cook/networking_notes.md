

# Networking

### DevP2P

Ethereum specific peer-to-peer protocol, transmits RLP encoded information between nodes. 

### Discovery

The process of finding other nodes in network
Step 1 is to send a message to a boot node. Boot nodes exist to bootstrap the entrance of a new node into the network by making introductions with peers. They do not participate in syncing the chain. A new node sends a message to a boot node, signifying that it wants to connect to the main network. The boot node then passes a list of potential peers the new node can connect to. The interactions between the new node and the boot nodes use a mechanism very similar to Kademlia, which is a distributed hash table.

After this discovery phase has completed, the new node has a network of connected peers. The boot nodes are not needed again because networked nodes can request new peers from other nodes they are already connected to.

### Sub protocol negotiation

Between connected peers, decide which sub protocols are supported. This process makes the system as a whole very extensible, as any sub-protocol can be implemented as long as connected peers agree to support them equivalently. These are the common sub-protocols:

#### Eth (wire)
The core Ethereum transaction protocol that enables sending and receiving blocks, making transactions and related metadata

#### Whisper
Secure messaging between peers without writing any information to the blockchain - many clients do not support this

#### Swarm
Swarm is a decentralised filesystem

Clients that support these sub-protocols exose them via the json-rpc