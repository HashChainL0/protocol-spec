# FAQ

> Draft. Pre-launch, spec stage. Answers describe the current design and may change.

## In short

Short answers to common questions. Each links to the document that explains it fully.

## Questions

**Is Hash Chain live?**
No. It is pre-launch and at the spec stage. Nothing is deployed or audited. See [Roadmap](10-roadmap.md).

**What does it do in one sentence?**
It lets you move between Ethereum, Base, Robinhood Chain and Monero through a private hub, so the entry and the exit cannot be linked by outsiders. See [Overview](00-overview.md).

**Is it anonymous?**
It is designed to be private, not magically anonymous. Privacy depends on how many people use it and on how you behave. See [Security and trust](09-security-and-trust.md).

**What do I pay fees in?**
ETH, for everything. See [Fees](08-fees.md).

**Is there a token?**
This spec describes none. Gas is ETH.

**Which assets are supported?**
ETH first. On Robinhood Chain, ETH only in v1. Other assets are to be specified. See [Routes](02-routes.md).

**How long do transfers take?**
Base deposits take minutes. Standard Base withdrawals wait the challenge window of about seven days, with fast exits available for a fee. Ethereum and Robinhood Chain use a liquidity network for fast moves. Monero swaps depend on confirmations on both sides. See [03](03-route-base.md), [04](04-route-ethereum-robinhood.md), [05](05-route-monero.md).

**Can a relayer steal my funds?**
By design, no. A relayer can refuse or be slow, but the recipient and amount are bound to your proof. This is a design claim, not an audited fact. See [04](04-route-ethereum-robinhood.md).

**Do I trust a custodian for Monero swaps?**
No. The swap uses adaptor signatures and timeouts. You do need to stay reachable within the deadlines. See [05](05-route-monero.md).

**What happens if I go offline during a swap?**
Depending on when, you may need to come back to refund or sweep. Funds are not meant to be stolen, but they can be frozen for a time. The failure matrix lists each case. See [05](05-route-monero.md).

**Can I prove my history to an accountant?**
Yes, with a view key. You choose the scope and can revoke it. Revocation stops future visibility, and data already viewed cannot be unseen. See [07](07-view-keys.md).

**Is this for hiding illegal activity?**
No. The goal is ordinary financial privacy with the option of scoped disclosure. Private, not shady.

**What are the main risks?**
Unaudited contracts, a small early anonymity set, sequencer censorship or downtime, prover bugs, relayer and swap liveness, and metadata leaks from careless use. See [09](09-security-and-trust.md).

**How can I help?**
Read the spec and open an issue. See the repository [CONTRIBUTING](../CONTRIBUTING.md). Report vulnerabilities privately through [SECURITY](../SECURITY.md).

## Technical details

For definitions of terms used here, see the [glossary](glossary.md).

## Open questions

- Which questions users ask most once the testnet exists, so this page can answer them.
- Whether a plain-language threat model page is needed in addition to [09](09-security-and-trust.md).
