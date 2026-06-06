---
tags: [core, perf, async]
---

# Async Pool (`core/async_pool.py`)

**Status**: active (v1.4.0)
**Added**: 2026-06-06 (BATCH B)
**Tests**: 28

## What
Async task pool with priority queue + worker limits.

## Why
- Bounded concurrency (don't overwhelm Modal/fal)
- Priority (high-priority tasks skip ahead)
- Per-task timeout + error handling
- Stats for monitoring

## API
```python
from core.async_pool import AsyncPool, PoolTask

pool = AsyncPool(max_workers=10, max_queue=1000)

async def my_task(x): return x * 2

task_id = await pool.submit(PoolTask(fn=my_task, args=(5,), priority=3))
results = await pool.map(my_task, [1, 2, 3, 4, 5], concurrency=2)
pool.stats()  # {"pending": 0, "running": 2, "completed": 3, "failed": 0}
```

## Files
- `ugc_ai_overpower/core/async_pool.py` (7.6K, 200 lines)
- `ugc_ai_overpower/tests/test_async_pool.py` (13.6K, 28 tests)

## Related
- [[Cache]]
- [[Rate-Limiter]]
- [[Performance]]
