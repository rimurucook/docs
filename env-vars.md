# Environment Variables

The MCP server reads these from your local environment — set them in your MCP client config under the `env` block. All are optional unless noted.

---

## MCP Server Variables

| Variable | Required | Purpose |
|----------|----------|---------|
| `NOELCLAW_API_KEY` | no | Links MCP sessions to your noelclaw.com account |
| `NOELCLAW_SESSION_TOKEN` | no | Alternative to API key for auth |
| `NOELCLAW_CONVEX_URL` | no | Override API endpoint (default: `https://api.noelclaw.com`) |
| `ALCHEMY_API_KEY` | no | Faster swap quotes and Base mainnet balance lookups |
| `BANKR_API_KEY` | no | BYOK — forwarded as `X-User-Bankr-Key` for swarm agents |
| `TELEGRAM_BOT_TOKEN` | no | BYOK — your own Telegram bot token for notifications |
| `TELEGRAM_CHAT_ID` | no | BYOK — your Telegram chat ID for delivery |
| `MINIMAX_API_KEY` | for `humanize_text` | MiniMax API key — required to use the humanizer tool |
| `AYRSHARE_API_KEY` | for `post_tweet` | Ayrshare API key — required to post to X |

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
        "MINIMAX_API_KEY": "your-key",
        "AYRSHARE_API_KEY": "your-key"
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
        "MINIMAX_API_KEY": "your-key",
        "AYRSHARE_API_KEY": "your-key",
        "BANKR_API_KEY": "your-key"
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
      AYRSHARE_API_KEY: your-key
    timeout: 30
    connect_timeout: 10
```

---

## Which Variables Do You Actually Need?

For most users, **no variables are required** — the core tools work without any keys:

- `get_market_data`, `get_token_data` — CoinGecko free API, no key needed
- `ask_noel` — works without any env var
- `start_swarm`, `vault_save`, `miroshark_simulate` — work without any env var
- `swap_tokens`, `send_token` — need a funded local wallet (generated automatically on first use)

Add keys only if you use:
- `humanize_text` → `MINIMAX_API_KEY`
- `post_tweet` → `AYRSHARE_API_KEY`
- Telegram notifications → `TELEGRAM_BOT_TOKEN` + `TELEGRAM_CHAT_ID`
