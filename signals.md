# Trading Signals

Noelclaw generates daily 4H trading signals for BTC and ETH using a multi-indicator A+ scoring system. Only high-conviction setups fire — most days return HOLD. Signals are tracked automatically and a weekly recap is generated every Sunday.

---

## How Signals Work

Signals are generated once per day at **08:00 UTC** using real OHLCV data from Bybit/OKX (4H candles, 90 periods). Each signal is scored across multiple dimensions — only A+ setups (score ≥ 70/100) generate BUY or SELL signals.

| Field | Description |
|-------|-------------|
| Token | BTC or ETH |
| Signal | BUY, SELL, or HOLD |
| Entry price | Current price at signal generation |
| TP1 / TP2 | Take-profit targets (minimum 2:1 R:R enforced) |
| Stop loss | Risk management level |
| Confidence | A+ score (0–100) |
| Reasoning | AI-generated rationale via Bankr LLM |
| Timeframe | 4H |

---

## Signal Engine — A+ Scoring System

The engine (`signalEngine.ts`) calculates six indicators and scores the setup:

### Indicators
| Indicator | Method |
|-----------|--------|
| RSI | 14-period RSI on 4H closes |
| Sigma extension | How many standard deviations price is from VWAP |
| Volume Profile | 60-bucket distribution → POC, VAH, VAL (70% value area) |
| Support & Resistance | Pivot detection (lookback=4) with 0.5% clustering |
| EMA trend | EMA20 vs EMA50 — trend direction filter |
| Volume surge | Current volume vs 20-period average |

### A+ Scoring (max 100 pts)
| Condition | Points |
|-----------|--------|
| RSI extreme (≤30 for BUY, ≥70 for SELL) | up to 35 |
| Sigma extension (price far from VWAP) | up to 25 |
| Price near Support/Resistance | up to 20 |
| Price near Volume Profile POC/VAL/VAH | up to 20 |
| Volume surge (>1.5x average) | up to 10 |
| EMA trend alignment | up to 10 |

**Threshold: score ≥ 70 → A+ setup → signal fires**
**Score < 70 → HOLD (no signal, no Telegram)**

### Risk/Reward
Minimum 2:1 R:R enforced on all signals. TP1 is always at least `entry + (risk × 2.0)`.

---

## Signal Generation Flow

```
Convex cron — 08:00 UTC daily
    │
    ▼
Fetch 4H OHLCV (90 candles)
  → Bybit primary
  → OKX fallback
  → CoinGecko fallback (14-day daily)
    │
    ▼
Calculate indicators
  RSI · VWAP · StdDev · Volume Profile · S/R · EMA20/50
    │
    ▼
Score setup (A+ if ≥ 70)
    │
    ▼
If A+: derive signal (BUY/SELL)
If not: HOLD — stop here
    │
    ▼
Generate reasoning via Bankr LLM (gpt-5.4-mini)
    │
    ▼
Save to tradingSignals table
    │
    ▼
Send Telegram (A+ only)
```

---

## Telegram Signal Card

```
⚡️ NOEL A+ SIGNAL — BTC

📊 Timeframe: 4H
🟢 Signal: BUY
💵 Entry: $103,000
🎯 TP1: $106,500 (+3.40%)
🎯 TP2: $109,000 (+5.83%)
🛑 Stop Loss: $101,000 (-1.94%)
📈 Confidence: 82/100

📊 Volume Profile
  POC: $102,800 | VAH: $105,200 | VAL: $100,900

📐 Key Levels
  Support: $101,200 | Resistance: $106,800

📝 Reasoning:
RSI at 31 with price touching VAL on high volume.
EMA20 above EMA50 confirms uptrend. A+ setup.
```

---

## Outcome Tracking

Every 30 minutes, the outcome tracker checks open signals:

- **Win**: Price hit TP1 before stop loss → `WIN`, PnL calculated
- **Loss**: Price hit stop loss → `LOSS`, PnL calculated
- **Expired**: Signal older than 6 hours with no outcome → `EXPIRED`

---

## Weekly Recap

Every Sunday at **23:55 UTC**, a full weekly recap is generated:

- Full 7-day signal log (BTC and ETH)
- Total wins, losses, win rate
- Best and worst performing signals (PnL)
- AI-written performance review

---

## Database

| Table | Purpose |
|-------|---------|
| `tradingSignals` | Individual signals with outcome tracking |
| `weeklyRecaps` | Sunday recap with full 7-day log |

**`tradingSignals` fields:**
```
token: "BTC" | "ETH"
signalType: "BUY" | "SELL" | "HOLD"
entryPrice, target1, target2, stopLoss: number
confidence: number (0–100)
timeframe: "4H"
reasoning: string
poc, vah, val: number (Volume Profile)
nearestSupport, nearestResistance: number
isAPlus: boolean
status: "active" | "win" | "loss" | "expired"
pnlPercent: number
telegramSent: boolean
generatedAt: number
```

---

## Configuration

Signals run automatically — no setup required. To receive them via Telegram, use `set_telegram` in your MCP client or configure it from the Profile page in the platform UI.

Signals are saved to the database and visible in the platform regardless of Telegram config.
