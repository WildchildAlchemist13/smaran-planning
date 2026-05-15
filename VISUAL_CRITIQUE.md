# Smaran — Visual Design Critique

> Audited: 2026-05-15. Reviewer brief: push the app from "competent indie devotional" to "best-in-class on the App Store, in the Calm/Hallow/Stoic/Aesop tier." Every finding below is grounded in a specific file:line and includes the exact change to make.

---

## Overall Assessment

Smaran is on the right *idea* — the palette is correct, the architecture (single canvas + bottom sheet) is right, the typography ambition is correct, and the motion vocabulary in `design-tokens.ts` is well thought out. But on the actual screens the execution is one polish pass short of premium. The app currently reads as **a thoughtful brand stretched over the default React Native primitives**: system fonts everywhere, ASCII glyphs (`✦`, `→`, `←`, `♡`) where real iconography or SF Symbols / lucide should live, gold borders on every container (which kills the gold's preciousness), spacing rhythm that defaults to `Spacing.lg` (24) almost everywhere, and a streak badge / hamburger menu / "Read full" gold pill on the same screen that pull the eye in three directions. The biggest gap from "best in class" is **typographic and spacing restraint** — Calm and Aesop hold a single focal element with 80–120pt of breathing room above and below; Smaran's home screen currently has the deity card pressed up against a cluttered top bar with `Spacing.lg` (24) of margin, and four content blocks (greeting, verse, actions, Ask Krishna) stacked at `Spacing.xl` (32) each. The screens feel **adjacent to premium** rather than premium. A targeted polish pass on the items below should close the gap.

---

## P0 Issues (ship-blockers — looks cheap/broken)

- **theme.ts:46–64** — `Fonts` object falls back to `'system-ui'` / `'serif'` on iOS+Android, so **no custom fonts are actually loaded anywhere**. Despite `BRAND.md` specifying Cormorant Garamond + Tiro Devanagari + Inter, the entire app renders in San Francisco / Roboto. This is the single biggest reason the app reads "indie" not "premium." **Fix:** install `expo-font` + `@expo-google-fonts/cormorant-garamond`, `@expo-google-fonts/tiro-devanagari-sanskrit`, `@expo-google-fonts/inter`. Load in `app/_layout.tsx` with `useFonts({...})`. Then add font keys to every `TypeScale` entry in `design-tokens.ts:35–53` (e.g. `display1: { ..., fontFamily: 'TiroDevanagariSanskrit_400Regular' }`, `verse: { ..., fontFamily: 'CormorantGaramond_300Light_Italic' }`, body sans → `Inter_400Regular`). Until this lands, nothing else will move the needle.

- **app/index.tsx:159–162** — The hamburger menu is hand-rendered as three `View` lines (`menuLine`). It looks like a placeholder, especially next to the gold streak pill. **Fix:** replace with a proper icon. Use `lucide-react-native`'s `<Menu size={20} color={Brand.ivory} strokeWidth={1.25} />`. Same package for the back arrow (`<ArrowLeft strokeWidth={1.25} />` in `verse-detail.tsx:40`, `about.tsx:30`, `ask-krishna.tsx:30`, `saved.tsx:61`, `settings.tsx:60`, `curator.tsx:95`) and for `↗` share / `→` chevrons. ASCII arrows in a premium app immediately date it.

- **app/index.tsx:241–247** — The save / share buttons are circles with the literal characters `♡` and `↗` rendered at 18pt. On Android these glyphs render in the system emoji font and look like 1990s clipart. **Fix:** use `lucide-react-native`'s `<Heart />`, `<HeartFill />`, `<Share2 />` at size 18, `strokeWidth={1.25}`, color `Brand.ivory`. Do the same for `actionText` on line 375.

- **app/index.tsx:185, 191, 199, 264** — The "ornament" `✦` is a raw Unicode glyph that renders inconsistently (sometimes black-and-white text, sometimes color emoji) across Android versions. Same problem at `MenuSheet.tsx:78` and `ask-krishna.tsx:108`. **Fix:** replace with a small SVG mark (an outlined diamond or a four-point star drawn with `react-native-svg` at 12px gold). Or import lucide's `<Sparkle size={12} color={Brand.sacredGold} strokeWidth={1} />`. Define once as `<Ornament />` component and reuse.

- **app/index.tsx:313–316** — The deity card has `borderWidth: 1, borderColor: Brand.sacredGold` (full opacity, full saturation gold). With actual deity art behind it, this reads as a "preview thumbnail with gold frame" — Etsy, not Aesop. **Fix:** change to `borderWidth: 0` when `deityImageSource` is present, and `borderColor: 'rgba(201, 169, 97, 0.18)'` when placeholder. Premium apps use no border on hero imagery; let the radius and shadow do the framing. Add `Shadow.elevated` instead.

- **app/onboarding.tsx:281–308** — The deity grid uses `width: '30%'` with `gap: 12` and `padding: 8`, producing chips that wrap awkwardly on narrow Android devices and have poor optical centering. The Sanskrit char (22pt) and the Latin name (12pt) are too close in weight and create a muddled hierarchy. **Fix:** set `width: '31%'` (so 3 across at lg=24 padding), `aspectRatio: 0.85` (slightly tall, not square), increase Sanskrit to `fontSize: 28, fontWeight: '300'` and Latin to `fontSize: 11, letterSpacing: 2, textTransform: 'uppercase', opacity: 0.7, marginTop: 8`. The Latin should read like a caption under the Sanskrit, not a label beside it.

- **app/onboarding.tsx:309–325** — `primaryBtn` is a flat gold rectangle with `borderRadius: 8` and no pressed state. On a dark indigo background with a flat-color button, it looks like a Bootstrap CTA from 2014. **Fix:** (1) add `({ pressed }) => [styles.primaryBtn, pressed && { opacity: 0.85, transform: [{ scale: 0.98 }] }]`. (2) increase `borderRadius` to `Radius.pill` (999) for an ovular pill, OR keep 8 but add `Shadow.card` and an inner highlight (`backgroundColor: Brand.sacredGold, shadowColor: Brand.sacredGold, shadowOpacity: 0.25, shadowRadius: 16, shadowOffset: {width: 0, height: 4}`). (3) Increase `paddingVertical` from 16 to 18 and `letterSpacing` to 2 on the label. Apply this same fix to the `readMoreBtn` in `index.tsx:374`.

- **app/about.tsx:174, ask-krishna.tsx:108–109, about.tsx:96** — The `🪔` diya emoji renders as the user's system emoji set (Apple's gradient lamp on iOS, Google's flat brown lamp on Android). Both look like a sticker pasted on top of a reverent layout. **Fix:** replace with a small monochrome SVG diya in `Brand.sacredGold` at 24px. If keeping an emoji is non-negotiable for now, set `opacity: 0.8` and add `fontSize: 22` (smaller than 24) with `marginTop: Spacing.lg` more breathing room. Even better: drop it entirely and use an `<Ornament />` (small gold dot or diamond).

---

## P1 Issues (next polish pass)

- **app/index.tsx:285–293** — The top bar is too tight. Wordmark, hamburger, streak pill all sit at `paddingVertical: Spacing.md` (16). Calm and Hallow give their top bar 56–64pt of vertical breathing room above the hero. **Fix:** change `paddingVertical: Spacing.md` to `paddingTop: Spacing.lg, paddingBottom: Spacing.xl` (24/32) and reduce wordmark `letterSpacing` from 6 to 4 (currently feels stretched at 13pt size).

- **app/index.tsx:294–305** — The streak badge uses `backgroundColor: 'rgba(201, 169, 97, 0.12)'` + `borderColor: 'rgba(201, 169, 97, 0.3)'` + gold number at `fontWeight: '600'`. This is three gold treatments stacked — the dot is bigger and louder than the wordmark. **Fix:** drop the background fill (`backgroundColor: 'transparent'`), keep only the 1px border at 0.2 opacity, change number weight to '400', and reduce `minWidth` from 32 to 28. The streak should whisper, not announce.

- **app/index.tsx:349–358** — The greeting block stacks `greetingSmall` (13pt soft gray), `greetingLarge` (h1 = 26pt), and `energyLine` (12pt gold uppercase) with only `Spacing.xs` (4) and `Spacing.sm` (8) between them. The hierarchy is too compressed. **Fix:** change `greetingLarge` to `marginTop: Spacing.sm` (8) and `energyLine` to `marginTop: Spacing.md` (16). Drop `energyLine` `letterSpacing` from 4 to 3 (4 starts to break optical word recognition at 12pt). Consider moving the energy line ABOVE the deity name (small caps gold → big serif name → no extra label), which is the Aesop product-card move.

- **app/index.tsx:359–362** — Verse block sits at `marginTop: Spacing.xl` (32) below the greeting. For the screen's actual hero text (the verse), this is not enough breathing room. **Fix:** increase to `marginTop: Spacing.xxl` (48) and add a thin gold divider (40% width, `rgba(201,169,97,0.2)`, centered) ABOVE the `verseRef` to visually separate "deity moment" from "verse moment." This single change does more than any other to make the screen feel composed.

- **app/index.tsx:362–377** — Action row: two ghost-circle icon buttons (`♡`, `↗`) and a solid gold "Read full →" pill share the same row with `gap: Spacing.md`. The visual weights are wildly mismatched (two thin outlined circles vs. one filled pill), so the row reads as off-balance. **Fix:** either (a) make all three the same height and use a ghost treatment for "Read full" too (gold border, gold text, indigo fill) — restraint move; or (b) keep the pill but right-align it and put the icons left-aligned with `justifyContent: 'space-between'`. Don't center three items of different weights.

- **app/index.tsx:383–390** — `askKrishnaLink` is italic gold at 14pt, opacity 0.85. Sitting alone at the bottom with no surrounding affordance, it reads as a tagline, not a tap target. **Fix:** wrap in a thin gold outline pill: `borderWidth: 1, borderColor: 'rgba(201, 169, 97, 0.25)', paddingHorizontal: Spacing.lg, paddingVertical: Spacing.md, borderRadius: Radius.pill`. Increases tap affordance without breaking the quiet tone. Also add a small `<Sparkle>` icon to the left of the text.

- **app/onboarding.tsx:266–276** — The name input has `borderWidth: 1, borderColor: 'rgba(201, 169, 97, 0.3)'` AND `backgroundColor: 'rgba(245, 239, 227, 0.04)'` AND `borderRadius: 8`. The brief in `SCREENS_DESIGN.md:139` explicitly asks for "no box border, just a thin gold underline; ivory text." **Fix:** remove `borderWidth`, `borderRadius`, `backgroundColor`. Add `borderBottomWidth: 1, borderBottomColor: 'rgba(201, 169, 97, 0.4)'` only. Increase `paddingVertical` to 18, drop horizontal padding to 0, set `fontSize: 22, fontWeight: '300', textAlign: 'center'`. This is the single change that will make onboarding feel curated.

- **app/onboarding.tsx:326–332** — The "Skip" link is `textDecorationLine: 'underline'` — wrong vibe entirely (looks like a 1998 hyperlink). **Fix:** remove underline, keep `color: Brand.softGray`, add `letterSpacing: 2, textTransform: 'uppercase', fontSize: 11`. Make it visually quieter than the primary button instead of louder.

- **app/onboarding.tsx (welcome screen, lines 73–88)** — The four blocks (Devanagari → Latin → tagline → bullet points → Begin button) are not animated. The brief explicitly calls for fade-in stagger (`SCREENS_DESIGN.md:124`). **Fix:** wrap each in `<Animated.Text entering={FadeIn.duration(Motion.slow).delay(N)}>` with delays 0 / 300 / 600 / 900 / 1200. Same for steps 1/2/3 (use `FadeIn.duration(Motion.base)` on step transitions instead of nothing).

- **app/onboarding.tsx:189–204** — There's no progress indicator (the brief says "Step 1 of 3" *is* the progress indicator, which is fine), but transitions between steps are instant (state change). **Fix:** swap to a single `Animated` view per step keyed on `step`, or use `react-native-reanimated`'s `Layout` + `entering={SlideInRight.duration(Motion.base)}` / `exiting={SlideOutLeft}` so the steps slide in from the right (matches `SCREENS_DESIGN.md:457`).

- **app/ask-krishna.tsx:36–42** — `mark` (the brand image) is rendered at 96×96. On a screen that's intentionally a "doorway" moment, the mark is too small. **Fix:** bump to 120×120, add `marginTop: Spacing.xxl` (48) above to push it down from the back arrow.

- **app/ask-krishna.tsx:160–168** — The "example" block has a `backgroundColor: 'rgba(245, 239, 227, 0.04)'` + `borderColor: 'rgba(201, 169, 97, 0.15)'` + `borderRadius: 12` card. This "card" treatment makes the example feel like a quote tile from a Medium article. The rest of the screen is borderless, breath-paced layout — the card breaks the language. **Fix:** remove `backgroundColor`, `borderWidth`, `borderColor`, `borderRadius`. Replace with vertical breathing room (`paddingVertical: Spacing.xxl`) and a thin gold rule above and below (`borderTopWidth: 1, borderBottomWidth: 1, borderColor: 'rgba(201, 169, 97, 0.18)'`).

- **app/ask-krishna.tsx:196–201** — `exampleSanskrit` is `fontSize: 18` — same as body text. The Sanskrit should be the visual peak of the example block. **Fix:** bump to `fontSize: 26, lineHeight: 38, letterSpacing: 1, marginVertical: Spacing.lg`. Add `fontWeight: '300'`.

- **app/verse-detail.tsx:124–129** — Sanskrit verse at `verseSanskrit` (22pt). For a screen whose entire job is to make the Sanskrit feel sacred, 22pt is small. **Fix:** in `design-tokens.ts:51` change `verseSanskrit` to `{ fontSize: 30, lineHeight: 46, fontWeight: '300', letterSpacing: 1 }`. Also add `paddingHorizontal: Spacing.md` on the `sanskrit` style so wrapped lines don't kiss the screen edge.

- **app/verse-detail.tsx:136–142** — The divider at `width: '40%'` creates a hard horizontal line every section. The detail screen has 3 dividers in quick succession (lines 72, 88) — feels like a Word document with section breaks. **Fix:** keep one divider between Sanskrit/translation, replace the others with vertical space (`marginTop: Spacing.xxl`) and let the small-caps section labels do the dividing work.

- **app/saved.tsx:122–131** — Saved cards have `backgroundColor: 'rgba(245, 239, 227, 0.04)'` + `borderRadius: Radius.md (8)` + `borderWidth: 1` + `borderColor: 'rgba(201, 169, 97, 0.12)'`. Same Medium-tile aesthetic as the example block in ask-krishna. **Fix:** drop the background and border. Use only a thin bottom rule (`borderBottomWidth: 1, borderBottomColor: 'rgba(201, 169, 97, 0.1)'`) and increase `paddingVertical` from `Spacing.lg` to `Spacing.xl`. Cards-as-rows, not cards-as-tiles.

- **app/saved.tsx:154–160** — Empty state ornament is a raw `·` (middle dot) at 48pt, opacity 0.6. Reads like a typo. **Fix:** replace with a small lotus or diya SVG at 32pt gold, OR use `<Sparkle size={28} color={Brand.sacredGold} strokeWidth={0.75} />` from lucide. Increase `marginBottom` from `Spacing.lg` to `Spacing.xxl`.

- **app/settings.tsx:145–157** — Setting rows have `paddingVertical: Spacing.lg` (24), which is good, but the `rowChevron: '→'` arrow in soft gold opacity 0.5 reads as "low-effort." **Fix:** swap for `<ChevronRight size={16} color={Brand.sacredGold} strokeWidth={1.25} style={{ opacity: 0.4 }} />`. Same fix for `MenuSheet.tsx:132` itemArrow.

- **components/MenuSheet.tsx:65** — The sheet uses `SlideInDown.duration(Motion.slow).springify().damping(20)` — `springify()` is the bouncy spring the brief explicitly forbids (`SCREENS_DESIGN.md:466`). **Fix:** remove `.springify().damping(20)`. Use just `SlideInDown.duration(Motion.slow).easing(Motion.easeOut)`. Same audit needed at `index.tsx:171` (`FadeInDown.duration(Motion.slow).delay(200).springify().damping(20)`) — drop springify, keep `easing(Motion.easeOut)`.

- **components/MenuSheet.tsx:122–127** — Items have `paddingVertical: Spacing.lg` (24) but no horizontal padding inside the `Pressable`, so the tap target ends at the text. **Fix:** add `paddingHorizontal: Spacing.md` inside `item`, then offset by reducing `sheetInner.paddingHorizontal` to compensate. Pressed state opacity 0.6 is too aggressive (looks broken on press). Change to 0.85 and add a subtle `backgroundColor: 'rgba(245, 239, 227, 0.04)'` on press.

- **components/MenuSheet.tsx:108–116** — Sheet handle is `width: 36, height: 4`. Modern bottom sheets (iOS 16+, Material You) are typically `width: 32, height: 4, marginTop: Spacing.sm` and use a more subtle color. **Fix:** drop opacity to 0.15, width to 32. Minor but it's the first thing eyes land on when sheet opens.

- **app/curator.tsx:188–202** — Curator rows have `borderBottomWidth: 1, borderBottomColor: 'rgba(201, 169, 97, 0.1)'`. Combined with the gold-bordered preview tile and the gold "Pick image" button, every deity row has 3 gold accents fighting. This is admin-only so lower priority, but **Fix:** drop `borderColor` on the preview to `rgba(201, 169, 97, 0.15)` and `borderBottomColor` to `rgba(245, 239, 227, 0.06)` (ivory wash, not gold).

---

## P2 Issues (future polish)

- **design-tokens.ts:35** — `display1` has `letterSpacing: 4`. At 56pt that's optically too much breathing for Devanagari (Devanagari letters are connected; tracking breaks the script). Consider `letterSpacing: 2` for Devanagari-dominant uses and reserve 4+ for Latin all-caps only.

- **design-tokens.ts:84–98** — `Shadow.card` has `shadowOpacity: 0.15` on a `#000000` color. On the indigo background (`#1A1B3A`), black shadows are invisible. **Fix:** use `shadowColor: '#000000', shadowOpacity: 0.5` for shadows on indigo, or define a separate `Shadow.onIndigo` token that uses higher opacity.

- **app/index.tsx:336–346** — `deitySanskritOnImage` uses `textShadowColor: 'rgba(0,0,0,0.5)'`. This is necessary for legibility but the shadow at radius 4 / opacity 0.5 looks crispy. **Fix:** use radius 12, opacity 0.6 for a softer halo. Or better: add a true gradient overlay (`expo-linear-gradient` from transparent to `rgba(26,27,58,0.7)` over the top 30% of the image).

- **app/index.tsx:317–320** — The single flat `imageOverlay` at `rgba(26, 27, 58, 0.25)` makes every deity image look slightly hazy. **Fix:** replace with a vertical gradient that's transparent in the middle and only darkens at top (for Sanskrit) and bottom (for any future caption). Use `LinearGradient` from `expo-linear-gradient`. Crisp center, dim edges.

- **app/index.tsx:170–203** — The deity card animation uses `FadeInDown` with `springify()`. The brief calls for "fade + 4px slide up" (`SCREENS_DESIGN.md:458`) — `FadeInDown` without specifying `withInitialValues` actually slides DOWN by ~25px. **Fix:** use `entering={FadeIn.duration(Motion.slow).delay(200)}` and animate translateY separately with `useAnimatedStyle` from -4 → 0 over 800ms. More controlled.

- **app/about.tsx:136–141** — The Devanagari hero (`स्मरण` at 48pt) has `letterSpacing: 4`. Same Devanagari tracking issue as above. Fix to `letterSpacing: 2`.

- **app/about.tsx:142–146** — The wordmark uses `TypeScale.display2` (32pt, weight 500, letterSpacing 6). On a dark background at 32pt, weight 500 looks blocky. **Fix:** weight 400 with letterSpacing 8 reads more "luxury wordmark" (think Aesop, Le Labo).

- **app/verse-detail.tsx:175–186** — "Sanskrit recitation coming soon" placeholder block has only a `borderTopWidth: 1`. After commentary divider + sit-with divider + audio top border, that's 3 horizontal lines on a contemplative screen. **Fix:** drop the border, replace with `marginTop: Spacing.xxxl` and small-caps centered text only.

- **app/_layout.tsx** — (Inferred from imports.) Worth verifying that `StatusBar` is set to `style="light"` and `backgroundColor={Brand.smaranIndigo}` so Android doesn't paint a white status bar over the indigo. Also confirm `SystemUI.setBackgroundColorAsync(Brand.smaranIndigo)` is called, otherwise the navigation gesture area on Android will flash white during transitions.

- **app/saved.tsx:81** — Each card has a stagger delay of `index * 60` ms. For a list that could grow to 30+ items, the last card will animate at 1800ms — feels broken. **Fix:** cap the delay: `Math.min(index, 8) * 60`.

- **app/index.tsx:263–265** — `askKrishnaLink` uses TWO spaces before `→` to give breathing room. Fragile (depends on font kerning) and won't survive a font swap. **Fix:** use a single ` ` (en-space) or wrap arrow in a separate `<Text>` with `marginLeft: Spacing.sm`.

- **app/onboarding.tsx:198, 202–203** — Hardcoded values (`padding: 24`, `minHeight: 600`, `paddingTop: 40`) bypass the token system. Replace with `Spacing.lg`, `Spacing.xxxl + Spacing.xxl`, etc.

---

## Top 5 changes that would push visual quality the most

Ranked by impact-per-hour. Do these first.

### 1. Load real fonts (Cormorant Garamond + Tiro Devanagari Sanskrit + Inter)

**Why first:** every other typographic decision in the design system is currently rendering as system San Francisco / Roboto. This single change moves the app from "tech app with brand colors" to "publication-quality reading experience."

- **Files:** `app/_layout.tsx`, `constants/theme.ts:45–64`, `constants/design-tokens.ts:33–53`
- **Change:**
  ```bash
  npx expo install expo-font @expo-google-fonts/cormorant-garamond \
    @expo-google-fonts/tiro-devanagari-sanskrit @expo-google-fonts/inter
  ```
  In `_layout.tsx` add `useFonts({ ... })` and gate the root render on font load.
  Then in `design-tokens.ts:35–53`, append `fontFamily` to each entry — display1/display2 → `'TiroDevanagariSanskrit_400Regular'` for Devanagari uses, `'CormorantGaramond_500Medium'` for Latin display, `verse` → `'CormorantGaramond_300Light_Italic'`, `body`/`bodySmall`/`caption`/`caps` → `'Inter_400Regular'` (or `'Inter_500Medium'` for caps).

### 2. Replace all ASCII glyph icons with lucide-react-native

**Files:** `app/index.tsx:159–162` (menu), `:241–247` (heart/share), `:185, 191, 199, 264` (✦), `app/verse-detail.tsx:40` (back), `app/about.tsx:30` (back), `app/saved.tsx:61` (back), `app/settings.tsx:60, 118` (back, chevron), `app/curator.tsx:95` (back), `app/ask-krishna.tsx:30` (back), `components/MenuSheet.tsx:78, 82, 132` (sparkle, arrow).

```bash
npx expo install lucide-react-native react-native-svg
```

Use `<Menu />`, `<Heart />`, `<Share2 />`, `<ChevronRight />`, `<ArrowLeft />`, `<Sparkle />` at `strokeWidth={1.25}`. Define a single `<Ornament />` wrapper component for the sparkle.

### 3. Remove the gold border on the deity card; let the image breathe

**File:** `app/index.tsx:313–316`

```ts
deityCard: {
  aspectRatio: Layout.cardAspectRatio,
  borderRadius: Radius.lg,
  marginTop: Spacing.xl,             // was Spacing.lg
  padding: Spacing.lg,
  justifyContent: 'space-between',
  overflow: 'hidden',
  // borderWidth: 1,                  // REMOVE
  // borderColor: Brand.sacredGold,   // REMOVE
  ...Shadow.elevated,                 // ADD — soft drop shadow
  position: 'relative',
},
```

Then add a thin (1px) hairline overlay in `rgba(201,169,97,0.15)` only when placeholder is shown (no real image). Premium hero imagery never has a gold frame.

### 4. Fix the home screen vertical rhythm

**File:** `app/index.tsx:285–390`

The current rhythm is `md → lg → xl → xl → xl → xxl`. It needs to breathe more between the deity moment and the verse moment. Apply:

- `topBar.paddingVertical: Spacing.md` → `paddingTop: Spacing.lg, paddingBottom: Spacing.xl` (line 289)
- `deityCard.marginTop: Spacing.lg` → `Spacing.xl` (line 309)
- `greetingBlock.marginTop: Spacing.xl` → `Spacing.xxl` (line 349)
- `verseBlock.marginTop: Spacing.xl` → `Spacing.xxl` (line 359), and **add a thin gold divider** above the `verseRef` (40% width, `rgba(201,169,97,0.18)`, centered, with `marginBottom: Spacing.xl`)
- `actions.marginTop: Spacing.xl` → `Spacing.xxl` (line 362)

### 5. Onboarding name input: kill the box, keep the underline

**File:** `app/onboarding.tsx:266–276`

This is the single onboarding moment that says "this is a thoughtful product." Currently it looks like a sign-up form.

```ts
input: {
  color: Brand.ivory,
  fontSize: 24,                        // was 18
  fontWeight: '300',
  paddingVertical: 18,                 // was 14
  paddingHorizontal: 0,                // was 18
  borderBottomWidth: 1,                // replaces borderWidth
  borderBottomColor: 'rgba(201, 169, 97, 0.4)',
  // borderRadius: 8,                  // REMOVE
  // backgroundColor: ...              // REMOVE
  textAlign: 'center',
  letterSpacing: 0.5,
  marginHorizontal: Spacing.xl,
},
```

---

## What's working well (preserve)

1. **`design-tokens.ts` motion vocabulary** (lines 59–70) — the `fast/base/slow/reverent` semantic naming is exactly right for a contemplative product. Keep this and use it consistently (the audit found a few `springify()` calls that violate it; once those are removed the language is clean).

2. **`Brand` palette enforcement via `theme.ts`** — every screen imports `Brand.*` rather than hardcoding hex. This makes the future "swap gold to bronze" or "test sage variant" experiments trivial. Don't let that discipline slip.

3. **The architecture decision: single canvas + bottom sheet menu** (`MenuSheet.tsx`) — this is correct for a ritual product and matches the brief. The execution needs polish (see P1) but the bone structure is right. Don't be tempted to bring back tabs.

4. **Verse-detail's content order** — Sanskrit → transliteration → divider → translation → reflection prompt is the correct devotional reading order. This is more thoughtful than 90% of religious apps. Preserve the sequence even when you re-spec the dividers and typography.

5. **The Ask Krishna "coming soon" page tone** — the body copy ("Every response will cite a real verse... never as the deity") is brand-perfect. The visual treatment needs the fixes above, but the *content* is doing real brand-building work and should not be diluted in v0.2 when the AI ships.

6. **Onboarding step labels** (`· Step 1 of 3 ·`) — the centered dot-flanked small-caps treatment is genuinely elegant and is exactly the kind of typographic restraint that separates Smaran from Sri Mandir. Carry this convention into Settings, About section headers, and any future "moment" screen.

7. **Empty-state copy in saved.tsx:71** ("The verses that meet you, you can save here.") — this is the kind of devotional, non-pushy microcopy that a brand voice document promises and most apps fail to deliver. The visual presentation needs help (see P1 ornament fix), but the words are right.

---

## Closing note

The gap between current Smaran and "best in class" is not architectural — it's **finish**. The bones (palette discipline, single-canvas IA, motion vocabulary, restraint-first instinct) are correct. The execution gaps are concentrated in three areas, in priority order: **(1) actually load the brand fonts**, **(2) replace ASCII glyphs with real iconography**, **(3) remove gold borders from hero containers and let one focal element command each screen.** Doing items 1, 2, 3, and the top-5 changes above — maybe 4–6 hours of focused work — would close 80% of the distance to Calm/Hallow tier.
