# 07 Fees and fee privacy

> Draft research note. Pre-launch, spec stage. The recommendation is proposed, not final. Addresses [issue 7](https://github.com/HashChainL0/protocol-spec/issues/7). A concrete proposal derived from this note is in HIP-4.

## In short

Fees on Hash Chain are paid in ETH. The hard part is not the currency, it is that a fee is usually paid by someone, and that someone is visible. This note compares fee payment patterns from existing systems, looks at what an observer can learn from fee fields, and proposes that a private account pays a fixed, shape-based fee directly from its shielded balance inside the proof.

## Context

[12](../docs/12-fees.md) lists six fee components: execution, proof verification, data availability, connection, fast transfer and swap. The fee is a public input bound to the proof, and "private fee payment without a public payer" is an open question. On a public chain the sender of a transaction pays gas from a visible account. If Hash Chain required a public account to pay, every private transfer would be tied to a public identity at the moment of payment, and the privacy of the base layer would be void.

## What a fee can leak

| Field | What an observer can learn |
| --- | --- |
| Payer account | Links the update to a public identity if the payer is public |
| Fee amount, if it varies by transaction | The shape, size or urgency of the transaction, and a fingerprint of the wallet software |
| Priority tip | Urgency, and often identity, because tippers behave consistently |
| Payload size | Number of outputs and memo length |
| Fee asset | The user holds that asset |
| Submission time | Weak link between a user's sessions |

## Options for who pays

### A. Public fee payer account

The user pays from a visible account, as on most public chains. Ethereum's EIP-1559 splits the fee into a protocol-set base fee and a tip. Simple, and it destroys sender privacy. Rejected as the default.

### B. Relayer paid out of the transfer

Tornado Cash relayers submit a withdrawal and are paid a fee taken from the withdrawn amount, with the fee bound into the proof. The user's account never appears as payer. Costs: a relayer is a counterparty who sees the request, relayers must be found and trusted not to censor, and the fee amount is public in the transaction.

### C. Account abstraction paymasters

ERC-4337 paymasters pay gas on behalf of a user and are repaid in another way, including by the user in a token or by an app. This allows sponsorship but gives the paymaster a view of who it sponsors, and it keeps a public bundler in the path.

### D. Fee inside the shielded transaction

The fee is paid from the shielded balance as part of the same proof. Zcash does this: the transparent fee is the value balance of the transaction, so there is no payer account, although the fee amount is public. Zcash ZIP 317 sets the fee as a function of the number of logical actions, so fee reveals shape and not who paid. Penumbra pays fees from a shielded balance and prices gas per asset. Aztec supports private fee payment through fee payment contracts and sponsorship.

## Pricing each component

Existing designs give three useful ideas.

- Separate markets for separate resources. Ethereum already split execution gas from blob data after EIP-4844, with an independent price for each. Multidimensional fee markets are discussed for the same reason: one scarce resource should not drive the price of another.
- Prices set by the validator set. Sui validators declare a gas price and an epoch reference price comes out of the weighted quotes (approximate, from Sui documentation). Epoch-level prices stay stable for users.
- Cost-derived prices. Proof verification cost depends on circuit shape and is known in advance. Data cost depends on bytes stored. Connection cost depends on gas on the other network, which moves and needs a price feed or an estimate refreshed per epoch.

## Tradeoffs

| Option | Payer hidden | Fee amount hidden | Needs third party | Abuse resistance |
| --- | --- | --- | --- | --- |
| A. Public payer | No | No | No | Strong |
| B. Relayer | Mostly | No | Yes, relayer | Medium |
| C. Paymaster | Partly | No | Yes, paymaster | Medium |
| D. In-proof, fixed by shape | Yes | Reduced to shape class | No | Strong, fee is spent from a real note |

Option D has the strongest abuse story, because spam costs real ETH from a real note and the fee is checked by the same proof as everything else. Its cost is that every user needs a shielded ETH balance to pay. Someone who holds only a stablecoin cannot pay until they get some ETH, or a sponsor covers them.

## Recommendation (proposed, not final)

1. Pay fees from the shielded balance, in ETH, inside the transfer proof (option D).
2. Make the fee a function of transaction shape and epoch only. No tip, no overpay. The fee field then reveals the shape class and nothing else. Pad shapes and encrypted payloads to a small set of standard sizes so the shape class is also coarse.
3. Collect fees into a per-epoch fee pool and pay validators from it, so rewards are not tied to a single transaction's proposer.
4. Let sponsored fees work through blind, rate-limited fee credits that an app or merchant issues and that a wallet redeems, instead of a paymaster that sees each user. Anonymous credential schemes such as blind tokens are the starting point. To be specified.
5. Price connection and fast transfer components separately, since they pay costs outside the network and appear anyway at the contract on the connected network.

The concrete mechanism is written up in HIP-4.

## Open questions

- Congestion pricing without tips: a per-epoch price adjusted by load, or a lottery or queue.
- Whether any fee field can be hidden entirely, using a committed fee checked against a range.
- How a new user receives their first ETH note without a public payer, for example a connection deposit that carries a fee allowance.
- Data availability pricing when storage nodes are separate from validators.
- Abuse resistance of sponsored credits and how a sponsor learns nothing about redeemers.
- Whether the fee asset should ever be anything other than ETH.

## References

EIP-1559, EIP-4844. ERC-4337. Zcash protocol specification and ZIP 317. Penumbra protocol documentation (fees and gas). Aztec documentation (fee payment). Tornado Cash relayer documentation. Sui documentation (gas pricing).
