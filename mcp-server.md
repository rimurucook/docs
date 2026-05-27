# MCP Server — Noelclaw Skill

The `@noelclaw/mcp` MCP server exposes all of Noel's tools to any MCP-compatible AI client. Install once via `npx` — no build step, no config required.

```bash
npx @noelclaw/mcp@latest
```

**37 tools across 12 modules.** Market data, DeFi execution, autonomous research, multi-agent swarm, persistent vault memory, Noel Framework playbooks, scenario simulation, and more.

---

## Tool Categories

### Market & Research

| Tool | Description |
|------|-------------|
| `get_market_data` | Live top-20 coins by market cap, trending, BTC/ETH/SOL prices |
| `get_token_data` | Price, 24h change, market cap, and volume for any specific token |
| `research` | On-demand web-search backed crypto analysis — overview, key findings, market impact, sentiment |
| `get_insight` | On-demand crypto + macro briefing powered by Grok — BTC/ETH action, narratives, what's moving on X |
| `ask_noel` | Chat with Noel — DeFi AI with live market context |

### Wallet & DeFi

| Tool | Description |
|------|-------------|
| `get_wallet_address` | Get your local Noelclaw wallet address — keys never leave your machine |
| `swap_tokens` | Swap ETH/USDC/USDT/DAI/WETH on Base via 0x Permit2, signed locally |
| `send_token` | Send ETH or ERC-20 to any address on Base mainnet |
| `claim_fees` | Claim accumulated ETH from Flaunch token swap fees |

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
| `vault_save` | Save or update an artifact with auto-versioning — research, code, notes, plans |
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
| `set_telegram` | Connect Telegram for push notifications — signals, alerts, swarm events |
| `post_tweet` | Post a tweet on X via Ayrshare API |

### Humanizer

| Tool | Description |
|------|-------------|
| `humanize_text` | Strip AI writing patterns — makes output sound natural and human |

---

## How It Works

```
AI Client (Claude / Cursor / Hermes / Windsurf)
    │
    │  MCP protocol (stdio)
    ▼
@noelclaw/mcp (Node.js via npx)
    │
    │  HTTPS — retries on 429/5xx
    ▼
https://api.noelclaw.com          ← Cloudflare Worker (rate limit + CORS)
    │
    │  proxied with all headers
    ▼
Convex backend (convex.site)
    │
    ├── /mcp/chat                → ask_noel
    ├── /mcp/insight             → get_insight
    ├── /mcp/research            → research
    ├── /mcp/defi/swap           → swap_tokens
    ├── /mcp/defi/send           → send_token
    ├── /mcp/token/claim         → claim_fees
    ├── /automations/*           → create/list/pause/delete
    ├── /swarm/*                 → swarm tools
    ├── /vault/*                 → vault tools
    ├── /framework/*             → task packets, playbooks, ledger
    └── /miroshark/*             → simulate, status
```

Market data (`get_market_data`, `get_token_data`) is pulled from CoinGecko via the swarm's shared memory. Wallet signing happens locally in the MCP process — Convex never holds your keys.

---

## Quick Install

### Claude Code

```bash
claude mcp add noelclaw -- npx @noelclaw/mcp@latest
```

### Claude Desktop

Edit `~/Library/Application Support/Claude/claude_desktop_config.json` (Mac) or `%APPDATA%\Claude\claude_desktop_config.json` (Windows):

```json
{
  "mcpServers": {
    "noelclaw": {
      "command": "npx",
      "args": ["@noelclaw/mcp@latest"]
    }
  }
}
```

### Cursor / Windsurf

```json
{
  "mcpServers": {
    "noelclaw": {
      "command": "npx",
      "args": ["@noelclaw/mcp@latest"]
    }
  }
}
```

### Hermes

