# Tool Pricing

Prices are set in `app/convex/x402Mcp.ts` in the `TOOL_PRICES` map. Free tools pass through unconditionally. Paid tools require either a session token or a USDC micropayment on Base mainnet.

---

## Free Tools

No auth required. Always return results.

| Tool | Description |
|------|-------------|
| `get_market_data` | Live top-20 coins by market cap, trending tokens, BTC/ETH/SOL prices |
| `get_latest_signal` | Latest BTC and/or ETH 4H trading signals — entry, TP targets, stop loss, confidence score |
| `get_signal_history` | Signal history with win/loss record and winrate stats |
| `get_smart_money_alerts` | Smart money and insider wallet movements for micro-cap tokens on Base chain |
| `get_daily_recap` | Today's trading performance recap with winrate, PnL stats, and AI review |

---

## Paid Tools

Require a Noelclaw session token (`NOELCLAW_SESSION_TOKEN`) or a per-call USDC payment. See the [Integration Guide](x402-integration.md) for how to authenticate.

### $0.25 USDC

| Tool | Description |
|------|-------------|
| `swap_tokens` | Swap ETH, USDC, USDT, DAI, WETH on Base mainnet via 0x Permit2 |
| `send_token` | Send ETH or any ERC-20 token to any address on Base mainnet |

### $0.10 USDC

| Tool | Description |
|------|-------------|
| `research` | On-demand crypto research — web-search backed analysis with overview, key findings, market impact, affected tokens, sentiment, and what to watch |
| `ask_noel` | Chat with Noel AI — DeFi analysis, trade ideas, market outlook with live context |
| `start_swarm` | Start the multi-agent swarm for autonomous market monitoring, sentiment tracking, and workflow execution |

### $0.05 USDC

| Tool | Description |
|------|-------------|
| `get_token_data` | Price, 24h change, market cap, and volume for any specific token |
| `create_automation` | Create an automation in plain English — DCA, price alerts, conditional buys/sells, recurring updates |

### $0.01 USDC

| Tool | Description |
|------|-------------|
| `list_automations` | List all automations with status, run counts, and next scheduled run |
| `pause_automation` | Pause or resume an automation by ID |
| `delete_automation` | Permanently delete an automation |
| `set_telegram` | Configure personal Telegram bot for signals, whale alerts, research reports, and market data |
| `stop_swarm` | Stop the active swarm session |
| `get_swarm_status` | Active agents, shared memory snapshot, execution scores, and recent runs |
| `write_swarm_memory` | Write a key-value entry to the swarm's shared memory |
| `get_swarm_memory` | Read a value from the swarm's shared memory by key |
| `get_execution_scores` | Self-improvement scores — success rate, win/loss, avg duration per skill |

---

## Notes

- Prices are in USDC on Base mainnet (chain ID 8453)
- Noelclaw account holders never pay x402 — their credit balance is debited instead
- USDC must be the official contract: `0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913`
- Each tool call requires a separate payment and unique `requestId`
- Heavy swarm users can enable BYOK in noelclaw.com → Settings to route Bankr LLM calls to their own API key
- Prices are subject to change as Noelclaw exits the promotional period
