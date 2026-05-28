# MCP Server Reference

`@noelclaw/mcp` is an MCP server that exposes all of Noelclaw's tools to any MCP-compatible AI client. Install once via `npx` — no build step, no config required.

**34 tools across 8 categories.** Market data, DeFi execution, multi-agent swarm, persistent vault, Noel Framework playbooks, MiroShark simulation, social, and humanizer.

---

## Tool Categories

### Market & Intel

| Tool | Description |
|------|-------------|
| `get_market_data` | Live top-20 coins by market cap, trending, BTC/ETH/SOL prices |
| `get_token_data` | Price, 24h change, market cap, and volume for any specific token |
| `ask_noel` | Chat with Noel — DeFi AI with live market context |

### Wallet & DeFi

| Tool | Description |
|------|-------------|
| `get_wallet_address` | Your local Noelclaw wallet address — keys never leave your machine |
| `swap_tokens` | Swap ETH/USDC/USDT/DAI/WETH on Base via 0x Permit2, signed locally |
| `send_token` | Send ETH or ERC-20 to any address on Base mainnet |

### Automations

| Tool | Description |
|------|-------------|
| `create_automation` | Create DCA, price alert, or conditional buy/sell in plain English |
| `list_automations` | All automations with status, run counts, and next run time |
| `pause_automation` | Pause or resume an automation by ID |
| `delete_automation` | Permanently delete an automation |

### Swarm

| Tool | Description |
|------|-------------|
| `start_swarm` | Start the multi-agent swarm — 5 coordinated agents |
| `stop_swarm` | Stop the active swarm session |
| `get_swarm_status` | Active agents, shared memory snapshot, execution scores, recent runs |
| `write_swarm_memory` | Write a key-value entry to shared memory (optional TTL) |
| `get_swarm_memory` | Read a shared memory entry by key |
| `get_execution_scores` | Skill success rates, win/loss, avg duration, last adapted |

### Noel Framework

| Tool | Description |
|------|-------------|
| `create_task_packet` | Define a scoped task with territory, permissions, and constraints |
| `list_task_packets` | List all task packets — draft, active, completed, blocked |
| `list_playbooks` | List available playbooks with step counts and usage |
| `run_playbook` | Execute a playbook with Sentinel gating per step |
| `get_noel_ledger` | Audit trail of Sentinel gate decisions — checks, duration, reason |
| `get_sentinel_rules` | Sentinel rules per agent/role — territory, permissions, caps |

### Noel Vault

| Tool | Description |
|------|-------------|
| `vault_save` | Save or update an artifact with auto-versioning — notes, code, plans |
| `vault_read` | Read a vault entry by key — full content, version, tags, links |
| `vault_list` | List vault entries filtered by type, agent, or pinned status |
| `vault_search` | Full-text search across the vault with ranking and previews |
| `vault_history` | Full version history of a vault entry (git log style) |
| `vault_diff` | Compare two versions of a vault entry (git diff style) |
| `vault_export` | Export entire vault or specific type as a structured bundle |

### MiroShark

| Tool | Description |
|------|-------------|
| `miroshark_simulate` | Run a multi-agent social simulation for any scenario in plain English |
| `miroshark_status` | Poll simulation status through prep, running, and completion phases |

### Notifications & Social

| Tool | Description |
|------|-------------|
| `set_telegram` | Connect Telegram for swarm events and automation alerts |
| `post_tweet` | Post a tweet on X via Ayrshare API |

### Humanizer

| Tool | Description |
|------|-------------|
| `humanize_text` | Strip AI writing patterns — makes output sound natural and human |

---

## How It Works

```
AI Client (Claude / Cursor / Hermes / Windsurf / Aeon)
    │
    │  MCP protocol (stdio)
    ▼
@noelclaw/mcp (Node.js via npx)
    │
    │  HTTPS — retries on 429/5xx
    ▼
https://api.noelclaw.com          ← Cloudflare Worker (rate limit + CORS)
    │
    ▼
Convex backend
    │
    ├── /mcp/chat                → ask_noel
    ├── /mcp/defi/swap           → swap_tokens
    ├── /mcp/defi/send           → send_token
    ├── /automations/*           → create/list/pause/delete
    ├── /swarm/*                 → swarm tools (via Supabase)
    ├── /vault/*                 → vault tools (via Supabase)
    ├── /framework/*             → task packets, playbooks, ledger
    └── /miroshark/*             → simulate, status (via Railway)
```

