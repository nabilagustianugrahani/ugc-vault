---
tags: [integration, ai, music, audio]
---

# Music Gen (`integrations/music_gen.py`)

**Status**: active (v1.3.0)
**Added**: 2026-06-06 (BATCH C)
**Tests**: 22

## What
Generate royalty-free background music (BGM) for videos.

## Why
Avoid copyright strikes on TikTok/YouTube. Auto-pick BGM genre based on video metadata.

## How
- **0-30s**: MusicGen-Small on Modal (zerocost)
- **30-180s**: MusicGen-Large on Modal
- **>180s**: Stable Audio on fal (premium)
- **License**: CC0 / CC-BY (always royalty-free)

## API
```python
from integrations.music_gen import MusicGenerator, MusicPrompt

gen = MusicGenerator()
prompt = MusicPrompt(genre="lo-fi", mood="calm", duration_sec=60, bpm=90)
track = await gen.generate(prompt, name="bgm_v1")

# Auto-pick from video metadata
track = await gen.generate_for_video(
    {"tags": ["cooking", "calm"], "duration_sec": 45},
    target_duration_sec=45
)
```

## Files
- `ugc_ai_overpower/integrations/music_gen.py` (10K, 200+ lines)
- `ugc_ai_overpower/tests/test_music_gen.py` (7.7K, 22 tests)

## Related
- [[Voice-Clone]] — sibling audio module
- [[Content-Repurposer]] — uses music for reformatting
