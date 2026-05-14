# Mandir — Risk Register

> Live document. Add risks as they emerge. Mark resolved with [RESOLVED] + date.

---

## 🚨 Critical Risks (existential — could kill the venture)

### R1. Cultural / religious sensitivity backlash
- **Trigger**: Inaccurate translation, AI giving wrong/offensive answer, perceived commercialization of sacred content
- **Impact**: 1-star review brigades, social media backlash, brand-killing
- **Mitigation**:
  - Hire scholar advisor for full commentary review
  - Heavy AI guardrails (cite verses always, refuse non-Gita questions, conservative tone)
  - Manual review of first 1000 Ask Krishna responses
  - Donate % of revenue to a dharma org (also great PR)
  - Position as "stewards, not extractors" in all marketing
- **Status**: Open — primary mitigation is scholar hire in Week 0
- **Owner**: Rev

### R2. AI cost runaway
- **Trigger**: Power users binge Ask Krishna; one viral abuse moment
- **Impact**: AI cost exceeds 100% of revenue; venture becomes unprofitable
- **Mitigation**:
  - Hard daily limits (5/day Premium, 20/day Devotee)
  - Prompt caching (60–80% cost reduction)
  - Cost monitoring dashboard with alerts
  - Devotee tier upsell for power users
- **Status**: Mitigated by design — must be enforced at code level (Week 3)
- **Owner**: Rev

### R3. Cold launch — no creator partner Day 1
- **Trigger**: Build-first strategy means no Bhavesh / creator distribution partner at launch
- **Impact**: Slower initial growth; first 1000 users come through organic channels only
- **Mitigation**:
  - WhatsApp viral mechanic built into product (send blessing → recipient lands in app)
  - Reddit launch posts in r/hinduism, r/bhagavadgita, r/spirituality
  - Cysters Club community as warm bridge audience
  - ASO from Day 1 (target Hindi + English devotional keywords)
  - Quora answers + organic IG content from Rev's own accounts
  - **Post-launch creator outreach with proof in hand** = much stronger pitch than cold cold outreach
- **Status**: Mitigated by design — accept slower Phase 1 in exchange for stronger Phase 2 outreach
- **Owner**: Rev

---

## ⚠️ High Risks (could meaningfully hurt outcomes)

### R4. Sri Mandir adds Gita-mode and competes directly
- **Trigger**: Sri Mandir notices traction in 12–18 months
- **Impact**: Loses our category-defining position
- **Mitigation**:
  - Build cultural authenticity moat through art + scholarship that can't be copied quickly
  - Lock in users via journal data + saved favorites = high switching cost
  - Stay narrower and deeper than they can (they're a supermarket, we're a temple)
- **Status**: Long-horizon; not urgent

### R5. Older demographic struggles with sub payment
- **Trigger**: Devotional users skew older; sub fatigue is real
- **Impact**: Lower premium conversion
- **Mitigation**:
  - Push lifetime pricing hard (₹999 one-time vs ₹399/yr)
  - WhatsApp-based payment instructions
  - Family-share feature so younger family members can gift Premium
- **Status**: Open — design lifetime UX prominently

### R6. Art quality is mid → product feels generic
- **Trigger**: Artist budget compromise; AI-generated fallback
- **Impact**: Product loses its "wow" → no daily-open habit forms → no growth
- **Mitigation**:
  - Budget ₹3–5L for original art is non-negotiable
  - Sample test artists before commissioning full set
  - Curate from established Indian devotional artists, not generalists
  - **Never ship AI-generated faces of deities** (people will tell)
- **Status**: Open — Week 0 critical decision

### R7. Razorpay payment failures / KYC delays
- **Trigger**: Merchant account approval takes >5 days; webhook failures
- **Impact**: Can't ship on time
- **Mitigation**:
  - Start KYC Day 1 of Week 0
  - Stripe as backup for international users
  - Test webhooks thoroughly in Week 3 gate
- **Status**: Open — start KYC immediately

---

## ⚡ Medium Risks (worth tracking, manageable)

### R8. Anthropic raises API prices
- **Trigger**: Anthropic announcement
- **Impact**: AI cost margin shrinks
- **Mitigation**: Build model-agnostic LLM wrapper; can swap to Gemini Flash, Llama in 1 day
- **Status**: Open — wrapper to be built in Week 3

### R9. Rev burns out / loses interest
- **Trigger**: 5 weeks is intense; multiple ventures running in parallel (Reeloom, Cysters Club)
- **Impact**: Project stalls midway, can't ship
- **Mitigation**:
  - Be honest about why this project — soul + business, not just business
  - Realistic time budget (25 hrs/week, not 60)
  - Use agents heavily; protect Rev's decision-making time
- **Status**: Open — self-monitor

### R10. Translation copyright issues
- **Trigger**: Using Easwaran or Prabhupada translations without license
- **Impact**: Legal takedown, app removal
- **Mitigation**: Use public domain (Edwin Arnold 1885, Sacred Books of the East) OR commission fresh original translation from scholar
- **Status**: Open — Week 0 decision with scholar

### R11. Play Store rejection (religious content category)
- **Trigger**: Google flags for "sensitive content" or unclear monetization
- **Impact**: Launch delayed by 1–2 weeks
- **Mitigation**:
  - Clear app description as "spiritual / wellness / philosophy"
  - Comply with all subscription disclosure rules
  - Have marketing collateral ready showing legitimacy (scholar advisor, etc.)
- **Status**: Open — Week 5 risk

---

## 🟢 Low Risks (monitor only)

- **R12.** WhatsApp share API changes break the viral mechanic — fallback to plain link share
- **R13.** Notification deliverability issues on Xiaomi/Vivo — known Android quirk; use FCM properly + in-app fallback
- **R14.** Supabase free-tier limits hit early — easy upgrade path
- **R15.** Audio CDN cost spike — switch from Supabase Storage to Bunny CDN if needed

---

## Resolved

_(Empty — none resolved yet)_
