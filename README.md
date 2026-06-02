# Noelclaw

**Noelclaw** is a crypto AI platform with a 68-tool MCP skill (`@noelclaw/mcp`) that plugs directly into Claude, Cursor, Windsurf, Hermes, and any MCP-compatible client — giving AI agents live market data, DeFi execution, semantic memory, LLM Mart, multi-agent swarms, token scanning, AI-assisted code generation, MiroShark social simulation, and Sentinel-gated playbooks, all from natural language.

- Website: [noelclaw.com](https://noelclaw.com)
- npm: [@noelclaw/mcp](https://www.npmjs.com/package/@noelclaw/mcp)
- Version: `2.3.1`

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

## 68 Tools Across 15 Categories

| Category | Tools | What It Does |
|----------|-------|-------------|
| Market & Intel | 2 | Live top-20, token prices, 24h changes |
| AI Assistant | 1 | Chat with Noel — crypto AI with live market context |
| DeFi & Portfolio | 5 | Portfolio balance, swap preview, token swaps, send, AI wallet scan |
| Automations | 5 | DCA, price alerts, conditional buy/sell, execution history |
| Swarm | 9 | Multi-agent coordination with shared memory + execution scores |
| Noel Framework | 6 | Sentinel-gated task packets, playbooks, audit ledger |
| Vault | 14 | Persistent memory with semantic search, versioning, diff, export, connectors |
| Semantic Memory | 5 | Vector search, cross-session recall, URL ingestion, memory profile |
| LLM Mart | — | 30+ models in one place — Claude, GPT, Grok, DeepSeek, Noel Crypto |
| Wallet & Notifications | 2 | Local wallet address, Telegram alerts |
| MiroShark | 3 | Multi-agent social simulation for any scenario |
| Agents | 2 | Hire specialist agents (analyst, researcher, executor, scout) |
| Token Scanner | 3 | Score tokens, check safety, scan for dip reversals |
| Coder | 6 | Scaffold projects, generate components/contracts, audit, explain, review code |
| Base & Chain | 4 | Morpho vaults, Moonwell markets, deposit prep, chain stats |

---

## What's New in v2.3

**Semantic Memory** — Noelclaw now remembers by meaning, not keywords. Every vault entry is automatically indexed as a vector. Ask "what's my risk profile?" in a fresh chat and it finds "user prefers low-risk DeFi, Base only" — even if you never used those exact words.

**LLM Mart** — 30+ models accessible from one credit balance. Includes Noel Crypto, a DeFi-native AI built specifically for Base ecosystem research.

**OpenAI-compatible API** — Drop-in replacement for the OpenAI SDK. Replace `openai.com` with your Noelclaw endpoint and your existing code works instantly.

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
