# Packets — Reusable Workflow Flows

Packets are reusable workflow units. Define a sequence of steps once, run it on demand, and share it with others. Think of a packet as a macro: you describe a workflow in plain English, Noelclaw converts it into a named runnable unit, and you or anyone else can execute it by name.

---

## Tools

### `packet_create`

Define a new workflow packet.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `name` | string | yes | Short slug name — e.g. `"morning-brief"`, `"base-scan"` |
| `description` | string | yes | What this packet does |
| `steps` | string[] | yes | Ordered list of steps in plain English |
| `tags` | string[] | no | Tags for discovery |

**Example:**
```
packet_create
  name="morning-brief"
  description="Daily market + vault summary"
  steps=[
    "Get market overview for ETH, BTC, SOL",
    "Search vault for recent research saved this week",
    "Summarize key findings in 3 bullets"
  ]
```

---

### `packet_run`

Execute a packet by name.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `name` | string | yes | Packet name from `packet_list` |
| `context` | object | no | Runtime variables to inject into steps |

**Example:**
```
packet_run name="morning-brief"
packet_run name="base-scan" context={"minScore": 70, "mode": "dips"}
```

---

### `packet_list`

List all available packets.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `tag` | string | no | Filter by tag |

Returns name, description, step count, and last run time.

---

### `packet_share`

Share a packet so others can use it.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `name` | string | yes | Packet name to share |
| `public` | boolean | no | Make it publicly discoverable (default false) |

Returns a share URL or ID that others can use to import the packet.

---

## Use Cases

**Morning brief** — Every morning: check market, scan dips, search vault for recent notes, send summary.

**Research pipeline** — Web search → extract facts → save to vault → add chronicle entry.

**Base DeFi scan** — Get portfolio → scan dips → list Morpho vaults → recommend action.

**Agent kickoff** — Spawn a named agent → give it a context → log the spawn in chronicle.

---

## Packets vs Automations

| | Packets | Automations |
|--|---------|-------------|
| **Trigger** | On demand (you call it) | Scheduled (cron) or event-driven |
| **Steps** | Multi-step, sequential | Single rule (if/then) |
| **Shareable** | Yes | No |
| **Best for** | Research flows, recurring manual tasks | DCA, price alerts, recurring actions |

Use packets for work you want to run deliberately. Use automations for things that should run without you.
