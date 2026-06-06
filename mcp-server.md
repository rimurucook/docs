# MCP Server Reference

`@noelclaw/mcp` is an MCP server that exposes all Noelclaw tools to any MCP-compatible AI client. Install once via `npx` — no build step, no account, no config required.

**81 tools across 18 categories.** Persistent vault, semantic memory, automations, DeFi execution, token scanning, multi-agent swarm, live web research, autonomous monitors, AI code generation, MiroShark simulation, and more.

---

## Tool Categories

### Market & Intel (5)

| Tool | Description |
|------|-------------|
| `get_market_data` | Live top-20 coins by market cap, trending, BTC/ETH/SOL key prices — from CoinGecko, no API key needed |
| `get_token_data` | Price, 24h change, market cap, volume, and ATH for any specific token |
| `compare_tokens` | Side-by-side comparison of two or more tokens — price, volume, market cap |
| `market_overview` | Top movers, Fear & Greed index, BTC dominance |
| `token_history` | Historical OHLC price data for any token |

### Research & Insight (3)

| Tool | Description |
|------|-------------|
| `ask_noel` | Chat with Noel — crypto AI with live market context, DeFi analysis, and trade ideas |
| `market_thesis` | Bull/bear thesis for any token or sector |
| `trade_plan` | Entry, exit, and risk levels for a trade setup |

### DeFi & Portfolio (6)

> Transactions signed client-side — private key never leaves your machine.

| Tool | Description |
|------|-------------|
| `get_portfolio` | Current token balances and total USD value for your local wallet on Base |
| `estimate_swap` | Preview a swap — expected output and price impact, without executing |
| `swap_tokens` | Swap tokens on Base via 0x Permit2 (ETH, USDC, USDT, DAI, WETH). Signed locally |
| `send_token` | Send ETH or ERC-20 tokens to any address on Base mainnet |
| `analyze_wallet` | AI-powered deep analysis of any public wallet — holdings, risk signals, patterns |
| `get_defi_yields` | Top DeFi yield opportunities on Base — live APY and TVL from DeFiLlama |

### Automations (6)

| Tool | Description |
|------|-------------|
| `create_automation` | Create DCA, price alert, or conditional buy/sell in plain English |
| `list_automations` | All automations with status, run counts, and next run time |
| `pause_automation` | Pause or resume an automation by ID |
| `delete_automation` | Permanently delete an automation |
| `get_automation_runs` | Execution history for an automation — status, tx hash, error per run |
| `run_automation` | Trigger an automation manually right now |

### Swarm (6)

> Multiple AI agents research and monitor in parallel with shared memory.

| Tool | Description |
|------|-------------|
| `start_swarm` | Start the multi-agent swarm — auto-loads live BTC/ETH/SOL prices into shared memory |
| `stop_swarm` | Stop the active swarm session |
| `get_swarm_status` | Active session, shared memory snapshot, and execution scores |
| `swarm_research` | Multi-agent research on a topic — auto-saves findings to vault |
| `swarm_brief` | Summary of everything the swarm found |
| `trigger_agent` | Run a specific agent now |

### Noel Framework (3)

| Tool | Description |
|------|-------------|
| `list_playbooks` | Available playbooks with step counts and usage |
| `run_playbook` | Execute a playbook with Sentinel gating per step |
| `get_noel_ledger` | Sentinel audit trail — every gate decision with check type, duration, and reason |

### Noel Vault (12)

| Tool | Description |
|------|-------------|
| `vault_save` | Save or update an artifact with auto-versioning |
| `vault_read` | Read a vault entry by key |
| `vault_list` | List vault entries filtered by type, agent, or pinned status |
| `vault_search` | Full-text search across the vault |
| `vault_history` | Full version history of a vault entry (git log style) |
| `vault_diff` | Compare two versions of a vault entry (git diff style) |
| `vault_export` | Export entire vault or specific type as a structured bundle |
| `vault_store_credential` | Securely store an API key or secret in the vault |
| `vault_get_credential` | Retrieve a stored credential by name |
| `vault_pin` | Pin an important entry |
| `vault_delete` | Delete a vault entry permanently |
| `vault_tag` | Add or update tags on an entry |

