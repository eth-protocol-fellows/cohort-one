
# Networking

This doc contains notes on Ethereum's networking layer. The layer can be divided into two stacks: the first is the discovery stack which sits on top of UDP and allows a new node to find peers to connect to and the second is the DevP2P layer that sits on top of TCP-IP and allows nodes to exchange information. These layers act in parallel, with the discovery layer feeding new network participants into the network, and the DevP2P layer enabling their interactions.


### Discovery

#### Overview

Discovery is the process of finding other nodes in network. This is bootstrapped using a small set of bootnodes that are [hardcoded](https://github.com/ethereum/go-ethereum/blob/master/params/bootnodes.go) into the client. These bootnodes exist to introduce a new node to a set of peers - this is their sole purpose, they do not participate in normal client tasks like syncing the chain. On receiving a message from a new node, the bootnode returns a list of potential peers that the new node could connect to. Boot nodes only used the very first time a client is spun up - the node table is persisted so subsequent times the client is started up connections are made directly to real peers.

The protocol used for the node-bootnode interactions is a modified form of [Kademlia](https://medium.com/coinmonks/a-brief-overview-of-kademlia-and-its-use-in-various-decentralized-platforms-da08a7f72b8f) which uses a distributed hash table to share lists of nodes. The routing table is a database that stores the information required to find each node in the network (IP addresses, port and ID). A copy of this table is held by each node and it is divided into "k-buckets" where each bucket represents groups of neighbours that sit at different distances from the node. Distances between nodes are defined by the XOR metric (i.e. for each bit in a nodeID an XOR returns 0 if two nodes match and 1 if they differ - the sum of  XOR's bits gives the distance). Each node's routing table orders buckets (lists) of neighbours in ascending order of distance, and the node connects to its N closest peers. Randomness and regular refreshing of each node's routing table is implemented as a security feature - a random target is generated for each node and its peers are selected based on the distance to that target. The node actually requests thr 16 closest nodes to the target, then request the 16 closest nodes from each of those, giving 16 x 16 = 256 potential peers derived from each randomly generated target. The refresh period is a configurable variable in each client, with defaults in various clients ranging from ~5 mins to ~1 day.

In Ethereum, the node ID used in the routing table is actually a [Keccak hash](https://keccak.team/keccak.html) of the node's public key. Other significant differences between Ethereum's protocol and Kademlia include the nuances of the distance calculation, the process used to determine a node's reputation (which can vary between clients), and the implementation of bonding to protect against DDOS attacks. 

This discovery process uses just 4 messages,consisting of 2 requests and 2 responses: 

REQUESTS:

- PING 
- FIND-NEIGHBOUR

RESPONSE:

- pong
- NEIGHBOUR

These messages are exchanged using [UDP](https://en.wikipedia.org/wiki/User_Datagram_Protocol).

#### Bonding

Discovery starts wih a game of PING-PONG. A successful PING-PONG "bonds" the new node to a boot node. The initial message that alerts a boot node to the existence of a new node entering the network is a PING. This PING includes hashed information about the new node, the boot node and an expiry time-stamp. The boot node receives the PING and returns a PONG containing the PING hash. If the PING and PONG hashes match then the connection between the new node and boot node is verified and they are said to have "bonded". 

Once bonded, the new node can send a FIND-NEIGHBOURS request to the boot node. The data returned by the boot node includes a list of peers that the new node can connect to. if the nods are not bonded, the FIND-NEIGHBOURS request will fail, so the new node will not be able to enter the network.

Once the new node receives a list of neighbours from the boot node, it begins a PING-PONG exchange with each of them. Successful PING-PONGs bond the new node with its neighbours, enabling message exchange.

```
start client --> connect to boot node --> bond to boot node --> find neighbours --> bond to neighbours
```

## Interactions between nodes

After new nodes enter the network, their interactions are governed by protocols in the DevP2P stack. These all sit on top of TCP and include the RLPx transport protocol, wire protocol and several sub-protocols. RLPx is the transport layer responsible for initiating, authenticating and maintaining sessions between nodes. The discovery process finds RLPx enabled peers for nodes to communicate with. RLPx itself defines how nodes talk to each other. RLPx encodes messages using RLP (Recursive Length Prefix) which is a very space-efficient method of encoding data into a minimal structure for sending between nodes.

A RLPx session between two nodes begins with an initial cryptographic handshake. This involves the initiator node sending an auth message which is then verified by the recipient node. On a successful verification, the recipient node generates an auth-acknowledgement message to return to the initiator node. This is a key-exchange process that enables the nodes to communicate privately and securely. A successful cryptographic handshake then triggers both nodes to to send a "hello" message to one another. This message is part of the wire protocol. The wire protocol is initiated by a successful exchange of hello messages.

The hello messages contain:

 - protocol version
 - client ID
 - port
 - node ID
 - list of supported sub-protocols
  
This is the information required for a successful interaction because it defines what capabilities are shared between both nodes and configures the communication. There is a process of sub-protocol negotiation where the lists of sub-protocols supported by each node are compared and those that are common to both nodes can be used in the session.

Along with the hello messages, the wire protocol can also send a "disconnect" message that gives warning to a peer that the connection will be closed. The wire protocol also includes PING and PONG messages that are sent periodically to keep a session open.
The RLPx and wire protocol exchanges therefore establish the foundations of communication between the nodes, providing the scaffolding for useful information to be exchanged according to a specific sub-protocol.

### Sub-protocols

#### Eth (wire)
The core Ethereum transaction protocol that enables sending and receiving blocks, making transactions and related metadata

#### les (light ethereum subprotocol)
minimal protocol for syncng light clients

#### Whisper
Secure messaging between peers without writing any information to the blockchain - many clients do not support this

#### Swarm
Swarm is a decentralised filesystem



Clients that support these sub-protocols exose them via the json-rpc





### Discv4 and Discv5

Ethereum's current node discovery protocol is called Discv4. This is the protocol I have been referring to when making comparisons between Ethereum's protocol and Kademlia, for example. Discv4 has been implemented in Ethereum clients for several years. The Discv4 protocol includes the aforementioned reduced Kademlia-style distributed hash table for managing its neighbours










## libP2P

DevP2P and libP2P are both collections of protocols that can be used to define the networking layer of peer-to-peer systems. libP2P is a general-purpose, modular set of components that can be sed to build a wide range of p2p networks. libP2P did not exist when Ethereum's networking layer was first built - instead Ethereum was built on a bespoke set of packages designed only to make Ethereum work, rather than to make a generic p2p framework. These Ethereum-specific packages are collectively known as DevP2P and they have formed the backbone of Ethereum's peer-to-peer communication system since Ethereum's inception.


## light clients

The exisiting DevP2P LES network is designed using a client/server architecture – with light-clients as the clients, and full nodes as the servers (today, the term light-client is usually used to refer to a client of the existing DevP2P LES network).

Since this architecture puts all the load on full nodes, and full nodes are already heavy to run, most node runners don’t activate this option.

So while the current network design serves it’s original purpose well, it is severely lacking from a light-client perspective.





## Links:
networking video https://www.youtube.com/watch?v=hnw59hmk6rk&list=PLNLh1EyDzSGP-lkNCBhCptoJ-NMu_BYfS&index=3
kademlia to discv5 https://vac.dev/kademlia-to-discv5
eclipse attack paper (contains nice explanation of Geth p2p in 2018) https://eprint.iacr.org/2018/236.pdf
kademlia paper https://pdos.csail.mit.edu/~petar/papers/maymounkov-kademlia-lncs.pdf
intro to ethereum p2p video https://p2p.paris/en/talks/intro-ethereum-networking/