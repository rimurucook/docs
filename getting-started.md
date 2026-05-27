# Quick Start

---

## Option A — Use the Hosted Platform

Go to [noelclaw.com](https://noelclaw.com), sign up, and you're in. No setup needed.

To add the MCP skill to your AI client (Claude, Cursor, Hermes, etc.), see [How to Setup](setup.md).

---

## Option B — Run Locally (Self-Hosted)

### Prerequisites
- Node.js >= 18
- npm or pnpm
- Convex account (free at convex.dev)

### 1. Install dependencies

```bash
cd noelapp/app
npm install
```

### 2. Set up Convex

```bash
npx convex dev
# Follow the prompts to log in and create a deployment
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

### 4. Start the frontend

```bash
npm run dev
# App runs at http://localhost:5173
```

### 5. Deploy Convex functions

```bash
npx convex deploy
```

---

## First Things to Try

After installing the MCP skill (see [How to Setup](setup.md)):

**Get live market data:**
```
get_market_data
```

**Get a fresh crypto + macro briefing:**
```
get_insight
```

**Ask Noel:**
```
ask_noel: "What's your read on the market this week?"
```

**Research a topic:**
```
research query: "What is happening with Ethereum ETFs?"
```

Noel searches the web and returns a structured analysis in seconds.

**Swap tokens:**
```
swap_tokens fromToken: ETH toToken: USDC amount: "0.01"
```

**Save something to the vault:**
```
vault_save key: "eth-thesis" content: "ETH is..." type: "research"
```

**Run a scenario simulation:**
```
miroshark_simulate scenario: "How would markets react if the Fed cuts rates by 100bps?"
```

**Claim your token fees:**
```
claim_fees
```

**Humanize AI-generated text:**
```
humanize_text text: "In conclusion, it is imperative to..."
```

---

## Key Pages (Platform UI)

| Page | Path | What It Does |
|------|------|-------------|
| Dashboard | `/` | Overview, quick stats |
| Chat | `/chat` | Talk to any agent |
| Brain | `/brain` | Noel's research activity log |
| Automations | `/automations` | Enable/configure skills |
| Arcade | `/arcade` | Play games, earn credits |
| Wallet | `/wallet` | Manage tokens, send/swap |
| Agent Hub | `/marketplace` | Browse 40+ agents |
| Profile | `/profile` | Account settings, Telegram link |
| Build | `/build` | NoelBuild — AI app generator |
| Swarm | `/swarm` | Multi-agent swarm dashboard |
