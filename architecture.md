# Architecture Overview

## Infrastructure Components

| Component | What It Is | What It Does |
|---|---|---|
| **Cloudflare Worker** | `api.noelclaw.com` | Rate limiting, CORS, header proxy |
| **Convex** | Main backend | DB, serverless actions, cron scheduling, real-time |
| **Supabase Edge Functions** | Supabase-hosted functions | Swarm (`/swarm/*`) and vault (`/vault/*`) |
| **Railway** | Hobby plan, $5/mo, 8 GB RAM | MiroShark multi-agent simulation backend |

---

## Full System Diagram

```
  AI Clients
  ┌───────────────────────────────────────────────────┐
  │  Claude Code / Claude Desktop / Cursor / Windsurf │
  │  Hermes / Aeon                                    │
  └────────────────────┬──────────────────────────────┘
                       │ MCP protocol (stdio)
                       ▼
            ┌─────────────────────┐
            │   @noelclaw/mcp     │
            │   (Node.js, npx)    │
            │   35 tools          │
            └──────────┬──────────┘
                       │ HTTPS (retries on 429/5xx)
                       ▼
  ┌────────────────────────────────────────────────────┐
  │        Cloudflare Worker — api.noelclaw.com        │
  │  Rate limit: 100 req/min/IP  |  CORS  |  Headers  │
  └────────────────────┬───────────────────────────────┘
                       │
          ┌────────────┴─────────────┐
          │                          │
          ▼                          ▼
  ┌───────────────────┐    ┌──────────────────────┐
  │  Convex Backend   │    │ Supabase Edge Funcs   │
  │                   │    │                       │
  │  - Queries        │    │  /swarm/*             │
  │  - Mutations      │    │  /vault/*             │
  │  - Actions        │    │                       │
  │  - Cron jobs      │    └──────────────────────┘
  │  - HTTP router    │
  │                   │
  │  Routes:          │
  │  /mcp/chat        │
  │  /mcp/defi/*      │
  │  /automations/*   │
  │  /framework/*     │
  │  /miroshark/*     │
  └───────────────────┘
          │
          ▼
  ┌───────────────────────────────────────────────────┐
  │              External Services                    │
  │  0x Permit2 API      — swap_tokens               │
  │  Telegram Bot API    — set_telegram              │
  │  Ayrshare API        — post_tweet                │
  │  MiniMax M2.7        — humanize_text             │
  └───────────────────────────────────────────────────┘

  ┌───────────────────────────────────────────────────┐
  │         Railway — MiroShark Backend               │
  │  Hobby plan, $5/mo, 8 GB RAM                      │
  │  miroshark_simulate → miroshark_status            │
  │  Knowledge graph, agent personas, belief prop     │
  └───────────────────────────────────────────────────┘
```

---

## Cloudflare Worker

`api.noelclaw.com` is a Cloudflare Worker that proxies all MCP and platform API traffic to Convex.

- **Rate limiting:** 100 requests/min per IP using KV-based fixed-window counters
- **CORS:** correct headers returned for browser clients
- **Header forwarding:** passes all `X-User-*` BYOK headers through to Convex (Grok key, Bankr key, Telegram token/chat ID)
- **Error handling:** 503 if Convex is unreachable, 429 with `Retry-After` header on rate limit
- **Routes:** `/miroshark/*` proxied with prefix stripped and admin token injected

---

## Convex Backend

Convex is the main backend — database, serverless functions, real-time subscriptions, and HTTP router.

| Layer | Details |
|---|---|
| Database | Document DB with real-time subscriptions |
| Serverless | Actions (`"use node"`) for external API calls |
| Scheduling | Cron jobs: automations, swarm heartbeat (5 min), scheduled tasks |
| HTTP | `convex.site` endpoints for MCP, proxied via `api.noelclaw.com` |
| Auth | Session token validation per request |

---

## Supabase Edge Functions

Supabase hosts the swarm and vault backends.

- `/swarm/*` — all swarm tool operations (start, stop, status, memory read/write, execution scores)
- `/vault/*` — all vault operations (save, read, list, search, history, diff, export)