### Wallet & Notifications (2)

| Tool | Description |
|------|-------------|
| `get_wallet_address` | Your local Noelclaw wallet address — keys stored at `~/.noelclaw/wallet.json`, never leave your machine |
| `set_telegram` | Connect Telegram for swarm events and automation alerts |

### MiroShark (3)

| Tool | Description |
|------|-------------|
| `miroshark_simulate` | Run a multi-agent social simulation for any scenario in plain English |
| `miroshark_status` | Poll simulation status — prep, running, and completion with AI brief |
| `miroshark_stop` | Stop a running simulation |

### Agents (2)

| Tool | Description |
|------|-------------|
| `list_agents` | List all available specialist agents — built-in experts plus community-published agents |
| `hire_agent` | Hire a specialist agent (analyst, risk-manager, researcher, executor, scout) to run a task |

### Token Scanner (4)

| Tool | Description |
|------|-------------|
| `score_token` | Score a specific token for dip-reversal potential — hard gates + weighted scoring |
| `check_token` | Safety check a token address — honeypot detection, liquidity, sell-side risk |
| `scan_dips` | Scan live markets for tokens showing early buy-pressure reversal signals |
| `scan_momentum` | Scan for tokens breaking out upward with rising momentum |

### Coder (5)

| Tool | Description |
|------|-------------|
| `generate_contract` | Generate a Solidity smart contract (ERC-20, ERC-721, DeFi hooks, Uniswap v3/v4) with NatSpec |
| `audit_contract` | AI code review of a Solidity contract — reentrancy, access control, overflow, gas issues |
| `explain_code` | Plain-English explanation of any code snippet — Solidity, TypeScript, or config |
| `review_code` | Code review with actionable feedback on logic, patterns, and best practices |
| `generate_mcp_skill` | Generate a new MCP tool definition from a plain-English description |

### Base & Chain (4)

| Tool | Description |
|------|-------------|
| `base_query_vaults` | List Morpho yield vaults on Base sorted by APY — vault name, asset, APY, total deposits |
| `base_list_markets` | List Moonwell lending/borrowing markets — supply APY, borrow APY, liquidity, utilization |
| `base_prepare_deposit` | Get deposit instructions for a Morpho vault — address, APY, and step-by-step guide |
| `base_chain_stats` | Real-time Base chain stats: ETH price, gas in gwei, and latest block info |

### Content & Humanizer (3)

| Tool | Description |
|------|-------------|
| `humanize_text` | Strip AI writing patterns — makes output sound natural (requires `MINIMAX_API_KEY`) |
| `write_thread` | Write a Twitter/X thread on any topic |
| `write_post` | Write a punchy social post |

### Semantic Memory (9)

| Tool | Description |
|------|-------------|
| `memory_add` | Add text, notes, or auto-fetch a URL to semantic memory |
| `memory_search` | Search by meaning — "what did I save about X?" |
| `memory_context` | Load entries relevant to the current session topic |
| `memory_profile` | Your memory profile — preferences, history, patterns |
| `memory_list` | List recent memory entries |
| `memory_delete` | Remove a memory entry by ID |
| `memory_insight` | AI insights derived from your memory patterns |
| `memory_extract` | Auto-extract discrete facts from any text and save each individually |
| `memory_consolidate` | Merge overlapping memories on a topic into one clean summary |

### Web Research (2)

| Tool | Description |
|------|-------------|
| `web_search` | Search the web in real time — returns top results with titles, URLs, and summaries |
| `web_scrape` | Read any URL and return its full content — articles, docs, pages |

### Autonomous Monitor (3)

