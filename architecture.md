# Architecture Overview

## System Diagram

```
┌─────────────────────────────────────────────────────────┐
│                    User Interfaces                       │
│                                                          │
│  noelclaw.fun         noelapp (React 19 + Vite)         │
│  (Landing Page)       (Full Platform)                    │
│                            │                             │
│                     Privy / Email Auth                   │
└────────────────────────────┼─────────────────────────────┘
                             │
┌────────────────────────────▼─────────────────────────────┐
│            Cloudflare Worker — api.noelclaw.com           │
│   Rate limit: 100 req/min/IP  |  CORS  |  Header proxy   │
└────────────────────────────┬─────────────────────────────┘
                             │
┌────────────────────────────▼─────────────────────────────┐
│                   Convex Backend                          │
│  ┌──────────────┐  ┌──────────────┐  ┌───────────────┐  │
│  │   Queries    │  │  Mutations   │  │    Actions    │  │
│  │ (real-time)  │  │ (DB writes)  │  │ (node.js env) │  │
│  └──────────────┘  └──────────────┘  └───────┬───────┘  │
│                                               │           │
│  ┌────────────────────────────────────────────▼───────┐  │
│  │              HTTP Router (convex.site)              │  │
│  │  /mcp/market      /mcp/chat    /mcp/insight        │  │
│  │  /mcp/research    /signals/*   /whales/latest      │  │
│  │  /recap/today     /mcp/defi/*  /automations/*      │  │
│  │  /swarm/*         /swarm/ledger                    │  │
│  │  /user/telegram   /app/*                           │  │
│  └────────────────────────────────────────────────────┘  │
└──────────────────────────────────────────────────────────┘
                             │
        ┌────────────────────┼────────────────────┐
        │                    │                     │
┌───────▼──────┐  ┌──────────▼────────┐  ┌────────▼─────┐
│ Bankr LLM +  │  │   0x Permit2 API  │  │   Grok API   │
│ Agent APIs   │  │   swap routing    │  │  X sentiment │
│ (all agents) │  │   Base mainnet    │  │   analysis   │
└──────────────┘  └───────────────────┘  └──────────────┘
                             │
                  ┌──────────▼─────────┐
                  │   Telegram Bot API │
                  │  (signals/alerts)  │
                  └────────────────────┘

                    ┌────────────────────────┐
                    │   MCP Server v1.4.1    │
                    │   @noelclaw/mcp        │
                    │  12 modules, 37 tools  │
                    │  stdio transport       │
                    └───────────┬────────────┘
                                │
              ┌─────────────────┼──────────────────┐
              │                 │                  │
       ┌──────▼──────┐  ┌───────▼──────┐  ┌───────▼──────┐
       │   Hermes    │  │ Claude Code  │  │   Cursor /   │
       │  OpenClaw   │  │   Desktop    │  │   Windsurf   │
       └─────────────┘  └──────────────┘  └──────────────┘
```

---

## Frontend Stack

| Layer | Technology |
|-------|-----------|
| Framework | React 19.2.0 + TypeScript |
| Build | Vite 7.2.4 |
| Styling | TailwindCSS 3.4 + Radix UI |
| Animation | Framer Motion 12.38, GSAP |
| State | Zustand |
| Data | Convex React SDK (real-time) |
| Auth | Privy SDK + Noelclaw email/password |
| Web3 | Wagmi 3.6 + Viem 2.48 + Ethers 6.16 |
| Payments | x402 protocol (Base mainnet) |
| Games | Phaser 3.90 (PokemonAutoChess), Canvas API |

---

## Backend Stack (Convex)

Convex is the entire backend — database, serverless functions, scheduling, and HTTP router in one.

| Layer | Details |
|-------|---------|
| Database | Convex (document DB with real-time subscriptions) |
| Serverless | Actions (`"use node"`) for external API calls |
| Scheduling | Cron jobs: research (5 min), signals (daily 08:00 UTC), whale alerts (6h), weekly recap (Sunday 23:55 UTC) |
| HTTP | `convex.site` endpoints for MCP + webhooks, proxied via `api.noelclaw.com` |
| Auth | JWT validation via Privy tokens + Noelclaw session tokens |

---

## Cloudflare Proxy Layer

`api.noelclaw.com` is a Cloudflare Worker that sits in front of the Convex backend:

- **Hides** the raw Convex URL from users and MCP configs
- **Rate limiting:** 100 req/min per IP (KV-based fixed-window counter)
- **CORS:** returns correct headers for browser clients
- **Header forwarding:** passes all `X-User-*` BYOK headers through to Convex
- **Error handling:** returns 503 if Convex is unreachable, 429 with `Retry-After` on rate limit

Source: `cloudflare-api-proxy/worker.js`

---

## Noel Framework / Sentinel

Every agent action in the swarm passes through a mechanical safety gate before execution:

