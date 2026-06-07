# MCP Server Reference

`@noelclaw/mcp` is an MCP server that exposes all Noelclaw tools to any MCP-compatible AI client. Install once via `npx` — no build step, no account, no config required.

**90 tools across 19 categories.** Persistent vault, semantic memory, automations, DeFi execution, token scanning, multi-agent swarm, live web research, autonomous monitors, GitHub integration, AI code generation, MiroShark simulation, and more.

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
| `swarm_synthesize` | Synthesize all swarm findings into one intelligence report |
| `trigger_agent` | Run a specific agent now |

### Noel Framework (3)

| Tool | Description |
|------|-------------|
| `list_playbooks` | Available playbooks with step counts and usage |
| `run_playbook` | Execute a playbook with Sentinel gating per step |
| `get_noel_ledger` | Sentinel audit trail — every gate decision with check type, duration, and reason |

### Noel Vault (14)

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
| `vault_link` | Create a semantic relationship between two vault entries — build a knowledge graph |
| `vault_related` | Traverse the knowledge graph — see all entries linked to a given key |

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

### Agents (5)

| Tool | Description |
|------|-------------|
| `list_agents` | List all available specialist agents — built-in experts plus community-published agents |
| `hire_agent` | Hire a specialist agent (analyst, risk-manager, researcher, executor, scout) to run a task |
| `agent_spawn` | Create a persistent named agent with a goal — survives across sessions, state saved to vault |
| `agent_recall` | Recall a persistent agent — loads goal, status, progress history, and next step |
| `agent_update` | Log progress and findings to a persistent agent — creates a new vault version automatically |

### Token Scanner (3)

| Tool | Description |
|------|-------------|
| `score_token` | Score a specific token for dip-reversal potential — hard gates + weighted scoring |
| `check_token` | Safety check a token address — honeypot detection, liquidity, sell-side risk |
| `scan_market` | Scan live Base pools — `mode=dips` for reversal setups, `mode=momentum` for breakouts |

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

### Content & Humanizer (2)

| Tool | Description |
|------|-------------|
| `humanize_text` | Strip AI writing patterns — makes output sound natural (requires `MINIMAX_API_KEY`) |
| `write_content` | Write a Twitter/X thread or single post — `format=thread` (default) or `format=post` |

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

### Session OS (1)

| Tool | Description |
|------|-------------|
| `noel_status` | Full dashboard — memory usage, swarm health, active automations, recent research, execution scores |

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
api.noelclaw.com  ← rate limit + CORS + auth
    │
    ├── Noelclaw backend  → vault, memory, automations, swarm, DeFi, OS, monitors
    └── MiroShark backend → multi-agent simulation engine
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

### `vault_link`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `fromKey` | string | yes | Source vault entry key |
| `toKey` | string | yes | Target vault entry key |
| `relation` | string | yes | How `fromKey` relates to `toKey` — see relation types below |

Creates a directed edge between two vault entries, building a knowledge graph over time. Both entries must already exist in your vault. Duplicate links are updated in-place instead of creating duplicates.

**Relation types:**

| Relation | Meaning |
|----------|---------|
| `references` | This entry cites or uses information from the target |
| `derived_from` | This entry was built from or synthesizes the target |
| `supersedes` | This entry replaces or improves on the target |
| `related` | General association — thematically connected |
| `continues` | This entry is a follow-on to the target (e.g. part 2 of a thread) |

**Example:**
```
vault_link fromKey="research/eth-analysis" toKey="research/btc-analysis" relation="related"
vault_link fromKey="research/q2-synthesis" toKey="research/eth-analysis" relation="derived_from"
vault_link fromKey="agent/market-researcher" toKey="research/q2-synthesis" relation="references"
```

---

### `vault_related`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `key` | string | yes | Vault key to find connections for |
| `relation` | string | no | Filter to only this relation type (omit to return all) |

Traverses the knowledge graph from a given entry. Returns both **outbound** links (entries this entry references) and **inbound** links (entries that reference this one), so you see the full network around any entry.

Each result includes `key`, `title`, `type`, `relation`, and `direction` (`outbound` or `inbound`).

**Example output:**
```
Related entries for `agent/market-researcher` (2)

- BTC Analysis Q2 (research/btc-analysis) [research] — outbound references
- ETH Momentum Thesis (research/eth-analysis) [research] — outbound related
```

**How the graph builds over time:** As you save research, link entries together, and spawn agents that reference vault content, the graph accumulates context automatically. `vault_related` is how you see that accumulated structure — what an agent is drawing on, what a synthesis was built from, how findings chain together.

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

