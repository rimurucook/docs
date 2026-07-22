# Add to Codex

Install the Noelclaw MCP skill in OpenAI's Codex CLI. No build step - runs via `npx`.

**Requirement:** Node.js >= 18.

---

## Via `codex mcp add`

```bash
codex mcp add noelclaw -- npx -y -p @noelclaw/mcp@3.43.1 noelclaw-mcp
```

## Via Config File

Edit `~/.codex/config.toml` (or a project-scoped `.codex/config.toml`):

```toml
[mcp_servers.noelclaw]
command = "npx"
args = ["-y", "-p", "@noelclaw/mcp@3.43.1", "noelclaw-mcp"]
```

Restart Codex, then verify with `/mcp` in the TUI - `noelclaw` should show as active with all 121 tools loaded.

---

## Optional: With Environment Variables

```toml
[mcp_servers.noelclaw]
command = "npx"
args = ["-y", "-p", "@noelclaw/mcp@3.43.1", "noelclaw-mcp"]

[mcp_servers.noelclaw.env]
BANKR_API_KEY = "your-bankr-key"
```

See [Environment Variables](env-vars.md) for the full list.

---

## Test It

```
get_market_data
```

```
ask_noel question: "What's moving in crypto right now?"
```

---

## Run Memory Fully Local (Optional, Free)

By default, memory is stored via the Noelclaw-hosted proxy. To run it entirely on your own machine instead - private, zero cost, no account needed - run the setup wizard from a terminal:

```bash
npx -y -p @noelclaw/mcp@3.43.1 noelclaw setup
```

This walks you through bringing your own LLM key (Bankr, Anthropic, OpenAI, or a custom self-hosted endpoint) and enabling local memory, which auto-installs a free, open-source [supermemory](https://github.com/supermemoryai/supermemory) server on your machine. Restart Codex afterward - memory tools pick up the local server automatically. Check status with `npx -y -p @noelclaw/mcp@3.43.1 noelclaw doctor`.

---

## Troubleshooting

| Problem | Fix |
|---|---|
| Server not showing in `/mcp` | Restart Codex after editing `config.toml` |
| `npx: command not found` | Install Node.js 18+ from [nodejs.org](https://nodejs.org) |
| Slow first start | `npx` downloads the package on first run. Subsequent starts are instant |
