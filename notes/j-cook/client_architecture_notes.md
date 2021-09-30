

# Networking

# ethereum core
  ## block processing
    ### block validator
    ### tx processor
    ### bytecode executor
    ### evm
  ## synchronizer
  ## consensus
    ### pow
    ### pos
    ### poa

# storage
# wallet

# json-rpc

user interacts with the client via the json-rpc
networking allows the information stored in the client to be transferred to other nodes, and to receive data


## how much of this does a light client persist?
full json-rpc
synchroniser
transaction pool
no storage except for a small cache
no consensus
partial implementation of the networking layer - receives blocks, validates them, send transactions
includes minimal subset of the current world state.

does a light client add transactions to the mempool or does it delegate that to a full node?

A full node only includes the current world state and does not do consensus
An archive node has the full historical world state
A mining node has all the above plus participates in consensus and create new blocks

## links
https://www.youtube.com/watch?v=VH0obZ2A0Yg&list=PLNLh1EyDzSGP-lkNCBhCptoJ-NMu_BYfS&index=2