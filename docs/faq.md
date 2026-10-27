# FAQ

> Draft. Pre-launch, spec stage. Answers describe the current design and may change.

## In short

Short answers to common questions. Each links to the document that explains it fully.

## Questions

**Is Hash Chain live?**
No. It is pre-launch and at the spec stage. Nothing is deployed or audited. See [Roadmap](13-roadmap.md).

**What does it do in one sentence?**
It is a privacy-first Layer 0 network: accounts, payments, swaps, assets and apps are private by default, settle in moments, and connect to Ethereum, Base, Robinhood Chain and Monero. See [Overview](00-overview.md).

**What does Layer 0 mean here?**
Hash Chain is a base network of its own, and other networks plug into it. It does not sit on top of another chain. See [Architecture](01-architecture.md).

**Is Hash Chain a bridge?**
No. It is its own network with its own validators. Its connections to other networks are connection contracts and swaps that the network verifies, but the network itself is not a bridge. See [Connections](10-connections.md).

**Who secures it?**
Its own validator set, which stakes ETH. You rely on a supermajority of validators being honest. See [Validators](02-validators-and-staking.md) and [Security and trust](09-security-and-trust.md).

**Is it anonymous?**
It is designed to be private, not magically anonymous. Privacy depends on how many people use it and on how you behave. See [Security and trust](09-security-and-trust.md).

**What do I pay fees in?**
ETH, for everything. See [Fees](12-fees.md).

**Is there a token?**
No token is required to use the network. Fees are paid in ETH.

**Which assets are supported?**
ETH first. On Robinhood Chain, ETH first and tokenized assets later. Other assets are to be specified. See [Connections](10-connections.md).

**How fast are transfers?**
The design target is fast deterministic finality, so private payments settle in moments. No figures are promised. Moving in and out depends on the connected network, and the liquidity network speeds up exits. See [01](01-architecture.md) and [10](10-connections.md).

**Can a liquidity provider steal my funds?**
By design, no. A provider can refuse or be slow, but the recipient and amount are bound to your proof. This is a design claim, not an audited fact. See [10](10-connections.md).

**Do I trust a custodian for Monero swaps?**
No. The swap uses adaptor signatures and timeouts. You do need to stay reachable within the deadlines. See [11](11-connection-monero.md).

**Does a Monero swap ever pass through another network?**
Not in the proposed design. A swap is a direct corridor on one connected network, in the proposed order Robinhood Chain, Base, Ethereum. There is no bridging step inside a swap, and its deadlines are never added up over two networks. This is a proposal, not a decision, and no corridor has a date. See [11](11-connection-monero.md) and [HIP-5](https://github.com/HashChainL0/hips/blob/main/hips/hip-0005.md).

**What happens if I go offline during a swap?**
Depending on when, you may need to come back to refund or sweep. Funds are not meant to be stolen, but they can be frozen for a time. The failure matrix lists each case. See [11](11-connection-monero.md).

**Can I prove my history to an accountant?**
Yes, with a view key. You choose the scope and can revoke it. Revocation stops future visibility, and data already viewed cannot be unseen. You can also prove single facts with credentials. See [08](08-proofs-and-view-keys.md).

**How are changes to the design decided?**
Changes are proposed as HIPs, short documents in a public repository that describe one change and record the objections. A Draft HIP is a proposal and not a decision. See the [hips repository](https://github.com/HashChainL0/hips).

**Is this for hiding illegal activity?**
No. The goal is ordinary financial privacy with the option of scoped disclosure. Private, not shady.

**Can I build on it?**
Private apps and an SDK are planned. The SDK shown in the spec is a preview and may change. See [07](07-private-apps.md).

**What are the main risks?**
Unaudited software, a small early anonymity set, validator collusion or downtime, proof system bugs, liquidity provider and swap liveness, and metadata leaks from careless use. See [09](09-security-and-trust.md).

**How can I help?**
Read the spec and open an issue. See the repository [CONTRIBUTING](../CONTRIBUTING.md). Report vulnerabilities privately through [SECURITY](../SECURITY.md).

## Technical details

For definitions of terms used here, see the [glossary](glossary.md).

## Open questions

- Which questions users ask most once the testnet exists, so this page can answer them.
- Whether a plain-language threat model page is needed in addition to [09](09-security-and-trust.md).