### `agent_spawn`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `name` | string | yes | Unique agent name — lowercase alphanumeric with hyphens (e.g. `market-researcher`, `base-tracker`) |
| `goal` | string | yes | What this agent is trying to accomplish |
| `context` | string | no | Starting context, data, or notes the agent should carry |

Creates a persistent named agent and saves its initial state to vault at key `agent/{name}`. The agent starts with a goal, a status of `active`, and an empty update log. It survives indefinitely across sessions — recall it anytime with `agent_recall`.

**How it works:** Agent state is stored as a versioned vault entry (type `memory`). Every `agent_update` creates a new vault version, so the full history of an agent's work is preserved and diffable.

```
agent_spawn name="base-tracker" goal="monitor emerging Base chain protocols weekly"
→ Agent base-tracker spawned. Recall with agent_recall.
```

---

### `agent_recall`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `name` | string | yes | Agent name as used in `agent_spawn` |

Loads a persistent agent's full state from vault — goal, current status, vault version, last updated timestamp, and the 3 most recent progress updates with findings and next steps.

Use this at the start of a session to pick up where you left off, or to check what an agent last did before continuing its work.

```
agent_recall name="base-tracker"
→ Goal: monitor emerging Base chain protocols weekly
→ Status: active — v4
→ Last progress: found 3 new protocols — Morpho, Aerodrome v2, Seamless
→ Next: check TVL trends for each
```

---

### `agent_update`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `name` | string | yes | Agent name |
| `progress` | string | yes | What was accomplished in this update |
| `findings` | string | no | Key findings, data, or outputs from this step |
| `status` | string | no | `active` (default) \| `blocked` \| `complete` |
| `nextStep` | string | no | What should happen next — helps on the next recall |

Appends a progress entry to the agent's update log and saves a new vault version. The log keeps the last 20 updates — older entries are trimmed automatically. `nextStep` is surfaced prominently on `agent_recall` so the agent always knows where to continue.

**Status values:**
- `active` — ongoing, will continue
- `blocked` — stuck, needs input or a different approach
- `complete` — goal achieved

```
agent_update name="base-tracker" progress="analyzed Morpho TVL trend" findings="TVL up 40% in 30d, protocol is gaining traction" status="active" nextStep="check Aerodrome v2 next"
→ Agent base-tracker updated (v5)
```

**Typical session pattern:**
```
agent_recall name="base-tracker"        ← resume from last state
[do the work]
agent_update name="base-tracker" ...    ← save progress
agent_update name="base-tracker" ...    ← save more progress
```

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

### `scan_market`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `mode` | string | no | `"dips"` (default) for reversal setups, `"momentum"` for breakouts |
| `minScore` | number | no | Minimum score (default 50) |
| `minLiquidity` | number | no | Minimum liquidity in USD (default 50000) |
| `limit` | number | no | Max pools to scan (default 40) |

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

### `write_content`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `topic` | string | yes | What to write about |
| `format` | string | no | `"thread"` (default) for multi-tweet thread, `"post"` for single tweet |
| `tone` | string | no | Thread: `"alpha"`, `"educational"`, `"opinion"`, `"story"`. Post: `"hook"`, `"hot-take"`, `"alpha"`, `"question"`, `"observation"` |
| `tweets` | number | no | Number of tweets in a thread, 4–12 (default 7) |
| `long` | boolean | no | Allow up to 500 chars for post format (default false = 280 chars) |
| `voice_sample` | string | no | Your own tweets to match your voice |

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

### `noel_status`

No parameters. Full dashboard — memory usage, swarm health, active automations, recent research, and execution scores.

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
| `GITHUB_TOKEN` | Personal access token — required for private repos, recommended for higher rate limits |

---

### GitHub (8)

> Read repos, PRs, issues, files, and commits from any GitHub repository. Set `GITHUB_TOKEN` for private repos — public repos work without a token.

| Tool | Description |
|------|-------------|
| `github_list_repos` | List repos for a user or org. Leave username empty to list your own (requires token) |
| `github_list_prs` | List pull requests for a repo — open, closed, or all |
| `github_get_pr` | Full PR details: body, changed files with diffs, reviews, and comments |
| `github_list_issues` | List issues for a repo — filter by state and label |
| `github_get_issue` | Full issue details with all comments |
| `github_get_file` | Read any file from a repo — decoded content up to 10k chars |
| `github_get_commits` | Recent commits for a repo, branch, or specific file |
| `github_search_code` | Search code on GitHub with qualifiers (repo:, language:, path:, filename:) |

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
| GitHub 401 | Set `GITHUB_TOKEN` in env — required for private repos |
| GitHub 403 rate limit | Add `GITHUB_TOKEN` — unauthenticated requests have lower limits |
