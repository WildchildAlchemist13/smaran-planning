# Smaran — 2-Day MVP Sprint Plan

> **Locked decision (D14)**: Ship MVP to Play Store **internal testing track** within 2 days.
> Public launch follows in 5 weeks per BUILD_PLAN.md with full features.

---

## Why this exists

Rev wants the dopamine of shipping fast. Internal testing track means **invite-only** — the public Play Store listing is NOT burned. Real device feedback, real notification testing, real share flows — all without compromising the eventual public launch.

---

## MVP Scope (locked, do not expand)

### What's IN

- ✅ Daily Darshan home screen (already built)
- ✅ Deity-of-day logic (already built)
- ✅ Sample Gita verses (already built — expand to 14 for variety)
- ➕ Beautiful placeholder deity cards (gradient + Sanskrit + ornamental SVG patterns; NO commissioned art yet, NO AI-generated faces)
- ➕ Real persistent streak counter (local storage)
- ➕ Save favorite verses (local storage)
- ➕ WhatsApp share blessing
- ➕ Daily 7am notification
- ➕ Simple onboarding (set name + ishta-devata)
- ➕ Privacy policy + Terms (template, hosted)
- ➕ Play Store internal testing listing (screenshots, icon, description)

### What's OUT (explicitly)

- ❌ Ask Krishna AI (Week 3)
- ❌ Razorpay / Premium tier (Week 3)
- ❌ Commissioned deity art (Week 1-3 once artists deliver)
- ❌ Sanskrit / English audio (Week 2-12 — mom records)
- ❌ Scholar commentary (full version Week 1-6)
- ❌ Phone OTP auth (use anonymous device ID for MVP)
- ❌ Cloud backend / Supabase (everything local for MVP)
- ❌ Journal
- ❌ Themes browse
- ❌ Festival packs
- ❌ Devotee tier
- ❌ Voice mode

These all come in v0.2+ after MVP validates daily ritual works on real devices.

---

## Day 1 — Build (10-12 focused hours)

### Morning (4-5 hrs)
- [ ] Install required Expo packages: `expo-notifications`, `expo-sqlite`, `expo-sharing`, `expo-secure-store`
- [ ] Build streak counter with persistence (open daily +1, miss day reset)
- [ ] Build save favorites (verse-id list in local storage)
- [ ] Build WhatsApp share — generate text + UTM link + share via expo-sharing

### Afternoon (4-5 hrs)
- [ ] Build daily 7am local notification (expo-notifications)
- [ ] Build onboarding flow (3 screens: welcome, name, ishta-devata pick)
- [ ] Add ornamental SVG patterns to deity cards (subtle gold accents, no faces)
- [ ] Expand sample verses from 7 to 14 for content variety

### Evening (2-3 hrs)
- [ ] End-to-end test on Rev's phone via Expo Go
- [ ] Fix any bugs from real-device testing
- [ ] Commit + push to GitHub

---

## Day 2 — Ship (8-10 focused hours)

### Morning (4-5 hrs)
- [ ] Install EAS CLI (`npm install -g eas-cli`)
- [ ] `eas login`
- [ ] `eas build:configure`
- [ ] Build production APK: `eas build --platform android --profile preview`
- [ ] Wait for build (~15-20 min)

### Afternoon (3-4 hrs)
- [ ] Generate Play Store screenshots (4-6 from Rev's phone)
- [ ] Generate app icon (1024x1024 — use Smaran wordmark on indigo with gold border)
- [ ] Write Play Store listing description (short + long, follow BRAND.md voice)
- [ ] Create privacy policy + terms (template, host on smaran.app/privacy)
- [ ] Upload APK to Play Console internal testing track
- [ ] Create internal tester list (10-25 emails: Rev, family, friends, future scholar/artist)

### Evening
- [ ] Send testers the install link
- [ ] Internal testers download via the Play Store invite link (live within minutes, no Google review needed for internal track!)

---

## Success Criteria for MVP

After 2 days, you should have:
1. **App live on Play Store internal testing** — invitees can install via Play Store
2. **10+ internal testers** with the app on their phones
3. **Daily notification firing** at 7am for at least 3 testers (verify Day 3-5)
4. **Real streak counter** working (Day 3 = streak of 3 for testers who opened daily)
5. **Real WhatsApp share** verified working

This validates: **does the daily ritual actually form a habit on real phones?**

If yes — proceed to 5-week public launch with confidence.
If no — diagnose what's not working before adding AI + payments.

---

## What we are NOT doing in 2 days (and why it's OK)

| Feature | Why deferred |
|---|---|
| AI Ask Krishna | Cost + safety + scholar review needed; 2 days insufficient |
| Razorpay payments | EAS dev build complexity; need premium content first to justify paywall |
| Commissioned art | Artists need 7-14 days; MVP validates without it |
| Mom's Sanskrit audio | Mic not ordered; recordings take weeks |
| Scholar commentary | 5+ days minimum lead time |
| Cloud backend | Local-only MVP is fine for 25 testers |
| Auth | Anonymous device ID is fine for MVP |

---

## Risk: testers see "basic" version and lose excitement

**Mitigation**: be transparent in onboarding. First-screen text:

> *"Smaran is in early access. The daily ritual works — the deity art and AI guidance are coming soon. Thank you for being here at the beginning."*

Set expectations. Internal testers are not consumers; they're early believers. They'll forgive incompleteness if they understand it.

---

## Day 3+ (after MVP ships)

Resume original BUILD_PLAN.md Week 1 activities **in parallel** with internal beta running:
- Artist outreach (continue)
- Scholar outreach (continue)
- Mom's recording setup (continue)
- v0.2 development (real art integration as it arrives, then audio, then AI, then payments)

Public launch target: **Week 5** of original plan, with internal beta data informing every decision.
