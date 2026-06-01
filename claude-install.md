# Add to Claude

Install the Noelclaw MCP skill in Claude Code or Claude Desktop. No build step — runs via `npx`.

**Requirement:** Node.js >= 18.

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

All 61 tools are now available in every Claude Code session.

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

Save the file, then **restart Claude Desktop**. All 61 tools appear automatically in the tool list.

---

## Optional: With Environment Variables

Add any env vars you need directly in the config:

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

After install, try these in any Claude session:

```
get_market_data
```

```
ask_noel question: "What's the market doing right now?"
```

```
get_portfolio
```

```
vault_save type: "memory" title: "My Note" content: "Testing noelclaw vault"
```

```
miroshark_simulate scenario: "What happens if BTC hits $200k this cycle?"
```
