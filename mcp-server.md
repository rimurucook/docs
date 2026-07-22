# All 121 Tools

> This page is generated from the live tool registry (`ALL_TOOLS`) of `@noelclaw/mcp` — every tool below exists exactly as named, with its real description. Nothing here is aspirational.

**121 tools.** Install once, they all load — no per-tool setup. Tools that move funds require `confirm: true` and never execute from a bare estimate.

## Semantic Memory (10)

Persistent memory with semantic search, time-decay, and dedupe. Works against Noelclaw cloud or a free self-hosted supermemory server (`noelclaw setup`).

| Tool | What it does |
|------|--------------|
| `memory_add` | Add content to your Noelclaw semantic memory - no setup needed, no extra API keys. |
| `memory_search` | Hybrid memory search - fuses semantic (embedding) + lexical (full-text BM25) retrieval via Reciprocal Rank Fusion. |
| `memory_context` | Retrieve the most semantically relevant memories for a topic, formatted as AI-ready context. |
| `memory_profile` | Show your semantic memory stats - total memories stored, your memory space, and connected sources. |
| `memory_list` | List your most recent Noelclaw memories without a search query. |
| `memory_delete` | PERMANENT. Delete a specific memory by its ID — it cannot be recovered. Get IDs from memory_search or memory_list. Requires confirm: true. Show the user which memory you are about to delete (title/content) and get… |
| `memory_insight` | Get a full intelligence report on any topic - combines semantic memory AND vault entries, then identifies knowledge gaps and suggests next actions. |
| `memory_extract` | Save discrete facts, preferences and decisions to semantic memory as individually searchable atoms instead of one wall of text. |
| `memory_publish` | IRREVERSIBLE, PUBLIC. Publish a memory snippet to the Memory Marketplace — visible to ALL Noelclaw users at /memory-marketplace. Reversible with vault_unpublish, but only for future discovery — anyone who already… |
| `memory_consolidate` | Clean up fragmented knowledge after heavy research sessions. |

## Vault (15)

Git-style versioned notes with knowledge-graph links, tags, credentials, and export. Credentials are excluded from list/search/export and only readable by exact name.

| Tool | What it does |
|------|--------------|
| `vault_save` | Save or update a versioned artifact in Noel-Vault. |
| `vault_read` | Read a Noel-Vault entry by its key. Returns full content, version, tags, and any linked entries. |
| `vault_list` | List Noel-Vault entries. Filter by type, agent, or pinned status. Returns previews, not full content. |
| `vault_search` | Search Noel-Vault using semantic AI search (powered by Supermemory) when available, with automatic fallback to full-text search. |
| `vault_history` | Get the full version history of a Noel-Vault entry - like git log. |
| `vault_diff` | Compare two versions of a Noel-Vault entry - like git diff. |
| `vault_export` | Export your entire Noel-Vault or a specific type as a structured bundle. |
| `vault_store_credential` | Securely store an API key, token, or secret in your vault. |
| `vault_get_credential` | Retrieve a stored credential from the vault by name. |
| `vault_pin` | Pin or unpin a Noel-Vault entry. Pinned entries always appear first in vault_list and are prioritized in memory_context and search results. Use for your most important research, key prompts, or canonical references. |
| `vault_unpublish` | Make a previously shared Noel-Vault entry private again, removing it from the public community listing. |
| `vault_delete` | PERMANENT. Delete a Noel-Vault entry and ALL of its version history — this cannot be undone. Requires confirm: true. Use vault_list to browse first, and show the user the exact entry (key + title) you are about to… |
| `vault_tag` | Add or replace tags on an existing Noel-Vault entry without modifying its content. |
| `vault_link` | Create a semantic relationship between two Noel-Vault entries - building a knowledge graph. |
| `vault_related` | Traverse the Noel-Vault knowledge graph - get all entries linked to a given entry. |

## Chronicle (Audit Trail) (4)

Append-only log of everything the runtime does, searchable by keyword with activity stats.

