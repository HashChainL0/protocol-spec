# 06 Connection verification

> Draft research note. Pre-launch, spec stage. The recommendation is proposed, not final. Addresses [issue 6](https://github.com/HashChainL0/protocol-spec/issues/6).

## In short

Connections to Ethereum, Base and Robinhood Chain move funds through contracts that the Hash Chain validator set controls. Validators must be sure a deposit really happened before crediting it, and the contract must be sure a release was really approved. This note compares ways to prove events, states what each one trusts, analyses the main failures, and proposes safety limits.

## Context

[10](../docs/10-connections.md) lists the mechanism as "to be specified per network". The security of a connection is the weakest of three parts: how Hash Chain learns about the other network, how the other network learns about Hash Chain, and what happens when either side stops working. The history of cross-network bridges is mostly a history of failures in exactly these parts. Ronin lost funds in 2022 when attackers obtained a majority of its validator keys. Nomad lost funds the same year after an initialization mistake made unproven messages look valid. Neither was a cryptography break, so limits matter as much as proofs.

## Direction 1: connected network to Hash Chain (deposits)

### Option A. Validators run full nodes and sign what they see

Each validator runs its own node for each connected network and signs a deposit attestation once the event is deep enough. Simple and cheap. Trust: a threshold of validators is honest and its nodes are not fed false data. It is only as strong as the stake behind the signatures ([01](01-stake-and-slashing.md)).

### Option B. Light client verification

Validators verify a cryptographic proof of the event against the connected network's consensus, rather than trusting their own node.

- **Ethereum.** The Altair light client protocol lets a client follow a sync committee of 512 validators, rotated about every day, whose aggregate signature attests to block headers. A client then proves the deposit log with a Merkle proof against the receipts root in the header. Open-source light clients such as Helios implement this. The trust assumption is that the sync committee is mostly honest. Its economic security is lower than that of full finality, because it is a small sample, so waiting for a finalized header is safer than using the latest signed header.
- **Base.** Base is an OP Stack chain. Its sequencer produces blocks quickly and the data is later posted to Ethereum, after which it is derivable from Ethereum alone. Withdrawals to Ethereum rely on output roots posted to Ethereum and challengeable by fault proofs, with a challenge window of about a week by default on OP chains (approximate, from OP Stack documentation). Deposits into Hash Chain do not need that window: once a block's data is posted to Ethereum and Ethereum finalizes it, a validator that re-executes the chain agrees with any honest observer. Using the sequencer's unconfirmed blocks is faster but can be reversed.
- **Robinhood Chain.** It is publicly described as built on Arbitrum technology, where assertions about chain state are posted to Ethereum and confirmed after a challenge period of about a week by default on Arbitrum One (approximate, from Arbitrum documentation). Its settlement parameters may differ and must be checked against the final network. The same logic holds: derive the deposit from data on Ethereum, do not depend on the final challenge window.

### Option C. Zero-knowledge light client

A proof of the source consensus is verified on Hash Chain, so no validator is trusted for that step. Early products exist for Ethereum consensus proofs. Strongest assumptions, highest engineering and proving cost. An upgrade, not a launch dependency.

## Direction 2: Hash Chain to the connected network (releases)

The connection contract checks a validator-set threshold signature over a release message that binds the recipient, amount, asset and a unique nonce. BLS12-381 aggregate signatures keep the check to one pairing-based verification, and Ethereum added precompiles for BLS12-381 operations in Pectra (EIP-2537), which lowers the gas cost. The contract tracks the validator set through signed epoch handoffs ([note 02](02-consensus-and-finality.md)). Replay protection through the nonce is the exact place Nomad-style mistakes happen, so the contract should be minimal, with no upgrade path that skips the delay below.

## Failure analysis

| Failure | What happens | Mitigation |
| --- | --- | --- |
| Dishonest validator majority | Signs a false release | Stake larger than contract holdings, outflow caps, delays, monitoring |
| Reorg on the connected network | Deposit credited then reversed | Credit at finality or safe head only; fast path covered by liquidity providers |
| Hash Chain halts | Funds stuck in contracts | Escape path, below |
| Contract bug | Funds drained | Audits, staged rollout, hard caps, no instant upgrades |
| Validator set rotation bug | Wrong set accepted | Handoffs signed by the outgoing set, delay on set changes |

## Safety limits (proposed)

- A per-period outflow cap per asset on each contract, enforced in the contract, so a compromise loses at most one period's cap.
- A delay on large releases during which a guardian set can pause. The guardian can only pause, never release, and its power expires.
- Caps that start low and rise only as the validator stake and audit history support it.

## Halt and escape

If no valid validator-signed update reaches a contract for a long time, the contract should let users exit without the validators. Rollup designs with forced exits, and validium designs with a freeze plus escape hatch, show the pattern. Here the validators would checkpoint the shielded state root to the contract, and a user could prove ownership of a note against the last checkpoint and burn its nullifier on the contract. This depends on users being able to rebuild their Merkle path, which is a data availability requirement, tied to [note 02](02-consensus-and-finality.md). The timeout length is to be specified.

## Liquidity network rules

The provider fronts funds on the destination, bound to the user's proof so the destination and amount cannot change. Relayer and intent systems, and the cross-chain intents standard ERC-7683, show the pattern: fill first, repay after the verified message. Open design points are provider collateral against a fill that is never repaid, whether the fee is set by auction or by schedule, and a cap on how much unrepaid exposure one provider can hold.

## Recommendation (proposed, not final)

Launch with Option A on Ethereum and Base, using finalized or safe data only, with all limits above. Use the Ethereum light client for Ethereum as soon as it is ready, since it removes node trust at modest cost. Treat Robinhood Chain as a later door: ETH first with the same release path, tokenized assets only after their issuer controls (freezes, allow lists) are mapped, since those controls can stop a release the contract believes is valid. Treat zero-knowledge light clients as an upgrade path. First assets: ETH, then major stablecoins, whose issuers can freeze balances, which is a risk to document.

## Open questions

- Safe head versus finalized head per network, and the delay this adds for users without the liquidity network.
- Guardian composition and expiry.
- Checkpoint frequency for the escape path and its data availability cost.
- Provider collateral sizing.
- How a reorg deeper than the confirmation rule is handled after funds were credited.

## References

Ethereum consensus specifications (Altair light client, sync committee). Helios documentation. OP Stack documentation (output roots, fault proofs). Arbitrum documentation (assertions and challenge period). EIP-2537, ERC-7683. Public post-mortems of the 2022 Ronin and Nomad incidents.
