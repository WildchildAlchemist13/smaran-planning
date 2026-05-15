# Smaran — Visual Benchmark

> A focused tear-down of the apps Smaran sits next to in the App Store, distilled into 14 specific patterns we can ship in 1–2 days of focused design work. Every pattern is keyed to Smaran's palette (#1A1B3A indigo, #C9A961 gold, #F5EFE3 ivory, #A62024 sindoor, #6B8E5C sage, #2C2C2C charcoal) and Cormorant/Inter type stack.

---

## Apps Studied

- **Calm** — gold standard for ambient-first design: gentle gradients, soft blurs, hero imagery that breathes, transitions that *glide* rather than snap.
- **Hallow** — closest conceptual neighbor (devotional + audio + ritual). Modern minimalist layout, music-app navigation logic, audio player with persistent "intention" overlay.
- **Stoic** — extreme typographic restraint. Almost monochrome, large serif headlines, journaling as the hero surface. Proves that less imagery + more typography = more reverence.
- **Headspace** — best onboarding in the category. Personalization quiz, illustrated empty states, gamified streaks done with restraint.
- **Pray.com** — dark theme reference. Cosmic backgrounds, audio-first hierarchy, leader-follow personalization.
- **Insight Timer** — bold imagery + clean type, but cautionary tale on hierarchy: their menu mixes goals/formats/levels and feels noisy. We will avoid this.
- **Aesop (web + retail)** — the master of premium serif typography (Optima-derived wordmark, Suisse Int'l body), monochrome warmth, and generous negative space. The single best reference for what "premium and ageless" looks like.

---

## 14 Patterns to Adopt

### 1. The "single hero, oceans of space" home screen
- **Best example**: Calm's home screen and Aesop's product detail pages — one image/object, surrounded by 30–40% negative space top and bottom.
- **Why it works**: Premium products *frame* the subject. When 60% of the screen is breathing room, the eye treats the central object as precious. Devotional apps almost always over-fill — Smaran's competitive moat is restraint.
- **Apply to Smaran**: On `app/(tabs)/index.tsx`, the deity-of-the-day card should occupy the **middle 55%** of the viewport. Top safe-area padding **minimum 96px**, bottom padding above the verse card **minimum 64px**. No badges, no streak counters, no greeting text within 32px of the deity image.
- **Priority**: P0

### 2. Display serif at 32–44pt with -1% to -2% letter-spacing
- **Best example**: Stoic and Aesop — large serif headlines (Stoic's morning prompt screen, Aesop's "Skin", "Hair" category labels).
- **Why it works**: Serifs at large sizes feel literary and sacred; tight tracking adds confidence. Most devotional apps use 24pt sans + heavy weights and feel app-store-generic.
- **Apply to Smaran**: Define a `displayLarge` token: **Cormorant Garamond Medium, 36pt, line-height 44pt, letter-spacing -0.4px**, color #F5EFE3 on indigo or #2C2C2C on ivory. Use for: deity name on home, verse number on verse screen, screen titles. Reserve for one element per screen — never two display elements competing.
- **Priority**: P0

### 3. The "1.6 line-height for body" rule
- **Best example**: Aesop's product copy and Hallow's reflection text.
- **Why it works**: Devotional content asks for slow reading. 1.6 line-height (vs the typical 1.4) literally slows the eye down and adds whitespace without adding padding. This is the cheapest "premium" tweak in the entire app.
- **Apply to Smaran**: Body text token = **Inter Regular 17pt, line-height 27pt (1.59), letter-spacing +0.1px**. Apply to commentary, Ask Krishna responses, journal entries, all multi-line UI text. **Never** stack two paragraphs without 16px gap between.
- **Priority**: P0

### 4. The "8-point grid, but breathe at multiples of 24"
- **Best example**: Calm's section spacing — 24/48/96.
- **Why it works**: Default 8/16 spacing makes apps feel functional. Devotional UX needs *generous* rhythm: section gaps of 48–96px communicate "this moment matters."
- **Apply to Smaran**: Spacing tokens: `xs=4, sm=8, md=16, lg=24, xl=48, xxl=96`. **Rule**: any time content shifts from one semantic block to another (e.g., deity image → verse card, verse → commentary), use `xl` (48) minimum. Tap targets stay at md/lg.
- **Priority**: P0

### 5. Gold used as a *single thread*, never decoration
- **Best example**: Hallow's gold cross icon and active-state highlights — gold appears in maybe 3% of the pixel area and only ever signals "this is sacred or this is the path forward."
- **Why it works**: Brand colors lose meaning when over-used. Gold should mark exactly one thing on each screen: either the primary action, the active nav state, or a sacred-content marker (verse number, deity name underline).
- **Apply to Smaran**: Audit every screen for gold usage and enforce a **"max 2 gold elements per screen"** rule. Primary CTAs (Begin, Continue, Submit) use gold. Nav active state uses a 2px gold underline or a 4px gold dot. Streaks, badges, and decorative chrome use Soft Gray #9A9A9A or Sage #6B8E5C — never gold.
- **Priority**: P0

### 6. Hero entrance animation: scale-from-0.96 + fade, 600ms, ease-out-quart
- **Best example**: Calm's home meditation card and Hallow's prayer-of-the-day card both use a slow scale + opacity instead of slide/fade.
- **Why it works**: Sliding feels app-generic. A subtle scale-up reads as "this object is arriving with gravitas." 600ms is slow enough to register, fast enough to never feel sluggish.
- **Apply to Smaran**: In `app/(tabs)/index.tsx`, replace any current `FadeIn` on the deity card with `useSharedValue` + `withTiming({ duration: 600, easing: Easing.out(Easing.quad) })` driving both `opacity: 0→1` and `transform: [{ scale: 0.96→1 }]`. Apply the same to verse-of-the-day card on its screen, with a 120ms stagger after the deity.
- **Priority**: P0

### 7. Audio player as a *poster*, not a control panel
- **Best example**: Hallow's audio player puts the cover art at ~70% of viewport with the user's "intention" text overlaid in serif italic; controls live at the bottom in a single row of small icons.
- **Why it works**: Most audio players are utility. A devotional player is a *focus surface* — it should feel like sitting in front of an altar, not piloting a media app.
- **Apply to Smaran**: Mantra/chant playback screen: **deity art or mandala 70% of viewport**, Sanskrit transliteration overlaid at bottom-third in **Cormorant Italic 22pt #F5EFE3 with 60% opacity**. Controls = single row, **24pt icon size, 32px gaps**, no skip-back/skip-forward chrome (just play/pause + scrub). Background: the deity image blurred + darkened to #1A1B3A at 85% opacity.
- **Priority**: P0

### 8. Onboarding as a *ritual*, not a quiz
- **Best example**: Hallow's onboarding asks 3 calm questions ("What brings you here?" "How often do you want to pray?" "When?"), each on its own full-bleed screen with a single sentence.
- **Why it works**: Headspace and Hallow proved that personalization questions framed as *intention-setting* feel meaningful, not extractive. Stoic flows the last onboarding answer directly into the first journal prompt — value is delivered before the paywall.
- **Apply to Smaran**: 4 onboarding screens, each one full-bleed, one question per screen, **48pt Cormorant question + single-line subtitle in Inter 14pt #9A9A9A**: (1) "What brings you to Smaran?" — Stillness / Wisdom / Devotion / Curiosity; (2) "What time is your morning?"; (3) "Sanskrit, English, or both?"; (4) "May we send you one quiet morning note?" Then immediately deliver today's darshan — paywall comes only after the user has experienced the product.
- **Priority**: P0

### 9. Empty states as *invitations*, with a single illustrated motif
- **Best example**: Headspace empty states feature a single calm illustration + one-sentence prompt + one CTA. Stoic's empty journal screen just says "What's on your mind today?" in serif.
- **Why it works**: Empty states are where most apps look cheapest. Devotional apps especially: a blank journal screen with a "+" button is a missed sacred moment.
- **Apply to Smaran**: Build a reusable `<EmptyState>` component. Three motifs commissioned (or SVG-traced): a single diya outline, a lotus bud, an unstrung mala — all in **#C9A961 at 40% opacity, 64px tall, centered**. Prompt below in **Cormorant 20pt #F5EFE3**, e.g., "Your first reflection waits." CTA is a text link, not a button. Apply to: empty journal, no chants saved, no Ask Krishna history.
- **Priority**: P0

### 10. Bottom nav: icon-only, 1.5px line-weight, 2px gold underline for active
- **Best example**: Aesop's site nav and Calm's tab bar — almost no chrome, no labels, the active state is a thin line not a filled pill.
- **Why it works**: Filled active states (the iOS default) shout. A 2px line whispers, fits the meditative tone, and reads as confident design.
- **Apply to Smaran**: Tab bar background **#1A1B3A with 1px top border #2C2C2C**. Icons: **Lucide or Phosphor at "duotone-light" or stroke 1.5px, 24px size**, color #9A9A9A inactive. Active = icon turns **#F5EFE3** with a **2px x 16px gold #C9A961 underline** centered 4px below the icon. No labels. No background pill. Total nav height **64px including safe area**.
- **Priority**: P0

### 11. Devanagari + Latin lockup: same optical size, different baselines
- **Best example**: Aesop's bilingual labels (Japanese/English on JP packaging) — they optically size scripts differently to match weight, never just match pixel height.
- **Why it works**: Devanagari has more density per glyph than Latin. Setting स्मरण at the same font-size as SMARAN makes the Sanskrit feel heavy. Optical balance requires the Latin label to be ~70% of the Devanagari point size.
- **Apply to Smaran**: Logo lockup token: **Tiro Devanagari Sanskrit 28pt for स्मरण**, **Cormorant SmCp 18pt with letter-spacing +2px for SMARAN**, **vertical gap 6px**, both centered. Same rule for any verse with Sanskrit + English: Sanskrit display at 24pt, English transliteration at 16pt italic. Document this as a `bilingual-lockup.md` design doc.
- **Priority**: P1

### 12. Soft grain + subtle vignette on hero imagery
- **Best example**: Calm's nighttime scenes have a barely-perceptible noise texture; Pray.com's cosmic backgrounds use radial darkening from edges.
- **Why it works**: Pure flat color reads as "AI generated" or "wireframe." 2–4% film grain + 8% radial vignette adds analog warmth — the same principle Pichwai paintings use with their textured backgrounds.
- **Apply to Smaran**: Add a single reusable `<GrainOverlay>` component: a 256×256 tileable PNG of monochrome noise at **3% opacity** layered above any full-bleed image. Add a `LinearGradient` vignette: radial from transparent center to **#0F102B at 12% opacity** at corners. Apply globally to deity art and onboarding backgrounds.
- **Priority**: P1

### 13. Streaks shown as a *quiet count*, never celebrated
- **Best example**: Stoic shows streaks as a tiny number in the top corner. Headspace dialed back its confetti for streaks years ago.
- **Why it works**: Brand voice rule says no "Awesome streak!" — visual treatment must match. A loud streak banner contradicts the entire reverent aesthetic.
- **Apply to Smaran**: Streak displayed only on the profile/settings screen. Format: **"30 days" in Cormorant 32pt #C9A961**, with a one-line caption in Inter 13pt #9A9A9A: *"The practice is becoming yours."* No flames, no progress rings, no fireworks. On milestone days (7, 30, 108, 365), use a single **Sindoor Red #A62024 dot** next to the number — that's the entire celebration.
- **Priority**: P1

### 14. Loading state = a single slow-pulsing bindu
- **Best example**: Calm uses a single slowly-pulsing dot for buffering states. Hallow has a small breathing-circle loader.
- **Why it works**: Spinners are utility-app coded. A breathing element matches the app's metaphor (breath, presence) and visually reinforces the brand mark (the recommended Bindu logo).
- **Apply to Smaran**: Build `<BinduLoader>`: a **6px gold #C9A961 dot** centered, animating **scale 0.8 → 1.2 → 0.8 over 2400ms ease-in-out, infinite**. Use everywhere there's a network wait — Ask Krishna response loading, audio buffering, content fetch. Replace any `ActivityIndicator` with this. The loader becomes a brand moment instead of dead time.
- **Priority**: P1

---

## Anti-patterns we should avoid

1. **Marketing-app onboarding carousels** — the "Welcome to Smaran! Swipe to learn more!" pattern is dead. It's friction with no value. Hallow and Stoic both deliver the product on screen 1.
2. **Filled-pill bottom nav active states** (iOS default Tab Bar) — too loud, too generic. Replace with the 2px gold underline.
3. **Ratings prompts and "rate us" modals in the first session** — breaks the sacred moment, screams "engagement metric." Wait for week 2 minimum.
4. **Confetti / fireworks / celebratory motion on streaks** — directly violates brand voice. Quiet acknowledgment only.
5. **Decorative gradients behind every card** — Sri Mandir does this; it's why their app feels cheap. Cards live on flat surfaces; depth comes from spacing, not shadows.
6. **Emoji in UI** (🪔🙏🕉) — even in notifications, restraint. The brand doc allows 🪔 sparingly; the UI itself uses zero emoji.
7. **"Premium" badges in gold on every paid feature** — gold becomes wallpaper. Use a single "—" Sage Green divider line under premium-only sections instead.

---

## One-line synthesis

> *Smaran's visual principle is **restraint as reverence** — every pixel earned by a serif at 36pt with breathing room around it, every animation 600ms slower than you think it should be, every gold mark used once and only once per screen.*

If we hold that line, the app will feel like Aesop's window display crossed with the moment before a temple aarti — premium, ageless, and unmistakably Smaran.

---

## Sources

- [The Aesthetics of Calm UX — Raw.Studio](https://raw.studio/blog/the-aesthetics-of-calm-ux-how-blur-and-muted-themes-are-redefining-digital-design/)
- [How Headspace Designs for Mindfulness — Raw.Studio](https://raw.studio/blog/how-headspace-designs-for-mindfulness/)
- [Hallow: Prayer & Meditation — ScreensDesign](https://screensdesign.com/showcase/hallow-prayer-meditation)
- [Catholic Prayer App, Hallow: 6-Month Review — Medium](https://medium.com/catholicism-for-the-modern-world/catholic-prayer-app-hallow-6-month-review-88a699674229)
- [Stoic — Self-Reflection Journaling App Review — MobileSyrup](https://mobilesyrup.com/2019/06/09/stoic-self-reflection-journaling-app-review/)
- [Pray.com: Bible & Daily Prayer — ScreensDesign](https://screensdesign.com/showcase/praycom-bible-daily-prayer)
- [Insight Timer Design Critique — IXD@Pratt](https://ixd.prattsi.org/2021/02/design-critique-insight-timer-android-app/)
- [Aesop logo, website and packaging — Fonts In Use](https://fontsinuse.com/uses/20234/aesop-logo-website-and-packaging)
- [Headspace iOS Onboarding Flow — Mobbin](https://mobbin.com/flows/7cdc08c0-3bcb-4882-90dd-5cf92019616f)
- [Empty States, Error States & Onboarding — Raw.Studio](https://raw.studio/blog/empty-states-error-states-onboarding-the-hidden-ux-moments-users-notice/)