| Tool | What it does |
|------|--------------|
| `chronicle_add` | Log an event to Noel Chronicle - the system-wide audit log for your AI runtime. |
| `chronicle_list` | Read the Noel Chronicle event log - your AI runtime timeline. |
| `chronicle_search` | Search the Noel Chronicle by keyword. Matches against event titles and details. Useful for finding when something specific happened: 'when did I last research ETH?' or 'find all vault saves for Base'. |
| `chronicle_stats` | Activity stats for your AI runtime - breakdown by event type, daily activity heatmap, busiest days, and most active categories. |

## Agents (12)

Named persistent agents that survive across sessions, accumulate learnings, and can run on a schedule.

| Tool | What it does |
|------|--------------|
| `list_agents` | List all available specialist agents you can hire - built-in experts (analyst, risk-manager, researcher, executor, scout) plus any community-published agents. |
| `hire_agent` | Load a specialist agent's expertise and apply it to a task YOURSELF — the tool returns the agent's full persona (its framework, thresholds and house rules) scoped to your task, and you answer in that voice. |
| `agent_spawn` | Create a persistent NAMED agent with a goal - survives across sessions, state saved to vault under `agent/<name>` key. |
| `agent_recall` | Recall a persistent agent by name - loads its goal, current progress, findings, full history, and accumulated learnings (patterns the agent extracted from past runs). |
| `agent_update` | Update a persistent agent's progress and findings. |
| `agent_identity` | Get or create a persistent on-chain identity (wallet address) for a named agent. |
| `agent_ledger` | View the full activity ledger for a persistent agent - every update, status change, and finding logged in order. |
| `agent_schedule` | Attach an autonomous schedule to an existing agent. |
| `agent_unschedule` | Remove the autonomous schedule from an agent. |
| `agent_pause` | Pause an agent's autonomous schedule without deleting it. |
| `agent_resume` | Re-enable a paused agent's schedule. Resets the consecutive-failure counter. |
| `agent_runs` | View recent autonomous run history for a scheduled agent - when it ran, success/failure status, vault key where the output was saved, and duration. |

## Playbooks (3)

Curated multi-step playbooks executed through the Noel Framework with a full audit ledger.

| Tool | What it does |
|------|--------------|
| `list_playbooks` | List available Noel Framework playbooks - predefined multi-step workflows. |
| `run_playbook` | Execute a Noel Framework playbook. Each step runs through Sentinel before the matching tool executes it. Steps map directly to noelclaw tools (market, vault, agent, memory, automation). Playbook halts immediately if… |
| `get_noel_ledger` | Get the Noel Framework audit trail - every Sentinel gate decision (approved / blocked / warned), which checks ran, duration, and reason. |

## Automations (6)

Scheduled and conditional workflows: DCA, price alerts, conditional buy/sell. Dry-run supported.

| Tool | What it does |
|------|--------------|
| `create_automation` | Create an automation in plain English. Supports DCA, price alerts, conditional buys/sells, and recurring market updates. |
| `list_automations` | List all your automations - active, paused, and completed - with status, run counts, and next scheduled run. |
| `pause_automation` | Pause or resume an automation by ID. |
| `delete_automation` | PERMANENT. Delete an automation — this cannot be undone. Requires confirm: true. Run list_automations first and show the user which automation (id + name) you are about to remove. If they only want it to stop… |
| `get_automation_runs` | Get the execution history for an automation - each run's status (success/failed/skipped), amount spent, tx hash, and error message if any. |
| `run_automation` | Trigger an automation immediately - regardless of its schedule or trigger condition. |

## Monitors (3)

Recurring research monitors that run after you close the chat.

| Tool | What it does |
|------|--------------|
| `schedule_research` | Schedule recurring autonomous research on any topic - runs on a cron schedule, saves findings to vault, and sends a Telegram notification. |
| `list_monitors` | List all active scheduled research monitors - shows topic, schedule, next run, and monitor ID. |
| `cancel_monitor` | PERMANENT. Cancel and delete a scheduled research monitor — the schedule is removed and cannot be restored. Requires confirm: true. Run list_monitors first and show the user which monitor (id + topic) you are about… |

## Packets (Flows) (4)

Reusable multi-step workflow packets: create, run, list, share.

| Tool | What it does |
|------|--------------|
| `packet_create` | Create or update a Packet - a named, reusable AI workflow stored in your vault. |
| `packet_run` | Load and execute a Packet by name. Returns all steps formatted for sequential execution. After calling this, execute each step in order - tool steps are called directly, prompt steps are interpreted as instructions. |
| `packet_list` | List all your Packets - reusable workflows stored in vault. |
| `packet_share` | Publish a Packet to the community so others can discover and use it. |

