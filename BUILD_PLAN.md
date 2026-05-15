# Smaran — 5-Week Build Plan

> **Updated 2026-05-16**: Synthesis from `RESEARCH_SYNTHESIS.md` reshaped Week 0, Week 2, and Week 3. New rows below carry an asterisk (*).

> Day 0 = the day Rev commits to build. Adjust dates from there.
> Each week ends with a **gate** — a hard go/no-go decision before continuing.

---

## Week 0 — Pre-Build Sprint (1 week, parallel to nothing)

**Goal**: Lock all decisions and dependencies before writing a single line of Flutter.

> **Build-first strategy locked**: ship to Play Store before any creator outreach. The app speaks for itself. Bhavesh + creators are post-launch outreach with proof in hand.

| Day | Task | Owner |
|---|---|---|
| 1 | Lock final app **name** (Mandir / Darshan / Pranam / Subh / etc.) | Rev |
| 1 | Open Razorpay merchant account (KYC takes 2–3 days) | Rev |
| 2 | Open Google Play Console developer account ($25 one-time) | Rev |
| 2 | Register domain + reserve handles (IG, X, YT) | Rev |
| 3 | Brief **3–5 Indian devotional artists** for sample illustrations (1 trial each, paid) | Rev |
| 3 | Find + brief a **Vedanta scholar / Gita teacher** for sample commentary (3 verses trial) | Rev |
| 4 | Decide **deity-day calendar** (which deity gets which day, with traditional sourcing) | Rev + scholar |
| 4 | Decide **philosophical lane** (recommended: mainstream Vedanta) | Rev |
| 5 | Receive artist samples + scholar samples; pick winners | Rev |
| 5 | Sign artist + scholar contracts; commission **30 deity illustrations + 30 verses commentary** for launch | Rev |
| 6 | Commission **Sanskrit reciter** for 30 launch verses (audio sample first) | Rev |
| 7 | Open RevenueCat account, link Razorpay | Rev |
| 1–7 | * **Safety architecture spike (D16)**: scholar-reviewed refusal rules, scoped-retrieval canon list, system-prompt v0 — pre-build blocker for Week 3 AI work | Rev + scholar + Tara |
| 5–7 | * **AI feature rebrand (D17)**: shortlist 5 candidate names, include in interview/survey instruments | Tara |
| 7 | * **RevenueCat diaspora SKUs (D19)**: configure $19.99/yr · $49.99 lifetime · Devotee $49.99/$124.99 alongside India tiers | Rev |

**🛑 GATE 0 (end of Week 0)**: App name locked. Artist + scholar locked. Razorpay merchant approved. Play Console active. **Safety architecture refusal rules drafted and scholar-reviewed.** Diaspora SKUs configured in RevenueCat. **DO NOT start Week 1 build until these are clear.**

---

## Week 1 — Core App Skeleton

**Goal**: Working app with auth, daily image display, and basic notification flow.

