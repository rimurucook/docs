# Semantic Memory

Noelclaw remembers things you tell it — across sessions, across chats, across days.

This isn't just chat history. It's a vector knowledge base: your preferences, research, decisions, and strategies are indexed by **meaning**, not keywords. Ask "what's my risk tolerance?" and it finds "user avoids leverage and prefers Base-only DeFi" — even if you never used those exact words.

---

## How It Works

Every time you save something to the vault or tell Noelclaw to remember something, it gets stored in two places:

1. **Vault** — structured, versioned, searchable by keyword
2. **Semantic memory (vector index)** — indexed by meaning, searchable by intent

When you ask a question, Noelclaw searches semantic memory first. If it finds relevant entries, it uses those to answer. If not, it falls back to keyword search.

You don't need to do anything special. It just works.

---

## Basic Usage

### Save a preference
> "Remember that I prefer low-risk DeFi. Lido for staking, Aerodrome for LP on Base. No leverage, no meme coins."

### Save research
> "Remember: ETH/USDC LP on Aerodrome targets 12-18% APY. Rebalance weekly. Exit if IL exceeds 5%."

### Save your portfolio
> "Remember my allocation: 60% ETH, 25% USDC, 15% other. DCA on 15%+ dips monthly."

---

## Test Recall (Do This in a New Chat)

Close your current chat. Open a fresh one. Then ask:

> "What do you know about my DeFi strategy?"

> "Should I look at Ethereum mainnet for yields?"

> "What's my portfolio allocation?"

Noelclaw will answer correctly — pulling from memory it saved in a completely different session.

---

## Memory Tools

These tools are available directly if you want more control:

| Tool | What It Does |
|------|-------------|
| `memory_add` | Add content to semantic memory — text, notes, or auto-fetch a URL. |
| `memory_search` | Search by meaning — finds relevant entries even without exact keywords. |
| `memory_context` | Load the most relevant memories for a topic as ready-to-use AI context. |
| `memory_profile` | See your memory stats — total entries, recent activity, patterns. |
| `memory_list` | List recent memory entries, optionally filtered by tag. |
| `memory_delete` | Remove a specific memory entry by ID. |
| `memory_insight` | AI-generated insights derived from patterns across your saved memories. |
| `memory_extract` | Extract discrete facts from any block of text and save each individually. |
| `memory_consolidate` | Merge overlapping memories on a topic into one clean summary. |
| `vault_search` | Full-text keyword search across your structured vault. |

---

## Connecting External Sources

### Index a URL or GitHub repo
> "Index this into my memory: https://github.com/noelclaw/mcp"

Or directly:
> "Connect this URL to my memory: https://docs.noelclaw.com"

Noelclaw fetches the page, indexes the full content, and makes it searchable in ~30 seconds.

### Full workspace sync
For Google Drive, Gmail, and full Notion workspace sync — connect via your dashboard at [noelclaw.com](https://noelclaw.com).

---

## Cross-Session Memory: The Demo

This is the best way to see it in action.

**Session 1 (save):**
> "Remember these about me: I only trade on Base mainnet. I prefer low-risk DeFi — Lido for ETH staking, Aerodrome for LP. I never touch leverage or meme coins. Portfolio is 60% ETH, 25% USDC, 15% other. I DCA monthly on 15%+ dips."

Close the chat. Open a completely new one.

**Session 2 (recall):**
> "I'm thinking of bridging to Ethereum mainnet for some yield plays. Is that smart for me?"

Expected response: Noelclaw immediately says no — referencing your Base-only preference, your low-risk profile, and the gas cost argument. All from memory. Zero context given in this session.

---

## Tips

- **Be specific when saving** — "I prefer low-risk DeFi on Base, Lido and Aerodrome specifically" is better than "I like DeFi"
- **Memory is per-user** — your memories are isolated. Other users can't see yours.
- **No setup needed** — semantic memory is on by default for all Noelclaw users. No API key required.
- **Vault and memory are linked** — `vault_save` automatically syncs to semantic memory. You don't need to use both.
