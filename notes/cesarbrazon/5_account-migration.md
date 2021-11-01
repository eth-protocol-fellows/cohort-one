# Account Migration

## EIP 3074

- Alternative to AA which is improving the UX of EOA
- We need to find a way to authenticate as an EOA in the execution.
- Allows you to pass the `msg.sender` through different jumps (or calls).
- Further downs the two type accounts
- With [Rich Transactions](https://ethereum-magicians.org/t/rich-transactions-via-evm-bytecode-execution-from-externally-owned-accounts/4025/) we can make EOA batch multiple transactions into one

## Account Abstraction

- Reduces client complexity - Account abstraction removes EOA. Authorization logic will be moved to Smart Contract execution layer
- If a new user wants to create a new wallet, he needs to pay for the deployment of the AA contract

With this EIP we want to add multiple improvements to the entire Ethereum stack, which I think the two more importants are:

- The capacity of a contract to pay for gas (natively), instead of having the EOA to pay for it every time
- We can add signature verification other than ECSDA which allows to make Ethereum post-quantum resistant

In order to implement AA, we require merging EIP 2937 or preserve nonce on self destruct

- Two categories: Single-tenant and Multi-tenants.
  Single-tenant are for wallet and basically an account per user case. Multi-tenants will aim in account for many users, like tornado cash or uniswap. AA support for multi-tenant applications requires more research and is proposed as future work; so first, single-tenant is the ony that's going to be developed

**Changes proposed for AA implementation**

- `NONCE` and `PAYGAS` opcodes will be introduced with this EIP
- Changes to mempool rules
- A new type of transaction would be added, they are called "AA transactions" and the payload should be `rlp([nonce, target, data])`

  The transaction would consists on two process:

  1. Authentication, is the process where the caller can indeed call this contract. This will happen in an isolated environment, throwing if the authentication process try to access memory, or write storage. This process should be a pure function, or only interact with precompiled contracts.
  2. Execution, here, the opcode `PAYGAS` will be triggered at first, and will calculate the gas price and gas limit for the transaction. Note that if the transaction fails, the contract will not be refunded.

### Resources

- https://blog.ethereum.org/2015/07/05/on-abstraction/
- EIP 101 - https://github.com/ethereum/EIPs/issues/28
- Implementing AA on Eth1.X https://ethereum-magicians.org/t/implementing-account-abstraction-as-part-of-eth1-x/4020
- EIP 2938 - https://github.com/ethereum/EIPs/blob/master/EIPS/eip-2938.md
- Spending gas from contract's balance - https://github.com/ethereum/EIPs/issues/61
- Account Abstraction: What & Why - https://our.status.im/account-abstraction-eip-2938/
- Account Abstraction with EOF - https://notes.ethereum.org/@axic/account-abstraction-with-eof
- Tradeoffs in AA - https://ethresear.ch/t/tradeoffs-in-account-abstraction-proposals/263/37
- Account Abstraction from main chain - https://github.com/ethereum/EIPs/issues/859
- Account Abstraction rationale - https://hackmd.io/y7uhNbeuSziYn1bbSXt4ww?view

## TODO:

- Why do we want to migrate away from EOAs? The first argument on post-quantum
- Read about State transition functions (EVM)