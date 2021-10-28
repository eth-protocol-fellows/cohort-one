# EIP-2930 Annotated

This EIP adds a transaction type which contains an access list, a list of addresses and storage keys that the transaction plans to access. Accesses outside the list are possible but are penalised (become more expensive)
___
**What are transaction types in Ethereum?**
There are a few different types of transactions in Ethereum. Regular transactions - a transaction from one wallet to another, and contract deployment transactions - a transaction without a 'to' address, where the data field is used for contract code.

There are also typed transaction envelopes - a transaction type that is an envelope for future transaction types. In this new standard (EIP-2718), transactions are interpreted as: Transaction Type || Transaction Payload

where TransactionType is a number between 0 and 0x7f, for a total of 128 possible transaction types, and TransactionPayload is an arbitrary byte array defined by the transaction type.
___

### Abstract
This EIP introduces a new transaction type with the format

```
0x01 || rlp([chainId, nonce, gasPrice, gasLimit, to, value, data, accessList, signatureYParity, signatureR, signatureS])
```

The access list specifies a list of addresses and storage keys; these would be added into the `accessed_addresses` and `accessed_storage_keys` global sets (from EIP-2929). A gas cost is charged, though at a discount relative to the cost of accessing outside the list.

### Motivation
This EIP serves two functions:

1. Contract breakage risks introduced by EIP-2929 are mitigated - as transactions could pre-specify and pre-pay for the accounts and storage slots that the transaction plans to access.
2. Introduces the access list format, and the logic for handling that format. This logic can later be repurposed for many other purposes, including block wide witnesses, use in reGenesis, moving towards static state access over time etc

### Specification

#### Definitions

`TransactionType` 1

`ChainID` The transaction is only valid on networks with this chain ID

`YParity` The parity of the y-value of a secp256k1 signature

#### Parameters

`FORK_BLOCK` TBD

`ACCESS_LIST_STORAGE_KEY_COST` 1900

`ACCESS_LIST_ADDRESS_COST` 2400

As of `FORK_BLOCK`, a new transaction is introduced with `TransactionType` 1.

The `TransactionPayload` for this transaction is `rlp([chainId, nonce, gasPrice, gasLimit, to, value, data, accessList, signatureYParity, signatureR, signatureS]).`

The three signature elements of this transaction represent a secp256k1 signature.

The `ReceiptPayload` for this transaction is `rlp([status, cumulativeGasUsed, logsBloom, logs])`.

For the transaction to be valid, accessList must be of type `[[{20 bytes}, [{32 bytes}...]]...]`, where ... means “zero or more of the thing to the left”. 

___
**What does this mean in different syntax?**

Access list must have type list of 2 element lists, where the first elem is a 20 byte string, and the second element is another list of 32 byte strings.

___

```
[
    [
        "0xde0b295669a9fd93d5f28d9ec85e40f4cb697bae",
        [
            "0x0000000000000000000000000000000000000000000000000000000000000003",
            "0x0000000000000000000000000000000000000000000000000000000000000007"
        ]
    ],
    [
        "0xbb9bc244d798123fde783fcc1c72d3bb8c189413",
        []
    ]
]
```

At the beginning of execution, we charge additional gas for the access list: `ACCESS_LIST_ADDRESS_COST` gas per address, and `ACCESS_LIST_ATORAGE_KEY_COST` gas per storage key. For example, the above example would be charged `ACCESS_LIST_ADDRESS_COST * 2 + ACCESS_LIST_STORAGE_KEY_COST * 2` gas.

Non-unique addresses and keys are not disallowed, though they will be charged for multiple times, and there is no difference in outcome from multiple inclusion of a value.

The address and storage keys would be immediately be loaded into the `accessed_addresses` and `accessed_storage_keys` global sets.

This is done using the following logic (which is also a specification of validation of the RLP-decoded access list)

```
def process_access_list(access_list) -> Tuple[List[Set[Address], Set[Pair[Address, Bytes32]]], int]:
    accessed_addresses = set()
    accessed_storage_keys = set()
    gas_cost = 0
    assert isinstance(access_list, list)
    for item in access_list:
        assert isinstance(item, list) and len(item) == 2
        # Validate and add the address
        address = item[0]
        assert isinstance(address, bytes) and len(address) == 20
        accessed_addresses.add(address)
        gas_cost += ACCESS_LIST_ADDRESS_COST
        # Validate and add the storage keys
        assert isinstance(item[1], list)
        for key in item[1]:
            assert isinstance(key, bytes) and len(key) == 32
            accessed_storage_keys.add((address, key))
            gas_cost += ACCESS_LIST_STORAGE_KEY_COST
    return (
        accessed_addresses,
        accessed_storage_keys,
        gas_cost
    )
```
___
**What is this code doing?**
Given an access list, it will produce a tuple containing a list of addresses and address/storage key pairs, along with an int.

First, we set up our variables to empty or 0, accessed_addresses, accessed_storage_keys, and gas_cost.

We assert that the access_list parameter is in fact a list.

For item in the access_list, assert that each item is a list, and that it has length of 2. This corresponds to address and storage keys.

Store the address of the current item in a temporary address variable.

Verify that the address is bytes and it has length of 20.

Add it to the set of accessed addresses.

Do the same for the second items which is the keys - check that they aare a list, and that each is bytes and length of 32. 

We then add each key and its corresponding address as a key pair to the accessed_storage_keys set. Here, the gas_cost is also increased by adding the `ACCESS_LIST_STORAGE_KEY_COST`

We return a tuple of the sets of the `accessed_addresses`, `accessed_storage_keys` and gas cost.
___

### Rationale

Charging less for accesses in the access list is done to encourage transactions to use the access list as much as possible, and because processing transactions is easier when their storage reads are predictable.

Allowing duplicates maximises simplicity, avoiding questions of what to prevent duplication against.

Signature signs over the transaction type as well as the transaction data in order to ensure that the transaction cannot be "re-interpreted" as a transaction of a different type.

