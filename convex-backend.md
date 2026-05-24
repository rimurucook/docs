# Convex Backend

Convex is Noelclaw's entire backend — database, serverless functions, cron jobs, and HTTP router in one managed service.

---

## Database Tables (20 total)

### Users & Auth
| Table | Purpose |
|-------|---------|
| `users` | Core user (email, username, profile, USDC balance, Telegram ID) |
| `sessions` | Session tokens with 30-day expiry |
| `otps` | One-time passwords for email auth |
| `pendingUsers` | Users who started registration but haven't verified |
| `telegramLinkTokens` | 6-char link codes for Telegram account linking |

### Wallets & Payments
| Table | Purpose |
|-------|---------|
| `wallets` | Wallet address + encrypted private key per user |
| `transactions` | Credits: deposit, deduction, refund with token/model tracking |
| `trades` | Buy/sell/swap/send actions (web, X, Telegram sources) |
| `xMentions` | Incoming X/Twitter mentions to process as trades |
| `agentPayments` | Legacy x402 payment records (deprecated) |

### Games
| Table | Purpose |
|-------|---------|
| `gameScores` | Individual game sessions (score, kills, level, credits) |
| `gameProfiles` | User aggregate stats + best scores per game |

### Agents & Skills
| Table | Purpose |
|-------|---------|
| `customAgents` | User-created agents with prompts, pricing, categories |
| `agentRuns` | Skill execution logs (status, tokens, cost, result) |
| `userSkillConfig` | Which skills each user has enabled + their config |

### Research System
| Table | Purpose |
|-------|---------|
| `researchJobs` | 8h shift metadata (userId, status, timings, Telegram chat) |
| `researchDataPoints` | Raw data per 30min collection (CoinGecko, Grok, Bankr) |
| `researchReports` | Generated interim/final reports with full result JSON |
| `researchCache` | 15-min cache for on-demand research results |
| `memoryRefs` | IPFS memory references (CID-based) |

### Activity & Notifications
| Table | Purpose |
|-------|---------|
| `activities` | Real-time activity feed entries |
| `notifications` | User notifications (low/medium/high/urgent priority) |

### Dev Tools (Gitlawb)
| Table | Purpose |
|-------|---------|
| `gitlawbRepos` | IPFS-backed git repos |
| `gitlawbCommits` | Signed commits stored on IPFS |
| `gitlawbPRs` | Pull requests (open/merged/closed) |
| `gitlawbUCANs` | UCAN capability tokens for auth |

---

## Backend Files

| File | Type | Purpose |
|------|------|---------|
| `agent.ts` | Action | Noel research loop, scheduled loop, on-demand research |
| `researchEngine.ts` | Action | 8h shift engine: collect, synthesize, send Telegram |
| `researchJobs.ts` | Mutation/Query | DB layer for shift jobs, data points, reports |
| `agentMutations.ts` | Mutation | Log agent runs, push notifications, cache research |
| `userNotifications.ts` | Query/Mutation | Skill status, agent run history, notification management |
| `chat.ts` | Action | LLM gateway for agent chat |
| `auth.ts` | Mutation/Query | Login, register, session management |
| `balance.ts` | Mutation/Query | Credit balance, top-up, deductions |
| `wallets.ts` | Mutation/Query | Wallet CRUD, address lookup |
| `tradeActions.ts` | Action | Execute trades on-chain |
| `xWebhookActions.ts` | Action | Parse X mentions into trades |
| `gameActions.ts` | Action | Submit scores, update profiles |
| `http.ts` | HTTP Router | All `convex.site` endpoints |
| `crons.ts` | Scheduler | Background jobs |
| `schema.ts` | Schema | All table definitions |

---

## Cron Jobs

| Cron | Interval | Handler |
|------|----------|---------|
| `collect-research-data` | Every 5 min | Check active shifts, collect data every 30min |
| `send-research-reports` | Every 5 min | Generate/send reports at 2.5h, 5h, 8h |

---

## Key Patterns

### Internal vs Public Functions

- `internalAction` / `internalMutation` / `internalQuery` — only callable from other Convex functions
- `action` / `mutation` / `query` — callable from frontend and HTTP handlers
- HTTP handlers use `ctx.runAction(internal.xxx)` to call internal functions

### Node.js Actions

Files with `"use node"` at the top run in a Node.js environment (vs. Convex's default sandboxed runtime). These can make `fetch()` calls to external APIs:

- `agent.ts`
- `researchEngine.ts`
- `chat.ts`
- All `*Actions.ts` files

### Real-time Subscriptions

Frontend uses `useQuery()` from Convex React — this creates a WebSocket subscription that re-renders automatically when the DB changes. No polling needed.

```typescript
// Brain page: updates live when a new research run completes
const runs = useQuery(api.userNotifications.getAgentRuns, { userId, limit: 50 });
```

---

## HTTP Endpoints Summary

| Endpoint | Method | Purpose |
|----------|--------|---------|
| `/mcp/market` | GET | Live CoinGecko market data |
| `/mcp/chat` | POST | Chat with any agent |
| `/mcp/research` | POST | On-demand research |
| `/mcp/research/start` | POST | Start 8h shift |
| `/mcp/research/stop` | POST | Stop shift |
| `/mcp/research/status` | GET | Shift status + recent reports |
| `/telegram/connect` | POST | Link Telegram account |
| `/telegram/wallet` | GET | Look up wallet by Telegram ID |
| `/alchemy/deposit` | POST | USDC deposit webhook |
