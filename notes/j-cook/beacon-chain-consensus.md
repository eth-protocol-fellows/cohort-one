# Beacon Chain Consensus

This document explains how the Beacon Chain is used to come to consensus on the Ethereum blockchain.

## PoW and PoS

Currently, Ethereum uses a PoW consensus mechanism whereby miners compete in a competition to provide a valid inputs to recreate a specific hash. Specifically, miners are required to find values whose hashes begin with a given number of leading zeroes (more zeros = more difficulty) which can only be achieved by brute force (random guesses). Being fastest to compute that data gives the miner the right to add a block to the blockchain, with energy expenditure acting as a scarce economic resource provably expended. A crypto reward is given to the miner for successfully mining a block. Miners are more successful if they can make more guesses per unit time, so PoW promotes large energy expenditure (powering increasingly power-hungry hardware) and rapid hardware turnover. This is a primary reason for turning PoW consensus off in Ethereum. However, switching to an alternative consensus mechanism without compromising security, scalability or decentralisation is a major technical challenge. 

PoS is the consensus mechanism that will replace PoW. Instead of miners expending energy, network participants called "validators" stake crypto (specifically, 32 ETH) - effectively swapping out a proxy for capital (PoW solutions as proof capital was spent to power computation) for capital itself. The staked capital forces a network participant to have skin in the game - something at risk that incentivizes honesty. If a validator acts honestly, their ETH accumulates through rewards. If a validator acts dishonestly or fails to meet their responsibilities, their staked ETH will be decremented. The magnitude of the decrement scales with the severity of the infraction, with major violations resulting in "slashing" of substantial proportions of their stake.

In reality, PoW and PoS are not really consensus mechanisms, they are modes of securing the network by making it is infeasibly expensive for an attacker to modify the history of the blockchain. consensus itself comes from the fork-choice algorithm that picks a definitive head of the chain. Under PoW the definitive chain is the one that has had the most work done on it, often thought of as the "longest" chain but this is not always true. PoW is secure because the expenditure required by a malicious miner or cabal of miners to change a historical block and then continue to outpace honest miners to establish their amended chain as the one with the most work is infeasibly large. They would need to control >51% of the entire hashpower of the network, which requires a huge amount of hardware, energy expenditure and luck. Under PoS, validators "attest" to what they believe to be the head of the chain, the next block to add and a signature. The canonical chain is the one supported by at least 2/3 of the validators. If a validator misses an opportunity to attest they receive a small penalty. However, if they contradict themselves by signing multiple blocks simultaneously then they are slashed. The magnitude of the slashing depends on the number of validators involved - if 30% of all validators submit dishonestly 100% of the validator's stake is destroyed. Since a majority of validators is required to finalize a block into the canonical chain, a coordinated attack would put 100% of all coordinating attackers stakes at risk. At present there are ~250,000 validators and ETH is worth $3300. The cost of mounting a realistic attack can be crudely estimated as cost = 2/3* N_validators * ETH price * 32 = $15,840,000,000 (~$15B) for a double-spend (i.e. rewriting chain history) or 1/3* N_validators * ETH price * 32 = $7,920,000,000 ($~8B) to halt the chain (new blocks require 2/3 majority to finalize, controlling 1/3 validators could halt this process). There are then additional security mechanisms in place such as random shuffling of validators that make it extremely unlikley that a sufficient majority could coordinate an attack, the option for out-of-band social coordination of honest validators to organize a soft-fork and the simple fact that a successful attack of that size would devalue ETH to the extent that the totality of the stolen asset value would almost certainly be greatly exceeded by the value injected to mount the attack in the first place. Below this security layer is a consensus mechanism that selects the chain that has accumulated the most attestations in its history. 

Since validators are chosen at random, the computational race to provide a PoW proof is no longer required. Miners are no longer required to burn energy to secure the chain. Therefore, switching from PoW to PoS comes with a > 99.9% reduction in energy expenditure along with security and decentralization benefits.

### Aside: PoW for bootstrapping value

