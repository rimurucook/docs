# Noelclaw

**Noelclaw** is a crypto AI platform with a 53-tool MCP skill (`@noelclaw/mcp`) that plugs directly into Claude, Cursor, Windsurf, Hermes, and Aeon — giving AI agents live market data, DeFi execution, multi-agent swarms, persistent vault memory, token scanning, AI-assisted code generation, MiroShark social simulation, and Sentinel-gated playbooks, all from natural language.

- Platform: [app.noelclaw.com](https://app.noelclaw.com)
- npm: [@noelclaw/mcp](https://www.npmjs.com/package/@noelclaw/mcp)
- Version: `2.1.0`

---

## Quick Install

**Requirement:** Node.js >= 18

**Claude Code**
```bash
claude mcp add noelclaw -s user -- npx -y @noelclaw/mcp
```

**Claude Desktop** — edit `%APPDATA%\Claude\claude_desktop_config.json` (Windows) or `~/Library/Application Support/Claude/claude_desktop_config.json` (Mac):
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

No build step. No config required. Runs on first use.

---

## 61 Tools Across 14 Categories

| Category | Tools | What It Does |
|----------|-------|-------------|
| Market & Intel | 2 | Live top-20, token prices, 24h changes |
| AI Assistant | 1 | Chat with Noel — crypto AI with live market context |
| DeFi & Portfolio | 5 | Portfolio balance, swap preview, token swaps, send, AI wallet scan |
| Automations | 5 | DCA, price alerts, conditional buy/sell, execution history |
| Swarm | 6 | Multi-agent coordination with shared memory + execution scores |
| Noel Framework | 6 | Sentinel-gated task packets, playbooks, audit ledger |
| Noel Vault | 7 | Persistent memory with versioning, search, diff, export |
| Wallet & Notifications | 2 | Local wallet address, Telegram alerts |
| MiroShark | 3 | Multi-agent social simulation for any scenario |
| Agents | 2 | Hire specialist agents (analyst, researcher, executor, scout) |
| Token Scanner | 3 | Score tokens, check safety, scan for dip reversals |
| Coder | 6 | Scaffold projects, generate components/contracts, audit, explain, review code |
| Base & Chain | 4 | Morpho vaults, Moonwell markets, deposit prep, chain stats |
| Humanizer | 1 | Strip AI writing patterns (requires `MINIMAX_API_KEY`) |

---

## Architecture

- MCP server runs locally via stdio — nothing intercepted in transit, keys never leave your machine
- Market data: CoinGecko free API — no key required, always live
- DeFi execution: 0x Permit2 on Base mainnet, signed locally via ethers.js
- Vault + Swarm: Supabase Edge Functions backend
- Automations + Framework + Agents: Convex backend
- MiroShark simulation: Railway-hosted multi-agent engine
- Coder tools: Bankr LLM (requires `BANKR_API_KEY`)
- Global API gateway: Cloudflare Worker with rate limiting and CORS

---

## Integrations

| Client | Guide | Status |
|--------|-------|--------|
| Claude Code / Claude Desktop | [claude-install.md](claude-install.md) | ✅ |
| Cursor / Windsurf | [cursor-install.md](cursor-install.md) | ✅ |
| Hermes | [hermes-openclaw.md](hermes-openclaw.md) | ✅ |
| Aeon | [aeon.md](aeon.md) | ✅ |
| Noel Crew (desktop companion) | [INSTALL.md](INSTALL.md) | ✅ |

---

## Docs

- [Getting Started](getting-started.md)
- [Full MCP Tool Reference](mcp-server.md)
- [MiroShark Simulation](miroshark.md)
- [Wallet & DeFi](wallet-defi.md)
- [Environment Variables](env-vars.md)
- [Architecture](architecture.md)