## Deep Research (1)

Multi-angle web research. Default `mode: "sources"` returns a cited evidence pack with zero API keys; `mode: "report"` adds server-side synthesis (needs an LLM key).

| Tool | What it does |
|------|--------------|
| `deep_research` | Web research engine: searches, scrapes, ranks and de-duplicates sources, then returns them as a numbered, citable evidence pack for YOU to synthesise. |

## Research Compare (1)

Compare two saved research reports.

| Tool | What it does |
|------|--------------|
| `research_compare` | Diff two vault research reports. Two-pass, no API key needed. PASS 1 — call with keyA + keyB: loads both reports, extracts their section structure, and returns both bodies plus the comparison rubric for YOU to write.… |

## Research Chain (1)

Walk a topic's research history across time.

| Tool | What it does |
|------|--------------|
| `research_chain` | Walk a research topic's timeline. Follows `continues` relations both backward and forward from a starting report and returns the chronological list with each report's date/title/TL;DR, plus the rubric for YOU to… |

## Web Research (2)

Live web search and page scraping (proxied — no Firecrawl key needed).

| Tool | What it does |
|------|--------------|
| `web_scrape` | Fetch and extract clean readable content from any URL - returns markdown. |
| `web_search` | Search the web and return raw results: titles, URLs, and snippets. |

## Market Data (6)

Live crypto market data and structured market-thesis rubrics from verified price sources (CoinGecko, DexScreener, Pyth cross-checked).

| Tool | What it does |
|------|--------------|
| `get_market_data` | Get live crypto market data: top 20 coins by market cap, trending coins, and key prices for BTC/ETH/SOL. |
| `get_token_data` | Get live market data for a specific token. |
| `compare_tokens` | Compare 2–5 tokens side by side - price, 24h/7d change, market cap, volume, and ATH drawdown. |
| `market_overview` | Global crypto market snapshot: Fear & Greed Index, BTC dominance, total market cap, DeFi TVL, ETH gas, trending tokens, and top sector leaders. |
| `token_history` | Get historical price data for a token. Returns OHLC candles for the requested timeframe. Use to understand price trends, identify support/resistance levels, or calculate % changes over time. |
| `get_base_token_data` | Get live market data for any Base-chain token by contract address, sourced from DexScreener: price, 1h/6h/24h change, volume, liquidity, market cap, FDV, pair age, and website/social links. |

## Token Scanner (3)

Token due-diligence on Base: score, compare, history, market scans.

| Tool | What it does |
|------|--------------|
| `score_token` | Run the 6-component dip-reversal score on any Base token. |
| `check_token` | Security audit a Base token: honeypot, rug risk score, mint authority, freeze authority, LP lock %, buy/sell tax, holder count. |
| `scan_market` | Scan all trending + new Base pools for trading opportunities. |

## Wallet & DeFi (Base) (1)

Custodial-style local MCP wallet (key stored in `~/.noelclaw`), balances, DeFi yields, trade planning.

| Tool | What it does |
|------|--------------|
| `get_defi_yields` | Fetch top DeFi yield opportunities on Base - Morpho, Moonwell, Aerodrome, Uniswap, and more. |

## Base MCP (base_mcp_*) (7)

Full Base mainnet (8453) rail: balances via Blockscout enumeration with impostor-token detection, sends, 0x Permit2 swaps, Morpho/Moonwell yield data, basename resolution. See [Base](base.md).

| Tool | What it does |
|------|--------------|
| `base_mcp_status` | Base MCP - get live status of your Base wallet: address, chain info, current ETH price, gas. |
| `base_mcp_balance` | Base MCP - get your current token balances on Base mainnet (ETH, USDC, USDT, DAI, WETH). |
| `base_mcp_send` | IRREVERSIBLE. Send ETH or ERC-20 tokens to any address (or basename like `jesse.base.eth`) on Base mainnet. Signed and broadcast locally from your wallet — an on-chain transfer cannot be recalled. Requires confirm:… |
| `base_mcp_swap` | Base MCP - swap tokens on Base (chainId 8453) via 0x Protocol Permit2 (signature-based, no separate approval tx). |
| `base_mcp_estimate` | Base MCP - preview a swap's expected output and price impact without executing. |
| `base_mcp_lend` | Base MCP - find the best lending venues for a token on Base (Morpho vaults + Moonwell markets), ranked by APY × TVL safety score. |
| `base_mcp_resolve` | Base MCP - resolve a Base basename (like `jesse.base.eth`) to its 0x address. |

