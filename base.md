# Base DeFi (`base_mcp_*`)

The Base rail gives your AI a real wallet on **Base mainnet (chainId 8453)** — balances, transfers, swaps, and yield data. 11 tools, prefix `base_mcp_*`.

Your MCP wallet is generated locally on first use and stored in `~/.noelclaw` — the same address is used on Base and Robinhood Chain.

## Tools

| Tool | What it does |
|------|--------------|
| `base_mcp_status` | Wallet address, chain info, live ETH price, gas. Start any Base session here. |
| `base_mcp_network` | Real-time network stats: ETH/USD, gas in gwei, latest block. No wallet needed. |
| `base_mcp_balance` | Every ERC-20 you hold, enumerated from the Base Blockscout indexer — not a fixed allowlist. Flags impostor tokens (symbol matches a known asset but the contract address doesn't). |
| `base_mcp_send` | Send ETH or any ERC-20 to an address or basename (`jesse.base.eth`). Irreversible; requires `confirm: true`. |
| `base_mcp_estimate` | Preview a swap's output and price impact. Always call before swapping. |
| `base_mcp_swap` | Swap via 0x Protocol Permit2 (signature-based, no separate approval tx). Supported: ETH, USDC, USDT, DAI, WETH. Refuses when price impact exceeds `maxPriceImpactPct` (default 3%). |
| `base_mcp_resolve` | Basename ↔ address resolution via Coinbase's resolver, both directions. |
| `base_mcp_yield_vaults` | Morpho vaults on Base ranked by APY. |
| `base_mcp_lending_rates` | Moonwell markets: supply APY, borrow APY, liquidity, utilization per asset. |
| `base_mcp_lend` | Best lending venues for a token (Morpho + Moonwell), ranked by APY × TVL safety. Returns instructions only — never broadcasts a deposit. |
| `base_mcp_deposit_guide` | Step-by-step manual deposit instructions for a specific Morpho vault. |

Related tools outside the prefix: `get_base_token_data` (token market data on Base), `score_token` / `check_token` / `compare_tokens` (scanner), `get_wallet_address`, `get_wallet_balance`, `wallet_sign_message`.

## Safety model

- **Estimate → confirm → execute.** `base_mcp_swap` and `base_mcp_send` require `confirm: true`. A bare estimate never moves funds.
- **Price-impact guard.** Swaps refuse when impact exceeds the cap (default 3%) unless you explicitly raise it.
- **Impostor detection.** Because balances are enumerated from the indexer, `base_mcp_balance` sees scam airdrops and symbol-spoofed tokens — and flags any "USDC"-style token whose contract is not the canonical one (`0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913`).
- **Lending is read-only.** Yield tools return data and instructions; deposits are always done by you in the protocol's own UI.

## Example session

```
you:  what's my balance on base?
ai:   → base_mcp_balance
      0.0042 ETH · 12.50 USDC (canonical) · ⚠ 1 flagged token (fake "USDC" at 0x3462…)

you:  swap 5 usdc to eth
ai:   → base_mcp_estimate   (shows rate + price impact)
      → asks you to confirm
      → base_mcp_swap { confirm: true }
      ✓ tx hash + Basescan link
```
