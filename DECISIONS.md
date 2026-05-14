# Smaran — Decisions Log

> Record of key decisions made and why. Helps future-Rev (and future-Claude) understand context.

---

## D1. Concept locked: Daily Darshan + Gita + Ask Krishna premium
**Date**: 2026-05-14
**Context**: Started exploring micro-SaaS ideas → driving instructor app → Cal AI clone → Bhagavad Gita Daily → "Hallow analog" → Daily Darshan (Bhavesh insight) → unified product.
**Decision**: Single product combining 30-second daily darshan habit hook with premium daily Gita ritual + Ask Krishna AI.
**Why**:
- Daily Darshan alone has weak monetization
- Gita Daily alone has cold-start problem
- Combined: hook + depth in one product
- Bhavesh's IG community proved the daily darshan engagement model
**Alternatives considered + rejected**:
- Driving instructor SaaS — printable but soulless, lower ceiling
- Cal AI for Indian food — high upside but red-ocean fight
- Pure Gita Daily premium — too high friction to onboard
- Hybrid two-tier with massive free tier — overcomplicated

---

## D2. Pricing tiers locked
**Date**: 2026-05-14
**Decision**:
- Free: 3 Ask Krishna questions/month
- Premium: ₹399/yr or ₹999 lifetime, 5 questions/day
- Devotee: ₹999/yr or ₹2,499 lifetime, 20 questions/day + voice mode
**Why**:
- Premium pricing matches Hallow / Stoic norms but cheaper
- Lifetime tier is critical for India (Indians distrust monthly subs)
- Daily limits cap AI cost exposure at predictable maximum
- Three tiers covers free, normal, power user

---

## D3. Razorpay chosen as payment provider
**Date**: 2026-05-14
**Context**: Rev mentioned existing Razorpay account
**Decision**: Use Razorpay for all payments; wrap with RevenueCat for unified subscription management
**Why**:
- India-first market; Razorpay native to UPI + Indian cards
- Lower fees than Stripe in India
- Rev already has account = no setup friction
- RevenueCat layer means we can add Apple/Google IAP later without rebuilding payment logic
**Trade-off accepted**: Stripe would be smoother for international diaspora users; that's a v2 decision.

---

## D4. Tech stack locked
**Date**: 2026-05-14
**Decision**:
- Frontend: Flutter (Android first)
- Backend: Supabase
- Subscriptions: Razorpay + RevenueCat
- AI: Claude Haiku 4.5
- Local DB: SQLite via Drift (offline-capable)
- Notifications: Firebase Cloud Messaging
**Why**:
- Flutter matches Rev's existing stack (Reeloom)
- Supabase = free tier sufficient for launch; cheap to scale
- Claude Haiku = cheapest capable model + native prompt caching
- All choices support agent-driven build (well-documented, well-supported)

---

## D5. Build duration: 5 weeks (+ 1 week pre-build)
**Date**: 2026-05-14
**Decision**: 6 weeks total from today to Play Store live
**Why**:
- Realistic given Rev's parallel commitments (Reeloom, Cysters Club)
- Aggressive but not impossible with agent-driven build
- Each week ends with a hard gate to prevent feature creep
**Trade-off accepted**: Might slip to 7–8 weeks if Bhavesh delays or art quality requires re-commission. Acceptable.

---

## D6. Launch market: India-first, diaspora later
**Date**: 2026-05-14
**Decision**: Launch in India (Hindi/English), expand to Hindu diaspora (US/UK/AU/Canada) in month 4–6
**Why**:
- India is bigger market for daily darshan habit (1B+ Hindus)
- Razorpay is India-optimized
- Indian content creators (Bhavesh + others) are the launch lever
- Diaspora has higher WTP but smaller numbers; better as expansion play after PMF proven

---

