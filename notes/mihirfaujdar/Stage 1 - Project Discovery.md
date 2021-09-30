# **Stage 1 - Project Discovery**
The following document reveals the chosen project for Cohort One and explains the reasoning for progressing directly into "Stage 2 - Project Exploration" without exploring other ideas.    

## Chosen Project
In Github, Piper recommended a project idea about researching how to move forward with removing the 2300 gas stipend. It is an EVM feature that requires removal because it may become unusable due to future gas repricings. Currently, it does not achieve its intended purpose because most users still use EOA's (Externally Owned Accounts) that do not support logging. Furthermore, the opcode `SELFDESTRUCT` bypasses the stipend limit as [mentioned by Vitalk Buterin](https://hackmd.io/@vbuterin/evm_feature_removing#The-2300-gas-stipend).  

## Reasoning
Given the time constraints of the program and the upcoming merge event, it would be valuable to choose a unique project and make consistent progress towards achieving its goal. As mentioned by Piper in one of the CDAP meetings, finding a "good" project is a challenging aspect of the apprenticeship program as it needs to account for feasibility and meaningful contribution. Taking these into consideration, moving forward with this project seems sensible for making a contributing change to the protocol layer. 

## Current Strategy
After reading the discussions in the Ethereum Magicians forum, it was found that decreasing or setting the limit to 0 would cause backward compatibility issues for legacy contracts that rely on `.send` or `.transfer` for security. The upcoming post will provide additional details outlining the discussions relevant to the remediation of reentrancy attacks and increasing the gas stipend limit. For now, the approach is to:
* gain an understanding of how `CALL` opcodes are executed,
* calculate the average gas costs for `LOG` opcodes,
* read the first proposal mentioned by Arachnid in the [Remediation for EIP-1283 reentracy bug](https://ethereum-magicians.org/t/remediations-for-eip-1283-reentrancy-bug/2434) post.

## Resources
- https://github.com/ethereum-cdap/cohort-one/issues/28
- https://hackmd.io/@vbuterin/evm_feature_removing#The-2300-gas-stipend
- https://github.com/ethereum/EIPs/issues/1285