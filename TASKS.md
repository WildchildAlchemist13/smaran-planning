# Smaran — Active Tasks

> Live working list. Mark done with [x]. Add new as they emerge.
> Format: `- [ ] [PRIORITY] Task — owner — notes`

---

## 🔥 NEXT 2 DAYS — MVP Sprint to Internal Testing Track

> **D14 locked**: 2-day MVP, NOT public launch. Full public launch still 5 weeks per BUILD_PLAN.md.
> See **MVP_PLAN.md** for full scope.

### Day 2 (tomorrow) — Ship to Internal Testing
- [ ] [P0] Run app on phone via Expo Go to verify Day 1 build works (10 min)
- [ ] [P0] `npm install -g eas-cli` + `eas login` (5 min)
- [ ] [P0] `eas build:configure` then `eas build --platform android --profile preview` (15-20 min wait)
- [ ] [P0] Generate 4-6 Play Store screenshots from Rev's phone
- [ ] [P0] Create app icon (1024×1024 — Smaran wordmark on indigo, gold border)
- [ ] [P0] Write Play Store listing (short + long description, follow BRAND.md voice)
- [ ] [P0] Create + host privacy policy + terms (template, e.g., termly.io or PrivacyPolicies.com)
- [ ] [P0] Upload APK to Play Console internal testing track
- [ ] [P0] Add 10-25 tester emails to Play Console internal track
- [ ] [P0] Send testers the install link

---

## 🔧 Week 0 — Pre-Build (parallel content sourcing — continues)

> **Build-first strategy locked**: ship to Play Store before any creator outreach. App speaks for itself.

- [x] [P0] ~~Lock app name~~ → **Smaran (स्मरण)** locked 2026-05-14
- [x] [P0] ~~Open Razorpay merchant account~~ → existing team account, will reuse
- [x] [P0] ~~Open Google Play Console developer account~~ → already active
- [ ] [P0] Brief 3–5 Indian devotional artists for trial illustration — Rev — Behance, Instagram (@artists handles), Dribbble
- [ ] [P0] Find + brief Vedanta scholar / Gita teacher for sample commentary — Rev — start with Chinmaya Mission grads, Sanskrit dept teachers
- [ ] [P1] Decide deity-day calendar with traditional Hindu sourcing — Rev + scholar
- [ ] [P1] Lock philosophical lane (recommended: mainstream Vedanta) — Rev
- [ ] [P1] Open RevenueCat account, link Razorpay — Rev
- [ ] [P2] Check + register domain (smaran.com / .app / .in) + reserve handles (@smaran or backup) — Rev — see DECISIONS.md D10 for backup options
- [ ] [P1] Talk to mom — get her on board for Sanskrit recitations + agree on schedule — Rev
- [ ] [P1] Order recording mic on Amazon (Maono PD400X / Rode NT-USB Mini, ~₹10k) — Rev
- [ ] [P2] Schedule first recording session with mom (~5 days from now, after mic arrives) — Rev

---

## 🛑 Gate 0 Check (end of Week 0)

- [ ] App name + brand identity decided
- [ ] Artist locked + first 30 illustrations commissioned
- [ ] Scholar locked + first 30 commentaries commissioned
- [ ] Razorpay merchant account approved
- [ ] Play Console account active

**DO NOT START WEEK 1 BUILD until all of above are checked.**

---

## 📋 Backlog (visible, not active)

### Content production (parallel to build)
- [ ] [P0] First 30 deity illustrations delivered (artist)
- [ ] [P0] First 30 verse commentaries delivered (scholar)
- [ ] [P0] First 30 Sanskrit + English audio recordings delivered (reciter)
- [ ] [P1] Year-1 art roadmap: 365 illustrations over 6 months
- [ ] [P1] Year-1 commentary roadmap: 365 verses
- [ ] [P2] Festival pack v1 (Diwali) — special 10 illustrations + verses

### Build (Weeks 1–5)
- See `BUILD_PLAN.md` for full week-by-week breakdown

### Documentation to write
- [x] ~~`CONTENT_BRIEF.md`~~ ✅ Drafted 2026-05-14 — ready for Rev to send outreach
- [x] ~~`ASK_KRISHNA_PROMPT.md`~~ ✅ Drafted 2026-05-14 — needs scholar review before shipping
- [x] ~~`BRAND.md`~~ ✅ Drafted 2026-05-14 — full brand identity locked
- [ ] `RAZORPAY_SETUP.md` — config notes for subscription + lifetime IAP
- [ ] `POST_LAUNCH_OUTREACH.md` — creator partnership playbook (Bhavesh + 10 others), to be drafted after launch

---

## ✅ Done

- [x] Strategic concept locked (Daily Darshan + Gita + Ask Krishna premium)
- [x] Pricing tier structure decided (Free / Premium ₹399/yr / Devotee ₹999/yr)
- [x] Razorpay chosen as payment provider
- [x] AI model + cost structure locked (Claude Haiku 4.5 + caching)
- [x] Daily limits locked (Free: 3/mo, Premium: 5/day, Devotee: 20/day)
- [x] Project folder + planning files created
- [x] Build-first strategy locked (no creator outreach pre-launch)
- [x] **App name locked: Smaran (स्मरण)** — meaning "remembrance" from Gita 8.5, 8.14
- [x] Daily morning standup + Friday weekly review automated via scheduled-tasks
- [x] Stack pivot from Flutter to Expo (D13) — confirmed Expo Go is Rev's existing tooling
- [x] **Smaran-app/ Expo scaffold initialized** — git repo, brand colors, deity-of-day logic, sample verses, daily darshan home screen, Ask Krishna placeholder. Runnable v0.1 ready.
- [x] Both repos pushed to GitHub (smaran-app + smaran-planning, both private)
- [x] **D14 locked: 2-day MVP to internal testing track**, full public launch still 5 weeks
- [x] **MVP Day 1 BUILD COMPLETE** — storage layer, daily notifications, share, onboarding flow, real persistent streak counter, haptic feedback. All TypeScript clean.
