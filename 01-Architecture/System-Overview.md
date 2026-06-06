---
tags: [architecture, overview]
---

# System Overview

## High-Level
```
                ┌────────────────────┐
                │   USER (Nabila)    │
                │   Obsidian lokal   │
                └──────────┬─────────┘
                           │ git pull
                           ▼
   ┌──────────────────────────────────────────┐
   │  VPS (thin coordinator)                  │
   │  /home/aer/ugc-vault  ← KB              │
   │  /home/aer/ugc        ← integration code│
   │  OpenCode (minimax-m3-free)             │
   │  auto-heal daemon                        │
   │  Notion sync                             │
   └──────────┬───────────────┬───────────────┘
              │ git+PAT      │ gh codespace ssh
              ▼               ▼
   ┌──────────────────┐  ┌──────────────────────┐
   │ GitHub: ugc repo │  │ Codespace 1 (2c/8g)  │
   │                  │  │ BATCH C content tools│
   │                  │  └──────────────────────┘
   │                  │  ┌──────────────────────┐
   │                  │  │ Codespace 2 (4c/16g) │
   │                  │  │ BATCH B perf layer   │
   │                  │  └──────────────────────┘
   └──────────────────┘
              │ external API
              ▼
   ┌──────────────────────────────────────────┐
   │ External services                        │
   │  Modal.com   (zerocost, SGLang)          │
   │  fal.ai      (premium fallback)         │
   │  TikHub      (16 social platforms)      │
   │  Notion      (8 databases)              │
   │  Umami       (analytics)                │
   │  e-Commerce  (Shopee/TT/Lazada/Tokped)  │
   └──────────────────────────────────────────┘
```

## Key Components

### VPS (Coordinator Only)
- **NO heavy compute** — semua push ke codespace
- OpenCode (free model)
- Auto-heal daemon (16/16 tests passing)
- Notion sync (8 DBs)
- Health monitor + fail-over
- Obsidian vault (this!)

### Codespaces (Compute)
- Codespace 1: `symmetrical-palm-tree-5gpr979vgv6jhvqr` (2/8) — BATCH C
- Codespace 2: `opencode-3-big-pjqw6567j9g9hg96` (4/16) — BATCH B
- Both run OpenCode with same plugins/config
- All git push via nabila PAT

### Integrations (v1.3.0)
20+ modules covering social, e-com, AI gen, character lock, analytics.

### Storage
- SQLite default, Redis optional
- Relationship graph FTS5
- Notion 8-DB mirror
- Modal volume caches

## Scaling Strategy
1. Need more compute? Add codespace (3 PATs available)
2. Need more AI? Apply Modal academic $10k
3. Need more storage? Migrate SQLite → MongoDB Atlas (GSP)
4. Need web UI? Use Appwrite (GSP, $40/mo value)

## Related
- [[VPS-Codespace-Setup]]
- [[OpenCode-Setup]]
- [[Cost-Architecture]]
