# Noelclaw

**Noelclaw** is a crypto AI platform with a 35-tool MCP skill (`@noelclaw/mcp`) that plugs directly into Claude, Cursor, Windsurf, Hermes, and Aeon — giving AI agents live market data, DeFi execution, multi-agent swarms, persistent vault memory, MiroShark social simulation, and Sentinel-gated playbooks, all from natural language.

- Platform: [noelclaw.com](https://noelclaw.com)
- npm: [@noelclaw/mcp](https://www.npmjs.com/package/@noelclaw/mcp)
- Version: `1.5.6`

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

## 35 Tools Across 8 Categories

| Category | Tools | What It Does |
|----------|-------|-------------|
| Market & Intel | 3 | Live top-20, token prices, 24h changes, AI market analysis |
| Wallet & DeFi | 3 | Local wallet, token swaps on Base via 0x, send tokens |
| Automations | 4 | DCA, price alerts, conditional buy/sell in plain English |
| Swarm | 6 | Multi-agent coordination with shared memory + execution scores |
| Noel Framework | 6 | Sentinel-gated task packets, playbooks, audit ledger |
| Noel Vault | 7 | Persistent memory with versioning, search, diff, export |
| MiroShark | 3 | Multi-agent social simulation for any scenario |
| Social & Utils | 3 | Telegram alerts, post to X, humanize AI text |

---

## What Was Built & Solved

**Architecture:**
- MCP server runs locally via stdio — nothing intercepted in transit, keys never leave your machine
- Market data: CoinGecko free API — no key required, always live
- DeFi execution: 0x Permit2 on Base mainnet, signed locally via ethers.js
- Vault + Swarm: Supabase Edge Functions backend
- Playbooks + Automations: Convex backend
- MiroShark simulation: Railway-hosted multi-agent engine
- Global API gateway: Cloudflare Worker with rate limiting and CORS

**Key fixes shipped in v1.5.x:**

| Problem | What Was Wrong | Fix |
|---------|---------------|-----|
| `get_market_data` → 401 | Was reading from Supabase swarm memory (auth required) | Rewrote to call CoinGecko free API directly |
| Stale `btc_price` in swarm memory | Old price data from previous sessions persisted | `start_swarm` now auto-fetches live BTC/ETH/SOL and writes fresh to memory |
| No way to stop a simulation | `miroshark_stop` didn't exist | Added `miroshark_stop` tool |
| Ghost tools in server | `get_insight`, `claim_fees`, `research` had no working backend | Removed, count corrected to 35 |
| Wrong `vault_save` params everywhere | Docs showed `type: "note"`, `key` as required — both invalid | Fixed across all docs and examples |

---

## Integrations

| Client | Guide | Status |
|--------|-------|--------|
| Claude Code / Claude Desktop | [claude-install.md](claude-install.md) | ✅ |
| Cursor / Windsurf | [cursor-install.md](cursor-install.md) | ✅ |
| Hermes | [hermes-openclaw.md](hermes-openclaw.md) | ✅ |
| Aeon | [aeon.md](aeon.md) | ✅ Merged |
| Noel Crew (desktop companion) | [INSTALL.md](INSTALL.md) | ✅ |

---

## Docs

- [Getting Started](getting-started.md)
- [Full MCP Tool Reference](mcp-server.md)
- [MiroShark Simulation](miroshark.md)
- [Wallet & DeFi](wallet-defi.md)
- [Environment Variables](env-vars.md)
- [Architecture](architecture.md)
