# Research notes

> Draft. Pre-launch, spec stage. These notes are working research, not decisions. Every recommendation is "proposed, not final".

## In short

The spec leaves several design questions open and tracks them as research issues. This directory holds one note per question. Each note states the context, compares real prior art, weighs the tradeoffs, and ends with a proposed direction and the questions that remain. The notes feed the spec documents and, where a change is concrete enough, a [HIP](https://github.com/HashChainL0/hips).

## Index

| Note | Question | Issue | Related spec |
| --- | --- | --- | --- |
| [01 Stake and slashing](01-stake-and-slashing.md) | Native or restaked ETH, and which faults are slashable | [#1](https://github.com/HashChainL0/protocol-spec/issues/1) | [02](../docs/02-validators-and-staking.md), [09](../docs/09-security-and-trust.md) |
| [02 Consensus and finality](02-consensus-and-finality.md) | Which BFT design fits account chains and shielded state | [#2](https://github.com/HashChainL0/protocol-spec/issues/2) | [01](../docs/01-architecture.md) |
| [03 Proof system](03-proof-system.md) | Proof system, setup model, prover cost on consumer devices | [#3](https://github.com/HashChainL0/protocol-spec/issues/3) | [03](../docs/03-accounts-and-private-transfers.md), [08](../docs/08-proofs-and-view-keys.md) |
| [06 Connection verification](06-connection-verification.md) | How validators verify events on Ethereum, Base and Robinhood Chain | [#6](https://github.com/HashChainL0/protocol-spec/issues/6) | [10](../docs/10-connections.md) |
| [07 Fees and fee privacy](07-fees-and-fee-privacy.md) | ETH fee model and paying fees without a public payer | [#7](https://github.com/HashChainL0/protocol-spec/issues/7) | [12](../docs/12-fees.md) |

Numbers 04 and 05 are unused. They were never assigned to a research note.

## How to read a note

Each note has the same shape:

1. **Context.** What the spec says today and why the question matters.
2. **Options.** Candidate designs, with the real systems they come from.
3. **Tradeoffs.** What each option costs and what it buys.
4. **Recommendation.** A proposed direction. It is a starting point for argument.
5. **Open questions.** What the note does not settle.

## Ground rules

- Figures quoted from other projects are approximate and attributed to the project they come from. They are not Hash Chain benchmarks. Hash Chain has none yet.
- Prior art is described from public documentation. If a description is wrong, open an issue.
- A note never changes the spec by itself. Spec changes go through a pull request that updates every affected document, see [CONTRIBUTING.md](../CONTRIBUTING.md).
