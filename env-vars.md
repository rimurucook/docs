# Environment Variables

All secrets are stored as Convex environment variables — never in the frontend bundle or MCP server.

---

## Set via CLI

```bash
# Run from the app/ folder (where convex/ is)
npx convex env set KEY_NAME "value"
```

Or go to **Convex Dashboard → your deployment → Settings → Environment Variables**.

---

## Required Variables

| Variable | Purpose | Where to Get |
|----------|---------|-------------|
| `BANKR_API_KEY` | Bankr LLM and Agent API — powers ask_noel and swarm agents | bankr.bot dashboard |
| `TELEGRAM_BOT_TOKEN` | Default bot token for swarm and automation notifications | @BotFather on Telegram |
| `TELEGRAM_CHAT_ID` | Default chat ID for Telegram delivery | Your Telegram chat/group ID |
| `GROK_API_KEY` | X.AI Grok API — powers get_insight | console.x.ai |
| `WALLET_ENCRYPTION_KEY` | AES-256 key for encrypting user private keys in DB — never expose this | Generate a strong random string |
| `ZX_API_KEY` | 0x API key for Permit2 swap quotes on Base mainnet | dashboard.0x.org |

> **Telegram is optional** and only needed if you want push notifications outside your AI client (Claude, Hermes, OpenClaw, Cursor). If you use Noelclaw through an MCP client, you already get all results inline — no Telegram setup needed.

---

## Optional Variables

| Variable | Purpose | Default |
|----------|---------|---------|
| `BASE_RPC_URL` | RPC endpoint for Base mainnet | `https://mainnet.base.org` |
| `LLM_ENDPOINT` | Override Bankr LLM gateway URL | `https://llm.bankr.bot/v1/chat/completions` |
| `NOEL_WALLET_ADDRESS` | Platform wallet address for USDC deposits (x402 endpoint) | — |
| `COINGECKO_API_KEY` | CoinGecko Pro API key for higher rate limits | — |
| `RESEND_API_KEY` | Email delivery for OTP verification | — |
| `ALCHEMY_API_KEY` | Alchemy key for portfolio lookups and token balances | — |

> **Wallet security:** `WALLET_ENCRYPTION_KEY` must be set before any user calls `get_portfolio`, `swap_tokens`, or `send_token`. Private keys are AES-256-CBC encrypted before storage — the plaintext key never touches the database.

> **Per-user Telegram:** Users can set their own bot token and chat ID via `POST /user/telegram`. When set, their personal bot is used for research results and agent notifications instead of the system default.

---

## MCP Server Variables

The MCP server reads these from the local environment (set in your MCP client config's `env` block):

| Variable | Purpose | Default |
|----------|---------|---------|
| `NOELCLAW_API_KEY` | Link to your noelclaw.com account | — (wallet-native auth used) |
| `NOELCLAW_SESSION_TOKEN` | Alternative session token | — |
| `NOELCLAW_CONVEX_URL` | Override API proxy URL | `https://api.noelclaw.com` |
| `NOELCLAW_PAYMENT_HEADER` | x402 payment proof header (single-use) | — |
| `ALCHEMY_API_KEY` | Faster swap quotes and portfolio queries | — |
| `GROK_API_KEY` | BYOK — forwarded as `X-User-Grok-Key` | — |
| `BANKR_API_KEY` | BYOK — forwarded as `X-User-Bankr-Key` | — |
| `TELEGRAM_BOT_TOKEN` | BYOK — forwarded as `X-User-Telegram-Token` | — |
| `TELEGRAM_CHAT_ID` | BYOK — forwarded as `X-User-Telegram-Chat` | — |

> **Telegram is optional** and only needed if you want push notifications outside your AI client (Claude, Hermes, OpenClaw, Cursor). If you use Noelclaw through an MCP client, you already get all results inline — no Telegram setup needed.

Example with all options in Claude Desktop config:

```json
{
  "mcpServers": {
    "noelclaw": {
      "command": "npx",
      "args": ["@noelclaw/mcp"],
      "env": {
        "NOELCLAW_API_KEY": "noel_sk_xxx",
        "ALCHEMY_API_KEY": "your-alchemy-key",
        "GROK_API_KEY": "xai-xxx"
      }
    }
  }
}
```

---

## Frontend Variables (Vite)

These go in `app/.env` or `app/.env.local`:

| Variable | Purpose |
|----------|---------|
| `VITE_CONVEX_URL` | Convex deployment URL for the frontend |
| `VITE_PRIVY_APP_ID` | Privy app ID for authentication |
