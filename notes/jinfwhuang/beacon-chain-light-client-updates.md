## Beacon Chain Light Client Implementation Updates

All codes are in this PR:
https://github.com/jinfwhuang/prysm/pull/5

### Completed
1. Figured out prysm's protobuf code generation mechanism
1. Figured out prysm's ssz code generation mechanism
1. Wrote light-client proto objects: LightClient service, LightClientUpdate, SkipSyncUpdate, etc.
1. Configured prysm's client to serve light client api endpoints
1. Wrote a small library for fixed fifo queue
1. Use a fixed fifo queue to store LightClientUpdate objects
1. Verify basic ssz serder and merkleiation functionality of ztyp library
1. Compare ztyp with fastssz
1. Wrote help functions for generalized indices
1. Wrote merkle proof generation and verification mechanism for BeaconChain using ztyp library 
1. Implement SkipSyncUpdate objects
1. Fully implemented API 'ethereum.eth.v1alpha1.LightClient.SkipSyncUpdate' 
1. Configure light-client node able to take in CLI args
1. Configure light-client to serve GRPC APIs
1. Update light-client store.snapshot
1. Finishing the sync logic
1. Be able to recover from previous snapshot
1. Create an easy way to get a starting point for first time light-client user


### Next
1. Clean up the sync-logic
2. Start a conversation about LES style networking ...


https://github.com/ethereum/consensus-specs/pull/2762

