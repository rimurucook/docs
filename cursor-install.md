# Add to Cursor / Windsurf

Install the Noelclaw MCP skill in Cursor or Windsurf. No build step - runs via `npx`.

**Requirement:** Node.js >= 18.

---

## Cursor

### Via Settings UI

1. Open Cursor → **Settings** (Ctrl+, / Cmd+,)
2. Search **MCP** or navigate to **Features → MCP**
3. Click **Add Server** and fill in:
   - Name: `noelclaw`
   - Command: `npx`
   - Args: `-y -p @noelclaw/mcp@3.43.1 noelclaw-mcp`
4. Save and restart Cursor

### Via Config File

Edit `~/.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "noelclaw": {
      "command": "npx",
      "args": ["-y", "-p", "@noelclaw/mcp@3.43.1", "noelclaw-mcp"]
    }
  }
}
```

Restart Cursor. Tools appear in **Composer** when using Agent mode.

---

## Windsurf

Edit `~/.windsurf/mcp_config.json`:

```json
{
  "mcpServers": {
    "noelclaw": {
      "command": "npx",
      "args": ["-y", "-p", "@noelclaw/mcp@3.43.1", "noelclaw-mcp"]
    }
  }
}
```

Restart Windsurf after saving.

---

## Optional: With Environment Variables

```json
{
  "mcpServers": {
    "noelclaw": {
      "command": "npx",
      "args": ["-y", "-p", "@noelclaw/mcp@3.43.1", "noelclaw-mcp"],
      "env": {
        "MINIMAX_API_KEY": "your-minimax-key",
        "BANKR_API_KEY": "your-bankr-key"
      }
    }
  }
}
```

See [Environment Variables](env-vars.md) for the full list.

---

## First Steps

After install, try in Composer (Agent mode):

```
get_market_data
```

```
ask_noel question: "What's moving in crypto right now?"
```

```
base_mcp_balance
```

```
base_mcp_swap fromToken: "ETH" toToken: "USDC" amount: "0.01"
```

---

## Run Memory Fully Local (Optional, Free)

By default, memory is stored via the Noelclaw-hosted proxy. To run it entirely on your own machine instead - private, zero cost, no account needed - run the setup wizard from a terminal:

```bash
npx -y -p @noelclaw/mcp@3.43.1 noelclaw setup
```

This walks you through bringing your own LLM key (Bankr, Anthropic, OpenAI, or a custom self-hosted endpoint) and enabling local memory, which auto-installs a free, open-source [supermemory](https://github.com/supermemoryai/supermemory) server on your machine. Restart Cursor/Windsurf afterward - memory tools pick up the local server automatically. Check status with `npx -y -p @noelclaw/mcp@3.43.1 noelclaw doctor`.

---

## Troubleshooting

| Problem | Fix |
|---|---|
| Tools not showing | Restart Cursor/Windsurf after saving config |
| `npx: command not found` | Install Node.js 18+ from [nodejs.org](https://nodejs.org) |
| Slow first start | `npx` downloads the package on first run. Subsequent starts are instant |
