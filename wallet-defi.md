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

## Credits & Balance System

Credits are the in-platform currency, separate from on-chain tokens.

**Earning credits:**
- Playing arcade games (`creditsEarned = levelReached × 100`)
- USDC deposits (1 USDC = credits via Alchemy webhook)

**Spending credits:**
- Running AI agents (deducted per action)
- Token-based agent access

**Database:** `transactions` table
```
type: "deposit" | "deduction" | "refund"
amount: number (credits)
model: which model was used
tokensUsed: LLM token count
agentId: which agent
```

---

## MCP Wallets (Agent / CLI Users)

Users accessing Noel through MCP clients (Hermes, Claude, Cursor) get a full DeFi wallet on Base mainnet, managed entirely through MCP tools. No separate signup required.

Wallets are created with **ethers.js**, encrypted with AES-256-CBC using a server-side `WALLET_ENCRYPTION_KEY`, and stored in the `mcpWallets` table. Private keys never leave the server in plaintext.

### Get portfolio (auto-creates wallet on first use)

```
get_portfolio(userId: "your-user-id")
```

Returns your wallet address and all token balances. If no wallet exists yet, one is created automatically.

### Swap tokens

```
swap_tokens(
  userId: "your-user-id",
  fromToken: "ETH",
  toToken: "USDC",
  amount: "100000000000000000"
)
```

Routes through **0x Permit2** on Base mainnet. Token approval is handled automatically when needed.

### Send tokens

```
send_token(
  userId: "your-user-id",
  toAddress: "0xRecipient...",
  token: "USDC",
  amount: "10000000"
)
```

Supports ETH, USDC, USDT, DAI, and WETH on Base.

### Via HTTP

```bash
# Get portfolio (auto-creates wallet if needed)
curl "https://valuable-fish-533.convex.site/mcp/defi/portfolio?userId=your-user-id"

# Swap
curl -X POST https://valuable-fish-533.convex.site/mcp/defi/swap \
  -H "Content-Type: application/json" \
  -d '{"userId": "your-user-id", "fromToken": "ETH", "toToken": "USDC", "amount": "100000000000000000"}'

# Send
curl -X POST https://valuable-fish-533.convex.site/mcp/defi/send \
  -H "Content-Type: application/json" \
  -d '{"userId": "your-user-id", "token": "ETH", "toAddress": "0x...", "amount": "10000000000000000"}'
```

### Security

- Private keys are encrypted with AES-256-CBC before storage
- The encryption key (`WALLET_ENCRYPTION_KEY`) lives only in Convex environment variables
- No private key is ever returned to the MCP client or user
- If a user asks for their private key via any tool, Noelclaw returns a security message instead

---

## x402 Payment Protocol

Noelclaw implements the x402 micropayment protocol for agent access:

- Agents can require payment per call
- Payment verified on-chain (Base mainnet)
- Session-based: pay once, access for N minutes
- USDC on Base

**Noel paid research endpoint:** $1.00 USDC per report (`POST /noel/research/paid`)

Without payment header: returns `402` with USDC address and amount.
With `X-Payment` header: runs the research and returns the report.

---

## Deposit via USDC (Alchemy Webhook)

Noelclaw listens for USDC deposits via an Alchemy webhook:

```
Alchemy Webhook → POST /alchemy/deposit
  → verifies USDC transfer to Noelclaw wallet
  → matches sender address to user
  → credits user balance
```

Setup requires:
- `NOEL_WALLET_ADDRESS` in Convex env vars
- Alchemy webhook configured for Base mainnet

---

## Telegram Trading (via X/Telegram Bot)

Users can link their Telegram account to their Noelclaw wallet and execute trades directly from Telegram messages.

**Link flow:**
1. User generates link code in Profile → Telegram Connect
2. Sends `/connect <code>` to the bot
3. Bot calls `POST /telegram/connect` with the code
4. Wallet linked to Telegram ID

**Telegram bot** (`noelclaw-tele`):
- Separate repository: `C:\Users\sagir\noelclaw-tele`
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
