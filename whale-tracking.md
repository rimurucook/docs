# Smart Money Tracking

Noelclaw monitors micro-cap token activity on Base chain using real DexScreener data — tracking buy/sell flow, volume spikes, and early accumulation patterns in tokens under $100k market cap.

---

## What Gets Tracked

| Signal Type | Description |
|-------------|-------------|
| Buy/sell flow | 1h buy vs sell ratio — detects aggressive accumulation |
| Volume/mcap ratio | Unusually high volume relative to market cap |
| Price momentum | 1h price change with strong buy pressure |
| New pairs | Tokens less than 24h old with significant early activity |
| Buy count | Number of buy transactions in the last hour |

**Filters applied:**
- Chain: **Base only**
- Market cap: **strictly < $100,000**
- Liquidity: **> $500 USD** (filters honeypots/rugs)
- Minimum score: **30/100** (noise reduction)

---

## How It Works

The smart money engine runs hourly via Convex cron. It fetches real on-chain data from DexScreener and scores each token.

**Flow:**

```
Convex cron (hourly)
    │
    ▼
whaleTracker.ts — internalAction
    │
    ▼
DexScreener API
  → GET https://api.dexscreener.com/token-profiles/latest/v1
  → Filter: chainId === "base", take first 20 addresses
  → Batch fetch pair data (chunks of 5)
  → Hard filter: marketCap < 100,000 AND liquidity.usd > 500
    │
    ▼
Score each pair (0–100)
  • Buy/sell ratio 1h    → up to 30 pts
  • Volume/mcap ratio   → up to 25 pts
  • Price change 1h     → up to 20 pts
  • Pair age < 24h      → 15 pts
  • Buy count > 20      → 10 pts
    │
    ▼
Filter score >= 30
    │
    ▼
Analyze top pairs with Grok (real-time X/Twitter data)
Fallback: Claude Haiku via Bankr
    │
    ▼
Save to whaleAlerts table
    │
    ▼
Send Telegram notification (if configured)
```

---

## Telegram Alert Format

```
🧠 NOEL SMART MONEY — Base Chain

⏱ Last 24h | Micro-cap < $100k

🔴 HIGH — TOKEN / BUY
Strong accumulation detected in micro-cap token
Mcap: $45,200 | 1h buys: $12,400 vs sells: $1,800
💡 Early accumulation pattern — watch for breakout

🟡 MEDIUM — TOKEN2 / BUY
New pair with unusual volume spike
Mcap: $28,000 | Volume/Mcap ratio: 0.85
💡 High volume relative to size — monitor closely
```

---

## Database

Alerts are stored in the `whaleAlerts` table:

```
token: string
direction: "BUY" | "SELL"
amountUsd: number (optional)
significance: "HIGH" | "MEDIUM" | "LOW"
description: string
implication: string
detectedAt: number
telegramSent: boolean
```

---

## Configuration

Smart money alerts run automatically every hour with no setup required. To receive them via Telegram, use `set_telegram` in your MCP client or configure it from the Profile page in the platform UI.

Alerts are saved to the database and visible in the platform regardless of Telegram config.
