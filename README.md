# Noelclaw

## The runtime layer for AI.

**Your AI remembers, keeps working, and survives every session.**

Most AI assistants disappear when the conversation ends. Noelclaw gives them persistent state - memory that accumulates, agents that keep running, vaults that version knowledge, and workflows that continue after you close the chat.

Works in **Cursor, Windsurf, Claude Desktop, ChatGPT, Zed, Hermes, Bankr, Aeon**, and anywhere [MCP](https://modelcontextprotocol.io) runs.

- Website: [noelclaw.com](https://noelclaw.fun)
- App: [app.noelclaw.com](https://app.noelclaw.com)
- npm: [@noelclaw/mcp](https://www.npmjs.com/package/@noelclaw/mcp)
- Version: `3.43.1`

---

## The three pillars

### Memory - what your AI remembers
Semantic, versioned, deduplicated. Vault gives you git-style versioning and knowledge-graph links. Memory gives you semantic search with 90-day time-decay and same-session deduplication. Together they form a two-tier persistent state your AI can read from and write to.

### Agents - what runs in the background
Named, persistent agents that survive across sessions. Spawn one with a goal, recall it weeks later, audit every state change via the ledger. Each agent can hold its own Base wallet address for on-chain identity.

### Workflows - what executes on a schedule
Packets, automations, monitors, and deep research. Anything that runs after you close the chat - daily research that lands in your vault, DCA orders that fire weekly, multi-agent research swarms that complete async.

---

## Quick Install

**Requirement:** Node.js >= 18 - check with `node --version`, download from [nodejs.org](https://nodejs.org) if needed.

### One-command setup (auto-detects all MCP clients)
```bash
npx -y -p @noelclaw/mcp@3.43.1 noelclaw install
```

Detects Claude Desktop, Cursor, Windsurf, VS Code, Zed, and configures each automatically. Then restart your client.

### Claude Code
```bash
claude mcp add noelclaw -s user -- npx -y -p @noelclaw/mcp@3.43.1 noelclaw-mcp
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
      "args": ["-y", "-p", "@noelclaw/mcp@3.43.1", "noelclaw-mcp"]
    }
  }
}
```

Restart Claude Desktop after saving.

### Hermes
```bash
hermes mcp add noelclaw -- npx -y -p @noelclaw/mcp@3.43.1 noelclaw-mcp
```

No build step. No config required. Runs on first use.

---

## 121 Tools

> **Noel Shell:** The webapp chat supports native tool calling. Shell tools let the chat spawn agents, save to vault, search memory, create automations, estimate + execute swaps, list agents, and check wallet balances — all from natural conversation. See [Noel Shell](noel-shell.md).

Grouped by pillar — counts match the live tool registry exactly. Full catalog: [All 121 Tools](mcp-server.md).

| Pillar | Category | Tools | What it does |
|--------|----------|:-----:|-------------|
| **Memory** | Vault | 15 | Versioned notes: save, read, search, history, diff, export, credentials, tags, links, knowledge graph |
| **Memory** | Semantic Memory | 10 | Semantic search, cross-session recall, extract, consolidate, insight, publish/delete with confirm |
| **Memory** | Chronicle | 4 | Append-only audit trail — add, list, search, activity stats |
| **Agents** | Agents | 12 | Persistent named agents: spawn, recall, update, identity, ledger, schedule/pause/resume, runs, hire, list |
| **Agents** | Playbooks | 3 | Browse and run curated playbooks with an audit ledger |
| **Workflows** | Automations | 6 | DCA, price alerts, conditional buy/sell, dry-run, pause/delete, execution history |
| **Workflows** | Monitors | 3 | Scheduled research monitors: schedule, list, cancel |
| **Workflows** | Packets (Flows) | 4 | Create, run, list, and share reusable workflow packets |
| **Workflows** | Research | 3 | `deep_research` (keyless evidence packs), compare two reports, walk a research chain |
| **Execution** | Base DeFi | 12 | `base_mcp_*`: balances with impostor detection, send, 0x Permit2 swaps, Morpho/Moonwell yields, basenames — see [Base](base.md) |
| **Execution** | Robinhood Chain | 14 | `rh_*`: 22 tokenized stocks + any RH-chain crypto, V2/V3/V4 best-fill routing, risk screens, DCA/TP/SL orders, stock bridge — see [Robinhood Chain](robinhood.md) |
| **Execution** | Stocks (SEC) | 3 | Fundamentals (XBRL), insider trades (Form 4), material events (8-K) — see [Stocks & SEC Data](stocks-sec.md) |
| **Execution** | Market Data | 6 | Live prices, token data, market overview, OHLC history, market thesis |
| **Execution** | Token Scanner | 3 | Score, safety-check, and compare tokens on Base |
| **Execution** | Web Research | 2 | Live web search + scrape any URL (proxied, keyless) |
| **Execution** | GitHub | 8 | Repos, PRs, issues, files, commits, code search (read-only) |
| **Execution** | Code Audit | 1 | Static Solidity security scan with review rubric |
| **Execution** | Insight | 3 | `ask_noel`, trade planning, DeFi yields |
| **Execution** | MiroShark | 3 | Multi-agent market simulation |
| **Execution** | Wallet | 3 | Wallet address, live balance with USD pricing, EIP-191 message signing |
| **Runtime** | OS | 3 | `noel_status` dashboard, `noel_diagnostics` health check, Noel Shell chat bridge |

---

## What's New

### v3.43.1 - x402 API Market, Robinhood Chain Trading, 121 Tools
- **x402 API Market** — sell any HTTPS API per call, buy with USDC on Base via the x402 protocol (HTTP 402 challenge → pay → retry with `PAYMENT-SIGNATURE`). No account or API key needed for buyers. Payments are replay-protected at both the request-id and tx-hash level. See [x402 API Market](x402-api-market.md).
- **Robinhood Chain rail (`rh_*`)** — 22 tokenized stocks (added NFLX, SPY, QQQ, GME) plus trending chain tokens, arbitrary crypto by ticker or contract address, V2/V3/V4 best-fill routing, receipt-backed swap confirmations, safety scans (`rh_analyze`, `rh_safety_check`), DCA/TP-SL order engine.
- **Investor triad from SEC primary sources, all keyless** — `stock_fundamentals` (XBRL financials), `stock_insider` (Form 4), `stock_events` (8-K decoded), plus `rh_stock_bridge` (tokenized vs real price + pool depth).
- **Client-first refactor** — tools return evidence and structure; your model does the reasoning. `deep_research` works with zero API keys (`mode: "sources"`).
- **Cleaner metadata** — tool count corrected to 121 everywhere, emoji removed from all tool and package descriptions.

### v3.32.7 - Local Memory, OpenAI BYOK, Critical Install Fix
- **Local memory** — run memory tools on a free, self-hosted [supermemory](https://github.com/supermemoryai/supermemory) server on your own machine. Zero cost, private, no Noelclaw account or Convex proxy needed once enabled. Run `noelclaw setup` to switch. *(Beta - code-reviewed and unit-verified, live-server testing pending.)*
- **OpenAI BYOK** — OpenAI joins Bankr/Anthropic/Grok as a direct LLM provider. `OPENAI_BASE_URL` lets you route to any self-hosted OpenAI-compatible gateway instead (LiteLLM, vLLM, Ollama, OpenRouter, your own VPS).
- **`noelclaw setup`** — new guided CLI wizard for picking an LLM provider and/or enabling local memory in one flow.
- **Critical fix** — `noelclaw install` was writing a broken MCP server entry (`npx -y @noelclaw/mcp@latest`, ambiguous bin resolution) into every detected client config, so every fresh auto-install was non-functional. Fixed to the unambiguous, version-pinned form.

### v3.32.5 - Chronicle Search, execute_swap, Live Wallet Pricing, Diagnostics
- **`execute_swap`** — execute token swaps on Base mainnet from Noel Shell. Enforces estimate → confirm → execute flow. Routes via 0x Permit2. Returns tx hash + Basescan link. Hard-blocked without `confirmed=true`.
- **`chronicle_search`** — keyword search across runtime events by title and detail. Find when your agent last researched any topic without scrolling the full log.
- **`chronicle_stats`** — runtime activity analytics: event breakdown by type, daily heatmap, busiest days, avg events/day over a configurable window (default 30 days, max 90).
- **`get_wallet_balance`** — live ETH + USDC balance from Base mainnet with real-time USD pricing from CoinGecko. No API key required.
- **`wallet_sign_message`** — EIP-191 personal_sign to prove wallet ownership off-chain without sending a transaction.
- **`noel_diagnostics`** — pre-flight health check: pings Convex, Firecrawl, Supermemory; lists configured API keys; warns on missing LLM key or Firecrawl key with actionable hints.
- **Base Builder Code** — all x402 payment flows now include `bc_7diuqbqo` as `builderCode`. Transactions are attributed to Noelclaw in the Base Dashboard.

### v3.31.0 - Noel Shell, Multi-Provider Chat, Security Hardening
- **Noel Shell** - native tool calling from the webapp chat. Shell tools: `spawn_agent`, `save_to_vault`, `search_memory`, `create_automation`, `estimate_swap`, `list_agents`, `get_wallet_balance`. The chat can now act, not just answer. See [Noel Shell docs](noel-shell.md).
- **7 Agents** - Noel (crypto), CoinGecko (crypto data), Sage (analysis), Forge (developer), Quill (creative), Spectre (trading), Atlas (general). Each agent has its own persona and tool access.
- **Multi-provider chat** - provider cascade: Bankr → OpenAI → Anthropic → Groq → OpenRouter → Custom → Local fallback. No single provider dependency.
- **ConnectMcpModal** - onboarding flow for connecting the MCP server to your IDE directly from the webapp.
- **Security hardening** - 8 security boundaries enforced + vulnerability fixes: `getDecryptedPKByUserId` → internalAction, `createWallet` → internalAction, `getPrivateKey` returns address only (never raw key). Auth is Privy + API key only (OTP removed Jul 2026).
- **Theme refresh** - Claude-style warm palette, Inter font, neural-network knowledge-graph visual.

### v3.23.1 - Polish
- **Memory dedup** - `memory_add` deduplicates identical content via SHA-256 hash with in-process LRU cache + recent-memories lookup. `force: true` overrides.
- **Agent race-safe** - concurrent `agent_update` calls on the same agent serialize through a per-name async mutex. No more silent write loss.
- **Automation dry-run + error categories** - `run_automation dryRun: true` simulates without broadcasting. Failed runs show category badges (`INSUFFICIENT_BALANCE`, `QUOTE_FAILED`, `TX_REVERTED`, etc.) with one-line fix suggestions.
- **MCP Resources pagination** - vault entries past the 50th are now visible to clients via cursor pagination. MIME types derive from `contentType` (markdown/json/code/text).

### v3.22.0 - UX
- **HTTP cache + 429 backoff** - every external API call (CoinGecko, DexScreener, GeckoTerminal) flows through a 45s LRU + exponential backoff that honors `Retry-After`. Agent loops can't trip rate limits anymore.
- **MEV-protect RPC opt-in** - `NOELCLAW_BROADCAST_RPC=<private-relay-url>` routes signed transactions through a private relay. Reads stay on the fast Base RPC.
- **Tool count drift fixed** - banner, login, and `noelclaw doctor` all derive counts from the actual registered tools. No more 104/110/36/100+ mismatch.
- **Humanizer model pin** - `NOELCLAW_HUMANIZER_MODEL` lets you lock the model used by `humanize_text` and `write_content` for consistent voice.

### v3.21.0 - Safety
- **Slippage + price-impact guards** - `swap_tokens` and `base_mcp_swap` refuse execution when price impact exceeds `maxPriceImpactPct` (default 3%). Default slippage cap 1%. Configurable per call.
- **Silent supermemory sync fixed** - retries 3× exponential, logs to chronicle on permanent failure. No more ghost memories that exist in one tier but not the other.
- **SWARM_TOOLS zombie removed** - dead dispatch handler cleaned up. Multi-agent research is built into `deep_research` now (`depth: "standard" | "deep"`).

### v3.20.0 - Grounding & streaming
- **`audit_contract` grounded** - 13-pattern static Solidity scan runs before the LLM, mandatory disclaimer appended, no more "secure"/"safe" claims.
- **`github_search_code` helpful error** - clear setup instructions and a link to `https://github.com/settings/tokens` when `GITHUB_TOKEN` is missing.
- **`memory_search` time-decay** - 90-day half-life weighting + promotion hints when 4+ memories cluster on a topic.
- **Pyth oracle in `fetchVerifiedPrice`** - cross-source price verification against CoinGecko + DexScreener + Pyth with disagreement detection.
- **`deep_research` streaming progress** - MCP `notifications/progress` for long-running multi-agent research.
- **`noelclaw doctor`** - 5-second CLI health check with ✓/⚠/✗ status and inline fix hints.

---

## Docs

- [Getting Started](getting-started.md)
- [Install on Claude](claude-install.md)
- [Install on Cursor / Windsurf](cursor-install.md)
- [Install on Hermes](hermes-openclaw.md)
- [Semantic Memory Guide](memory.md)
- [Chronicle (Audit Trail)](chronicle.md)
- [Packets (Flows)](packets.md)
- [Full MCP Tool Reference](mcp-server.md)
- [MiroShark Simulation](miroshark.md)
- [Wallet & DeFi](wallet-defi.md)
- [Environment Variables](env-vars.md)
- [Architecture](architecture.md)
- [Noel Shell](noel-shell.md)
