# Add to Cursor / Windsurf

Install the Noelclaw MCP skill in Cursor or Windsurf. No build step — runs via `npx`.

---

## Cursor

### Via Settings UI

1. Open Cursor → **Settings** (Ctrl+, / Cmd+,)
2. Search **MCP** or go to **Features → MCP**
3. Add server:
   - Name: `noelclaw`
   - Command: `npx`
   - Args: `@noelclaw/mcp`

### Via Config File

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

Restart Cursor. Tools appear in Composer (Agent mode).

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

## Optional: With Custom Backend

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
