# Aeon Integration

The noelclaw skill pack is live in the Aeon ecosystem (merged PR #253). Once added, all 108 Noelclaw tools are available in Hermes and the Aeon agent runtime.

---

## What Aeon Is

Aeon is an AI agent platform with its own skill registry. Skills are MCP-compatible tool bundles that agents can call during task execution. The noelclaw skill pack brings crypto intel, DeFi execution on Base, autonomous agents, vault memory, MiroShark simulation, and the Noel Framework into Aeon.

---

## Install

### Via Aeon Skill Registry (CLI)

```bash
aeon skill add noelclaw
```

This pulls the skill from the Aeon registry and registers `@noelclaw/mcp` as the underlying MCP server.

### Manual Config

If your Aeon version requires manual config, add to your Aeon config file:

```yaml
skills:
  noelclaw:
    mcp_server:
      command: npx
      args:
        - "@noelclaw/mcp"
    enabled: true
```

### Via Hermes in Aeon

If you use Hermes inside Aeon, follow the [Hermes install guide](hermes-openclaw.md). Once the skill is registered in Hermes, Aeon agents can invoke it.

---

## Verify

In Hermes or any Aeon agent session:

```
/list-tools
```

You should see all 108 Noelclaw tools listed.

---

## Tools That Work Best in Aeon

Aeon agents are well-suited to longer autonomous tasks. These tool groups see the most usage in Aeon:

### Deep Research

Aeon agents can run multi-stage research that plans, searches, scrapes, and synthesizes - saving results to vault automatically:

```
deep_research topic: "analyze BTC market structure"
research_compare fromKey: "research/btc-q1" toKey: "research/btc-q2"
```

### Noel Vault

Vault is persistent across sessions - ideal for Aeon agents that need to store intermediate findings, plans, or outputs:

```
vault_save type: "execution" title: "Agent Output 001" content: "..." key: "agent-output-001"
vault_search query: "BTC analysis"
vault_history key: "agent-output-001"
```

### Noel Framework

Use playbooks to give Aeon agents structured, Sentinel-gated execution:

```
list_playbooks
run_playbook playbookId: "..."
get_noel_ledger
```

### MiroShark

Run social simulations as part of research or strategy workflows:

```
miroshark_simulate scenario: "How would a major exchange hack affect altcoin markets?"
miroshark_status simulation_id: "..."
```

### Market & Intel

Feed live data into agent reasoning:

```
get_market_data
get_token_data question: "PEPE, WIF, DOGE"
ask_noel question: "What narratives are gaining traction on Base?"
```

---

## Example: Aeon Agent Workflow

A typical Aeon agent task using noelclaw tools:

1. `ask_noel` - pull market analysis and current context
2. `deep_research` - run multi-stage research, auto-saves to vault
3. `agent_spawn` - create a persistent agent to track the topic
4. `vault_save` - store the analysis output
5. `create_automation` - set a recurring task based on the analysis
6. `agent_schedule` - schedule the agent to run daily

---

## Notes

- All 108 tools are available - no Aeon-specific limitations
- BYOK env vars (Grok, MiniMax) work the same way as in other clients
- See [Environment Variables](env-vars.md) for optional keys
- See [MCP Server Reference](mcp-server.md) for full tool parameter docs
