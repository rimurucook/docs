# Setup & Install

The `@noelclaw/mcp` package runs via `npx` — no build step, no cloning, no local files needed. One command gives you all 61 tools in any MCP-compatible AI client.

**Requirement:** Node.js >= 18. Check with `node --version`. Download from [nodejs.org](https://nodejs.org) if needed.

---

## Claude Code

```bash
claude mcp add noelclaw -s user -- npx -y @noelclaw/mcp
```

Verify the server is registered:

```bash
claude mcp list
# noelclaw   npx -y @noelclaw/mcp
```

Then in any Claude Code session:

```
get_market_data
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
      "args": ["-y", "@noelclaw/mcp"]
    }
  }
}
```

Save the file, then **restart Claude Desktop**. all 61 tools appear automatically in the tool list.

---

## Cursor

### Via Settings UI

1. Open Cursor → **Settings** (Ctrl+, / Cmd+,)
2. Search **MCP** or go to **Features → MCP**
3. Add server:
   - Name: `noelclaw`
   - Command: `npx`
   - Args: `-y @noelclaw/mcp`

### Via Config File

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

Restart Cursor. Tools appear in Composer when in **Agent** mode.

---

## Windsurf

Edit `~/.windsurf/mcp_config.json`:

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

Restart Windsurf after saving.

---

## Hermes

### CLI Method

```bash
hermes mcp add noelclaw --command npx --args -y --args @noelclaw/mcp
```

Reload without restarting:

```
/reload-mcp
```

### Config File Method

Edit `~/.hermes/config.yaml`:

```yaml
mcp_servers:
  noelclaw:
    command: npx
    args:
      - "-y"
      - "@noelclaw/mcp"
    timeout: 30
    connect_timeout: 10
```

Then run `/reload-mcp` in any Hermes session.

---

## Aeon

See [Aeon integration guide](aeon.md) for the full setup.

Quick install via Aeon skill registry:

```bash
aeon skill add noelclaw
```

Or add manually to your Aeon config — see the [Aeon page](aeon.md).

---

## Any MCP Client

If your client accepts a generic MCP server definition:

```json
{
  "command": "npx",
  "args": ["-y", "@noelclaw/mcp"]
}
```

---

## First Steps After Install

### 1. Confirm tools are loaded

Ask your AI client:

```
list all noelclaw tools
```

You should see 61 tools.

### 2. Pull live market data

```
get_market_data
```

### 3. Check your portfolio

```
get_portfolio
```

### 4. Ask Noel

```
ask_noel question: "What's your read on BTC right now?"
```

### 5. Connect Telegram (optional)

```
set_telegram telegramBotToken: "your-bot-token" telegramChatId: "your-chat-id"
```

To get credentials:
1. Open Telegram → search **@BotFather** → `/newbot` → copy the token
2. Start a chat with your bot and send any message
3. Visit `https://api.telegram.org/bot<TOKEN>/getUpdates` → copy the `chat.id`

### 6. Save something to vault

```
vault_save type: "memory" title: "First Note" content: "Started using noelclaw"
```

---

## Optional: Environment Variables

Pass env vars to unlock extra tools or use your own API keys:

```json
{
  "mcpServers": {
    "noelclaw": {
      "command": "npx",
      "args": ["-y", "@noelclaw/mcp"],
      "env": {
        "MINIMAX_API_KEY": "your-key",
        "BANKR_API_KEY": "your-key"
      }
    }
  }
}
```

See [Environment Variables](env-vars.md) for the full list.

---

## Troubleshooting

| Problem | Fix |
|---------|-----|
| Tools not showing | Restart your MCP client after saving the config |
| `npx: command not found` | Install Node.js 18+ from [nodejs.org](https://nodejs.org) |
| Server starts but no response | Normal — the MCP server waits for stdin (MCP protocol), it does not serve HTTP |
| Slow first start | `npx` downloads the package on first run. Subsequent starts are instant |
| `connect_timeout` errors | Increase to `connect_timeout: 20` in your config — first run takes longer |
| `humanize_text` fails | Requires `MINIMAX_API_KEY` env var |
| Coder tools fail | Requires `BANKR_API_KEY` env var |
