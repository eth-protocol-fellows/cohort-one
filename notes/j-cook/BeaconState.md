## The Beacon State Object
### (With specific reference to Lighthouse/Rust)

The `beacon_state` object is a core struct that must be agreed upon across the entire network as it represents the current state of the Beacon Chain. The root of its Merkleized form is used to validate a Beacon (light) client's view of reality. For this reason, the `beacon_state` has a spec that defines the type, size and order of each value stored within it. A Beacon client must parse the `beacon_state` object conforming precisely to the spec or else its root hash will not match the rest of the network.

In Lighthouse the state object is a custome type `BeaconState<MainnetEthSpec>`. The pat inside the <> symbols indicates that the type is configured for the Ethereum mainnet rather than a minimal version for testing. This type is a container with several fields of several types including some sub-containers. In order to ssz-serialize and merkleize this object, the first task is to determine which fields are fixed-length and which ones have variable lengths, as this determines whether the field data or an offset value is assigned to each field's position when the object is serialized. Below is a list of the fields inside `beacon_state` showing whether it is fixed or variable length. 
This is important because fixed-length fields are serialized "in place" whereas variable length fields have an "offset" in place of their actual data. The actual data is appended to a heap at the end of the serialized object. The offset defines where in the serialized object the data begins.

```
BeaconState(Container):
-------------------------------------------------------------------------------------------------------------------------------
field                        | LH Type                                                          | Fixed/Variable length?
-------------------------------------------------------------------------------------------------------------------------------
genesis_time                 | u64                                                              | Fixed
genesis_validators_root      | Hash256                                                          | Fixed
slot                         | Slot (wraps a u64)                                               | Fixed
fork                         | Fork                                                             | Fixed
  |- previous_version        | [u8;4]                                                           | Fixed
  |- current_version         | [u8;4]                                                           | Fixed
  |- Epoch                   | Epoch (wraps u64)                                                | Fixed
latest_block_header          | BeaconBlockHeader                                                | Fixed
  |- slot                    | Slot (wraps a u64)                                               | Fixed
  |- proposer_index          | u64                                                              | Fixed
  |- parent_root             | Hash256                                                          | Fixed
  |- state_root              | Hash256                                                          | Fixed
  |- body_root               | Hash256                                                          | Fixed
block_roots                  | FixedVector<Hash256, T::SlotsPerHistoricalRoot>                  | Fixed
state_roots                  | FixedVector<Hash256, T::SlotsPerHistoricalRoot>                  | Fixed
historical_roots             | VariableList<Hash256, T::HistoricalRootsLimit>                   | Variable
eth1_data                    | Eth1Data                                                         | Fixed
  |- deposit_root            | Hash256                                                          | Fixed
  |- deposit_count           | u64                                                              | Fixed
  |- block_hash              | Hash256                                                          | Fixed
eth1_data_votes              | VariableList<Eth1Data, T::SlotsPerEth1VotingPeriod>              | Variable
eth1_deposit_index           | u64                                                              | Fixed
validators                   | VariableList<Validator, T::ValidatorRegistryLimit>               | Variable
balances                     | VariableList<u64, T::ValidatorRegistryLimit>                     | Variable
randao_mixes                 | FixedVector<Hash256, T::EpochsPerHistoricalVector>               | Fixed
slashings                    | FixedVector<u64, T::EpochsPerSlashingsVector>                    | Fixed      
previous_epoch_participation | VariableList<ParticipationFlags, T::ValidatorRegistryLimit>      | Variable
current_epoch_participation  | VariableList<ParticipationFlags, T::ValidatorRegistryLimit>      | Variable
justification_bits           | BitVector<T::JustificationBitsLength>                            | Fixed  // SHOULD BE VARIABLE??
previous_justified_checkpoint| Checkpoint                                                       | Fixed 
  |- epoch                   | Epoch (wraps u64)                                                | Fixed
  |- root                    | Hash256                                                          | Fixed
current_justified_checkpoint | Checkpoint                                                       | Fixed
  |- epoch                   | Epoch (wraps u64)                                                | Fixed
  |- root                    | Hash256                                                          | Fixed
finalized_checkpoint         | Checkpoint                                                       | Fixed
  |- epoch                   | Epoch (wraps u64)                                                | Fixed
  |- root                    | Hash256                                                          | Fixed
inactivity_scores            | VariableList<u64, T::ValidatorRegistryLimit>                     | Variable
current_sync_committee       | Arc<SyncCommittee<T>>                                            | Fixed
  |- pubkeys                 | FixedVector<PublicKeyBytes, T::SyncCommitteeSize>                | Fixed
  |- aggregate_pubkey        | PublicKeyBytes (from bls lib, wraps [u8; PUBLIC_KEY_BYTES_LEN] ) | Fixed
next_sync_committee          | Arc<SyncCommittee<T>>                                            | Fixed
  |- pubkeys                 | FixedVector<PublicKeyBytes, T::SyncCommitteeSize>                | Fixed
  |- aggregate_pubkey        | PublicKeyBytes (from bls lib, wraps [u8; PUBLIC_KEY_BYTES_LEN] ) | Fixed

------------------------------------------------------------------------------------------------------------------------
```

An ssz serialization algorithm will work through the `beacon_state` in the order presented above. For each fixed-length field, the data is converted into raw bytes. If the data is smaller than 32 bytes, then some data from the next field will be appended to make the serialization space efficient.  For each variable length field, the offset is calculated and added in place of the field's data in the serialization object. For example, for a container `myContainer ={ field1: fixed, field2: variable, field3: fixed}` the serialized object will be ordered as follows: `field1 data, field2 offset, field 3 data, field 2 data`.

