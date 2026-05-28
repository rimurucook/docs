# Add to Hermes

Add Noelclaw as an MCP skill in Hermes. Once connected, all 34 tools are available directly in your agent conversations.

No build step needed — runs via `npx @noelclaw/mcp`.

**Requirement:** Node.js >= 18.

---

## Method 1 — CLI (Fastest)

```bash
hermes mcp add noelclaw --command npx --args @noelclaw/mcp
```

Reload without restarting Hermes:

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

Then run `/reload-mcp` in any Hermes session.

---

## Verify Tools Are Loaded

```
/list-tools
```

You should see all 34 tools including `get_market_data`, `ask_noel`, `swap_tokens`, `vault_save`, `miroshark_simulate`, and more.

---

## Usage Examples

**Live market data:**

```
use get_market_data to check the current crypto market
```

**Ask Noel for analysis:**

```
ask_noel question: "What is the latest narrative driving ETH?"
```

**Save to vault:**

```
vault_save type: "research" title: "SOL Thesis" content: "My thesis on SOL..."
```

**Run a simulation:**

```
miroshark_simulate scenario: "What happens if ETH flips BTC in market cap?"
```

**Set up Telegram notifications:**

```
set_telegram telegramBotToken: "your-token" telegramChatId: "your-chat-id"
```

**Start a swarm:**

```
start_swarm
```

---

## Optional: With Environment Variables

```yaml
mcp_servers:
  noelclaw:
    command: npx
    args:
      - "@noelclaw/mcp"
    env:
      MINIMAX_API_KEY: your-minimax-key
      AYRSHARE_API_KEY: your-ayrshare-key
      BANKR_API_KEY: your-bankr-key
    timeout: 30
    connect_timeout: 10
```

See [Environment Variables](env-vars.md) for the full list.

---

## Troubleshooting

| Problem | Fix |
|---|---|
| Tools not showing after `/reload-mcp` | Check Node.js 18+ is installed: `node --version` |
| `connect_timeout` errors | Increase to `connect_timeout: 20` — first run downloads the package |
| `npx: command not found` | Set full path: `command: /usr/local/bin/npx` — find it with `which npx` |
| `humanize_text` fails | Set `MINIMAX_API_KEY` in the env section |
| `post_tweet` fails | Set `AYRSHARE_API_KEY` in the env section |
