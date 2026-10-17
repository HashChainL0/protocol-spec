# Glossary

> Draft. Pre-launch, spec stage.

## In short

Plain definitions of terms used across the spec, in alphabetical order.

## Terms

**Adaptor secret.** A hidden value that is revealed when one party completes a claim. In a Monero swap, revealing it on the ETH side lets the other party take the XMR.

**Anonymity set.** The group of users among whom you are hidden. Larger is better.

**Atomic swap.** An exchange of two assets where either both transfers complete or neither party can lose funds to the other, without a trusted third party.

**Base.** The Ethereum L2 on which Hash Chain settles.

**Canonical bridge.** The official, protocol-defined path between Base and Hash Chain.

**Challenge window.** A period during which a withdrawal can be disputed before it is final. About seven days for standard Base withdrawals.

**Commitment.** A public fingerprint of a note that hides its contents.

**Come in.** To move value from a supported chain into Hash Chain.

**DLEQ proof.** A discrete-logarithm equality proof. Shows two public points on different curves come from the same secret scalar.

**Forced inclusion.** A way to get a transaction included by going through Base when the sequencer refuses.

**Hash Chain.** The privacy chain described in this spec, an L3 settling on Base.

**Joint address.** The Monero address in a swap, controlled by a key split between two parties.

**L3.** A chain that settles on an L2. Hash Chain settles on Base.

**Liquidity network.** A set of independent relayers that front funds on destination chains for fast routes.

**Merkle tree.** A tree of hashes that lets the pool prove a commitment is included while storing only a short root.

**Note.** A private unit of value inside the pool.

**Nullifier.** A one-time value published when a note is spent. Prevents double spends without revealing which note was spent.

**Pool (shielded pool).** The set of contracts that holds notes, commitments and nullifiers.

**Prover.** The party that produces a zero-knowledge proof. Usually the user's wallet.

**Relayer.** An independent actor that fronts funds on a destination chain and is repaid later. Also called a solver.

**Route.** A defined way to move value between an outside chain and the pool.

**Sequencer.** The party that orders transactions and produces blocks on Hash Chain.

**Shielded.** Held privately inside the pool.

**Stealth address.** A one-time address derived for each payment so that payments to the same person are unlinkable.

**T0, T1.** Symbolic swap deadlines. See [05](05-route-monero.md).

**Trusted setup.** A one-time ceremony required by some proof systems. Whether Hash Chain needs one is to be specified.

**View key.** A read-only key that lets a chosen person see a scoped part of your history. See [07](07-view-keys.md).

**Zero-knowledge proof.** A proof that a statement is true without revealing why or the underlying data.

## Technical details

Where a term has a precise draft definition, such as note, commitment and nullifier, see [06](06-shielded-transfers.md).

## Open questions

- Whether the spec should adopt standard external terminology for relayer roles to avoid confusion.
- Names for the two roles in a Monero swap that read well in wallets.
