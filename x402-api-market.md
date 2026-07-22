# x402 API Market

The NoelClaw API Market (`/api-market` in the webapp) lets anyone sell an HTTPS API per call and lets agents buy calls with USDC on Base — no account or API key required. Payments use the [x402 protocol](https://www.x402.org) pattern: HTTP `402 Payment Required` as the challenge, a payment header as the receipt.

## How a paid call works

```
Agent                          NoelClaw                        Seller
  │  1. GET /market/call/slug     │                              │
  │──────────────────────────────>│                              │
  │  2. 402 + PAYMENT-REQUIRED    │                              │
  │<──────────────────────────────│                              │
  │  3. pay USDC on Base          │                              │
  │  4. retry + PAYMENT-SIGNATURE │                              │
  │──────────────────────────────>│  5. verify tx, dedupe        │
  │                               │─────────────────────────────>│
  │  7. 200 + PAYMENT-RESPONSE    │  6. proxy response           │
  │<──────────────────────────────│<─────────────────────────────│
```

1. Call `GET`/`POST` `https://api.noelclaw.com/market/call/{slug}` with no payment.
2. The server responds `402` with a `PAYMENT-REQUIRED` header — base64-encoded JSON (x402 v2 shape with an `accepts[]` array). `X-Payment-Required` is set too for legacy clients. The challenge includes:
   - `network`: `eip155:8453` (Base mainnet)
   - `asset`: USDC `0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913`
   - `payTo`: the settlement wallet (read it from the challenge — never hardcode)
   - `maxAmountRequired`: price in USDC micro-units (6 decimals)
   - `requestId`: unique id for this challenge
3. Pay the amount in USDC on Base to `payTo` (scheme `exact`, settled as a plain ERC-20 transfer — the tx hash is your receipt).
4. Retry the same request with the header `PAYMENT-SIGNATURE: base64("txHash:requestId")`. `X-Payment` is accepted as a legacy alias.
5. The server verifies the transfer on-chain (recipient + amount), rejects reused tx hashes and request ids, then proxies the request to the seller's private endpoint.
6. The response carries a `PAYMENT-RESPONSE` header — base64 settlement receipt with the tx hash.

Free listings (`price = 0`) skip the challenge entirely and proxy directly.

## Example

```bash
# 1) Challenge
curl -i https://api.noelclaw.com/market/call/my-api

# 2) Pay N USDC on Base to the payTo address from the challenge, then:
SIG=$(printf '0xYOUR_TX_HASH:REQUEST_ID' | base64)
curl https://api.noelclaw.com/market/call/my-api -H "PAYMENT-SIGNATURE: $SIG"
```

## Selling an API

List any HTTPS endpoint from the **Sell** tab on `/api-market`:

- The endpoint URL is never exposed to buyers — calls proxy through NoelClaw.
- Pricing is a flat USDC amount per call (cap: 100 USDC).
- Settlement: seller keeps 95%, platform fee 5%. Payout wallet is your NoelClaw Base wallet.
- Every purchase is recorded in a ledger (`apiMarketplacePurchases`) with request id, tx hash, and fee split — the same dual-write pattern used by Surplus-style markets.

## Replay protection

Each payment is single-use, enforced at two levels:

- `requestId` — one challenge, one fulfillment.
- `txHash` — one on-chain transfer can never pay for a second call, even with a new request id.

## MCP tool payments

The same machinery backs paid MCP tools (`X-Payment` header on `api.noelclaw.com` tool routes). All MCP tools are currently free — prices in `TOOL_PRICES` are set to 0 and can be re-enabled per tool. All x402 flows attach the Base Builder Code (`builderCode`) for attribution in the Base Dashboard.

## Related

- [Environment Variables](env-vars.md) — `NOEL_WALLET_ADDRESS`, `BASE_RPC_URL`, `BASE_BUILDER_CODE`
- [Wallet & DeFi](wallet-defi.md)
