# MiroShark

MiroShark is a multi-agent social simulation engine. You describe a scenario in plain English, and MiroShark spins up a network of AI agents with distinct personas and belief systems, runs them through multiple rounds of interaction and belief propagation, and returns a behavioral analysis of how the scenario plays out across the agent population.

Use it for: market narrative modeling, social dynamics simulation, policy impact analysis, community reaction forecasting.

---

## How It Works

MiroShark runs on Railway (Hobby plan, $5/mo, 8 GB RAM). The pipeline has four stages:

### 1. Knowledge Graph Construction

MiroShark parses the scenario description and builds a knowledge graph — a structured representation of entities, relationships, and claims relevant to the scenario. This gives the agents shared factual grounding.

### 2. Persona Generation

N agent personas are generated with:
- Role (trader, analyst, retail investor, whale, developer, media, etc.)
- Initial belief state — how strongly they hold each claim in the knowledge graph
- Influence weight — how much other agents are affected by this agent's assertions
- Information access — which agents see which signals

### 3. Belief Propagation

Over M rounds, agents exchange signals, update their beliefs based on neighbor influence, and form or revise positions. This models how information spreads, narratives form, and consensus or dissent emerges.

### 4. Analysis Output

After all rounds complete, MiroShark returns:
- Consensus narrative — what the majority of agents converged on
- Dissent clusters — minority belief groups that held different views
- Signal strength — how quickly and strongly beliefs propagated
- Agent behavior summary — which persona types were most influential
- Round-by-round belief evolution (if full trace requested)

---

## Infrastructure

```
@noelclaw/mcp
      │
      ▼
api.noelclaw.com (Cloudflare Worker)
  → strips /miroshark/* prefix
  → injects admin token
      │
      ▼
Railway — MiroShark backend
  Knowledge graph builder
  Persona generator
  Belief propagation engine
  Analysis formatter
```

---

## MCP Tools

### `miroshark_simulate`

Start a new simulation. Returns a `simulation_id` immediately — the simulation runs asynchronously.

| Parameter | Type | Required | Description |
|---|---|---|---|
| `scenario` | string | yes | What to simulate — plain English description |
| `num_agents` | number | no | Number of agents (default: 20, max: 100) |
| `num_rounds` | number | no | Simulation rounds (default: 50) |

**Example:**

```
miroshark_simulate scenario: "How would crypto markets react if the US SEC approves all spot ETH ETF applications in a single day?" num_agents: 30 num_rounds: 60
```

Returns:

```json
{
  "simulation_id": "sim_abc123",
  "status": "pending",
  "scenario": "...",
  "num_agents": 30,
  "num_rounds": 60
}
```

---

### `miroshark_status`

Poll simulation progress and retrieve results when complete.

| Parameter | Type | Required | Description |
|---|---|---|---|
| `simulation_id` | string | yes | ID returned from `miroshark_simulate` |

**Status flow:**

```
pending → preparing → running → complete
```

- `pending` — queued, not started
- `preparing` — building knowledge graph and personas
- `running` — belief propagation rounds in progress
- `complete` — full results available

**Example:**

```
miroshark_status simulation_id: "sim_abc123"
```

When complete, the response includes the full analysis.

---

## Example Scenarios

**Market reaction:**
```
miroshark_simulate scenario: "Bitcoin hits $200,000 this cycle — how do different market participants react?"
```

**Regulatory impact:**
```
miroshark_simulate scenario: "The EU bans self-custody wallets — how does the crypto community respond?"
```

**Narrative spread:**
```
miroshark_simulate scenario: "A new L2 on Ethereum launches with $500M in liquidity incentives — how does the DeFi community respond?"
```

**Social dynamics:**
```
miroshark_simulate scenario: "A major crypto influencer with 5M followers claims ETH is going to zero — how does the community react?"
```

**Macro event:**
```
miroshark_simulate scenario: "The Federal Reserve cuts interest rates to 0% — how does crypto market sentiment shift over 30 days?"
```

---

## What the Output Looks Like

A completed simulation returns a structured analysis:

```
Scenario: Bitcoin hits $200,000 this cycle

Consensus Narrative:
  68% of agents converged on "accumulation phase complete, distribution beginning"
  Key drivers: whale wallet movements, exchange inflows, social sentiment shift

Dissent Clusters:
  Cluster A (18% of agents): "this is early in the bull run, further upside ahead"
  Cluster B (14% of agents): "macro factors will cause reversal before $200k"

Most Influential Personas:
  1. Whale traders (high influence weight, early belief convergence)
  2. Financial media (fast signal propagation)
  3. Retail FOMO cohort (late adopters, amplify existing trends)

Signal Strength: High — beliefs propagated to 85% of agents within 12 rounds

Belief Evolution:
  Rounds 1-10: Initial divergence, strong disagreement
  Rounds 11-25: Whale/analyst cluster gains dominance
  Rounds 26-50: Retail cluster catches up, final consensus forms
```

---

## Tips

- More agents (50-100) and more rounds (100+) give richer, more stable results at the cost of longer run time
- Specific scenarios give better results than vague ones — include numbers, entities, and timeframes
- Use `miroshark_status` in a loop to poll until status is `complete`
- Combine with `vault_save` to store simulation results for later reference
- Combine with `ask_noel` to get Noel's interpretation of the simulation output
