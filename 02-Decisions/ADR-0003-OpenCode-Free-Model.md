---
tags: [adr, decision]
status: accepted
date: 2026-06-06
---

# ADR-0003: OpenCode Free Model (opencode/minimax-m3-free)

## Context
OpenCode supports many models. Anthropic Sonnet, GPT-4, etc. are paid. We need cost-effective for VPS.

## Decision
- **opencode/minimax-m3-free** on OpenCode Zen
- Free tier, no API key, baseURL `https://opencode.ai/zen/v1`
- Verified fast (5-30s responses)
- No rate limit encountered in 100s of sessions

## Consequences
**Easier:**
- Zero cost
- No API key management
- OpenCode Zen handles auth

**Harder:**
- Limited to model capabilities (not as smart as Sonnet)
- No fine-tuning
- If OpenCode Zen goes down, no fallback

## Alternatives Considered
- **deepseek-v4-flash-free** (previous): slow + inconsistent
- **Anthropic Sonnet**: paid, expensive
- **Ollama local**: requires GPU

## Notes
- Date: 2026-06-06
- Deciders: @aer
- Related: [[OpenCode-Setup]]
