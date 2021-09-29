# Notes

- [This issue](https://github.com/ethereum-cdap/cohort-zero/issues/64) that [`pipermerriam`](https://github.com/pipermerriam/) has documented caught my attention. Since I am a data person, this seemed like doable.
    * Pipermerriam stated the probability that transaction tracing APIs can be used, but in [this tutorial](https://ethereum.org/en/developers/tutorials/learn-foundational-ethereum-topics-with-sql/) 
        > The account balance on Etherscan comprises regular transactions and internal transactions. Internal transactions, despite the name, are not actual transactions that change the state of the chain. They are value transfers initiated by executing a contract (source). Since internal transactions have no signature, they are not included on the blockchain and cannot be queried with Dune Analytics.

        it seems like the contract transactions are not recorded. It will be harder to use transactions API to fully compute the cost. However, this might be negligible.
    * Another option that is offered is to modify a client code and export the related contract operations. This will require inserting custom code, writing operations into an independent db or a file. Also, some kind of snapshot can be used if possible to decrease the total computation and increase the efficiency in debugging process.
    * Another approach would be to analyse contract codes, using a script to estimate the operations and their counts for selected popular contracts.

## Research Questions

- How to select and filter contracts to be analyzed?
- Is it possible to use a light node and request necessary data?
- What is the size of the data that will be analyzed?
- What would be schema of the data that is exported to be analyzed easily, or should the analysis be computed on the fly?
- Find details about the current cost, how many versions to compare?


### Additional Discussions might be usefull for implementation

https://ethereum.stackexchange.com/questions/3417/how-to-get-contract-internal-transactions
https://ethereum.stackexchange.com/questions/4446/instrumenting-evm
https://github.com/Arachnid/etherquery