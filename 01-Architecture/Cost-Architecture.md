---
tags: [cost, budget]
---

# Cost Architecture

## Modal.com
- **Budget**: $5/month (current)
- **Apply Academic**: $10k credits (verifiable students)
- **Models used** (zerocost tier):
  - SGLang-Diffusion (FLUX.2-klein 4B)
  - Wan 2.1 1.3B video gen
  - CosyVoice 2.0 voice clone
  - MusicGen-Small/Large
  - NLLB-200 translation
- **Apps**: 3 serverless (`modal_apps/`)

## fal.ai
- **Pay as you go** (no free tier, but student = 50-90% off)
- **Use case**: premium models (Kling 3.0, Veo 3.1, LTX-2.3)
- **14 models** in `integrations/fal_dispatch.py`
- **Budget guard**: `fal_dispatch.py` tracks spend

## Pricing comparison (5s 720p video)
| Service | Cost | Multiplier |
|---------|------|------------|
| Modal Wan 2.1 1.3B | $0.005 | 1x (best) |
| fal Wan 2.1 720p | $0.40 | 80x |

## Strategy: Zerocost-First
1. Try Modal (open source, cheap)
2. Fall back to fal (premium, expensive)
3. Cache results in Redis/SQLite
4. Use circuit breaker for fail-fast

## Cost Projections
- 1000 videos/day @ Modal = $5/day
- 1000 videos/day @ fal = $400/day
- **Saving**: $1185/month if we use Modal

## Related
- [[ADR-0002-Zerocost-First]]
- [[Modal-Academic]]
- [[FAL-Education]]
