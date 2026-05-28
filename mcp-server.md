# MCP Server Reference

`@noelclaw/mcp` is an MCP server that exposes all Noelclaw tools to any MCP-compatible AI client. Install once via `npx` — no build step, no config required.

**35 tools across 8 categories.** Market data, DeFi execution, multi-agent swarm, persistent vault, Noel Framework playbooks, MiroShark simulation, social notifications, and humanizer.

---

## Tool Categories

### Market & Intel

| Tool | Description |
|------|-------------|
| `get_market_data` | Live top-20 coins by market cap, trending, BTC/ETH/SOL key prices — from CoinGecko, no API key needed |
| `get_token_data` | Price, 24h change, market cap, volume, and ATH for any specific token |
| `ask_noel` | Chat with Noel — crypto AI with live market context |

### Wallet & DeFi

| Tool | Description |
|------|-------------|
| `get_wallet_address` | Your local Noelclaw wallet address — keys stored at `~/.noelclaw/wallet.json`, never leave your machine |
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
| `start_swarm` | Start the multi-agent swarm — auto-loads live BTC/ETH/SOL prices into shared memory |
| `stop_swarm` | Stop the active swarm session |
| `get_swarm_status` | Active session, shared memory snapshot, and execution scores |
| `write_swarm_memory` | Write a key-value entry to shared memory (optional TTL) |
| `get_swarm_memory` | Read a shared memory entry by key |
| `get_execution_scores` | Skill success rates, win/loss counts, avg duration |

### Noel Framework

| Tool | Description |
|------|-------------|
| `create_task_packet` | Define a scoped task with territory, permissions, and constraints |
| `list_task_packets` | List all task packets — draft, active, completed, blocked |
| `list_playbooks` | Available playbooks with step counts and usage |
| `run_playbook` | Execute a playbook with Sentinel gating per step |
| `get_noel_ledger` | Sentinel audit trail — every gate decision with check type, duration, reason |
| `get_sentinel_rules` | Sentinel rules per agent/role — territory, permissions, caps |

### Noel Vault

| Tool | Description |
|------|-------------|
| `vault_save` | Save or update an artifact with auto-versioning |
| `vault_read` | Read a vault entry by key |
| `vault_list` | List vault entries filtered by type, agent, or pinned status |
| `vault_search` | Full-text search across the vault |
| `vault_history` | Full version history of a vault entry (git log style) |
| `vault_diff` | Compare two versions of a vault entry (git diff style) |
| `vault_export` | Export entire vault or specific type as a structured bundle |

### MiroShark

| Tool | Description |
|------|-------------|
| `miroshark_simulate` | Run a multi-agent social simulation for any scenario in plain English |
| `miroshark_status` | Poll simulation status — prep, running, and completion |
| `miroshark_stop` | Stop a running simulation |

### Notifications & Social

| Tool | Description |
|------|-------------|
| `set_telegram` | Connect Telegram for swarm events and automation alerts |
| `post_tweet` | Post to X via Ayrshare API (requires `AYRSHARE_API_KEY`) |

### Humanizer

| Tool | Description |
|------|-------------|
| `humanize_text` | Strip AI writing patterns — makes output sound natural (requires `MINIMAX_API_KEY`) |

---

## How It Works

```
AI Client (Claude / Cursor / Hermes / Windsurf / Aeon)
    │
    │  MCP protocol (stdio)
    ▼
@noelclaw/mcp (Node.js)
    │  Market data → CoinGecko (free, no key)
    │  Everything else → HTTPS with auto-retry on 429/5xx
    ▼
api.noelclaw.com  ← Cloudflare Worker (rate limit + CORS)
    │
    ├── Convex backend      → ask_noel, automations, framework, DeFi
    ├── Supabase Edge       → swarm, vault
    └── Railway             → MiroShark simulation engine
```

Wallet signing happens locally in the MCP server via ethers.js. Convex never holds or sees your private key.

---

## Install

### Claude Code

```bash
claude mcp add noelclaw -s user -- npx -y @noelclaw/mcp
```

### Claude Desktop

Mac: `~/Library/Application Support/Claude/claude_desktop_config.json`
Windows: `%APPDATA%\Claude\claude_desktop_config.json`

```json
{
  "mcpServers": {
    "noelclaw": {
      "command": "npx",
      "args": ["-y", "@noelclaw/mcp"]
    }
  }
}
```

### Cursor / Windsurf

Edit `~/.cursor/mcp.json` (Cursor) or `~/.windsurf/mcp_config.json` (Windsurf):

```json
{
  "mcpServers": {
    "noelclaw": {
      "command": "npx",
      "args": ["-y", "@noelclaw/mcp"]
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
      - "-y"
      - "@noelclaw/mcp"
```

### Aeon

```yaml
skills:
  noelclaw:
    mcp_server:
      command: npx
      args:
        - "-y"
        - "@noelclaw/mcp"
    enabled: true
```

---

## Tool Reference

