# UI/UX Pro Max Skill — Setup 2026-06-06

## Repo
https://github.com/nextlevelbuilder/ui-ux-pro-max-skill
- **87.9k stars**, 9.1k forks, MIT License
- v2.5.0 (Mar 10, 2026)
- 161 reasoning rules, 67 UI styles, 96 color palettes, 57 font pairings, 99 UX guidelines, 25 chart types, 13 tech stacks

## Installation
```bash
npm install -g uipro-cli
uipro init --ai opencode    # project-level: .opencode/skills/ui-ux-pro-max/
uipro init --ai opencode    # (no --global flag, just run from each project root)
```

## Usage
```bash
# Get design system for a product
python3 .opencode/skills/ui-ux-pro-max/scripts/search.py "<query>" --design-system -p "<name>" -f markdown

# Domain-specific search
python3 .../search.py "<query>" --domain {style|color|typography|chart|ux-guideline|landing|product}

# Stack-specific
python3 .../search.py "<query>" --stack {react|nextjs|vue|html-tailwind|swiftui|...}

# Persist to design-system/ folder (Master + Page overrides)
python3 .../search.py "<query>" --design-system --persist -p "<name>" [--page "<page>"]
```

## Design System Applied to UGC Project
See `docs/DESIGN-SYSTEM.md` in the project.

**Landing/Marketing:** Vibrant & Block-based, Video-First Hero, rose/blue palette
**Analytics Dashboard:** Data-Dense Dashboard, blue/amber, Fira Code + Fira Sans

## Files
- `SKILL.md` — auto-activates for UI/UX work
- `data/*.csv` — 161 product types, 67 styles, 96 palettes, 57 fonts
- `scripts/search.py` — BM25-ranked search + design system generator
- `cli/` — uipro CLI installer

## Why this matters
- Indonesian user complained Notion dashboard is ugly ("jelek banget")
- This skill gives industry-specific design systems in seconds
- Replaces 3+ hours of design decisions with 1 prompt
- OpenCode auto-loads skill on UI/UX requests

## Re-design Targets
1. **Notion dashboards** (notion_sync.py) — apply status colors, compact views
2. **Web dashboard** (web/dashboard.py) — Data-Dense Dashboard style, blue/amber
3. **Landing page** (if created) — Video-First Hero
4. **CLI help output** (main.py) — better visual hierarchy

## Next Steps
- [ ] Dispatch BATCH L: redesign web/dashboard.py with new design system
- [ ] Update Notion DB schema in notion_sync.py with proper status colors
- [ ] Create design system overrides per page (dashboard, campaigns, content)
