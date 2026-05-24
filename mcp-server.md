# MCP Server — Noelclaw Skill

The `@noelclaw/research` MCP server exposes Noel's tools to any MCP-compatible AI client. Install once via `npx` — no build step, no config required — and get live crypto signals, market data, on-chain DeFi, autonomous research, and a multi-agent swarm from Claude, Hermes, Cursor, or any other MCP host.

```bash
npx @noelclaw/research@latest
```

---

## Available Tools

### Market & Signals

| Tool | Description |
|------|-------------|
| `get_market_data` | Live top-20 coins by market cap, trending, BTC/ETH/SOL prices |
| `get_token_data` | Price, 24h change, market cap, and volume for any specific token |
| `get_latest_signal` | Latest BTC/ETH 4H trading signals — entry, TP, SL, confidence score |
| `get_signal_history` | Signal history with win/loss record and winrate stats |
| `get_smart_money_alerts` | Smart money / insider wallet movements for micro-cap tokens |
| `get_daily_recap` | Today's trading performance recap with AI review |

### Research & AI

| Tool | Description |
|------|-------------|
| `research` | On-demand web-search backed crypto analysis — overview, key findings, market impact, sentiment |
| `ask_noel` | Chat with Noel — DeFi AI with live market context |
| `get_insight` | On-demand crypto + macro briefing powered by Grok — what's happening right now |

### Wallet & DeFi

| Tool | Description |
|------|-------------|
| `get_portfolio` | Wallet address and full token balances on Base mainnet with USD values |
| `swap_tokens` | Swap ETH/USDC/USDT/DAI/WETH on Base via 0x Permit2, human-readable amounts |
| `send_token` | Send ETH or ERC-20 to any address on Base mainnet |
| `deploy_token` | Launch a memecoin on Base via Flaunch. Sets your revenue share (default 80% of swap fees). Returns Memestream NFT that earns ETH from every swap forever. |
| `claim_fees` | Claim accumulated ETH from your Flaunch token swap fees. No params needed. |
| `mint_nft` | Auto-mint any NFT on Base. Pass URL or contract address — Noel detects the contract, checks eligibility and balance, mints from your wallet. |

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
| `start_swarm` | Start the multi-agent swarm |
| `stop_swarm` | Stop the swarm |
| `get_swarm_status` | Active agents, shared memory snapshot, execution scores |
| `write_swarm_memory` | Write a key-value entry to shared memory (optional TTL) |
| `get_swarm_memory` | Read a shared memory entry by key |
| `get_execution_scores` | Skill success rates, win/loss, avg duration, last adapted |

---

## How It Works

```
AI Client (Claude / Cursor / Hermes)
    │
    │  MCP protocol (stdio)
    ▼
@noelclaw/research (Node.js via npx)
    │
    │  HTTPS — retries on 429/5xx
    ▼
https://api.noelclaw.xyz          ← Cloudflare Worker (rate limit + CORS)
    │
    │  proxied with all headers
    ▼
https://[convex].convex.site      ← Convex backend (hidden from users)
    │
    ├── /mcp/market              → Market data
    ├── /mcp/chat                → Noel / CoinGecko agent
    ├── /mcp/insight             → Grok-powered briefing
    ├── /mcp/research            → Web-search analysis (Bankr Agent)
    ├── /signals/latest          → BTC/ETH signals
    ├── /signals/history         → Signal history
    ├── /signals/winrate         → Winrate stats
    ├── /whales/latest           → Smart money alerts
    ├── /recap/today             → Daily recap
    ├── /mcp/defi/portfolio      → Wallet balances
    ├── /mcp/defi/swap           → 0x Permit2 swap quote
    ├── /mcp/defi/send           → Token send
    ├── /mcp/token/deploy        → Flaunch token deploy tx
    ├── /mcp/token/claim         → Flaunch fee claim tx
    ├── /mcp/nft/mint            → NFT auto-mint tx
    ├── /automations/*           → CRUD + list
    ├── /swarm/*                 → Swarm start/stop/status/memory/scores
    └── /user/telegram/notify    → Per-user Telegram delivery
```

