# Smaran — Screen Design Blueprint

> Design thinking for every screen of Smaran. This document is the bridge between BRAND.md (visual identity) and the actual app code. Every screen built (or designed) should reference back to this.
>
> Authored: 2026-05-15. Living document — update as we learn.

---

## 0. Design Principles (the rules that override everything)

These five principles override every other consideration. If a design choice violates one of these, it's wrong even if it looks pretty.

### 1. Restraint as a statement
One focal element per screen. ONE. Generous negative space is part of the design. The empty indigo space says as much as the gold accent does. Calm app does this perfectly — every screen has 3-4 elements, never more.

### 2. Reverence over efficiency
This is a sacred-feeling product. Slowness is intentional. A 0.4s transition is more reverent than a 0.2s transition. A pause before showing content is more devotional than instant load. Hallow leans into this hard.

### 3. Calm over engagement
We do NOT pursue engagement metrics that violate the spirit. No "Don't break your streak!" guilt notifications. No "You haven't opened the app today!" nags. No popups, no badges, no red dots. The user comes to us; we don't chase them.

### 4. Typography as the primary visual language
Smaran's design is text-first. The Bhagavad Gita is a TEXT. Sanskrit is a sacred SCRIPT. Beautiful typography (serif for English, quality Devanagari for Sanskrit) is more devotional than imagery. When in doubt, use type, not graphics.

### 5. Motion that breathes
All animations follow a "breath" curve — slow in, slower out. No bouncy springs, no harsh snaps. Easing should feel like an exhale. Default duration: 400-600ms. Reference: how Apple animates Notes app, how Headspace fades between screens.

---

## 1. The User Journey (jobs-to-be-done)

| Moment | What user wants | How Smaran serves it |
|---|---|---|
| 6:30am notification fires | "What is today bringing me?" | One quiet bell, deity name only |
| User opens app | "A small moment of peace before the day" | Full-screen deity card, calming colors |
| Reads the verse | "Wisdom for today" | One line, beautifully set |
| Wants to know more | "What does this verse mean?" | Tap → full commentary screen |
| Wants to save it | "This one I want to remember" | One tap, no friction, persistent |
| Wants to share | "My mom would love this" | One tap → WhatsApp with framed message |
| Comes back days later | "Where was that verse about anxiety?" | Saved screen, themed organization |
| Has a real life question | "What does the Gita say about this?" | Ask Krishna (v0.3+) — quiet, considered |

---

## 2. Screen Inventory

### MVP / v0.1 (must ship before launch)

| # | Screen | Job |
|---|---|---|
| 1 | Splash | Brand moment as app loads |
| 2 | Onboarding — Welcome | Brand introduction |
| 3 | Onboarding — Name | Personal investment |
| 4 | Onboarding — Ishta-devata | Choose connection |
| 5 | Onboarding — Notifications | Permission with intention |
| 6 | Home / Daily Darshan | The morning ritual itself |
| 7 | Verse Detail | Read the full verse + transliteration + future commentary |
| 8 | Saved Verses | The personal collection |
| 9 | Settings | Adjust without friction |
| 10 | About / Credits | Founder voice, scholar credit, mom credit |

### v0.2+ (post-launch, second sprint)

| # | Screen | Job |
|---|---|---|
| 11 | Ask Krishna entry | Doorway to the killer feature |
| 12 | Ask Krishna conversation | The AI chat interface |
| 13 | Themes browse | Find verses by life situation |
| 14 | Premium paywall | The "why pay" moment |
| 15 | Streak history | Calendar / quiet record of practice |
| 16 | Festival pack | Seasonal exclusive content |

### v1.0+ (later)

| # | Screen | Job |
|---|---|---|
| 17 | Journal | Write reflection on a verse |
| 18 | Personal Mandir | The deepest engaged user's home |
| 19 | Audio download manager | Offline practice |
| 20 | Devotee tier features | Voice mode, advanced AI |

---

## 3. Per-Screen Design Vision

### Screen 1 — Splash

**Job**: First impression. The 1-2 seconds while app loads must feel reverent, not corporate.

