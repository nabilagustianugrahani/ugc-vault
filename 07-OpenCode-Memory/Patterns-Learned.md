---
tags: [opencode, patterns, learnings]
---

# Patterns Learned (OpenCode)

> Code patterns that worked. OpenCode can read this before generating new code.

## Integration Module Pattern
- Dataclass for inputs (`@dataclass class XInput`)
- Dataclass for outputs (`@dataclass class XResult`)
- Class with optional dispatcher injection (`__init__(self, ai_dispatcher=None)`)
- Zerocost-first: try Modal → fall back to fal
- Budget guard: check cost before call
- Tests: 20-30 per module, all in `tests/test_X.py`

## Subagent Task Spec Pattern
- Header: name, codespace, plugins, skills
- Read first: list of files to learn from
- Per file: lines + tests + dataclasses
- Wire it together: cross-file integration
- Final commit: bash script with PAT
- Report: 200 tokens max

## Codespace Task Spec (for OpenCode in codespace)
```bash
nohup /home/vscode/.opencode/bin/opencode run --model opencode/minimax-m3-free --agent build "$(cat /tmp/task.md)" > /tmp/task.log 2>&1 &
```

## Useful Skills (auto-invoke before action)
- `systematic-debugging` — for any bug
- `verification-before-completion` — before "done"
- `test-driven-development` — for new features
- `brainstorming` — before creative work
- `writing-plans` — for multi-step tasks
- `dispatching-parallel-agents` — for 2+ independent tasks
- `requesting-code-review` — before major PR

## Plugin Loading Order
1. `superpowers` — meta-workflow (loaded automatically)
2. `opencode-skills-collection` w/ SkillPointer (255 tokens at startup)
3. `opencode-power-pack` — Claude Code ports
4. `ecc-universal` — 251 skills

## Test Count Milestones
- 164: initial autoheal
- 334: integrations v1
- 453: integrations v1.1
- 772: integrations v1.3.0 (BATCH A complete)
- 850+ : expected after BATCH B + C
