# x402 Troubleshooting

---

## Error Reference

| Error / Status | Cause | Fix |
|----------------|-------|-----|
| HTTP 402 on every call | No `NOELCLAW_SESSION_TOKEN` set, or set but invalid | Get a fresh token from noelclaw.com → Settings → API and set `NOELCLAW_SESSION_TOKEN` |
| HTTP 402 with `"Payment already used"` | `requestId` was already recorded — each is single-use | Send a new USDC payment, get a new `txHash`, rebuild the header |
| HTTP 402 with `"No matching USDC transfer found"` | Wrong amount, wrong address, or sent on the wrong network | Verify the exact `amount` and `address` from the 402 response; must be on Base mainnet (chain 8453) |
| HTTP 402 with `"Transaction failed on-chain"` | The on-chain tx reverted | Check the tx on Basescan — may need more gas or token approval |
| HTTP 402 with `"Transaction not found"` | Tx not yet indexed, or wrong `txHash` | Wait 10–15 seconds for Base confirmation and retry; double-check the hash |
| HTTP 402 with `"NOEL_WALLET_ADDRESS not configured"` | Self-hosted deployment missing env var | Run `npx convex env set NOEL_WALLET_ADDRESS "0x..."` in your Convex project |
| `"Invalid payment header"` | Malformed `X-Payment` header | Use `buildPaymentHeader(txHash, requestId)` or ensure format is `base64(txHash:requestId)` |
| HTTP 402 on a tool listed as free | Tool categorized incorrectly or route not updated | Check the [pricing page](x402-pricing.md); free tools have `price === 0` in `TOOL_PRICES` |
| Tool returns result but credits not deducted | Taking the x402 path, not the session path | Set `NOELCLAW_SESSION_TOKEN` to use the credit system |
| `NOELCLAW_PAYMENT_HEADER` ignored | Session token takes priority | If `NOELCLAW_SESSION_TOKEN` is set, x402 is never triggered — remove it to test the payment path |
| High credit usage from swarm | Multiple agents calling Bankr LLM on each run | Enable BYOK in noelclaw.com → Settings to route LLM calls to your own Bankr API key |

---

## Diagnosing a 402

The 402 response body always includes a `paymentDetails` object:

```json
{
  "error": "Payment required",
  "paymentDetails": {
    "network": "base",
    "asset": "USDC",
    "address": "0x...",
    "amount": "0.10",
    "amountRaw": 100000,
    "description": "Noelclaw MCP — research",
    "requestId": "550e8400-e29b-41d4-a716-446655440000"
  }
}
```

Check:
1. `address` — are you sending to this exact address?
2. `amount` — are you sending this exact USDC amount? (6-decimal precision on-chain)
3. `requestId` — are you including this in the payment header?

---

## FAQ

**Can I use a session token and x402 at the same time?**

Session token takes priority. If `NOELCLAW_SESSION_TOKEN` is set and valid, `X-Payment` is ignored and credits are used instead.

**What if I send the wrong USDC amount?**

Payment verification fails. The transaction is not refunded. Send the correct amount in a new transaction with a new `requestId` from a fresh 402 response.

**Which USDC contract must I use?**

Only the official USDC on Base: `0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913`. Other tokens or bridged variants are not accepted.

**Can I batch multiple tool calls into one payment?**

No. Each tool call requires its own payment and unique `requestId`.

**Why does `get_swarm_status` work but `start_swarm` returns 402?**

`get_swarm_status` costs $0.01 and `start_swarm` costs $0.10. Both require auth. Check the [pricing page](x402-pricing.md) for the full list.

**Does x402 work from Claude Desktop, Cursor, or Hermes?**

Yes — any MCP client that supports `env` in its config works. Set `NOELCLAW_SESSION_TOKEN` for the simplest setup, or `NOELCLAW_PAYMENT_HEADER` for pre-authorized x402 calls.

**What's the difference between Noelclaw credits and x402?**

Credits are the internal balance system for Noelclaw account holders — topped up on the platform and deducted per call. x402 is for external callers who pay directly on-chain in USDC with no account required.

**How long does on-chain verification take?**

Verification uses `eth_getTransactionReceipt` against the Base RPC. If the transaction is confirmed, it typically completes in under a second. If the tx is pending (not yet in a block), verification will fail — wait for Base confirmation (~2 seconds) and retry.

---

## See also

- [Overview & Payment Flow](x402-overview.md)
- [Tool Pricing](x402-pricing.md)
- [Integration Guide](x402-integration.md)
