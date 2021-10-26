# Understanding Ethereum architecture

As in any complex software, the Ethereum Protocol is divided into different layers. In the Ethereum documentation [[1]](https://ethereum.org/en/developers/docs/ethereum-stack/) we can see an overview of the different pieces of the stack.

- Decentralized Applications (dApps)
- Client APIs
- Nodes & Clients
- Smart Contracts
- EVM

## dApps

Decentralized Applications are the User Interface of Ethereum. You can see it as the way to interact with it in a friendly way.

The main difference with traditional apps is that dApps interact with Ethereum through JSON-RPC [[2]](https://ethereum.org/en/developers/docs/apis/json-rpc/) requests, instead of a backend server through HTTP requests or WebSocket connection.

- Client APIs: Libraries that allows interaction with Ethereum in different programing languages

## Nodes & Clients

According to [ethereum.org](https://ethereum.org/en/developers/docs/nodes-and-clients/#what-are-nodes-and-clients)

```
"Node" refers to a running piece of client software. A client is an implementation of Ethereum that verifies all transactions in each block, keeping the network secure and the data accurate."
```

Nodes are what makes Ethereum really descentralized, they are software running all over the world; they can mine blocks and verify data.

A consensus algorithm has been implemented so the nodes can interact between them in a secure way, guaranteeing that the network is robust enough to handle malicious actors. The incentive for a node to be honest is that, if they mine a block they will be rewarded by the network.

**Consensus**

Currently, Eth 1.0 uses Proof of Work as the consensus algorithm, meaning that nodes that wants to be miners needs to have the entire history of Ethereum stored. Meaning that, when a new block is mined, **all** the nodes are updated with the new information. This (between a lot of more reasons) makes the Proof of Work consensus not the best option if we want Ethereum to be scalable. With Serenity (Eth 2.0) this problem will be solved with Casper (PoS) & Sharding.

**MEV** (Miner Extractable Value)

When a miner gets the nonce to produce a new block into the chain, it has the ability to reorder the transactions.
This allow the miner to arbitrarily reorder transactions, insert its own transactions before or after other transactions, and delay transactions outright until the next block, and it turns out that there are a lot of ways that one can earn money from this; hence, making it a consensus-layer risk

## EVM

Is a lightweight virtual machine - The purpose of it is that it can run anywhere without depending on any hardware or operating system. It cannot access network, file system or other processes; acting as an independent sandbox.

### Bibliography

- [1] - https://ethereum.org/en/developers/docs/ethereum-stack/
- [2] - https://ethereum.org/en/developers/docs/apis/json-rpc/
