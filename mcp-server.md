# MCP Server Reference

`@noelclaw/mcp` is an MCP server that exposes all Noelclaw tools to any MCP-compatible AI client. Install once via `npx` — no build step, no config required.

**74 tools across 15 categories.** Persistent vault, semantic memory, automations, DeFi execution, token scanning, multi-agent swarm, research & insight, AI code generation, MiroShark simulation, and more.

---

## Tool Categories

### Market & Intel

| Tool | Description |
|------|-------------|
| `get_market_data` | Live top-20 coins by market cap, trending, BTC/ETH/SOL key prices — from CoinGecko, no API key needed |
| `get_token_data` | Price, 24h change, market cap, volume, and ATH for any specific token |

### AI Assistant

| Tool | Description |
|------|-------------|
| `ask_noel` | Chat with Noel — crypto AI with live market context, DeFi analysis, and trade ideas |

### DeFi & Portfolio

| Tool | Description |
|------|-------------|
| `get_portfolio` | Current token balances and total USD value for your MCP wallet on Base |
| `estimate_swap` | Preview a swap — expected output and price impact, without executing |
| `swap_tokens` | Swap tokens on Base via 0x Permit2 (ETH, USDC, USDT, DAI, WETH). Signed locally |
| `send_token` | Send ETH or ERC-20 tokens to any address on Base mainnet |
| `analyze_wallet` | AI-powered deep analysis of any public wallet — holdings, risk signals, patterns |

### Automations

| Tool | Description |
|------|-------------|
| `create_automation` | Create DCA, price alert, or conditional buy/sell in plain English |
| `list_automations` | All automations with status, run counts, and next run time |
| `pause_automation` | Pause or resume an automation by ID |
| `delete_automation` | Permanently delete an automation |
| `get_automation_runs` | Execution history for an automation — status, tx hash, error per run |

### Swarm

| Tool | Description |
|------|-------------|
| `start_swarm` | Start the multi-agent swarm — auto-loads live BTC/ETH/SOL prices into shared memory |
| `stop_swarm` | Stop the active swarm session |
| `get_swarm_status` | Active session, shared memory snapshot, and execution scores |
| `swarm_research` | Multi-agent research on a topic — auto-saves findings to vault |
| `swarm_brief` | Summary of everything the swarm found |
| `trigger_agent` | Run a specific agent now |

### Noel Framework

| Tool | Description |
|------|-------------|
| `list_playbooks` | Available playbooks with step counts and usage |
| `run_playbook` | Execute a playbook by ID |
| `get_noel_ledger` | Credits and full audit trail |

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
| `vault_remember` | One-liner quick save — just pass content, type and title are auto-inferred |
| `vault_context` | Load relevant vault entries as formatted context for prompt injection |
| `vault_store_credential` | Securely store an API key or secret in the vault |
| `vault_get_credential` | Retrieve a stored credential by name |
| `vault_pin` | Pin an important entry |
| `vault_delete` | Delete an entry |
| `vault_tag` | Add or update tags |

### Wallet & Notifications

| Tool | Description |
|------|-------------|
| `get_wallet_address` | Your local Noelclaw wallet address — keys stored at `~/.noelclaw/wallet.json`, never leave your machine |
| `set_telegram` | Connect Telegram for swarm events and automation alerts |

### MiroShark

| Tool | Description |
|------|-------------|
| `miroshark_simulate` | Run a multi-agent social simulation for any scenario in plain English |
| `miroshark_status` | Poll simulation status — prep, running, and completion |
| `miroshark_stop` | Stop a running simulation |

### Agents

| Tool | Description |
|------|-------------|
| `list_agents` | List all available specialist agents — built-in experts plus community-published agents |
| `hire_agent` | Hire a specialist agent (analyst, risk-manager, researcher, executor, scout) to run a task |

### Token Scanner

| Tool | Description |
|------|-------------|
| `score_token` | Score a specific token for dip-reversal potential — hard gates + weighted scoring |
| `check_token` | Safety check a token address — honeypot detection, liquidity, sell-side risk |
| `scan_dips` | Scan live markets for tokens showing early buy-pressure reversal signals |