Traffic flows: MCP client → `@noelclaw/mcp` → `api.noelclaw.com` (Cloudflare) → Supabase Edge Functions.

---

## Railway — MiroShark

MiroShark runs on a Railway Hobby plan ($5/mo, 8 GB RAM). It is a multi-agent social simulation engine.

- Builds a knowledge graph from the scenario description
- Generates agent personas with initial belief states
- Runs belief propagation across N rounds
- Returns behavioral analysis: consensus, dissent, signal strength per narrative

The `/miroshark/*` proxy on `api.noelclaw.com` strips the prefix and injects the admin token before forwarding to the Railway service.

---

## Data Flow: MCP Tool Call

```
User prompts AI client
      │
      ▼ MCP protocol (stdio)
@noelclaw/mcp — validates input with Zod
      │
      ▼ HTTPS POST
api.noelclaw.com (Cloudflare Worker)
  → rate limit check
  → CORS headers
  → forward BYOK headers
      │
      ▼
Convex / Supabase / Railway (depending on tool)
      │
      ▼
Response returned to MCP server
      │
      ▼ MCP protocol
AI client renders result
```

---

## Data Flow: Swarm Execution

```
start_swarm called
      │
      ▼
POST /swarm/start (Supabase Edge Function)
  → creates session record
  → initializes 5 agent contexts:
      market-monitor, sentiment-tracker,
      workflow-executor, memory-manager, risk-verifier
  → each agent reads/writes shared memory via KV
      │
      ▼
get_swarm_status polls session state
write_swarm_memory / get_swarm_memory operate on shared KV
get_execution_scores returns per-skill metrics
      │
      ▼
stop_swarm terminates session
```

---

## Data Flow: MiroShark Simulation

```
miroshark_simulate called with scenario string
      │
      ▼
POST /miroshark/simulate (Railway backend)
  → returns simulation_id immediately
  → async pipeline starts:
      1. Build knowledge graph from scenario
      2. Generate N agent personas
      3. Initialize belief states
      4. Run M rounds of belief propagation
      5. Compute consensus metrics
      │
      ▼
miroshark_status polls with simulation_id
  → states: pending → preparing → running → complete
  → complete response includes full analysis
```

---

## Data Flow: DeFi Wallet (MCP)

```
swap_tokens / send_token called
      │
      ▼
@noelclaw/mcp:
  swap_tokens:
    POST api.noelclaw.com/mcp/defi/swap
    → Convex fetches 0x Permit2 quote
    → returns quote + tx params to MCP server
    → MCP server signs tx locally (ethers.js)
    → MCP server broadcasts to Base mainnet

  send_token:
    POST api.noelclaw.com/mcp/defi/send
    → Convex builds tx params
    → MCP server signs + broadcasts locally
```

Keys at `~/.noelclaw/wallet.json` never leave the local machine. Convex never holds or sees private keys for MCP users.

---


## Sentinel / Noel Framework

Every action in the Noel Framework passes through the Sentinel gate before execution.

```
Action requested
      │
      ▼
Sentinel checks (in order):
  1. DoNotDo list — blocked keywords per agent
  2. Territory check — action must be in agent's allowed domain
  3. Value limit — reject if USD value exceeds per-agent cap
  4. Grudge book — block repeated offenders (per-user, per-agent)
  5. Rate limit — max N actions per agent per 60s window
      │
      ▼
Decision: approved | warned | blocked
  blocked → error thrown, logged to ledger
  approved/warned → execution continues
```

Ledger accessible via `get_noel_ledger`. Rules accessible via `get_sentinel_rules`.

---

## Security Model

| Concern | How It's Handled |
|---|---|
| Convex URL hidden | All traffic goes through `api.noelclaw.com` — raw Convex URL is never exposed |
| Rate limiting | 100 req/min/IP at the Cloudflare layer |
| Wallet keys (MCP) | Stored locally at `~/.noelclaw/wallet.json`, encrypted with a machine-derived key, never leave the device |
| API keys | Stored as server-side env vars, never in the MCP package or client bundle |
| Agent actions | Sentinel gates all Framework actions before execution |
