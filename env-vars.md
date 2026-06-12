# Environment Variables

The MCP server reads these from your local environment — set them in your MCP client config under the `env` block. All are optional unless noted.

---

## MCP Server Variables

| Variable | Required | Purpose |
|----------|----------|---------|
| `ANTHROPIC_API_KEY` | no | Use your own Anthropic key for the CLI agent. Without it, calls proxy through the Noelclaw platform automatically |
| `BANKR_API_KEY` | no | Use Bankr LLM gateway (Claude + Grok via single API) for the CLI agent instead of Anthropic direct |
| `ANTHROPIC_MODEL` | no | Override model for the CLI agent (default: `claude-haiku-4-5-20251001`) |
| `FIRECRAWL_API_KEY` | for `deep_research`, `web_search` | Required for `deep_research` and `web_search`; optional for `web_scrape` (falls back to basic fetch) |
| `TRIGGER_SECRET_KEY` | for `create_monitor` | Required for autonomous scheduled monitors |
| `ALCHEMY_API_KEY` | no | Faster swap quotes and Base mainnet balance lookups |
| `TELEGRAM_BOT_TOKEN` | no | Your Telegram bot token — for monitor and automation notifications |
| `TELEGRAM_CHAT_ID` | no | Your Telegram chat ID for delivery |
| `MINIMAX_API_KEY` | for `humanize_text` | MiniMax API key — required to use the humanizer tool |
| `GITHUB_TOKEN` | no | GitHub personal access token — required for private repos and higher API rate limits. Public repos work without it |

> **Telegram** is only needed if you want push notifications outside your AI client. If you use Noelclaw through Claude, Cursor, Hermes, or Aeon, you already get all results inline.

---

## How to Set Them

### Claude Code

```bash
claude mcp add noelclaw -s user -- npx -y @noelclaw/mcp
```

Then edit `~/.claude.json` to add env vars under the server entry:

```json
{
  "mcpServers": {
    "noelclaw": {
      "command": "npx",
      "args": ["-y", "@noelclaw/mcp"],
      "env": {
        "ANTHROPIC_API_KEY": "sk-ant-...",
        "MINIMAX_API_KEY": "your-key"
      }
    }
  }
}
```

### Claude Desktop

Mac: `~/Library/Application Support/Claude/claude_desktop_config.json`
Windows: `%APPDATA%\Claude\claude_desktop_config.json`

```json
{
  "mcpServers": {
    "noelclaw": {
      "command": "npx",
      "args": ["-y", "@noelclaw/mcp"],
      "env": {
        "ANTHROPIC_API_KEY": "sk-ant-...",
        "MINIMAX_API_KEY": "your-key"
      }
    }
  }
}
```

### Hermes

```yaml
mcp_servers:
  noelclaw:
    command: npx
    args:
      - "-y"
      - "@noelclaw/mcp"
    env:
      MINIMAX_API_KEY: your-key
    timeout: 30
    connect_timeout: 10
```

---

## Which Variables Do You Actually Need?

For most users, **no variables are required** — all 102 tools work out of the box:

- A local wallet auto-generates at `~/.noelclaw/wallet.json` on first use
- Market data, vault, memory, swarm, MiroShark — no keys needed
- `swap_tokens`, `send_token` — just need a funded local wallet

Add keys only if you want:
- Your own Anthropic account for the CLI agent → `ANTHROPIC_API_KEY`
- Bankr/Grok-3 for the CLI agent → `BANKR_API_KEY`
- `humanize_text` → `MINIMAX_API_KEY`
- Telegram notifications → `TELEGRAM_BOT_TOKEN` + `TELEGRAM_CHAT_ID`
- GitHub private repos + higher rate limits → `GITHUB_TOKEN`
