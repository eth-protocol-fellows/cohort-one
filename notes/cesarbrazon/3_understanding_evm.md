## EVM

Ethereum Virtual Machine is an execution environment where the execution of logic happens. 
Bytecode is the input the EVM expects, and it uses an interface to interact with Ethereum Network (It can be EEI or EVMC).

It updates the state of the chain - It receives any state when initiating and based on that input it 
can search and do computations based on existing data

When the EVM recieves the bytecode, it translate it to opcode and then interprets it, in the end it will send the computed data (Transaction) to the 
Ethereum Mainnet through an Ethereum Client-VM Connector API.

The implementation of the EVM is called the Execution Layer. Currently (before the merge), the EVM is attached to the Consensus Layer, 
which is the Proof of Stake algorithm (Called Ethash). A new API has been defined to modularize these two layers - 
This API is called Engine JSON-RPC API (https://github.com/ethereum/execution-apis/tree/main/src/engine).

The objective of developing this API is that now we can detach these two layers and this allows Ethereum to be a modular blockchain,
allowing developers to compose different implementations of EL & CL to achieve even more descentralization between nodes; this
will also open a new world for roll-ups, because you can just interact with the consensus layer in order to store data in mainnet but 
the execution layer can be a zkEVM, for example.