**Design**:
- Full-screen indigo `#1A1B3A` background
- Center: the Smaran gold brushstroke logo, no text
- Below logo, after 0.4s fade-in: **"स्मरण"** in small Devanagari, gold, very subtle
- No tagline, no loading spinner, no progress bar
- Total visible time: ~1.5s before transitioning to home

**What it does NOT have**:
- Loading bar (creates anxiety)
- Tagline (cluttered)
- Skip button (insulting)
- Animated logo (showing off)

**Reference**: Hallow's splash. Calm's splash. Quiet, brand-only.

---

### Screen 2 — Onboarding: Welcome

**Job**: Make the user think "this is different" within 3 seconds.

**Design**:
- Full-screen indigo
- Top center, after a quiet 0.3s: large gold Devanagari "स्मरण"
- Below it: "SMARAN" in serif, all caps, tracked wide
- Center body (after slight delay): single italic line in ivory:
  > *"Daily darshan. Bhagavad Gita. A 30-second practice of remembrance."*
- Below: 3 small bullet points in soft gold
  - · One deity each morning
  - · One verse from the Gita
  - · A streak that grows quietly
- Bottom: a single gold button **"Begin"** — large, generous padding, simple
- NO skip option (users complete welcome — it's 3 seconds)

**Animation**: Each element fades in with a 0.3s offset (Devanagari → Latin → tagline → bullets → button). Total: 1.2s to fully assemble. Feels like the screen is taking a breath as it appears.

---

### Screen 3 — Onboarding: Name

**Job**: Capture name without it feeling like a form.

**Design**:
- Full-screen indigo
- Top: small "· Step 1 of 3 ·" in gold, all caps, tracked wide
- Center: the question in serif, large:
  > *"What may we call you?"*
- Below the question: subtle ivory subtext:
  > *"Your name will appear in your daily greeting. Used only on this device."*
- Center input field: minimal, no box border, just a thin gold underline; ivory text; placeholder "Your name" in soft gray
- Below: gold "Continue" button (disabled until name typed)
- Below that: small "Skip" link in soft gray
- NO progress bar (the "Step 1 of 3" text IS the progress indicator)

**Why this works**: The question is asked *softly* ("What may we call you?" not "Enter your name"). The input has no box — it's literally just a line with text. The whole screen feels like a gentle inquiry, not data collection.

---

### Screen 4 — Onboarding: Ishta-devata

**Job**: User picks the deity they feel closest to. This personalizes their experience without demographics.

**Design**:
- Full-screen indigo
- Top: "· Step 2 of 3 ·"
- Center heading: *"Choose your ishta-devata"*
- Subtext: *"The form of the divine you feel closest to. (You can change this anytime.)"*
- Grid (3 columns × 3 rows = 9 deities, but we have 7, so 3+3+1 layout):
  - Each deity card:
    - Small Devanagari name (e.g., "शिव")
    - Below: English name in serif (e.g., "Shiva")
    - Card: subtle indigo background with thin gold border (1px)
    - Selected state: gold border 2px + soft gold background tint
- Bottom: "Continue" button (works even if nothing picked — defaults handled later)
- Skip link below

**Why grid not list**: Visual recognition is faster than reading. Users feel the deity they're drawn to.

**Possible future enhancement**: A small ornamental motif unique to each deity inside its card (a trishul for Shiva, a peacock feather for Krishna, etc.) — but these are AI-generation hard, so v2.

---

### Screen 5 — Onboarding: Notifications

**Job**: Get notification permission without feeling like every other app's spam request.

**Design**:
- Full-screen indigo
- Top: "· Step 3 of 3 ·"
- Center heading: *"One quiet bell at sunrise"*
- Subtext (longer, intentional):
  > *"Smaran sends a single, calm notification at 7 a.m. each morning — naming the day's deity and inviting you to your practice. No other notifications. Ever."*
- The promise of "no other notifications, ever" is the entire pitch
- Bottom: gold "Enable morning bell" button
- Below: "Not now" link

**Why this works**: Most apps say "Allow notifications to stay updated!" — wishy-washy. We say exactly what notifications we'll send (one, at 7am, with this content) and exactly what we won't (nothing else). Trust is built in this moment.

---

### Screen 6 — Home / Daily Darshan (THE most important screen)

**Job**: The 30-second morning ritual. This is the entire product. If this screen doesn't feel right, nothing else matters.

**Layout (top → bottom)**:

```
┌─────────────────────────────┐
│ SMARAN              · 12 ·  │  ← top bar: wordmark + streak (quiet)
│                             │
│       [DEITY CARD]          │  ← 3:4 aspect, deity-themed color
│      देवनागरी               │     deity name in Sanskrit, large gold
│      [art space]            │     deity art (placeholder for now,
│                             │     real art Week 1+)
│                             │
│ Today is Tuesday            │  ← greeting block
│ Hanuman ji's day            │     deity name in serif
│       · COURAGE ·           │     energy theme in small caps gold
│                             │
│ BHAGAVAD GITA 2.47          │  ← verse reference (small, gold, caps)
│ "You have a right to        │     verse one-line preview
│  action — never to its      │     italic serif, ivory
│  fruits."                   │
│                             │
│   ♡    ↗    Read full →     │  ← actions row
│                             │
│ early access note in soft   │  ← MVP transparency note
│ gray, for v0.1 only         │
└─────────────────────────────┘
```

**Why this layout works**:
- **Above fold (top 60%)**: deity card commands attention — this is the "darshan" moment
- **Below fold (bottom 40%)**: verse + actions — read it if you want, share if you want
- Streak counter is QUIET (top right corner, gold pill) — present but not nagging
- No bottom nav bar visible (current Expo template has tabs — should hide on home OR move to a profile button top-left)

**Critical animation**: When user opens the app, the deity card should fade in slightly LATER than the rest of the page. Like the deity is "arriving." 0.6s delay, 0.5s fade. Reverent.

**Decision needed**: Drop the tabs? Move to a single-screen-with-modal architecture instead? Tabs feel app-y; one main canvas feels meditative. **My recommendation: drop bottom tabs, use top-left profile icon for settings/saved access.**

---

### Screen 7 — Verse Detail (tap "Read full")

**Job**: For users who want to go deeper. The premium tier eventually lives here (commentary, audio).

**Design**:
- Full-screen indigo, scrollable
- Top: a back arrow (gold, top-left), no title
- Body (centered, max-width):
  - Verse reference small caps (e.g., "BHAGAVAD GITA 2.47")
  - Sanskrit in Devanagari, large, gold, generous line height
  - Below: Roman transliteration, italic, small, ivory
  - Below: English translation, serif, ivory, larger line height
  - Thin gold horizontal divider line
  - Commentary heading: "Reflection" in small caps gold
  - Commentary body in ivory serif, generous spacing (premium gated for v0.2+)
  - Below commentary: thin gold divider
  - "Reflection prompt" — the question scholar wrote
  - Below: a journal text area (premium gated)
- Bottom (sticky): audio play button (premium) + share + save

**Free vs Premium gating** (v0.2+):
- Free: sees Sanskrit + transliteration + 1-line translation
- Premium: full translation + commentary + audio + journal
- Paywall placement: when free user taps "Read more →" beneath the 1-line translation, gentle paywall appears

---

### Screen 8 — Saved Verses

**Job**: User's personal collection of verses that meant something.

**Design**:
- Full-screen indigo
- Top: small back arrow + "Your saved verses" in soft caps
- Body: list of saved verses, each as a card:
  - Verse reference (small caps gold)
  - One-line preview (italic serif ivory)
  - Date saved (small soft gray, right-aligned)
- Empty state (when no saves yet):
  - Center: thin gold ornamental dot
  - Below: *"The verses that meet you, you can save here."*
  - Soft, not pushy

**Detail**: Tapping a saved card → opens that verse's Verse Detail screen.

---

### Screen 9 — Settings

**Job**: Quick adjustments. Not a feature dump.

**Design** (a single scroll, no sections initially):
- Top: back arrow + "Settings" small caps
- Items (each row: label left, value or arrow right, thin gold divider):
  1. Your name → tap to edit
  2. Ishta-devata → tap to change (opens picker grid)
  3. Morning bell → toggle on/off
  4. Bell time → tap to change (default 7:00 AM)
  5. — divider —
  6. About Smaran → opens About screen
  7. Privacy policy → opens external link
  8. Terms → opens external link
  9. Send feedback → opens email composer
  10. — divider —
  11. App version → small text, no action

NO logout (no accounts in MVP). NO premium upsell here (lives on Verse Detail). NO theme/dark mode toggle (we're locked in the brand colors).

---

### Screen 10 — About / Credits

**Job**: Tell the story. Build trust. Acknowledge contributors.

**Design**:
- Full-screen indigo, scrollable
- Top: back arrow
- Hero section:
  - Smaran logo (small, centered)
  - *"A daily practice of remembrance."*
- Body (long-form, beautifully typeset):
  - **About Smaran** — 2 paragraphs from the founder voice (you, in your words)
  - **Sanskrit recitations** — credit to mom
  - **Scholar advisor** — name + tradition (when locked in)
  - **Original art** — list of artists when commissioned
  - **Built in** — Bengaluru / wherever
  - **Open source acknowledgments** — Expo, React Native, Supabase, Claude
- Bottom:
  - Version number
  - "Made with reverence in India 🪔"

This screen is where the brand story lives. Tap-rate is low (5-10% of users) but the ones who tap become evangelists.

---

## 4. Information Architecture

### Navigation model decision: NO bottom tabs

The default Expo template uses tabs. For Smaran, this is wrong. Tabs make the app feel like a tool with multiple "destinations." Smaran has ONE destination — the daily ritual. Everything else is supporting.

**Proposed model**:
- **Home is the only "place"** — Daily Darshan
- **Top-left**: small profile icon → opens drawer/menu with: Saved verses, Settings, About
- **Top-right**: streak badge (display only, no navigation)
- **Body action buttons**: ♡ save, ↗ share, **Read full →** (opens Verse Detail as a stack)

This is more like Calm's architecture than Sri Mandir's. One canvas, not five.

### Stack vs modal hierarchy

```
Root (Splash → Onboarding flow → Home)
└── Home (Daily Darshan)
    ├── Verse Detail (push)
    ├── Drawer (slide-in from left)
    │   ├── Saved Verses (push)
    │   ├── Settings (push)
    │   │   └── Edit Name, Change Deity, Bell Time (modals)
    │   └── About (push)
    └── Ask Krishna (push, v0.2+)
```

---

## 5. Design Tokens (locked, ready to embed in code)

```ts
// /constants/design-tokens.ts (to create after agreeing this doc)

export const Spacing = {
  xs: 4,   // tightest gaps
  sm: 8,
  md: 16,
  lg: 24,  // standard screen padding
  xl: 32,
  xxl: 48, // generous breathing room
  xxxl: 64,
};

export const Radius = {
  none: 0,
  sm: 4,
  md: 8,    // standard cards
  lg: 12,   // deity card
  xl: 16,
  pill: 999,
};

export const TypeScale = {
  // Display (large, serif)
  display1: { size: 56, lineHeight: 64, weight: '300', font: 'serif' }, // Devanagari hero
  display2: { size: 32, lineHeight: 40, weight: '500', font: 'serif' }, // Smaran wordmark
  // Headings
  h1: { size: 26, lineHeight: 34, weight: '500' },
  h2: { size: 22, lineHeight: 30, weight: '500' },
  h3: { size: 18, lineHeight: 26, weight: '500' },
  // Body
  body: { size: 16, lineHeight: 26, weight: '400' },
  bodySmall: { size: 14, lineHeight: 22, weight: '400' },
  // Special
  caption: { size: 12, lineHeight: 18, weight: '400', letterSpacing: 1 },
  caps: { size: 11, lineHeight: 16, weight: '500', letterSpacing: 4, transform: 'uppercase' },
  verse: { size: 18, lineHeight: 28, weight: '300', style: 'italic' }, // verse text
};

export const Motion = {
  // Easing
  ease: 'cubic-bezier(0.4, 0, 0.2, 1)', // standard "breath"
  easeOut: 'cubic-bezier(0, 0, 0.2, 1)',
  // Duration
  fast: 200,    // micro-interactions only
  base: 400,    // most transitions
  slow: 600,    // deity card arrival, verse fade
  reverent: 1000, // splash screen, sacred moments
};

export const Shadow = {
  // Subtle only — premium products avoid harsh drop shadows
  card: {
    shadowColor: '#000000',
    shadowOpacity: 0.15,
    shadowRadius: 12,
    shadowOffset: { width: 0, height: 4 },
    elevation: 4,
  },
};
```

---

## 6. Edge Cases & States

Every screen has at least 4 states. Design specs for each:

| State | Treatment |
|---|---|
| **Loading** | Subtle, non-anxiety-inducing. NO spinning circle. Use a single 2-3px gold dot pulsing slowly. |
| **Empty** | Calm copy + tiny ornamental motif. Never feels like an error. |
| **Error** | Quiet ivory text. NO red. Suggest one action. Example: "The morning bell could not be set. Try again, or open settings." |
| **Offline** | App works fully offline (local data). Only show indicator if user tries to do something needing network (none in MVP). |
| **Slow connection** | Same as offline. Local-first means most things just work. |

---

## 7. Accessibility (do not skip)

- All text must meet WCAG AA contrast (gold #C9A961 on indigo #1A1B3A passes; verify with tool)
- Minimum tap target: 44×44 pt
- All interactive elements must have accessibilityLabel
- Support dynamic type (font size changes)
- Sanskrit transliteration should have proper diacritic rendering (test on real Android)
- Screen reader: read order should be: greeting → deity → verse → actions

---

## 8. Motion Language

Specific animations to nail:

| Animation | Where | Duration | Curve |
|---|---|---|---|
| Splash logo fade-in | Splash | 600ms | ease |
| Splash → Onboarding | First open | 400ms cross-fade | ease |
| Onboarding step transitions | Onboarding | 400ms slide right | ease |
| Deity card arrival on home | Home open | 800ms fade + 4px slide up | ease |
| Verse fade-in | Home | 600ms after deity | ease |
| Save button heart fill | Tap save | 200ms scale + color | spring (gentle) |
| Streak +1 | First open of day | 400ms scale pulse on streak badge | spring |
| Verse Detail push | Tap "Read full" | 400ms slide from right | ease |
| Drawer slide | Tap profile icon | 400ms slide from left | ease |

**Never use**:
- Bouncy springs
- Hard snaps
- Confetti
- Any animation < 200ms (feels jittery)
- Loading spinners

---

## 9. What's NOT designed for v0.1 (deliberate cuts)

- No themed dark/light mode (we're already on a dark devotional palette by default)
- No customizable bell time inside MVP (default 7am, edit in v0.2)
- No swipe-between-deities (deity is fixed by day)
- No "yesterday's verse" navigation (the practice is daily; don't break it)
- No share-with-recipient-name (basic share only in MVP, personalized share in v0.2)
- No widget (post-launch)
- No watch face complications (later)

---

## 10. Reference Inspiration Board

When in doubt, consult these:

| App | What to learn |
|---|---|
| **Hallow** | Splash + onboarding + religious daily ritual UX |
| **Calm** | Restraint, motion, dark premium aesthetic |
| **Stoic** | Daily quote + journal flow, single-screen architecture |
| **Headspace** | Color discipline, illustrative restraint |
| **Apple Notes** | Settings-as-page (no tab bar), single canvas |
| **Aesop site** | Typography as the entire visual identity |
| **Le Labo** | Apothecary serif + restraint |
| **The Browser Company / Arc** | Premium dark theme + soft motion |

NOT to learn from:
- Sri Mandir (too transactional)
- AstroSage / AstroTalk (too busy)
- Any "Hindu wallpaper" app
- Generic meditation apps with stock yoga photos

---

## 11. Next Steps (after this doc is approved)

In order:

1. **Lock the navigation decision** — drop tabs, go single-canvas with drawer (recommended)
2. **Build design tokens file** in code — `/constants/design-tokens.ts` per Section 5
3. **Generate Canva mockups** for the 6 most important screens (Splash, Welcome, Home, Verse Detail, Saved, Settings) — visual sign-off before code
4. **Refactor app screens** to match the new design (currently the home screen is good but onboarding could be tightened)
5. **Add motion** per Section 8
6. **Build remaining MVP screens**: Verse Detail (full), Saved, Settings, About
7. **Take screenshots** AFTER above is done
8. **Then** Play Store listing
