# Noelclaw

**Noelclaw** is a persistent AI layer with an 87-tool MCP skill (`@noelclaw/mcp`) that plugs directly into Claude, Cursor, Windsurf, Hermes, Bankr, Aeon, and any MCP-compatible client — giving your AI persistent memory across sessions, autonomous research monitors, live market data, DeFi execution on Base, multi-agent swarms, web research, AI-assisted code generation, and MiroShark simulation, all from natural language.

- Website: [noelclaw.com](https://noelclaw.com)
- App: [app.noelclaw.com](https://app.noelclaw.com)
- npm: [@noelclaw/mcp](https://www.npmjs.com/package/@noelclaw/mcp)
- Version: `3.3.0`

---

## Quick Install

**Requirement:** Node.js >= 18 — check with `node --version`, download from [nodejs.org](https://nodejs.org) if needed.

### Terminal (any MCP client)
```bash
npx -y @noelclaw/mcp
```

### Claude Code
```bash
claude mcp add noelclaw -s user -- npx -y @noelclaw/mcp
```

### Claude Desktop
Edit your config file:
- **Mac:** `~/Library/Application Support/Claude/claude_desktop_config.json`
- **Windows:** `%APPDATA%\Claude\claude_desktop_config.json`

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

Restart Claude Desktop after saving.

### Hermes
```bash
hermes mcp add noelclaw -- npx -y @noelclaw/mcp
```

No build step. No config required. Runs on first use.

---

## 81 Tools Across 18 Categories

Three pillars: **Remember · Act · Know**

| Category | Tools | What It Does |
|----------|-------|-------------|
| Vault | 14 | Persistent notes with versioning, search, diff, export, credentials, knowledge graph |
| Semantic Memory | 9 | Vector search, cross-session recall, URL ingestion, extract facts, consolidate |
| Session OS | 3 | Boot, status dashboard, shutdown with session summary |
| Automations | 6 | DCA, price alerts, conditional buy/sell, execution history |
| DeFi Execution | 6 | Portfolio, swap via 0x, send tokens, wallet analysis, yields |
| Base Chain | 4 | Morpho vaults, Moonwell markets, deposit prep, chain stats |
| Wallet & Notifications | 2 | Local wallet address, Telegram alerts |
| Playbooks | 3 | Browse and run playbooks, audit ledger |
| Market & Intel | 5 | Live prices, token data, comparison, overview, OHLC history |
| Token Scanner | 4 | Score tokens, safety check, dip scan, momentum scan |
| Research & Insight | 3 | AI analyst, market thesis, trade plan |
| Web Research | 2 | Live web search, scrape any URL |
| Autonomous Monitor | 3 | Schedule daily research runs, get Telegram briefings, manage monitors |
| Agent Network | 11 | Multi-agent swarm, parallel research, briefing, hire specialists, persistent agents |
| Coder | 5 | Generate contracts, audit, explain, review, MCP skill builder |
| Content & Humanizer | 3 | Humanize text, write threads and posts |
| MiroShark | 3 | Multi-agent market simulation |

---

## What's New in v3.3.0

**Persistent Agents** — `agent_spawn`, `agent_recall`, `agent_update`. Create a named agent with a goal, log progress and findings across sessions, and pick up exactly where you left off. State is versioned in vault — every update is a new version with full history. Use `vault_link` to wire an agent's vault into the broader knowledge graph.

**Knowledge Graph** — `vault_link` + `vault_related`. Connect any two vault entries with a typed relation (`references`, `derived_from`, `supersedes`, `related`, `continues`). `vault_related` traverses the graph in both directions — outbound (what this entry references) and inbound (what references this entry). Context accumulates over time instead of getting buried.

**Autonomous Monitors** — Set up a recurring research agent with `create_monitor`. It runs on a schedule (daily, weekly, or any cron), searches the web, saves findings to your vault, and sends a Telegram briefing. Compares each run to the previous one — highlights what changed, not just what happened.

**Live Web Research** — `web_search` searches the web in real time. `web_scrape` reads any URL and returns the full content. Feed the results into `market_thesis` or `trade_plan` for analysis grounded in today's news.

**Smart urgency system** — Monitor notifications scale by urgency (1–5). Routine days get a quiet summary. Breaking news gets a loud alert with the headline surfaced immediately.

**Auto-save on insight tools** — `market_thesis` and `trade_plan` automatically save outputs to vault. Your research history builds up over time without any extra steps.

---

## Docs

- [Getting Started](getting-started.md)
- [Install on Claude](claude-install.md)
- [Install on Hermes](hermes-openclaw.md)
- [Install on Cursor / Windsurf](cursor-install.md)
- [Semantic Memory Guide](memory.md)
- [Full MCP Tool Reference](mcp-server.md)
- [MiroShark Simulation](miroshark.md)
- [Wallet & DeFi](wallet-defi.md)
- [Environment Variables](env-vars.md)
- [Architecture](architecture.md)
