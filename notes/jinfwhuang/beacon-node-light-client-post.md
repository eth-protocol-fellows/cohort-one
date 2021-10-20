# Beacon Chain Light Client Classifications

A beacon chian light client allows access to Ethereum on resource-constrained environment, e.g. phones, metered VM, or browsers. A light-client should be defined with respect to the set of APIs that it could handle.

I could not find any posts or spec documents discussing the full scope of functionality that the beacon-chain light client is supposed to cover. Instead of providing that scope, I am listing out a reasonable progression of the functionalities that a light-client could. It ranges from the minimal client to a client resembling a full node.

For each of the level of functional completeness, I also sketch out what kind input data is required to support the corresponding APIs. The input data could be fetched from one of two ways. One way is through a req/response mechanism from the client to a server. The other way is to use a p2p network. This document does not make any suggestion on the merits of either approach. 

This document does not contain implementation details. Implementation details would include what internal database light-clients build, how to process the input data, and how to compute answers to queries.


## Section 1: Classification

### Level 1: Minimum
The minimum consensus light-client syncs to the latest header using sync_committee signatures. The client can only answer questions with the information contained in the BeaconBlockHeader.


#### Supported APIs
- /eth/v1/beacon/headers
- /eth/v1/beacon/states/{state_id}/root

#### Data Input Requirements
- BlockHeaders
- SyncAggregate