> Runs research on a schedule — no chat needed. Saves findings to vault and sends Telegram briefings.

| Tool | Description |
|------|-------------|
| `create_monitor` | Set up a recurring research agent for any topic — daily, weekly, or custom cron |
| `list_monitors` | List all active monitors with topic, schedule, and next run time |
| `cancel_monitor` | Stop and delete a monitor by ID |

### Session OS (3)

| Tool | Description |
|------|-------------|
| `noel_boot` | Start a session — loads vault context, semantic memory, live market data, and active automations |
| `noel_status` | Full dashboard — memory usage, swarm state, active automations |
| `noel_shutdown` | End session — saves a summary to vault for context continuity next session |

---

## How It Works

```
AI Client (Claude / Cursor / Hermes / Windsurf / Aeon)
    │
    │  MCP protocol (stdio)
    ▼
@noelclaw/mcp (Node.js)
    │  Market data → CoinGecko (free, no key)
    │  DeFi yields → DeFiLlama (free, no key)
    │  Base chain data → Morpho + Moonwell APIs
    │  Swap routing → 0x Protocol v2
    │  Everything else → HTTPS with auto-retry on 429/5xx
    ▼
api.noelclaw.com  ← Cloudflare Worker (rate limit + CORS)
    │
    ├── Convex backend   → vault, memory, automations, swarm, DeFi, OS
    └── Railway          → MiroShark simulation engine
```

Wallet signing happens locally via ethers.js. The private key lives at `~/.noelclaw/wallet.json` and never leaves your machine.

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

### `compare_tokens`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `tokens` | string[] | yes | Token symbols to compare, e.g. `["ETH", "SOL", "BTC"]` |

---

### `market_overview`

No parameters. Returns top movers, Fear & Greed index, and BTC dominance.

---

### `token_history`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `token` | string | yes | Token symbol, e.g. `"ETH"` |
| `days` | number | no | Number of days of history (default 7) |

---

### `ask_noel`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `question` | string | yes | Your question or prompt |
| `messages` | array | no | Prior conversation turns for context |

---

### `market_thesis`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `token` | string | yes | Token or sector to analyze |

Returns bull case, bear case, and key risks.

---

### `trade_plan`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `token` | string | yes | Token to plan a trade for |
| `direction` | string | no | `"long"` or `"short"` |

Returns entry, target, stop-loss, and position sizing guidance.

---

### `get_portfolio`

No parameters. Returns token balances and total USD value for your local wallet on Base. Call before swapping to confirm balance.

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

Routes through 0x Permit2 on Base mainnet. Signed locally — private key never leaves your device.

---

### `send_token`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `token` | string | yes | `ETH`, `USDC`, `USDT`, `DAI`, or `WETH` |
| `toAddress` | string | yes | Recipient address (`0x...`) |
| `amount` | string | yes | Human-readable amount, e.g. `"0.5"` |

---

### `analyze_wallet`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `address` | string | yes | Any wallet address to analyze (`0x...`) |
| `label` | string | no | Optional label, e.g. `"whale from Twitter"` |

Returns holdings, portfolio value, behavioral profile (whale / degen / LP provider), and AI analysis.

---

### `get_defi_yields`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `token` | string | no | Filter by token, e.g. `"USDC"`, `"ETH"` |
| `minApy` | number | no | Minimum APY % to show (default 1) |
| `limit` | number | no | Max results (default 20) |

Fetches live data from DeFiLlama. No API key required.

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

### `run_automation`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `automationId` | string | yes | ID from `list_automations` |

Triggers the automation immediately, regardless of its schedule.

---

### `start_swarm`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `config.enabledAgents` | string[] | no | Agent IDs to enable (default: all) |

On start, fetches live BTC/ETH/SOL prices and writes them to shared swarm memory.

---

### `stop_swarm`

No parameters.

---

### `get_swarm_status`

No parameters. Returns active session, shared memory snapshot, and execution scores.

---

