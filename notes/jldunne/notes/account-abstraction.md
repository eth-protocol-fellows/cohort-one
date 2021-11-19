# Account Abstraction

It has long been the goal of Ethereum to provide a general purpose blockchain. The reasoning behind this is to provide something as future-proof as possible - even if society decides that DeFi is no good after all, developers can program something else on Ethereum. However, there are a number of places within Ethereum that are not as abstract as they could be.

What if people were allowed to use any cryptographic verification algorithm that they wanted? This could be done by adding a piece of verification code to the contract. It seems like the bigger advantage here is to stop transactions that are invalid before they make it to the blockchain, rather than have them go to the blockchain without any effect.

There have been a few different early proposals for currency and code and data abstraction, allowing for increases in generality. Users are free to implement their own cryptographic signatures, and ether is moved up a level of abstraction, so that it is treated the same as subtokens.

**Project Ideas:**

- testnet implement an experimental fork to tstart testing implementations of AA
- bring AA sandbox up to date with the spec
- do something with new alt_abstraction proposal: produce sandbox or experimental VM

## Link Tree

### 2021
- [Proposal for account abstraction via alternative mempool, Vitalik Buterin](https://notes.ethereum.org/@vbuterin/alt_abstraction)

### 2020
- [Implementing Account Abstraction as part of Eth1.x, Vitalik Buterin](https://ethereum-magicians.org/t/implementing-account-abstraction-as-part-of-eth1-x/4020)
- [Meta Transactions x Account Abstraction](https://hackmd.io/@matt/S1Jg85588)
- [Account Abstraction, Stateless Mining Eth1.x/Eth2 Implementation, Rationale Document, Will Villanueva](https://hackmd.io/y7uhNbeuSziYn1bbSXt4ww?view)
- [AA Open Questions, Ansgar Dietrichs](https://hackmd.io/@adietrichs/Byd91DvKI)
- [DoS Vectors in Account Abstraction (AA) or Validation Generalisation, a Case Study in Geth, Will Villanueva](https://ethresear.ch/t/dos-vectors-in-account-abstraction-aa-or-validation-generalization-a-case-study-in-geth/7937)
- [EIP-2938: Account Abstraction, Ansgar Dietrichs](https://ethereum-magicians.org/t/eip-2938-account-abstraction/4630)
- [EIP-2938: Account Abstraction, Vitalik Buterin, Ansgar Dietrichs, Matt Garnett, Will Villanueva, Sam Wilson](https://eips.ethereum.org/EIPS/eip-2938)
- [EIP-2938: Account Abstraction Explained, Sam Wilson](https://hackmd.io/@SamWilsn/ryhxoGp4D)
- [Account Abstraction Beyond EIP-2938, Sam Wilson](https://hackmd.io/@SamWilsn/S1UQDOzBv#Read-write-Calls)
- [Account Abstraction (EIP-2938): Why and What, Rajeev Gopalkrishna](https://our.status.im/account-abstraction-eip-2938/)

### 2019
- [Maximally simple account abstraction without gas refunds, Vitalik Buterin](https://ethresear.ch/t/maximally-simple-account-abstraction-without-gas-refunds/5007)
- [EIP-86, Alex Beregszaszi](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-86.md)

### 2018
- [A new account type in abstraction, Vitalik Buterin](https://ethresear.ch/t/a-new-account-type-in-abstraction/1379)
- [A recap of where we are at on account abstraction, Vitalik Buterin](https://ethresear.ch/t/a-recap-of-where-we-are-at-on-account-abstraction/1721)
- [Application-layer account abstraction, Justin Drake](https://ethresear.ch/t/application-layer-account-abstraction/1734)
- [Layer 2 Gas Payment Abstraction, Vitalik Buterin](https://ethresear.ch/t/layer-2-gas-payment-abstraction/4513)

### 2017
- [Tradeoffs in Account Abstraction Proposals, Vitalik Buterin](https://ethresear.ch/t/tradeoffs-in-account-abstraction-proposals/263)
- [Account abstraction, miner data and auto-updating witnesses, Justin Drake](https://ethresear.ch/t/account-abstraction-miner-data-and-auto-updating-witnesses/332)

### 2016
- [Spending gas from contract's balance - proposed optional opcode for a contract, Alex Beregszaszi](https://github.com/ethereum/EIPs/issues/61)
- [EIP-101 (Serenity): Currency and Crypto Abstraction, Vitalik Buterin](https://github.com/ethereum/EIPs/issues/28)

### 2015
- [On Abstraction, Vitalik Buterin](https://blog.ethereum.org/2015/07/05/on-abstraction/)
