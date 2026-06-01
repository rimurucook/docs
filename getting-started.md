# Getting Started

---

## Step 1 — Sign Up

Go to [app.noelclaw.com](https://app.noelclaw.com) and create an account. This gives you access to the web UI for managing automations, monitoring your wallet, and chatting with Noel AI.

---

## Step 2 — Install the MCP Skill

The skill runs via `npx` — no install, no cloning, nothing to maintain.

**Requirement:** Node.js >= 18. Check with `node --version`. Download from [nodejs.org](https://nodejs.org) if needed.

### Claude Code

```bash
claude mcp add noelclaw -s user -- npx -y @noelclaw/mcp
```

### Claude Desktop

Edit your config file:
- **Mac:** `~/Library/Application Support/Claude/claude_desktop_config.json`
- **Windows:** `%APPDATA%\Claude\claude_desktop_config.json`

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

Restart Claude Desktop after saving.

### Cursor

Edit `~/.cursor/mcp.json`:

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

### Windsurf

Edit `~/.windsurf/mcp_config.json` — same JSON format as above.

→ [Full setup guide for Hermes, Aeon, and more](setup.md)

---

## Step 3 — Try It Out

### Pull live market data

```
get_market_data
```

Returns top-20 coins by market cap, BTC/ETH/SOL prices, and trending tokens.

### Ask Noel

```
ask_noel question: "What's your read on the ETH/BTC ratio right now?"
```

Noel AI with live market context — DeFi analysis, trade ideas, market outlook.

### Check your portfolio

```
get_portfolio
```

Returns your Base wallet balances and total portfolio value in USD.

### Swap tokens on Base

```
swap_tokens fromToken: "ETH" toToken: "USDC" amount: "0.01"
```

Routes through 0x Permit2 on Base mainnet, signed locally from your wallet.

### Save to the vault

```
vault_save type: "research" title: "ETH Thesis" content: "ETH is undervalued because..."
```

Persistent, auto-versioned storage accessible across any MCP session.

### Scan for dip reversals

```
scan_dips
```

Scans live markets for tokens showing early reversal signals — buy pressure rising after a 1h dip.

### Run a MiroShark simulation

```
miroshark_simulate scenario: "How would markets react if the Fed cuts rates 100bps?"
```

Multi-agent social simulation — belief propagation, persona modeling, behavioral analysis.

---

## Step 4 — Connect Telegram (Optional)

Get swarm events and automation alerts pushed to Telegram.

```
set_telegram telegramBotToken: "your-bot-token" telegramChatId: "your-chat-id"
```

To get credentials:
1. Open Telegram → search **@BotFather** → `/newbot` → copy the token
2. Start a chat with your new bot
3. Visit `https://api.telegram.org/bot<TOKEN>/getUpdates` → copy the `chat.id` value

---

## What's Next

- [Setup guide for all clients](setup.md)
- [Complete tool reference](mcp-server.md)
- [MiroShark simulation](miroshark.md)
- [Aeon integration](aeon.md)