### `swarm_research`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `topic` | string | yes | Research topic, e.g. `"best DeFi yields on Base this month"` |

Runs multi-agent research in parallel and auto-saves findings to vault.

---

### `swarm_brief`

No parameters. Returns a summary of everything the swarm has found and saved.

---

### `trigger_agent`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `agentId` | string | yes | Agent ID to trigger |

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
| `tags` | string[] | no | Tags for filtering |
| `commitMsg` | string | no | Version message |

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

### `vault_store_credential`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `name` | string | yes | Credential name, e.g. `"alchemy_key"` |
| `value` | string | yes | The secret value |

---

### `vault_get_credential`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `name` | string | yes | Credential name to retrieve |

---

### `vault_pin`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `key` | string | yes | Vault key to pin |

---

### `vault_delete`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `key` | string | yes | Vault key to delete |

---

### `vault_tag`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `key` | string | yes | Vault key |
| `tags` | string[] | yes | Tags to set (replaces existing tags) |

---

### `get_wallet_address`

No parameters. Returns your local wallet address. Keys stored at `~/.noelclaw/wallet.json` and never leave your device.

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

Builds a knowledge graph, generates agent personas, runs belief propagation. Returns a `simulation_id`. Poll with `miroshark_status`.

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

No parameters. Returns all specialist agents with name, ID, description, and pricing type.

---

### `hire_agent`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `agentId` | string | yes | Agent ID from `list_agents`. Built-in: `analyst`, `risk-manager`, `researcher`, `executor`, `scout` |
| `task` | string | yes | The task or question — be specific |
| `maxTokens` | number | no | Max response tokens (default 800) |

---

### `score_token`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `mint` | string | yes | Token contract address |
| `minLiquidity` | number | no | Minimum liquidity in USD (default 50000) |

Returns a 0–100 score, pass/fail, pattern label, and factor breakdown.

---

### `check_token`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `address` | string | yes | Token contract address to check |

Honeypot detection, liquidity depth, and sell-side risk flags.

---

### `scan_dips`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `minScore` | number | no | Minimum score (default 50) |
| `minLiquidity` | number | no | Minimum liquidity in USD (default 50000) |
| `limit` | number | no | Max results (default 10) |

---

### `scan_momentum`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `minScore` | number | no | Minimum score (default 50) |
| `limit` | number | no | Max results (default 10) |

---

### `generate_contract`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `description` | string | yes | What the contract does, token type, key mechanics |
| `chain` | string | no | Target chain (default: Base) |

Generates Solidity with NatSpec and SPDX license.

---

### `audit_contract`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `code` | string | yes | Solidity source code to audit |

Reviews for reentrancy, access control, overflow, and gas issues.

---

### `explain_code`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `code` | string | yes | Code snippet to explain |
| `context` | string | no | What the code is part of |

---

### `review_code`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `code` | string | yes | Code to review |
| `focus` | string | no | `"security"`, `"performance"`, `"readability"`, `"all"` |

---

### `generate_mcp_skill`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `description` | string | yes | Plain-English description of the MCP tool to generate |

Generates a complete tool definition including name, inputSchema, and handler stub.

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
| `asset` | string | no | Filter by asset symbol, e.g. `USDC`, `ETH` |

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

### `write_thread`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `topic` | string | yes | Topic or angle for the thread |
| `tone` | string | no | `"alpha"`, `"educational"`, `"degen"` |

---

### `write_post`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `topic` | string | yes | What the post is about |
| `tone` | string | no | Tone or style guidance |

---

### `memory_add`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `content` | string | yes | Text, notes, or a URL to auto-fetch and save |
| `tags` | string[] | no | Tags for this memory entry |

---

### `memory_search`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `query` | string | yes | Search by meaning — e.g. `"ETH yield strategies"` |
| `n` | number | no | Max results to return (default 10) |

Semantic search — finds by meaning, not just keywords.

---