### Coder

> Requires `BANKR_API_KEY` — powered by Bankr LLM (Grok-3 by default).

| Tool | Description |
|------|-------------|
| `generate_contract` | Generate a Solidity smart contract (ERC-20, ERC-721, DeFi hooks, Uniswap v3/v4) with NatSpec |
| `audit_contract` | AI code review of a Solidity contract — reentrancy, access control, overflow, gas issues |
| `explain_code` | Plain-English explanation of any code snippet — Solidity, TypeScript, or config |
| `review_code` | Code review with actionable feedback on logic, patterns, and best practices |

### Base & Chain

| Tool | Description |
|------|-------------|
| `base_query_vaults` | List Morpho yield vaults on Base sorted by APY — vault name, asset, APY, total deposits |
| `base_list_markets` | List Moonwell lending/borrowing markets — supply APY, borrow APY, liquidity, utilization |
| `base_prepare_deposit` | Get deposit instructions for a Morpho vault — address, APY, and step-by-step guide |
| `base_chain_stats` | Real-time Base chain stats: ETH price, gas in gwei, and latest block info |

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
    │  Base chain data → Morpho + Moonwell APIs
    │  Coder tools → Bankr LLM (BANKR_API_KEY required)
    │  Everything else → HTTPS with auto-retry on 429/5xx
    ▼
api.noelclaw.com  ← Cloudflare Worker (rate limit + CORS)
    │
    ├── Convex backend      → ask_noel, automations, agents, framework, DeFi
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

### `get_portfolio`

No parameters. Returns token balances and total USD value for your MCP wallet on Base. Call this before swapping to confirm available balance.

---

### `estimate_swap`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `fromToken` | string | yes | Token to sell: `ETH`, `USDC`, `USDT`, `DAI`, `WETH` |
| `toToken` | string | yes | Token to buy |
| `amount` | string | yes | Amount, e.g. `"0.01"` or `"50%"` |

Returns expected output and price impact. Does not execute.

---

### `swap_tokens`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `fromToken` | string | yes | Token to sell: `ETH`, `USDC`, `USDT`, `DAI`, `WETH` |
| `toToken` | string | yes | Token to buy |
| `amount` | string | yes | Human-readable amount, e.g. `"0.01"` or `"50%"` of balance |

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

### `get_automation_runs`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `automationId` | string | yes | ID from `list_automations` |
| `limit` | number | no | Max runs to return (default 20) |

Returns each run's status (success/failed/skipped), amount, tx hash, and error message if any.

---

### `start_swarm`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `config.enabledAgents` | string[] | no | Agent IDs to enable (default: all) |

On start, automatically fetches live BTC/ETH/SOL prices from CoinGecko and writes them to shared memory.

---

### `stop_swarm`

No parameters.

---

### `get_swarm_status`

No parameters. Returns active session, shared memory snapshot, and top execution scores.

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

Auto-versions on every update — all previous versions accessible via `vault_history`.

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

### `get_wallet_address`

No parameters. Returns your local Noelclaw wallet address. Keys stored at `~/.noelclaw/wallet.json` and never leave your device.

---

### `set_telegram`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `telegramBotToken` | string | yes | Bot token from @BotFather |
| `telegramChatId` | string | yes | Your Telegram chat ID |

---

### `miroshark_simulate`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `scenario` | string | yes | What to simulate — plain English, any topic |

Automatically builds a knowledge graph, generates agent personas, runs belief propagation, and returns a `simulation_id`. Poll with `miroshark_status`.

---

### `miroshark_status`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `simulation_id` | string | yes | ID from `miroshark_simulate` |

Polls through: `preparing → running → complete`.

---

### `miroshark_stop`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `simulation_id` | string | yes | Simulation ID to stop |

---

### `list_agents`

No parameters. Returns all available specialist agents with name, ID, description, pricing type (free / token-based), and run count.

---

