# Notes on the go-ethereum Project

The goal of this document is to provide high-level documentation and overviews of the go-ethereum project. 

[Ethereum Block Structure](https://i.stack.imgur.com/7eiCt.jpg)

This document is intended for internal learning only, and by no means should serve as "official" documentation or protocol characterization. 

## geth Packages

- [accounts](https://github.com/ethereum/go-ethereum/tree/master/accounts) - Implements high-level Ethereum Account Mgmt. 
- ~~build~~
- ~~cmd~~ 
- common
- consensus - Implements different Ethereum consensus engines
- console
- core
- crypto
- ~~docs~~
- eth - Implements the Ethereum Protocol
- ethclient - Implements and provides a client for the Ethereum RPC API
- ethdb - Defines the interfaces for an Ethereum data store
- ethstats - Implements the network stats for reporting service
- event - Implements subscriptions to real-time events 
- graphql - Provides a GraphQL interface to Ethereum node data
- internal
- les - Implements the [Light Ethereum Subprotocol](https://github.com/ethereum/devp2p/blob/master/caps/les.md)
- light - Implements on-demand retrieval capable state and chain objects for Ethereum Ligh Client
- log - log15 toolkit for best-practice logging in Go
- metrics - Go port of Coda Hale's Metrics library
- miner - Implements Ethereum block creation and mining 
- mobile
- node - Sets up multi-protocol Ethereum nodes
- p2p - Implements the Ethereum p2p network protocols
- params
- rlp - Implements the RLP (Recursive Linear Prefix) serialization format
- rpc - Implements bi-directional JSON-RPC 2.0 on multiple transports
- signer
- swarm
- tests
- trie - Implements Merkle Patricia Tries
