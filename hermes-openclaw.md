# Add to Hermes

Add Noelclaw as an MCP skill in Hermes. Once connected, all 37 Noel tools are available directly in your agent conversations.

No build step needed — runs via `npx @noelclaw/mcp`.

---

## Method 1 — CLI (Fastest)

```bash
hermes mcp add noelclaw --command npx --args @noelclaw/mcp@latest
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
      - "@noelclaw/mcp@latest"
    timeout: 30
    connect_timeout: 10
```

Run `/reload-mcp` in any Hermes session.

---

## Verify Tools Are Loaded

```
/list-tools
```

You should see all 37 tools including `get_market_data`, `research`, `get_insight`, `swap_tokens`, `vault_save`, `miroshark_simulate`, and more.

---

## Usage Examples

**Live market data:**
```
use get_market_data to check the current crypto market
```

**Research a topic:**
```
research query "What is the latest on Solana ecosystem?"
```

**Save to vault:**
```
vault_save key: "sol-thesis" content: "..." type: "research"
```

**Run a simulation:**
```
miroshark_simulate scenario: "What happens if ETH flips BTC in market cap?"
```

**Set up Telegram:**
```
set_telegram telegramBotToken: "..." telegramChatId: "..."
```

---

## Optional: Custom Backend

```yaml
mcp_servers:
  noelclaw:
    command: npx
    args:
      - "@noelclaw/mcp@latest"
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
| `npx: command not found` | Set full path: `command: /usr/local/bin/npx` — find with `which npx` |
