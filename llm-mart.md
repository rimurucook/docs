# LLM Mart

Access 30+ AI models from a single credit balance. No separate accounts, no juggling API keys — one place for everything.

Available at [noelclaw.com](https://noelclaw.com) → LLM Mart.

---

## Models Available

| Provider | Models |
|----------|--------|
| Noelclaw | **Noel Crypto** — DeFi-native AI built for Base ecosystem |
| Anthropic | Claude Opus 4, Claude Sonnet 4, Claude Haiku 4 |
| OpenAI | GPT-4o, GPT-4o Mini, o3, o4-mini, o3-mini |
| Google | Gemini 2.5 Pro, Gemini 2.0 Flash, Gemini 1.5 Pro |
| xAI | Grok 3, Grok 3 Mini |
| Meta | Llama 3.3 70B, Llama 3.1 405B |
| Mistral | Mistral Large 2, Mistral Small, Mixtral 8x22B |
| DeepSeek | DeepSeek R1, DeepSeek V3, DeepSeek Coder V3 |
| Qwen | Qwen 2.5 72B, QwQ 32B, Qwen 2.5 Coder 32B |
| Cohere | Command R+, Command R |
| Microsoft | Phi-4, Phi-3.5 Mini |
| Moonshot | Kimi K1.5 |
| MiniMax | MiniMax Text-01 |
| 01.AI | Yi Lightning |

Free models (marked in the UI) require no credits.

---

## Noel Crypto

Noel Crypto is Noelclaw's own DeFi-native model — built specifically for Base ecosystem research.

It knows:
- DeFi protocols on Base (Morpho, Moonwell, Uniswap, Aerodrome)
- On-chain data analysis and wallet intelligence
- Token research, market structure, and sentiment
- Swap routing, yield optimization, and risk assessment
- Smart money tracking and on-chain signals

Use it when you want concise, actionable crypto answers without having to prompt a general model to "think like a DeFi trader."

---

## Credits

- 1 credit = $0.01 USDC
- Add credits at [noelclaw.com](https://noelclaw.com) → LLM Mart → Add Credits
- Credits are consumed per message based on model + token usage
- Free models (Llama 8B, Qwen 7B, Mistral Nemo, etc.) cost 0 credits

---

## OpenAI-Compatible API

Noelclaw provides an OpenAI-compatible endpoint. If you have code that uses the OpenAI SDK, you can point it at Noelclaw instead — same interface, Noelclaw models and credits.

**Base URL:** `https://valuable-fish-533.convex.site/v1`

### Using the OpenAI SDK (Python)

```python
from openai import OpenAI

client = OpenAI(
    api_key="noel_sk_your_key_here",
    base_url="https://valuable-fish-533.convex.site/v1"
)

response = client.chat.completions.create(
    model="noel-crypto",
    messages=[
        {"role": "user", "content": "What are the best yield opportunities on Base right now?"}
    ]
)

print(response.choices[0].message.content)
```

### Using curl

```bash
curl https://valuable-fish-533.convex.site/v1/chat/completions \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer noel_sk_your_key_here" \
  -d '{
    "model": "noel-crypto",
    "messages": [{"role": "user", "content": "Best Base DeFi yields right now?"}]
  }'
```

### Get your API key

1. Go to [noelclaw.com](https://noelclaw.com)
2. Settings → API Keys → Generate Key
3. Copy the `noel_sk_...` key

### List available models

```bash
curl https://valuable-fish-533.convex.site/v1/models \
  -H "Authorization: Bearer noel_sk_your_key_here"
```

---

## Model IDs for the API

Use these IDs in the `model` field:

| Display Name | API Model ID |
|-------------|-------------|
| Noel Crypto | `noel-crypto` |
| Claude Opus 4 | `claude-opus-4-6` |
| Claude Sonnet 4 | `claude-sonnet-4-6` |
| Claude Haiku 4 | `claude-haiku-4-5` |
| GPT-4o | `gpt-4o` |
| GPT-4o Mini | `gpt-4o-mini` |
| o3 | `o3` |
| o4-mini | `o4-mini` |
| Gemini 2.5 Pro | `gemini-2.5-pro` |
| Gemini 2.0 Flash | `gemini-2.0-flash` |
| Grok 3 | `grok-3` |
| Grok 3 Mini | `grok-3-mini` |
| DeepSeek R1 | `deepseek-r1` |
| DeepSeek V3 | `deepseek-v3` |
| Llama 3.3 70B | `llama-3.3-70b` |
| Llama 3.1 8B (free) | `llama-3.1-8b` |
| Qwen 2.5 72B | `qwen2.5-72b` |
| Mistral Large 2 | `mistral-large-2` |
| Mistral Nemo (free) | `mistral-nemo` |
