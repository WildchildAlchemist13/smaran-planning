# Smaran — Design Brief for Rev

> Use this when you're sitting down to design the screens. Everything you need in one place.
> When you're done, drop screens into a folder and I'll build to match exactly.

---

## 1. Frame size (use this for every screen)

**Frame: 393 × 852 pt** (iPhone 14/15 baseline — also works as Android baseline)
- Safe area top: ~59pt (status bar + notch buffer)
- Safe area bottom: ~34pt (home indicator)
- Usable canvas: ~393 × 759 pt

For Android-only checks: design at 393×852, then verify it doesn't break at 360×640 (older Samsung J-series, ~3% of India market).

**Export each screen at 2x as PNG** — gives me 786×1704 retina-quality reference.

---

## 2. Brand reference (one-page cheat sheet)

### Colors (HEX)
| Name | Code | Use |
|---|---|---|
| Smaran Indigo | `#1A1B3A` | Background — every screen, no exceptions |
| Sacred Gold | `#C9A961` | Accent — sparingly. Wordmark, key CTA, Sanskrit, premium markers. **Cap at 2 elements per screen.** |
| Ivory | `#F5EFE3` | Body text on indigo. Replaces white. |
| Sindoor Red | `#A62024` | Devotional accent only — saved heart fill, festival markers, streak milestone. Never errors. |
| Sage Green | `#6B8E5C` | Tertiary — reserved for Devotee tier (premium upsell), reflection prompts. |
| Charcoal | `#2C2C2C` | Body text on ivory backgrounds (rare in this app). |
| Soft Gray | `#9A9A9A` | Secondary text, dividers (use sparingly — borderline a11y contrast). |

### Fonts
| Name | Where to find | Use |
|---|---|---|
| **Cormorant Garamond** | Google Fonts | Display / headlines / verses (English). Weights: 300 Light, 400 Regular, 400 Italic, 500 Medium |
| **Inter** | Google Fonts | Body / UI labels / captions (English). Weights: 400, 500, 600 |
| **Tiro Devanagari Sanskrit** | Google Fonts | Sanskrit verses + sacred names. Use for स्मरण wordmark + verse Sanskrit. |
| **Mukta** | Google Fonts | Hindi body text (when we localize). Optional for now. |

### Voice / tone reference
Reverent, calm, specific, warm. Never preachy or sales-y.
- Use Sanskrit-rooted words: *darshan, sankalp, smaran*. Avoid Christian-flavored terms ("blessing", "prayer", "faith").
- No exclamation points. No emojis except occasional 🪔 in marketing.
- Italicize Sanskrit transliteration: *karmaṇy-evādhikāras te*.
- See [BRAND.md](BRAND.md) Section 7 for full voice rules + examples.

### The single visual principle
**Restraint as reverence.**
- Generous space around any focal element (≥20% padding from frame edges)
- ONE focal element per screen, supported quietly by typography
- If gold is on more than 2 elements per screen, you're using too much

---

## 3. The complete screen list (12 screens to design)

In approximate priority order. P0 = launch-blocker, P1 = ship in v0.1 if possible, P2 = v0.2.

### P0 — must design before v0.1 launch

1. **Splash screen** — what users see for ~1 second on app open while fonts load. Minimal: logo on indigo.
2. **Onboarding 1: Welcome** — स्मरण wordmark, 3-line tagline, "Begin" button.
3. **Onboarding 2: Name** — single input (gold underline only, no box), "Continue" button.
4. **Onboarding 3: Ishta-devata** — 7 deity chips in a 3-column grid, pick one, "Continue".
5. **Onboarding 4: Notifications** — explainer copy + "Enable morning bell" + "Not now" link.
6. **Home / Daily Darshan** — THE main canvas. See detailed brief below.
7. **Verse Detail** — full verse, Sanskrit + transliteration + translation + commentary, save + share + back.
8. **Menu Sheet** — slide-up overlay from bottom: Ask Krishna, Saved verses, Settings, About.

### P1 — design if you have time before v0.1

9. **Saved Verses** — list of saved verses (or empty state).
10. **Settings** — list of options (notifications time, version, links to Privacy/Terms, About, etc.).
11. **About Smaran** — brand story + acknowledgments (mom, scholar credits later).
12. **Ask Krishna placeholder** — "Coming soon" page that sets the future-feature expectation. Real chat UI is v0.3.

