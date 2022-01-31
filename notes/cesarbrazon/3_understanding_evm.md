## EVM

Ethereum Virtual Machine is an execution environment, is where the execution of logic happens.

Bytecode is the input the EVM expects, and it uses an interface to interact with Ethereum Network (i.e: EEI or EVMC). This allows
the EVM to update the state of the chain, by receiving any state on initialization and based on this init state, it 
can do modifications on the current state by 

When the EVM receives the bytecode, it translates it to opcode and then interprets it, in the end it will send the computed data (Transaction) to the 
Ethereum Mainnet through an Ethereum Client-VM Connector API. The opcodes interact with the stack of the VM by pushing or taking values from it. This is
what allows Ethereum to be Touring complete, because with the supported opcodes you can do any desired logic. It is worth mentioning that actually
Ethereum it's "quasi" Touring complete because the gas wont allow to do heave computation to prevent ddos attack to the network. 

The implementation of the EVM is called the Execution Layer. However, before the merge, the EVM is attached to the Consensus Layer, 
which is the Proof of Stake algorithm (Called Ethash). A new API has been defined to modularize these two layers - 
This API is called Engine JSON-RPC API (https://github.com/ethereum/execution-apis/tree/main/src/engine).

The objective of developing this API is that now we can detach these two layers and this allows Ethereum to be a modular blockchain,
allowing developers to compose different implementations of EL & CL to achieve even more decentralization between nodes; this
will also open a new world for roll-ups, because you can just interact with the consensus layer in order to store data in mainnet but 
the execution layer can be a zkEVM, for example.