#Project Description Document
## Validating EIP-3584 Block Access List against Witness

### Concise Summary

In [EIP-3584](https://eips.ethereum.org/EIPS/eip-3584), a construction and format for a block-level access list was proposed. However, the EIP stopped short of exploring how this would tie in with the longer-term vision for stateless Ethereum i.e. that witnesses can be verified against the access list that is present in each block header. 

This project aims to perform this exploration and validation, first by extending `py-evm` to conform to the access list structure set out in the EIP, and then experimenting with additional metadata in the witnesses in order for the proofs to work. The basic meta-witness, Merkle proof, and potentially Verkle if time allows, will be covered by this project.


### Rationale
Acceptance and implementation of EIP-3584 has a number of benefits, the most immediate of which is to help prevent griefing attacks on light or fast sync protocols. Making a block access list available in the header allows a node to verify that the data they are about to download corresponds to this block's witness. This EIP also plays a role in the stateless Ethereum effort, whereby the access list could be extended to include witness data.

However, there remain a number of unanswered questions about how this access list construction will work with various witness proofs. In order for this EIP to move forward, it is necessary to verify if this construction is sufficient for Merkle witnesses, as well as exploring any potential changes or extensions required for it to handle Verkle proofs.


### Details

#### Current State
To date, a client implementation of what is laid out in EIP-3584 has not been found. However, most of the major clients seem to have implemented the prior work on transaction level access lists, EIP-2929 and EIP-2930. It is therefore proposed to take this as a starting point, specifically the `py-evm` implementation.

Similarly, `wit/0` has been implemented on a number of clients, but to date this specification only covers what is known as meta-witnesses - information about block access that could conceivably be used to construct a witness. The `py-evm` implementation of `wit/0` will also be used for this project.


#### Project Plan 
This project naturally breaks down into two threads of work - one on the block access list construction, the other on the witness generation and protocol. The intent here is to define a number of project stages, each focusing on incremental improvement of one or both of these threads. The development work will take place on an experimental fork of `py-evm`.

#### Stage 1
Extend `py-evm` so that the transaction-level access lists defined in EIP-2929 and EIP-2930 can be collated into a format that matches EIP-3584. Specifically, we need the lists to be included in the block header, and to be in the format:

```
Set [ AccessedAddress, 
      List [AccessedStorageKeys] , 
      Set  [ AccessedInBlockTransactionNumber, List [ AccessedStorageKeys ]]  ]
```

**Goal:** The experimental fork of `py-evm` implements naive block access lists

#### Stage 2
Recall that the `AccessListRoot` is a hash of the block's access list. Given a block witness, we want to check if we can construct an `AccessListRoot` and verify that both match.

It is possible that the witness implementation in py-evm will need minor extension at this stage to capture some extra metadata e.g. the block transaction number.

**Goal:** An `AccessListRoot` can be generated from the existing witness specification. This can then be compared to the `AccessListRoot` in the block header.

#### Stage 3
Given a block number, call `GetBlockWitnessHashes` on it. This will produce the witness as verified in the previous stage. Figure out a programmatic reference implementation to verify that this witness corresponds to what is in the header for said block number.

**Goal:** Write a function that, given a block number, will access the witness via API, and the header for said block. It will then construct an `AccessRootList` from the witness and verify that it is the same as what is in the header.

#### Stage 4
Explore what would be required to construct a full Merkle proof witness for a block. Extend `witness.py` to produce this or something sufficient for a PoC. Extend access list construction and validation function to use this new witness.

**Goal:** An upgraded version of `witness.py` that produces something approximating a full Merkle proof. Modified access list construction and validation function that can be verified against this witness. Document how this verification was done and work to date for inclusion as an appendix to the EIP.

#### Stage 5 (Bonus Stage, time permitting)
Repeat the previous stage but for Verkle trees. This will likely require further research and may be outside of the time available for CDAP, but is the logical next step for this work.

**Goal:**
A PoC of a Verkle tree witness, and documented verification of the proof against the block access list.