# 14 Threat model

> Draft. Pre-launch, spec stage. This describes who the design defends against and who it does not. Nothing here has been audited, and the list will grow as the design firms up.

## In short

This document names the parties who might attack or spy on Hash Chain, and for each one says what they can learn, what they can do, and what the design does about it. Where the honest answer is "this breaks the guarantee", it says so. It complements the trust table in [09](09-security-and-trust.md).

## How to read this

Each adversary is described by capability, not by identity. "Learns" means information. "Can do" means actions that change outcomes. "Mitigations" are design responses and may be unbuilt. Terms are defined in the [glossary](glossary.md).

## Summary

| Adversary | Learns | Can do | Main mitigation |
| --- | --- | --- | --- |
| Network observer | Timing, sizes, network addresses at the edges | Correlate entry and exit by patterns | Fixed-shape updates, wallet guidance, larger anonymity set |
| Malicious validator minority | When updates arrive, which account chains are active | Delay or censor a few updates, try to equivocate | Stake, slashing, finality threshold, rotating inclusion |
| Dishonest validator majority | Still not contents, unless proofs break | Finalize invalid history, censor, sign false connection messages | Limits and delays on connections, monitoring, exit paths |
| Connection contract bug | Nothing about private state | Drain or lock funds held on a connected network | Audits, per-period limits, staged rollout |
| Swap counterparty | Amounts, timing and Monero-side addresses of that swap | Freeze funds for a time, exploit a careless wallet | Verified locks, bonds, bounded deadlines |
| Device compromise | Everything on that device, up to its keys | Spend, or read history | Key separation, view keys that cannot spend, recovery guidance |

## Network observer

A party watching traffic on the internet, on a connected network, or on Hash Chain as a public participant.

- **Can learn.** Which addresses deposit into and withdraw from connected networks, with amounts and times. That an IP address talked to a node and when. Sizes of messages if they vary.
- **Cannot learn.** Balances, amounts or parties of transfers inside Hash Chain. Which note was spent. Whether two payments went to the same person (stealth addresses).
- **Can do.** Link an entry to an exit when the amount is unusual or the gap in time is short. Link a wallet to its IP if it connects directly.
- **Mitigations.** Wallet defaults that avoid unique amounts and add delay, uniform-size updates, relays that hide IPs (to be specified), growth of the anonymity set. User advice is in [09](09-security-and-trust.md).
- **Not solved by the protocol.** IP-level privacy, and any correlation caused by user behavior.

## Malicious validator minority

A coalition below the threshold that finality requires.

- **Can learn.** Arrival time of updates and which account chain they belong to. Not contents.
- **Cannot do.** Finalize an invalid update, forge a spend, change history, or sign connection messages alone.
- **Can do.** Refuse to include or relay particular updates, delay their own votes, try to sign conflicting messages.
- **Mitigations.** Other validators include the update. Equivocation is a provable fault and is slashed ([02](02-validators-and-staking.md)). Users can submit through several validators.
- **Residual.** Timing information leaks to anyone who runs a validator.

## Dishonest majority

A coalition at or above the finality threshold. This is the point where the safety guarantee of the network ends, so it is stated plainly.

- **Can learn.** Still not amounts or parties, as long as the proof system holds, because privacy does not depend on validator honesty. Colluding validators can pool timing data.
- **Can do.** Finalize invalid history if proofs are not checked by anyone else, censor users, halt the network, sign false messages that release funds from connection contracts, and release data they should keep.
- **Mitigations.** Large stake at risk, slashing for provable faults, validator diversity. On connections: per-period limits, delays for large releases, and independent monitoring with a public alert path ([10](10-connections.md)). Users and wallets can verify proofs themselves, so invalid history is detectable even when it cannot be prevented.
- **Residual.** A majority can still cause losses up to the connection limits. Slashing deters but does not undo. Recovery paths after a majority fault are to be specified.

## Connection contract bug

A flaw in the contracts that hold funds on Ethereum, Base, Robinhood Chain, or the swap contract on Hash Chain.

- **Can learn.** Nothing about private state. Public contract state is already public.
- **Can do.** Allow release without a valid validator message, lock funds, or mis-account balances. A swap contract flaw could allow an early reveal of `a` or `b`.
- **Mitigations.** Independent audits before mainnet, formal checks on core invariants, limits per period so one bug cannot drain everything, pausable release with a time limit, and a staged rollout starting with small caps.
- **Residual.** Limits reduce the loss, they do not remove it. An emergency pause has its own abuse risk, listed as open in [09](09-security-and-trust.md).

## Swap counterparty

The other side of a Monero swap ([11](11-connection-monero.md)).

- **Can learn.** The amounts and timing of the swap, and the Monero-side joint address. Not the other party's Hash Chain account if the lock and claim use private accounts as designed.
- **Cannot do.** Take both sides, as long as the contract and the equality proofs are correct and the other party follows the verified steps.
- **Can do.** Walk away after a lock, freezing funds for the length of the deadlines. In the worst case, an ETH holder who goes silent after both locks freezes the XMR holder without a guaranteed end. Exploit a wallet that signals ready without checking the lock.
- **Mitigations.** Wallets verify the Monero lock automatically before signalling ready. Bounded deadlines (see the parameter table in [11](11-connection-monero.md)). Candidate bond and forced refund reveal for the freeze.
- **Residual.** The freeze is not yet solved. Counterparty selection is open.

## Device compromise

Malware or physical access on the user's device, or theft of a backup.

- **Can learn.** Everything the device can decrypt: notes, history, view keys, swap secrets.
- **Can do.** If the master secret is taken: spend all funds, read all history. If only a view key is taken: read the scoped history and nothing more. If swap secrets are taken during a swap: potentially redirect that swap.
- **Mitigations.** Keys are derived so a view key never opens spending ([08](08-proofs-and-view-keys.md)). Hardware-backed key storage and optional spending confirmation on a second device are candidates, to be specified. Swap secrets are stored before any lock.
- **Residual.** A compromised device is outside what the protocol can fix. Lost keys cannot be recovered.

## Out of scope

- Attacks on the user's operating system, browser, or hardware not specific to the protocol.
- Social engineering, coercion, and legal compulsion of a user to hand over keys.
- Network-level anonymity such as hiding the IP address from every observer.
- Flaws in the security of Ethereum, Base, Robinhood Chain or Monero themselves, except where this design adds a dependency on them.
- Economic attacks on prices of connected assets.
- Mistakes by users, such as sharing a view key with the wrong person (the consequences are explained in [08](08-proofs-and-view-keys.md)).
- Quantum attacks. Post-quantum choices are to be specified.

## Open questions

- A formal statement of which properties hold under which adversary assumptions.
- Validator finality threshold and what it implies for the minority and majority cases above.
- Recovery process after a dishonest majority event.
- Relay design for hiding IP addresses of wallets.
- Review plan: which parts get independent review first.
