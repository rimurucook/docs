# Noelclaw

**Noelclaw** is a persistent AI layer with a 99-tool MCP skill (`@noelclaw/mcp`) that plugs directly into Claude, Cursor, Windsurf, Hermes, Bankr, Aeon, and any MCP-compatible client — giving your AI persistent memory across sessions, autonomous research monitors, live market data, DeFi execution on Base, multi-agent swarms, web research, GitHub integration, AI-assisted code generation, audit trails, reusable workflow packets, and MiroShark simulation, all from natural language.

- Website: [noelclaw.com](https://noelclaw.com)
- App: [app.noelclaw.com](https://app.noelclaw.com)
- npm: [@noelclaw/mcp](https://www.npmjs.com/package/@noelclaw/mcp)
- Version: `3.9.5`

---

## Quick Install

**Requirement:** Node.js >= 18 — check with `node --version`, download from [nodejs.org](https://nodejs.org) if needed.

### One-command setup (auto-configures all detected MCP clients)
```bash
npx -y @noelclaw/mcp install
```

Detects Claude Desktop, Cursor, Windsurf, VS Code, Zed, and configures each automatically. Then restart your client.

### Manual — Claude Code
```bash
claude mcp add noelclaw -s user -- npx -y @noelclaw/mcp
```

### Manual — Claude Desktop
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

### Manual — Hermes
```bash
hermes mcp add noelclaw -- npx -y @noelclaw/mcp
```

No build step. No config required. Runs on first use.

---

## 99 Tools Across 22 Categories

Three pillars: **Remember · Act · Know**

| Category | Tools | What It Does |
|----------|-------|-------------|
| Vault | 14 | Persistent notes with versioning, search, diff, export, credentials, knowledge graph |
| Semantic Memory | 10 | Vector search, cross-session recall, URL ingestion, extract facts, consolidate, publish |
| Agents | 7 | Multi-agent swarm, hire specialists, persistent named agents, agent identity & ledger |
| Automations | 6 | DCA, price alerts, conditional buy/sell, execution history |
| DeFi Execution | 6 | Portfolio, swap via 0x, send tokens, wallet analysis, yields |
| Base Chain | 4 | Morpho vaults, Moonwell markets, deposit prep, chain stats |
| Autonomous Monitor | 4 | Schedule research, create recurring monitors, list, cancel |
| GitHub | 8 | List repos/PRs/issues, read files, commits, search code |
| Packets (Flows) | 4 | Create, run, list, and share reusable workflow packets |
| Chronicle | 2 | Append-only audit trail — add and list entries |
| Market & Intel | 5 | Live prices, token data, comparison, overview, OHLC history |
| Token Scanner | 3 | Score tokens, safety check, scan for dips or momentum |
| Research & Insight | 3 | AI analyst, market thesis, trade plan |
| Swarm | 5 | Multi-agent parallel research, synthesis, trigger, stop, status |
| Web Research | 2 | Live web search, scrape any URL |
| Coder | 5 | Generate contracts, audit, explain, review, MCP skill builder |
| Content & Humanizer | 2 | Humanize text, write threads and posts |
| MiroShark | 3 | Multi-agent market simulation |
| Base Chain | 4 | Morpho vaults, Moonwell markets, deposit prep, chain stats |
| Wallet & Notifications | 2 | Local wallet address, Telegram alerts |
| Playbooks | 3 | Browse and run playbooks, audit ledger |
| Session OS | 1 | System dashboard — memory, swarm, automations, research, scores |

---

## What's New in v3.9.x

**One-command install** — `npx -y @noelclaw/mcp install` auto-detects and configures all MCP clients (Claude Desktop, Cursor, Windsurf, VS Code, Zed) in one shot. No JSON editing.

**`noelclaw login`** — Sign in from the terminal. Saves your session to `~/.noelclaw/config.json`.

---

## What's New in v3.5.0

**GitHub Integration** — 8 tools: `github_list_repos`, `github_list_prs`, `github_get_pr`, `github_list_issues`, `github_get_issue`, `github_get_file`, `github_get_commits`, `github_search_code`. Read repos, PRs, issues, and files directly from your AI client. Set `GITHUB_TOKEN` for private repos.

**Chronicle** — Append-only audit trail with `chronicle_add` and `chronicle_list`.

**Packets (Flows)** — Reusable workflow packets: `packet_create`, `packet_run`, `packet_list`, `packet_share`.

---

## What's New in v3.4.0

**Vault Knowledge Graph** — `vault_link` + `vault_related`. Connect vault entries with typed relations and traverse them in both directions.

**Persistent Agents** — `agent_spawn`, `agent_recall`, `agent_update`. Named agents that survive across sessions, state versioned in vault.

---

## What's New in v3.3.0

**Autonomous Monitors** — Set up a recurring research agent with `create_monitor`. Runs on schedule, saves findings to vault, sends Telegram briefing.

**Live Web Research** — `web_search` searches in real time. `web_scrape` reads any URL. Feed results into `market_thesis` for analysis grounded in today's news.

**Smart urgency system** — Monitor notifications scale by urgency (1–5). Routine days quiet. Breaking news loud.

---

## Docs

- [Getting Started](getting-started.md)
- [Install on Claude](claude-install.md)
- [Install on Hermes](hermes-openclaw.md)
- [Install on Cursor / Windsurf](cursor-install.md)
- [Semantic Memory Guide](memory.md)
- [Chronicle (Audit Trail)](chronicle.md)
- [Packets (Flows)](packets.md)
- [Full MCP Tool Reference](mcp-server.md)
- [MiroShark Simulation](miroshark.md)
- [Wallet & DeFi](wallet-defi.md)
- [Environment Variables](env-vars.md)
- [Architecture](architecture.md)