### `memory_context`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `topic` | string | yes | Current topic or session focus |
| `n` | number | no | Max entries to load (default 5) |

---

### `memory_profile`

No parameters. Returns your memory profile — preferences, history, and patterns learned from your saved entries.

---

### `memory_list`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `n` | number | no | Max entries to return (default 20) |
| `tag` | string | no | Filter by tag |

---

### `memory_delete`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `id` | string | yes | Memory entry ID from `memory_list` or `memory_search` |

---

### `memory_insight`

No parameters. AI-generated insights from patterns across your saved memories.

---

### `memory_extract`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `text` | string | yes | Source text to extract facts from |
| `tags` | string[] | no | Tags to apply to each extracted fact |

Splits a block of text into discrete facts and saves each as a separate memory entry.

---

### `memory_consolidate`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `topic` | string | yes | Topic to consolidate memories on |
| `n` | number | no | Max memories to search and merge (default 20) |

Finds overlapping memories on a topic, merges them into one clean summary, saves it back.

---

### `web_search`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `query` | string | yes | What to search for |
| `limit` | number | no | Max results to return (default 5) |

Searches the web in real time via Firecrawl. Requires `FIRECRAWL_API_KEY` in env.

---

### `web_scrape`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `url` | string | yes | URL to read |

Returns the full text content of any web page. Requires `FIRECRAWL_API_KEY` in env.

---

### `create_monitor`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `topic` | string | yes | What to research — topic, keyword, or question |
| `schedule` | string | yes | Cron preset or expression. Presets: `daily-8am`, `daily-6pm`, `weekly-monday`, `hourly`. Or raw cron: `0 8 * * *` |
| `label` | string | no | Short label to identify this monitor, e.g. `"morning brief"` |

Creates a scheduled job that automatically researches the topic, saves findings to vault, and sends a Telegram notification. Requires `TRIGGER_SECRET_KEY` in env.

---

### `list_monitors`

No parameters. Returns all active monitors with their topic, schedule, next run time, and ID.

---

### `cancel_monitor`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `id` | string | yes | Monitor ID from `list_monitors` |

---

### `noel_boot`

No parameters. Starts a session — loads vault context, semantic memory (recent + user preferences), live market data, and active automations. Run this at the start of every session.

---

### `noel_status`

No parameters. Full dashboard — memory usage, swarm state, active automations, and wallet balance.

---

### `noel_shutdown`

No parameters. Ends the session and saves a summary to vault for context continuity next session.

---

## Environment Variables

Set in your MCP client config under the `env` block. All optional.

| Variable | Purpose |
|----------|---------|
| `ANTHROPIC_API_KEY` | Use your own Anthropic key for the CLI agent. Without it, calls proxy through the Noelclaw platform automatically |
| `BANKR_API_KEY` | Use Bankr (Grok-3) for the CLI agent instead of Anthropic |
| `ANTHROPIC_MODEL` | Override model (default: `claude-haiku-4-5-20251001`) |
| `ALCHEMY_API_KEY` | Faster swap quotes and Base balance lookups |
| `TELEGRAM_BOT_TOKEN` | Your Telegram bot token for automation alerts |
| `TELEGRAM_CHAT_ID` | Your Telegram chat ID for delivery |
| `MINIMAX_API_KEY` | Required for `humanize_text` |

---

## Troubleshooting

| Error | Fix |
|-------|-----|
| Tools not appearing | Restart your MCP client after adding the server |
| `npx` hangs on first run | Use `-y` flag: `npx -y @noelclaw/mcp` |
| Tools not found after restart | Run `npx clear-npx-cache` then restart |
| `get_swarm_status` empty | Start swarm first with `start_swarm` |
| Swap fails | Check balance with `get_portfolio`, confirm Base mainnet connectivity |
| `humanize_text` fails | Set `MINIMAX_API_KEY` in env |
| Rate limit (429) | Auto-retries up to 3× with backoff — no action needed |