Market data is fetched from CoinGecko. Wallet signing happens locally — Convex never holds your keys.

---

## Install

### Claude Code

```bash
claude mcp add noelclaw -- npx @noelclaw/mcp
```

### Claude Desktop

Mac: `~/Library/Application Support/Claude/claude_desktop_config.json`
Windows: `%APPDATA%\Claude\claude_desktop_config.json`

```json
{
  "mcpServers": {
    "noelclaw": {
      "command": "npx",
      "args": ["@noelclaw/mcp"]
    }
  }
}
```

### Cursor

Edit `~/.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "noelclaw": {
      "command": "npx",
      "args": ["@noelclaw/mcp"]
    }
  }
}
```

### Windsurf

Edit `~/.windsurf/mcp_config.json` — same JSON format as above.

### Hermes

```yaml
mcp_servers:
  noelclaw:
    command: npx
    args:
      - "@noelclaw/mcp"
```

---

## Tool Reference

### `get_market_data`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `token` | string | no | Focus on a specific token, e.g. `"BTC"` |

---

### `get_token_data`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `question` | string | yes | Token name or query, e.g. `"show me ETH and SOL"` |

---

### `ask_noel`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `question` | string | yes | Your question or prompt |
| `messages` | array | no | Prior conversation turns for context |

---

### `get_wallet_address`

No parameters. Returns your local Noelclaw wallet address. Keys are stored at `~/.noelclaw/wallet.json` and never leave your device.

---

### `swap_tokens`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `fromToken` | string | yes | Token to sell: `ETH`, `USDC`, `USDT`, `DAI`, `WETH` |
| `toToken` | string | yes | Token to buy |
| `amount` | string | yes | Human-readable amount or percentage, e.g. `"0.01"` or `"50%"` |

Routes through 0x Permit2 on Base mainnet. Signed locally — not by Convex.

---

### `send_token`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `token` | string | yes | `ETH`, `USDC`, `USDT`, `DAI`, or `WETH` |
| `toAddress` | string | yes | Recipient address (`0x...`) |
| `amount` | string | yes | Human-readable amount |

---

### `create_automation`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `rawInput` | string | yes | Plain English description |

Examples:
- `"Buy 50 USDC of ETH every day"`
- `"Alert me when BTC hits $120,000"`
- `"If ETH drops 5%, buy $100"`

---

### `list_automations`

No parameters. Returns all automations with status, run counts, and next scheduled run.

---

### `pause_automation`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `automationId` | string | yes | ID from `list_automations` |

---

### `delete_automation`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `automationId` | string | yes | ID from `list_automations` |

---

### `start_swarm`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `config.enabledAgents` | string[] | no | Agent IDs (default: all 5) |

Available agents: `market-monitor`, `sentiment-tracker`, `workflow-executor`, `memory-manager`, `risk-verifier`

---

### `stop_swarm`

No parameters.

---

### `get_swarm_status`

No parameters. Returns active session, shared memory snapshot, and top execution scores.

---

### `write_swarm_memory`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `agentId` | string | yes | ID of the writing agent |
| `key` | string | yes | Memory key |
| `value` | string | yes | Value (JSON-serializable string) |
| `ttlSeconds` | number | no | Auto-delete after N seconds |

---

### `get_swarm_memory`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `key` | string | yes | Memory key to read |

---

### `get_execution_scores`

No parameters. All skill scores: success rate, win/loss counts, avg duration, last adapted.

---

### `create_task_packet`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `name` | string | yes | Task name |
| `description` | string | yes | What the task does |
| `territory` | string[] | yes | Allowed action domains |
| `permissions` | string[] | yes | Allowed operations |
| `constraints` | object | no | Value limits, rate limits |

---

### `list_task_packets`

No parameters. Returns all task packets with status and Sentinel outcomes.

