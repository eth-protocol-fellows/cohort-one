# Why do we want to migrate away from EOAs

- With EOAs the only way to recover an account is through the private key/seed phrase/mnemonic; all of these different words play the same game: If they are compromised or lost the user wont be able to access his funds again. By removing the EOAs and using "Smart Wallets" mechanisms like social recovery, the blockchain itself will be easier and more comfortable for users because errors can be reversed (From accounts POV).

- With EOAs users do not needs to pay for gas when onboarding in Ethereum, if we would move to "Smart Wallets", which are Smart Contracts with EOA capabilities, the user will need to deploy this SC in order to create his account

- With EIP-3074 we can have a relationship of one to many. This means that an EOA account can have `authorized` on many contracts 

- With EIP-3074 two new opcodes will be introduced (`AUTH` and `AUTHCALL`) which is less intrusive than creating a new transaction type (this is the EIP-2938)


### Resources

- https://www.argent.xyz/blog/self-custody-mass-adoption/
- https://eips.ethereum.org/EIPS/eip-3074
- https://ethresear.ch/t/tradeoffs-in-account-abstraction-proposals/263