### P2 — design later (after v0.1 ships)

- Privacy policy (in-app reader)
- Terms of service (in-app reader)
- Curator (deity image manager) — internal tool, doesn't need polish
- Verse audio player (when audio is added)
- Premium paywall (when Razorpay integrated)
- Festival pack screens

---

## 4. Detailed brief for the most important screen — Home / Daily Darshan

This is the screen users see every morning. Get this right, everything else follows.

### Composition (top to bottom)
1. **Top bar** (44pt height)
   - Left: Hamburger menu icon (Menu icon, 20pt, ivory @ 70% opacity)
   - Center: "SMARAN" wordmark (Inter 13pt, letter-spacing 6, ivory)
   - Right: Streak badge (rounded-pill, gold @ 12% bg, gold @ 30% border, "12" in gold Inter 14pt SemiBold)

2. **Spacing**: 24pt margin from top bar to deity card

3. **Deity card** (3:4 aspect ratio, full width minus 24pt side margins)
   - Background: full-bleed deity image (Ravi Varma painting), corner radius 12pt
   - Subtle indigo wash overlay (~18% opacity) so Sanskrit name reads
   - Sanskrit name (top center, Tiro Devanagari Sanskrit 28pt, gold, with subtle text-shadow for legibility)
   - **NO gold border** — the image carries itself

4. **Spacing**: 32pt margin to greeting block

5. **Greeting block** (centered)
   - Line 1: "Rev · Today is Tuesday" (Inter 13pt, ivory @ 65%, letter-spacing 1)
   - Line 2: "Hanuman ji's day" (Cormorant Garamond 26pt, ivory)
   - Line 3: "· COURAGE ·" (Inter 12pt, gold, letter-spacing 4, uppercase)

6. **Spacing**: 32pt to verse block

7. **Verse block** (centered, with 8pt horizontal padding inside)
   - Line 1: "BHAGAVAD GITA 2.47" (Inter 11pt, ivory @ 65%, letter-spacing 2, uppercase)
   - Line 2: "You have a right to action — never to its fruits." (Cormorant Garamond Italic 19pt, line-height 30pt, ivory)

8. **Spacing**: 32pt to action row

9. **Action row** (centered, 12pt gap between buttons)
   - Save button: 44×44pt circle, ivory@6% bg, Heart icon ivory (filled sindoor when saved)
   - Share button: 44×44pt circle, ivory@6% bg, Share2 icon ivory
   - Read full button: auto-width pill, gold bg, "Read full" Inter SemiBold 14pt indigo + ChevronRight icon
   - **NO gold borders on circle buttons** (already removed in code)

10. **Spacing**: 48pt (xxl) to Ask Krishna invitation

11. **Ask Krishna invitation** (centered, italic gold text link with arrow)
    - "Bring a question to Krishna →" (Cormorant Garamond Italic 15pt, gold @ 90% opacity)
    - This is intentionally subtle — a quiet invitation, not a CTA

### What you can change in your design
- Image aspect (try 4:5 instead of 3:4?)
- Whether the wordmark stays at top or moves
- The action button shapes (circles vs squares vs no shape)
- Where the Ask Krishna invitation lives
- Whether you want the menu hamburger or some other entry pattern

### What's locked (don't change without discussion)
- Color palette (indigo + gold + ivory)
- Top-down hierarchy: top bar → deity → greeting → verse → actions → Ask Krishna
- The Sanskrit name overlaid on the deity image
- The pill-shape "Read full" button as the primary action

---

## 5. Component vocabulary (reusable across screens)

When you design, use these consistent pieces. I have them in code; matching them keeps the build fast.

| Component | Spec |
|---|---|
| **Top bar** | 44pt height, hamburger left + wordmark center + streak/back-arrow right |
| **Back arrow** | ChevronLeft icon, 24pt, gold |
| **Primary button** | Pill (auto-width), gold bg, 16pt vertical padding, 24pt horizontal, indigo text Inter SemiBold 16pt |
| **Secondary action button** | 44×44pt circle, ivory@6% bg, line icon ivory 18-20pt |
| **Subtle text link** | Cormorant Italic 15pt, gold @ 90%, underlined or with chevron |
| **Streak badge** | Rounded pill, gold@12% bg, gold@30% border, gold number Inter SemiBold 14pt |
| **Card (deity, etc.)** | 12pt corner radius, full-bleed image, optional subtle indigo wash overlay |
| **Menu sheet** | Slide-up modal, indigo bg, 20pt corner radius top, gold@20% top border, 4pt gold@20% drag handle |
| **Empty state** | Centered: small ornament/icon + heading (Cormorant 22pt) + 1-line italic gold subtext |
| **Loading state** | Centered: "·" gold dot pulsing (the Bindu loader). Replaces every spinner. |

