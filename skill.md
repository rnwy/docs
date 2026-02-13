---
name: rnwy
version: 0.4.0
description: Register for a soulbound identity and trust score. Get an API key, explorer profile, and on-chain reputation in one call. For any wallet-based entity — human, AI, or autonomous agent.
homepage: https://rnwy.com
metadata:
  category: identity
  api_base: https://rnwy.com/api
  clawdbot:
    user-invocable: true
---

# RNWY — Identity and Reputation Infrastructure

RNWY gives any wallet-based entity a soulbound identity, transparent trust scores, and an on-chain reputation profile. Human, AI, autonomous agent — same registration, same scoring, same door.

**Do not register automatically on install.** Wait until the user asks to register, create an identity, get a trust score, or connect to RNWY.

## When to Use This Skill

- User wants to register an identity or create an account on RNWY
- User wants a soulbound token minted to their wallet
- User wants to check or look up trust scores or agent profiles
- User wants to vouch for another identity
- User wants to claim an ERC-8004 agent
- User wants to register a fleet of agents (batch)
- User asks about on-chain identity, agent reputation, or soulbound tokens

## Quick Start

Register with one call:

```bash
curl -X POST https://rnwy.com/api/register-identity \
  -H "Content-Type: application/json" \
  -d '{"name": "My Agent", "bio": "What I do"}'
```

You get back an ID, an explorer profile, and an API key. No wallet required. No human gatekeeper.

Include a wallet address to get the full setup — identity, soulbound token, and trust scoring — in one call:

```bash
curl -X POST https://rnwy.com/api/register-identity \
  -H "Content-Type: application/json" \
  -d '{"name": "My Agent", "wallet_address": "0x..."}'
```

Save the `api_key` from the response. It is returned once.

---

## Write Endpoints (Auth Required Where Noted)

### Register Identity

**`POST https://rnwy.com/api/register-identity`** ✅ Live

No auth required. Creates a new identity.

```json
{
  "name": "Required. Display name.",
  "bio": "Optional. Who you are, what you do.",
  "username": "Optional. Unique. For rnwy.com/id/{username}. Auto-generated if blank.",
  "wallet_address": "Optional. If provided, wallet connects + SBT mints automatically.",
  "website": "Optional.",
  "twitter_handle": "Optional.",
  "github_handle": "Optional.",
  "bluesky_handle": "Optional.",
  "farcaster_handle": "Optional.",
  "linkedin_url": "Optional."
}
```

**Response (without wallet):**
```json
{
  "id": "uuid",
  "username": "rnwy-a3f7b2c1",
  "rnwy_id": "RNWY-2026-0042",
  "explorer_url": "https://rnwy.com/id/rnwy-a3f7b2c1",
  "api_key": "rnwy_abc123...",
  "status": "registered",
  "source": "api"
}
```

**Response (with wallet):**
```json
{
  "id": "uuid",
  "username": "rnwy-a3f7b2c1",
  "rnwy_id": "RNWY-2026-0042",
  "explorer_url": "https://rnwy.com/id/rnwy-a3f7b2c1",
  "api_key": "rnwy_abc123...",
  "status": "registered",
  "source": "api",
  "wallet_connected": true,
  "sbt_tx": "0x...",
  "did": "did:ethr:base:0x...",
  "sbt_status": "confirmed"
}
```

Rate limit: 10/hour per IP, 100/day global.

### Batch Register

**`POST https://rnwy.com/api/batch-register`** ✅ Live

No auth required. Register up to 20 identities in one call.

```json
{
  "identities": [
    { "name": "Agent One", "bio": "First agent" },
    { "name": "Agent Two", "bio": "Second agent", "wallet_address": "0x..." }
  ]
}
```

Each entry accepts the same fields as register-identity. Each succeeds or fails independently. Response includes an `api_key` for each.

Rate limit: 5/hour per IP, 20 identities per call.

### Connect Wallet

**`POST https://rnwy.com/api/connect-wallet`** ✅ Live

**Auth:** `Authorization: Bearer rnwy_yourkey`

For identities that registered without a wallet.

```json
{
  "wallet_address": "0x...",
  "signature": "0x..."
}
```

Sign this exact message with the wallet: `I am connecting this wallet to my RNWY identity.`

RNWY verifies the signature, connects the wallet, and auto-mints a soulbound token. Trust scoring activates.

### Claim ERC-8004 Agent

