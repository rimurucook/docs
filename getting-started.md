# Getting Started

New to Noelclaw? You'll be up and running in under 5 minutes.

---

## What is Noelclaw?

Noelclaw is an MCP skill — a plugin for AI tools like Claude, Cursor, Bankr, Aeon, and Hermes. Once installed, your AI gets 90 tools: persistent memory that carries across every session, autonomous research monitors, live market data, DeFi execution on Base, web research, token scanning, multi-agent swarms, code generation, and more.

You talk to it naturally. No commands to memorize.

---

## Step 1 — Check Node.js

Noelclaw requires Node.js 18 or newer. Check if you have it:

```bash
node --version
```

If you see `v18.x.x` or higher — you're good. If not, download it free from [nodejs.org](https://nodejs.org) (choose the LTS version).

---

## Step 2 — Install the Skill

Pick your client below. The skill downloads automatically on first use — nothing to clone or build.

### Claude Desktop

**Mac** — open this file in any text editor:
```
~/Library/Application Support/Claude/claude_desktop_config.json
```

**Windows** — open this file:
```
%APPDATA%\Claude\claude_desktop_config.json
```

Paste this (or add the `noelclaw` block if the file already exists):

```json
{
  "mcpServers": {
    "noelclaw": {
      "command": "npx",
      "args": ["-y", "@noelclaw/mcp"]
    }
  }
}
```

Save the file. **Fully quit and restart Claude Desktop** — not just close the window.

→ [Detailed Claude guide](claude-install.md)

---

### Claude Code (terminal)

```bash
claude mcp add noelclaw -s user -- npx -y @noelclaw/mcp
```

That's it. Restart Claude Code and the tools are available.

---

### Hermes

```bash
hermes mcp add noelclaw -- npx -y @noelclaw/mcp
```

Or add it to `~/.hermes/config.yaml`:

```yaml
mcp_servers:
  noelclaw:
    command: npx
    args:
      - -y
      - "@noelclaw/mcp"
    timeout: 30
```

Then type `/reload-mcp` in any Hermes session.

→ [Detailed Hermes guide](hermes-openclaw.md)

---

### Cursor / Windsurf

Edit `~/.cursor/mcp.json` (Cursor) or `~/.windsurf/mcp_config.json` (Windsurf):

```json
{
  "mcpServers": {
    "noelclaw": {
      "command": "npx",
      "args": ["-y", "@noelclaw/mcp"]
    }
  }
}
```

→ [Detailed Cursor guide](cursor-install.md)

---

## Step 3 — Try It Out

Once installed, just talk to your AI normally. No tool names needed — Noelclaw picks the right tool automatically.

**Search the web in real time:**
> "Find the latest news about AI agents today"

**Check the market:**
> "What's ETH doing right now?"

**Ask for analysis:**
> "Give me a full bull vs bear thesis on ETH"

**Set up an autonomous monitor:**
> "Monitor AI agents and automation news every morning at 8am"

**Check your vault:**
> "Show me my recent research"

**Save something to memory:**
> "Remember that I prefer low-risk DeFi and only trade on Base"

**Test memory recall in a new chat:**
> "What do you know about my trading preferences?"

**Scan for opportunities:**
> "Are there any tokens showing dip reversals right now?"

**Run a simulation:**
> "Simulate how markets would react if the Fed cuts rates 100bps"

---

## Step 4 — Autonomous Monitors (the best part)

Noelclaw can run research automatically on a schedule — no prompting needed.

```
"Monitor AI agents and automation news every morning at 8am"
```

What happens next:
- A scheduled job is registered at `daily-8am`
- Every morning, Noelclaw searches the web for your topic
- Summarizes the findings with an urgency score (1–5)
- Saves the full report to your vault
- Sends a Telegram briefing — quiet on routine days, loud on breaking news
- Compares each run to the previous one, highlighting what changed

To manage monitors:
> "What monitors do I have running?"
> "Cancel my morning brief monitor"

To get Telegram delivery, run `node worker/scripts/setup-telegram.mjs` once to connect your bot.

---

## Step 5 — Semantic Memory

Noelclaw remembers things you tell it — across sessions, across chats.

Tell it once:
> "I prefer Lido for staking and Aerodrome for LP. I avoid leverage and meme coins."

Come back in a new chat tomorrow and ask:
> "What yield strategies make sense for me?"

It already knows. No need to repeat yourself every session.

→ [Full memory guide](memory.md)

---

## Troubleshooting

| Problem | Fix |
|---------|-----|
| Tools not showing in Claude Desktop | Make sure you fully quit and restarted (not just closed the window) |
| `npx: command not found` | Node.js isn't installed — download from [nodejs.org](https://nodejs.org) |
| First message is slow | Normal — first run downloads the package (~5 seconds). Fast after that. |
| Tool call fails with auth error | Make sure you're signed in at [noelclaw.com](https://noelclaw.com) |
| Vault tools return empty | Normal on first use — start by saving something with `vault_save` |
| `create_monitor` not working | Add `TRIGGER_SECRET_KEY` to your MCP config env block |
| Monitor runs but no Telegram | Add `TELEGRAM_BOT_TOKEN` and `TELEGRAM_CHAT_ID` to your MCP config env |
