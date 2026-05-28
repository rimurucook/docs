# Getting Started

---

## Step 1 — Sign Up on the Platform

Go to [noelclaw.com](https://noelclaw.com) and create an account. The platform gives you access to the web UI for managing automations, viewing trading signals, and tracking your wallet.

---

## Step 2 — Install the MCP Skill

The MCP skill runs via `npx` — no install step, no files to clone.

**Requirement:** Node.js >= 18. Check with `node --version`.

### Claude Code

```bash
claude mcp add noelclaw -- npx @noelclaw/mcp
```

### Claude Desktop / Cursor / Windsurf

Add to your MCP config file:

```json
{
  "mcpServers": {
    "noelclaw": {
      "command": "npx",
      "args": ["@noelclaw/mcp"]
    }
  }
}
```

Config file locations:
- Claude Desktop (Mac): `~/Library/Application Support/Claude/claude_desktop_config.json`
- Claude Desktop (Windows): `%APPDATA%\Claude\claude_desktop_config.json`
- Cursor: `~/.cursor/mcp.json`
- Windsurf: `~/.windsurf/mcp_config.json`

Restart your client after saving.

For full setup guides including Hermes and Aeon, see [Setup & Install](setup.md).

---

## Step 3 — First Things to Try

### Get live market data

```
get_market_data
```

Returns the top-20 coins by market cap, BTC/ETH/SOL prices, and trending tokens.

### Get a crypto briefing

```
get_insight
```

On-demand analysis via Grok — BTC/ETH price action, macro context, and what's moving on X.

### Ask Noel

```
ask_noel question: "What's your read on the ETH/BTC ratio right now?"
```

Noel AI with live market context for DeFi analysis.

### Check your wallet address

```
get_wallet_address
```

Returns your local wallet address. Keys are stored at `~/.noelclaw/wallet.json` and never leave your machine.

### Swap tokens on Base

```
swap_tokens fromToken: "ETH" toToken: "USDC" amount: "0.01"
```

Routes through 0x Permit2 on Base mainnet, signed locally.

### Save something to the vault

```
vault_save key: "eth-thesis" content: "ETH is undervalued because..." type: "note"
```

Auto-versioned persistent storage accessible across any MCP session.

### Run a MiroShark simulation

```
miroshark_simulate scenario: "How would markets react if the Fed cuts rates 100bps?"
```

Returns a multi-agent social simulation with belief propagation and behavioral analysis.

### Humanize AI-generated text

```
humanize_text text: "In conclusion, it is important to note that..."
```

Strips AI writing patterns using MiniMax-M2.7. Requires `MINIMAX_API_KEY` env var.

---

## Step 4 — Connect Telegram (Optional)

Receive trading signals, smart money alerts, and swarm events in Telegram.

```
set_telegram telegramBotToken: "your-bot-token" telegramChatId: "your-chat-id"
```

To get credentials:
1. Open Telegram → search **@BotFather** → `/newbot` → copy the token
2. Start a chat with your new bot
3. Visit `https://api.telegram.org/bot<TOKEN>/getUpdates` → copy the `chat.id` value

Signals arrive daily at 08:00 UTC. Smart money alerts fire hourly for Base micro-caps under $100k mcap.

---

## What's Next

- [Full setup guides for all clients](setup.md)
- [Complete MCP tool reference](mcp-server.md)
- [Swarm, vault, and framework docs](agents.md)
- [MiroShark simulation](miroshark.md)
- [Aeon integration](aeon.md)
