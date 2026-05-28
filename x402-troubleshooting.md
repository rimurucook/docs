# x402 Troubleshooting

If you receive a `402 Payment Required` error, your `NOELCLAW_API_KEY` is missing or invalid.

Set it in your MCP client config:

```json
{
  "mcpServers": {
    "noelclaw": {
      "command": "npx",
      "args": ["@noelclaw/mcp"],
      "env": {
        "NOELCLAW_API_KEY": "noel_sk_xxx"
      }
    }
  }
}
```

Get your API key from [noelclaw.com](https://noelclaw.com) → Profile → API Keys.
