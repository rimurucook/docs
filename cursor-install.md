# Add to Cursor / Windsurf

Install the Noelclaw MCP skill in Cursor or Windsurf. No build step — runs via `npx`.

**Requirement:** Node.js >= 18.

---

## Cursor

### Via Settings UI

1. Open Cursor → **Settings** (Ctrl+, / Cmd+,)
2. Search **MCP** or navigate to **Features → MCP**
3. Click **Add Server** and fill in:
   - Name: `noelclaw`
   - Command: `npx`
   - Args: `-y @noelclaw/mcp`
4. Save and restart Cursor

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

Restart Cursor. Tools appear in **Composer** when using Agent mode.

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

## Optional: With Environment Variables

```json
{
  "mcpServers": {
    "noelclaw": {
      "command": "npx",
      "args": ["-y", "@noelclaw/mcp"],
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
get_portfolio
```

```
swap_tokens fromToken: "ETH" toToken: "USDC" amount: "0.01"
```

---

## Troubleshooting

| Problem | Fix |
|---|---|
| Tools not showing | Restart Cursor/Windsurf after saving config |
| `npx: command not found` | Install Node.js 18+ from [nodejs.org](https://nodejs.org) |
| Slow first start | `npx` downloads the package on first run. Subsequent starts are instant |
