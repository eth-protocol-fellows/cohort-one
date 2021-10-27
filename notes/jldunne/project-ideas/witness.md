# Witness

## Test cases for client implementations of wit/0
#### Prior Work
- [The Formal Witness Spec: An Illustrated Primer](https://notes.ethereum.org/bjRBM5IxQQOgqp7bez15xQ)
- [Ethereum Witness Protocol](https://github.com/ethereum/devp2p/blob/master/caps/wit.md)
- [Creating a State Test with retesteth](https://github.com/ethereum/retesteth/wiki/Creating-a-State-Test-with-retesteth)

#### Description
Create a set of tests or test data that clients can use to verify their implementation of the `wit/0` protocol and witness specification. This could be a standalone tool or test suite. It could also be included in `retesteth` as a state test.

#### Motivation
As of May, it seems that clients are implementing the `wit/0` protocol. Based on some messages in `#witness` on Discord, there are no tests available for clients to ensure that their implementations are correct. Teams are verifying their client implementations against each other.

From what I can see, some/most clients have already implemented the basic protocol, but not the entire specificiation. It may therefore be worthwhile to produce some common tests so the client teams can verify changes to their implementation quickly and easily.


## Validate that EIP-3584 is sufficient for witnesses
#### Prior Work

- [EIP-3584: Block Access List](https://eips.ethereum.org/EIPS/eip-3584)
- [Block Access List v0.1](https://ethresear.ch/t/block-access-list-v0-1/9505)
- [EIP-2929: Gas cost increases for state access opcodes](https://eips.ethereum.org/EIPS/eip-2929)
- [EIP-2930: Optional Access Lists](https://eips.ethereum.org/EIPS/eip-2930)

#### Description
There have been a number of EIPs around the topic of block access lists. Most recently, EIP-3584 has included a proposed construction of a canonical block access list. This project would validate that the construction set out in this EIP ensures that witnesses are verifiable against the header data. This would first be done for Merkle proofs, as they are well documented and understood, and then potentially extended to Verkle if time allows.

#### Motivation
This analysis has been identified in both the EIP and the corresponding `ethresear.ch` thread as a necessary next step before this EIP can be implemented. I don't see anyone actively working on this - maybe because there are two other prior EIPs that need to be done before this one is, and in general time is being taken up by the merge. Hopefully this would answer the outstanding questions on this EIP and unblock some witness-related efforts when the time comes for people to focus on it again.