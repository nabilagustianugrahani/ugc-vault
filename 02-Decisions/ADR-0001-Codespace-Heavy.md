---
tags: [adr, decision]
status: accepted
date: 2026-06-06
---

# ADR-0001: Codespace-Heavy Architecture

## Context
VPS has 8GB RAM, 1 vCPU. Heavy work (opencode runs, integration tests, ffmpeg) would OOM the VPS and slow down the auto-heal daemon and Notion sync.

## Decision
- **VPS = thin coordinator only** — runs OpenCode in minimal mode, dispatches work, monitors health
- **Codespaces = heavy compute** — all build/test/agent work happens there
- Codespaces already at Pro level (4-core/16GB available)
- 3 PATs available if we need more codespaces

## Consequences
**Easier:**
- VPS stays responsive (health check <1s)
- Codespace can be killed/restarted without losing state
- Parallelism by adding codespaces, not upgrading VPS
- Free compute (GitHub Pro codespace hours)

**Harder:**
- 2 places to maintain (VPS config + codespace config)
- Network round-trip latency for `gh codespace ssh` (2-3s)
- Git pushes need PAT in remote URL

## Alternatives Considered
- **Upgrade VPS**: rejected because $25/month for 16GB is wasteful when codespace is free
- **Run everything on VPS**: rejected because OOM risk + single point of failure
- **Use cloud (Railway/Render)**: rejected because $5/mo gets less than codespace

## Notes
- Date: 2026-06-06
- Deciders: @aer
- Related: [[VPS-Codespace-Setup]], [[System-Overview]]
