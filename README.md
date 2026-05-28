# Noelclaw

**Noelclaw** is a crypto AI platform with a 34-tool MCP skill (`@noelclaw/mcp`) that plugs directly into Claude, Cursor, Windsurf, Hermes, and Aeon — giving AI agents live market data, DeFi execution, multi-agent swarms, persistent vault memory, and social simulation, all from natural language.

- Platform: [noelclaw.com](https://noelclaw.com)
- Docs: [docs.noelclaw.fun](https://docs.noelclaw.fun)
- npm: [@noelclaw/mcp](https://www.npmjs.com/package/@noelclaw/mcp)

---

## What You Get

| Layer | Description |
|---|---|
| **MCP Skill** | 34 tools — market intel, wallet/DeFi, automations, swarm, vault, framework, MiroShark, social |
| **Platform** | Web UI at [noelclaw.com](https://noelclaw.com) — AI agents, automations, wallet |
| **Aeon** | All 34 tools available in the Aeon/Hermes ecosystem |
| **MiroShark** | Multi-agent social simulation on Railway |

---

## Install

One command, no setup:

```bash
# Claude Code
claude mcp add noelclaw -- npx @noelclaw/mcp

# Claude Desktop / Cursor / Windsurf — add to MCP config
{
  "mcpServers": {
    "noelclaw": {
      "command": "npx",
      "args": ["@noelclaw/mcp"]
    }
  }
}
```

→ [Full setup guide for all clients](setup.md)

---

## 34 Tools

### Market & Intel

| Tool | Description |
|---|---|
| `get_market_data` | Live top-20 coins, BTC/ETH/SOL prices, trending |
| `get_token_data` | Price, 24h change, market cap, volume for any token |
| `ask_noel` | Ask Noel AI — DeFi analysis, trade ideas, market outlook with live context |

### Wallet & DeFi

| Tool | Description |
|---|---|
| `get_wallet_address` | Your local wallet address — keys never leave your machine |
| `swap_tokens` | Swap ETH/USDC/USDT/DAI/WETH on Base via 0x Permit2, signed locally |
| `send_token` | Send ETH or any ERC-20 on Base mainnet |

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
| `start_swarm` | Start 5 coordinated AI agents |
| `stop_swarm` | Stop the active swarm session |
| `get_swarm_status` | Active agents, shared memory, execution scores |
| `write_swarm_memory` | Write key-value to shared memory (optional TTL) |
| `get_swarm_memory` | Read shared memory by key |
| `get_execution_scores` | Skill success rates, win/loss, avg duration |

### Noel Framework

| Tool | Description |
|---|---|
| `create_task_packet` | Define a scoped task with territory, permissions, constraints |
| `list_task_packets` | List all task packets — draft, active, completed, blocked |
| `list_playbooks` | Available playbooks with step counts |
| `run_playbook` | Execute a playbook with Sentinel gating per step |
| `get_noel_ledger` | Audit trail of every Sentinel gate decision |
| `get_sentinel_rules` | Sentinel rules per agent — territory, permissions, caps |

### Noel Vault

| Tool | Description |
|---|---|
| `vault_save` | Save or update an artifact with auto-versioning |
| `vault_read` | Read entry by key (specific version optional) |
| `vault_list` | List entries filtered by type, agent, or pinned |
| `vault_search` | Full-text search across the vault |
| `vault_history` | Full version history — git log style |
| `vault_diff` | Compare two versions — git diff style |
| `vault_export` | Export vault or specific type as a structured bundle |

### MiroShark

| Tool | Description |
|---|---|
| `miroshark_simulate` | Run a multi-agent social simulation for any scenario |
| `miroshark_status` | Poll simulation status (pending → preparing → running → complete) |

### Social & Other

| Tool | Description |
|---|---|
| `set_telegram` | Connect Telegram for swarm events and automation alerts |
| `post_tweet` | Post a tweet on X via Ayrshare API |
| `humanize_text` | Strip AI writing patterns — makes output sound natural |

---

## Infrastructure

| Component | Role |
|---|---|
| **Cloudflare Worker** (`api.noelclaw.com`) | Rate limiting, CORS, header proxy |
| **Convex** | Main backend — DB, serverless functions, scheduling, real-time |
| **Supabase Edge Functions** | Swarm (`/swarm/*`) and vault (`/vault/*`) |
| **Railway** (8 GB RAM) | MiroShark simulation backend |

---

## Key Numbers

- 34 MCP tools across 8 categories
- 5 coordinated swarm agents per session
- 7 vault operations with full version history
- Wallet keys never leave your machine (`~/.noelclaw/wallet.json`)