### `get_market_data`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `token` | string | no | Focus on a specific token, e.g. `"BTC"`, `"ETH"` |

No parameters = returns top-20 by market cap + trending + BTC/ETH/SOL key prices.

---

### `get_token_data`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `question` | string | yes | Token name or query, e.g. `"show me ETH"`, `"PEPE price"` |

---

### `ask_noel`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `question` | string | yes | Your question or prompt |
| `messages` | array | no | Prior conversation turns for context |

---

### `get_wallet_address`

No parameters. Returns your local Noelclaw wallet address. Keys stored at `~/.noelclaw/wallet.json` and never leave your device.

---

### `swap_tokens`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `fromToken` | string | yes | Token to sell: `ETH`, `USDC`, `USDT`, `DAI`, `WETH` |
| `toToken` | string | yes | Token to buy |
| `amount` | string | yes | Amount, e.g. `"0.01"` or `"50%"` |

Routes through 0x Permit2 on Base mainnet. Signed locally — Convex never sees your key.

---

### `send_token`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `token` | string | yes | `ETH`, `USDC`, `USDT`, `DAI`, or `WETH` |
| `toAddress` | string | yes | Recipient address (`0x...`) |
| `amount` | string | yes | Human-readable amount, e.g. `"0.5"` |

---

### `create_automation`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `rawInput` | string | yes | Plain English description |

Examples:
- `"Buy 50 USDC of ETH every day at 9am"`
- `"Alert me when BTC hits $120,000"`
- `"If ETH drops 5% in 1 hour, buy $100 worth"`

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
| `config.enabledAgents` | string[] | no | Agent IDs to enable (default: all) |

On start, automatically fetches live BTC/ETH/SOL prices from CoinGecko and writes them to shared memory — no stale cached data.

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
| `value` | string | yes | Value to store |
| `ttlSeconds` | number | no | Auto-delete after N seconds |

---

### `get_swarm_memory`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `key` | string | yes | Memory key to read |

---

### `get_execution_scores`

No parameters. All skill scores: success rate, win/loss, avg duration, last adapted timestamp.

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

Auto-versions on every update — all previous versions are preserved and accessible via `vault_history`.

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
| `fromVersion` | number | yes | Earlier version number |
| `toVersion` | number | yes | Later version number |

---

### `vault_export`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `type` | string | no | Export only this type (omit for full vault export) |

---

### `miroshark_simulate`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `scenario` | string | yes | What to simulate — plain English, any topic |

Automatically builds a knowledge graph, generates agent personas, runs belief propagation, and returns a `simulation_id`. Poll with `miroshark_status`. Agent count and rounds are determined by MiroShark based on scenario complexity.

---

### `miroshark_status`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `simulation_id` | string | yes | ID from `miroshark_simulate` |

Polls through: `preparing → running → complete`. Automatically starts the simulation when agent preparation finishes.

---

### `miroshark_stop`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `simulation_id` | string | yes | Simulation ID to stop |

Stops a running or preparing simulation immediately.

---

### `set_telegram`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `telegramBotToken` | string | yes | Bot token from @BotFather |
| `telegramChatId` | string | yes | Your Telegram chat ID |

---

### `post_tweet`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `content` | string | yes | Tweet text (max 280 chars) |

Requires `AYRSHARE_API_KEY` in env.

---

### `humanize_text`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `text` | string | yes | AI-generated text to rewrite |

Strips AI patterns using MiniMax. Requires `MINIMAX_API_KEY` in env.

---

## Environment Variables

Set in your MCP client config under the `env` block:

| Variable | Required | Purpose |
|----------|----------|---------|
| `NOELCLAW_API_KEY` | no | Links to your noelclaw.com account |
| `ALCHEMY_API_KEY` | no | Faster swap quotes and Base balance lookups |
| `BANKR_API_KEY` | no | BYOK — forwarded as your own Bankr key for swarm agents |
| `TELEGRAM_BOT_TOKEN` | no | Telegram bot token (from @BotFather) |
| `TELEGRAM_CHAT_ID` | no | Your Telegram chat ID |
| `MINIMAX_API_KEY` | for `humanize_text` | MiniMax API key |
| `AYRSHARE_API_KEY` | for `post_tweet` | Ayrshare API key |

---

## Troubleshooting

| Error | Fix |
|-------|-----|
| Tools not appearing | Restart your MCP client after adding the server |
| `npx` hangs on first run | Use `-y` flag: `npx -y @noelclaw/mcp` |
| Tools not found after restart | Run `npx clear-npx-cache` then restart |
| `get_swarm_status` empty | Start swarm first with `start_swarm` |
| Swap fails | Check ETH balance and Base mainnet connectivity |
| `humanize_text` fails | Set `MINIMAX_API_KEY` in env |
| `post_tweet` fails | Set `AYRSHARE_API_KEY` in env |
| Rate limit (429) | Auto-retries up to 3× with backoff — no action needed |
