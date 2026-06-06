---
tags: [architecture, infrastructure]
---

# VPS + Codespace Setup

## VPS (home/aer, 8GB RAM, 1 vCPU)
- **Role**: thin coordinator only
- **No heavy work** — push everything to codespace
- **Services**:
  - OpenCode (minimax-m3-free, 5 plugins)
  - Auto-heal daemon
  - Notion sync
  - Health monitor
  - Obsidian vault (this!)

## Codespaces (nabilagustianugrahani)
| Name | Specs | Role |
|------|-------|------|
| symmetrical-palm-tree-5gpr979vgv6jhvqr | 2/8/32 | Codespace 1 — content tools |
| opencode-3-big-pjqw6567j9g9hg96 | 4/16/32 | Codespace 2 — perf layer |

Both have:
- OpenCode 1.16+ w/ 5 plugins
- 106 skills
- Python 3.12.13
- sshd feature (for `gh codespace ssh`)
- ffmpeg, gcc, git

## Git
- Repo: `nabilagustianugrahani/ugc`
- PAT in remote URL (for codespace push)
- VPS uses `gh` CLI (nabila token)
- Sign-off disabled (`commit.gpgsign=false`)

## PATs Available
1. nabilagustianugrahani (original) — owner
2. onaema — extra
3. badjals — extra
