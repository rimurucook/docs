# Wallet & DeFi

Noelclaw includes a built-in crypto wallet and DeFi execution layer, tightly integrated with the agent system.

---

## Wallet Setup

Every registered user gets an embedded wallet automatically:
- **Chain:** Base mainnet
- **Key storage:** Private key encrypted and stored in Convex DB
- **Wallet address:** Visible in the Wallet page and Profile

Users can also connect external wallets via Privy (WalletConnect, MetaMask, Coinbase Wallet).

---

## Wallet Page Features

### Token Holdings
- All Base mainnet tokens with USD value
- Dust filtering (hides tokens < $1 USD)
- 24h price change indicators
- NFT gallery tab

### Send
- Enter recipient address or ENS
- Select token + amount
- Confirms with wallet signature
- Transaction tracked in activity feed

### Swap
- Token-to-token swap
- Honeypot detection warning (flags tokens with no sell transactions)
- Slippage settings
- Uses on-chain DEX routing

### Receive
- QR code for wallet address
- Copy-to-clipboard

### Transaction History
- All deposits, swaps, sends
- Status: pending / success / failed
- Links to BaseScan

---

## USDC Balance & AI Model Access

AI model access is pay-per-use via USDC on Base. Your custodial wallet is created automatically on signup — no separate setup needed.

**Check balance:**
- Balance is shown in the Models page header as a live USDC amount
- Balance is read directly from Base mainnet (updates every 30 seconds)

**Deposit USDC:**
1. Go to Models → click **Deposit**
2. Copy your wallet address
3. Send USDC on the **Base network** from any exchange (Coinbase, Kraken, etc.)
4. Balance updates automatically within ~30 seconds of on-chain confirmation

> Send only on **Base (L2)**, not Ethereum mainnet. Gas fees are <$0.01 on Base.

**How billing works:**
- Each AI model call deducts a small amount of USDC based on tokens used
- If your balance runs low mid-request, the system auto-retries the payment (x402 protocol) without interrupting your workflow
- No prepaid bundles or credits — pure pay-as-you-go

---

## MCP Wallets (Agent / CLI Users)

Users accessing Noel through MCP clients (Hermes, Claude, Cursor) get a full DeFi wallet on Base mainnet, managed entirely through MCP tools.

Wallets are created with **ethers.js** and stored **locally** at `~/.noelclaw/wallet.json`. The private key never leaves your machine — Noelclaw's backend only receives routing calldata (0x), not your key. Non-custodial by design.

### Check your portfolio

```
get_portfolio
```

Returns all token balances and total USD value.

### Swap tokens

```
swap_tokens fromToken: "ETH" toToken: "USDC" amount: "0.1"
```

Routes through **0x Permit2** on Base mainnet. Signed locally, broadcast directly to Base. Amount is human-readable — no wei conversion needed.

### Send tokens

```
send_token token: "USDC" toAddress: "0xRecipient..." amount: "10"
```

Supports ETH, USDC, USDT, DAI, and WETH on Base.

### Analyze any wallet

```
analyze_wallet address: "0xAnyPublicAddress"
```

AI-powered analysis of any public wallet — holdings, portfolio value, behavioral profile.

### Security

- Private key lives at `~/.noelclaw/wallet.json` — local only, never transmitted
- All transactions are signed client-side before broadcast
- The Noelclaw backend provides routing (0x calldata) but never holds or sees your private key
- Non-custodial — you own your funds

---

## Telegram Trading

Users can link their Telegram account to their Noelclaw wallet and execute trades directly from Telegram messages.

**Link flow:**
1. User generates link code in Profile → Telegram Connect
2. Sends `/connect <code>` to the bot
3. Bot calls `POST /telegram/connect` with the code
4. Wallet linked to Telegram ID

**Telegram bot:**
- Reads trades from Telegram, posts to Convex
- Stored in `trades` table with `source: "telegram"`

---

## Trade Types

All trade actions tracked in `trades` table:

| Action | Description |
|--------|-------------|
| `buy` | Purchase token |
| `sell` | Sell token |
| `swap` | Token-to-token exchange |
| `send` | Transfer to address |
| `unknown` | Unparseable trade |

Sources: `web` (app UI), `x` (Twitter mentions), `telegram` (Telegram bot)

---

## X/Twitter Trading

Noelclaw monitors X mentions for trade commands:

```
@noelclaw buy 0.1 ETH
@noelclaw swap 100 USDC to SOL
```

Flow:
1. `xWebhook` receives mention
2. `xWebhookActions.ts` parses trade intent
3. Confidence score calculated
4. Stored in `xMentions` table
5. If confidence > threshold, executed

Trade stored in `xMentions`:
```
tweetId, authorHandle, authorId, text
status: "pending" | "processed" | "failed" | "ignored"
tradeId → linked trade
```
