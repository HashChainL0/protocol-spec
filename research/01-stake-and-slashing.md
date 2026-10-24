# 01 Stake and slashing

> Draft research note. Pre-launch, spec stage. The recommendation is proposed, not final. Addresses [issue 1](https://github.com/HashChainL0/protocol-spec/issues/1).

## In short

Hash Chain validators put ETH at risk so that cheating costs money. The open question is where that ETH sits and what can take it. This note compares three designs: ETH bonded in a dedicated contract, ETH staked on the Ethereum beacon chain with withdrawal credentials pointed at Hash Chain, and restaked ETH in a general restaking system. It proposes the first, with an interface that can accept the others later under caps.

## Context

[02](../docs/02-validators-and-staking.md) says validators stake ETH and lists four slashing conditions: double signing, invalid verification of connected networks, censorship proofs and downtime. It leaves the form of the stake open. Two facts shape the choice.

- ETH lives on Ethereum. Hash Chain has its own consensus, so any slash must be enforced by a contract on Ethereum that can check evidence of a fault on Hash Chain.
- Hash Chain validators also sign messages that release funds from connection contracts ([10](../docs/10-connections.md)). The stake is the main deterrent against a validator majority signing false messages, so the stake must be larger than what a dishonest majority could steal. This is a sizing rule, not a number.

## Options

### A. Bonded ETH in a dedicated contract

A staking contract on Ethereum accepts ETH deposits from validators. Hash Chain reads the contract to set the validator set and voting weight. Slashing evidence is submitted to the same contract.

- Closest in spirit to proof-of-stake chains that bond their own asset, such as Cosmos SDK chains, where double signing and downtime are the two classic slashable faults. On the Cosmos Hub the double-sign penalty is a few percent of stake and the downtime penalty is tiny, and both come with jailing (approximate, from the Hub's published parameters).
- The bond earns no Ethereum consensus rewards. The validator's cost of capital is the full opportunity cost of the ETH, so it is the most expensive option for validators.
- Simple and auditable. One contract, one set of rules, no dependency on another protocol's operators or slashing policy.

### B. Beacon-chain stake with Hash Chain withdrawal credentials

A validator runs an Ethereum validator and points its withdrawal credentials at a contract that Hash Chain can slash. This is how native restaking pods work in EigenLayer: a pod contract is the withdrawal address, and the restaking system gains a claim on the ETH when the validator exits.

- The same 32 ETH earns Ethereum rewards and secures Hash Chain, so capital cost is lower.
- It stacks risks. A Hash Chain slash and an Ethereum slash can hit the same ETH. Ethereum's own slashing includes a correlation penalty that grows with the share of validators slashed in a window, so a correlated incident costs more than a single one.
- Exits go through the Ethereum withdrawal queue, so unbonding is set by Ethereum, not by Hash Chain. After the Pectra upgrade the maximum effective balance per validator is higher than 32 ETH (EIP-7251), which changes pod sizing.

### C. Restaked ETH through a general restaking protocol

Validators opt in to Hash Chain as one service among many in a restaking system such as EigenLayer, where operators allocate stake to services and services define slashing. Symbiotic and Karak are similar in kind.

- Fastest bootstrap, because stake already exists. Liquid staking tokens can often be restaked, which widens participation.
- Hash Chain inherits the restaking system's contract risk, its operator set and its slashing governance. A bug or policy change there is a bug or policy change here.
- The security budget is shared. The same ETH can back many services at once, so a fault in one can reduce the stake available for another. Public EigenLayer material introduced allocations and operator sets so stake can be assigned to specific services and slashed only for them. That reduces but does not remove the overlap.

## Tradeoffs

| Property | A. Bonded contract | B. Beacon with credentials | C. Restaking protocol |
| --- | --- | --- | --- |
| Capital cost to validator | Highest | Lower | Lowest |
| Dependencies | Hash Chain contract only | Ethereum plus pod contract | Restaking protocol and its operators |
| Unbonding control | Hash Chain | Ethereum withdrawal queue | Restaking protocol |
| Correlated slashing | Only Hash Chain faults | Hash Chain plus Ethereum faults | Many services at once |
| Complexity | Low | Medium | Medium, but outsourced |
| Speed to a large stake | Slow | Medium | Fast |

## Slashing conditions: proposed split

Only faults that anyone can verify from signed data should cost stake.

- **Double signing.** Two conflicting finality signatures from one validator in the same round. Objective, evidence is two signatures. Slash heavily.
- **Invalid connection attestation.** Signing a message that contradicts the source network, for example a deposit that did not happen. Objective if the contract can check the claim against a source network proof. See [06](06-connection-verification.md). Slash heavily.
- **Downtime.** Not objective in the same way, and a halted network could slash everyone at once. Proposed: lose rewards and be jailed, no stake loss.
- **Censorship.** No known way to prove deliberate exclusion from a private mempool. Proposed: not slashable. Mitigate with rotation and inclusion rules in [note 02](02-consensus-and-finality.md).

Slashing evidence should be submittable by anyone, with a bounty from the slashed amount. Penalties should scale with the fraction of stake slashed in a window, as Ethereum's correlation penalty does, so an isolated fault is cheap and a coordinated attack is ruinous.

## Recommendation (proposed, not final)

Launch with option A and design the staking contract so that a stake source is an interface. Option B and C sources can be added later as capped tranches once their failure modes are understood and the contract has been audited. The reason is control: the main purpose of the stake is to deter false connection messages, and that deterrent must not depend on another system's governance. The unbonding period should be longer than the longest window in which an invalid connection message could be challenged.

## Open questions

- Minimum stake and the cap on any one operator, and how stake size is tied to what connection contracts hold.
- Whether a slash can be disputed, by whom, and how long the dispute window is. A dispute process creates a trusted party, so its shape matters.
- Whether the Hash Chain contract can verify finality signatures on Ethereum cheaply. The BLS12-381 precompiles added in Pectra (EIP-2537) make this more plausible, to be measured.
- How a validator exits when Hash Chain is halted.
- What share of slashed stake is burned, paid to the reporter, or returned to affected users. Burning is an ETH burn, not a token.

## References

EigenLayer documentation (restaking, native restaking pods, operator sets and slashing). Ethereum consensus specifications (slashing, correlation penalty, withdrawal queue). EIP-7251 and EIP-2537. Cosmos SDK slashing module documentation.
