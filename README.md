# Noelclaw — Documentation

Noelclaw is an AI-native crypto platform built on autonomous agents, real-time market intelligence, and on-chain execution. Users chat with specialized AI agents, research any crypto topic on demand, earn credits through an arcade, manage crypto wallets, and automate DeFi workflows — all in one interface.

---

## What Noelclaw Does

| Feature | Description |
|---------|-------------|
| **AI Agents** | 40+ specialized agents for DeFi, news, market data, code, and more |
| **Noel Research** | On-demand crypto research via web search — ask about any token, event, or narrative |
| **Trading Signals** | BTC and ETH 4H signals generated daily at 08:00 UTC with full outcome tracking — Telegram delivery |
| **Smart Money Alerts** | Hourly micro-cap token monitoring on Base chain (<$100k mcap) via DexScreener — Telegram delivery |
| **Automations** | DCA, price alerts, and conditional swaps in plain English |
| **Wallet & DeFi** | Encrypted wallet, token swaps via 0x Permit2, on-chain execution on Base mainnet |
| **Swarm** | 5 coordinated agents that monitor markets, execute automations, and self-improve over time |
| **Noel Framework** | Sentinel-gated playbooks — define what your AI can and can't do, then run it with a full audit trail |
| **Noel Vault** | Persistent memory layer — save, version, diff, and search any artifact across sessions |
| **NoelBuild** | AI app generator — describe any app in plain English, get a working HTML/JS app |
| **Game to Earn** | 4 arcade games that pay out credits |
| **MiroShark** | Multi-agent social simulation — model how any scenario plays out across hundreds of agents |
| **MCP Skill** | 37 tools accessible via `npx @noelclaw/mcp@latest` in Claude, Cursor, Hermes, Windsurf, or any MCP client |

---

## Two Projects

### `noelapp/` — The Platform

The full application: React frontend + Convex serverless backend. This is the core product.

- **Frontend:** React 19 + TypeScript + TailwindCSS
- **Backend:** Convex (real-time DB + serverless actions)
- **Auth:** Noelclaw email/password + Privy (OAuth + embedded wallets)
- **Web3:** Base mainnet, USDC, ethers.js encrypted wallets, 0x Permit2, x402 payment protocol

### `noelclaw.fun/` — The Landing Page

Marketing website only — no backend, no auth. Static React with Framer Motion animations.

---

## MCP Skill — 37 Tools

The `@noelclaw/mcp` skill exposes Noel's capabilities to any MCP-compatible AI client:

| Category | Tools |
|----------|-------|
| **Market** | `get_market_data`, `get_token_data` |
| **Research** | `research`, `get_insight`, `ask_noel` |
| **DeFi** | `swap_tokens`, `send_token`, `claim_fees`, `get_wallet_address` |
| **Automations** | `create_automation`, `list_automations`, `pause_automation`, `delete_automation` |
| **Swarm** | `start_swarm`, `stop_swarm`, `get_swarm_status`, `write_swarm_memory`, `get_swarm_memory`, `get_execution_scores` |
| **Framework** | `create_task_packet`, `list_task_packets`, `list_playbooks`, `run_playbook`, `get_noel_ledger`, `get_sentinel_rules` |
| **Vault** | `vault_save`, `vault_read`, `vault_list`, `vault_search`, `vault_history`, `vault_diff`, `vault_export` |
| **MiroShark** | `miroshark_simulate`, `miroshark_status` |
| **Social** | `set_telegram`, `post_tweet` |
| **Humanizer** | `humanize_text` |

Install:
```bash
npx @noelclaw/mcp@latest
```

---

## Key Numbers

- 37 MCP tools across 12 modules
- On-demand research via Bankr LLM gateway (real-time, seconds)
- Swarm heartbeat every 5 minutes with 5 coordinated agents
- Signals generated once per day (08:00 UTC, 4H timeframe, A+ ≥70/100) — platform + Telegram
- Smart money alerts every hour — Base chain, mcap <$100k — platform + Telegram
- Base mainnet — ETH, USDC, USDT, DAI, WETH
- Vault: unlimited versioned artifacts with git-style diff and history

---

## Core Philosophy

Noelclaw is built around the idea that AI agents should work **for** users — not just respond to prompts. The Swarm monitors markets autonomously, the Framework lets you define exactly what agents are allowed to do, and the Vault gives agents persistent memory across sessions. Everything that matters gets delivered to your Telegram automatically.
