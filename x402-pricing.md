# Tool Pricing

Prices are set in `app/convex/x402Mcp.ts` in the `TOOL_PRICES` map. Free tools pass through unconditionally. Paid tools require either a Noelclaw session token or a USDC micropayment on Base mainnet.

---

## Free Tools

No auth required. Always return results.

| Tool | Description |
|------|-------------|
| `get_market_data` | Live top-20 coins by market cap, trending tokens, BTC/ETH/SOL prices |
| `get_token_data` | Price, 24h change, market cap, and volume for any specific token |

---

## Paid Tools

Require a Noelclaw session token (`NOELCLAW_API_KEY`) or a per-call USDC payment. See the [Integration Guide](x402-integration.md) for how to authenticate.

### $0.25 USDC

| Tool | Description |
|------|-------------|
| `swap_tokens` | Swap ETH, USDC, USDT, DAI, WETH on Base mainnet via 0x Permit2 |
| `send_token` | Send ETH or any ERC-20 token to any address on Base mainnet |

### $0.10 USDC

| Tool | Description |
|------|-------------|
| `ask_noel` | Chat with Noel AI — DeFi analysis, trade ideas, market outlook with live context |
| `get_insight` | On-demand crypto + macro briefing via Grok — BTC/ETH action, narratives, what's moving on X |
| `start_swarm` | Start the multi-agent swarm for autonomous market monitoring and workflow execution |
| `miroshark_simulate` | Run a multi-agent social simulation for any scenario |

### $0.05 USDC

| Tool | Description |
|------|-------------|
| `create_automation` | Create a DCA, price alert, or conditional buy/sell in plain English |
| `vault_save` | Save or update an artifact to persistent vault memory |

### $0.01 USDC

| Tool | Description |
|------|-------------|
| `list_automations` | List all automations with status, run counts, and next scheduled run |
| `pause_automation` | Pause or resume an automation by ID |
| `delete_automation` | Permanently delete an automation |
| `stop_swarm` | Stop the active swarm session |
| `get_swarm_status` | Active agents, shared memory snapshot, execution scores |
| `write_swarm_memory` | Write a key-value entry to swarm's shared memory |
| `get_swarm_memory` | Read a value from swarm's shared memory by key |
| `get_execution_scores` | Skill success rates, win/loss counts, avg duration |
| `set_telegram` | Connect Telegram for push notifications |
| `vault_read` | Read a vault entry by key |
| `vault_list` | List vault entries |
| `vault_search` | Full-text search across the vault |
| `miroshark_status` | Poll simulation status |

---

## Notes

- Prices are in USDC on Base mainnet (chain ID 8453)
- Noelclaw account holders (`NOELCLAW_API_KEY`) never pay x402 — their session is used directly
- USDC contract: `0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913`
- Each tool call requires a separate payment and unique `requestId`
- Heavy users can enable BYOK in noelclaw.com → Settings to route LLM calls to your own API key
