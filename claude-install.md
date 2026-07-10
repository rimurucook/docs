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
      "args": ["-y", "-p", "@noelclaw/mcp@3.32.7", "noelclaw-mcp"]
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
      "args": ["-y", "-p", "@noelclaw/mcp@3.32.7", "noelclaw-mcp"]
    }
  }
}
```

### 3. Restart Claude Desktop

**Fully quit the app** - don't just close the window. On Mac: right-click the dock icon → Quit. On Windows: close all windows then check the system tray.

Reopen Claude Desktop. All 108 tools are now available.

### 4. Verify it works

Type this in any chat:

> "What's ETH trading at right now?"

Claude will use `get_market_data` automatically and return live prices.

---

## Claude Code

```bash
claude mcp add noelclaw -s user -- npx -y -p @noelclaw/mcp@3.32.7 noelclaw-mcp
```

Verify it's registered:

```bash
claude mcp list
# noelclaw   npx -y -p @noelclaw/mcp@3.32.7 noelclaw-mcp
```

All 108 tools are now available in every Claude Code session.

---

## Test the Memory Feature

This is the standout feature - try it in two separate chats.

**Chat 1 - save your preferences:**
> "Remember that I only trade on Base mainnet, prefer low-risk DeFi like Lido and Aerodrome, and avoid leverage and meme coins"

**Open a new chat - test recall:**
> "What do you know about my DeFi strategy?"

Claude will pull from vault memory and answer accurately - even though this is a brand new conversation.

---

## Run Memory Fully Local (Optional, Free)

By default, memory is stored via the Noelclaw-hosted proxy. To run it entirely on your own machine instead - private, zero cost, no account needed - run the setup wizard from a terminal:

```bash
npx -y -p @noelclaw/mcp@3.32.7 noelclaw setup
```

This walks you through:
- Bringing your own LLM key (Bankr, Anthropic, OpenAI, or a custom self-hosted endpoint) instead of the shared proxy
- Enabling local memory, which auto-installs a free, open-source [supermemory](https://github.com/supermemoryai/supermemory) server on your machine

Once enabled, restart Claude Desktop (or start a new Claude Code session) - memory tools will use your local server automatically. Check status anytime with:

```bash
npx -y -p @noelclaw/mcp@3.32.7 noelclaw doctor
```

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
      "args": ["-y", "-p", "@noelclaw/mcp@2.3.1", "noelclaw-mcp"]
    }
  }
}
```

Remove the version tag to always get the latest on restart.
