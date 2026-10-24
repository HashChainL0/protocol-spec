# 02 Consensus and finality

> Draft research note. Pre-launch, spec stage. The recommendation is proposed, not final. Addresses [issue 2](https://github.com/HashChainL0/protocol-spec/issues/2).

## In short

Hash Chain wants fast, deterministic finality and parallel settlement for unrelated accounts. This note compares three families of Byzantine fault tolerant protocols: leader-based with instant finality, pipelined leader-based, and DAG-based with separate data dissemination. It also points out that the shielded state is shared, which limits how much parallelism account chains can really deliver.

## Context

[01](../docs/01-architecture.md) describes account chains: each account has its own hash-linked sequence of updates, validators verify proofs and finalize with a supermajority of stake. The consensus protocol is not chosen. Four requirements follow from the spec.

1. Deterministic finality. A finalized update is final, not probabilistic.
2. Payment-scale throughput without promising figures.
3. Light clients and connection contracts must be able to verify finality cheaply ([06](06-connection-verification.md)).
4. Ordering of shared-state operations such as swaps must not leak order flow.

## A hidden constraint: shared shielded state

Account chains order one account's own updates. They do not remove shared state. In a note-based design every transfer does two things to global structures.

- It inserts nullifiers into one spent set. Two transfers spending different notes touch different nullifiers, so they do not conflict, but both must be checked against the same set.
- It appends commitments to one Merkle tree. Appends are order dependent: the position of a commitment and the resulting root depend on the order of all appends before it.

So proof verification can run fully in parallel, but state application needs one agreed order per round. Splitting the tree per account would remove the conflict and also shrink the anonymity set to the account, which defeats the purpose. The practical split is: verify in parallel, then apply a block's updates in a deterministic order. Consensus must agree on the set and order of updates, not on every update separately.

## Options

### A. Leader-based, instant finality (Tendermint / CometBFT style)

A rotating proposer, two voting rounds, finality when more than two thirds of voting power precommits. Deployed widely in Cosmos chains. Finality is immediate and simple to reason about, and light client verification of a commit is well understood. Costs: all-to-all voting messages grow quadratically with validator count, a faulty leader costs a timeout, and one proposer carries the block data, which limits throughput.

### B. Pipelined leader-based (HotStuff family)

HotStuff made leader replacement linear in communication and pipelined the phases so each block's vote doubles as the next stage of the previous one. Variants with two-chain commit, such as Jolteon, reduce latency. Used in several production chains in different forms (the Diem lineage and its successors). Better scaling in validator count than A and still leader-bottlenecked for data. Finality is deterministic.

### C. DAG-based with separate dissemination (Narwhal with Tusk or Bullshark, Mysticeti)

Narwhal separates data availability from ordering. Validators continuously broadcast batches and certify that they hold them, producing a DAG of certified batches. Ordering protocols such as Tusk and Bullshark then order the DAG with little extra communication. The Narwhal and Bullshark papers reported large throughput gains over a leader-bottlenecked design (approximate, from the authors' evaluations). Sui first used Narwhal and Bullshark, then moved to Mysticeti, which drops certification of every block to cut latency. Aptos adopted a Narwhal-derived dissemination layer for its mempool. Costs: more complex, higher implementation risk, and commit takes a few rounds.

A related idea fits account chains directly. Sui and the FastPay line of work use consistent broadcast for objects owned by a single party: a quorum of validators signs the owner's update and no total ordering is needed. That works when updates touch only data the owner controls. Shielded transfers append to the shared tree, so the fast path does not apply to them as it stands. It could apply to account-local actions that do not touch shared state, which is to be specified.

## Tradeoffs

| Property | A. Tendermint style | B. HotStuff family | C. DAG with dissemination |
| --- | --- | --- | --- |
| Finality | Deterministic, one commit | Deterministic | Deterministic after DAG commit |
| Scaling with validator count | Quadratic messages | Linear leader communication | Good, work spread over all validators |
| Throughput limit | Proposer bandwidth | Proposer bandwidth | Aggregate validator bandwidth |
| Complexity and maturity | Lowest, most deployed | Medium | Highest |
| Fit for proofs verified in parallel | Fine | Fine | Natural, batches carry proofs |
| Data availability | Separate question | Separate question | Built in by certified batches |

## Shared-state ordering without leaking order flow

Private swaps ([05](../docs/05-private-swaps.md)) need an order that validators cannot exploit. Two proven patterns.

- Encrypted submission with threshold decryption after ordering is committed, so ordering cannot depend on content.
- Batch execution. Penumbra, which runs on CometBFT, aggregates swap flow per block under encryption and decrypts only the batch total, so there is no per-trade ordering to exploit. This is the closest existing match to Hash Chain's goals.

Both need a validator-set key for threshold decryption, so that key's distributed generation and rotation become part of the consensus design.

## Light clients and validator set changes

Finality should produce a certificate: an aggregate signature from a threshold of stake over a block header. A light client or a connection contract checks one aggregate signature plus the current validator set. Validator set changes happen at epoch boundaries, and the outgoing set signs the handoff to the incoming set, as in Tendermint light client verification. For comparison, Ethereum's own finality takes two epochs, on the order of minutes (approximate, from the Ethereum specifications), and light clients follow a rotating sync committee. Hash Chain should aim to verify faster, but the certificate shape can be the same.

## Recommendation (proposed, not final)

Specify the protocol in two layers. A dissemination layer in the Narwhal style, where validators certify batches of updates with their proofs, and an ordering layer that is deterministic and produces a finality certificate. For the devnet, start with the simplest option that satisfies the interface, a Tendermint-style protocol, and keep the layer boundary so the ordering can be swapped for a DAG protocol before the public testnet. Apply each block's updates in a canonical order derived from the consensus order. Use batch execution and threshold decryption for swaps.

## Open questions

- Validator set size and the voting power threshold, tied to [note 01](01-stake-and-slashing.md).
- Which account-local actions can use a no-ordering fast path.
- Who stores encrypted note payloads long term: validators or storage nodes ([02](../docs/02-validators-and-staking.md)).
- Threshold decryption key setup and its failure mode when validators are offline.
- Behavior under validator set change while a swap batch is in flight.
- Maximum acceptable commit latency for the first release, to be set by measurement, not by this note.

## References

Tendermint and CometBFT documentation. Yin et al., HotStuff. Danezis et al., Narwhal and Tusk. Spiegelman et al., Bullshark. Babel et al., Mysticeti. Baudet et al., FastPay. Penumbra protocol documentation (flow encryption and batch swaps).
