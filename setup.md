# How to Setup

Noelclaw's MCP server runs via `npx` — no build step, no cloning, no local files needed. One command and all 36 tools are available in any MCP-compatible AI client.

**Requirement:** Node.js >= 18.

---

## Claude Code

```bash
claude mcp add noelclaw -- npx @noelclaw/mcp
```

Verify:
```bash
claude mcp list
# noelclaw   npx @noelclaw/mcp
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
      "args": ["@noelclaw/mcp"]
    }
  }
}
```

Restart Claude Desktop.

---

## Cursor

Edit `~/.cursor/mcp.json`:

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

Or via Settings → Features → MCP → Add Server (Name: `noelclaw`, Command: `npx`, Args: `@noelclaw/mcp`).

---

## Windsurf

Edit `~/.windsurf/mcp_config.json`:

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

---

## Hermes

### CLI

```bash
hermes mcp add noelclaw --command npx --args @noelclaw/mcp
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
      - "@noelclaw/mcp"
    timeout: 30
    connect_timeout: 10
```

---

## Any MCP Client

```json
{
  "command": "npx",
  "args": ["@noelclaw/mcp"]
}
```

---

## First Steps After Install

### 1. Check tools are loaded

```
list all noelclaw tools
```

Should show 36 tools.

### 2. Get live market data

```
get_market_data
```

### 3. Set up Telegram (optional)

```
set_telegram telegramBotToken: "your-bot-token" telegramChatId: "your-chat-id"
```

How to get credentials:
1. Open Telegram → search **@BotFather** → `/newbot` → copy the token
2. Start a chat with your bot → send any message
3. Visit `https://api.telegram.org/bot<TOKEN>/getUpdates` → copy the `chat.id`

### 4. Research a topic

```
ask_noel: "What is happening with Ethereum this week?"
```

### 5. Save to vault

```
vault_save key: "eth-notes" content: "My thoughts on ETH..." type: "note"
```

---

## Troubleshooting

| Problem | Fix |
|---------|-----|
| Tools not showing | Restart your MCP client after adding the config |
| `npx: command not found` | Install Node.js 18+ from [nodejs.org](https://nodejs.org) |
| Server starts but no response | Normal — MCP server waits for stdin, not HTTP |
| Slow first start | `npx` downloads the package on first run (~2s). Subsequent starts are instant |
