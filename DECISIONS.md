# Smaran — Decisions Log

> Record of key decisions made and why. Helps future-Rev (and future-Claude) understand context.

---

## D20. No advertising in Smaran (banner / interstitial / video / network)
**Date**: 2026-05-16
**Context**: Rev asked whether ads were part of the monetization plan. Per existing decisions D2 + D15 + D18 + D19, the model is subs + lifetime IAP + transactional offerings + diaspora-tier pricing — no ad layer was ever planned. Worth locking as an explicit refusal so it can't drift in.
**Decision**: Smaran will not run banner ads, interstitial ads, video ads, or any third-party ad network (AdMob, Meta Audience Network, AppLovin, Unity Ads, etc.) — ever, in any version, free or paid tier.
**Why**:
- **Brand-killing**: every aesthetic-neighbor app (Calm, Hallow, Stoic, Headspace, Aesop) runs zero ads. Sri Mandir + AstroSage do — they're the supermarket, we're the temple. Ads collapse the "restraint as reverence" positioning the moment a Mahindra ad sits next to *karmaṇy-evādhikāras te*.
- **Yield is bad**: Indian Android devotional eCPMs are $0.30–1.50 per 1000 impressions. At every scale stage, subscription revenue out-earns ad revenue by 5–10×. Ads also depress premium conversion ("free works fine with ads, why pay?").
- **Privacy regression**: PRIVACY_POLICY.md explicitly promises "no advertisements, no tracking, no third-party SDKs collecting data." This is the biggest unfair-advantage marketing line we have versus Sri Mandir. AdMob/Meta SDK breaks the promise the day it lands. Play Store data-safety form requires disclosure. Diaspora especially recoils.
- **Tonal disaster risk**: an algorithmic ad network can't be controlled. A tampon ad next to a deity verse, a Muslim brand on a Hindu screen, a competitor's app served as an ad — any one of these is a viral 1-star-review screenshot waiting to happen.
**Allowed alternative — curated sponsorships, never ad networks** (only post-v0.3, only with editorial control):
- A Pichwai art house sponsors Janmashtami art (credited tastefully)
- A Vedanta org sponsors a verse's commentary (attribution + pointer to their classes)
- Devotional book publishers offer giveaways tied to specific verses
These are partnerships, not impressions. Different shape, different feel.
**Trade-offs accepted**: Free users contribute nothing direct. That's fine — they're top of funnel for premium conversion + viral share-loop, not an ad inventory pool.
**Status**: Locked. If anyone (future Tara, future Rev, future engineer) ever proposes adding ads, this decision overrides — no exception without rewriting D20.

---

## D19. Diaspora pricing tier + diaspora store listing in V1
**Date**: 2026-05-16
**Context**: Desk research (see `RESEARCH_SYNTHESIS.md`) confirmed Sri Mandir's diaspora ARPU at ~$81 vs ~$7–9 in India — a ~10× gap. 20% of Sri Mandir usage is already diaspora, growing 15% QoQ. Supersedes D6's "diaspora in months 4–6" timing on the pricing dimension only.
**Decision**: Build geofenced diaspora pricing into V1.
- **India**: Premium ₹399/yr or ₹999 lifetime; Devotee ₹999/yr or ₹2,499 lifetime (unchanged).
- **Diaspora (US/UK/AU/CA)**: Premium **$19.99/yr or $49.99 lifetime**; Devotee **$49.99/yr or $124.99 lifetime**.
- Diaspora-targeted Play Store listing variant (different screenshots, English-only copy) in V1.
**Why**:
- Diaspora unit economics are the realistic path to $2–3k/month per the recalibrated revenue math.
- Same product, geofenced pricing — minimal extra build cost via RevenueCat tier configuration.
- D6's "marketing focus India-first" still holds; this is pricing/SKU, not creator outreach geography.
**Trade-offs accepted**: Higher Razorpay/Stripe complexity for cross-border. RevenueCat layer absorbs most of it.
**Status**: Locked. Updates D2 pricing tiers and D6 diaspora timing.