```
Agent run requested
      │
      ▼
steward.check (internalAction)
  1. DoNotDo check — blocked keywords per agent
  2. Territory check — action must be in agent's allowed domain
  3. Value limit — reject if USD value exceeds per-agent cap
  4. Grudge book — block repeated bad actors (per-user, per-agent)
  5. Rate limit — max N actions per agent per 60s window
      │
      ▼
decision: approved | warned | blocked
      │
      ├── blocked → throw error, log to stewardLedger
      └── approved/warned → continue to agent execution
```

**Tables:** `stewardRules`, `grudgeBook`, `stewardLedger`

The ledger is visible in the Swarm Dashboard → Steward Ledger section.

---

## Data Flow: User Sends Chat Message

```
User types message
      │
      ▼
React component (Chat.tsx)
      │
      ▼
Convex action: api.chat.chat
      │
      ▼
Bankr LLM gateway (https://llm.bankr.bot)
  → model: gpt-4o / claude-sonnet-4-6
      │
      ▼
Response streamed back
      │
      ▼
Convex mutation: save to DB
      │
      ▼
React re-renders (real-time subscription)
```

---

## Data Flow: On-Demand Research

```
User calls research(query: "...") via MCP or HTTP POST /mcp/research
      │
      ▼
Convex action: research.queryResearch
  → POST to Bankr LLM gateway (web search + AI analysis)
  → returns structured text in seconds: overview, findings, impact, sentiment
      │
      ▼
MCP server formats and returns result to user
```

---

## Data Flow: Trading Signal (Daily)

```
Convex cron fires at 08:00 UTC
      │
      ▼
signalEngine.ts — internalAction
  → sends 4H market analysis prompt to Bankr LLM
  → parses JSON: signal, entry, TP1, TP2, SL, confidence
  → saves to tradingSignals table
  → sends Telegram signal card
      │
      ▼ (every 2h, cron)
outcomeTracker.ts
  → fetches live price from Bankr
  → for each PENDING signal:
       if price >= TP1 → WIN
       if price <= SL  → LOSS
       if age > 6h     → EXPIRED
  → updates tradingSignals record
```

---

## Data Flow: DeFi Wallet (MCP)

```
MCP client calls swap_tokens / send_token
      │
      ▼
MCP server (local Node.js process)
      │
      ├── swap_tokens:
      │     POST api.noelclaw.com/mcp/defi/swap
      │     → Convex fetches 0x Permit2 quote
      │     → returns quote to MCP server
      │     → MCP server signs + broadcasts locally (ethers.js)
      │     → transaction signed on user's machine, not server
      │
      └── send_token:
            POST api.noelclaw.com/mcp/defi/send
            → Convex builds tx params
            → MCP server signs + broadcasts locally
```

All transaction signing happens in the MCP server process on the user's machine. Convex only provides quotes and tx parameters — it never holds a signing key for MCP users.

---

## MCP Architecture (v1.4.1)

The MCP server is split into 13 modules:

```
mcp-server/src/
  index.ts        — entrypoint: boots server, logs wallet address
  server.ts       — Server setup, composes all handlers
  convex.ts       — callConvex with retry/backoff + BYOK headers
  wallet.ts       — wallet creation, RPC helpers, signAndBroadcast
  types.ts        — shared ToolResult interface
  tools/
    market.ts     — 2 tools: get_market_data, get_token_data
    research.ts   — 1 tool: research
    insight.ts    — 2 tools: get_insight, ask_noel
    defi.ts       — 3 tools: swap_tokens, send_token, claim_fees
    automation.ts — 4 tools: create/list/pause/delete automation
    swarm.ts      — 6 tools: start/stop swarm, status, memory, scores
    framework.ts  — 6 tools: task packets, playbooks, ledger, sentinel
    vault.ts      — 7 tools: save, read, list, search, history, diff, export
    wallet.ts     — 2 tools: get_wallet_address, set_telegram
    twitter.ts    — 1 tool: post_tweet
    miroshark.ts  — 2 tools: miroshark_simulate, miroshark_status
    humanizer.ts  — 1 tool: humanize_text
```

**callConvex features:**
- Retry on 429/500/502/503/504 — 3 attempts, 500ms/1s/2s delays
- BYOK headers forwarded on every request: `X-User-Grok-Key`, `X-User-Bankr-Key`, `X-User-Telegram-Token`, `X-User-Telegram-Chat`
- Zod validation on all 37 tool inputs

---

## Security Model

- **API keys** — stored as Convex env vars, never in client bundle or MCP server
- **Convex URL** — hidden behind `api.noelclaw.com` Cloudflare proxy
- **Wallet keys (platform)** — AES-256-CBC encrypted before storing in DB
- **Wallet keys (MCP)** — stored locally at `~/.noelclaw/wallet.json`, encrypted with a machine-derived key, never leave the user's device
- **Auth** — Privy JWT tokens or Noelclaw session tokens validated per request; MCP uses wallet-native signatures
- **Agent safety** — Steward layer gates all swarm agent actions mechanically before execution
- **Payments** — x402 protocol, on-chain verification via Base mainnet
