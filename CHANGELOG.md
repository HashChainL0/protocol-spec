# Changelog

All notable changes to this specification are documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/). Versions describe the state of the draft, not a deployed protocol.

## [Unreleased]

### Added

- Research notes in [research/](research/README.md), one per open research issue. Each compares prior art, states tradeoffs and gives a proposed direction that is not final:
  - [01 Stake and slashing](research/01-stake-and-slashing.md): bonded, beacon-chain and restaked ETH, and which faults to slash. Addresses #1.
  - [02 Consensus and finality](research/02-consensus-and-finality.md): leader-based, pipelined and DAG-based BFT, and the shared shielded state. Addresses #2.
  - [03 Proof system](research/03-proof-system.md): Groth16, PLONK, Halo 2 and STARKs, setup model and a device measurement plan. Addresses #3.
  - [06 Connection verification](research/06-connection-verification.md): event proofs for Ethereum, Base and Robinhood Chain, safety limits and an escape path. Addresses #6.
  - [07 Fees and fee privacy](research/07-fees-and-fee-privacy.md): fee payment patterns and shielded fee payment. Addresses #7.
- Link to the research notes from the README.

## [0.2.0] - 2026-10-21

### Added

- Threat model ([14](docs/14-threat-model.md)): network observer, malicious validator minority, dishonest majority, connection contract bug, swap counterparty, device compromise, and what is out of scope.
- Key derivation tree, epoch and range scoping, disclosure levels, credential structure, presentation and credential revocation approach in [08](docs/08-proofs-and-view-keys.md). Closes #4.
- Timelock and refund parameter table for Monero swaps, online requirements, and griefing mitigations in [11](docs/11-connection-monero.md), with figures taken from the reference calculator at https://github.com/HashChainL0/xmr-swap-model. Closes #5.
- Glossary entries: connection contract, connection verifier, counterparty, epoch, ETH holder, XMR holder, griefing, issuer, nullifier key, receiving key, revocation, spending key, storage node, swap contract, validator set, verifier, watchtower.

### Changed

- Terminology aligned with the glossary: "bridge contract" is now "connection contract" throughout, and Hash Chain itself is still described as not a bridge. Closes #8.
- Swap deadlines `T0` and `T1` are measured from the finalized ETH lock, and claims at or after `T1` are rejected.
- Security and trust, README index and swap docs link to the threat model and the parameters.

### Fixed

- Duplicated title text in the hub diagram.
- Swap sequence diagram label now matches the contract rules: refund is possible at any time after the lock.

## [0.1.0] - 2026-10-18

### Added

- Initial draft specification:
  - Overview and architecture of the network
  - Validators and staking
  - Private accounts and transfers, payments, swaps, assets and apps
  - Proofs and view keys
  - Security and trust
  - Connections to Ethereum, Base and Robinhood Chain, and the Monero connection with trustless atomic swaps
  - Fees in ETH
  - Roadmap
  - FAQ and glossary

### Changed

- Repositioned Hash Chain as the privacy Layer 0: its own network with its own validators, with privacy built into the base layer.
