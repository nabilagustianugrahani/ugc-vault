---
tags: [integration, content, repurposing]
---

# Content Repurposer (`integrations/content_repurposer.py`)

**Status**: active (v1.3.0)
**Added**: 2026-06-06 (BATCH C)
**Tests**: 28

## What
Auto-repurpose one source video/image for all platforms (TikTok, Reels, Shorts, Twitter, LinkedIn, YouTube).

## Why
- 1 video → 6 platform versions
- Auto-generate platform-specific captions
- Auto-pick best posting time per platform

## How
- **Reformat**: ffmpeg via `video_editor.py`
- **Caption**: AI-generated per platform tone
- **Hashtags**: niche-aware
- **Posting time**: from `analytics_pipeline.py` + niche

## Platform Specs
| Platform | Aspect | Duration |
|----------|--------|----------|
| Reels | 9:16 | 15-90s |
| TikTok | 9:16 | 15-90s |
| YouTube Shorts | 9:16 | 15-60s |
| Twitter Video | 16:9 or 1:1 | 30-140s |
| LinkedIn | 1:1 | 30-90s |
| YouTube long | 16:9 | >60s |

## API
```python
from integrations.content_repurposer import ContentRepurposer, SourceContent

rep = ContentRepurposer()
source = SourceContent(
    media_url="https://...",
    title="My video",
    description="...",
    duration_sec=60,
    source_platform="tiktok",
)
results = await rep.repurpose_for_all_platforms(source)
for r in results:
    print(f"{r.target_platform}: {r.media_url} @ {r.best_posting_time}")
```

## Files
- `ugc_ai_overpower/integrations/content_repurposer.py` (16.9K, 350+ lines)
- `ugc_ai_overpower/tests/test_content_repurposer.py` (10K, 28 tests)

## Related
- [[Video-Editor]] — ffmpeg ops
- [[AI-Dispatcher]] — caption gen
- [[Analytics-Pipeline]] — best posting time