The MCP server is a typed proxy — no API keys stored locally. All secrets live in Convex environment variables.

---

## Quick Install

### Claude Code

```bash
claude mcp add noelclaw -- npx @noelclaw/research@latest
```

### Claude Desktop

Edit `~/Library/Application Support/Claude/claude_desktop_config.json` (Mac) or `%APPDATA%\Claude\claude_desktop_config.json` (Windows):

```json
{
  "mcpServers": {
    "noelclaw": {
      "command": "npx",
      "args": ["@noelclaw/research@latest"]
    }
  }
}
```

Restart Claude Desktop. Tools appear automatically.

### Cursor / Windsurf

```json
{
  "mcpServers": {
    "noelclaw": {
      "command": "npx",
      "args": ["@noelclaw/research@latest"]
    }
  }
}
```

### Hermes Agent

```yaml
mcp_servers:
  noelclaw:
    command: npx
    args:
      - "@noelclaw/research@latest"
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
| `question` | string | yes | Token name or natural-language query, e.g. `"show me ETH and SOL"` |

---

### `get_latest_signal`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `token` | string | no | `"BTC"`, `"ETH"`, or omit for both |

Returns the latest 4H signal generated at 08:00 UTC — entry price, TP1, TP2, stop loss, and confidence score.

---

### `get_signal_history`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `token` | string | no | `"BTC"` or `"ETH"` |
| `days` | number | no | Days to look back (default: 7) |

---

### `get_smart_money_alerts`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `hours` | number | no | How far back to look (default: 24) |

Returns large on-chain movements, smart money accumulation, and CEX inflow/outflow patterns.

---

### `get_daily_recap`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `date` | string | no | `YYYY-MM-DD` (default: today UTC) |

---

### `research`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `query` | string | yes | Topic to research, e.g. `"Ethereum ETF approval impact"` |

Returns structured analysis: overview, key findings, market impact, affected tokens, sentiment, and what to watch.

---

### `ask_noel`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `question` | string | yes | Your question or prompt |
| `messages` | array | no | Prior conversation turns for context |

---

### `get_insight`

No parameters. Returns an on-demand crypto + macro briefing generated by Grok: BTC/ETH price action, macro events, and trending narratives on X/Twitter.

---

### `get_portfolio`

No parameters. Returns your Base mainnet wallet address and full token balances with USD values. Auto-creates a secure encrypted wallet on first use.

The wallet is stored at `~/.noelclaw/wallet.json` on your machine, encrypted with a machine-derived key.

---

### `swap_tokens`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `fromToken` | string | yes | Token to sell: `ETH`, `USDC`, `USDT`, `DAI`, `WETH` |
| `toToken` | string | yes | Token to buy |
| `amount` | string | yes | Human-readable amount, e.g. `"0.01"` for 0.01 ETH, `"10"` for 10 USDC |

Routes through 0x Permit2 on Base mainnet (chainId 8453). Transaction is signed and broadcast locally from your wallet — the MCP server signs it, not Convex.

---

### `send_token`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `token` | string | yes | `ETH`, `USDC`, `USDT`, `DAI`, or `WETH` |
| `toAddress` | string | yes | Recipient address (`0x...`) |
| `amount` | string | yes | Human-readable amount, e.g. `"5"` for 5 USDC |

---

### `deploy_token`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `name` | string | yes | Token name, e.g. `"Pepe Noel"` |
| `symbol` | string | yes | Ticker, 3–6 chars, e.g. `"PNOEL"` |
| `imageUrl` | string | yes | Public image URL for the token |
| `description` | string | no | Token description |
| `initialMarketCapUSD` | number | no | Starting market cap in USD (default: 10000, min: 1000) |
| `creatorFeePercent` | number | no | Your % of swap fees (default: 80, max: 100) |
| `preminePercent` | number | no | % of supply to premine at launch (default: 0, max: 50) |
| `fairLaunchDurationMinutes` | number | no | Fair launch window in minutes (default: 30) |

Returns unsigned tx data. The MCP server signs and broadcasts locally from `~/.noelclaw/wallet.json`. After launch, your wallet holds a Memestream NFT that earns ETH from every swap on the token forever.

---

### `claim_fees`

No parameters. Calls `claim()` on the Flaunch PositionManager — pulls all pending ETH from your deployed tokens to your wallet.

---

### `mint_nft`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `mintUrl` | string | yes | NFT mint page URL (OpenSea, Zora, Highlight) or raw contract address (`0x...`) |
| `quantity` | number | no | How many to mint (default: 1, max: 100) |

Detects the contract from the URL, fetches the verified ABI from Basescan (if available), checks your max-per-wallet eligibility and ETH balance, then returns signed tx data minted from your local wallet.

---

### `create_automation`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `rawInput` | string | yes | Plain English description of the automation |

Examples:
- `"Buy 50 USDC of ETH every day. Stop after spending 500 USDC."`
- `"If ETH drops 5% from current price, buy $100 of ETH"`
- `"Alert me when BTC dominance drops below 50%"`
- `"Sell 20% of my ETH if it's up 3x from current price"`

---

### `list_automations`

No parameters. Returns all automations (active, paused, completed) with status, run counts, total spent, and next scheduled run.

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

Permanent — cannot be undone.

---

### `start_swarm`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `config.enabledAgents` | string[] | no | Agent IDs to start (default: all 5) |
| `config.byok` | boolean | no | Use your own `BANKR_API_KEY` from env |

Available agent IDs: `market-monitor`, `sentiment-tracker`, `workflow-executor`, `memory-manager`, `risk-verifier`

---

### `stop_swarm`

No parameters.

---

### `get_swarm_status`

No parameters. Returns active job status, shared memory snapshot (up to 5 entries), and top execution scores.

---

### `write_swarm_memory`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `agentId` | string | yes | ID of the agent writing this entry |
| `key` | string | yes | Memory key |
| `value` | string | yes | Value (JSON-serializable string) |
| `ttlSeconds` | number | no | Auto-delete after this many seconds |

---

### `get_swarm_memory`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `key` | string | yes | Memory key to read |

---

### `get_execution_scores`

No parameters. Returns all skill scores sorted by performance: success rate, win/loss counts, average duration, and when thresholds were last adapted.

---

## Environment Variables

No env vars are required for basic use. Set these to unlock additional features:

| Var | Purpose |
|-----|---------|
| `NOELCLAW_API_KEY` | Link to your noelclaw.xyz account (`noel_sk_xxx` from Settings → API Keys) |
| `ALCHEMY_API_KEY` | Faster, more reliable swap quotes and portfolio lookups on Base |
| `GROK_API_KEY` | Your own X.AI key for `get_insight` and signal generation |
| `BANKR_API_KEY` | Your own Bankr key for swarm agents and research |
| `TELEGRAM_BOT_TOKEN` | Your Telegram bot token (from @BotFather) |
| `TELEGRAM_CHAT_ID` | Your Telegram chat ID |

> **Telegram is optional.** It's only needed if you want push notifications outside your AI client. If you use Noelclaw through Claude, Hermes, Cursor, or any MCP client, you already get all results inline — no Telegram setup needed.

---

## Troubleshooting

| Error | Fix |
|-------|-----|
| Tools not appearing | Restart your MCP client after adding the config |
| `Noelclaw API error: 404` | Wrong `NOELCLAW_CONVEX_URL` or Convex not deployed |
| Server starts but no response | Normal — it waits for MCP stdin, not HTTP |
| Research times out | Try again — Bankr LLM gateway may be under load |
| Swap or send fails | Check your wallet has enough balance with `get_portfolio` |
| `get_swarm_status` returns empty | Start the swarm first with `start_swarm` |
| Rate limit (429) | The server retries automatically up to 3 times with backoff |
