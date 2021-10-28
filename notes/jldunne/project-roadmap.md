# Project Roadmap

This work should cover 12-14ish weeks of effort.

### Stage 1 (~ 2 weeks)
- [] Run local development version of `py-evm`
- [] Capture extra data from transaction-level lists
- [] Collate transaction-level access lists to a block list
- [] Ensure that block-level access lists that match the EIP are being published by the experimental fork

### Stage 2 (~1 week)
- [] Capture extra witness metadata so that `AccessRootList` can be constructed from witness

### Stage 3 (~2 weeks)
- [] Write a reference implementation that
	- [] given a block number
	- [] accesses the witness via API
	- [] construct `AccessRootList` from this witness
	- [] verify that it is the same as what is in the block header

### Stage 4 (~3 weeks)
- [] Research what is needed to construct a full Merkle proof version of the witness
- [] Modify access list construction and validation function so that it can be verified against the new witness
- [] Document and publish how this works

### Stage 5 (~4 weeks)
- [] Research what is needed to construct Verkle proof version of the witness
- [] Make necessary changes to witness
- [] Prove this witness against the `AccessRootList` in the block header
- [] Document and publish how this works