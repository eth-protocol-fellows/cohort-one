# Benefits of Ethereum 2.0

## Modular vs Monolithic blockchains

- Monolithic blockchain: Every blockchain that people is familiar with, is a blockchain that tries to do everything in the same stop, the 3 main things: Consensus (How the block are validated into the chain), Data Availability (How much space is avaiable per block), Execution (Computation of a blockchain, you have a block which have state of things and when a new block is added to the chain it needs CPU requirements).

With modular blockchains we want to componetize these three main things

- Consensus layer: Defines what is true (trust layer)
- Execution layer: Is what updates the chain through computation
- Data layer: Is basically what is already stored - Reliable state

Scalability trilemma
- Decentrilize
- Scalable
- Secure

how modular would work:
- The execution wont happen inside of the mainnet (even tho it can) - but all the execution will be moved to rollups 
- Data layer is improved through sharding: 


Rollup: Another chain that is secured by ethereum, it doesn't has to figure out data availabilty or consensus, it just takes care of execution. Bundle different transactions, compresses everything to the smallest size it can and then it sends it to mainnet. zkSnark are probably the best ones