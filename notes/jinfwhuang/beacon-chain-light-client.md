# Beacon Chain Light Client

## Writings, Research, Discussions
- How to think about the beacon chain light client landscape
  - [Light client classifications](https://ethresear.ch/t/beacon-chain-light-client-classification/11061)
  - [Light client networking](https://ethresear.ch/t/beacon-chain-light-client-networking/11063)

- Bootstrapping the beacon chain sub-networks using the portal network core designs
  - [Initial beacon chain spec](https://github.com/ethereum/portal-network-specs/pull/99)
  - [Skip sync and initial sync](https://github.com/ethereum/portal-network-specs/pull/102)

- Proposals
  - [Light client proposal post in ethresear](https://ethresear.ch/t/a-beacon-chain-light-client-proposal/11064)
  - [A work in progress proposal](https://www.notion.so/prysmaticlabs/Prysm-Light-Client-v1-9720501820da412d8cb12ffe54eba235)


## POC
I am building light client on the Prysm codebase. 
Work in progress: https://github.com/jinfwhuang/prysm/pull/5/files

### Goal
I want to build a light client that:
- Has a trustless setup
- Be able to answer API queries about the latest beacon state
- Have a working networking layer for the light client to acquire input data, i.e. light-client-updates, skip-sync-updates, and merkle proofs of beacon state. 
- The initial networking layer will be json rpc and grpc. 
- It is not yet decided if I will pursue in developing the spec that is similar to LES, using discv5 + libp2p.
- I will definitely pursue implementing the portal networks specific to the beacon chain light client.

### Implementation Plan
###### Step 1
Be able to run the server and get back some new data types; both grpc and http json. I am deviating slightly from what the [Lodestar team has done](https://github.com/ChainSafe/lodestar/tree/mpetrunic/lightclient-sync-protocol). I had discussions with those developers through writing the spec in the portal network. We are going to pursue using data types that could be better adopted in a peer to peer networking environment. See [discussion](https://github.com/ethereum/portal-network-specs/pull/102).

The key endpoints are:
```
GET /eth/light-client/v1/skip-sync-update?key=<content-key>
GET /eth/light-client/v1/updates
```

###### Step 2
- Implement client logic to use new data type to advance its head
- The light client serve some basic APIs, e.g.
```
GET /eth/v1/beacon/headers
```

###### Step 3
Use the proof mechanism to ask and interpret beacon-state-proof
```
Server: 
    - GET /eth/light-client/v1/beacon-state-proof?key=<content-key>
Light-Client: Be able to serve these APIs
    - GET /eth/v1/beacon/states/{state_id}/validators
    - GET /eth/v1/beacon/states/{state_id}/finality_checkpoints
    - ...
```

###### Step 4
- Design and implement a peer discovery mechanism
  - Similar to LES
  - Use discv5 + libp2p
- This step might be skipped to favor the implementation of step 5 instead. This will be determined later.

###### Step 5
Implement [portal network](https://github.com/ethereum/portal-network-specs/tree/master/beacon-chain) as the networking layer.


## Related Links:
- Lodestar
  - Lodestar has done the most in producing a POC
  - https://hackmd.io/hsCz1G3BTyiwwJtjT4pe2Q
  - https://github.com/ethereum/consensus-specs/issues/2429
  - https://medium.com/chainsafe-systems/lodestar-releases-light-client-prototype-40f300361c65
  - https://github.com/ChainSafe/lodestar/tree/mpetrunic/lightclient-sync-protocol

- Alex Stoke's writeup
  - https://notes.ethereum.org/@ralexstokes/S1RSe1JlF
  - https://notes.ethereum.org/@ralexstokes/HJxDMi8vY