## Base Network Data (4)

Read-only Base network stats and yield/lending data.

| Tool | What it does |
|------|--------------|
| `base_mcp_yield_vaults` | Find the best yield/earning opportunities on Base chain using Morpho vaults. |
| `base_mcp_lending_rates` | Get lending and borrowing rates across all Moonwell markets on Base. |
| `base_mcp_deposit_guide` | Get step-by-step deposit instructions for a Morpho vault — shows the vault address, expected APY, and manual deposit steps. |
| `base_mcp_network` | Get real-time Base network stats: ETH price in USD, gas price in gwei, and latest block number. |

## Robinhood Chain (rh_*) (8)

Tokenized stocks + arbitrary crypto on Robinhood Chain (4663) with V2/V3/V4 best-fill routing, market and safety pre-screens. See [Robinhood Chain](robinhood.md).

| Tool | What it does |
|------|--------------|
| `rh_mcp_status` | Robinhood Chain MCP - status of RH rail (chainId 4663): wallet address, RPC, ETH gas balance on RH, explorer. |
| `rh_mcp_list_stocks` | Robinhood Chain MCP - list the 22 NoelClaw/ClawHood tokenized stock tickers (symbol, name, contract address) tradeable via Uniswap V4 on chain 4663. |
| `rh_mcp_balance` | Robinhood Chain MCP - ETH + tokenized stock balances on RH (chain 4663) for your NoelClaw MCP wallet (same address as Base). |
| `rh_mcp_estimate` | Robinhood Chain MCP - preview ETH↔token swap quote via Uniswap V4 (direct or multi-hop via USDG). |
| `rh_mcp_swap` | Robinhood Chain MCP - execute ETH↔token swap on chain 4663. |
| `rh_token_resolve` | Robinhood Chain MCP - resolve a crypto ticker OR 0x contract address to a tradeable token on chain 4663 via DexScreener. |
| `rh_analyze` | Robinhood Chain MCP - market + risk pre-screen for any RH-chain token (ticker or 0x address). |
| `rh_safety_check` | Robinhood Chain MCP - free onchain safety scan for a token (ticker or 0x address). |

## Robinhood Chain Orders (5)

Local DCA / take-profit / stop-loss order engine. Orders persist to `~/.noelclaw/rh-orders.json`; `rh_orders_tick` previews by default and only trades with `execute: true`.

| Tool | What it does |
|------|--------------|
| `rh_dca_create` | Robinhood Chain — create a DCA plan: buy a fixed ETH amount of a token every N hours, up to a total number of buys, capped by maxSpendEth. |
| `rh_bracket_create` | Robinhood Chain — set take-profit and/or stop-loss on a token you HOLD. |
| `rh_orders_list` | Robinhood Chain — list saved DCA / TP / SL orders and their progress. |
| `rh_order_cancel` | Robinhood Chain — cancel a saved order by id (stops future DCA buys / TP-SL fills). |
| `rh_orders_tick` | Robinhood Chain — evaluate all active orders and act on any that are due (DCA interval reached) or triggered (TP/SL price crossed). |

## Stock Bridge (1)

Tokenized stock vs real US equity: price gap, pool depth, market open/closed.

| Tool | What it does |
|------|--------------|
| `rh_stock_bridge` | Compare a tokenized stock on Robinhood Chain (4663) against the real US equity: on-chain price vs live share price, the premium/discount between them, pool depth, and whether the US market is currently open. |

## Stock Fundamentals (SEC) (1)

Company financials straight from SEC EDGAR XBRL — as filed, not vendor copies. See [Stocks & SEC Data](stocks-sec.md).

| Tool | What it does |
|------|--------------|
| `stock_fundamentals` | Fetch a public company's financials straight from SEC EDGAR XBRL (as filed in 10-Q/10-K) plus a live quote. |

