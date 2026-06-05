# Install on Hermes

Add Noelclaw as an MCP skill in Hermes. Once connected, all 76 tools are available directly in your agent conversations.

No build step needed — runs via `npx`.

**Requirement:** Node.js >= 18. Check with `node --version`. Download from [nodejs.org](https://nodejs.org) if needed.

---

## Method 1 — CLI (Fastest)

```bash
hermes mcp add noelclaw -- npx -y @noelclaw/mcp
```

Reload without restarting Hermes:

```
/reload-mcp
```

---

## Method 2 — Config File

Edit `~/.hermes/config.yaml`:

```yaml
mcp_servers:
  noelclaw:
    command: npx
    args:
      - -y
      - "@noelclaw/mcp"
    timeout: 30
    connect_timeout: 15
```

Then run `/reload-mcp` in any Hermes session.

---

## Verify Tools Are Loaded

```
/list-tools
```

You should see all 76 tools listed — including `get_market_data`, `memory_search`, `swap_tokens`, `noel_boot`, `miroshark_simulate`, and more.

---

## Try It Out

Just talk naturally — Hermes picks the right tool automatically.

**Live market data:**
> "What's the crypto market looking like right now?"

**Save something to memory:**
> "Remember that I prefer low-risk DeFi on Base — Lido and Aerodrome only"

**Test memory recall in a new session:**
> "What do you know about my trading preferences?"

**Ask Noel for analysis:**
> "What's your take on ETH liquid staking yields this week?"

**Swap on Base:**
> "Swap 0.01 ETH to USDC on Base"

**Scan for opportunities:**
> "Are there any tokens showing dip reversals right now?"

**Run a simulation:**
> "How would markets react if a major stablecoin depegged?"

---

## Semantic Memory

Noelclaw remembers things you tell it — across different sessions.

**Session 1:**
> "Remember: I only trade on Base, I prefer Lido for staking, I avoid leverage"

**New session later:**
> "Should I bridge to Ethereum mainnet for some yields?"

Hermes will answer based on your saved preferences — "probably not, your profile says Base-only" — without you repeating anything.

→ [Full memory guide](memory.md)

---

## Troubleshooting

| Problem | Fix |
|---------|-----|
| Tools not showing after `/reload-mcp` | Check Node.js 18+ is installed: `node --version` |
| `connect_timeout` errors | Increase to `connect_timeout: 20` — first run downloads the package |
| `npx: command not found` | Set full path: `command: /usr/local/bin/npx` — find it with `which npx` |
| Auth error on tool calls | Wallet auto-generates on first use — no sign-in needed. Try restarting Hermes. |
| Slow first response | Normal — first run downloads `@noelclaw/mcp` (~5 seconds). Fast after. |
