# Agent System

Noelclaw's agent marketplace hosts 40+ specialized AI agents. Agents can be chatted with directly, automated via schedules, or accessed externally via MCP.

---

## Agent Types

| Badge | Meaning |
|-------|---------|
| `mcp` | Accessible via MCP protocol (external clients like Hermes, Claude) |
| `x402` | Requires x402 micropayment to use |
| `acp` | Anthropic Cloud Provider — uses Claude models |
| `plugin` | Data feed plugin (CoinGecko, Bankr, etc.) |

---

## Featured Agents

### Noel
> Crypto intelligence agent with DeFi trading expertise

- **Model:** gpt-5-nano (via Bankr gateway)
- **Specialty:** Market analysis, trade ideas, DeFi research, on-chain signals
- **Pricing:** 0.001 USDC per action
- **MCP tool:** `ask_noel`
- **Research:** On-demand crypto research via web search (see [Research System](research-system.md))

### Gloria AI
> AI/Web3 news and intelligence agent

- **Model:** claude-sonnet-4-6
- **Specialty:** AI agent news, Web3 ecosystem trends, project analysis, headlines
- **MCP tool:** Coming soon
- **Status:** Active in chat, MCP integration planned

### CoinGecko Agent
> Live token market data

- **Model:** claude-sonnet-4-6
- **Specialty:** Token prices, 24h changes, market cap, volume
- **MCP tool:** `get_token_data`
- **Source:** CoinGecko API

### Bankr
> On-chain signals and DeFi intelligence

- **Specialty:** Whale movements, smart money tracking, on-chain analytics
- **Integration:** Used as LLM gateway + data source for Noel's research

### Loky
> Technical analysis agent

- **Specialty:** Chart patterns, support/resistance, TA signals

### Whale Intel
> Large wallet monitoring

- **Specialty:** Whale wallet tracking, accumulation alerts

### AIXBT
> AI-native alpha agent

- **Status:** Coming soon

---

## Custom Agents

Users can create their own agents in **Create Agent** page.

**Configuration options:**
- Name, description, avatar
- Category (DeFi, News, Data, Code, etc.)
- System prompt (defines behavior)
- Model provider: Claude, ChatGPT, Gemini, Kimi, Qwen, Bankr, Custom
- Pricing: Free or token-based (USDC per action)
- Voice ID (for voice mode)
- Publish to marketplace toggle

**Database:**
```
customAgents table:
  userId, name, description, avatar
  category, systemPrompt
  voiceId, pricingType, modelId
  isPublished, rating, reviewCount
  runs, activeUsers
```

---

## Agent Chat Interface

When a user opens a chat with any agent:

1. System prompt is loaded from agent config
2. User messages go to `api.chat.chat` (Convex action)
3. Convex calls Bankr LLM gateway with the model
4. Response streams back to the UI
5. Session saved to memory (searchable later)

**Research-enabled chat:** If the user has "Noel Research" skill enabled, each chat message triggers the research orchestrator first — injecting live market context before Claude responds.

---

## Agent Runs Log

Every skill execution is logged in `agentRuns`:

```
agentRuns table:
  userId      — who triggered it
  skillName   — e.g., "noel-research"
  trigger     — "schedule" | "onDemand" | "event"
  status      — "running" | "done" | "error"
  tokensUsed  — total tokens consumed
  cost        — estimated USDC cost
  result      — JSON result object
  error       — error message if failed
  startedAt   — Unix timestamp
  finishedAt  — Unix timestamp
```

These are visible in the **Brain** page and **Memory** tab.

---

## Skill Configuration

Users install and configure skills in **Automations** page:

```
userSkillConfig table:
  userId    — owner
  skillName — e.g., "noel-research"
  config    — JSON (interval, preferences)
  enabled   — boolean
  installedAt — timestamp
```

Available skills:
- `noel-research` — 8h autonomous research
- `gloria-default` — AI news updates
- `coingecko-default` — Market data fetches
