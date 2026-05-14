# Smaran — Play Store Listing Copy

> Ready to paste into Google Play Console tomorrow. Two versions: MVP (internal testing) and v1 (public launch later).

---

## App Title (max 30 characters)

**MVP / Internal**:
```
Smaran — Daily Darshan
```
*(22 chars)*

**Public v1 (later)**:
```
Smaran: Daily Gita Practice
```
*(27 chars)*

---

## Short Description (max 80 characters)

**MVP**:
```
A 30-second daily practice. One deity. One verse. One quiet bell at sunrise.
```
*(76 chars)*

**Alternates if first feels too long**:
- `Daily darshan + Bhagavad Gita verse. A 30-second morning practice.` *(67 chars)*
- `Wisdom of the Gita, in your pocket. A daily practice of remembrance.` *(68 chars)*

---

## Long Description (max 4000 characters)

```
स्मरण

Smaran (smah-RUN) is the Sanskrit word for "remembrance" — the practice Krishna teaches throughout the Bhagavad Gita: remember the divine in every moment.

Smaran is a daily practice. Every morning at sunrise, you receive one quiet bell. You open the app. The deity of the day greets you. You read one verse from the Bhagavad Gita. You sit with it for thirty seconds. You close the app.

That's it. That's the whole practice.

Over 30 days, the practice becomes a thread. Over 100 days, it becomes part of who you are.

—

WHAT'S INSIDE

· One deity each morning
The traditional Hindu vāra (weekday) deity — Sunday with Surya, Monday with Shiva, Tuesday with Hanuman, Wednesday with Ganesh, Thursday with Vishnu, Friday with Lakshmi, Saturday with Shani.

· One Bhagavad Gita verse, curated to the day
A real verse from the Gita, in Sanskrit, with English translation. Chosen to match the energy of today's deity.

· A streak that grows quietly
Open daily. Watch the days accumulate. Skip a day, the count returns to one. No shame, no notifications nagging — just a quiet record of your practice.

· Save the verses that meet you
A simple favorite. The verses that find you on hard days, you can return to.

· Send today's blessing
One tap to send today's verse and deity to family on WhatsApp. The way blessings have always been shared.

· One quiet bell, every morning
A single notification at 7 a.m. naming the day's deity. No other notifications. Ever.

—

WHAT IT IS NOT

· Not a content marketplace
· Not a horoscope app
· Not a chant streaming service
· Not a transactional puja booking platform

Smaran is a daily ritual. Nothing more, nothing less.

—

EARLY ACCESS

You are reading this because you've been invited to test Smaran in early access. The daily ritual works on real devices. The deity art (commissioned from Indian artists), the Sanskrit recordings (sung by my mother), the scholar-written commentary, and the Ask Krishna AI feature are all coming in the weeks ahead.

Thank you for being here at the beginning.

—

PRIVACY

Smaran stores your name, your chosen ishta-devata, your streak count, and your saved verses on your device only. Nothing is sent to any server. There are no ads. There is no tracking. Read our privacy policy: smaran.app/privacy

—

A note from the founder

Smaran is built in India, for Hindus everywhere — and for every seeker who knows that some questions deserve older answers. If something here moves you, please tell a friend.

🪔
```

*(~2,400 chars — well under 4000 limit)*

---

## What's New (changelog, max 500 characters per release)

**v0.1.0 — MVP / Internal Testing**:
```
First release. The daily ritual: one deity each morning, one verse from the Bhagavad Gita, a streak that grows quietly. Notification at 7 a.m. Share today's blessing on WhatsApp. Save the verses that meet you. Deity art, Sanskrit audio, and the Ask Krishna AI are coming soon.

Thank you for being here at the beginning. 🪔
```

---

## Keywords / Tags (for Google Play SEO)

Suggested primary keywords (Play Store searches these in title + short description):
- bhagavad gita
- daily darshan
- sanskrit
- hindu daily
- gita verses
- spiritual practice
- daily mantra
- hindu wisdom
- krishna teachings
- sanatana dharma

Suggested category: **Lifestyle** (better than "Books & Reference" for daily-ritual positioning)
Alt category: **Health & Fitness** (mind/spirit angle)

---

## Content Rating Questionnaire (anticipated answers)

When Play Console asks:
- Violence: None
- Sexual content: None
- Profanity: None
- Drugs/alcohol: None
- User-generated content: None (in MVP)
- User location: No
- Personal info collection: Minimal (name on device only)
- Religious content: Yes — Hindu spiritual content

Expected rating: **Everyone** / **3+**

---

## Required Play Store Assets (sizes to prep)

| Asset | Size | Status |
|---|---|---|
| App icon | 512×512 PNG | TO CREATE — Smaran wordmark on indigo + gold border |
| Feature graphic | 1024×500 PNG | TO CREATE — hero banner with brand |
| Screenshot 1 (home) | 1080×1920 PNG | Capture from Rev's phone |
| Screenshot 2 (deity card) | 1080×1920 PNG | Capture |
| Screenshot 3 (verse) | 1080×1920 PNG | Capture |
| Screenshot 4 (Ask Krishna preview) | 1080×1920 PNG | Capture |
| Screenshot 5 (onboarding name) | 1080×1920 PNG | Capture |
| Screenshot 6 (ishta picker) | 1080×1920 PNG | Capture |

**Tip for screenshots**: take them on a real Android phone (not emulator) so notification bar + status look natural. Use a Pixel-style phone if possible.

---

## Internal Testing Track Setup (Play Console)

1. Play Console → Smaran app → **Testing → Internal testing**
2. Create new release → upload APK from EAS Build
3. Add release notes (use "What's New" copy above)
4. **Testers** tab → Create email list → "Smaran Inner Circle" → paste emails
5. Save → Review → Publish to internal track
6. Get **opt-in URL** — share with testers via WhatsApp
7. Testers click link → opt in → install via Play Store

**Internal track has no Google review wait — live within minutes.**

---

## Bundle Identifier

Locked: `com.reeloom.smaran` (already in app.json)

---

## Version Numbering

Start at: `0.1.0` (1)
- Major.Minor.Patch (versionName)
- Integer code (versionCode) increments per upload

Convention going forward:
- `0.1.x` = MVP iterations (internal only)
- `0.2.x` = First feature additions (real art, scholar commentary)
- `0.3.x` = AI integration
- `0.4.x` = Razorpay + premium tier
- `1.0.0` = First public release
