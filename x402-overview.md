# x402 Micropayments

Noelclaw implements HTTP 402 Payment Required for MCP tool access. Instead of a monthly subscription, external developers pay per call in USDC on Base mainnet. Noelclaw account holders use credits and never see a 402.

---

## Who it affects

| Caller type | Auth path |
|-------------|-----------|
| Noelclaw account holder | Session token — credits deducted, no USDC payment |
| External developer | Per-call USDC payment via `X-Payment` header |
| Free tools | No auth required — passes through unconditionally |

---

## Payment flow

```
MCP Client → Noelclaw Convex endpoint
                    │
            Read Authorization header
           ┌────────┴────────┐
   Bearer token?         No token
       │                     │
  Validate session    Read X-Payment header
       │              ┌──────┴──────┐
  Session valid?  Has payment?   No payment?
       │               │              │
   Tool runs    Verify on-chain   Return 402
                       │           + amount
               Payment valid?     + address
               ┌───────┴───────┐  + requestId
           Valid?           Invalid?
               │                │
           Record payment   Return 402
               │            (error msg)
           Tool runs
```

### Path 1 — Session token

Set `NOELCLAW_SESSION_TOKEN` in your MCP client config. Every call is authenticated against your Noelclaw credit balance. No per-call USDC payments.

### Path 2 — x402 pay-per-call

Call any paid tool without auth → receive a 402 with amount, address, and `requestId` → send exact USDC on Base → retry with `X-Payment` header. No Noelclaw account required.

### Path 3 — Free tools

Market data, signal history, whale alerts, and daily recap always return results without any auth check.

---

## 402 Response Format

When a tool requires payment and no valid auth is present:

```json
{
  "error": "Payment required",
  "paymentDetails": {
    "network": "base",
    "asset": "USDC",
    "address": "0xYourNoelWalletAddress",
    "amount": "0.10",
    "amountRaw": 100000,
    "description": "Noelclaw MCP — research",
    "requestId": "550e8400-e29b-41d4-a716-446655440000"
  }
}
```

| Field | Description |
|-------|-------------|
| `address` | Wallet to send USDC to on Base mainnet |
| `amount` | Human-readable USDC amount (string) |
| `amountRaw` | USDC in 6-decimal units (integer) |
| `requestId` | UUID — include in the `X-Payment` header; single-use |

---

## Network Details

| Property | Value |
|----------|-------|
| Network | Base mainnet |
| Chain ID | 8453 |
| Currency | USDC |
| USDC contract | `0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913` |
| Payment header | `X-Payment: base64(txHash:requestId)` |

---

## Replay attack prevention

Each `requestId` is stored in the `x402Payments` table after verification. A second request with the same `requestId` returns a 402 `"Payment already used"` immediately, before any on-chain check.

---

## On-chain verification

Noelclaw uses the Base public RPC (or `BASE_RPC_URL` if configured) to verify payments. It checks:

1. Transaction receipt status is `0x1` (success)
2. A USDC Transfer log exists from the USDC contract
3. The `to` address in the log matches `NOEL_WALLET_ADDRESS`
4. The transferred amount is ≥ the price for the requested tool

No calldata or memo is required in the transaction.

---

## See also

- [Tool Pricing](x402-pricing.md)
- [Integration Guide](x402-integration.md)
- [Troubleshooting](x402-troubleshooting.md)
