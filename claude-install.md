# Install on Claude

Add Noelclaw to Claude Desktop or Claude Code. No build step - runs via `npx`.

**Requirement:** Node.js >= 18. Check with `node --version`. Download from [nodejs.org](https://nodejs.org) if needed.

---

## Claude Desktop

### 1. Find your config file

**Mac:**
```
~/Library/Application Support/Claude/claude_desktop_config.json
```

**Windows:**
```
%APPDATA%\Claude\claude_desktop_config.json
```

Open it in any text editor (Notepad, VS Code, TextEdit).

### 2. Add Noelclaw

If the file is empty or doesn't exist yet, paste this:

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

If the file already has other MCP servers, just add the `"noelclaw"` block inside `"mcpServers"`:

```json
{
  "mcpServers": {
    "other-server": { ... },
    "noelclaw": {
      "command": "npx",
      "args": ["-y", "@noelclaw/mcp"]
    }
  }
}
```

### 3. Restart Claude Desktop

**Fully quit the app** - don't just close the window. On Mac: right-click the dock icon → Quit. On Windows: close all windows then check the system tray.

Reopen Claude Desktop. All 103 tools are now available.

### 4. Verify it works

Type this in any chat:

> "What's ETH trading at right now?"

Claude will use `get_market_data` automatically and return live prices.

---

## Claude Code

```bash
claude mcp add noelclaw -s user -- npx -y @noelclaw/mcp
```

Verify it's registered:

```bash
claude mcp list
# noelclaw   npx -y @noelclaw/mcp
```

All 103 tools are now available in every Claude Code session.

---

## Test the Memory Feature

This is the standout feature - try it in two separate chats.

**Chat 1 - save your preferences:**
> "Remember that I only trade on Base mainnet, prefer low-risk DeFi like Lido and Aerodrome, and avoid leverage and meme coins"

**Open a new chat - test recall:**
> "What do you know about my DeFi strategy?"

Claude will pull from vault memory and answer accurately - even though this is a brand new conversation.

---

## Troubleshooting

| Problem | Fix |
|---------|-----|
| Tools don't appear | You restarted Claude but tools aren't showing - try fully quitting from the dock/taskbar |
| First message is slow (~5 sec) | Normal - first run downloads `@noelclaw/mcp` from npm. Fast after that. |
| `command not found: npx` | Node.js not installed - get it at [nodejs.org](https://nodejs.org) |
| Auth error on tool calls | Wallet auto-generates on first use - no sign-in needed. Try restarting the client. |

---

## Optional: Pin to a Specific Version

```json
{
  "mcpServers": {
    "noelclaw": {
      "command": "npx",
      "args": ["-y", "@noelclaw/mcp@2.3.1"]
    }
  }
}
```

Remove the version tag to always get the latest on restart.
