# Quick Start

---

## Option A — Use the Hosted Platform

Go to [noelclaw.xyz](https://noelclaw.xyz), sign up, and you're in. No setup needed.

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

**Get the latest BTC signal:**
```
get_latest_signal token: BTC
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

**Check your portfolio:**
```
get_portfolio
```

Auto-creates an encrypted Base mainnet wallet on first use. Wallet is stored locally at `~/.noelclaw/wallet.json`.

**Swap tokens:**
```
swap_tokens fromToken: ETH toToken: USDC amount: "0.01"
swap_tokens fromToken: ETH toToken: USDC amount: "50%"
```

Amounts are human-readable (`"0.01"` = 0.01 ETH) or a percentage of your balance (`"50%"`, `"100%"`).

**Launch a memecoin:**
```
deploy_token name: "Pepe Noel" symbol: "PNOEL" imageUrl: "https://..."
```

Deploys via Flaunch on Base. You earn 80% of swap fees forever via your Memestream NFT.

**Auto-mint an NFT:**
```
mint_nft mintUrl: "https://zora.co/collect/base:0x..."
```

Pass any mint page URL or raw contract address. Noel detects the contract, checks your eligibility and balance, and mints from your wallet.

**Claim your token fees:**
```
claim_fees
```

Pulls all pending ETH from your deployed Flaunch tokens to your wallet.

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
