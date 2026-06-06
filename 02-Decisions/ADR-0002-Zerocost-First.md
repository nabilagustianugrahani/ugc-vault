---
tags: [adr, decision]
status: accepted
date: 2026-06-06
---

# ADR-0002: Zerocost-First (Modal → fal)

## Context
For open source models (Wan 2.1, FLUX.2, CosyVoice, MusicGen), Modal is 80x cheaper than fal.ai. But fal has premium models (Kling 3.0, Veo 3.1, LTX-2.3) we need for top quality.

## Decision
- **UnifiedAIDispatcher** (`integrations/ai_dispatch.py`) routes by cost tier:
  1. **Zerocost** (Modal): try first
  2. **Premium** (fal): fallback if Modal fails or for premium models
  3. **Cache** results to avoid re-charge
  4. **Circuit breaker** to fail-fast on outages
- **MODAL_TO_FAL_BRIDGE**: when Modal doesn't have a model, fall through to fal
- **FAL_ONLY_MODELS**: explicit list of premium-only models

## Consequences
**Easier:**
- 80x cost savings on common models
- Single API for callers (`dispatch_ai(model, prompt)`)
- Auto-failover to premium when needed
- Budget guard prevents surprise charges

**Harder:**
- More complex dispatch logic
- Need to test both Modal + fal paths
- Modal apps need maintenance (3 serverless apps)

## Alternatives Considered
- **Modal-only**: rejected because missing premium models
- **fal-only**: rejected because 80x cost
- **Replicate**: rejected because mid-tier pricing, no advantage

## Notes
- Date: 2026-06-06
- Deciders: @aer
- Related: [[Cost-Architecture]], [[AI-Dispatcher]]
