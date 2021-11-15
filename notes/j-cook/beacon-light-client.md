# Beacon Chain Light Client

## Purpose

The idea of a light client is to enable access to Ethereum by resource constrained devices, such as a wallet on a mobile phone or a very small daemon that could run with negligible resource capture on a normal laptop. The client should be able to participate in maintaining the chain:

- Does the light client participate in attestation? no only a validator does
- Does the light client participate in fork choice? not yet, constraining the light client to the last finalized block not the true head
- Can a light client be a validator (presumably not)?
- So why would someone run a beacon chain light client? Access to self-verified up to date chain info
- Must couple to an execution layer light-client for processing transactions? No, but light client net will have to serve execution data eventually
- Must be towards the stateless end of the statelessness continuum (low data storage) yes!
- Must have access to recent state information (subset thereof)? yes, by request to nodes
- Must have access to account trie (subset thereof)? eventually yes, when nodes serve execution data
- Must have access to receipt trie (subset thereof)? ditto
- Must be able to retrieve non-stored information from the p2p network: yes by request to nodes (or by searching portal network DHTs)
- Must be able to advertise stored information over the p2p network (not in gossip model, but yes in portal net model)
- Must put minimal load on peers and minimise interactions with full clients (yes in gossip/portal models)
- Must communicate over secure p2p channels with verificable peers (yes, either by connection to node, gossip or portal network)


A light client should keep up with the head of the chain and provide access to the most recent blockchain state. Specific information about account balances etc would be accessed by making requests for specific merkle branches from a full node. This might provide useful information about account balances etc that could influence a user to go back to their execution client to make a transaction.

Starting from a known block header, the task of a light client is to learn a future block header and verify its authenticity. The initial known block would have to come from some 3rd party for example a full Beacon node. Inside the block header is a state root, inside the state is the current and upcoming sync committee public keys. Verifying the merkle branch with the currenct sync committee verifies the identities (pubkeys) of the current committee, doing the same to the branch with the next sync committee and the leaf verifies the identity of the next committee. 

The pubkeys are aggregated into a single aggregate pubkey by the block proposer. This can be easiy validated by the light client by doing the aggregation of the exposed sync committee pubkeys again and comparing the result. The aggregate signature is combined with the next block header root to produce the block signature. So by recombining the aggregate signature and hashign it with the block header, the block signature should be reproduced identically. If so, you know the nxt block you found was definitely signed over by the right cync committee. It may not have been finalised, but it must have been justified. One model is to then walk back through the state via a merkle branch to find the most recent finalised block. This circumvents the need to incorporate a fork choice rule but at the cost of lagging a few epochs behind the real head of the chain.

light_client_update object emitted by full nodes contains all necessary info for this.


Design Ideas:

server/client:
- light client needs to connect to multiple full nodes for liveness redundancy - sigs could come directy from p2p network
- light clients do not need to trust the full node as they can verify the block signatures
- light clients needs to get committee signatures AND merkle branches from full nodes
- light client should go through dicovery and connect to peers in the same way as full nodes
- light clients can then make get requests to the full node to retrieve the update_light_client object
- the update_light_client object can then be deconstructed to verify signatures
- 
- build server that interacts with full beacon node by entering libp2p network and requesting relevant data
- this likely means populating a db that can in turn be queried by a light client
- in the portal network the p2p network becomes the light client server, by making data searchable and available in DHTs



Two lightness options: superlight client just tracks finalized blocks and necessarily has some lag behind the true tip of the chain, or light clients that track the true tip of the chain and are capable of reorgs where necessary between finalized checkpoints.

https://notes.ethereum.org/@vbuterin/HkiULaluS#Preserving-light-client-guaranteesteku

https://github.com/ethereum/consensus-specs/pull/2147 

https://github.com/ethereum/consensus-specs/issues/2182 

https://github.com/ethereum/consensus-specs/issues/2315 (light client checklist)

https://github.com/ethereum/consensus-specs/blob/dev/specs/altair/sync-protocol.md (light client spec)

DJR and VB on beacon light client models https://www.youtube.com/watch?v=iaAEGs1DMgQ

lodestar beacon light client details https://medium.com/chainsafe-systems/lodestar-releases-light-client-prototype-40f300361c65

Transactions are not handled directly by Beacon clients - transaction gossip, transaction mempool and block production happens at the execution layer. This suggests that a Beacon chain light client should not attempt to participate in transaction processing - this is in the domain of an execution layer (light) client. A Beacon chain light clent should listen to block gossip and participate in validating new blocks and selecting the head of the chain. This provides a light client operator with useful information about consensus.

It seems to me that submitting transactions has to happen via an execution layer client (light or otherwise) because transaction gossip still sits in the execution layer. Unless a Beacon loght client also participated in execution layer transaction gossip, I don't see how transactions could be emitted from the light client. Equally, since a Beacon light client sends execution packets down to the execution layer in order to validate blocks, it's not clear to me how a Beacon light client could participate in block validation. The only potential route to this functionality I can see is to either allow a light client to inject transactions into the execution layer transaction gossip protocol, or to establish a link between a consensus layer light client and an execution layer full client. Alternatively, the light clients could mirror the full client arcitecture in having both an execution and consensus light client running in parallel, potentially with the portal network as the execution layer p2p network and a new network for consensus p2p. Otherwise, the Beacon light client is restricted to passively reporting data from beacon blocks.



Q: how can we get *trusted* merkle roots for each block? (committees? only with finality?, explicitly check for 2/3 validator attestation?)
A: likely solved using the beacon block header, which contains a merkle root for each shard 



How lodestar supports light clients: https://hackmd.io/hsCz1G3BTyiwwJtjT4pe2Q