---

### `list_playbooks`

No parameters. Returns available playbooks with step counts and last run.

---

### `run_playbook`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `playbookId` | string | yes | ID from `list_playbooks` |
| `context` | object | no | Runtime variables |

Each step passes through Sentinel gating. Blocked steps halt the run and log to the ledger.

---

### `get_noel_ledger`

No parameters. Sentinel audit trail — every gate decision with check type, duration, and reason.

---

### `get_sentinel_rules`

No parameters. Sentinel rules per agent and role: territory, permissions, value caps, rate limits.

---

### `vault_save`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `type` | string | yes | `research`, `execution`, `workflow`, `prompt`, `file`, `memory` |
| `title` | string | yes | Human-readable title |
| `content` | string | yes | Content — markdown, JSON, code, or plain text |
| `key` | string | no | Slug key, e.g. `research/btc-analysis` (auto-generated if omitted) |
| `contentType` | string | no | `markdown`, `json`, `text`, `code` |
| `agentId` | string | no | Agent ID writing this entry |
| `tags` | string[] | no | Tags for filtering |
| `commitMsg` | string | no | Version message, e.g. `"initial draft"` |

Auto-versions on every update — all previous versions are preserved.

---

### `vault_read`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `key` | string | yes | Vault key |

---

### `vault_list`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `type` | string | no | Filter by type |
| `pinned` | boolean | no | Only pinned entries |
| `agentId` | string | no | Filter by writing agent |
| `limit` | number | no | Max entries (default 50) |

---

### `vault_search`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `query` | string | yes | Full-text search query |
| `type` | string | no | Filter by type |
| `limit` | number | no | Max results (default 20) |

---

### `vault_history`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `key` | string | yes | Vault key |

---

### `vault_diff`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `key` | string | yes | Vault key |
| `fromVersion` | number | yes | Earlier version |
| `toVersion` | number | yes | Later version |

---

### `vault_export`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `type` | string | no | Export only this type (omit for full export) |

---

### `miroshark_simulate`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `scenario` | string | yes | What to simulate in plain English |
| `num_agents` | number | no | Number of agents (default: 20, max: 100) |
| `num_rounds` | number | no | Simulation rounds (default: 50) |

Builds a knowledge graph, generates personas, runs belief propagation, returns behavioral analysis.

---

### `miroshark_status`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `simulation_id` | string | yes | ID from `miroshark_simulate` |

Poll through: `pending → preparing → running → complete`.

---

### `set_telegram`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `telegramBotToken` | string | yes | Bot token from @BotFather |
| `telegramChatId` | string | yes | Your chat ID |

---

### `post_tweet`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `content` | string | yes | Tweet text |

Requires `AYRSHARE_API_KEY` env var.

---

### `humanize_text`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `text` | string | yes | AI-generated text to rewrite |

Strips AI patterns using MiniMax-M2.7. Requires `MINIMAX_API_KEY` env var.

---

## Environment Variables

| Var | Purpose |
|-----|---------|
| `NOELCLAW_API_KEY` | Link to your noelclaw.com account |
| `ALCHEMY_API_KEY` | Faster swap quotes and Base balance lookups |
| `BANKR_API_KEY` | Your own Bankr key for swarm agents (BYOK) |
| `TELEGRAM_BOT_TOKEN` | Telegram bot token (from @BotFather) |
| `TELEGRAM_CHAT_ID` | Your Telegram chat ID |
| `MINIMAX_API_KEY` | Required for `humanize_text` |
| `AYRSHARE_API_KEY` | Required for `post_tweet` |

---

## Troubleshooting

| Error | Fix |
|-------|-----|
| Tools not appearing | Restart your MCP client |
| Server starts but no response | Normal — waits for MCP stdin, not HTTP |
| Swap fails | Check ETH balance and Base mainnet connectivity |
| `get_swarm_status` empty | Start swarm first with `start_swarm` |
| Rate limit (429) | Auto-retries up to 3 times with backoff |
| `humanize_text` fails | Set `MINIMAX_API_KEY` in env |
| `post_tweet` fails | Set `AYRSHARE_API_KEY` in env |
