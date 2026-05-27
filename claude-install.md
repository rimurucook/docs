# Add to Claude

Install the Noelclaw MCP skill in Claude Code or Claude Desktop. No build step — runs via `npx`.

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

Save the file, then **restart Claude Desktop**. Tools appear automatically.

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

---

## First Things to Try

```
get_market_data
get_insight
ask_noel: "What's the market doing right now?"
vault_save key: "my-note" content: "..." type: "note"
miroshark_simulate scenario: "What happens if BTC hits $200k?"
```
