# Add to Hermes

Add Noelclaw as an MCP skill in Hermes. Once connected, all 16 Noel tools are available directly in your agent conversations.

No build step needed — runs via `npx @noelclaw/mcp`.

---

## Method 1 — CLI (Fastest)

```bash
hermes mcp add noelclaw --command npx --args @noelclaw/mcp
```

Reload without restarting:
```
/reload-mcp
```

---

## Method 2 — Config File

Edit `~/.hermes/config.yaml`:

```yaml
mcp_servers:
  noelclaw:
    command: npx
    args:
      - "@noelclaw/mcp"
    timeout: 30
    connect_timeout: 10
```

Run `/reload-mcp` in any Hermes session.

---

## Verify Tools Are Loaded

```
/list-tools
```

You should see all 43 tools including `get_market_data`, `get_latest_signal`, `get_smart_money_alerts`, `swap_tokens`, `research`, and more.

---

## Usage Examples

**Live market data:**
```
use get_market_data to check the current crypto market
```

**Latest trading signals:**
```
get the latest BTC and ETH signals from noelclaw
```

**Whale activity:**
```
get_whale_alerts for the last 6 hours
```

**Research a topic:**
```
research query "What is the latest on Solana ecosystem?"
```
Noel searches the web and returns a structured analysis with key findings, market impact, and sentiment.

**Set up Telegram:**
```
set_telegram userId: "my-id" telegramBotToken: "..." telegramChatId: "..."
```

---

## Optional: Custom Backend

```yaml
mcp_servers:
  noelclaw:
    command: npx
    args:
      - "@noelclaw/mcp"
    env:
      NOELCLAW_CONVEX_URL: https://your-deployment.convex.site
    timeout: 30
    connect_timeout: 10
```

---

## Troubleshooting

| Problem | Fix |
|---------|-----|
| Tools not showing after `/reload-mcp` | Check Node.js 18+ is installed: `node --version` |
| `connect_timeout` errors | Increase to `connect_timeout: 20` — first run downloads the package |
| `NOELCLAW_CONVEX_URL` wrong | Default is `https://valuable-fish-533.convex.site` (no trailing slash) |
| `npx: command not found` | Set full path: `command: /usr/local/bin/npx` — find with `which npx` |
