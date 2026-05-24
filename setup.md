# How to Setup

Noelclaw's MCP server runs via `npx` — no build step, no cloning, no local files needed. One command and all 16 tools are available in any MCP-compatible AI client.

**Requirement:** Node.js >= 18 installed on your machine. That's it.

---

## Claude Code

```bash
claude mcp add noelclaw -- npx @noelclaw/research
```

Verify:
```bash
claude mcp list
# noelclaw   npx @noelclaw/research
```

Done. Use any Noel tool directly in conversation:
```
get_market_data
get_latest_signal token: BTC
research query: "Latest BTC news and market outlook"
```

---

## Claude Desktop

**Mac** — Edit `~/Library/Application Support/Claude/claude_desktop_config.json`

**Windows** — Edit `%APPDATA%\Claude\claude_desktop_config.json`

```json
{
  "mcpServers": {
    "noelclaw": {
      "command": "npx",
      "args": ["@noelclaw/research"]
    }
  }
}
```

Save the file, then **restart Claude Desktop**. Tools appear automatically in the tool picker when starting a new conversation.

---

## Cursor

### Via Settings UI

1. Open Cursor → **Settings** (Ctrl+, / Cmd+,)
2. Search **MCP** or go to **Features → MCP**
3. Add server:
   - Name: `noelclaw`
   - Command: `npx`
   - Args: `@noelclaw/research`

### Via Config File

Edit `~/.cursor/mcp.json` (create if it doesn't exist):

```json
{
  "mcpServers": {
    "noelclaw": {
      "command": "npx",
      "args": ["@noelclaw/research"]
    }
  }
}
```

Restart Cursor. Tools appear in Composer (Agent mode).

---

## Windsurf

Edit `~/.windsurf/mcp_config.json`:

```json
{
  "mcpServers": {
    "noelclaw": {
      "command": "npx",
      "args": ["@noelclaw/research"]
    }
  }
}
```

Restart Windsurf.

---

## Hermes

### CLI (fastest)

```bash
hermes mcp add noelclaw --command npx --args @noelclaw/research
```

Then reload:
```
/reload-mcp
```

### Config File

Edit `~/.hermes/config.yaml`:

```yaml
mcp_servers:
  noelclaw:
    command: npx
    args:
      - "@noelclaw/research"
    timeout: 30
    connect_timeout: 10
```

Run `/reload-mcp` in any Hermes session.

---

## Any MCP-Compatible Client

Use this generic config anywhere that accepts `command / args / env`:

```json
{
  "command": "npx",
  "args": ["@noelclaw/research"]
}
```

With optional custom backend:

```json
{
  "command": "npx",
  "args": ["@noelclaw/research"],
  "env": {
    "NOELCLAW_CONVEX_URL": "https://your-deployment.convex.site"
  }
}
```

---

## First Steps After Install

### 1. Check tools are loaded

In any client that supports tool listing:
```
list all noelclaw tools
```
Should show 16 tools.

### 2. Get live market data

```
get_market_data
```

### 3. Set up Telegram (optional)

To receive signals, whale alerts, and research reports directly in Telegram:

```
set_telegram(
  userId: "pick-any-id",
  telegramBotToken: "your-bot-token",
  telegramChatId: "your-chat-id"
)
```

How to get your Telegram credentials:
1. Open Telegram → search **@BotFather** → `/newbot` → copy the token
2. Start a chat with your new bot → send any message
3. Visit `https://api.telegram.org/bot<TOKEN>/getUpdates` → copy the `chat.id`

### 4. Get your wallet

```
get_portfolio userId: "your-id"
```

Auto-creates an encrypted Base mainnet wallet on first call. Returns your wallet address and token balances.

### 5. Research a topic

```
research query: "What is happening with Ethereum this week?"
```

Noel searches the web and returns a structured analysis with overview, key findings, market impact, and sentiment.

---

## Troubleshooting

| Problem | Fix |
|---------|-----|
| Tools not showing | Restart your MCP client after adding the config |
| `npx: command not found` | Install Node.js 18+ from [nodejs.org](https://nodejs.org) |
| `Noelclaw API error: 404` | Wrong `NOELCLAW_CONVEX_URL` or Convex not deployed |
| Server starts but no response | Normal — MCP server waits for stdin, not HTTP |
| `BANKR_API_KEY not set` | Set via `npx convex env set BANKR_API_KEY "..."` in the Convex project |
| Slow first start | `npx` downloads the package on first run (~2s). Subsequent starts are instant from cache |
