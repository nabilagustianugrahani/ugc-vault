---
tags: [adr, decision]
status: accepted
date: 2026-06-06
---

# ADR-0004: Obsidian Vault Pattern for OpenCode Memory

## Context
OpenCode on VPS needs persistent memory across sessions. Skills load at startup but lose session context.

## Decision
- **Obsidian vault** at `/home/aer/ugc-vault/` (this!)
- Markdown + `.obsidian/` config + git sync to GitHub
- OpenCode reads/writes via standard `read`/`write`/`edit` tools
- User can browse in Obsidian locally via `git pull`
- 9 folders: Inbox, Architecture, Decisions, Research, Integrations, Operations, Daily, OpenCode-Memory, Templates

## Consequences
**Easier:**
- Plain markdown — OpenCode can manage
- Git-backed → version history
- Obsidian app does backlinks/graph/tags for free
- User can read in any markdown viewer
- Searchable across sessions

**Harder:**
- Manual organization (or skill-driven)
- Need discipline to update
- Git push required to sync to GitHub

## Alternatives Considered
- **Notion-only**: rejected because VPS-side, local-first preferred
- **Pure git notes**: rejected because Obsidian UX is better for browsing
- **Custom DB**: rejected because over-engineering

## Notes
- Date: 2026-06-06
- Deciders: @aer
- Related: [[System-Overview]], [[Patterns-Learned]]
