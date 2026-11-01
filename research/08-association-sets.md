# 08 Association sets

> Draft. Research note, not a decision. Proposed, not final.

## Context

The spec describes shielded accounts and private transfers ([03](../docs/03-accounts-and-private-transfers.md)) and view keys for scoped disclosure ([08](../docs/08-proofs-and-view-keys.md)). It does not yet say how a user can show that funds leaving the shielded pool did not come from a known bad deposit, without revealing which deposit they came from.

An association set answers that. It is a public list of deposits that someone vouches for. A withdrawal carries a proof that its deposit is a member of a chosen set, and nothing more. The pool does not learn which member.

The [shielded pool model](https://github.com/HashChainL0/shielded-pool-model) already implements an optional association set check, so the shape can be tested before the spec takes a position.

## Options

1. **No association sets.** Withdrawals prove only that a valid unspent note exists. Simplest, and the anonymity set is the whole pool.
2. **Optional sets, user chosen.** Anyone can publish a set root. A withdrawal may attach a membership proof for any accepted root. This is the design in the Privacy Pools paper (Buterin, Illum, Nadler, Schär and Soleimani, 2023).
3. **Mandatory set, one curator.** Every withdrawal must prove membership in a set run by one party. Easy to explain, but that party becomes a gatekeeper for every exit.
4. **Mandatory set, several curators.** Every withdrawal must prove membership in at least one of several accepted sets. Spreads the gatekeeping but still requires a set for every exit.

## Tradeoffs

| Option | Privacy | Exit guarantee | Disclosure story | Complexity |
| --- | --- | --- | --- | --- |
| 1 | Whole pool | Any valid note can exit | View keys only | Lowest |
| 2 | Whole pool for the transfer, the chosen set for the claim | Any valid note can exit | View keys plus set membership | Medium |
| 3 | The curator's set | Only notes the curator accepts | Strong, but one party decides | Medium |
| 4 | The union of accepted sets | Notes in at least one set | Strong, decision shared | Highest |

Two costs are common to options 2 to 4. A set root is one more input to the withdrawal proof, and the pool has to track which roots it accepts and for how long, the same way it tracks recent Merkle roots.

## Recommendation

Option 2. Sets are optional, any party can publish one, and the user chooses which to prove against. It keeps the exit guarantee of option 1 and adds a disclosure tool that does not reveal the deposit. It fits the existing rule that privacy is the default and disclosure is a choice.

The pool should accept a root only for a bounded window, so that a set that later removes a deposit stops being usable for new withdrawals.

## Open questions

1. Who can submit an accepted set root, and how is a root retired?
2. Should a change note carry the origin of the deposit it came from, as the model does today, or should membership be proven again on each split?
3. Does a set proof reveal anything through timing, for example a withdrawal that appears just after a new root is accepted?
4. Is a set a HIP of its own, or part of a wider HIP on shielded withdrawals?
