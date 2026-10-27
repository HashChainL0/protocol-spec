# Glossary

> Draft. Pre-launch, spec stage.

## In short

Plain definitions of terms used in the spec, in alphabetical order.

## Terms

**Account chain.** The ordered sequence of updates belonging to one account. Separate accounts have separate chains, so unrelated updates settle in parallel. See [01](01-architecture.md).

**Adaptor secret.** A hidden value that is revealed when one party completes a claim. In a Monero swap, revealing it on the ETH side lets the other party take the XMR.

**Anonymity set.** The group of users among whom you are hidden. Larger is better.

**Atomic swap.** An exchange of two assets where either both transfers complete or neither party can lose funds to the other, without a trusted third party.

**Base.** A network that connects to Hash Chain, alongside Ethereum, Robinhood Chain and Monero. See [10](10-connections.md).

**Commitment.** A public fingerprint of a note that hides its contents.

**Connection.** A door between Hash Chain and another network: Ethereum, Base, Robinhood Chain or Monero. See [10](10-connections.md).

**Connection contract.** A contract on Ethereum, Base or Robinhood Chain that holds funds for a connection and releases them when the Hash Chain validator set signs a matching message. See [10](10-connections.md).

**Connection verifier.** A validator role that checks events on connected networks and signs messages about them. See [02](02-validators-and-staking.md).

**Corridor.** In the proposal for ETH and Monero swaps, one connected network read as a direct swap route: Robinhood Chain, Base or Ethereum. A corridor holds the whole swap on its own network and never routes through another one. Proposed, not decided. See [11](11-connection-monero.md).

**Counterparty.** The other party in a swap.

**Credential.** A signed statement from an issuer about an account, used privately inside proofs to show one fact. See [08](08-proofs-and-view-keys.md).

**Data availability.** The guarantee that the data recipients need, such as encrypted note payloads, can be retrieved. To be specified.

**DLEQ proof.** A discrete-logarithm equality proof. Shows two public points on different curves come from the same secret scalar.

**Epoch.** A fixed span of Hash Chain time used to scope view keys. Length to be specified. See [08](08-proofs-and-view-keys.md).

**ETH holder.** In a Monero swap, the party who locks ETH on Hash Chain and wants XMR. See [11](11-connection-monero.md).

**Ethereum.** A network that connects to Hash Chain, alongside Base, Robinhood Chain and Monero. See [10](10-connections.md).

**Finality.** The point at which an update can no longer be reversed. Hash Chain targets fast, deterministic finality.

**Griefing.** Behavior that costs another party time or locked funds without the attacker gaining the funds.

**Hash Chain.** The privacy Layer 0 described in this spec: its own network with its own validators.

**HIP.** A Hash Chain Improvement Proposal, a short design document that proposes one change and records the objections. HIPs are proposals until they are Final. See the [hips repository](https://github.com/HashChainL0/hips).

**Issuer.** The party that signs a credential about an account. See [08](08-proofs-and-view-keys.md).

**Joint address.** The Monero address in a swap, controlled by a key split between two parties.

**Layer 0.** A base network that other networks plug into. Hash Chain is a Layer 0 with privacy as a base-layer property.

**Liquidity network.** A set of independent liquidity providers that front funds on destination networks for fast transfers.

**Liquidity provider.** An independent actor that fronts funds on a destination network and is repaid later.

**Merkle tree.** A tree of hashes that lets the network prove a commitment is included while storing only a short root.

**Monero.** A privacy-focused network with no smart contracts. It connects to Hash Chain through atomic swaps instead of a connection contract. See [11](11-connection-monero.md).

**Note.** A private unit of value held by an account.

**Nullifier.** A one-time value published when a note is spent. Prevents double spends without revealing which note was spent.

**Nullifier key.** The secret used to compute nullifiers for an account's notes. Not derivable from a view key.

**Private account.** An account whose balances and history are hidden by default.

**Prover.** The party that produces a zero-knowledge proof. Usually the user's wallet.

**Receiving key.** A long-lived public key a receiver publishes so senders can derive stealth addresses. See [03](03-accounts-and-private-transfers.md).

**Revocation.** Ending the usefulness of a view key or credential for future use. It cannot undo what was already seen.

**Robinhood Chain.** A network that connects to Hash Chain, alongside Ethereum, Base and Monero. ETH first, tokenized assets later. See [10](10-connections.md).

**Shape class.** A coarse category of update, by its size and structure. In the proposed direction for private fees, the fee depends only on the shape class and the epoch, so the fee field reveals the shape class and nothing else. Proposed, not decided. See [research note 07](../research/07-fees-and-fee-privacy.md).

**Shielded.** Held privately on Hash Chain. A shielded account and a private account are the same thing.

**Shielded state.** The set of commitments and nullifiers the validators maintain.

**Slashing.** Reducing a validator's stake after a provable fault.

**Spending key.** The secret that authorizes spending notes. Never shared.

**Stealth address.** A one-time address derived for each payment so that payments to the same person are unlinkable.

**Storage node.** A role that keeps data available so wallets can scan for and recover notes. See [02](02-validators-and-staking.md).

**Swap contract.** The contract on Hash Chain that holds the ETH side of a Monero swap. See [11](11-connection-monero.md).

**T0, T1.** The two swap deadlines, measured from the ETH lock. `T0` is the deadline to signal ready, `T1` the deadline to claim. See [11](11-connection-monero.md).

**Trusted setup.** A one-time ceremony required by some proof systems. Whether Hash Chain needs one is to be specified.

**Validator.** A participant that stakes ETH, verifies proofs, orders updates and takes part in finality. See [02](02-validators-and-staking.md).

**Validator set.** The group of staked validators currently running the network.

**Verifier.** The party that checks a proof or credential presentation, such as a bank, auditor or app.

**View key.** A read-only key, derived from the master secret, that lets a chosen person see a scoped part of your history. It cannot spend. See [08](08-proofs-and-view-keys.md).

**Watchtower.** An optional service that acts for an offline user, such as submitting a refund, without learning secrets. To be specified.

**XMR holder.** In a Monero swap, the party who locks XMR and wants ETH on Hash Chain. See [11](11-connection-monero.md).

**Zero-knowledge proof.** A proof that a statement is true without revealing why or the underlying data.

## Technical details

Where a term has a precise draft definition, such as note, commitment and nullifier, see [03](03-accounts-and-private-transfers.md).

## Open questions

- Whether the spec should adopt standard external terminology for liquidity provider roles.
- Names for the two roles in a Monero swap that read well in wallets.
