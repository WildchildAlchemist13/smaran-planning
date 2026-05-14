# CLAUDE.md — Smaran Project Context

> When Claude works in this folder, it's working on **Smaran (स्मरण)** — a daily darshan + Bhagavad Gita app with Ask Krishna AI.
>
> **Important**: This folder (`Smaran/`) holds **planning + content + brand docs**. The actual app code lives in the sibling folder `Smaran-app/` — that's a separate Expo project with its own git repo.

## Quick Context

- **Project**: Smaran (स्मरण) — daily darshan + Bhagavad Gita mobile app
- **Name meaning**: Sanskrit for "remembrance" — Krishna's central teaching in Gita 8.5, 8.14
- **Owner**: Rev (solo founder, agent-driven, non-technical)
- **Status**: Pre-build (Week 0 — locking decisions and dependencies)
- **Target**: Ship to Google Play Store in 5–6 weeks
- **Stack**: Expo (React Native + TypeScript) + Supabase + Razorpay + RevenueCat + Claude Haiku 4.5
- **Code location**: `/Users/revanthsharma/Claude Code Desktop/Smaran-app/` (separate from this planning folder)

## Files in this folder (read these for context)

- `README.md` — product overview, decisions, tiers, budget
- `BUILD_PLAN.md` — week-by-week 5-week execution plan with gates
- `TASKS.md` — live working task list
- `RISKS.md` — risk register
- `DECISIONS.md` — log of key decisions with rationale

## How to operate in this folder

1. **Always read `TASKS.md` first** to know current state
2. **Check `BUILD_PLAN.md`** to know what week we're in and what's expected
3. **Update `TASKS.md`** as tasks complete or new ones emerge
4. **Add to `DECISIONS.md`** whenever a new strategic call is made
5. **Add to `RISKS.md`** whenever a new risk surfaces
6. **Don't add features beyond the locked spec in `README.md`** without flagging it as a scope change

## Cultural / sensitivity note

This is a devotional product. When working on:
- AI prompts → reverent tone, citation-driven, never claim divinity
- Marketing copy → "stewards, not extractors"
- Art / design → original commissioned only, never AI-generated deity faces
- Translations → public domain or commissioned, never copyrighted (Easwaran, Prabhupada)

## Don't do

- Don't add features Rev hasn't approved
- Don't ship without scholar review of religious content
- Don't compromise on art quality for budget
- Don't bypass the weekly gates in `BUILD_PLAN.md`
- Don't write code without first updating `TASKS.md`

## Bias toward action

Rev wants execution. When in doubt, do the work and ask after — but only on reversible things. For irreversible decisions (paid commissions, public launches, payment flows in production), confirm first.
