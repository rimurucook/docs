# Chronicle - Audit Trail

Chronicle is an append-only log. Every entry you add is timestamped and permanent - nothing is updated or deleted. Use it to record decisions, track what happened in a session, or keep a running journal that accumulates across all your AI interactions.

---

## Tools

### `chronicle_search`

Search the audit trail by keyword.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `query` | string | yes | Keyword to search across event titles and details |
| `limit` | number | no | Max results to return (default 100) |

**Example:**
```
chronicle_search query="ETH"
chronicle_search query="Morpho" limit=20
chronicle_search query="swap"
```

Returns matching events with timestamps. Scans the most recent 100 entries.

---

### `chronicle_stats`

Runtime activity analytics over a time window.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `days` | number | no | Window in days (default 30, max 90) |

**Example:**
```
chronicle_stats
chronicle_stats days=7
chronicle_stats days=90
```

Returns: event count by type, daily heatmap, busiest days, average events per day.

---

### `chronicle_add`

Add an entry to the audit trail.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `content` | string | yes | What to record - a decision, finding, note, or event |
| `tags` | string[] | no | Tags for filtering later |
| `source` | string | no | Where this came from - e.g. `"swarm"`, `"manual"`, `"automation"` |

**Example:**
```
chronicle_add content="Decided to pause ETH automation - volatility too high" tags=["defi","decision"]
chronicle_add content="Swarm found 3 new Base protocols worth tracking" source="swarm"
chronicle_add content="Vault knowledge graph now has 47 linked entries"
```

Every entry is timestamped on the server - you can't backdate or edit.

---

### `chronicle_list`

Read the audit trail.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `limit` | number | no | Max entries to return (default 50) |
| `tag` | string | no | Filter by tag |
| `since` | string | no | ISO date - only entries after this date |

**Example:**
```
chronicle_list limit=20
chronicle_list tag="decision"
chronicle_list since="2026-06-01"
```

---

## What to Use It For

**Decision log** - Record every significant choice: why you paused an automation, why you pivoted on a trade idea, why an agent was marked complete.

**Research trail** - When swarm or monitor saves findings to vault, also add a chronicle entry: "Swarm found X on topic Y - see vault key research/xyz".

**Session summary** - At the end of a working session: "Completed: linked 5 vault entries, spawned 2 agents, set up morning monitor."

**AI accountability** - Every tool call that matters leaves a trace. Over time, chronicle becomes a record of what your AI actually did.

---

## Chronicle vs Vault

| | Chronicle | Vault |
|--|-----------|-------|
| **Structure** | Append-only log entries | Versioned key-value artifacts |
| **Editable** | Never | Yes (creates new version) |
| **Best for** | Events, decisions, what happened | Research, plans, documents, credentials |
| **Search** | Filter by tag or date | Full-text + semantic search |

Use vault to store the *content*, use chronicle to record that it happened.
