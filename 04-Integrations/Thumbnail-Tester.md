---
tags: [integration, ai, ab-test, ctr]
---

# Thumbnail Tester (`integrations/thumbnail_tester.py`)

**Status**: active (v1.3.0)
**Added**: 2026-06-06 (BATCH C)
**Tests**: 28

## What
A/B test thumbnails to maximize CTR. Generate variants, run test, pick winner.

## Why
Thumbnail = 80% of CTR. Test 4 variants, ship winner.

## How
- **Variant generation**: Modal FLUX.2-klein (zerocost)
- **Significance**: z-test (uses `integrations/ab_testing.py`)
- **CTR prediction**: trained on `analytics_pipeline.py` data

## API
```python
from integrations.thumbnail_tester import ThumbnailTester

tester = ThumbnailTester()
variants = await tester.create_variants("https://...", n=4)
results = await tester.run_test("video_123", variants, min_impressions=1000)
winner = tester.declare_winner("test_123")
predicted = await tester.predict_ctr("https://...", niche="tech")
```

## Files
- `ugc_ai_overpower/integrations/thumbnail_tester.py` (12.8K, 250+ lines)
- `ugc_ai_overpower/tests/test_thumbnail_tester.py` (8.6K, 28 tests)

## Related
- [[AB-Testing]] — significance engine
- [[Analytics-Pipeline]] — CTR data source
- [[AI-Dispatcher]] — variant generation
