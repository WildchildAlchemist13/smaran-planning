# Smaran (स्मरण) — Daily Darshan + Bhagavad Gita App

> *Smaran (smah-RUN) — Sanskrit for "remembrance." Krishna's central teaching in the Gita: remember the divine in every moment. Now, in your pocket.*

> *Wisdom of the Gita, in your pocket. Daily darshan every morning. Ask Krishna anything, anytime.*

## The Product (locked)

A premium devotional + wisdom mobile app combining:

1. **Daily darshan hook** — every morning, push notification with stunning art of the deity for that day (Mon=Shiva, Tue=Hanuman, Wed=Ganesh, Thu=Vishnu, Fri=Lakshmi, Sat=Shani/Hanuman, Sun=Surya)
2. **Daily Gita verse** — one verse, curated to match the deity's energy
3. **Ask Krishna AI** (premium killer feature) — user asks any life question; gets a Gita-grounded response with verse citations

## The Tiers

| Tier | India | Diaspora (US/UK/AU/CA) | AI questions | Other features |
|---|---|---|---|---|
| **Free** | ₹0 | $0 | 1 question/week | Daily image + 1-line verse + streak + share |
| **Premium** | ₹399/yr or ₹999 lifetime | $19.99/yr or $49.99 lifetime | 5 / day | Full commentary, audio, journal, themes, HD wallpapers |
| **Devotee** | ₹999/yr or ₹2,499 lifetime | $49.99/yr or $124.99 lifetime | 20 / day + voice mode | All Premium + priority + exclusive art |

Plus a **transactional layer** (per D15): light a virtual diya ₹51 / $1.99 · sponsor a verse's audio ₹101 / $3.99 · send a personalized deity-of-day blessing ₹21 / $0.99.

## Core Tech Decisions (locked)

- **Frontend**: Expo (React Native + TypeScript), Expo Router. Android-first, iOS later. *(D4 superseded by D13.)*
- **Backend**: Supabase (Postgres + Auth + Storage + Edge Functions)
- **Subscriptions / Payments**: Razorpay (India-first, UPI + cards + lifetime IAP) + diaspora-premium SKUs in RevenueCat
- **Subscription management layer**: RevenueCat (handles Razorpay + later Apple/Google IAP)
- **AI**: Claude Haiku 4.5 with prompt caching + scoped Vedanta-canon retrieval (per D16 safety architecture)
- **Audio**: Sanskrit recitation by Rev's mother (D12); English narration commissioned
- **Notifications**: Firebase Cloud Messaging
- **Local DB / offline**: expo-sqlite

## Goal

Ship to Google Play Store within 5–6 weeks of build start.

## Distribution

**Phase 1 (build → launch)**: Cold-launch strategy. No creator dependency. Channels:
- WhatsApp viral via "send today's blessing" feature (built into product)
- ASO targeting Hindu / devotional / Gita keywords from Day 1
- Reddit launch (r/hinduism, r/bhagavadgita, r/spirituality)
- Cysters Club community as warm bridge audience
- Friends, family, diaspora contacts
- Rev's own organic Instagram + Quora content

**Phase 2 (post-launch, Week 6+)**: Creator outreach with proof in hand
- Bhavesh Bhimanathani + 10 other devotional Instagram accounts
- Approach with metrics: "we have N users opening daily — would you try it?"
- Much stronger pitch than cold pre-launch outreach

## Budget (Phase 1 — first launch version)

| Item | Estimate |
|---|---|
| Build (agent-led, my time) | ₹15–25k |
| Original deity art (30 illustrations to launch, 365 over year 1) | ₹1.5–2.5L |
| Sanskrit + English audio (700 verses) | ₹50–80k |
| Scholar commentary (365 verses) | ₹50k–1.5L |
| Designer (brand + key screens) | ₹40–80k |
| Marketing seed (creator collabs) | ₹50k–1L |
| Misc (Play Console, Razorpay setup, domain) | ₹15k |
| **Total Phase 1** | **₹3.5–6.5L** |

## Files in this folder

- `README.md` — this file
- `BUILD_PLAN.md` — week-by-week, day-by-day execution plan
- `TASKS.md` — active task list (live, updated)
- `RISKS.md` — risk register with mitigations
- `DECISIONS.md` — log of key decisions made and rationale
- `CONTENT_BRIEF.md` — art + audio + commentary commissioning spec (to be built)
- `BHAVESH_OUTREACH.md` — outreach message + partnership terms (to be built)
- `ASK_KRISHNA_PROMPT.md` — system prompt and persona for the AI feature (to be built)
