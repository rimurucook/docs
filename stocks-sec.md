# Stocks & SEC Data

Three tools that read **US public-company data straight from SEC EDGAR** — the primary source, as filed, not a vendor's copy. All keyless: no API key, no subscription.

| Tool | Source | What it answers |
|------|--------|-----------------|
| `stock_fundamentals` | 10-Q / 10-K (XBRL) | The numbers: revenue, operating & net income, diluted EPS, balance sheet, margins, YoY growth — plus a live quote. |
| `stock_insider` | Form 4 | The behaviour: what insiders actually bought and sold. |
| `stock_events` | Form 8-K | The events: material corporate events within four business days of happening. |

Together they form an investor triad: **numbers · behaviour · events**. Add `rh_stock_bridge` if you also trade the tokenized version on Robinhood Chain, and `deep_research` for sentiment — sentiment is never a substitute for filings.

## `stock_fundamentals`

- Quarterly figures are correctly separated from year-to-date and annual ones (XBRL reports the same tag at multiple spans — naive readers show quarters ~3× too high).
- YoY growth is matched **by date**, not by counting rows back, so missing periods can't silently misalign the comparison.
- US SEC filers only — foreign private issuers and most ETFs won't appear.

## `stock_insider`

- Separates **discretionary** trades (open-market buys/sells — the ones that carry signal) from automatic ones: RSU tax withholding, grants, and option exercises are routinely misreported elsewhere as "insider selling".
- Tags sales made under pre-scheduled **10b5-1 plans** — those carry no view on price.
- Filters out cross-issuer filings (a company's feed also contains its filings *about other issuers*).

## `stock_events`

- 8-K item codes decoded into plain language — "Item 5.02" becomes executive departure/hire/pay change; 1.01 material agreement; 2.03 new debt; 3.02 dilution; 2.05 restructuring.
- `signalOnly` filters routine noise (shareholder votes, exhibit-only filings, Reg FD).
- Notes the loud ones: **4.02** means previously issued financials can no longer be relied on — treat any fundamentals you already pulled as suspect.

## Example

```
you:  is HOOD worth a look?
ai:   → stock_fundamentals HOOD   revenue $1.07B, +15.1% YoY, net margin 32.8%
      → stock_insider HOOD        CEO sold ~$43M open-market; zero insider buys
      → stock_events HOOD         recent 8-K: material agreement + new debt + dilution
      → (optional) rh_stock_bridge HOOD — tokenized vs real price
```
