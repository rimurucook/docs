# Add to Cursor / Windsurf

No build step needed. The Noelclaw MCP server runs via `npx @noelclaw/mcp`.

---

## Cursor

### Via Settings UI

1. Open Cursor → **Settings** (Ctrl+, / Cmd+,)
2. Search for **MCP** or go to **Features → MCP**
3. Add a new server:
   - **Name:** `noelclaw`
   - **Command:** `npx`
   - **Args:** `@noelclaw/mcp`

### Via Config File

Edit `~/.cursor/mcp.json` (create it if it doesn't exist):

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

Restart Cursor. Tools appear in Composer when in **Agent mode**.

### Use in Cursor Composer

Open Composer (Cmd+I / Ctrl+I), enable Agent mode, then:
```
use noelclaw to get the current crypto market
ask noel what the best DeFi opportunities are right now
get the latest BTC signal from noelclaw
```

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

Restart Windsurf.

---

## Optional: Custom Backend

```json
{
  "mcpServers": {
    "noelclaw": {
      "command": "npx",
      "args": ["@noelclaw/mcp"],
      "env": {
        "NOELCLAW_CONVEX_URL": "https://your-deployment.convex.site"
      }
    }
  }
}
```

---

## Troubleshooting

| Problem | Fix |
|---------|-----|
| Tools not showing | Restart Cursor/Windsurf after saving config |
| `npx: command not found` | Install Node.js 18+ from nodejs.org |
| Connection timeout | Normal on first run — `npx` downloads the package. Retries will be instant |
