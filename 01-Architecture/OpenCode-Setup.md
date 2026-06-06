---
tags: [opencode, infrastructure]
---

# OpenCode Setup

## Model
- **opencode/minimax-m3-free** on OpenCode Zen
- No API key needed
- Free tier, no rate limit visible
- Fast (responses in 5-30s)

## Plugins (4 active)
1. **superpowers** (obra) — meta-workflow skills
2. **opencode-skills-collection** (FrancoStino) — 1000+ universal + SkillPointer (255 tokens at startup)
3. **opencode-power-pack** (amitdmore) — Claude Code ports
4. **ecc-universal** (affaan-m) — 251 skills

Total: 106 skills accessible.

## Custom Agents (in `.opencode/agents/`)
- `build.md` — default build
- `plan.md` — planning
- `test.md` — test writing
- `fix.md` — bug fix
- `review.md` — code review

## Memory / Knowledge
- **Obsidian vault** at `/home/aer/ugc-vault/` (this!)
- OpenCode reads/writes markdown via standard tools
- Synced to GitHub → user can browse in Obsidian

## Skills Most Used
- `systematic-debugging` — for any bug
- `verification-before-completion` — before claiming done
- `test-driven-development` — for new features
- `using-superpowers` — auto-loaded for meta-workflow
