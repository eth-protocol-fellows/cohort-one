# Accounts


## Playground for Mempool Account Abstraction
#### Prior Work
- [Proposal for account abstraction via alternative mempool](https://notes.ethereum.org/@vbuterin/alt_abstraction)
- [Quilt Team's Account Abstraction Playground](https://github.com/quilt/account-abstraction-playground)
- [EIP-2938 Account Abstraction Explained](https://hackmd.io/@SamWilsn/ryhxoGp4D)
- [Account Abstraction Beyond EIP-2938](https://hackmd.io/@SamWilsn/S1UQDOzBv)

#### Description
This project would create an experimental fork of go-ethereum, where the latest account abstraction proposal from Vitalik would be implemented. The plan would be to follow the Quilt team's strategy for feasibility of the original account abstraction work, except based on the new proposal.

#### Motivation
A member of the Quilt team recently confirmed on Discord that their account abstraction work has been paused until after the merge is complete. The new proposal appeared in July and there doesn't seem to be any further analysis on it. I think it would be cool to use what Quilt did as a framework to investigate the feasibility of the new proposal, produce a playground for others to use and experiment with, as well as have a head start on the analysis when this gets picked up again.


## Formalise different approaches to "Account Migration"
#### Prior Work
- [We should be moving beyond EOAs, not enshrining them even further (EIP3074-related](https://ethereum-magicians.org/t/we-should-be-moving-beyond-eoas-not-enshrining-them-even-further-eip-3074-related/6538)
- [Validation Focused Smart Contract Wallets](https://ethereum-magicians.org/t/validation-focused-smart-contract-wallets/6603)
- [EIP-3074](https://eips.ethereum.org/EIPS/eip-3074)
- [Project Idea: Formalize different approaches to 'Account Migration'](https://github.com/ethereum-cdap/cohort-one/issues/31)

#### Description
This project would analyse the different proposals and schools of thought around how to proceed with account abstraction (AA) and EIP-3074, and formalise each of the options. Note that this project was proposed as a `cohort-one` suitable project by lightclients. 

#### Motivation
EIP-3074 solves only a small part of what the overall AA effort needs to achieve. We would like to figure out how to improve/extend EIP-3074 so that it is as forward-compatible with the end goals of AA as possible, and reduces unnecessary technical debt.

The current proposals and suggested improvements/fixes are scattered in forum posts. This would provide a cohesive and central analysis that would hopefully aid future decision-making.