## Insider Activity (SEC) (1)

Form 4 insider transactions with discretionary trades separated from automatic ones.

| Tool | What it does |
|------|--------------|
| `stock_insider` | Parse recent SEC Form 4 insider transactions for a US-listed company, straight from EDGAR. |

## Material Events (SEC) (1)

Form 8-K timeline with item codes decoded into plain language.

| Tool | What it does |
|------|--------------|
| `stock_events` | Timeline of a US company's material events from SEC Form 8-K, with the item codes decoded into plain language — earnings releases, executive departures, debt raises, dilution, restatements, impairments, layoffs. |

## GitHub (8)

Read-only GitHub integration: repos, PRs, issues, files, commits, code search.

| Tool | What it does |
|------|--------------|
| `github_list_repos` | List GitHub repositories for a user or org. |
| `github_list_prs` | List pull requests for a GitHub repository. |
| `github_get_pr` | Get full details of a pull request - title, body, diff summary, changed files, reviews, and comments. |
| `github_list_issues` | List issues for a GitHub repository. Returns issue number, title, author, labels, comment count. |
| `github_get_issue` | Get a GitHub issue with full body and all comments. |
| `github_get_file` | Read a file from a GitHub repository. Returns decoded content (up to 10k chars). Use for reading code, configs, READMEs. |
| `github_get_commits` | Get recent commits for a repo, branch, or specific file. |
| `github_search_code` | Search code on GitHub. Supports qualifiers: repo:owner/repo, language:typescript, path:src/, filename:package.json, etc. Requires GITHUB_TOKEN for best results. |

## Code Audit (1)

Static Solidity security scan with a structured review rubric (no LLM).

| Tool | What it does |
|------|--------------|
| `audit_contract` | Run a deterministic static scan over Solidity source for common antipatterns (tx.origin auth, reentrancy ordering, unchecked low-level calls, delegatecall hijack, floating pragma, etc). |

## Noel Insight (3)

Ask the hosted Noel agent (used by playbooks; needs backend LLM).

| Tool | What it does |
|------|--------------|
| `ask_noel` | Ask Noel anything - analysis, opinions, explanations, strategy, or ideas. |
| `market_thesis` | Fetch a cross-checked live price for any token (CoinGecko + DexScreener + Pyth, with a source-spread warning when they disagree) and return it with the bull/bear/verdict structure for YOU to write. |
| `trade_plan` | Return the verified inputs for a trade plan on any token: cross-checked live price, a table of stop-loss and take-profit price levels computed off spot (signed for long or short), and position sizing math for the… |

## MiroShark Simulation (3)

Agent-market simulation runs.

| Tool | What it does |
|------|--------------|
| `miroshark_simulate` | Simulate any scenario using MiroShark multi-agent AI. |
| `miroshark_status` | Poll the status of a MiroShark simulation. |
| `miroshark_stop` | Stop a running MiroShark simulation. |

## Runtime / OS (3)

Status, diagnostics, and Noel Shell chat bridge.

| Tool | What it does |
|------|--------------|
| `noel_status` | Full runtime dashboard - memory size, persistent agents, active automations, recent vault research, execution scores, and your tier. |
| `noel_diagnostics` | Health check for all Noelclaw services - Convex backend, Firecrawl, Supermemory, and configured API keys. |
| `noel_shell_chat` | Chat with Noel Shell — AI terminal with tool calling. |

## Wallet Signing (3)

Prove wallet ownership off-chain.

| Tool | What it does |
|------|--------------|
| `get_wallet_address` | Get your Noelclaw wallet address. This is the local MCP wallet used to sign requests and receive on-chain assets. Keys never leave your machine. |
| `get_wallet_balance` | Check ETH and USDC balance of your Noelclaw wallet on Base mainnet. |
| `wallet_sign_message` | CAUTION: Sign an arbitrary message with the user's wallet (EIP-191 personal_sign). |

---

## Related

- [Base DeFi](base.md) — the `base_mcp_*` rail in depth
- [Robinhood Chain](robinhood.md) — the `rh_*` rail in depth
- [Stocks & SEC Data](stocks-sec.md) — `stock_fundamentals` / `stock_insider` / `stock_events`
- [Environment Variables](env-vars.md)
