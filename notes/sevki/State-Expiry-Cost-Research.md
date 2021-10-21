# [State-Expiry-Cost-Research](https://github.com/ethereum-cdap/cohort-zero/issues/64)

There is a need to calculate how the state expiry updates would affect the gas cost of contracts. We will have a high level analysis here about how the new approach affects the gas cost. Please comment on any aspect if you feel there is missing major parts in the analysis.

We will first present the findings for your convenience, you can continue to read for the foundations. 

You can see one of the outdourced effort on this here: https://www.youtube.com/watch?v=HipI3vCd8_Q

## Findings

We see that, in the actual road map, we will end up charging code access cost as we do in an access storage and address events (i.e. Charging per chunk access count). In the preliminary document (EIP-witness-gas-cost see below) there are complex calculations that take into account the code access and its gas cost which are based on bytes read. However, these are not implemented yet. In addition to that in EIP-2929, it is argued that byte charging is not feasible, and we can charge chunk access counts. Hence, there should be some assumptions about the cost estimation of existing contracts that will take into account code access costs. We can figure out these assumptions and calculate an estimated contract cost accordingly.

As a conclusion we can argue that this kind of witness charging will not be backwards compitable and would increase the cost of every contract that has code access which is by default, all contracts. In addition to that, the cost will increase linearly for each other `CallCode` if the chunk access counts is charged, otherwise, it is proportional to the code size.


## Current Gas Calculation

From the [EIP-1559](https://eips.ethereum.org/EIPS/eip-1559) we can see that:
>The intrinsic cost of the new transaction is inherited from EIP-2930, specifically 21000 + 16 * non-zero calldata bytes + 4 * zero calldata bytes + 1900 * access list storage key count + 2400 * access list address count.


## State Expiry Gas Calculation

Since there seems no EIP related to this, we will take [the notes](https://notes.ethereum.org/@vbuterin/witness_gas_cost_2) as base and call it EIP-witness-gas-cost.

In this document, the major difference is the access of the contract code, which is not charged for now:
>This EIP introduces a cleaner and simpler gas cost schedule that directly charges for accessing a subtree and accessing an element within the subtree. For (1) and (2), this EIP does not greatly change costs and is largely a simplification, as EIP 2929 already raised gas costs for those operations to a sufficiently high level. For (3), however, it does add substantial gas cost increases, as contract code access is not properly charged for without this EIP.

When we look at the EIP-2929 we see that there is an argument for not to charge for bytes read for witnesses:
>Opcode costs vs charging per byte of witness data
The natural alternative path to changing gas costs to reflect witness sizes is to charge per byte of witness data. However, that would take a longer time to implement, hampering the goal of providing short-term security relief. Furthermore, following that path faithfully would lead to extremely high gas costs to transactions that touch contract code, as one would need to charge for all 24576 contract code bytes; this would be an unacceptably high burden on developers. It is better to wait for code merklization to start trying to properly account for gas costs of accessing individual chunks of code; from a short-term DoS prevention standpoint, accessing 24 kB from disk is not much more expensive than accessing 32 bytes from disk, so worrying about code size is not necessary.

## Action Items

TBD

## Further Steps

1 - Investigate data export tools, 
[Resource](https://medium.com/coinmonks/solution-to-extract-data-from-ethereum-52d0b8007d1b):<br />
[EthSlurp-a.k.a. trueblocks](http://ethslurp.com/) - a client build on top of Infura <br />
[Infura](https://infura.io/docs/ethereum) All client APIs, including archive nodes, can be useful, but a paid acoount might be needed for an extensive study<br />
[Presto](https://github.com/xiaoyao1991/presto-ethereum) - SQL based meta querying platform<br />
[The Graph](https://thegraph.com/docs/developer/query-the-graph) - primitive, seems usefull for dApps<br />

2 - We need to study further these EIPs:<br />
[EIP-2929](https://eips.ethereum.org/EIPS/eip-2929)<br />
[EIP-2930](https://eips.ethereum.org/EIPS/eip-2930)<br />
[EIP-2200](https://eips.ethereum.org/EIPS/eip-2200)<br />

3 - Go deeper in EVM<br />
4 - Investigate clients, which one is suitable for this task (Trin? Fluffy? ...)