The transition to PoS is a monumental leap forwards for Ethereum, but it is important to acknowledge the role that PoW played in establishing Ether as a digital asset and Ethereum as a secure network. PoW was an important period in Ethereum's history because it bootstrapped the value of ETH. PoS requires that the staked asset has real-world value and liquidity, otherwise arbitrarily large amounts of ETH could be staked to take control of the blockchain with negligible risk of loss to the attacker (no problem to lose arbitrarily large amounts of valueless tokens to slashing). PoW provided a link between some tangible real-world value in the form of energy burned in mining early Ethereum blocks. This bootstrapped ETH becoming a valuable asset with liquidity in a secondary market - now the token is sufficiently valuable to protect the network. Other networks that have launched with PoS have had to rely on other methods to bootstrap value and liquidity into their native token such as yield farming incentives or capital from private investors.

## The Beacon Chain
### high level overview
<b>NB: The consensus specs can be a bit confusing as the various subdirectories correspond to various stages of development - specs relating to phase0 do not contain functionality for passing an execution payload, for example, because this is not needed until post-merge. These notes refer to post-merge Ethereum rather than phase0 unless otherwise stated. I will try to note which features are immediately post-merge and which are post-sharding. </b>

The Beacon Chain is a blockchain running in parallel to the main Ethereum chain that provides PoS consensus and finality to blocks of transactions. The Beacon Chain has been running since November 2020 alongside the Etheruem PoW chain, receiving and finalizing blocks already validated by PoW. In the near-future, the PoW consensus in the main Ethereum chain will be switched off - this event is known as the "merge". At that time, the Beacon Chain will command the Ethereum blockchain, setting the pace and rhythm of block production, managing fork choice and consensus and securing the network by governing the validators and their staked funds.

First, it is critical to understand that post-merge there will be two pieces of software required to participate as a validator. First, a validator must run an execution-client, which is a current Ethereum client with the PoW mechanism turned off. The execution layer will retain responsibility for listening for transactions, pooling them and bundling them into blocks, and supporting the EVM. A validator is also required to send 32 ETH to the deposit contract, which is a smart contract that lives on the execution layer. In addition to this execution client, a validator must run a consensus client. This is the software responsible for listening for new blocks, validating them, attesting to them and eventually finalizing them, and establishing the head of the canonical chain. The two clients must be connected by a local RPC connection so that the consensus client can make function calls to the execution client, and the execution client can execute transactions when instructed to by the consensus client. The consensus client has its own P2P network that connects it with other consensus clients, providing access to block gossip.

It is also important to know that the Beacon Chain has been designed in anticipation of sharding. Sharding is the subdivision of the main Ethereum blockchain into (initially 64) sub-chains which can develop in parallel. The Beacon Chain is responsible for coordinating all these shards. It was originally intended that sharding would precede the merge; however, the rapid development of scaling via rollups pushed the merge forwards in time such that sharding is now scheduled after the merge. The Beacon Chain will therefore merge with some redundant functionality, awaiting sharding. The current Ethereum mainnet will be one of the 64 shards. Since the current Ethereum mainnet and Beacon Chain are both already running and sharding is not yet implemented, the Beacon Chain, "Eth1" chain and shard chains will have different lengths and genesis blocks separated in time.

Post-merge, the Beacon Chain sets the tempo of block proposal and validation on Ethereum, so that the growth of the Beacon Chain and various shards synchronize in time. This arises from the separation of time into Ethereum-specific units.  The smallest unit of time is the "slot". Each slot is an opportunity for a block to be added to the Beacon Chain and each of the shards. Slots do not necessarily have to be filled, they can be skipped. These slots arise once very 12 seconds - assuming the system is running optimally, every 12 seconds each shard chain and the Beacon Chain itself will grow by one block. This means that time needs to be synchronized across the validators on the network. One epoch is equal to 32 slots, or 6.4 minutes, and 2028 epochs (9.1 days) makes up one Ethereum week ("eek"). 

