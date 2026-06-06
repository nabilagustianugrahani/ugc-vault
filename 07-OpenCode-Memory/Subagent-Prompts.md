---
tags: [opencode, prompts, learnings]
---

# Subagent Prompts (Successful Specs)

> Reusable task specs. Copy/paste and customize.

## BATCH A: Integrations v1.3.0
5 modules in parallel via subagent_type: swarm-worker:
- `integrations/analytics_pipeline.py` (PostMetrics, ROIDashboard)
- `integrations/ab_testing.py` (Variant, z-test)
- `integrations/seo_optimizer.py` (Keyword, SEOScore)
- `integrations/translation_pipeline.py` (14 langs, NLLB-200)
- `integrations/image_enhancer.py` (10 ops, Real-ESRGAN+GFPGAN)
Result: 5 files, +200 tests, merged in 1 commit.

## BATCH B: Perf Layer
4 files in core/:
- `core/cache.py` (MemoryCache + RedisCache + TieredCache)
- `core/async_pool.py` (AsyncPool w/ priority)
- `core/rate_limiter.py` (TokenBucket + SlidingWindow)
- `core/circuit_breaker.py` (CircuitBreaker + Registry)
Plus `core/performance.py` (RateLimitedCachedBreaker)

## BATCH C: Content Tools
4 files in integrations/:
- `integrations/voice_clone.py` (Modal CosyVoice 2.0)
- `integrations/music_gen.py` (MusicGen-Small/Large)
- `integrations/thumbnail_tester.py` (FLUX.2-klein variants)
- `integrations/content_repurposer.py` (multi-platform auto)

## Common Pattern
```bash
# In codespace, dispatch build agent
nohup /home/vscode/.opencode/bin/opencode run --model opencode/minimax-m3-free --agent build "$(cat /tmp/task.md)" > /tmp/task.log 2>&1 &
echo "PID=$!"
```
