---
tags: [core, perf, rate-limit]
---

# Rate Limiter (`core/rate_limiter.py`)

**Status**: active (v1.4.0)
**Added**: 2026-06-06 (BATCH B)
**Tests**: 30

## What
Token bucket + sliding window rate limiters with multi-key support.

## Why
Modal: 60/min. fal.ai: 100/sec. TikHub: 1000/min. Each has its own limits.

## Classes
- `RateLimiter` (ABC)
- `TokenBucketLimiter` — capacity tokens, refill rate/sec
- `SlidingWindowLimiter` — max requests in window
- `MultiKeyLimiter` — different limits per key (per-service)

## API
```python
from core.rate_limiter import TokenBucketLimiter, SlidingWindowLimiter, MultiKeyLimiter

tb = TokenBucketLimiter(capacity=60, refill_per_sec=1.0)
if await tb.acquire("modal"):
    await modal_api_call()

# Multi-service
limiter = MultiKeyLimiter({
    "modal": TokenBucketLimiter(60, 1.0),
    "fal": TokenBucketLimiter(100, 1.66),  # 100/sec
    "tikhub": SlidingWindowLimiter(1000, 60),  # 1000/min
})
if await limiter.acquire("fal"):
    await fal_api_call()
```

## Files
- `ugc_ai_overpower/core/rate_limiter.py` (8.4K, 200 lines)
- `ugc_ai_overpower/tests/test_rate_limiter.py` (11.8K, 30 tests)

## Related
- [[Circuit-Breaker]]
- [[Cache]]
- [[Performance]]
