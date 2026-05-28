# Add to Claude

Install the Noelclaw MCP skill in Claude Code or Claude Desktop. No build step — runs via `npx`.

**Requirement:** Node.js >= 18.

---

## Claude Code

```bash
claude mcp add noelclaw -- npx @noelclaw/mcp
```

Verify it's registered:

```bash
claude mcp list
# noelclaw   npx @noelclaw/mcp
```

All 34 tools are now available in every Claude Code session.

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

Save the file, then **restart Claude Desktop**. All 34 tools appear automatically in the tool list.

---

## Optional: With Environment Variables

Add any env vars you need directly in the config:

```json
{
  "mcpServers": {
    "noelclaw": {
      "command": "npx",
      "args": ["@noelclaw/mcp"],
      "env": {
        "MINIMAX_API_KEY": "your-minimax-key",
        "AYRSHARE_API_KEY": "your-ayrshare-key",
        "BANKR_API_KEY": "your-bankr-key"
      }
    }
  }
}
```

See [Environment Variables](env-vars.md) for the full list.

---

## First Steps

After install, try these in any Claude session:

```
get_market_data
```

```
ask_noel question: "What's the market doing right now?"
```

```
vault_save key: "my-note" content: "Testing noelclaw vault" type: "note"
```

```
miroshark_simulate scenario: "What happens if BTC hits $200k this cycle?"
```
