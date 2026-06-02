# Getting Started

New to Noelclaw? You'll be up and running in under 5 minutes.

---

## What is Noelclaw?

Noelclaw is an MCP skill — a plugin for AI tools like Claude, Cursor, and Hermes. Once installed, your AI gets 68 new abilities: live crypto prices, DeFi swaps on Base, persistent memory that carries across every session, 30+ LLMs in one place, token scanning, multi-agent swarms, and more.

You talk to it naturally. No commands to memorize.

---

## Step 1 — Check Node.js

Noelclaw requires Node.js 18 or newer. Check if you have it:

```bash
node --version
```

If you see `v18.x.x` or higher — you're good. If not, download it free from [nodejs.org](https://nodejs.org) (choose the LTS version).

---

## Step 2 — Sign Up

Go to [noelclaw.com](https://noelclaw.com) and create a free account. This gives you access to the web dashboard for managing automations, LLM Mart credits, and your vault.

---

## Step 3 — Install the Skill

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

## Step 4 — Try It Out

Once installed, just talk to your AI normally. No tool names needed — Noelclaw picks the right tool automatically.

**Check the market:**
> "What's ETH doing right now?"

**Ask Noel for analysis:**
> "Give me your read on Base DeFi yields this week"

**Check your portfolio:**
> "Show me my wallet balance on Base"

**Save something to memory:**
> "Remember that I prefer low-risk DeFi and only trade on Base"

**Test memory recall in a new chat:**
> "What do you know about my trading preferences?"

**Scan for opportunities:**
> "Are there any tokens showing dip reversals right now?"

**Run a simulation:**
> "Simulate how markets would react if the Fed cuts rates 100bps"

---

## Step 5 — Semantic Memory (the best part)

Noelclaw remembers things you tell it — across sessions, across chats.

Tell it once:
> "I prefer Lido for staking and Aerodrome for LP. I avoid leverage and meme coins."

Come back in a new chat tomorrow and ask:
> "What yield strategies make sense for me?"

It already knows. No need to repeat yourself every session.

→ [Full memory guide](memory.md)

---

## Step 6 — LLM Mart (Optional)

Access 30+ AI models from one place — Claude, GPT-4o, Grok, DeepSeek, Gemini, and Noel Crypto (a DeFi-native model built for Base).

Go to [noelclaw.com](https://noelclaw.com) → LLM Mart. Add USDC credits and start chatting with any model.

You also get an OpenAI-compatible API endpoint — works as a drop-in replacement for any code that uses the OpenAI SDK.

→ [LLM Mart guide](llm-mart.md)

---

## Troubleshooting

| Problem | Fix |
|---------|-----|
| Tools not showing in Claude Desktop | Make sure you fully quit and restarted (not just closed the window) |
| `npx: command not found` | Node.js isn't installed — download from [nodejs.org](https://nodejs.org) |
| First message is slow | Normal — first run downloads the package (~5 seconds). Fast after that. |
| Tool call fails with auth error | Make sure you're signed in at [noelclaw.com](https://noelclaw.com) |
| Vault tools return empty | Normal on first use — start by saving something with vault_remember |