---

## D18. Hard paywall on depth; tighten free tier
**Date**: 2026-05-16
**Context**: Category conversion benchmarks (RevenueCat State of Subscription Apps 2025): freemium converts at 2–3%; hard-paywall apps at ~12%. Hallow (the Christian analog Smaran cites as aesthetic neighbor) paywalls 85% of content. Current Smaran free tier (image + 1-line verse + streak + share + 3 AI Q/mo) leaks the funnel.
**Decision**: Re-architect the tier wall.
- **Free**: Daily darshan image. One-line verse preview. Streak. Share. **One Ask Krishna question per week** (down from 3/month).
- **Premium**: Everything else — full commentary, audio, journal, festival packs, 5 AI Q/day.
- **Devotee**: 20 AI Q/day + voice mode + early access.
**Why**:
- Hard paywall on depth (commentary, audio, AI) drives ~4–6× higher conversion at category benchmarks.
- Breadth-free / depth-paid is the Hallow / Calm / Stoic pattern and reads premium not stingy.
- Aligns with the "temple, not supermarket" positioning — the deep act of sitting with a verse is where the product earns money.
**Trade-offs accepted**: Some users will bounce at the tighter free limit. That's fine — the goal is paying users, not vanity DAU.
**Status**: Locked. Supersedes free-tier limits in D2.

---

## D17. Rebrand the "Ask Krishna" feature
**Date**: 2026-05-16
**Context**: "Ask Krishna AI - Bhagavad Gita" is already a Play Store app. "GitaGPT," "Ask Swamiji" (JKYog), and "Bhagavad Gita Community AI" are also live. The 2023 Rest of World / CBC / Quint investigation tied "Gita chatbot" branding to documented safety failures (violence approval, political bias). Smaran's current feature name is both undifferentiated and brand-adjacent to the public failure narrative.
**Decision**: Rebrand the AI feature before public launch. Final name to be selected via primary research (interview Q21 + survey G2) from candidates: *Smaran Reflection* / *Gita Companion* / *Sit with Krishna* / *Ask the Gita* / *Krishna's Counsel*.
**Why**:
- Differentiation from a crowded, branded-failed category.
- A reverent name that does not promise to *be* Krishna — protects against the "voice of god" criticism.
- Sub-brand inside the Smaran umbrella reads as premium product, not generic AI wrapper.
**Trade-offs accepted**: Internal docs and the current Smaran-app placeholder screen carry the old name; replace as part of V1 build.
**Status**: Locked. Final name pending primary research (Week 3).

---

## D16. Ask Krishna AI safety architecture is launch-blocking
**Date**: 2026-05-16
**Context**: 2023 Rest of World investigation (corroborated by CBC News and The Quint) documented Indian Gita chatbots condoning violence ("acceptable to kill if it is one's dharma"), showing political bias (praising Modi while criticizing Gandhi), and reproducing casteist and misogynist framings. Promotes RISKS R1 from "open" to "existential confirmed." See `RESEARCH_SYNTHESIS.md` for sourcing.
**Decision**: Safety architecture is a hard launch-blocker. The AI feature does not ship without all of the following:
1. **Scoped retrieval** — RAG over a curated Vedanta-aligned canon only (Gita with scholar-reviewed commentaries: Vivekananda / Chinmaya / Easwaran-equivalent original commission). No general-knowledge fallback.
2. **System-prompt refusal rules** — refuses political questions, caste-based framings, justifications of violence, predictions of the future, medical/legal advice, sectarian comparisons.
3. **Citation-only outputs** — every response cites a specific Gita verse. Refuses if no citation is appropriate.
4. **Conservative tone prompt** — speaks *from* the text, never *as* the deity. "What the Gita teaches in this verse is..." not "Krishna says to you..."
5. **Scholar review of refusal rules + first 1,000 production responses** before broader rollout.
6. **About-screen safety statement** — publicly state how Smaran's AI is built differently than the failed Gita chatbots. Turn the risk into positioning.
**Why**:
- Failure mode is brand-killing. One viral screenshot of Smaran approving violence ends the product.
- The same architecture is also a marketing wedge — "the safe Gita AI" is a defensible category.
**Trade-offs accepted**: Slower build than a thin GPT wrapper. Higher scholar cost. Both are correct.
**Status**: Locked. Becomes Week 0 P0 task and a Gate 3 condition in `BUILD_PLAN.md`.