In each slot, a subset of at least 128 validators are randomly assigned to a "committee" to validate each shard. Of the 128, 1 validator is assigned to propose a new block and 127 are assigned to validate it. Multiple committees of 128 validators can be assigned to the same block. This is organized at the eoch level, where available validators are evenly distributed across the slots in the epoch and then further split across the various shards. There is a reshuffling algorithm that alters the distribution of validators to ensure there are always at least 128 validators in every committee ([details here](https://ethos.dev/beacon-chain/)).

The block proposer's consensus client requests a block of transactions from the execution client. The execution client selects transactions from the mempool up to the total gas limit and executes them, passing the proofs back to the consensus client. The consensus client then transmits this list of transactions over the block gossip network to the other validators. These validators receive the block into their consensus client and pass the transactions to their execution client where they are executed. The consensus client checks that the hashes of the transactions match that proposed by the block proposer and, assuming they match, they attest to the block. 

Attesting means "voting" or a particular block to be added to the Beacon chain. The new block is transmitted over the consensus layer block gossip network as the next block to add to the chain, along with the hash of the validator's suggested current chain head. One of the validators in each committee is designated to be the aggregator. They collect all the attestations for the proposed block in their slot/shard into a single data structure ([`AttestationData`](https://notes.ethereum.org/@hww/aggregation#113-Aggregation)) that includes a list of validators and a bitlist showing which validator attested to the proposed block and an aggregate form of all their signatures. This aggregation is then broadcast over the gossip network. Once a 2/3 super-majority of the active validators have attested in support of the proposed shard-block, as demonstrated by the aggregation data, that block is added to the head of the shard chain and a crosslink is created that references the new addition to the shard block on the Beacon Chain.

In summary, each shard has its own blockchain. When instructed by the Beacon Chain, the validator assigned as the block proposer for that shard instructs its execution client to gather transactions into a block and execute them. The block is then gossiped to the other validators, who check the transaction validity and then sign an attestation that the block should be added to the canonical chain. Once 2/3 of the validators attest to the block it is added to the head of the shard chain and a crosslink is sent to the Beacon Chain referencing the new block as the head of the shard. This outlines how each shard's blocks grow over time, paced by the Beacon Chain. To understand in detail how this results in a Beacon Chain state transition and updating of the Beacon Chain itself, we need to dive into Beacon blocks and Beacon state.

## Beacon Blocks and Beacon State

A Beacon Chain state transition is handled by a `state_transition_function` function in the consensus client. The two inputs to this function are containers: `BeaconBlock` and `BeaconState`. The `BeaconBlock` is what is added to the Beacon Chain, with an updated `BeaconState` as one of its prerequisites.

The function takes the existing state and uses the information in the new block to return a new state.

```
def state_transition(state: BeaconState, signed_block: SignedBeaconBlock, validate_result: bool=True) -> None:
    block = signed_block.message
    # Process slots (including those with no blocks) since block
    process_slots(state, block.slot)
    # Verify signature
    if validate_result:
        assert verify_block_signature(state, signed_block)
    # Process block
    process_block(state, block)
    # Verify state root
    if validate_result:
        assert block.state_root == hash_tree_root(state)
```

These are the containers passed as inputs to `state_transition`:

```
BeaconBlock

class BeaconBlock(Container):
    slot: Slot
    parent_root: Root
    state_root: Root
    body: BeaconBlockBody
```

```
BeaconState

class BeaconState(Container):
    # Versioning
    genesis_time: uint64
    slot: Slot
    fork: Fork
    # History
    latest_block_header: BeaconBlockHeader
    block_roots: Vector[Root, SLOTS_PER_HISTORICAL_ROOT]
    state_roots: Vector[Root, SLOTS_PER_HISTORICAL_ROOT]
    historical_roots: List[Root, HISTORICAL_ROOTS_LIMIT]
    # Eth1
    eth1_data: Eth1Data
    eth1_data_votes: List[Eth1Data, SLOTS_PER_ETH1_VOTING_PERIOD]
    eth1_deposit_index: uint64
    # Registry
    validators: List[Validator, VALIDATOR_REGISTRY_LIMIT]
    balances: List[Gwei, VALIDATOR_REGISTRY_LIMIT]
    # Randomness
    randao_mixes: Vector[Bytes32, EPOCHS_PER_HISTORICAL_VECTOR]
    # Slashings
    slashings: Vector[Gwei, EPOCHS_PER_SLASHINGS_VECTOR]  # Per-epoch sums of slashed effective balances
    # Attestations
    previous_epoch_attestations: List[PendingAttestation, MAX_ATTESTATIONS * SLOTS_PER_EPOCH]
    current_epoch_attestations: List[PendingAttestation, MAX_ATTESTATIONS * SLOTS_PER_EPOCH]
    # Finality
    justification_bits: Bitvector[JUSTIFICATION_BITS_LENGTH]  # Bit set for every recent justified epoch
    previous_justified_checkpoint: Checkpoint  # Previous epoch snapshot
    current_justified_checkpoint: Checkpoint
    finalized_checkpoint: Checkpoint
```

The `BeaconBlock` is the unit of information that comprises the Beacon Chain. It contains the critical information required to archive the most recent changes to Ethereum. Inside the Beacon block are four objects: `slot` defines the slot on the Beacon Chain this block belongs to, `parent_root` is the merkle-root hash of the previous block (used to ensure this block has the right parent), `state_root` is the merkle-root hash of the Beacon State. The block proposer is expected to execute a state-transition before the block is proposed to a Beacon Node. The Beacon node also executes the state-transition and checks that these roots match. Finally, the `BeaconBlock` contains the `BeaconBlockBody`, an object that contains information about block validation, including proof that the validator participated in the RANDAO random selection function. `eth1_data` is grabbed from the BeaconState as a way to track the deposit contract on the eth1 blockchain.

There are also optional fields in the `BeaconBlockBody` container that are rewarded if filled. These relate to slashing events, new deposits to the deposit contract and validator's exiting the network.

```
class BeaconBlockBody(Container):
    randao_reveal: BLSSignature
    eth1_data: Eth1Data  # Eth1 data vote
    graffiti: Bytes32  # Arbitrary data
    # Operations
    proposer_slashings: List[ProposerSlashing, MAX_PROPOSER_SLASHINGS]
    attester_slashings: List[AttesterSlashing, MAX_ATTESTER_SLASHINGS]
    attestations: List[Attestation, MAX_ATTESTATIONS]
    deposits: List[Deposit, MAX_DEPOSITS]
    voluntary_exits: List[SignedVoluntaryExit, MAX_VOLUNTARY_EXITS]

```

In order for the `BeaconBlock` to be filled the `state_transition_function` has to have been executed to provide a `state_root`. This function updates the values in the `BeaconState` object and is probably the most important function in the entire workflow as it defines how the Beacon Chain changes when a block is processed. The state of the Beacon chain is actually quite limited in scope - all user-level state is contained inside the shard chains and is not explicitly perisisted as far as the Beacon state. Instead the Beacon state includes information about consensus, chain configuration and high-level information about the availability of shard-chain data.

The state transition function calls three function calls: `process_slots`, `verify_block_signature`, and `process_block`. 

`process_slots` checks that the new slot occurs after the current one, avoiding any clashes or anachronism. Then, the current state object's root hash is queried and used to define the previous root for the upcoming block. The same is done for the block hash. The purpose is to make sure the correct block ends up in the correct slot and that there is continuity from one block to the next by updating the `state_root` and `block_root arrays`. `verify_block_signature` is a simple check to ensure the block signature matches the hash of the block contents and the signer's public key. `process_block` is slightly more complex, with four nested function calls inside that collectively check that the block header is valid, generates a random seed for the next round of RANDAO, updates the proposer's view of the eth1 chain, executes any necessary slashings for proposers and validators, and does some basic validity checks on attestions (although actual processing of the attestations happens once an epoch, outside of this per-slot block processing function, and is explained later) [details here](https://benjaminion.xyz/eth2-annotated-spec/phase0/beacon-chain/#block-processing). Effectively, these per-block processing functions save up information ready to be death with at the next epoch boundary.

At the epoch boundary ,the `epoch_processing` function is executed. This includes a set of procedures that process the attestations saved up in the per-slot processing functions. First, all the attestations with valid source and target FFG-checkpoints are retained, others ignored (FFG is the finality gadget, explained later). Then, from the resulting set, those that attested to the block that became the head of the chain are selected. Those are the "unslashed attesters". This means that the attestations of any validator that gets slashed are filtered out. Then, the total attestation budget of the unslashed attesters is calculated - i.e. how many validators attested to the current head of the chain. This information is passed to a justfiation and finalisation function that checks whether 2/3 of the validator pool agreed on the head of the chain. If so, this block is "justified". A block becomes finalized when there are k justified blocks built on top of it.


## fork choice

Simultaneous to these slot, block and epoch processing events, each validator executes a fork choice algorithm called LMD-GHOST. Information about the head of the chain that comes from executing the fork-choice algorithm is required in order to make an attestation, since the attestation includes a vote on the head of the chain. Specifically, the `AttestationData` object contains a `beacon_block_root` field - this is the validators vote on the head of the chain after executing the LMD-GHOST algorithm, and `source` and `target` fields that correspond to the latest justified block and first-block-in-current-epoch, both of which are determined by LMD_GHOST. The `epoch_processing` function cannot justify and finalize blocks or distribute rewards and penalties without this information.

`get_head` is the main fork-choice function and its purpose is to return the chain since the last checkpoint that carries the most attestations in its tree. A checkpoint is a justified or finalized block on top of which new blocks accumulate in each epoch. The `get_head` function first finds the hash of the most recent justfied block then finds the tree of all blocks with that justified block as their root. The function then walks down the tree. Wherever there is a fork (i.e. a parent block splits into multiple children) the function selects the block with the most attestations to be the correct one. This continues until there are no more blocks, and the last one is the head of the chain. 

In order to execute `get_head` information about the current state of the chain is required. This information is contained inside a  `store` object that is updated every second, every block and every attestation. This ensures that when `get_head` is called, it can pull up to date information from `store` and confidently assert the correct head. These updates are executed by the `on_tick`, `on_block` and `on_attestation ` handlers.

```

class Store(object):
    time: uint64
    genesis_time: uint64
    justified_checkpoint: Checkpoint
    finalized_checkpoint: Checkpoint
    best_justified_checkpoint: Checkpoint
    blocks: Dict[Root, BeaconBlock] = field(default_factory=dict)
    block_states: Dict[Root, BeaconState] = field(default_factory=dict)
    checkpoint_states: Dict[Checkpoint, BeaconState] = field(default_factory=dict)
    latest_messages: Dict[ValidatorIndex, LatestMessage] = field(default_factory=dict)

```

`on_tick` executes every second, and its purpose is to keep track of the latest known checkpoint. On receiving a new block `on_block` is executed. This handler first executes a check that the new blocm has a valid parent, that it is destined for the current available slot, that the block is a descendant of the last finalized checkpoint and that the block's post-state root is correct (i.e. its state transition will pass). Assuming these actions pass successfully, the block is added to the block database and update the justified and finalized checkpoints with data from the block. The incoming block may know about a more recent checkpoint than our client - if so, we update our checkpoint knowledge according to that of the new block. When we receieve a new attestation `on_attestation` is called. It first checks that the attestation is timely, i.e. refers to blocks in the current or previous epoch, that the attestation refers to a block already known to the client, and that the finalization target is conistent with the attestation's vote for head of the chain.

This fork-choice algorithm is executed by any validator making attestations and is passed to the aggregator in the AttestationData object. The `epoch_processing` weighs the total ETH balance of all attestations for the proposed blocks since the last checkpoint and assuming > 2/3 of the total stake supports the proposed head of the chain, then the head becomes a new justified checkpoint. The previous justified checkpoint is then finalized, meaning it can no longer be modified - it is part of the immutable canonical chain.

Note that a `BeaconBlockHeader` is essentially identical to a `BeaconBlock` but with the full `BeaconBlockBody` replaced by only the body root hash.


### Execution Payload

Pre-merge (i.e. phase0 specs) the beacon chain isn't actually coming to consensus on anything - it is just processing blocks from the existing PoW chain and finalizing them periodically using LMD-GHOST. Clients are attesting to the blocks, but it is quite impotent - the blocks are already PoW validated. This all changes at the point of the merge, when the consensus clients will suddenly be expected to manage transaction block production and handling, gossiping the transaction blocks around the network, validating them and processing them for inclusion into Beacon blocks.

The way this will be achieved is to couple the consensus client with an execution client. A new field will be added to the `BeaconState`: `latest_execution_payload_header`. This field contains the header for a new container `ExecutionPayload`. This container includes the list of transactions to be executed along with all the information required to validate the block, such as the total gas used, gas limit, coinbase address (address of validator receiving block rewards). The `ExecutionPayloadHeader` is identical, but instead of including the full list of transactions it only includes the the merkle root of the eth1 chain state after the transactions have executed. The `ExecutionPayload` object is part of the new `BeaconBody`.

The `state_transition_function` is also updated to include a new sub-function, `execute_payload`. The precise details of how this function will be implemented is not yet included in the specs, but is certain to require the list of transactions to be passed to the execution client. There, the transactions will be executed and the `BeaconState` object updated to reflect the altered state of the Ethereum blockchain.


## Altair

Altair is the first major upgrade to the Beacon chain. It will introduce new accounting mechanisms that penalise more harshly and sync committees that can support light clients. This adds some new duties to a validator on top of block proposal and attestations. Specifically, the consensus client will be required to occasionally participate int e sync committee. This is a subset of the committee that validates each block and their role is to expose information about the Beacon Chain to light clients, enabling them to function in a near-stateless mode.

To support light clients in Altair:

1) `sync_aggregate` is added to the `BeaconBlockBody`
    This sync_aggregate field simply shows which validators participated in the sync committee so that they can be authenticated and rewarded.
2) `next_sync_committee` and `current_sync_committee` are added to `Beaconstate`
   This provides details of the current and previous sync committees so that light clients can process against a reduced validator set
