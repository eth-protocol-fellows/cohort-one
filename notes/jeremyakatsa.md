# Ethereum Capacity Improvents:

## Project Focus: Altair -- Minimal Light Client

### Phase 0 (current):
- Altair: https://github.com/ethereum/consensus-specs/blob/dev/specs/altair/sync-protocol.md
#### Basics:
- Beacon Chain: A registry of validator addresses, the state of each validator, attestations, and links to shards. Validators are activated by the Beacon Chain and can transition to states.
- Beacon Chain Checkpoints (similar structure as a Linked-List): A checkpoint is a block in the first slot of an epoch.  If there is no such block, then the checkpoint is the preceding most recent block.  There is always one checkpoint block per epoch. A block can be the checkpoint for multiple epochs.
- Epoch boundary blocks (EBB): can be considered synonymous with checkpoints.
- Sharding: A multi-phase upgrade to improve Ethereum’s scalability and capacity. Shard chains spread the network's load across 64 new chains. They make it easier to run a node by keeping hardware requirements low. This upgrade is planned to follow the merge of Mainnet with the Beacon Chain.
- Attestation: a validator’s vote, weighted by the validator’s balance.  Attestations are broadcasted by validators in addition to blocks. Validators also police each other and are rewarded for reporting other validators that make conflicting votes, or propose multiple blocks.
### Phase 1:
- Crosslinks: how the Beacon Chain follows the head of a shard chain. As there are 64 shards, each beacon block can contain up to 64 crosslinks.  A beacon block might only have one crosslink, if at that slot, there were no proposed blocks for 63 of the shards.

#### Other Info:
- Eth2 Specs: https://github.com/ethereum/eth2.0-specs
- Github: https://github.com/ethereum/eth2.0-specs/blob/dev/specs/merge/beacon-chain.md
- Transition (Eth1 - Eth2): https://ethresear.ch/t/the-eth1-eth2-transition/6265
- Eth2 Notes: https://notes.ethereum.org/m9IX3OkkTveXCCOSzGkUiw
- Sharding Proposal: https://hackmd.io/@vbuterin/sharding_proposal

_____________________________________________________________________________________________________________________________________________________________________________________________________________________________________________________________________________

#### Sharding:
- Base: https://ethereum.org/en/eth2/shard-chains/
- Sharding Consensus: https://blog.ethereum.org/2020/03/27/sharding-consensus/
- Beacon Chain: https://ethos.dev/beacon-chain/

#### Interesting Findings, Observations and Discoveries:
- NFT marketplaces are essentially like "Digital-Museums" (OpenSea, Foundation, etc) and the wallets to store NFTs (Rainbow, etc) are like "Digital-Houses".

#### NFT Building Blocks:
- https://ethereum.org/en/developers/docs/standards/tokens/erc-721/
- https://eips.ethereum.org/EIPS/eip-2309

#### Misc:
- NFT Basics: https://ethereum.org/en/nft/
- Block Chain Scalability (Future of Ethereum): https://vitalik.ca/general/2021/05/23/scaling.html
- Why Sharding? (Vitalik Buterin): https://vitalik.ca/general/2021/04/07/sharding.html
- Contributing to Ethereum Ecosystem (EIP-1 (i.e. how Ethereum Improvement Proposals Work)): https://eips.ethereum.org/EIPS/eip-1
