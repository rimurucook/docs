# Noelclaw

**Noelclaw** is a persistent AI layer with a 76-tool MCP skill (`@noelclaw/mcp`) that plugs directly into Claude, Cursor, Windsurf, Hermes, Bankr, Aeon, and any MCP-compatible client — giving your AI persistent memory across sessions, autonomous automations, live market data, DeFi execution on Base, multi-agent swarms, AI-assisted code generation, and MiroShark simulation, all from natural language.

- Website: [noelclaw.fun](https://noelclaw.fun)
- App: [app.noelclaw.com](https://app.noelclaw.com)
- npm: [@noelclaw/mcp](https://www.npmjs.com/package/@noelclaw/mcp)
- Version: `3.2.1`

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

## 76 Tools Across 16 Categories

Three pillars: **Remember · Act · Know**

| Category | Tools | What It Does |
|----------|-------|-------------|
| Vault | 12 | Persistent notes with versioning, search, diff, export, credentials |
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
| Agent Network | 8 | Multi-agent swarm, parallel research, briefing, hire specialists |
| Coder | 5 | Generate contracts, audit, explain, review, MCP skill builder |
| Content & Humanizer | 3 | Humanize text, write threads and posts |
| MiroShark | 3 | Multi-agent market simulation |

---

## What's New in v3.2.1

**memory_extract & memory_consolidate** — Two new semantic memory tools. Extract discrete facts from any block of text, or merge overlapping memories on a topic into one clean summary.

**Zero-friction setup** — No account, no API key, no website required. A local wallet auto-generates at `~/.noelclaw/wallet.json` on first use and signs every request transparently.

**Bankr & Aeon integration** — Officially available as a skill in Aeon (merged). Bankr integration in review (PR open). Install once, works everywhere.

---

## Docs

- [Getting Started](getting-started.md)
- [Install on Claude](claude-install.md)
- [Install on Hermes](hermes-openclaw.md)
- [Install on Cursor / Windsurf](cursor-install.md)
- [Semantic Memory Guide](memory.md)
- [LLM Mart Guide](llm-mart.md)
- [Full MCP Tool Reference](mcp-server.md)
- [MiroShark Simulation](miroshark.md)
- [Wallet & DeFi](wallet-defi.md)
- [Environment Variables](env-vars.md)
- [Architecture](architecture.md)
