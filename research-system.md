# Research

Noel can research any crypto topic on demand — like Perplexity but for crypto. Ask about a token, protocol, market event, or narrative and get a structured web-search-backed analysis in seconds.

---

## How It Works

When you call `research`, Noel:

1. Sends your query to the **Bankr LLM gateway** (web search + AI analysis)
2. Returns a structured report in seconds — overview, key findings, market impact, affected tokens, sentiment, and what to watch

The result is returned directly to your MCP client and optionally sent to your Telegram.

---

## Usage

### Via MCP

```
research(query: "What is happening with Ethereum ETFs?")
```

```
research(
  query: "SOL ecosystem news this week",
  userId: "your-user-id"
)
```

With `userId`, the result is also delivered to your Telegram if configured.

### Via HTTP

```bash
curl -X POST https://valuable-fish-533.convex.site/mcp/research \
  -H "Content-Type: application/json" \
  -d '{"query": "Latest news on Base ecosystem"}'
```

**Response:**

```json
{
  "success": true,
  "query": "Latest news on Base ecosystem",
  "text": "**Overview**\n...\n**Key Findings**\n...**Sentiment**\nBullish..."
}
```

---

## Report Structure

Noel returns a structured analysis in English with these sections:

| Section | What It Contains |
|---------|-----------------|
| **Overview** | Concise summary of the current situation |
| **Key Findings** | 3–5 specific, data-driven findings with numbers |
| **Market Impact** | How this affects the broader crypto market |
| **Tokens Most Affected** | Which tokens/projects are most impacted and why |
| **Sentiment** | Bullish / Bearish / Neutral with brief reasoning |
| **What to Watch** | 2–3 specific things to monitor in the coming hours/days |

---

## Telegram Delivery

Results are sent to your Telegram when you include `userId`:

```
research(
  query: "BTC halving impact on altcoins",
  userId: "your-user-id"
)
```

To set up Telegram delivery:

```
set_telegram(
  userId: "your-user-id",
  telegramBotToken: "7123456789:AAFxxxxxxxx",
  telegramChatId: "123456789"
)
```

**How to get credentials:**
1. Open Telegram → search `@BotFather` → `/newbot` → copy the bot token
2. Start a chat with your bot → send any message
3. Visit `https://api.telegram.org/bot<TOKEN>/getUpdates` → copy `chat.id`

---

## Example Queries

```
research(query: "What is the current outlook for DeFi on Base?")
research(query: "Explain the Hyperliquid airdrop controversy")
research(query: "ETH/BTC ratio technical analysis")
research(query: "Which L2s are gaining the most TVL this month?")
research(query: "Latest SEC crypto regulation news")
```

---

## Required Environment Variables

| Variable | Required | Purpose |
|----------|----------|---------|
| `BANKR_API_KEY` | Yes | Bankr Agent API — web search + AI analysis |
| `TELEGRAM_BOT_TOKEN` | Optional | Default Telegram bot (fallback) |
| `TELEGRAM_CHAT_ID` | Optional | Default Telegram chat ID (fallback) |
