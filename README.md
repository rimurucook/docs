# Noelclaw — Documentation

Noelclaw is an AI-native crypto platform built on autonomous agents, real-time market intelligence, and on-chain execution. Users chat with specialized AI agents, research any crypto topic on demand, earn credits through an arcade, manage crypto wallets, and automate DeFi workflows — all in one interface.

---

## What Noelclaw Does

| Feature | Description |
|---------|-------------|
| **AI Agents** | 40+ specialized agents for DeFi, news, market data, code, and more |
| **Noel Research** | On-demand crypto research via web search — ask about any token, event, or narrative |
| **Trading Signals** | BTC and ETH 4H signals generated daily at 08:00 UTC with full outcome tracking |
| **Whale Tracking** | Hourly smart money alerts on Base chain — micro-cap tokens (<$100k mcap) via DexScreener |
| **Automations** | DCA, price alerts, and conditional swaps in plain English |
| **Wallet & DeFi** | Encrypted wallet, token swaps via 0x Permit2, on-chain execution on Base mainnet |
| **Token Launch** | Deploy memecoins on Base via Flaunch — earn 80% of swap fees forever via Memestream NFT |
| **NFT Minting** | Auto-mint any FCFS NFT on Base from a URL — detects contract, checks eligibility, mints from your wallet |
| **Swarm** | 5 coordinated agents that monitor markets, execute automations, and improve over time |
| **Steward Layer** | Mechanical safety gates on every agent action — territory, value limits, grudge book, rate limit |
| **NoelBuild** | AI app generator — describe any app in plain English, get a working HTML/JS app |
| **Game to Earn** | 4 arcade games that pay out credits |
| **Noel Framework** | Sentinel-gated agent execution — define what your AI can and can't do before it runs |
| **MCP Skill** | 43 tools accessible via `npx @noelclaw/mcp@latest` in Claude, Cursor, Hermes, Windsurf, or any MCP client |

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

## Core Features

| Feature | Description |
|---------|-------------|
| **4H Trading Signals** | BTC and ETH signals generated daily at 08:00 UTC with entry, TP, SL, confidence score (A+ ≥70), Volume Profile, and outcome tracking |
| **Smart Money Alerts** | Hourly micro-cap token tracking on Base (<$100k mcap) — buy/sell flow, volume spikes, early accumulation via DexScreener |
| **Weekly Recap** | Every Sunday: full 7-day signal log, win/loss stats, AI performance review, Telegram delivery |
| **On-Demand Research** | Ask about any token, protocol, or market event — web-search backed analysis with structured findings |
| **DeFi Wallet** | Encrypted Base mainnet wallet auto-created on first use. Swap via 0x Permit2, send ETH/ERC-20 |
| **Telegram Delivery** | Signals, whale alerts, research reports, and weekly recaps delivered to your personal Telegram bot |
| **MCP Skill** | 43 tools accessible via MCP in Claude Desktop, Claude Code, Cursor, Hermes, and any MCP client |

---

## Key Numbers

- 43 MCP tools
- Token launch via Flaunch — 80% swap fees to deployer via Memestream NFT
- Signals generated once per day (08:00 UTC, 4H timeframe, A+ ≥70/100)
- 6-hour signal expiry window, 2:1 R:R minimum enforced
- On-demand research via Bankr gpt-5.4-mini (real-time, seconds)
- Smart money alerts every hour — Base chain, mcap <$100k, DexScreener data
- Swarm heartbeat every 5 minutes with 5 coordinated agents
- Weekly recap every Sunday at 23:55 UTC
- Base mainnet — ETH, USDC, USDT, DAI, WETH

---

## Core Philosophy

Noelclaw is built around the idea that AI agents should work **for** users — not just respond to prompts. Noel generates signals on a schedule, tracks whale movements, and researches any crypto topic on demand. Everything relevant is delivered to your Telegram automatically. The Steward layer ensures agents only do what they're supposed to do.