| Day | Task | Tech detail |
|---|---|---|
| 1 | Init Flutter project; set up Supabase backend | Flutter create + Supabase project |
| 1 | Set up app icon, splash screen, brand color palette | Use locked brand from Week 0 |
| 2 | Build phone OTP auth via Supabase | Test on real device |
| 2 | Build user profile schema (name, ishta-devata, streak, joined date) | Supabase Postgres |
| 3 | Build daily-image display screen (full-screen, beautiful) | Asset CDN + cache |
| 3 | Build deity-of-day logic (server-side day → deity mapping) | Supabase function |
| 4 | Build local notification scheduler (6:30am local time) | Flutter local_notifications |
| 4 | Add streak counter logic (open daily, +1; miss day, reset) | Local + sync |
| 5 | Build "Today's Verse" display (1-line preview under image) | Static text v1 |
| 5 | Add ❤️ save button (saves to user's favorites) | Supabase table |
| 6 | Add 📤 WhatsApp share — generates image + caption + UTM-tracked link | url_launcher + share_plus |
| 7 | Internal demo to self; fix obvious UX issues | Manual QA |

**🛑 GATE 1 (end of Week 1)**: Open app every morning for 7 days as your own user. Does it feel beautiful? Does the notification trigger you to open? If not — design issue, fix before Week 2.

---

## Week 2 — Premium Features Build

**Goal**: Premium tier feature set ready (commentary, audio, journal, themes).

| Day | Task | Tech detail |
|---|---|---|
| 1 | Build "Read full" → commentary screen | Markdown rendering |
| 2 | Audio player for Sanskrit + English narration | Flutter audioplayers package |
| 2 | Audio asset CDN setup (Supabase Storage or Bunny CDN) | Cost-optimized |
| 3 | Reflection prompt + journal entry screen | Local DB + cloud sync |
| 3 | Journal history view (past reflections as personal book) | Drift SQLite |
| 4 | Themes browse — verses by life situation (anxiety, work, grief, purpose, etc.) | Tag-based filter |
| 4 | HD wallpaper download (deity image, watermark-free for premium) | One-tap download to gallery |
| 5 | Personal deity selection (set ishta-devata, gets priority in rotation) | User pref |
| 5 | "Send personalized blessing" — friend's name on today's image, share via WhatsApp | Image overlay generation |
| 6 | Festival countdown banner (Diwali, Navratri, etc.) | Hardcoded festival calendar v1 |
| 7 | Premium gating logic — paywall on commentary/audio/AI/HD | Feature flag system |
| 7 | * **Hard-paywall architecture (D18)**: free tier = breadth only (image + verse line + streak + share + 1 AI Q/week); paywall on commentary, audio, AI ≥ 2/week, journal | Feature flag system |
| 7 | * **Diaspora Play Store listing variant (D19)**: separate screenshots, English-only copy, USD pricing visible | Tara drafts, Rev approves |

**🛑 GATE 2 (end of Week 2)**: All premium features functional in dev mode (paywall not yet enforced). Demo to 2 trusted friends. **Hard-paywall density validated against primary research synthesis (lands Monday W3).**

---

## Week 3 — Ask Krishna AI + Razorpay

**Goal**: Killer feature live + payments working end-to-end.

| Day | Task | Tech detail |
|---|---|---|
| 1 | Build Ask Krishna chat UI | Simple chat thread component |
| 1 | Set up Claude Haiku 4.5 API integration (server-side, not client) | Supabase Edge Function |
| 2 | Build verse RAG database (700 verses + translations + scholar commentary embedded) | pgvector or simple keyword retrieval v1 |
| 2 | Write Ask Krishna **system prompt v1** (persona + refusal rules + citation pattern) | See ASK_KRISHNA_PROMPT.md |
| 3 | Wire prompt caching (Anthropic native) — cache system prompt + verse DB | Cost control |
| 3 | Build daily limit logic (Free: 3/mo, Premium: 5/day, Devotee: 20/day) | Server-side enforcement |
| 4 | Build "limit reached" gentle UX ("5 reflections shared today...") | Premium upsell prompt at limit |
| 4 | Voice input for Ask Krishna (premium) — speech-to-text | Device-native |
| 5 | Razorpay subscription flow — Premium ₹399/yr | Razorpay subscriptions API |
| 5 | Razorpay one-time payment flow — Premium lifetime ₹999 | Razorpay orders API |
| 6 | RevenueCat integration (wrapping Razorpay + future Google Play IAP) | RevenueCat SDK |
| 6 | Devotee tier — ₹999/yr + ₹2,499 lifetime | Razorpay + RC |
| 7 | End-to-end test: free → Ask Krishna 1× (free weekly limit) → hit paywall → pay via Razorpay → unlock → ask 5× | Real transaction with own card |
| 6 | * **Transactional layer build (D15)**: virtual diya ₹51 / $1.99 flow + personalized blessing share ₹21 / $0.99 flow | Razorpay one-time orders API |
| 7 | * **Transactional layer test**: end-to-end purchase of virtual diya, confirmation screen, "offering" framing copy review | Real transaction |
| 7 | * **Ask Krishna safety production gate**: scholar reviews 50-prompt eval set; refusal rate, citation rate, off-canon leakage all within tolerance | Tara + scholar |
| 7 | * **About-screen safety statement live (D16)**: public language on how Smaran's AI is built differently | Tara drafts |

**🛑 GATE 3 (end of Week 3)**: Successfully complete Razorpay payment + premium unlock + AI response in production. **Transactional layer ships at least one purchase flow live. Scholar signs off on safety-architecture eval set.** **This is the riskiest gate — payment + safety + transactional must all work flawlessly.**

---

## Week 4 — Polish + Internal Beta

**Goal**: 25 beta testers actively using; bugs surfaced and fixed.

| Day | Task |
|---|---|
| 1 | Polish onboarding flow (3-screen intro: ritual / Gita / Ask Krishna) |
| 1 | Polish all loading states + error states |
| 2 | Cost monitoring dashboard (questions/user, AI cost trend, refusal rate) — internal admin view |
| 2 | Analytics: PostHog events for open, share, save, paywall hit, conversion |
| 3 | Recruit 25 beta testers — Hindu friends, family, diaspora contacts, Cysters Club community, devotional Reddit DMs |
| 3 | Push beta build via Play Console internal testing track |
| 4–5 | Daily review of beta feedback; fix top 5 issues |
| 6–7 | Final bug bash + UI polish |

**🛑 GATE 4 (end of Week 4)**: 25 beta users opened app at least 5 times each. Conversion rate to premium ≥ 2% in beta. If <1% — pricing or paywall placement is wrong; fix before launch.

---

## Week 5 — Launch Prep + Soft Launch

**Goal**: App live on Play Store; first 100 paying users.

| Day | Task |
|---|---|
| 1 | Final QA pass on real devices (3+ Android phones — different OEMs) |
| 1 | Play Store listing — screenshots, description, keywords, video preview |
| 2 | Submit to Google Play Store review (typically 24–48 hrs) |
| 2 | Build landing page (mandir.app or similar) — App Store badges + screenshots + Bhavesh quote (if engaged) |
| 3 | Prep launch creative (5 IG reels, 3 stories, WhatsApp launch image) |
| 3 | Set up own Instagram + share early app screenshots organically |
| 4 | Soft launch — friends + family + Cysters Club community + WhatsApp circles |
| 5 | Monitor cost dashboard hourly; watch for AI cost spikes / paywall failures |
| 5 | Reddit launch (r/hinduism, r/bhagavadgita, r/spirituality, r/devbhoomi) |
| 6 | Indian Quora threads on Gita / devotion — answer with subtle app mention |
| 7 | First week-end retro — what worked, what didn't, plan Week 6+ creator outreach |

**🛑 GATE 5 (end of Week 5)**: App live on Play Store. ≥500 downloads. ≥10 paying users. AI cost trending <30% of revenue. If any of these fail — pause, diagnose, don't add features.

---

## Post-Launch (Week 6+)

Out of scope for this plan, but key follow-ups:
- **Creator outreach with proof in hand** — Bhavesh + 10 other devotional Instagram accounts; pitch is "we have N users opening daily, would you try it / share with your community?"
- Continue art commissioning (target 365 unique illustrations by month 6)
- Continue commentary commissioning (1 verse/day rolling)
- iOS build (if Android proves PMF)
- Diaspora Instagram + Hindu Students Associations outreach (US/UK)
- Festival pack v1 (Diwali — late October)
- Devotee tier voice mode (audio responses)

---

## Daily Operating Rhythm (during build)

- **6:30am**: morning standup (auto-prompt from /schedule) — what's the one thing to ship today?
- **End of each day**: update `TASKS.md` (mark done, add new, surface blockers)
- **Friday EOD**: weekly review — gate check, plan adjustments

---

## Realistic Total Time Investment

- **Rev's time**: ~25 hrs/week for 5 weeks = 125 hrs (≈3 weeks of full-time)
- **Agent time** (Claude Code building): runs in parallel; not your bottleneck
- **External**: artist + scholar + reciter delivery = parallel; not your bottleneck

The **bottleneck is Rev's decision-making and review cadence**, not build speed. Plan for that.
