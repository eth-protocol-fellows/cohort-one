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


### Next
1. SkipSyncUpdate objects
1. Implement functioning Light Client APIs
