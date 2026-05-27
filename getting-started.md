# Quick Start

---

## Option A — Use the Hosted Platform

Go to [noelclaw.com](https://noelclaw.com), sign up, and you're in.

To add the MCP skill to your AI client, see [How to Setup](setup.md).

---

## Option B — Self-Hosted

### Prerequisites
- Node.js >= 18
- Convex account (free at convex.dev)

### 1. Install dependencies

```bash
cd noelapp/app
npm install
```

### 2. Set up Convex

```bash
npx convex dev
```

### 3. Set environment variables

```bash
npx convex env set BANKR_API_KEY "your-key"
npx convex env set GROK_API_KEY "your-grok-key"
npx convex env set TELEGRAM_BOT_TOKEN "your-bot-token"
npx convex env set TELEGRAM_CHAT_ID "your-chat-id"
npx convex env set WALLET_ENCRYPTION_KEY "your-strong-random-key"
npx convex env set ZX_API_KEY "your-0x-key"
```

See [Environment Variables](env-vars.md) for the full list.

### 4. Start

```bash
npm run dev         # frontend at http://localhost:5173
npx convex deploy   # deploy functions
```

---

## First Things to Try (MCP)

After installing the MCP skill:

**Live market data:**
```
get_market_data
```

**Daily crypto briefing:**
```
get_insight
```

**Ask Noel:**
```
ask_noel: "What's your read on the market this week?"
```

**Swap tokens on Base:**
```
swap_tokens fromToken: ETH toToken: USDC amount: "0.01"
```

**Save to vault:**
```
vault_save key: "eth-thesis" content: "ETH is undervalued because..." type: "note"
```

**Run a simulation:**
```
miroshark_simulate scenario: "How would markets react if the Fed cuts rates 100bps?"
```

**Humanize AI text:**
```
humanize_text text: "In conclusion, it is important to note that..."
```

---

## Key Pages (Platform UI)

| Page | Path | What It Does |
|------|------|-------------|
| Dashboard | `/` | Overview, quick stats |
| Chat | `/chat` | Talk to any agent |
| Brain | `/brain` | Research activity log |
| Automations | `/automations` | Enable/configure skills |
| Arcade | `/arcade` | Play games, earn credits |
| Wallet | `/wallet` | Manage tokens, send/swap |
| Agent Hub | `/marketplace` | Browse 40+ agents |
| Profile | `/profile` | Account settings, Telegram link |
| Build | `/build` | AI app generator |
| Swarm | `/swarm` | Multi-agent swarm dashboard |
