---
tags: [core, perf, resilience]
---

# Circuit Breaker (`core/circuit_breaker.py`)

**Status**: active (v1.4.0)
**Added**: 2026-06-06 (BATCH B)
**Tests**: 25

## What
Circuit breaker pattern for external APIs (fail-fast, auto-recover).

## States
- `CLOSED` — normal, calls pass through
- `OPEN` — failing, reject all calls immediately
- `HALF_OPEN` — testing recovery

## Why
Don't hammer Modal/fal when they're down. Don't waste money on calls that will fail.

## API
```python
from core.circuit_breaker import CircuitBreaker, CircuitBreakerRegistry

# Single
cb = CircuitBreaker("modal", failure_threshold=5, recovery_timeout_sec=30)
try:
    result = await cb.call(modal_api, prompt)
except CircuitBreakerOpen:
    pass  # fast-fail

# Registry
registry = CircuitBreakerRegistry()
modal_cb = registry.get("modal")
registry.list_open()  # ["fal"]  (currently open)
registry.force_close("fal")  # manual recovery
```

## Files
- `ugc_ai_overpower/core/circuit_breaker.py` (7.1K, 150 lines)
- `ugc_ai_overpower/tests/test_circuit_breaker.py` (8.3K, 25 tests)

## Related
- [[Rate-Limiter]]
- [[Cache]]
- [[Performance]] — combines all 3
