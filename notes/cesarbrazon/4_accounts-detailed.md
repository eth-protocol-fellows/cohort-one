# Accounts

Ethereum has two type of accounts: The externally owner account (EOAs) and contract accounts.

EOA are those that have a private key: Having the private key means control over access to funds. Actually, the EOAs are the only type of accounts that can initiate a transaction in Ethereum, the reason is that in order to send a transaction, the account needs to sign it with a private key. Currently, ethereum uses ECDSA (Elliptic Curve Cryptographic Signature Algorithm) to sign the transactions.

On the other side, Contract Accounts, has arbitrary code associated to it and it's owned and controller by the logic of it. This code is stored in Ethereum blockchain at contract account's creation address, and executed by the EVM.

The main differences between EOA and Contract Accounts is that EOA does not have code associated to it, and Contract Accounts are not associated with any private key

// @TODO: Explain how Ethereum store accounts 

