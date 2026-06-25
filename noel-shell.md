# Noel Shell

> Native tool calling from the webapp chat. Available at [app.noelclaw.com](https://app.noelclaw.com) since `@noelclaw/mcp@3.29.0`.

---

## Overview

Noel Shell bridges the gap between conversation and action. When you chat with an agent in the Noelclaw webapp, the chat doesn't just answer questions — it can **call tools** to spawn agents, save to your vault, search memory, create automations, estimate swaps, list agents, and check wallet balances.

This means a single natural-language message can trigger real work: creating a DCA, saving research, recalling what you know, or spawning a background agent — without leaving the chat.

---

## The 7 Shell Tools

| Tool | Description | Example Trigger |
|---|---|---|
| `spawn_agent` | Spawn a persistent named agent with a goal | "Start an agent to monitor DeFi yields" |
| `save_to_vault` | Save content to the vault with auto-versioning | "Save this analysis to my vault" |
| `search_memory` | Semantic search across accumulated memories | "What do I already know about ETH?" |
| `create_automation` | Create DCA, price alert, or conditional automation | "Set up a DCA buying $50 of ETH every day" |
| `estimate_swap` | Preview a swap — expected output and price impact | "How much USDC do I need for 0.5 ETH?" |
| `list_agents` | List all available specialist agents | "Show me my active agents" |
| `get_wallet_balance` | Check wallet balance on Base | "What's my Base balance?" |

---

## 7 Agents

Each agent has its own persona, model routing, and tool access. The chat routes your message to the right agent based on context, or you can switch agents manually.

| Agent | Specialty | Category |
|---|---|---|
| **Noel** | Crypto analysis and DeFi | crypto |
| **CoinGecko** | Live crypto market data | crypto |
| **Sage** | Research and analysis | analysis |
| **Forge** | Developer and code | developer |
| **Quill** | Creative writing and content | creative |
| **Spectre** | Trading and execution | trading |
| **Atlas** | General-purpose | general |

Crypto-category agents (Noel, CoinGecko) get live market data injected into their context and have trade-related suggestions available.

---

## Architecture

```
User types message in webapp chat
      │
      ▼
Chat.ts (app/convex/chat.ts)
  → Routes by agentId (Noel, Sage, Forge, etc.)
  → Injects context (memory, vault, market data)
      │
      ├── If the message implies an action:
      │     ▼
      │   NoelShell.ts (app/convex/noelShell.ts)
      │     → Parses intent
      │     → Dispatches to one of 7 shell tools
      │     → Returns structured result
      │
      └── Otherwise: standard LLM response
            ▼
      Multi-provider LLM cascade
        Bankr → OpenAI → Anthropic → Groq → OpenRouter → Custom → Local fallback
```

### Multi-Provider Chat

Chat requests flow through a provider cascade. If the first provider fails or has no API key configured, the next is tried automatically:

**Bankr → OpenAI → Anthropic → Groq → OpenRouter → Custom → Local fallback**

This ensures the chat is always available regardless of which providers have keys. Override with `NOELCLAW_PROVIDER=bankr|openai|anthropic|groq|openrouter`.

---

## How It Works

### Tool Calling Flow

1. **User sends a message** — e.g., "Save my ETH analysis to the vault"
2. **Chat router** identifies the active agent and injects relevant context
3. **Intent detection** — the LLM determines the message implies an action (saving)
4. **Noel Shell dispatch** — `save_to_vault` is called with the content
5. **Execution** — the vault write happens server-side on Convex
6. **Result returned** — confirmation + vault key shown in chat

### Spawn Agent Example

```
User: "Start an agent to monitor DeFi yields and report weekly"

Noel Shell:
  → spawn_agent
    name: "yield-monitor"
    goal: "Monitor DeFi yields on Base and report weekly"
    schedule: "weekly"

Response: "Agent 'yield-monitor' spawned. It will monitor DeFi yields
on Base and report every week. You can recall it anytime with
'recall yield-monitor'."
```

### Create Automation Example

```
User: "Set up a DCA buying $50 of ETH every day at 9am"

Noel Shell:
  → create_automation
    rawInput: "Buy 50 USDC of ETH every day at 9am"

Response: "Automation created: DCA buying $50 of ETH daily at 9am.
Use 'list automations' to see status or 'pause automation' to stop."
```

### Estimate Swap Example

```
User: "How much USDC do I need for 0.5 ETH?"

Noel Shell:
  → estimate_swap
    fromToken: "USDC"
    toToken: "ETH"
    amount: "0.5"

Response: "0.5 ETH ≈ 1,250 USDC at current price. Price impact: 0.2%.
Slippage capped at 1%. Would you like to execute?"
```

---

## Security

Noel Shell tools respect the same security model as all Noelclaw tools:

- **Row-level authorization** — every tool call resolves `userId` from the session token. User identity is never trusted from client args.
- **Private key safety** — `get_wallet_balance` returns balance only, never the private key. Private keys are stored encrypted (AES-256-CBC) and never leave the server.
- **Vault isolation** — `save_to_vault` and `search_memory` are scoped to the authenticated user. Cross-user access is impossible.
- **Automation limits** — `create_automation` enforces swap limits (`MAX_AMOUNT_USD = 500`, `MIN_AMOUNT_USD = 1`) and slippage/price-impact guards.

### 8 Security Boundaries

The backend enforces 8 security boundaries across wallet, vault, chronicle, API keys, notifications, activities, agent identities, and marketplace — all with row-level auth and cross-user isolation tested.

### 4 Vulnerability Fixes (v3.29.0)

1. `getDecryptedPKByUserId` → converted to `internalAction` (was publicly callable)
2. `createWallet` → converted to `internalAction` (was publicly callable)
3. `getPrivateKey` → returns address only, never the raw private key
4. OTP verification → 5-attempt lockout prevents brute-force

---

## ConnectMcpModal

The webapp includes a **ConnectMcpModal** — an onboarding flow for connecting the MCP server to your IDE directly from the webapp. When you click "Connect to IDE", the modal:

1. Detects your OS and available MCP clients (Cursor, Windsurf, Claude Desktop, VS Code, Zed)
2. Shows the exact install command: `npx -y @noelclaw/mcp@3.29.0`
3. Provides copy-paste config snippets for each client
4. Guides you through restarting your client to activate the tools

This makes it easy to go from using the webapp to having all 103 tools available in your IDE.

---

## Theme

The webapp features a **Claude-style warm palette** with:
- **Inter font** for all UI text
- **Neural-network knowledge graph** visual on the Brain/knowledge page
- Warm beige and amber tones throughout
- Smooth Framer Motion animations

---

## Related Docs

- [Getting Started](getting-started.md)
- [Architecture](architecture.md)
- [All 103 Tools](mcp-server.md)
- [Environment Variables](env-vars.md)