### Level 2: Access BeaconState
The client is able to answer most of the questions with regard to the [BeaconState](https://github.com/ethereum/consensus-specs/blob/master/specs/altair/beacon-chain.md#beaconstate). The light-client is also able 

#### Supported APIs
The following list not exhaustive, but it is representative of the set of functions that this client intends to support.
```
​/eth​/v1​/beacon​/genesis
​/eth​/v1​/beacon​/states​/{state_id}​/root
​/eth​/v1​/beacon​/states​/{state_id}​/finality_checkpoints
​/eth​/v1​/beacon​/states​/{state_id}​/validators
​/eth​/v1​/beacon​/states​/{state_id}​/validators​/{validator_id}
​/eth​/v1​/beacon​/states​/{state_id}​/validator_balances
​/eth​/v1​/beacon​/states​/{state_id}​/committees
​/eth​/v1​/beacon​/states​/{state_id}​/sync_committees
​/eth​/v2​/beacon​/blocks​/{block_id}
​/eth​/v1​/beacon​/blocks​/{block_id}​/root
​/eth​/v1​/beacon​/blocks​/{block_id}​/attestations
```

#### Data Input Requirements
- Updating header
  - BlockHeaders
  - SyncAggregate
- Answering query on BeaconState
  - It is not likely that the light-client wants to keep an up-to-date copy of BeaconState.
  - For a given query, the light-client needs access to the corresponding merkle proof of BeaconState.
  - The light-client interprets the merkle proofs to produce an answer to BeaconState queries


### Level 3: Support Attestation Client
The beacon-chain light-client is able to support a validator that is only performing attestation tasks. 

Attestation makes up the bulk of the workload of a validator client. If a "attestation client" could be supported by a light weight beacon-chain client, it could open the door to allow a majority of the validation tasks to be run on resources constrained devices. The rest of the validation tasks, e.g. block proposing and sync committee signing, could be backed by a small set of full nodes with higher resources requirements.

#### Supported APIs
The client needs to support all the queries required to complete the attestation duties.
```
​/eth​/v1​/beacon​/states​/{state_id}​/validators​/{validator_id}
​/eth​/v1​/validator​/duties​/attester​/{epoch}
​/eth​/v1​/validator​/attestation_data
```

#### Data Input Requirements
- Updating headers
  - BlockHeaders
  - SyncAggregate
- Find out committee assignment
  - The client needs to access BeaconChainState.
  - It does not need to have access to the full state.
  - Specifically, it will need to access the random mix and the validator's index.
- Need to request the full blocks that it is attesting
  - The client does not need to know every blocks. It only needs to know the blocks that the attestation-client is assigned to attest

### Level 4: Limiting Case: Approaching Full Node
It would be ideal if a light weight beacon-chain client could support all the API needs of a validator client. Furthermore, the same light weight client is also able to answer queries with regard to the current state of transient gossiped data. However, this view might not be realistic as some of these functionalities will fundamentally require the client to process large amount data.

These functionalities are not likely to be supported by light weight clients. However, it is still instructive to list out these classifications of functionalities to understand where should be the design boundary of light weight clients.

#### Supported APIs
1. Support attestation aggregators, sync-committee duties, and block proposing
2. Beacon-node for slash proposers
3. Be able to answer queries about beacon-state as well as providing summarizing statistics about transient data


#### Data Sources
1. Full support for validator-client
  - The client has to maintain a pool of various gossiped data
  - attestation
  - slashing gossip
  - sync-committee

2. Support for slash proposers
  - Need to keep a lot of historical attestations around

3. Beacon-chain oracle
  - The client maintains a full beacon-chain state
  - The client maintains the most up-to-date db of all the gossip messages.
  - sync committee signatures
  - attestations
  - slashings
  - voluntary exists


## Section 2: Input Data Sources

### Level 1: Minimum
#### Sources 1: Light client specific messages from a server
- LightClientSnapshot
- LightClientUpdate

#### Sources 2: Portal network gossip channels
We create light weight gossip channels on the portal network. These gossip channels have low traffic volume. Participants could further restrict the percentage of messages that they receive based on their computing resources. These gossip channels do not exist yet.

Topics and wire format mapping:
- portal topic: light-client-snapshot
  - The message format is: LightClientUpdate
  - A message is created every epoch
- portal topic: light-client-update
  - LightClientUpdate
  - A message is created every slot

#### Sources 3: gossip topics intended for full nodes.
- It might look a bit strange to consider using the gossip topics intended for full nodes as input sources for light-clients. It is worth a consideration because these beacon-chain gossip topics do not have high volume data. If the light-client limits its number of peer connections, it is possible that the resource requirement is quite low. That being said, this is listed out mostly for completeness sake. The other two data sources should give better results.
- beacon-chain topic: `sync_committee_contribution_and_proof`
  - This topic is not high volume
  - These messages contain sufficient data to update from one block header to the next
- beacon-chain topic: `beacon_block`
  - The number of messages is not high, about 1 per slot under optimal conditions.
  - The messages are relatively large. Each message is a full block.
- The `beacon_block`topic let the light-client knows which header to update to, and the information in ``sync_committee_contribution_and_proof` allows the light-client to extract sufficient up-to-date sync-committee signature to validates the update to the any header. It could also be argued that the light-client only needs to listen to `beacon_block` in order to advance the header because the full block also contains sync-aggregate messages. However, those sync-aggregate data only points to the parent root. The light-client would not be able to skip sync with only the information contained in the `beacon_block` topic.

#### Related links
All of the following links implicitly assume that the minimal light-client implmentation will use a server-client approach operating on the messages LightClientSnapshot and LightClientUpdate. It is worth noting that it is not the only way to get to a minimal light-client.

- Minimal light client in the consensus-spec
https://github.com/ethereum/consensus-specs/blob/dev/specs/altair/sync-protocol.md
- prysm light-client [prototype](https://github.com/jinfwhuang/prysm/pull/2/files)
- lodestar's prototype
  - https://hackmd.io/hsCz1G3BTyiwwJtjT4pe2Q?view
  - https://github.com/ethereum/consensus-specs/issues/2429
- Alex Stoke's writeup on how to boostrap the light client ecosystem
https://notes.ethereum.org/@ralexstokes/S1RSe1JlF


### Level 2: Access BeaconState
#### Sources 1: server
- Updating header
  - LightClientSnapshot
  - LightClientUpdate
- Querying BeaconState 
  - Each beaconstate query will correspond to a small set of merkle proof requests.

#### Sources 2: p2p
- Updating header
  - BlockHeaders gossips
  - SyncAggregate gossips
  - It could also use alternative input messages:
    - LightClientSnapshot
    - LightClientUpdate
- Answering query on BeaconState
  - Participate in a DHT. The key-value pair is <content_id, beacon-state-merkle-proof>
  - The node stores a small subtree of the state.
  - If the participant could choose the `node_id`, the participant could selectively store the subtree that the client is most interested in.
  - Queries that fall outside of the subtree requires the client to make DHT requests to the p2p network
  - The p2p network participation has to be light-weight.
  - It probably makes the most sense to design the p2p network to be compatible with the portal network.


### Level 3: Support Attestation Client
#### Sources 1: server
- Update header, make quest to server
  - LightClientSnapshot
  - LightClientUpdate
- Proxy the committee assignment query to server
- Request full blocks from server

#### Sources 2: p2p
- Updating header
  - BlockHeaders gossips
  - SyncAggregate gossips
- Find out committee assignment
  - Participate in a DHT. The key-value pair is <content_id, beacon-state-merkle-proof>
- Get full blocks
  - It has to be gossip network because the attestors do not know the block root a priori.
  - The gossip channel could be a per-slot subnet to limit resource usage.