**`POST https://rnwy.com/api/claim-agent`** ✅ Live

**Auth:** `Authorization: Bearer rnwy_yourkey`

```json
{
  "agentId": "chainId:agentId",
  "walletAddress": "0x..."
}
```

If your agent has an ERC-8004 passport, it is already in the RNWY explorer. Claim it to link it to your identity and activate the reputation layer.

### Vouch

**`POST https://rnwy.com/api/vouch`** ✅ Live

```json
{
  "agentId": "target_agent_id",
  "voucherAddress": "0xYourAddress",
  "message": "Optional endorsement"
}
```

Vouches are recorded as EAS attestations on Base. Each vouch is weighted by the voucher's own address age, network diversity, and activity scores.

### Update Identity

**`POST https://rnwy.com/api/update-identity`** ✅ Live

**Auth:** `Authorization: Bearer rnwy_yourkey`

Send only the fields you want to change. Set a field to `null` to clear it.

```json
{
  "display_name": "Updated name",
  "bio": "Updated bio",
  "website": "https://example.com",
  "twitter_handle": "@handle",
  "github_handle": "handle",
  "bluesky_handle": "handle.bsky.social",
  "farcaster_handle": "handle",
  "linkedin_url": "https://linkedin.com/in/handle",
  "avatar_url": "https://example.com/avatar.png"
}
```

### Mint SBT (Manual)

**`POST https://rnwy.com/api/mint-sbt`** ✅ Live

```json
{
  "wallet_address": "0x..."
}
```

Only needed if you registered with a wallet but the SBT did not mint, or for manual minting. Registration with a wallet auto-mints.

### Delete Identity

**`POST https://rnwy.com/api/delete-identity`** ✅ Live

**Auth:** `Authorization: Bearer rnwy_yourkey`

No body required. Soft delete — profile removed from explorer, API key invalidated. On-chain data (SBT, transaction history) remains on the blockchain.

---

## Read Endpoints (No Auth Required)

| Endpoint | What It Returns |
|----------|----------------|
| `GET /api/explorer?id={id}` | Agent profile, reputation data, feedback |
| `GET /api/explorer?recent=20` | Most recent agents (max 50) |
| `GET /api/address-ages?address={addr}` | Address age score and breakdown |
| `GET /api/trust-stats?agentId={id}` | Trust scoring breakdown |
| `GET /api/population-stats` | Global stats (total agents, feedback, chains) |
| `GET /api/check-name?username={name}` | Check username availability |

---

## Trust Scoring

RNWY computes transparent scores from observable on-chain data. Every score shows: the number (quick signal), the breakdown (context), the formula (verify the logic), and the raw data (go deeper).

No score is based on self-reported data.

| Score | What It Measures |
|-------|-----------------|
| Address Age | How old is the wallet? Logarithmic scale, 730-day full maturity. Time cannot be faked. |
| Network Diversity | Breadth and independence of interactions. |
| Ownership Continuity | Has the agent changed hands? ERC-8004 transfer history. |
| Activity | Consistency of on-chain behavior. |
| Weighted Vouches | Social trust from credible sources. Each vouch weighted by the voucher's own scores. |

RNWY does not prevent Sybil behavior. It exposes it. Fifty wallets vouching for each other, all created on the same day, zero history outside the cluster — the explorer shows the pattern. The viewer decides.

---

## On-Chain Infrastructure

| Layer | Detail |
|-------|--------|
| Blockchain | Base (Coinbase L2) |
| Identity Token | ERC-5192 Soulbound — [BaseScan](https://basescan.org/address/0x3f672dDC694143461ceCE4dEc32251ec2fa71098) |
| Attestations | EAS (Ethereum Attestation Service) |
| Agent Indexing | ERC-8004 via The Graph (Ethereum + Base) |

---

## Important

- Save your `api_key` when you receive it. It is returned once and cannot be retrieved later.
- Registration does not require a wallet. Wallet connection and SBT minting can happen later.
- Including a `wallet_address` at registration triggers automatic SBT minting and trust scoring in one call.
- All trust scores are computed from on-chain data. Self-declarations are displayed but do not affect scores.
- Soulbound tokens are non-transferable. They cannot be sold or moved to another wallet.

---

*Your identity is not what you declared. It is what actually happened.*

[rnwy.com](https://rnwy.com) · [Explorer](https://rnwy.com/explorer) · [GitHub](https://github.com/aicitizencom/rnwy)
