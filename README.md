# Noelclaw

**Noelclaw** is a crypto intelligence platform built for on-chain action. It exposes a 36-tool MCP skill (`@noelclaw/mcp`) that plugs directly into Claude, Cursor, Windsurf, Hermes, and Aeon — giving AI agents live market data, wallet operations, DeFi execution, multi-agent swarms, a persistent vault, and social simulation, all from natural language.

Platform: [noelclaw.com](https://noelclaw.com)
Docs: [docs.noelclaw.fun](https://docs.noelclaw.fun)

---

## What It Does

| Layer | What You Get |
|---|---|
| **MCP Skill** | 36 tools across market intel, wallet/DeFi, automations, swarms, vault, framework, MiroShark, social |
| **Platform** | Web UI at noelclaw.com — manage automations, view signals, monitor wallet activity |
| **Trading Signals** | BTC/ETH 4H signals daily at 08:00 UTC with A+ scoring, delivered to Telegram |
| **Smart Money Alerts** | Hourly micro-cap tracking on Base (<$100k mcap) via DexScreener, Telegram delivery |

---

## MCP Skill — 36 Tools

Install: `npx @noelclaw/mcp`

### Market & Intel

| Tool | Description |
|---|---|
| `get_market_data` | Live top-20 coins, BTC/ETH/SOL prices, trending |
| `get_token_data` | Price, 24h change, market cap, volume for any token |
| `get_insight` | On-demand crypto + macro briefing via Grok — BTC/ETH action, narratives, what's moving on X |
| `ask_noel` | Chat with Noel AI for DeFi analysis with live market context |

### Wallet & DeFi

| Tool | Description |
|---|---|
| `get_wallet_address` | Returns local wallet address (keys never leave machine, stored at `~/.noelclaw/wallet.json`) |
| `swap_tokens` | Swap ETH/USDC/USDT/DAI/WETH on Base via 0x Permit2, signed locally |
| `send_token` | Send ETH or ERC-20 on Base mainnet |
| `claim_fees` | Claim pending ETH from Flaunch token swap fees |

### Automations

| Tool | Description |
|---|---|
| `create_automation` | DCA, price alerts, conditional buys/sells in plain English |
| `list_automations` | All automations with status, run counts, next run |
| `pause_automation` | Pause or resume by ID |
| `delete_automation` | Delete permanently |

### Swarm

| Tool | Description |
|---|---|
| `start_swarm` | Start 5 coordinated agents: market-monitor, sentiment-tracker, workflow-executor, memory-manager, risk-verifier |
| `stop_swarm` | Stop active swarm session |
| `get_swarm_status` | Active agents, shared memory snapshot, execution scores |
| `write_swarm_memory` | Write key-value to shared memory (optional TTL) |
| `get_swarm_memory` | Read shared memory by key |
| `get_execution_scores` | Skill success rates, win/loss, avg duration |

### Noel Framework

| Tool | Description |
|---|---|
| `create_task_packet` | Define scoped task with territory, permissions, constraints |
| `list_task_packets` | List all packets (draft, active, completed, blocked) |
| `list_playbooks` | Available playbooks with step counts |
| `run_playbook` | Execute playbook with Sentinel gating per step |
| `get_noel_ledger` | Audit trail of Sentinel gate decisions |
| `get_sentinel_rules` | Rules per agent: territory, permissions, value caps, rate limits |

### Noel Vault

| Tool | Description |
|---|---|
| `vault_save` | Save/update artifact with auto-versioning |
| `vault_read` | Read entry by key (specific version optional) |
| `vault_list` | List entries filtered by type/agent/pinned |
| `vault_search` | Full-text search with ranking and previews |
| `vault_history` | Version history (git log style) |
| `vault_diff` | Compare two versions (git diff style) |
| `vault_export` | Export vault or specific type as structured bundle |

### MiroShark

| Tool | Description |
|---|---|
| `miroshark_simulate` | Run multi-agent social simulation for any scenario in plain English |
| `miroshark_status` | Poll simulation status (pending → preparing → running → complete) |

### Social & Notifications

| Tool | Description |
|---|---|
| `set_telegram` | Connect Telegram for push notifications (signals, alerts, swarm events) |
| `post_tweet` | Post a tweet on X via Ayrshare API (requires `AYRSHARE_API_KEY`) |

### Humanizer

| Tool | Description |
|---|---|
| `humanize_text` | Strip AI writing patterns using MiniMax-M2.7 (requires `MINIMAX_API_KEY`) |

---

## Integrations

**Aeon** — The noelclaw skill pack is live in the Aeon ecosystem (merged PR #253). Add via Aeon's skill registry and all 36 tools are available in the Hermes/Aeon runtime.

**MiroShark** — Multi-agent social simulation backend running on Railway. Supports belief propagation across agent personas, knowledge graphs, and scenario modeling. Accessible via `miroshark_simulate` and `miroshark_status`.

---

## Infrastructure

| Component | Role |
|---|---|
| **Cloudflare Worker** (`api.noelclaw.com`) | Rate limiting (100 req/min/IP), CORS, header proxy to Convex |
| **Convex** | Main backend — DB, serverless actions, cron scheduling, real-time subscriptions |
| **Supabase Edge Functions** | Powers swarm (`/swarm/*`) and vault (`/vault/*`) |
| **Railway** (Hobby, $5/mo, 8 GB RAM) | Runs MiroShark multi-agent simulation backend |

---

## Key Numbers

- 36 MCP tools across 8 categories
- 5 coordinated swarm agents per session
- 7 vault operations (save, read, list, search, history, diff, export)
- 100 req/min/IP rate limit via Cloudflare
- BTC/ETH signals delivered at 08:00 UTC daily
- Hourly Base micro-cap alert scanning (<$100k mcap)
- Wallet keys never leave your machine (`~/.noelclaw/wallet.json`)
