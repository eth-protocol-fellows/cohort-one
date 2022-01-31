# Cesar Brazon Notes

**DISCLAIMER:** These notes are my understanding of Ethereum based on what I have studied. If you see
something do not makes sense please feel free to reach out to me in discord (Cesar#8320) or twitter (@cesarbrazon)
so I can improve this documentation accordingly :-)

## Objectives

- [Describe the Ethereum architecture](./1_ethereum-architecture.md)
- [Understand what are the benefits of the upcoming improvements in Ethereum Protocol](./2_benefits-ethereum-upgrades.md)
- [Describe the EVM in depth](./3_understanding_evm.md)
- [Formalize Account Migration research](./4_account-migration.md)
- Implement a proof of concept of the formalization of AA research

## Areas of Interest

- EVM
- Sharding
- Layer two solutions (Roll ups, zkSnarks, Plasma Channels)
- Accounts Experience (AX)
- Account Abstraction
- Meta transactions

## General notes

- RLP is the main encoding method used to serialize objects in Ethereum
- Everytime a new transaction is triggered, the first opcode that runs is `JUMPDEST`
- Some positions in the code are marked valid to `JUMPDEST`, but there are some restrictions - you need to know the valid destinations before caling the `JUMPDEST`, otherwise it will revert
- The payload of a normal transaction is `RLP([nonce, gas_price, gas_limit, to, value, data, v, r, s])`
- Mempool: Is where transactions go before they are included on chain


## Questions

Q: What's the difference between precompiles and opcode?

A: Precompiles are contracts which are embedded into clients with default addresses. Opcodes are instructions+ stored in the storage of the EVM, and it's what allow us to interact with the stack of the EVM.
We can say that precompiles are predefined bytecode, which allow us to create more complex logic (like `ecrecover`) and opcodes are native functions that allow us to interact directly with the EVM

Q: How does the versionin current works in the EVM?
A: It's a way to add upgrades to the EVM with backwards compatibility

Q: How can we interact with WASM through roll ups?

A:

Q: How does `JUMPDEST` opcode exactly works? Why is it needed?

A: Jump dest marks a valid destination for jumps. This means that before trying to access to a new memory slot, the EVM checks it is available with this opcode


### Resources

- Understanding the Ethereum Protocol - https://www.youtube.com/watch?v=gjwr-7PgpN8
- Ethereum Illustrated - https://takenobu-hs.github.io/downloads/ethereum_evm_illustrated.pdf
- The limits to blockchain scalability - https://vitalik.ca/general/2021/05/23/scaling.html
- Ethereum stack - https://ethereum.org/en/developers/docs/ethereum-stack/
- Shard chains - https://ethereum.org/en/eth2/shard-chains/
- EVM - https://ethereum.org/en/developers/docs/evm/
- Beacon chain - https://ethereum.org/en/eth2/beacon-chain/
- Rollup-centric roadmap - https://ethereum-magicians.org/t/a-rollup-centric-ethereum-roadmap/4698
- Validation focused Smart Contract Wallets - https://ethereum-magicians.org/t/validation-focused-smart-contract-wallets/6603
- EIP 3074 - https://eips.ethereum.org/EIPS/eip-3074
- Native meta transactions - https://ethresear.ch/t/native-meta-transaction-proposal-roundup/7525/5
- EIP 3541 - https://eips.ethereum.org/EIPS/eip-3541
- CallWithSigner EIP - https://ethereum-magicians.org/t/eip-callwithsigner-as-a-potential-fix-for-the-msg-sender-problem/4340/2
- EOF Overview - https://notes.ethereum.org/@ipsilon/evm-object-format-overview
- EOF Discussion - https://ethereum-magicians.org/t/evm-object-format-eof/5727
- In-depth understanding of EVM storage mechanism and security issues - https://coinyuppie.com/in-depth-understanding-of-evm-storage-mechanism-and-security-issues/
- Ethereum 2.0: A complete guide - https://medium.com/chainsafe-systems/ethereum-2-0-a-complete-guide-d46d8ac914ce
- Ethereum 2.0: Scaling part 1 - https://medium.com/chainsafe-systems/ethereum-2-0-a-complete-guide-3739a74be61a
- Ethereum 2.0: Scaling part 2 (Sharding) - https://medium.com/chainsafe-systems/ethereum-2-0-a-complete-guide-scaling-ethereum-part-two-sharding-902370ac3be
- Ethereum 2.0: Caster and the Beacon chain - https://medium.com/chainsafe-systems/ethereum-2-0-a-complete-guide-casper-and-the-beacon-chain-be95129fc6c1
- Ethereum 2.0: Ewasm - https://medium.com/chainsafe-systems/ethereum-2-0-a-complete-guide-ewasm-394cac756baf
- Account abstraction via alternative mempool - https://notes.ethereum.org/@vbuterin/alt_abstraction
- Typed Transaction Envelop - https://github.com/ethereum/EIPs/blob/master/EIPS/eip-2718.md
- Contract code size limit - https://github.com/ethereum/EIPs/blob/master/EIPS/eip-170.md
- EVM Object Format: https://github.com/ethereum/EIPs/blob/master/EIPS/eip-3540.md & https://notes.ethereum.org/@axic/evm-object-format
- How accounts works? - https://twitter.com/iam_preethi/status/1454455451739906055


## Idea for PoC

Implement a fork in the EVM where the user can send a message with any signature and the transaction would be mined and the fees paid by the contract with the PAYGAS opcode