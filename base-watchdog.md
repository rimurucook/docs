# Base Watchdog

Persistent monitoring for any token on Base — treasury, holders, supply health, ecosystem news, and automated alerts, running server-side even after you close the chat. Works with any ERC-20 today, and with **B20 tokens** (Base's native compliance standard for stablecoins and RWAs, live on Base mainnet since July 8, 2026) with no changes needed.

Ships as a standalone skill: [`base-watchdog/SKILL.md`](https://github.com/noelclaw/noelapp/blob/main/base-watchdog/SKILL.md) in the Noelclaw repo.

**Requires:** [`@noelclaw/mcp`](https://www.npmjs.com/package/@noelclaw/mcp)

```bash
npx -y @noelclaw/mcp@3.32.5 install
noelclaw login
```

---

## Treasury Monitor

```
get_wallet_balance address="YOUR_TREASURY_ADDRESS"
base_mcp_status

create_automation
  rawInput: "Alert me if ETH balance at YOUR_TREASURY_ADDRESS drops below 10 ETH"

agent_spawn
  name: "treasury-watch"
  goal: "Monitor treasury wallet. Track ETH balance, flag large moves, summarize weekly."
  context: "Treasury: YOUR_TREASURY_ADDRESS"
```

## Holder & Supply Health

```
get_token_data question="YOUR_TOKEN_SYMBOL"
check_token address="YOUR_TOKEN_ADDRESS"
score_token address="YOUR_TOKEN_ADDRESS"

schedule_research
  topic: "YOUR_TOKEN_SYMBOL token holder distribution, supply health, and on-chain metrics on Base"
  schedule: "weekly-monday"
```

`check_token` runs a GoPlusLabs security audit (honeypot, mint/freeze authority, LP lock %, buy/sell tax, holder count). `score_token` runs a 6-component dip-reversal score against DexScreener data.

> **Quote-currency tokens:** `score_token` resolves the highest-liquidity Base pair for the address you pass in, and only scores it when your token is the *base* side of that pair. Stablecoins and RWA tokens (the B20 use case) are often the *quote* side of their most liquid pair — if so, the tool returns an explicit error rather than silently scoring the wrong asset.

## Ecosystem Updates & Weekly Reports

```
deep_research
  query: "YOUR_TOKEN_SYMBOL Base ecosystem updates announcements this week"
  depth: "deep"
  saveToVault: true

vault_save key: "watchdog/ecosystem-digest" content: "<digest>" type: "research"
vault_history key: "watchdog/ecosystem-digest"

schedule_research
  topic: "YOUR_TOKEN_SYMBOL weekly report: treasury, holders, ecosystem news"
  schedule: "weekly-monday"
```

## Automated Alerts

```
create_automation rawInput: "Alert me when YOUR_TOKEN_SYMBOL price drops more than 15% in 24h"
create_automation rawInput: "Alert me when YOUR_TOKEN_SYMBOL 24h volume exceeds $500k"

list_automations
get_automation_runs
```

---

## Full Setup

```
memory_context topic="YOUR_TOKEN_SYMBOL"
base_mcp_status
get_wallet_balance address="YOUR_TREASURY_ADDRESS"
get_token_data question="YOUR_TOKEN_SYMBOL"
check_token address="YOUR_TOKEN_ADDRESS"

agent_spawn
  name: "token-watchdog"
  goal: "Monitor YOUR_TOKEN_SYMBOL on Base: treasury, holders, ecosystem. Save weekly reports to vault. Flag anomalies."
  context: "Treasury: YOUR_TREASURY_ADDRESS | Token: YOUR_TOKEN_ADDRESS"

deep_research query: "YOUR_TOKEN_SYMBOL Base token: current status, treasury, holder metrics, recent news" depth: "deep" saveToVault: true
schedule_research topic: "YOUR_TOKEN_SYMBOL weekly report" schedule: "weekly-monday"
create_automation rawInput: "Alert me when YOUR_TOKEN_SYMBOL price drops more than 20% in 24h"
```

## Resume a Session

```
memory_context topic="YOUR_TOKEN_SYMBOL"
agent_recall name="token-watchdog"
vault_read key="watchdog/weekly-report"
chronicle_stats days=30
```

---

## Tool Reference

| Goal | Tool | Params |
|---|---|---|
| Treasury balance | `get_wallet_balance` | `address="0x..."` |
| Base connection check | `base_mcp_status` | — |
| Token price + market data | `get_token_data` | `question="SYMBOL"` |
| Holder count + safety audit | `check_token` | `address="0x..."` |
| Buy signal score | `score_token` | `address="0x..."` |
| Ecosystem research | `deep_research` | `query="..."` |
| Price / balance alert | `create_automation` | `rawInput="..."` |
| Weekly auto-research | `schedule_research` | `topic`, `schedule`, `label` |
| Persistent monitor agent | `agent_spawn` | `name`, `goal`, `context` |
| Recall agent | `agent_recall` | `name="..."` |
| Save report | `vault_save` | `key`, `content`, `type` |
| Restore session context | `memory_context` | `topic="..."` |

## See Also

- [Wallet & DeFi](wallet-defi.md)
- [All 108 Tools](mcp-server.md)
- [B20 Token Standard — Base Docs](https://docs.base.org/get-started/launch-b20-token)
