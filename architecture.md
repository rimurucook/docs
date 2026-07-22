# Architecture Overview

## How It Works

Noelclaw is a local MCP server (`@noelclaw/mcp`) that runs as a Node.js process on your machine. When you ask your AI a question, the MCP server handles it - some requests are answered directly from local data, others call the Noelclaw API.

```
  AI Clients
  ┌──────────────────────────────────────────────┐
  │  Claude Desktop / Claude Code / Cursor       │
  │  Windsurf / Hermes / Aeon / Bankr            │
  └─────────────────┬────────────────────────────┘
                    │ MCP protocol (stdio)
                    ▼
         ┌─────────────────────┐
         │   @noelclaw/mcp     │
         │   Node.js, local    │
         │   121 tools         │
         └──────────┬──────────┘
                    │
       ┌────────────┼────────────────┐
       │            │                │
       ▼            ▼                ▼
  CoinGecko   DeFiLlama       api.noelclaw.com
  (prices)    (yields)        (vault, memory,
  free, no    free, no         automations,
  key needed  key needed       swarm, AI, etc.)
```

---

## What Runs Where

| Category | Where | Details |
|---|---|---|
| Market prices | CoinGecko (direct) | Free, no API key needed |
| DeFi yields | DeFiLlama (direct) | Free, no API key needed |
| Token swap quotes | 0x Protocol (direct) | Free routing API |
| Vault, memory, automations | `api.noelclaw.com` | Persisted to your account |
| AI analysis (ask_noel, thesis, trade plan) | `api.noelclaw.com` | Proxied to Claude or Grok |
| Swarm, monitors, OS tools | `api.noelclaw.com` | Noelclaw backend |
| Wallet signing | Local only | Never leaves your machine |

---

## API - api.noelclaw.com

All cloud tools go through `api.noelclaw.com`. This endpoint handles:

- **Auth** - every request is authenticated by a wallet signature (generated automatically) or a session token
- **Rate limiting** - 100 requests/min per IP
- **BYOK header forwarding** - your own API keys (`X-User-*` headers) are forwarded to the right backend
- **Retry handling** - auto-retry on 429 and 5xx responses

---

## Authentication

Noelclaw uses wallet-based auth by default - no account or login required.

```
First run:
  ~/.noelclaw/wallet.json is created
  → contains a local Ethereum keypair (Base mainnet)
  → address is derived from the private key

Per request:
  MCP server signs: "noelclaw:{toolName}:{timestamp}"
  → sends X-Wallet-Address + X-Wallet-Signature + X-Wallet-Timestamp
  → API verifies the signature
  → request is authorized
```

Your private key never leaves your machine. The API only sees the wallet address and the signature - never the key itself.

If you have a session token (`NOELCLAW_SESSION_TOKEN`), it's used instead of wallet signing - simpler, and unlocks full tool access without a funded wallet.

---

## Data Flow: Tool Call

```
User prompts AI
      │
      ▼  MCP protocol (stdio)
@noelclaw/mcp - validates input with Zod
      │
      ├── Market/yield tools → direct to CoinGecko / DeFiLlama
      │
      └── Everything else → HTTPS POST to api.noelclaw.com
            → auth header (wallet sig or session token)
            → BYOK headers forwarded if set
            → response returned to MCP server
                  │
                  ▼  MCP protocol
            AI client renders result
```

---

## Data Flow: Autonomous Monitor

```
schedule_research called
      │
      ▼
1. Schedule registered on Trigger.dev (via api.noelclaw.com)
2. Monitor config (topic, label, cron) saved to vault
      │
At scheduled time (e.g. 8am daily):
      ▼
3. Trigger.dev fires the worker
4. Worker calls Firecrawl to search the web for the topic
5. LLM summarizes findings, assigns urgency 1–5
6. Summary saved to vault (versioned history)
7. Telegram notification sent (if configured)
      │
      ▼
Next run: compares new findings to previous briefing
→ highlights what changed since last time
```

---

## Data Flow: Wallet Transactions

```
base_mcp_swap / base_mcp_send called
      │
      ▼
@noelclaw/mcp:
  → fetches quote / tx params from api.noelclaw.com
  → MCP server signs the transaction locally (ethers.js)
  → MCP server broadcasts signed tx to Base mainnet directly
```

The private key is at `~/.noelclaw/wallet.json` and never leaves the local machine. The API helps build the transaction but never holds or sees your key.

---

## Data Flow: Swarm Research

```
swarm_research called
      │
      ▼
POST api.noelclaw.com/swarm/research
  → 5 agents run in parallel:
      market-monitor, sentiment-tracker,
      workflow-executor, memory-manager, risk-verifier
  → agents share findings via common memory
  → results merged into one summary
  → auto-saved to vault
```

---

## Security Model

| Concern | How It's Handled |
|---|---|
| Private key | Stored at `~/.noelclaw/wallet.json`, never sent anywhere |
| API keys | Set as env vars in your MCP config, forwarded as `X-User-*` headers, never logged |
| Auth | Wallet signature per request - or session token from app.noelclaw.com |
| Rate limiting | 100 req/min per IP at the API layer |
| Agent safety | Sentinel gates all Framework actions before execution |
| Private key decryption | `getDecryptedPKByUserId` is internalAction only — never callable from client |
| Wallet creation | `createWallet` is internalAction only — prevents arbitrary wallet creation |
| Private key retrieval | `getPrivateKey` returns address only, never the raw key |
| Session / API key abuse | Rate-limited auth + short-lived session tokens |

**8 security boundaries** enforced across the backend + **5 vulnerability fixes** in v3.32.7.

---

## Environment Variables

All optional. Set in the `env` block of your MCP config.

| Variable | What It Does |
|---|---|
| `NOELCLAW_SESSION_TOKEN` | Session token from app.noelclaw.com - simplest auth, recommended |
| `ANTHROPIC_API_KEY` | Use your own Claude quota instead of the platform |
| `BANKR_API_KEY` | Use Bankr/Grok instead of Anthropic |
| `FIRECRAWL_API_KEY` | Required for `deep_research` and `web_search`; optional for `web_scrape` (falls back to basic fetch) |
| `TRIGGER_SECRET_KEY` | Required for schedule_research (Trigger.dev) |
| `TELEGRAM_BOT_TOKEN` | Your Telegram bot token - for monitor notifications |
| `TELEGRAM_CHAT_ID` | Your Telegram chat ID - for monitor delivery |
| `ALCHEMY_API_KEY` | Faster swap quotes and Base balance lookups |
