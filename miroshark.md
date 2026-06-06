# MiroShark

MiroShark is a multi-agent social simulation engine. Describe any scenario in plain English — MiroShark spins up a network of AI agents with distinct personas and belief systems, runs them through multiple rounds of interaction and belief propagation, and returns a behavioral analysis of how the scenario plays out.

Use it for: market narrative modeling, social dynamics simulation, regulatory impact analysis, community reaction forecasting.

---

## How It Works

MiroShark runs on a dedicated high-memory backend. The pipeline has four stages:

### 1. Knowledge Graph Construction

MiroShark parses the scenario and builds a knowledge graph — a structured representation of entities, relationships, and claims. This gives all agents shared factual grounding.

### 2. Persona Generation

Agent personas are generated with:
- **Role** — trader, analyst, retail investor, whale, developer, media, skeptic
- **Initial belief state** — how strongly they hold each claim in the knowledge graph
- **Influence weight** — how much other agents are affected by their assertions
- **Information access** — which agents see which signals first

### 3. Belief Propagation

Over multiple rounds, agents exchange signals, update beliefs based on neighbor influence, and form or revise positions. This models how information spreads, narratives form, and consensus or dissent emerges — similar to real social dynamics.

### 4. Analysis Output

After rounds complete, MiroShark returns:
- Consensus narrative — what the majority converged on
- Dissent clusters — minority belief groups that held different views
- Signal strength — how quickly and strongly beliefs propagated
- Agent behavior summary — which persona types were most influential
- Round-by-round action log (Twitter and Reddit simulated activity)

---

## Infrastructure

```
@noelclaw/mcp
      │
      ▼
api.noelclaw.com
  → routes to MiroShark backend
      │
      ▼
MiroShark backend (8 GB RAM)
  Knowledge graph builder
  Persona generator
  Belief propagation engine
  Analysis formatter
```

---

## MCP Tools

### `miroshark_simulate`

Start a new simulation. Returns a `simulation_id` — the simulation runs asynchronously in the backend.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `scenario` | string | yes | What to simulate — plain English, any topic |

Agent count and number of rounds are determined automatically by MiroShark based on scenario complexity. A 24-hour scenario generates ~48 rounds; a 7-day scenario generates ~168 rounds.

**Example:**
```
run a miroshark simulation: "Bitcoin breaks $125,000 — how does the crypto community react over 24 hours?"
```

Returns a `simulation_id`. Pass it to `miroshark_status` to track progress.

---

### `miroshark_status`

Poll simulation progress and retrieve results when complete.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `simulation_id` | string | yes | ID from `miroshark_simulate` |

**Status flow:**
```
preparing → running → complete
```

- `preparing` — building knowledge graph and generating agent personas
- `running` — belief propagation rounds in progress, actions firing
- `complete` — full results available

When `running`, the response shows current round, total rounds, and action counts:
```
Round: 18 / 48 (37.5%)
Actions: 43 Twitter · 50 Reddit
```

Poll every 30-60 seconds. Preparation typically takes 3-5 minutes before rounds start.

---

### `miroshark_stop`

Stop a simulation that is currently preparing or running.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `simulation_id` | string | yes | Simulation ID to stop |

---

## Example Scenarios

**BTC price milestone:**
```
run a miroshark simulation: "Bitcoin breaks $125,000 for the first time — how does the crypto community react over 24 hours?"
```

**Regulatory event:**
```
run a miroshark simulation: "The EU bans self-custody wallets — how does the crypto community respond?"
```

**Market crash:**
```
run a miroshark simulation: "ETH drops 40% in 24 hours following a major hack — what happens to DeFi protocols and community sentiment?"
```

**Narrative spread:**
```
run a miroshark simulation: "A new L2 launches with $500M in liquidity incentives — how does the DeFi community respond?"
```

**Macro impact:**
```
run a miroshark simulation: "The Federal Reserve cuts rates to 0% — how does crypto market sentiment shift over 30 days?"
```

---

## Tips

- **Specific scenarios give better results** — include timeframes, numbers, and named entities
- **Poll with patience** — agent preparation takes 3-5 minutes before `running` starts; first rounds can be slow to initialize
- **You don't need to wait for completion** — Round 10-20 is usually enough for a meaningful snapshot
- **Combine with vault** — use `vault_save` to store results for later reference or comparison
- **Combine with ask_noel** — after getting results, ask Noel to interpret them in context of current market conditions
- **Use miroshark_stop** if a simulation is taking too long or you want to start a new scenario
