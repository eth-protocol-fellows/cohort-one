# Why do we want to migrate away from EOAs


- With EOAs users do not needs to pay for gas when onboarding in Ethereum, if we would move to "Smart Wallets", which are Smart Contracts with EOA capabilities, the user will need to deploy this SC to create his account

- With EIP-3074, one EOA can be authorized on many contracts simultaneously. 

### What are the problems we want to solve?

- Execution of transactions without paying gas
- Batched transactions
- Post-quantum resistance
- 

### Trying to understand differences between EOAs and Contract Accounts

- With EOAs the only way to recover an account is through the private key/seed phrase/mnemonic; all of these different words play the same game: If they are compromised or lost the user wont be able to access his funds again. By removing the EOAs and using "Smart Wallets", any arbitrary logic can be used to recover the account, removing the single point of failure, the blockchain itself will be easier and more comfortable for users because errors can be reversed (From accounts POV).

- With EOAs users do not needs to pay for gas when onboarding in Ethereum, if we would move to "Smart Wallets", which are Smart Contracts with EOA capabilities, the user will need to deploy this SC to create his account

- Currently, Ethereum uses ECDSA to verify the authentication of signers in transactions, this leaves Ethereum vulnerable to post-quantum computers (because ECDSA is not post-quantum resistant), to mitigate this, a change in Ethereum needs to happen - The most viable path of action is Account Abstraction, this is because it introduces a new Transaction Type which not depends on ECDSA, instead, contracts implements their own authentication logic meaning that they can expect a signature using a different algorithm, which can be post-quantum resistant. 
This means that, if the path of AA is not taken, the protocol itself will need to change eventually to support a new type of signature algorithm, making the Smart Wallets implementation _probably_ inevitable?

### Pros and cons

**Moving forward with 3074**

[According to Vitalik](https://ethereum-magicians.org/t/we-should-be-moving-beyond-eoas-not-enshrining-them-even-further-eip-3074-related/6538), this will mean that we will have a big technical debt, because we are adding two opcodes that are EOA opinionated, and if we eventually want to remove them, the contracts that uses these new opcodes will be useless, because they will expect a signature that is created with a private key.

On the other side, as mentioned before, this does not gives us a good solution for post-quantum resistance.

Even though for the long term the EIP 3074 does not seems like the best solution, it can be the first step to improve Ethereum Account Experience, which is a functionality that will help to mass adoption and easier interactions with the network. 


### Resources

- https://www.argent.xyz/blog/self-custody-mass-adoption/
- https://eips.ethereum.org/EIPS/eip-3074
- https://ethresear.ch/t/tradeoffs-in-account-abstraction-proposals/263
- https://ethereum-magicians.org/t/validation-focused-smart-contract-wallets/6603