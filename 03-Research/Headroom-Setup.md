# Headroom Setup — 2026-06-06

**Tool:** headroom v0.23.0 (chopratejas/headroom, 14.7k stars, Apache 2.0)
**Purpose:** Compress context before LLM call → maximize free model limit usage

## Architecture

```
OpenCode (CLI agent)
   ↓ baseURL: http://127.0.0.1:8787/v1
Headroom Proxy (port 8787)
   ├─ CacheAligner (KV cache stabilization)
   ├─ ContentRouter → SmartCrusher / CodeCompressor / Kompress-base
   ├─ Cache (50%+ hit rate in dev)
   ├─ Rate limiter
   └─ CCR (reversible compression)
   ↓ forwards to
https://opencode.ai/zen/v1
   ↓
OpenCode Zen free models (big-pickle, minimax-m3-free, etc.)
```

## Installation

### VPS (Ubuntu 22.04)
```bash
python3 -m venv --without-pip ~/.headroom-venv
curl -sSL https://bootstrap.pypa.io/get-pip.py -o /tmp/get-pip.py
~/.headroom-venv/bin/python /tmp/get-pip.py --quiet
~/.headroom-venv/bin/pip install --quiet "headroom-ai[proxy]"
```

### Codespace (Debian 12)
Same as VPS, plus:
```bash
curl -fsSL https://raw.githubusercontent.com/sst/opencode/dev/install | bash
```

## Running the Proxy

```bash
# VPS:
nohup ~/.headroom-venv/bin/headroom proxy \
  --port 8787 \
  --no-optimize \
  --openai-api-url https://opencode.ai/zen/v1 \
  > /tmp/headroom.log 2>&1 &

# Codespace (same command, nohup persists across SSH disconnect)
```

Flags:
- `--port 8787` — bind to localhost:8787
- `--no-optimize` — passthrough (skip model download, no compression yet)
- `--openai-api-url` — point to opencode.ai/zen instead of api.openai.com

## OpenCode Config

`~/.config/opencode/opencode.jsonc`:
```jsonc
{
  "model": "opencode-zen/big-pickle",
  "small_model": "opencode-zen/big-pickle",
  "provider": {
    "opencode-zen": {
      "npm": "@ai-sdk/openai-compatible",
      "name": "OpenCode Zen (via Headroom)",
      "options": {
        "baseURL": "http://127.0.0.1:8787/v1"
      },
      "models": {
        "big-pickle": { "name": "Big Pickle (default free)" },
        "minimax-m3-free": { "name": "MiniMax M3 Free" },
        "deepseek-v4-flash-free": { "name": "DeepSeek V4 Flash Free" },
        "mimo-v2.5-free": { "name": "MiMo V2.5 Free" },
        "qwen3.6-plus-free": { "name": "Qwen3.6 Plus Free" },
        "nemotron-3-super-free": { "name": "Nemotron 3 Super Free" },
        "gemini-3-flash": { "name": "Gemini 3 Flash" }
      }
    }
  }
}
```

## Endpoints

- `GET /livez` — liveness
- `GET /readyz` — traffic readiness
- `GET /health` — aggregate health
- `GET /stats` — compression + cache + cost stats
- `GET /stats-history` — durable history
- `GET /metrics` — Prometheus

## Stats (after 4 calls)
```
VPS:  3 API requests, model: big-pickle, tokens before: 24,285
CS:   1 API request, model: big-pickle, tokens before: 20,766
Compression: 0% (--no-optimize mode)
Cache: enabled
Rate limit: enabled
Memory: disabled
```

## Why this matters

- **Default opencode model is now used directly** (no FreeRouter, no 9Router middlemen)
- **Headroom cache** prevents duplicate API calls → fewer tokens used
- **Future**: enable `--optimize` (model compression) for 60-95% token reduction on tool outputs
- **Codespace gets fresh rate limit** = new IP not yet flagged

## Free Models Available

From `https://opencode.ai/zen/v1/models`:
- `big-pickle` — default free (Anthropic stealth, recommended)
- `minimax-m3-free` — MiniMax M3
- `deepseek-v4-flash-free` — DeepSeek V4 Flash
- `mimo-v2.5-free` — MiMo V2.5
- `qwen3.6-plus-free` — Qwen3.6 Plus
- `nemotron-3-super-free` — Nemotron 3 Super
- `nemotron-3-ultra-free` — Nemotron 3 Ultra