---

## D15. Transactional layer in V1 (sub + transactional + diaspora premium)
**Date**: 2026-05-16
**Context**: `RESEARCH_SYNTHESIS.md` finding: the category leader (Sri Mandir, ~$12M ARR) monetizes via commissions on temple services (e-puja, chadhava), not subscriptions. Take rate 20–25%. A sub-only Smaran fights category gravity; the $2–3k/month math works on a three-stream model.
**Decision**: Add a transactional layer to V1 alongside the subscription tiers from D2.
- **Light a virtual diya** for someone — ₹51 / $1.99
- **Sponsor a verse's audio recording in your name** — ₹101 / $3.99
- **Send a personalized deity-of-day blessing** to a friend's WhatsApp — ₹21 / $0.99
Final price points + acceptance tested in primary research (interview Q25 + survey F7); ship the highest-acceptance two transactions in V1, defer others.
**Why**:
- Matches proven category monetization pattern.
- Acts as the WhatsApp share-loop monetization endpoint (the existing viral mechanic now also carries a paid SKU).
- Repeat-engagement revenue (category norm: 80% of devotional revenue) without subscription friction.
**Trade-offs accepted**: Brand risk — transactional layer could read as "Sri Mandir lite" if executed without restraint. Mitigation: visual + copy restraint, no pop-ups, framed as "offering" not "purchase."
**Status**: Locked pending primary-research acceptance signal on the specific SKUs.

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

**Amended 2026-05-16**: Free-tier AI limit tightened to 1 Q/week per D18 (was 3/month). Diaspora-premium SKUs added per D19 ($19.99/yr · $49.99 lifetime).

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

**Amended 2026-05-16**: Diaspora *pricing tier* + diaspora-targeted Play Store listing now in V1 per D19. Marketing focus and creator outreach still India-first; this amendment is about SKU + store presence, not channel strategy.

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

## D14. 2-day MVP to internal testing track (full launch still 5 weeks)
**Date**: 2026-05-15
**Context**: Rev wanted to ship in 2 days, not 5 weeks. Discussed trade-offs honestly. Rev chose: ship MVP to Play Store **internal testing track** (invite-only, doesn't burn public listing) within 2 days; full public launch still follows original 5-week BUILD_PLAN.md.
**Decision**: Two-track approach.
- **Track A (2 days)**: Bare MVP — daily darshan + verse + share + streak + onboarding + notifications. No AI, no payments, no audio, no real art. Ships to internal testing.
- **Track B (5 weeks)**: Full public launch with art + AI + payments + audio per BUILD_PLAN.md.
**MVP scope locked in MVP_PLAN.md.** Do not expand.
**Why this is the right move**:
- Rev gets the dopamine of shipping fast
- Real device testing of notifications, streaks, share — all things that can break on real Androids
- Public Play Store listing is NOT burned with a half-product
- Tester feedback informs Week 1-5 decisions
- Internal track = no Google review wait; live within minutes of upload
**Risk**: testers see basic version. Mitigation: explicit "early access" framing in onboarding + home-screen note.
**Status**: Day 1 build COMPLETE (storage, notifications, share, onboarding, real streak). Day 2 tomorrow: EAS build + Play Store internal listing + first 10 testers.

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