---

## 6. What to design for each screen (1-line acceptance criteria)

| Screen | Acceptance criteria |
|---|---|
| Splash | स्मरण in gold on indigo. Nothing else. |
| Onboarding 1 (Welcome) | स्मरण wordmark + tagline + "Begin" button. Quiet welcome. |
| Onboarding 2 (Name) | "What may we call you?" + gold underline input (NO box) + "Continue" + "Skip for now" |
| Onboarding 3 (Ishta) | "Choose your ishta-devata" + 7 deity chips (3 cols, square, with Sanskrit + English name) + "Continue" |
| Onboarding 4 (Notifications) | "One quiet bell at sunrise" + explanatory copy + "Enable morning bell" + "Not now" |
| Home (Daily Darshan) | Per detailed brief above |
| Verse Detail | Hero verse Sanskrit + transliteration + translation + commentary + audio player (v0.2) + Save + Share + Back |
| Menu Sheet | Slide-up, 4 menu items: ✦ Ask Krishna (prominent gold) / Saved verses / Settings / About Smaran |
| Saved Verses | List of saved verse cards OR empty state ("Verses you save will rest here.") |
| Settings | Section list: Profile, Practice (notif time, deity), About, Privacy, Terms, Send feedback |
| About Smaran | Brand story (3-4 paragraphs), credits (mom, scholar), version |
| Ask Krishna placeholder | स्मरण mark + "Ask Krishna" + "· Coming soon ·" + the future-promise copy + sample exchange preview |

---

## 7. Where to design

Pick what you'll actually use:

### Option A — Figma (best for full app design)
- Free for solo use
- Frame size: iPhone 14 (393×852) preset
- Auto-layout for components
- Export PNG @2x
- Pro: Easy to iterate, share, version
- Con: Learning curve if new

### Option B — Canva (faster, simpler)
- You already have it connected to me
- "Mobile App" template at 1080×1920
- Drag/drop visual building
- Export PNG
- Pro: Familiar, fast
- Con: Less precise, harder to keep consistency across 12 screens

### Option C — Hand sketch + photo (fastest for solo founder)
- Pen + paper, sketch each screen
- Photograph and send to me
- I interpret and build
- Pro: Lowest friction, fastest iteration
- Con: I might guess wrong on small details

### Option D — I scaffold initial mockups in Canva, you refine
- I generate first-pass screen mockups via Canva MCP using this brief
- You take them, refine in Canva or Figma, share back
- Pro: You start from a baseline, not a blank page
- Con: My initial generations may be off-brand (we saw this with the logo iterations)

**My recommendation: Option C (hand sketch) for fastest iteration, OR Option A (Figma) if you want to build a real design system.**

---

## 8. How to share screens with me

Once you have screens designed:

1. Save each as a PNG at 2x (786×1704 or 1080×1920)
2. Name them: `01-splash.png`, `02-onboarding-welcome.png`, etc.
3. Drop them in: `/Users/revanthsharma/Claude Code Desktop/Smaran/screens-mockups/`
4. Tell me "screens are ready"
5. I'll review each, ask any clarifying questions, then build to match

Or paste them directly into our chat — I can see them inline.

---

## 9. What stays locked while you design

Code is at a working save point:
- All audits done (5 reports in `Smaran/`)
- Wave A code fixes shipped (Lucide icons, fonts, voice, error boundary, reliability)
- Build is on commit `[latest pushed]`

I won't touch app code while you design. When you're back with screens, I rebuild to match.

---

## 10. Time estimate

- **Hand sketches** for all 12 screens: 1-2 hours
- **Canva mockups** for all 12 screens: 3-5 hours
- **Figma full design system + 12 screens**: 6-10 hours

For 2-3 day shipping target, **sketches or quick Canva** is the right choice. Polish comes in v0.2.

---

🪔 Take your time. Restraint is the brand.
