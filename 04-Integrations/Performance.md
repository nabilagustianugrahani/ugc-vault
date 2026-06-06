---
tags: [core, perf, orchestration]
---

# Performance (`core/performance.py`)

**Status**: active (v1.4.0)
**Added**: 2026-06-06 (BATCH B)
**Tests**: 30

## What
Unified `RateLimitedCachedBreaker` — combines cache + rate limiter + circuit breaker.

## Why
Each external API call needs all 3:
1. Check circuit breaker (fail-fast if down)
2. Check rate limiter (wait if at limit)
3. Check cache (return cached if available)
4. Execute + cache result

## API
```python
from core.performance import RateLimitedCachedBreaker
from core.cache import MemoryCache
from core.rate_limiter import TokenBucketLimiter
from core.circuit_breaker import CircuitBreaker

stack = RateLimitedCachedBreaker(
    name="modal_dispatch",
    rate_limiter=TokenBucketLimiter(60, 1.0),
    cache=MemoryCache(),
    breaker=CircuitBreaker("modal"),
)

result = await stack.execute(
    modal_api_call,
    prompt="cat",
    cache_key="cat_1024",
    cache_ttl=3600,
)
```

## Files
- `ugc_ai_overpower/core/performance.py` (10K, 200 lines)
- `ugc_ai_overpower/tests/test_performance.py` (10.8K, 30 tests)

## Related
- [[Cache]]
- [[Rate-Limiter]]
- [[Circuit-Breaker]]