### `hire_agent`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `agentId` | string | yes | Agent ID from `list_agents`. Built-in: `analyst`, `risk-manager`, `researcher`, `executor`, `scout` |
| `task` | string | yes | The task or question. Be specific — better input = better output |
| `maxTokens` | number | no | Max response tokens (default 800, max 1200) |

---

### `score_token`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `mint` | string | yes | Token contract address |
| `minLiquidity` | number | no | Minimum liquidity threshold in USD (default 50000) |

Returns a 0–100 score, pass/fail, pattern label, and breakdown of all scoring factors.

---

### `check_token`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `address` | string | yes | Token contract address to check |

Safety check — honeypot detection, liquidity depth, sell-side risk flags.

---

### `scan_dips`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `minScore` | number | no | Minimum score to include (default 50) |
| `minLiquidity` | number | no | Minimum liquidity in USD (default 50000) |
| `limit` | number | no | Max results (default 10) |

Scans live markets for tokens showing buy-pressure reversal after a 1h dip. Returns scored candidates sorted by opportunity quality.

---

### `generate_contract`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `description` | string | yes | What the contract does, token type, key mechanics |
| `chain` | string | no | Target chain (default: Base) |

Generates Solidity with NatSpec, SPDX license, and pragma. Requires `BANKR_API_KEY`.

---

### `audit_contract`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `code` | string | yes | Solidity source code to audit |

Reviews for reentrancy, access control, overflow, gas issues, and common vulnerabilities. Requires `BANKR_API_KEY`.

---

### `explain_code`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `code` | string | yes | Code snippet to explain |
| `context` | string | no | What the code is part of |

Plain-English explanation of any Solidity, TypeScript, or config snippet. Requires `BANKR_API_KEY`.

---

### `review_code`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `code` | string | yes | Code to review |
| `focus` | string | no | Area to focus on: `security`, `performance`, `readability`, `all` |

Code review with actionable feedback. Requires `BANKR_API_KEY`.

---

### `base_query_vaults`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `asset` | string | no | Filter by asset symbol, e.g. `USDC`, `WETH`, `cbBTC` |
| `limit` | number | no | Max vaults to return (default 10) |

Returns Morpho yield vaults on Base sorted by APY.

---

### `base_list_markets`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `asset` | string | no | Filter by asset symbol, e.g. `USDC`, `ETH`, `cbBTC` |

Returns Moonwell lending/borrowing markets with supply APY, borrow APY, total liquidity, and utilization rate.

---

### `base_prepare_deposit`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `vaultName` | string | yes | Name or partial name of the vault, e.g. `"Gauntlet USDC"` |
| `amount` | string | yes | Amount to deposit, e.g. `"100"` |
| `asset` | string | yes | Asset to deposit, e.g. `USDC`, `WETH` |

Returns vault address, expected APY, and step-by-step deposit instructions. Does NOT execute the transaction.

---

### `base_chain_stats`

No parameters. Returns real-time Base chain stats: ETH price, gas price in gwei, and latest block info.

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
| `BANKR_API_KEY` | for Coder tools | Bankr LLM API key — required for all 6 coder tools |
| `TELEGRAM_BOT_TOKEN` | no | Telegram bot token (from @BotFather) |
| `TELEGRAM_CHAT_ID` | no | Your Telegram chat ID |
| `MINIMAX_API_KEY` | for `humanize_text` | MiniMax API key |

---

## Troubleshooting

| Error | Fix |
|-------|-----|
| Tools not appearing | Restart your MCP client after adding the server |
| `npx` hangs on first run | Use `-y` flag: `npx -y @noelclaw/mcp` |
| Tools not found after restart | Run `npx clear-npx-cache` then restart |
| `get_swarm_status` empty | Start swarm first with `start_swarm` |
| Swap fails | Check ETH balance with `get_portfolio` and confirm Base mainnet connectivity |
| Coder tools fail | Set `BANKR_API_KEY` in env |
| `humanize_text` fails | Set `MINIMAX_API_KEY` in env |
| Rate limit (429) | Auto-retries up to 3× with backoff — no action needed |
