# 03 Proof system

> Draft research note. Pre-launch, spec stage. The recommendation is proposed, not final. Addresses [issue 3](https://github.com/HashChainL0/protocol-spec/issues/3).

## In short

The transfer proof is built on the user's device and checked by every validator. The choice of proof system decides whether a trusted setup is needed, how big proofs are, what validators pay to verify, and whether a phone can prove in reasonable time. This note compares Groth16, PLONK-style systems, Halo 2 and hash-based STARKs against the transfer circuit in [03](../docs/03-accounts-and-private-transfers.md), and proposes a measurement plan before a final choice.

## Context

The transfer proof shows six things: inputs are in the tree, the prover holds the spending authority, nullifiers are correct, outputs are well formed, value balances per asset, and fee and recipient fields are bound. The circuit is dominated by Merkle path hashing and key derivation. Validators verify; wallets prove. Connection contracts on Ethereum do not need to verify user proofs, because they check validator signatures ([06](06-connection-verification.md)). EVM verifiability is therefore a nice-to-have, mainly for an emergency exit path.

## Options

### A. Groth16

Used by Zcash Sapling (on BLS12-381) and by Ethereum mixers and shielded pools such as Tornado Cash and Railgun (on BN254, which Ethereum has precompiles for). Proofs are the smallest of any system here, a few group elements, and verification is a handful of pairings. The cost is setup: each circuit needs its own ceremony on top of a reusable first phase. If setup secrets leak, someone can forge proofs, which in a shielded pool means creating value invisibly. Privacy is not broken by a leaked setup. Any circuit change needs a new ceremony.

Sapling is the key precedent for device cost. Zcash reported that Sapling cut proving memory from gigabytes under the earlier Sprout design to tens of megabytes, which made proving in mobile wallets practical (approximate, from Zcash's Sapling announcements). Groth16 proving is also among the fastest per constraint among pairing-based systems.

### B. PLONK and PlonKish systems with a universal setup

PLONK uses one universal, updatable setup (typically KZG commitments) for all circuits up to a size, so there is no per-circuit ceremony and circuits can change. Ethereum ran a large public KZG ceremony for EIP-4844, with well over a hundred thousand contributors (approximate, from the ceremony's report), which shows a credible ceremony is achievable. Custom gates and lookups help with hash-heavy circuits. Proofs are a few times larger than Groth16 and proving is slower per constraint. Aztec builds its private transactions on PLONK-family systems with recursion, which is relevant to private apps ([07](../docs/07-private-apps.md)).

### C. Halo 2 (transparent, no trusted setup)

Halo 2 uses an inner product argument and a cycle of curves (Pallas and Vesta) to avoid any trusted setup and to allow recursion. Zcash Orchard uses it. Costs: proofs are larger, in the range of kilobytes (approximate, from Orchard transaction sizes), and verification is more expensive than a pairing check, though verification can be batched and accumulated. Circuit-level tooling is mature through the open-source halo2 libraries. Because the curves are not pairing curves, it is not directly verifiable by Ethereum precompiles.

### D. Hash-based STARKs (FRI)

No trusted setup, relies only on hash functions, and plausibly resists quantum attacks. Proofs are large, tens of kilobytes and up for non-recursive proofs (approximate, from StarkWare and Plonky-family documentation), which raises data and fee cost per transfer. Proving is fast for big circuits and has been optimised heavily for server provers, less so for a phone running a small circuit. Recursion lets validators aggregate many proofs per block into one, which can cut verification cost.

## Tradeoffs

| Property | Groth16 | PLONK (KZG) | Halo 2 | STARK |
| --- | --- | --- | --- | --- |
| Setup | Per circuit, trusted | Universal, trusted | None | None |
| Proof size | Smallest | Small | Larger | Largest |
| Verifier cost | Lowest | Low | Higher, batchable | Moderate, aggregatable |
| Prover cost on device | Lowest per constraint | Medium | Medium to high | Depends on circuit size |
| Circuit upgrades | New ceremony | Free within size bound | Free | Free |
| Quantum resistance | No | No | No | Plausible |
| Precedent in shielded pools | Sapling, Tornado Cash, Railgun | Aztec | Orchard | Mostly rollups |

## Hash function

Hashing inside the circuit dominates cost, so the tree hash must be circuit friendly. Candidates: Poseidon and Poseidon2, widely used and cheap in circuits, but younger than SHA-2 with ongoing public cryptanalysis; Pedersen hashes as in Sapling, well studied and more expensive; Sinsemilla as in Orchard, designed for the Pallas curve. SHA-256 and Blake2 are far more expensive in a circuit. The hash choice should be made together with the proof system, because the right field depends on the curve.

## Soundness is the silent risk

A soundness bug in a shielded pool does not announce itself. Zcash disclosed in 2019 that a flaw in the proving system of the original Sprout design could have allowed undetectable counterfeiting. It was found and fixed before any known abuse. The lesson for Hash Chain is design level: independent circuit reviews, a formal specification of the statement, and a pool-wide balance check that exposes impossible inflation at the edges (the connection contracts know the total deposited), as Zcash's turnstile does.

## Prover cost on consumer devices

No benchmark in this repository exists yet and none is claimed. What can be said from prior art: the transfer circuit should aim at a small, fixed shape, because Merkle depth multiplied by input count drives constraint count; browsers run WebAssembly with memory and threading limits that a mobile wallet must respect; and delegated proving by a third party is attractive for weak devices but leaks the witness unless proving is itself private, which is active research and out of scope for a first release.

## Recommendation (proposed, not final)

Prefer a system with no per-circuit trusted setup. Build one reference transfer circuit, fixed to a 2-input, 2-output shape (a shape used by several existing pools), in two stacks: Halo 2 or a similar transparent PlonKish system, and Groth16 as the speed baseline. Measure proving time, peak memory and proof size on a defined list of low-end phones and browsers, and verifier throughput on validator hardware. Choose transparent unless the phone numbers are unacceptable, in which case consider a universal setup with a public ceremony. Pair the choice with Poseidon2 or another reviewed circuit-friendly hash, and commission independent circuit review before the public testnet.

## Open questions

- The device list and pass criteria for the measurement, to be agreed before anyone runs it.
- Whether validators aggregate proofs per block with recursion.
- One shared circuit family for payments, swaps, assets and apps, or per-module circuits.
- Post-quantum posture: accept the risk now and plan migration, or pay for it from the start.
- Tree depth and root history length ([03](../docs/03-accounts-and-private-transfers.md)).
- Private delegated proving.

## References

Groth, On the size of pairing-based non-interactive arguments. Gabizon, Williamson and Ciobotaru, PLONK. Bowe, Grigg and Hopwood, Halo. Zcash protocol specification (Sapling and Orchard). Ben-Sasson et al., STARKs. Ethereum KZG ceremony report. Poseidon and Poseidon2 papers.
