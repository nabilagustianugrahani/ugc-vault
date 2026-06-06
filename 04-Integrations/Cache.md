---
tags: [core, perf, cache]
---

# Cache (`core/cache.py`)

**Status**: active (v1.4.0)
**Added**: 2026-06-06 (BATCH B)
**Tests**: 25

## What
Async cache layer with L1 (memory) + L2 (Redis) + decorator.

## Why
- Modal/fal API results should be cached (5-10x faster on cache hit)
- Circuit breaker: cache last good response when API is down
- TTL prevents stale data

## Classes
- `CacheBackend` (ABC) — interface
- `MemoryCache` — LRU + TTL, max_size=10000
- `RedisCache` — uses aioredis
- `TieredCache` — L1 + L2 wrapper
- `cached()` — decorator

## API
```python
from core.cache import MemoryCache, cached, TieredCache, RedisCache

# Direct usage
cache = MemoryCache(max_size=1000)
await cache.set("key", "value", ttl_sec=60)
val = await cache.get("key")

# Tiered
l1 = MemoryCache()
l2 = RedisCache(url="redis://localhost:6379")
tiered = TieredCache(l1, l2)

# Decorator
@cached(ttl_sec=300, key_prefix="user")
async def get_user(user_id: int):
    return await expensive_api_call(user_id)
```

## Files
- `ugc_ai_overpower/core/cache.py` (8.5K, 250 lines)
- `ugc_ai_overpower/tests/test_cache.py` (10K, 25 tests)

## Related
- [[Performance]] — unified RateLimitedCachedBreaker
- [[Rate-Limiter]]
- [[Circuit-Breaker]]
