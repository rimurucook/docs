# x402 Integration Guide

Three ways to authenticate with Noelclaw MCP tools. Free tools work without any setup.

---

## Option 1 — Session Token (Recommended)

Get your session token from noelclaw.com → Settings → API. Set it as an env var when running the MCP server.

```bash
NOELCLAW_SESSION_TOKEN=your_token_here npx @noelclaw/mcp
```

Or in your MCP client config:

```json
{
  "mcpServers": {
    "noelclaw": {
      "command": "npx",
      "args": ["@noelclaw/mcp"],
      "env": {
        "NOELCLAW_SESSION_TOKEN": "your_token_here"
      }
    }
  }
}
```

All tool calls run against your Noelclaw credit balance. No per-call USDC payments.

---

## Option 2 — Pay Per Call (x402)

For callers without a Noelclaw account. No registration required — just send USDC on Base.

### Step 1 — Call the tool

Call any paid tool without auth. The server responds with HTTP 402:

```json
{
  "error": "Payment required",
  "paymentDetails": {
    "network": "base",
    "asset": "USDC",
    "address": "0xNoelWalletAddress",
    "amount": "0.10",
    "amountRaw": 100000,
    "description": "Noelclaw MCP — research",
    "requestId": "550e8400-e29b-41d4-a716-446655440000"
  }
}
```

Note the `requestId` — you need it in step 3.

### Step 2 — Send payment

Send the exact USDC amount in `paymentDetails.amount` to `paymentDetails.address` on Base mainnet (chain ID 8453).

No transaction calldata or memo is required. The server verifies by matching the on-chain Transfer log.

### Step 3 — Build the payment header

```typescript
// Using the built-in helper from the MCP server package
import { buildPaymentHeader } from '@noelclaw/mcp';

const header = buildPaymentHeader(txHash, requestId);
// → base64("0xYourTxHash:550e8400-e29b-41d4-a716-446655440000")
```

Or build it manually:

```typescript
const header = Buffer.from(`${txHash}:${requestId}`).toString('base64');
```

### Step 4 — Retry with the header

```http
POST /mcp/research HTTP/1.1
X-Payment: <base64_header>
Content-Type: application/json

{ "query": "ETH momentum" }
```

The server verifies on-chain, records the payment, and returns the tool result.

### Pre-authorize via env var

To attach a payment header automatically on every request, set it in your MCP config:

```json
{
  "env": {
    "NOELCLAW_PAYMENT_HEADER": "base64(txHash:requestId)"
  }
}
```

Each `requestId` is single-use. Clear `NOELCLAW_PAYMENT_HEADER` after the call succeeds or it will return `"Payment already used"` on the next request.

---

## Option 3 — BYOK (Bring Your Own Bankr Key)

For heavy swarm users. Routes Bankr LLM calls to your own Bankr API key instead of the shared Noelclaw key.

Enable in noelclaw.com → Settings → toggle **"Use my own Bankr API key"** → enter your Bankr API key.

Best suited for:

- Long research shifts (Noel Shift, 8h+)
- Active swarm sessions with multiple agents running concurrently
- High-volume automation workflows with frequent LLM calls

This does not bypass x402 — it only affects which Bankr account is billed for LLM inference.

---

## Full x402 flow in Node.js

```typescript
import { buildPaymentHeader } from '@noelclaw/mcp';

const CONVEX_SITE = 'https://valuable-fish-533.convex.site';

async function callWithPayment(path: string, body: unknown, walletSendUsdc: Function) {
  // First attempt
  const res = await fetch(`${CONVEX_SITE}${path}`, {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify(body),
  });

  if (res.status !== 402) {
    return res.json();
  }

  // Extract payment details from 402 response
  const { paymentDetails } = await res.json();
  const { address, amount, requestId } = paymentDetails;

  // Send USDC on Base (implement walletSendUsdc with your wallet library)
  const txHash = await walletSendUsdc({
    to: address,
    amountUsdc: parseFloat(amount),
    chainId: 8453,
  });

  // Retry with payment proof
  const retryRes = await fetch(`${CONVEX_SITE}${path}`, {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
      'X-Payment': buildPaymentHeader(txHash, requestId),
    },
    body: JSON.stringify(body),
  });

  return retryRes.json();
}

// Usage
const result = await callWithPayment('/mcp/research', { query: 'ETH momentum' }, sendUsdc);
```

---

## Environment Variables Reference

| Variable | Where set | Description |
|----------|-----------|-------------|
| `NOELCLAW_CONVEX_URL` | MCP client config | Convex deployment URL (default: hosted Noelclaw backend) |
| `NOELCLAW_SESSION_TOKEN` | MCP client config | Session token for credit-based auth — bypasses all x402 checks |
| `NOELCLAW_PAYMENT_HEADER` | MCP client config | Pre-built payment header (`base64(txHash:requestId)`) — single use |

### Convex server-side vars (self-hosted deployments only)

| Variable | Description |
|----------|-------------|
| `NOEL_WALLET_ADDRESS` | Wallet address that receives USDC payments — set via `npx convex env set` |
| `BASE_RPC_URL` | Base mainnet RPC for on-chain verification (default: `https://mainnet.base.org`) |

---

## See also

- [Tool Pricing](x402-pricing.md)
- [Overview & Payment Flow](x402-overview.md)
- [Troubleshooting](x402-troubleshooting.md)
