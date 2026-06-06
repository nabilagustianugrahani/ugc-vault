---
tags: [integration, ai, voice, tts]
---

# Voice Clone (`integrations/voice_clone.py`)

**Status**: active (v1.3.0)
**Added**: 2026-06-06 (BATCH C)
**Tests**: 28

## What
Clone a voice from audio samples, then synthesize new speech in that voice.

## Why
Indonesian UGC creators need:
- Consistent character voice across videos
- Re-record audio without re-shooting
- A/B test different "voice actors"

## How
- **Primary**: Modal CosyVoice 2.0 (zerocost)
- **Fallback**: fal KokoroTTS
- **Caching**: voice_id → VoiceCloneResult

## API
```python
from integrations.voice_clone import VoiceCloner, VoiceSample

cloner = VoiceCloner()
samples = [VoiceSample(audio_url="...", transcript="Halo semua")]
result = await cloner.clone(samples, name="my_voice", target_text="Halo guys!")
audio = await cloner.synthesize(result.voice_id, "New text here")
```

## Files
- `ugc_ai_overpower/integrations/voice_clone.py` (9.9K, 200+ lines)
- `ugc_ai_overpower/tests/test_voice_clone.py` (7.9K, 28 tests)

## Related
- [[AI-Dispatcher]] — zerocost-first routing
- [[Music-Gen]] — sibling audio module
- [[Subagent-Prompts]] — BATCH C spec