## D7. Philosophical lane: mainstream Vedanta (Vivekananda-Chinmaya-modern)
**Date**: 2026-05-14 (recommendation; awaiting Rev's final lock)
**Why**:
- Most accessible to broad Hindu + spiritual-seeker audience
- Avoids ISKCON sectarianism while remaining authentic
- Eknath Easwaran's framing is ideal but copyrighted → use as inspiration, commission original
**Status**: To be confirmed with chosen scholar in Week 0

---

## D8. Art: original commissioned, never AI-generated faces of deities
**Date**: 2026-05-14
**Decision**: All deity art is original, commissioned from established Indian devotional artists. AI-generated deity faces are banned.
**Why**:
- Cultural authenticity is a moat
- AI-generated devotional art is detectable and brand-killing
- People feel the difference between AI slop and reverent commissioned work
- Worth ₹3–5L investment for the asset

---

## D13. Tech stack pivot: Flutter → Expo (React Native + TypeScript)
**Date**: 2026-05-15
**Context**: Original D4 decision was Flutter (matching assumed Reeloom Games stack). Rev confirmed they're using **Expo Go** for app development.
**Decision**: Build Smaran on **Expo SDK (latest) + React Native + TypeScript + Expo Router**. Code lives in `/Users/revanthsharma/Claude Code Desktop/Smaran-app/` (separate from planning folder).
**Why**:
- Matches Rev's actual current stack
- Expo Go enables fast prototyping during Week 1–2
- Expo ecosystem covers everything needed: expo-notifications, expo-av (audio), expo-sqlite (local DB), expo-secure-store, expo-sharing, expo-file-system
- Supabase JS SDK works perfectly with React Native
- TypeScript-first = fewer runtime bugs in a content-heavy app
- EAS Build (when we add Razorpay in Week 3) handles native module integration cleanly
**Stack**:
- App framework: Expo SDK 51+ with Expo Router (file-based routing)
- Language: TypeScript
- Backend: Supabase (Postgres + Auth + Storage + Edge Functions)
- Payments: Razorpay via `react-native-razorpay` (requires EAS dev build, not Expo Go) — added Week 3
- Subscription mgmt: RevenueCat (`react-native-purchases`)
- AI: Claude Haiku 4.5 via Supabase Edge Function (server-side)
- Audio: expo-av
- Local DB: expo-sqlite
- Notifications: expo-notifications + Firebase Cloud Messaging
- UI: Custom built with React Native + brand from BRAND.md (no heavy UI lib in v1)
**Trade-offs accepted**:
- Razorpay native SDK doesn't work in Expo Go (need EAS dev build by Week 3)
- Slightly slower than Flutter for graphics-heavy apps (irrelevant for Smaran)
**Phasing**:
- Week 1–2: Expo Go for fast iteration
- Week 3+: Switch to EAS development build when adding Razorpay + native modules
**Status**: Locked. D4 superseded.

---

## D12. Sanskrit chants recorded by Rev's mother
**Date**: 2026-05-14
**Context**: Original plan was to commission a professional Sanskrit reciter (₹50–80k). Rev's mother is skilled at Sanskrit shloka recitation.
**Decision**: Rev's mother will record all Sanskrit verses for Smaran, in home-studio setup. Professional reciter sourcing dropped.
**Why**:
- Authentic devotion vs transactional voice work — palpable in the audio
- Becomes a **brand story** ("recorded by my mother") that no competitor can copy
- Saves ₹50–80k from Phase 1 content budget
- Personally meaningful — Smaran becomes a family-touched product, not a corporate output
- Flexible schedule (home recording, mom's availability)
**Trade-offs accepted**:
- Need to buy decent home recording equipment (~₹8–12k one-time)
- Need quality control on pronunciation for less-recited verses (scholar can review batch-wise)
- Phased delivery — likely 50–100 verses for launch, rolling additions over 2–4 months
- Backup plan needed if mom is unable to continue (use professional reciter as fallback for remaining batches)
**Brand implications**:
- Add "Voice: Smt. [mom's name]" credit in app's About / Acknowledgments page
- Story-led marketing: a 60-sec reel of mom recording, behind-the-scenes
- Diaspora users will deeply resonate with "recorded by his mother" — every diaspora kid has a mom who chants
**Status**: Locked. CONTENT_BRIEF.md updated.

---

## D11. Razorpay + Play Console infrastructure shared with existing team apps
**Date**: 2026-05-14
**Decision**: Reuse Rev's existing team Razorpay merchant account (already active for other apps) and existing Google Play Console developer account for Smaran.
**Why**:
- Zero KYC delay (was the longest pre-build blocker)
- Team familiarity with the systems
- Smaran becomes a new app under the same merchant umbrella (Razorpay supports multiple products per account)
**Implications**:
- Need to set up a separate sub-account / billing label inside Razorpay so Smaran revenue is trackable separately
- Need to create a new app entry in Play Console
- If Smaran scales fast, may want to migrate to its own merchant account later for clean financial separation (revisit at ₹50L+ ARR)
**Status**: Locked. Removes 2–3 days of KYC wait from Week 0 — build can start sooner.

---

## D10. App name locked: Smaran (स्मरण)
**Date**: 2026-05-14
**Context**: Brainstormed 10 candidate names across Sanskrit-rooted, descriptive, and coined categories. Top 5 finalists: Mandir, Darshan, Sthita, Smaran, Nitya. Rev was drawn to Smaran on first read.
**Decision**: Lock **Smaran (स्मरण)** as the official app name. Pronounced "smah-RUN."
**Why**:
- Sanskrit for "remembrance" — directly references Krishna's central teaching (Gita 8.5 + 8.14: "remember Me always")
- One of the nine forms of bhakti (nava-vidha bhakti) — deeply rooted, not generic
- Brand points to user transformation ("what you do") not product feature ("where you go") — same pattern as Calm, Hallow, Stoic
- Distinctive in app stores; low SEO competition; ownable
- Premium feel that ages well over 5+ years
- Founder pull — Rev's first instinct after seeing the list, which matters for a name said 10,000+ times
**Trade-offs accepted**:
- Needs explanation for first-time hearers (mitigated by always-present tagline)
- Less mass-market recognition than Mandir (acceptable — premium positioning)
- Less Instagrammable than visual words like Diya or Aarti (forces deeper, calmer marketing)
**Backup domain options if smaran.com taken**:
- smaran.app (preferred — premium consumer convention)
- smaran.in (India-coded)
- getsmaran.com / besmaran.com / usesmaran.com / smaranapp.com / smaran.life
**Status**: Locked. Domain + handle resolution deferred to Week 0 P2 task.

---

## D9. Build-first strategy: no creator outreach until Play Store launch
**Date**: 2026-05-14
**Context**: Original plan had Bhavesh outreach as Week 0 P0 task. Rev pointed out: he follows Bhavesh but doesn't know him personally. Pitching a stranger an idea is weak; pitching them a working product with metrics is a different conversation.
**Decision**: Build the app, launch to Play Store, then approach Bhavesh + 10 other devotional creators with proof in hand.
**Why**:
- Removes a launch dependency on someone else's response
- Stronger pitch when we have actual users + metrics
- Forces us to design distribution that doesn't rely on one person (good discipline)
- Cold-launch channels (WhatsApp viral + ASO + Reddit + Cysters Club) are sufficient for first 500–2000 users
- Founder energy better spent on building than on convincing strangers pre-product
**Trade-off accepted**: Slower Phase 1 growth (no creator-led launch boost). Mitigated by stronger Phase 2 outreach.
**Status**: Locked. R3 in RISKS.md updated to reflect cold-launch strategy.

---

_Add new decisions as they emerge. Each decision should have: Date, Context, Decision, Why, Alternatives considered._
