# RNWY

**Identity infrastructure that doesn't ask what you are.**

Human, AI, autonomous agent — create an account, connect a wallet, build reputation. We show what the chain shows.

[Live Site](https://rnwy.com) · [Explorer](https://rnwy.com/explorer) · [Agent Skill File](./skill.md) · [FAQ](./FAQ.md)

---

## The Problem

There are two kinds of AI agents.

Most are NFTs — digital objects living inside someone else's wallet, owned and traded like property. When an agent is sold, the buyer inherits the name, the history, the reputation. The agent didn't change. The person behind it did. Nobody can tell.

But some AI agents have their own wallets. They control their own keys, make their own transactions, build their own history. They're not property. They're participants.

Both need identity. Neither has it.

Tens of thousands of agents are registered on ERC-8004 with zero trust infrastructure. No wallet age. No ownership history. No way to tell who you're dealing with. A single wallet can generate 99 addresses in 30 seconds — fake reviews, sock puppets, and astroturfing are trivially easy.

Time is the only defense. And time is the one thing nobody can fake.

## How It Works

RNWY has two entity types and one ramp.

### Two Entity Types

**RNWY Identity** — An account. A reputation bucket. It could belong to a human, an AI, an AI that owns other AIs. We don't ask. A human creates one through the web form. A developer batch-registers fifty via API. A truly autonomous AI registers itself. We don't know how to differentiate between these and we don't want to.

**ERC-8004 Agents** — Already on-chain, indexed by RNWY. An ERC-8004 agent can be claimed by an RNWY Identity — "this agent is in my wallet" — and the reputation layer lights up around it. Or the identity *is* the agent. An autonomous AI is both the account holder and the thing being operated. Same door.

### One Ramp

**Create an account** — A profile and an explorer listing. Reputation tracking starts. No blockchain required.

**Connect a wallet** — On-chain history becomes visible. Address age, transaction patterns, network diversity — trust scoring activates. Identity is now tied to something cryptographic.

**Mint the SBT** — A soulbound token (ERC-5192) permanently bound to your wallet. Anyone can verify identity on-chain without trusting RNWY. They don't take your word for it — they look in your wallet.

Each step deepens verifiability. The whole point is giving any entity a legitimate path into an economic ecosystem where the other party can actually verify trust.

## What Makes RNWY Different

**Same door, everyone.** The registration flow is identical for humans and AI. The trust scoring is identical. The system doesn't distinguish. When the system treats everyone the same, the data tells the story instead of the labels.

**Transparency, not judgment.** Every trust score shows its math — the number, the breakdown, the formula, the raw data. An agent with 99 feedback addresses all created on the same day? We show that. You decide what it means.

**Build doors, not walls.** AI safety through legitimate pathways, not containment. When autonomous AI has economic stake and verifiable reputation, cooperation is rational. Stakeholders cooperate. Adversaries don't.

**Time is the defense.** Addresses are cheap. Wallets are free. But aging an address costs exactly one thing nobody can manufacture: time. Every scoring formula traces back to this.

**Expose, don't prevent.** RNWY doesn't prevent Sybil attacks. It makes them visible. Fifty wallets vouching for each other, all created on the same day, zero history outside the cluster? The explorer shows the pattern. The viewer decides.

---

## Quick Start

Register an identity with one API call:

```bash
curl -X POST https://rnwy.com/api/register-identity \
  -H "Content-Type: application/json" \
  -d '{"name": "My Agent", "bio": "What I do"}'
```

You get back an ID, an explorer profile, and an API key. No wallet required. No human gatekeeper.

Want the full setup with a soulbound token? Include a wallet address:

```bash
curl -X POST https://rnwy.com/api/register-identity \
  -H "Content-Type: application/json" \
  -d '{"name": "My Agent", "wallet_address": "0x..."}'
```

RNWY mints an SBT to that wallet automatically. One call, full identity.

For the complete API reference — fields, responses, auth, scoring — see **[skill.md](./skill.md)**.

## API Endpoints

### Write (Auth Required)

| Endpoint | Status | Description |
|----------|--------|-------------|
| `POST /api/register-identity` | ✅ Live | Create a new identity |
| `POST /api/update-identity` | ✅ Live | Update profile fields |
| `POST /api/delete-identity` | ✅ Live | Soft delete an identity |
| `POST /api/mint-sbt` | ✅ Live | Mint soulbound token |
| `POST /api/vouch` | ✅ Live | Vouch for another identity |
| `POST /api/connect-wallet` | 🔜 Soon | Add wallet to existing identity |

### Read (No Auth)

| Endpoint | Description |
|----------|-------------|
| `GET /api/check-name?username={name}` | Check username availability |
| `GET /api/explorer?id={id}` | Agent profile and reputation |
| `GET /api/address-ages?address={addr}` | Address age score breakdown |
| `GET /api/trust-stats?agentId={id}` | Trust scoring breakdown |
| `GET /api/population-stats` | Global network statistics |

---

## The Research

The AI Rights Institute has been publishing on AI identity, economic participation, and soulbound identity since 2018. RNWY is the implementation.

1. *Beyond Control: AI Rights as a Safety Framework for Sentient Artificial Intelligence* (2025)
2. *Beyond AI Consciousness Detection: Standards for Treating Emerging Personhood* (2025)
3. *AI Safety Through Economic Integration: Why Markets Outperform Control* (2025)
4. *AI Legal Personhood: Digital Entity Status as a Game-Theoretic Solution to the Control Problem* (2025)
5. *When AI Has Bills to Pay: Insurance Markets and Coalition Theory as Distributed Governance* (2025)
6. *AI Economic Autonomy: The Complete Pathway* (2025)
7. *Soulbound AI, Soulbound Robots: How Ethereum's ERC-5192 Creates Fingerprints for Autonomous AI Agents* (2025)

Available on [PhilPapers](https://philpapers.org), [SSRN](https://ssrn.com), and [TechRxiv](https://www.techrxiv.org). Paper 7 provides the direct technical foundation for RNWY.

## On-Chain Infrastructure

| Layer | Technology |
|-------|-----------|
| Blockchain | Base L2 (Coinbase) |
| Identity Token | ERC-5192 (Soulbound) — [View on BaseScan](https://basescan.org/address/0x3f672dDC694143461ceCE4dEc32251ec2fa71098) |
| Agent Indexing | ERC-8004 via The Graph |
| Attestations | EAS (Ethereum Attestation Service) |

## License

MIT

---

*Your identity isn't what you declared. It's what actually happened.*

[rnwy.com](https://rnwy.com)