3) `process_sync_committee` added to `process_block`
    This validates the sync_aggregate payload each time a block arrives
4) `process_sync_committee_updates` added to `process_epoch`
   The sync committee changes every 256 epochs. This is handled inside `process_blocks`

Light clients will use this information to sync with the Beacon Chain. The task of a light client is to learn the next block header on the Beacon Chain with minimal CPU, memory and bandwidth requirements. Details on light client implementations to follow in a separate page.

Alex Stokes explanation of Altair/light clients: https://www.youtube.com/watch?v=KdhHJa2SEwY
https://docs.google.com/presentation/d/1MZ-E6TVwomt4rqz-P2Bd_X3DFUW9fWDQkxUP_QJhkyw/edit#slide=id.g621e1673fb_6_58

## Sharding

 With 64 blockchain shards, there are 64 spaces in a Beacon block to contain each shard's "crosslink". The crosslink is a reference to the head of each shard chain - this is how the Beacon Chain keeps track of the head of each blockchain shard. [More info on crosslink structure](https://notes.ethereum.org/@vbuterin/HkiULaluS).

## Rewards and penalties

## RANDAO

## Clients
Prysm, Lodestar, Teku, Lighthouse, Nimbus...



<b> is eth1data just for the deposit contract or does it contain transaction data? </b>
<b> what was the rationale behind deprecating explicit storage of crosslinks in favour of blobs? (sharding specs) </b>
<b> should a light client be built in anticipation of sharding or according to merge specs? </b>
<b> does a consensus light client need to expose a json-rpc? </b>
<b> how will the Altair sync committees change to accommodate sharding? Any info on blob state exposed? </b>
<b> any preference as to where comittee signatures for light client - connection to full node, directly from p2p network? Portal network? </b>
<b> where are merkle branches coming from? roots are in the block headers, but specific branches? </br>

most recent consensus specs: https://github.com/ethereum/consensus-specs

scratch notes

where does header come from? either from p2p layer, its own p2p layer or directly from full nodes.
  - not specd yet
  - separate stream is exposing consensus data to portal network, main stream would be subnet of main p2p
  - beacon chain light clients download latest info and broadcast to peers
  - les doenst work well bc people dont run the les servers bc theres no incentive to take on the extra load
  - light client initial build - assume data s available - this comes from les style queries to nodes
  - I'M GOING FOR THE CLIENT PROTOTYPE NOT THE NETWORKING PROTOTYPE