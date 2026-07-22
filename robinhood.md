# Robinhood Chain (`rh_*`)

Trade **tokenized stocks and any Robinhood Chain crypto** on-chain — chainId **4663**, gas in ETH, swaps through Uniswap. 14 tools, prefix `rh_*`.

> This is the **on-chain rail**. It is not Robinhood's brokerage — no Robinhood account is involved, and it is separate from the official Robinhood Agentic MCP (`agent.robinhood.com`). You are trading ERC-20 tokens that track stocks, not owning shares.

## What you can trade

- **22 tokenized stocks** (official "Robinhood Token" contracts): AAPL, AMD, AMZN, BE, COIN, CRWV, GME, GOOGL, INTC, META, MSFT, MU, NFLX, NVDA, ORCL, PLTR, QQQ, SNDK, SPCX, SPY, TSLA, USAR. List them with `rh_mcp_list_stocks`.
- **Any RH-chain crypto** by ticker or 0x contract address — resolved live via DexScreener with `rh_token_resolve`. Tickers can be spoofed (16 tokens share the ticker "ROBINHOOD"), so always trade by the confirmed contract address.

## Routing: V2 / V3 / V4 best fill

`rh_mcp_estimate` quotes **Uniswap V2, V3, and V4 in parallel** and routes through whichever returns the most output. This matters: many RH tokens hold their real depth in a V3 pool while their V4 pool sits nearly empty — single-version routing has produced fills as bad as 9% of spot on such tokens, and some tokens are *only* tradeable on V2. The estimate shows all routes considered ("Compared: V2 … · V3 … · V4 … → best wins") so a routing decision is never invisible.

Multi-hop via USDG is used automatically for thin direct pairs.

## Tools

### Trading

| Tool | What it does |
|------|--------------|
| `rh_mcp_status` | Chain status: wallet, RPC, ETH gas balance, explorer link. |
| `rh_mcp_list_stocks` | The 22 tokenized stock tickers with contract addresses. |
| `rh_mcp_balance` | ETH + every token you hold on chain 4663 (enumerated via Blockscout, so non-catalog tokens show up too). |
| `rh_token_resolve` | Ticker or 0x address → tradeable token, candidates ranked by liquidity. |
| `rh_mcp_estimate` | Preview a swap (V2/V3/V4 compared). Never executes. |
| `rh_mcp_swap` | Execute with `confirm: true`. Reports a **receipt-backed confirmation** — mined status, block, gas, and the actual amount credited from Transfer logs, not the quote. |

### Risk pre-screens (free, no LLM, no API keys)

| Tool | What it does |
|------|--------------|
| `rh_analyze` | Market screen from DexScreener: price, liquidity, volume, buy/sell flow, pair age, FDV — plus implausibility flags (FDV that can't be exited vs liquidity, wash-trade symmetry, bot churn, honeypot buy/sell patterns). 0–100 risk score. |
| `rh_safety_check` | On-chain screen from Blockscout: contract verified?, holder count, launchpad vs self-deployed (an EOA self-deploy is the real red flag on this chain — launchpads are the norm), sellability. 0–100 risk score. |

They catch **different** failure modes — run both before buying anything unfamiliar.

### Orders (DCA / take-profit / stop-loss)

| Tool | What it does |
|------|--------------|
| `rh_dca_create` | Buy a fixed ETH amount every N hours, up to `totalBuys`, hard-capped by `maxSpendEth`. |
| `rh_bracket_create` | TP and/or SL on a token you already hold; sells `sellPct`% of the live balance when DexScreener price crosses your level. |
| `rh_orders_list` | List orders and progress. |
| `rh_order_cancel` | Cancel an order. |
| `rh_orders_tick` | Evaluate all active orders. **Previews by default** — only broadcasts with `execute: true`. |

Orders are **semi-automatic by design**: they persist locally in `~/.noelclaw/rh-orders.json`, and nothing trades until you (or your agent, on your instruction) run `rh_orders_tick { execute: true }`. Kill-switch: set `RH_ORDERS_DISABLED=1` or create `~/.noelclaw/rh-orders.OFF` to block all execution instantly.

### Stock bridge

| Tool | What it does |
|------|--------------|
| `rh_stock_bridge` | Tokenized price vs the real US share price: gap %, **pool depth**, and whether the US market is open. Omit `ticker` to scan all 22. |

A gap without depth is a trap — a +19% "discount" on a $1,200 pool cannot be exited. The bridge always shows both, and notes that while the US market is closed a gap is *expected*, not a mispricing.

## Safety model

- Estimates never execute; swaps require `confirm: true`.
- Swap results are receipt-backed (mined status + actual credited amount), so a failed or reverted trade can't be mistaken for a fill.
- Trade tools always print the resolved contract address before executing — verify it, not the ticker.
- Tokens on this chain are largely launchpad-launched memecoins with no locked LP. Use `rh_analyze` + `rh_safety_check`, and size positions to pool liquidity.
