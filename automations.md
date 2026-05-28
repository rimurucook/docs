# Automations

The Automations page lets users install agent skills, enable them, and configure run schedules. Skills run autonomously in the background.

---

## Available Skills

### Noel Research
**Skill name:** `noel-research`

Enhances Noel's chat with live market context. When enabled, every chat message triggers a data fetch (market data, trends, on-chain signals) before Noel replies — making every answer data-fresh.

- **Trigger:** Automatic (on each chat message when enabled)
- **MCP tool:** `research` — for on-demand topic research
- **Data sources:** Bankr Agent (web search + AI analysis)

### Gloria AI
**Skill name:** `gloria-default`

AI/Web3 news and intelligence updates. Fetches headlines, project analysis, and ecosystem trends.

- **Status:** Active in scheduled mode
- **MCP:** Coming soon

### CoinGecko
**Skill name:** `coingecko-default`

Periodic market data snapshots — trending coins, top movers, price updates.

- **Trigger:** Scheduled interval
- **MCP tool:** `get_market_data`, `get_token_data`

---

## Automation Card

Each skill in the Automations page shows:

| Field | Description |
|-------|-------------|
| **Name** | Skill display name |
| **Status** | Running / Done / Error / Idle |
| **Toggle** | Enable/disable the skill |
| **Interval** | How often it runs (minutes) |
| **Last Run** | Timestamp of last execution |
| **Run Now** | Manually trigger the skill |

---

## Enable/Disable a Skill

Toggling a skill calls:
```typescript
api.userNotifications.toggleResearchSkill({ userId, enabled })
```

This writes to `userSkillConfig`:
```
skillName: "noel-research"
enabled: true/false
userId: ...
config: { interval: 30 }
```

---

## Manual Trigger

"Run Now" button triggers an on-demand execution. For Noel Research, this fires a single research cycle and logs the result to `agentRuns`.

For Gloria and CoinGecko, it fires a single data fetch and logs the result to `agentRuns`.

---

## Scheduled Execution

Two background crons handle automatic execution:

| Cron | Interval | Handler |
|------|----------|---------|

---

## Research-Enabled Chat

When "Noel Research" skill is enabled in Automations, every chat message to Noel automatically runs the research orchestrator first:

```
User sends message
      │
      ▼
Orchestrator runs (CoinGecko + Grok + Bankr + Claude synthesis)
      │
      ▼
Research context injected into system prompt
      │
      ▼
Noel responds with live market context
      │
      ▼
Chat shows research block + Noel's answer
```

This makes every conversation data-fresh without the user having to trigger research manually.
