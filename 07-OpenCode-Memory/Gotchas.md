---
tags: [opencode, gotchas, learnings]
---

# Gotchas (Edge Cases to Avoid)

> Things that bit us. OpenCode should check this before repeating.

## Codespace SSH
- `gh codespace ssh` doesn't preserve CWD — must `cd` inside
- `gh codespace ssh -c NAME -- "cmd"` quotes are tricky
- Use base64 for complex scripts: `echo "..." | base64 -d | bash`
- 2-3s latency per command

## Git in Codespace
- GPG signing fails — use `git -c commit.gpgsign=false commit`
- PAT required in remote URL: `https://x-access-token:${TOKEN}@github.com/...`
- Default branch is main (not master) since 2026
- 1GB repo soft limit (warning at 5GB)

## OpenCode in Codespace
- Opencode caches plugins indefinitely — need to update after edit
- Some plugins slow startup 60s+ (don't add unnecessary ones)
- If model is "minimax-m3-free" but slow, check OpenCode Zen status
- Agent prompt files: `/workspaces/Coba/.opencode/agents/{name}.md`

## Mypy
- Was 52 errors, now 0
- Run before commit: `mypy ugc_ai_overpower/integrations/`
- Strict mode catches many bugs early

## Modal
- Apps need `modal deploy` after code change
- Volume caches: `modal volume ls`
- 3 serverless apps: text_to_image, text_to_video, voice_synth
- $5 budget tracked in `modal_dispatch.py`

## fal.ai
- Pay-as-you-go — no free tier
- Student discount 50-90% off (apply via education portal)
- 14 models in `fal_dispatch.py`
- Budget guard: `fal_dispatch.py` `check_budget()`

## GitHub API
- Codespaces billing endpoint has weird JSON structure
- Use `gh api` for safer queries
- Plan check: `gh api /user | jq .plan.name` → "pro"

## VPS
- 8GB RAM, 1 vCPU — keep light
- Opencode running: monitor RAM with `free -h`
- If OOM, kill old opencode sessions: `pkill -f opencode`
