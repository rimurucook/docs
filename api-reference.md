# HTTP API Reference

Base URL: `https://valuable-fish-533.convex.site`

All endpoints return JSON. POST endpoints accept `Content-Type: application/json`.

CORS is open (`Access-Control-Allow-Origin: *`).

---

## Market Data

### `GET /mcp/market`

Returns live crypto market data via Bankr LLM gateway.

**Response:**
```json
{
  "fetchedAt": "2026-05-13T10:00:00.000Z",
  "keyPrices": {
    "bitcoin": { "usd": 103412, "usd_24h_change": 2.34 },
    "ethereum": { "usd": 3891, "usd_24h_change": 4.12 },
    "solana": { "usd": 187, "usd_24h_change": 1.87 }
  },
  "trending": [
    { "name": "Ethereum", "symbol": "ETH", "rank": 2, "change24h": 4.12 }
  ],
  "top20": [
    { "rank": 1, "name": "Bitcoin", "symbol": "btc", "price": 103412, "change24h": 2.34, "marketCap": 2000000000000 }
  ]
}
```

---

## Agent Chat

### `POST /mcp/chat`

Chat with any Noel agent.

**Body:**
```json
{
  "question": "What's your outlook on ETH this week?",
  "agentId": "noel-default",
  "messages": []
}
```

**Agent IDs:**
| ID | Agent | Model |
|----|-------|-------|
| `noel-default` | Noel (crypto AI) | gpt-5-nano |
| `gloria-default` | Gloria (AI/Web3 news) | claude-sonnet-4-6 |
| `coingecko-default` | CoinGecko data | claude-sonnet-4-6 |

**Response:**
```json
{
  "answer": "ETH is showing strong accumulation signals...",
  "agentId": "noel-default",
  "model": "gpt-5-nano"
}
```

---

## Research — Shift Management

### `POST /mcp/research`

On-demand research snapshot.

**Body:**
```json
{
  "userId": "optional-user-id"
}
```

**Response:**
```json
{
  "success": true,
  "result": {
    "fullAnalysis": "...",
    "shortSummary": "🟢 ETH leads with strong accumulation",
    "marketOutlook": "bullish",
    "impacts": [
      {
        "title": "Ethereum",
        "detail": "Whale accumulation detected...",
        "sentiment": "bullish",
        "confidence": 0.87
      }
    ],
    "generatedAt": "2026-05-13T10:00:00.000Z"
  }
}
```

---

### `POST /mcp/research`

Research any crypto topic on demand via web search.

**Body:**
```json
{
  "query": "What is happening with Ethereum ETFs?"
}
```

**Response:**
```json
{
  "success": true,
  "query": "What is happening with Ethereum ETFs?",
  "text": "**Overview**\n...\n**Key Findings**\n...\n**Sentiment**\nBullish..."
}
}
```

---

## Research — Paid Endpoint (x402)

### `POST /noel/research/paid`

Pay-per-report endpoint using the x402 protocol.

**Without `X-Payment` header → 402:**
```json
{
  "error": "Payment required",
  "paymentDetails": {
    "network": "base",
    "asset": "USDC",
    "address": "0x...",
    "amount": "1.00",
    "description": "Noel Research Report — single on-demand report"
  }
}
```

**With `X-Payment` header → 200:**
```json
{
  "success": true,
  "result": { ... },
  "token": "ETH"
}
```

**Body:**
```json
{
  "userId": "user-123",
  "token": "ETH"
}
```

---

## Wallet (MCP Users)

These endpoints manage Base mainnet wallets for MCP/agent users. Separate from the main app wallet system (which uses Privy).

### `POST /mcp/wallet/create`

Create a Base mainnet wallet. Returns existing wallet if one already exists.

**Body:**
```json
{
  "userId": "mcp-user-123"
}
```

**Response:**
```json
{
  "address": "0xabc123...",
  "existing": false
}
```

---

### `GET /mcp/wallet/balance?userId=mcp-user-123`

Get ETH and USDC balance on Base mainnet.

**Response:**
```json
{
  "address": "0xabc123...",
  "eth": "0.012450",
  "usdc": "24.50",
  "network": "base-mainnet"
}
```

---

## User Config

### `POST /user/telegram`

Set per-user Telegram bot configuration. Used to receive research reports via a personal bot.

**Body:**
```json
{
  "userId": "user-123",
  "telegramBotToken": "7123456789:AAFxxxxxxxx",
  "telegramChatId": "123456789"
}
```

Both `telegramBotToken` and `telegramChatId` are optional — you can update either independently.

**Response:**
```json
{
  "success": true
}
```

---

## Telegram Account Linking

### `POST /telegram/connect`

Link a Telegram account to a Noelclaw user (called by the Telegram bot after the user sends a link code).

**Body:**
```json
{
  "linkToken": "ABC123",
  "telegramId": "123456789",
  "telegramUsername": "satoshi"
}
```

**Response:**
```json
{
  "success": true,
  "walletAddress": "0xabc..."
}
```

---

### `GET /telegram/wallet?telegramId=123456789`

Look up the wallet linked to a Telegram ID.

**Response:**
```json
{
  "linked": true,
  "walletAddress": "0xabc...",
  "userId": "j123..."
}
```

---

## Errors

All endpoints return errors in this format:
```json
{
  "error": "description of what went wrong"
}
```

Common HTTP status codes:
| Code | Meaning |
|------|---------|
| 400 | Missing required fields |
| 402 | Payment required (x402 endpoints) |
| 404 | Resource not found |
| 409 | Conflict (e.g., Telegram already linked) |
| 500 | Internal error or missing API key |
| 502 | Upstream LLM error |
