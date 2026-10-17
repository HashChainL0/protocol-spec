# Hash Chain Protocol Spec

Specification for Hash Chain, a privacy chain that works as a private hub between Ethereum, Base, Robinhood Chain and Monero.

> **STATUS: pre-launch, spec stage.** Every document here is a draft and may change without notice. Nothing described is deployed or audited. Parameters, mechanisms and names are proposals until marked otherwise. Where a point is undecided, the document says so under "Open questions".

![Hash Chain hub](diagrams/hub.svg)

## What this repository is

The written design of Hash Chain: what the system is for, how its parts fit, which assumptions it makes, and what is still undecided. It is meant to be read by users who want to know what they would be trusting, by integrators, and by reviewers who want to find mistakes early.

Each document starts with a plain "In short" block, then explains the topic, then gives "Technical details", then lists "Open questions".

## Table of contents

| Doc | Topic |
| --- | --- |
| [00 Overview](docs/00-overview.md) | The problem, the idea, and the whole system on one page |
| [01 Architecture](docs/01-architecture.md) | Layers, components and how a transaction moves |
| [02 Routes](docs/02-routes.md) | The route model shared by every path in and out |
| [03 Base route](docs/03-route-base.md) | Canonical bridge deposits, withdrawals and fast exits |
| [04 Ethereum and Robinhood Chain routes](docs/04-route-ethereum-robinhood.md) | Liquidity network and relayers |
| [05 Monero route](docs/05-route-monero.md) | Trustless XMR and ETH atomic swaps |
| [06 Shielded transfers](docs/06-shielded-transfers.md) | Notes, commitments, nullifiers, proofs, stealth addresses |
| [07 View keys](docs/07-view-keys.md) | Scoped, revocable disclosure for accountants and auditors |
| [08 Fees](docs/08-fees.md) | What you pay for, in ETH |
| [09 Security and trust](docs/09-security-and-trust.md) | Trust assumptions and risks, stated plainly |
| [10 Roadmap](docs/10-roadmap.md) | Phases, without dates |
| [Glossary](docs/glossary.md) | Terms used across the spec |
| [FAQ](docs/faq.md) | Common questions |

Suggested reading order: Overview, Routes, then the route you care about, then Security and trust.

## Diagrams

- [diagrams/hub.svg](diagrams/hub.svg): Hash Chain as a hub
- [diagrams/route-lifecycle.svg](diagrams/route-lifecycle.svg): the lifecycle shared by all routes
- [diagrams/xmr-swap-sequence.svg](diagrams/xmr-swap-sequence.svg): Monero swap, happy path

## Contributing

Issues and pull requests are welcome. See [CONTRIBUTING.md](CONTRIBUTING.md). Security reports go through [SECURITY.md](SECURITY.md). Participation is governed by the [Code of Conduct](CODE_OF_CONDUCT.md).

## Links

- X: https://x.com/hashchainL0
- GitHub: https://github.com/HashChainL0
- Brand kit: https://github.com/HashChainL0/brand-kit

## License

Documentation in this repository is licensed under [Creative Commons Attribution 4.0 International (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/). See [LICENSE](LICENSE). The Hash Chain name and logo are covered separately in the brand kit.