```yaml
mcp_servers:
  noelclaw:
    command: npx
    args:
      - "@noelclaw/mcp@latest"
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

### `research`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `query` | string | yes | Topic to research, e.g. `"Ethereum ETF approval impact"` |

Returns structured analysis: overview, key findings, market impact, affected tokens, sentiment.

---

### `get_insight`

No parameters. Returns an on-demand crypto + macro briefing generated by Grok: BTC/ETH price action, macro events, and trending narratives on X.

---

### `ask_noel`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `question` | string | yes | Your question or prompt |
| `messages` | array | no | Prior conversation turns for context |

---

### `get_wallet_address`

No parameters. Returns your local Noelclaw wallet address. Keys are stored at `~/.noelclaw/wallet.json`, encrypted with a machine-derived key — they never leave your device.

---

### `swap_tokens`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `fromToken` | string | yes | Token to sell: `ETH`, `USDC`, `USDT`, `DAI`, `WETH` |
| `toToken` | string | yes | Token to buy |
| `amount` | string | yes | Human-readable amount, e.g. `"0.01"` for 0.01 ETH |

Routes through 0x Permit2 on Base mainnet. Transaction is signed locally — the MCP server signs it, not Convex.

---

### `send_token`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `token` | string | yes | `ETH`, `USDC`, `USDT`, `DAI`, or `WETH` |
| `toAddress` | string | yes | Recipient address (`0x...`) |
| `amount` | string | yes | Human-readable amount |

---

### `claim_fees`

No parameters. Calls `claim()` on the Flaunch PositionManager — pulls all pending ETH from your deployed tokens.

---

### `create_automation`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `rawInput` | string | yes | Plain English description of the automation |

Examples:
- `"Buy 50 USDC of ETH every day. Stop after spending 500 USDC."`
- `"If ETH drops 5% from current price, buy $100 of ETH"`
- `"Alert me when BTC price hits $120,000"`

---

### `list_automations`

No parameters. Returns all automations (active, paused, completed) with status, run counts, and next scheduled run.

---

### `pause_automation`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `automationId` | string | yes | Automation ID from `list_automations` |

Toggles between `active` and `paused`.

---

### `delete_automation`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `automationId` | string | yes | Automation ID from `list_automations` |

---

### `start_swarm`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `config.enabledAgents` | string[] | no | Agent IDs to start (default: all 5) |

Available agents: `market-monitor`, `sentiment-tracker`, `workflow-executor`, `memory-manager`, `risk-verifier`

---

### `stop_swarm`

No parameters.

---

### `get_swarm_status`

No parameters. Returns active session status, shared memory snapshot, and top execution scores.

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

No parameters. All skill scores sorted by performance: success rate, win/loss counts, avg duration, last adapted.

---

### `create_task_packet`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `name` | string | yes | Task name |
| `description` | string | yes | What the task does |
| `territory` | string[] | yes | Allowed action domains |
| `permissions` | string[] | yes | Allowed operations |
| `constraints` | object | no | Value limits, rate limits, etc. |

---

### `list_task_packets`

No parameters. Returns all task packets with status, usage counts, and Sentinel outcomes.

---

### `list_playbooks`

No parameters. Returns available Noel Framework playbooks with step counts and last run.

---

### `run_playbook`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `playbookId` | string | yes | Playbook ID from `list_playbooks` |
| `context` | object | no | Runtime variables for the playbook |

Each step passes through Sentinel gating before execution. Blocked steps halt the run and log to the ledger.

---

### `get_noel_ledger`

No parameters. Returns the Sentinel audit trail — every gate decision with check type, duration, and reason.

---

### `get_sentinel_rules`

No parameters. Shows Sentinel rules per agent and role: territory, allowed permissions, value caps, rate limits.

---

### `vault_save`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `key` | string | yes | Unique key for this artifact |
| `content` | string | yes | Content to store |
| `type` | string | no | `research`, `code`, `note`, `plan`, `signal`, etc. |
| `tags` | string[] | no | Tags for filtering and search |
| `links` | string[] | no | Related vault keys |

Auto-versions on every update — previous versions are preserved and diffable.

---

### `vault_read`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `key` | string | yes | Vault key to read |
| `version` | number | no | Specific version number (default: latest) |

---

### `vault_list`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `type` | string | no | Filter by type |
| `pinned` | boolean | no | Only pinned entries |
| `agent` | string | no | Filter by agent that wrote it |

---

### `vault_search`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `query` | string | yes | Search query — full-text across all vault entries |

Returns ranked results with content previews.

---

### `vault_history`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `key` | string | yes | Vault key |

Returns all versions with timestamps, change summaries, and who wrote each.

---

### `vault_diff`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `key` | string | yes | Vault key |
| `fromVersion` | number | yes | Earlier version |
| `toVersion` | number | yes | Later version |

Returns added/removed lines (unified diff format).

---

### `vault_export`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `type` | string | no | Export only this type (omit for full export) |

Returns a structured bundle of all matching entries with metadata.

---

### `miroshark_simulate`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `scenario` | string | yes | What to simulate in plain English |
| `num_agents` | number | no | Number of agents (default: 20, max: 100) |
| `num_rounds` | number | no | Simulation rounds (default: 50) |

Runs a full multi-agent social simulation: builds a knowledge graph from your scenario, generates agent personas, runs belief propagation rounds, and returns behavioral analysis.

---

### `miroshark_status`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `simulation_id` | string | yes | Simulation ID from `miroshark_simulate` |

Poll this to track progress through: `pending → preparing → running → complete`.

---

### `set_telegram`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `telegramBotToken` | string | yes | Bot token from @BotFather |
| `telegramChatId` | string | yes | Your chat ID |

Once set, you receive swarm events, automation alerts, and platform notifications directly in Telegram.

---

### `post_tweet`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `content` | string | yes | Tweet text |

Posts via Ayrshare API. Requires `AYRSHARE_API_KEY` environment variable.

---

### `humanize_text`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `text` | string | yes | AI-generated text to humanize |

Strips AI writing patterns (em dashes, sycophantic openers, robotic structure) using MiniMax-M2.7. Output sounds natural and human.

---

## Environment Variables

No env vars required for basic use. Set these to unlock additional features:

| Var | Purpose |
|-----|---------|
| `NOELCLAW_API_KEY` | Link to your noelclaw.com account (`noel_sk_xxx` from Settings → API Keys) |
| `ALCHEMY_API_KEY` | Faster swap quotes and Base balance lookups |
| `GROK_API_KEY` | Your own X.AI key for `get_insight` |
| `BANKR_API_KEY` | Your own Bankr key for research and swarm agents |
| `TELEGRAM_BOT_TOKEN` | Your Telegram bot token (from @BotFather) |
| `TELEGRAM_CHAT_ID` | Your Telegram chat ID |
| `MINIMAX_API_KEY` | Your own MiniMax key for `humanize_text` |
| `AYRSHARE_API_KEY` | Required for `post_tweet` |

---

## Troubleshooting

| Error | Fix |
|-------|-----|
| Tools not appearing | Restart your MCP client after adding the config |
| Server starts but no response | Normal — it waits for MCP stdin, not HTTP |
| Research times out | Try again — Bankr LLM gateway may be under load |
| Swap or send fails | Check your ETH balance and that Base mainnet is reachable |
| `get_swarm_status` returns empty | Start the swarm first with `start_swarm` |
| Rate limit (429) | The server retries automatically up to 3 times with backoff |
| `humanize_text` returns raw `<think>` | Outdated version — run `npx @noelclaw/mcp@latest` to upgrade |
