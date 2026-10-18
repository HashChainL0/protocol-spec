# Glossary

> Draft. Pre-launch, spec stage.

## In short

Plain definitions of terms used in the spec, in alphabetical order.

## Terms

**Account chain.** The ordered sequence of updates belonging to one account. Separate accounts have separate chains, so unrelated updates settle in parallel. See [01](01-architecture.md).

**Adaptor secret.** A hidden value that is revealed when one party completes a claim. In a Monero swap, revealing it on the ETH side lets the other party take the XMR.

**Anonymity set.** The group of users among whom you are hidden. Larger is better.

**Atomic swap.** An exchange of two assets where either both transfers complete or neither party can lose funds to the other, without a trusted third party.

**Base.** An Ethereum network that connects to Hash Chain. See [10](10-connections.md).

**Commitment.** A public fingerprint of a note that hides its contents.

**Connection.** A door between Hash Chain and another network: Ethereum, Base, Robinhood Chain or Monero. See [10](10-connections.md).

**Credential.** A signed statement from an issuer about an account, used privately inside proofs to show one fact. See [08](08-proofs-and-view-keys.md).

**Data availability.** The guarantee that the data recipients need, such as encrypted note payloads, can be retrieved. To be specified.

**DLEQ proof.** A discrete-logarithm equality proof. Shows two public points on different curves come from the same secret scalar.

**Finality.** The point at which an update can no longer be reversed. Hash Chain targets fast, deterministic finality.

**Hash Chain.** The privacy Layer 0 described in this spec: its own network with its own validators.

**Joint address.** The Monero address in a swap, controlled by a key split between two parties.

**Layer 0.** A base network that other networks plug into. Hash Chain is a Layer 0 with privacy as a base-layer property.

**Liquidity network.** A set of independent liquidity providers that front funds on destination networks for fast transfers.

**Liquidity provider.** An independent actor that fronts funds on a destination network and is repaid later.

**Merkle tree.** A tree of hashes that lets the network prove a commitment is included while storing only a short root.

**Note.** A private unit of value held by an account.

**Nullifier.** A one-time value published when a note is spent. Prevents double spends without revealing which note was spent.

**Private account.** An account whose balances and history are hidden by default.

**Prover.** The party that produces a zero-knowledge proof. Usually the user's wallet.

**Shielded.** Held privately on Hash Chain.

**Shielded state.** The set of commitments and nullifiers the validators maintain.

**Slashing.** Reducing a validator's stake after a provable fault.

**Stealth address.** A one-time address derived for each payment so that payments to the same person are unlinkable.

**T0, T1.** Symbolic swap deadlines. See [11](11-connection-monero.md).

**Trusted setup.** A one-time ceremony required by some proof systems. Whether Hash Chain needs one is to be specified.

**Validator.** A participant that stakes ETH, verifies proofs, orders updates and takes part in finality. See [02](02-validators-and-staking.md).

**View key.** A read-only key that lets a chosen person see a scoped part of your history. See [08](08-proofs-and-view-keys.md).

**Zero-knowledge proof.** A proof that a statement is true without revealing why or the underlying data.

## Technical details

Where a term has a precise draft definition, such as note, commitment and nullifier, see [03](03-accounts-and-private-transfers.md).

## Open questions

- Whether the spec should adopt standard external terminology for liquidity provider roles.
- Names for the two roles in a Monero swap that read well in wallets.
