# Add to Claude Desktop & Claude Code

No build step needed. The Noelclaw MCP server runs via `npx @noelclaw/research`.

---

## Claude Code

```bash
claude mcp add noelclaw -- npx @noelclaw/research
```

Verify it's registered:
```bash
claude mcp list
# noelclaw   npx @noelclaw/research
```

Use tools in any conversation:
```
get_market_data
get_latest_signal token: BTC
research query: "Latest BTC news and market outlook"
```

---

## Claude Desktop

### Mac

Edit: `~/Library/Application Support/Claude/claude_desktop_config.json`

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

### Windows

Edit: `%APPDATA%\Claude\claude_desktop_config.json`

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

After saving, **restart Claude Desktop**. Tools appear in the tool picker automatically.

---

## Optional: Point to a Custom Backend

If you're running your own Convex deployment:

```json
{
  "mcpServers": {
    "noelclaw": {
      "command": "npx",
      "args": ["@noelclaw/research"],
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
| Tools not showing in Claude Desktop | Restart Claude Desktop after saving config |
| `npx: command not found` | Install Node.js 18+ from nodejs.org |
| `spawn npx ENOENT` | Use full path to npx: find it with `where npx` (Windows) or `which npx` (Mac) |
