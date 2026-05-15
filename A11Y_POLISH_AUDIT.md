# Smaran — Accessibility & Polish Audit

Audit scope: Expo + React Native + TypeScript app at `/Users/revanthsharma/Claude Code Desktop/Smaran-app/`. Files reviewed: `_layout.tsx`, `index.tsx`, `onboarding.tsx`, `ask-krishna.tsx`, `curator.tsx`, `settings.tsx`, `about.tsx`, `saved.tsx`, `verse-detail.tsx`, `components/MenuSheet.tsx`, `constants/theme.ts`, `constants/design-tokens.ts`. Date: 2026-05-15. Standard: WCAG 2.1 AA (Android-first).

---

## Accessibility Summary (WCAG 2.1 AA)

- **Pass / Fail**: **Conditional pass** — most surfaces clear AA easily because Smaran's palette is unusually high-contrast (ivory-on-indigo is 14.5:1, gold-on-indigo is 7.4:1). But there are **5 P0 violations** that fail AA outright, all in low-opacity decorative-text decisions where the developer used opacity as a styling shortcut.
- **Critical violations**: **5 P0** (contrast failures + missing accessibilityRole on the menu hamburger and Pressable rows + the heart "active" state reading as red on dark red).
- **Color contrast verified**: 24 combinations computed below. Headline numbers — gold (#C9A961) on indigo (#1A1B3A) = **7.40:1 ✓**, ivory (#F5EFE3) on indigo = **14.53:1 ✓**, soft gray (#9A9A9A) on indigo = **5.91:1 ✓**, sindoor red (#A62024) on indigo = **2.26:1 ✗** (fail — only safe on ivory at 6.43:1), sage green on indigo = **4.48:1 ✓ (just barely)**.

The brand passes AA on its core combinations. Where it fails is at the seams: opacity-dimmed text, sindoor on dark, and gold-at-low-opacity icons.

---

## P0 Critical Fixes (must fix before launch)

| Screen | File:line | Issue | Fix |
|---|---|---|---|
| Settings | `app/settings.tsx:88-91`, `:162-170` | Version block wrapper sets `opacity: 0.5` on already-soft-gray text. Effective color blends to `#5A5A6A` for **2.46:1** — fails AA for body text and large text alike. Reads "Smaran v0.1.0 · Early access" — load-bearing, not decorative. | Remove `opacity: 0.5` from `versionBlock`; if you want it dimmer, change color to a fixed hex that is at minimum 4.5:1 (e.g. `#8C8C9C` blended once = 4.5:1). |
| About | `app/about.tsx:169-176` | `footer` wraps `versionLabel` (soft gray) in `opacity: 0.6`. Effective color `#676774`, **2.99:1 — fail**. | Drop the wrapper opacity. Use a single color token like `Brand.softGray` at full opacity (5.91:1). |
| Curator | `app/curator.tsx:227-234` | `footerNote` is soft gray + `opacity: 0.7` — `#74747D`, **3.60:1**. Fails AA body (4.5:1). The text explains where images live and the cloud-sync promise — meaningful. | Remove the `opacity: 0.7` line; soft gray alone is 5.91:1. |
| Home | `app/index.tsx:235-244, 363-376` | The "saved" heart active state renders sindoor red (#A62024) on a sindoor-tinted indigo background (`rgba(166,32,36,0.2)` over indigo blends to `#361C36`) — **2.07:1**. The user's primary save-confirmation cue is invisible to anyone with even mild low-vision. | Either (a) render the active heart as **gold** on the sindoor background — gold-on-#361C36 is ~6.5:1 — or (b) keep red but fill the background to ivory and use red-on-ivory (6.43:1). Don't rely on the red-on-red contrast for the "saved" signal. |
| Home | `app/index.tsx:152-162` | Hamburger menu button has `accessibilityLabel="Open menu"` but no `accessibilityRole="button"`. TalkBack on Android reads it as "Open menu" with no affordance hint — sighted users see three lines, screen-reader users get no indication it's tappable. Same defect on every other Pressable in the app (Settings rows, MenuSheet items, action buttons). | Add `accessibilityRole="button"` to **every** `Pressable` that has an `accessibilityLabel`. This is a one-line fix repeated ~18 times. Consider creating a `<TouchableRow>` component to enforce it. |

These five must land before public Play Store launch. The version-block and curator-footer cases would also be a problem if Google's pre-launch report runs an accessibility scan.

---

## P1 Polish (highly recommended)

| Screen | File:line | Issue | Fix |
|---|---|---|---|
| Home | `app/index.tsx:291-292` | Menu button is `width: 32, height: 32` — **below 44pt minimum**. `hitSlop={12}` saves it (gives 56pt effective tap area), but visually the target reads as too small. | Bump visual size to 44×44 or accept the hit-slop solution but document it in the design tokens. The streak badge has the same issue (`32×32` + no hitSlop) — make it Pressable-able if you ever add a "tap to see calendar" feature. |
| Home | `app/index.tsx:294-305` | Streak badge is decorative-only currently but reads as a button visually. Either give it `accessibilityLabel={`{streak} day streak`}` and `accessibilityRole="text"`, or wrap in a Pressable that opens a streak-history modal. Today, screen readers will read "0" with no context. | Add `accessibilityLabel={`${streak} day streak`}` to the `Animated.View`. |
| Onboarding | `app/onboarding.tsx:128-141` | Deity chip Pressables have no `accessibilityLabel` and no `accessibilityState={{ selected: ... }}`. Selecting one is invisible to screen readers. | Add `accessibilityLabel={deity.name}`, `accessibilityRole="button"`, `accessibilityState={{ selected: selectedDeity === deity.key }}`. |
| Onboarding | `app/onboarding.tsx:114-117, 148-150, 164-166` | Three "Skip" Pressables with `<Text>` only — no `accessibilityLabel`, no role. | Add `accessibilityRole="button"`, `accessibilityLabel="Skip this step"`. |
| Onboarding | `app/onboarding.tsx:175-187` | `PrimaryButton` has no accessibility props at all. Disabled state is conveyed only by opacity 0.4. | Add `accessibilityRole="button"`, `accessibilityState={{ disabled }}`, `accessibilityLabel={label}`. |
| MenuSheet | `components/MenuSheet.tsx:49-55` | `<Modal>` lacks `accessibilityViewIsModal` (iOS) and the backdrop Pressable has `accessibilityLabel="Close menu"` but the sheet items don't have a sibling-trap behavior. On TalkBack the user can swipe to elements behind the sheet. | Set `accessibilityViewIsModal` on the sheet `Animated.View` and `importantForAccessibility="no-hide-descendants"` on the backdrop wrapper. |
| MenuSheet | `components/MenuSheet.tsx:71-86` | Items render `divider` at `position:'absolute', bottom:0` *inside* the Pressable. The divider extends outside the visual touch area and the conditional `index < length - 1` check is fine, but VoiceOver/TalkBack will announce the trailing arrow `→` as text. | Wrap the arrow in `<Text accessibilityElementsHidden={true} importantForAccessibility="no">` so screen readers don't read "right-arrow" after every menu item label. |
| Settings | `app/settings.tsx:69-72` | Disabled rows (name, ishta, bell time) are Pressable but `disabled={true}` — they read as buttons but do nothing. Screen reader users will tap and get no feedback. | Either swap to `<View>` (not Pressable) when disabled, or add `accessibilityHint="Edit not yet available"` so the user knows it's intentional. |
| Settings | `app/settings.tsx:109-121` | `SettingRow` accessibilityLabel is just the label string. For chevron rows it should announce as "About Smaran, button" — currently announces "About Smaran". Add the role. | Add `accessibilityRole={isInteractive ? 'button' : 'text'}` and `accessibilityHint` for nav rows: `"Opens about screen"`. |
| Verse Detail | `app/verse-detail.tsx:55-69` | Sanskrit text is rendered as `<Text>` with no `accessibilityLanguage="sa"` (or `lang="sa"`). TalkBack will pronounce Devanagari with English phonemes — sounds garbled to anyone listening. Same in onboarding (`स्मरण`), home (`deity.sanskritName`), about (`स्मरण`). | Add `accessibilityLanguage="sa"` (iOS) and on Android consider using Hindi (`hi-IN`) which has TTS support — Android does not ship Sanskrit TTS. Better still: provide an `accessibilityLabel="Sanskrit verse: chapter 2 verse 47"` with the *transliteration* spelled phonetically. |
| All screens with animations | `app/index.tsx:152, 170, 206, 220, 231, 259`; `onboarding`; `verse-detail` | Every entrance uses `FadeIn` / `FadeInDown` from `react-native-reanimated` with no check for `AccessibilityInfo.isReduceMotionEnabled()`. Users with reduce-motion enabled at OS level still get all the staggered fades. | At root, gate `Motion.base/.slow/.reverent` durations: ```const reduce = useReducedMotion(); const dur = reduce ? 0 : Motion.base;``` Reanimated 3.6+ ships `useReducedMotion`. Apply it once in `_layout.tsx` and pass through context, or replace bare `Motion.base` calls with a helper. |
| All screens | global | No `maxFontSizeMultiplier` is set anywhere. If a user bumps Android system font to 200%, the deity card title (`fontSize: 40`) becomes 80pt and breaks layouts (overflow on the deity card, button labels wrap, top bar wraps). | Decide a policy: either set `allowFontScaling={true}` with `maxFontSizeMultiplier={1.4}` on display text (Devanagari, deity names, hero) and `1.6` on body, or test the layouts at 200% and accept they need to scroll. The current uncontrolled state is the worst of both worlds. |
| Curator | `app/curator.tsx:135-153` | Pick/Replace/Remove buttons have **no accessibilityLabel**. Screen readers will read "Pick image" only because that's the visible text — fine — but disabled state during `isBusy` is not announced. | Add `accessibilityState={{ busy: isBusy, disabled: isBusy }}` and `accessibilityLabel={isBusy ? 'Picking image' : imagePath ? 'Replace image' : 'Pick image'}`. |
| Onboarding | `app/onboarding.tsx:97-107` | Name TextInput has no `accessibilityLabel`. Placeholder "Your name" is read but not as a label — when the field has text, screen readers read only the value. | Add `accessibilityLabel="Your name"`. Also add `textContentType="name"` for iOS autofill and `autoComplete="name"` for Android. |
| Saved | `app/saved.tsx:69-71` | Empty state is wrapped in `Animated.View` with no `accessibilityRole="text"`. The single-character "·" ornament gets read as "middle dot" by TalkBack — adds noise. | Wrap the ornament in `<Text accessibilityElementsHidden importantForAccessibility="no">·</Text>`. |
| Home | `app/index.tsx:264` | Ask Krishna pressable has `accessibilityLabel="Ask Krishna"` but the visible text is "Bring a question to Krishna →" — discrepancy means screen reader users hear something different from what sighted users see. | Set `accessibilityLabel="Bring a question to Krishna"` (drop the arrow). |
| Verse Detail | `app/verse-detail.tsx` whole screen | No semantic heading hierarchy. Section labels ("TRANSLATION", "REFLECTION", "SIT WITH THIS") are styled `<Text>` only. TalkBack users can't navigate by heading. | Add `accessibilityRole="header"` to each section label `<Text>`. |

---

## P2 Polish (nice-to-have)

| Screen | File:line | Issue | Fix |
|---|---|---|---|
| Theme | `constants/theme.ts:45-64` | `Fonts` defines `serif`, `rounded`, etc., but **none of the screens use Fonts**. Every screen relies on system default. The brand wants Cormorant Garamond–style serifs for Sanskrit/verse. | Either drop `Fonts` (it's dead code) or actually wire it through `TypeScale.verse` and `TypeScale.verseSanskrit`. The premium feel of a devotional app rests heavily on serif typography. |
| Tokens | `constants/design-tokens.ts:65-69` | `Motion.fast = 200`, `base = 400`, `slow = 600`. Layout uses `animationDuration: 400` in `_layout.tsx:59` (correct = `Motion.base`) and home uses `transition={400}` for the deity image (`index.tsx:180`) — both hardcoded, not tokenized. | Replace `400` literals with `Motion.base`. Greppable rule: `git grep -n "duration: [0-9]\|transition={[0-9]"` should yield zero hits in `app/`. |
| Spacing | `app/onboarding.tsx:195-333` | Onboarding hardcodes `padding: 24`, `marginTop: 40`, `paddingTop: 40`, `borderRadius: 8`, `borderRadius: 12`, `marginBottom: 32`, `gap: 12` instead of `Spacing.lg`, `Radius.md`, `Radius.lg`. The whole file ignores the design tokens. | Refactor to use `Spacing` + `Radius`. This is the single most polish-killing inconsistency in the codebase — the onboarding screen "feels different" from every other screen because it literally is. |
| Spacing | `app/index.tsx:291` | Menu button `gap: 5` — every other gap in the app is `Spacing.xs(4) | sm(8) | md(16)`. The one-off `5` is jarring. | Change to `gap: Spacing.xs` (4) or accept and add `Spacing.xxs = 5` to tokens. |
| Spacing | `app/index.tsx:293, :305, :350` | Repeated `fontSize` overrides on top of `TypeScale.caps` / `TypeScale.bodySmall`: `fontSize: 13` appearing twice, `fontSize: 12` for energyLine, `fontSize: 14` for streakNumber. Each override is a slight deviation from the type scale. | Either add explicit `TypeScale.label`, `TypeScale.tiny` tokens and use them, or accept the `caps` defaults. The current pattern of "use the token, then override one property" silently drifts. |
| Radii | `app/onboarding.tsx:273, :287, :312` | `borderRadius: 8` (input), `borderRadius: 12` (chip), `borderRadius: 8` (button). Same numbers as `Radius.md` and `Radius.lg` — should reference. | Use `Radius.md` / `Radius.lg`. |
| Press feedback | `app/index.tsx:235-256` | The 3 action buttons (heart, share, Read full) have no `({ pressed })` style — Android default ripple may or may not fire depending on Pressable internals. No `android_ripple={{ color: ... }}` set. | Add `android_ripple={{ color: 'rgba(201,169,97,0.2)', borderless: false }}` for tactile feedback that matches brand. The MenuSheet items also lack ripple. |
| Press feedback | `app/index.tsx:263-265` | Ask Krishna link has zero pressed state — no opacity change, no underline shift. Easy to miss the tap registered. | Add `style={({ pressed }) => [styles.askKrishnaContainer, pressed && { opacity: 0.6 }]}`. |
| Loading | `app/saved.tsx:23-41` | The screen renders the empty state for the first frame even when there *are* favorites, because `loaded` starts false and `favorites` is `[]`. The condition `loaded && favorites.length === 0` correctly prevents the empty flash, but if the user has 50 saved verses the FlatList renders unconditionally with empty data, then re-renders. | Render a tiny skeleton (3 fake card outlines at opacity 0.1) while `!loaded` — same animation budget, no blank-screen flash. |
| Loading | `app/curator.tsx:45-50` | `useEffect` loads images with no loading indicator. Screen renders instantly with all placeholders, then images flash in. | Show a single 1pt golden hairline progress at top of screen while loading, or accept the placeholder-first behavior as a feature. |
| Loading | `app/index.tsx:60-100` | Same issue — streak, name, deity image all load async with no loading state. Greeting flashes "Today is Friday" then "Rev · Today is Friday". | Either initialize state from a synchronous cache (`MMKV` pattern) or render the greeting only after `userName` resolves (with a 200ms delay before fallback). |
| Error | `app/curator.tsx:65-70` | `Alert.alert` is the only error path. No retry, no toast, no haptic. | Add `Haptics.notificationAsync(Haptics.NotificationFeedbackType.Error)` before the Alert. |
| Error | `app/_layout.tsx:43-47` | Silent catch on SecureStore failure — user never knows onboarding state was unreadable. | Log to Sentry/Crashlytics. For Play Store launch the silent fall-through is acceptable but should be observable. |
| Empty | `app/saved.tsx:68-72` | Empty state has no CTA — user is told "you can save here" but there's no link back to today's verse. | Add a small `Pressable` below the ornament: "Open today's verse →" that pops back to root. |
| Keyboard | `app/onboarding.tsx:67-72` | `KeyboardAvoidingView` uses `behavior: 'padding'` for iOS only, **undefined for Android**. On Android, when the keyboard opens for the name input, the "Continue" button hides under it. | Use `behavior={Platform.OS === 'ios' ? 'padding' : 'height'}` and add `keyboardVerticalOffset={20}`. Test on a small Android (5"). |
| Keyboard | `app/onboarding.tsx:97-107` | Name input `autoFocus` fires before the entrance animation finishes, causing the keyboard to fight the layout. | Delay autoFocus by 400ms (= `Motion.base`) using `useEffect` + `setTimeout` + `inputRef.current?.focus()`. |
| Icons | `app/index.tsx:185, :199, :191`; `verse-detail.tsx`; `ask-krishna.tsx:108`; `about.tsx:174`; `saved.tsx:70`; `curator.tsx` | The "✦", "·", "🪔", "→", "←" used as icons are emoji/text glyphs at varying sizes (`fontSize: 16, 18, 24, 40, 48`). Weights vary unpredictably. The 🪔 lamp emoji renders system-style — different on Pixel vs Samsung vs OnePlus. | Move to vector icons (Phosphor or Iconoir thin-weight set, sized to one consistent scale, e.g. 16/20/24). Keep ✦ as a brand glyph but ship it via SVG at fixed sizes. |
| Animation curves | `app/index.tsx:170-171` | Uses `springify().damping(20)` on home deity card and `damping(20)` on menu sheet. But streak pulse uses `withSpring(..., { damping: 6, stiffness: 80 })`. Three different spring profiles. | Centralize as `Motion.spring.gentle`, `Motion.spring.bouncy` in `design-tokens.ts`. |
| Status bar | `app/_layout.tsx:73` | `StatusBar style="light"` is set globally but verse-detail and verse-detail's white background flashes (none here, but if a future light surface ships, status bar will be wrong). | Move `StatusBar` per-screen via `<StatusBar style="light" />` so future screens can override. |
| Header pattern | `app/curator.tsx:94-100` vs `verse-detail.tsx:38-42` vs `about.tsx:28-32` | Three different top-bar patterns: curator has back+title+spacer (3-element row), verse-detail has just back (1-element), about has just back. Inconsistent navigation chrome. | Pick one (curator pattern) and use everywhere. Title should always be present even if just the screen name. |
| Card pattern | `app/saved.tsx:123-131` vs `app/ask-krishna.tsx:160-168` | Both use `rgba(245,239,227,0.04)` background + gold border but different `borderColor` opacities (`0.12` vs `0.15`) and different padding (`Spacing.lg` both — ok). | Extract a `<Card>` component with one shape. |

---

## Color Contrast Actual Calculations

All ratios computed using WCAG 2.1 luminance formula (sRGB linearization). AA threshold: 4.5:1 body, 3:1 large text (≥18pt or ≥14pt bold).

| Foreground | Background | Ratio | Pass? | Note |
|---|---|---|---|---|
| `#C9A961` sacred gold | `#1A1B3A` indigo | **7.40:1** | ✓ AAA | Headlines, accents, deity Sanskrit, primary button text-on-button-bg |
| `#F5EFE3` ivory | `#1A1B3A` indigo | **14.53:1** | ✓ AAA | Default body text, headings, greetings |
| `#9A9A9A` soft gray | `#1A1B3A` indigo | **5.91:1** | ✓ AA | Secondary text, captions, transliteration, dates |
| `#A62024` sindoor red | `#1A1B3A` indigo | **2.26:1** | ✗ FAIL | Used for active heart icon — fails AA outright |
| `#6B8E5C` sage green | `#1A1B3A` indigo | **4.48:1** | ✓ AA (just) | Tier marker, calm states — ok for body but on the edge |
| `#2C2C2C` charcoal | `#F5EFE3` ivory | **12.20:1** | ✓ AAA | Light-mode body (light mode never rendered in current build) |
| `#1A1B3A` indigo | `#C9A961` gold | **7.40:1** | ✓ AAA | Primary button text on gold button — same ratio inverted |
| `#A62024` sindoor | `#F5EFE3` ivory | **6.43:1** | ✓ AA | Sindoor on ivory (use this combination, not on indigo) |
| Ivory @ 0.85 → `#D4CFCA` | `#1A1B3A` indigo | **10.76:1** | ✓ | Body text in tagline / about / promise blocks |
| Ivory @ 0.90 → `#DFDAD2` | `#1A1B3A` indigo | **11.96:1** | ✓ | About body |
| Ivory @ 0.80 → `#C9C5C1` | `#1A1B3A` indigo | **9.70:1** | ✓ | ask-krishna tagline, saved empty text |
| Ivory @ 0.75 → `#BEBAB9` | `#1A1B3A` indigo | **8.65:1** | ✓ | Onboarding subtext |
| Ivory @ 0.70 → `#B3AFB0` | `#1A1B3A` indigo | **7.67:1** | ✓ | Verse-detail commentary placeholder |
| Ivory @ 0.40 → `#72707E` | `#1A1B3A` indigo | **3.44:1** | ✗ for body, ✓ for large only | "deity art coming" placeholder (decorative, ok) |
| Gold @ 0.90 → `#B89B5D` | `#1A1B3A` indigo | **6.25:1** | ✓ | Welcome point bullets |
| Gold @ 0.85 → `#AF945B` | `#1A1B3A` indigo | **5.71:1** | ✓ | Ask Krishna link |
| Gold @ 0.60 → `#837051` | `#1A1B3A` indigo | **3.49:1** | ✗ body / ✓ large | MenuSheet arrow `→` (16pt → fail), divider (decorative → ok) |
| Gold @ 0.50 → `#72624E` | `#1A1B3A` indigo | **2.83:1** | ✗ FAIL | Settings chevron `→` — meaningful affordance, fails AA |
| Soft gray @ 0.70 → `#74747D` | `#1A1B3A` indigo | **3.60:1** | ✗ body / ✓ large | Curator footer note (14pt → fail) |
| Soft gray @ 0.60 → `#676774` | `#1A1B3A` indigo | **2.99:1** | ✗ FAIL | About version label — fail |
| Soft gray @ 0.50 → `#5A5A6A` | `#1A1B3A` indigo | **2.46:1** | ✗ FAIL | Settings version block — fail |
| Gold @ 0.12 (streak bg) → `#2F2C3F` | `#1A1B3A` parent | n/a | n/a | Streak badge background |
| `#C9A961` gold | `#2F2C3F` streak bg | **6.01:1** | ✓ AA | Streak number on streak pill |
| `#F5EFE3` ivory | `#232341` saved card bg | **13.21:1** | ✓ AAA | Saved-card body |
| `#C9A961` gold | `#232341` saved card bg | **6.72:1** | ✓ AA | "BG 2.47" reference label |
| `#A62024` sindoor | `#361C36` heart-active bg | **2.07:1** | ✗ FAIL | Active heart icon — invisible to many users |
| `#9A9A9A` soft gray placeholder | `#22243C` input bg (ivory @ 0.04) | **5.38:1** | ✓ AA | Onboarding name placeholder |

**Pattern**: every contrast failure in this app comes from one of two anti-patterns:
1. **Wrapping a soft-gray text in `opacity: 0.5–0.7`** to dim it further. Soft gray is already at the AA edge; one more multiplier kills it.
2. **Sindoor red on dark indigo** — the brand color simply doesn't have enough luminance for dark surfaces. Use sindoor on ivory; on indigo, use gold.

---

## Top 10 polish issues that most hurt premium feel

Ranked by visible impact on a first-time user opening the app on a mid-range Pixel.

1. **Onboarding ignores design tokens** (`onboarding.tsx`). Hardcoded `24, 40, 32, 12, 8` everywhere. The screen the user sees first feels visually different from the rest of the app. Single biggest "this is built to spec" deficit.
2. **Heart "saved" state is invisible** (`index.tsx:373`). Sindoor-on-sindoor-tinted-indigo. The most-used micro-interaction in the app gives the weakest visual confirmation. Premium apps over-confirm; Smaran under-confirms.
3. **Three different top-bar patterns** across screens. Curator has back+title+spacer, ask-krishna has just back, about has just back, settings has back+title+spacer. Looks inconsistent in side-by-side screenshots.
4. **System fonts only** — `Fonts` token defined but never wired. A devotional app charging ₹79+ should ship with a Cormorant Garamond–style serif for verses. The current default-system-font rendering looks like a Lovable prototype.
5. **Emoji as icons** (🪔, ✦, →, ←). Renders differently on every Android skin. The lamp emoji is the **literal closing image of multiple screens** and it looks like Discord, not Apple-tier.
6. **Loading flashes everywhere**. Home shows "Today is Friday" → "Rev · Today is Friday" → image fades in. Saved shows empty state → cards. Settings shows "—" → values. None of these are skeletons; they're flashes of incorrect content.
7. **Animation drift**. Three spring configurations, two hardcoded `400ms` durations alongside `Motion.base`, deity card uses `damping(20)` and streak uses `damping(6)` — all in the same file. The result is motion that feels uncalibrated.
8. **No Reduce Motion support**. Users with vestibular sensitivity get the full FadeInDown stagger on every screen open. Especially uncomfortable on the deity card's `springify`.
9. **Disabled Settings rows are misleading** (`settings.tsx:69-72`). Name, Ishta, Bell time look tappable, are tappable, do nothing. Either make them edit-able or visually demote them (no chevron, label color softer).
10. **MenuSheet arrows are noisy**. Each item ends in a gold `→` at opacity 0.6. Four items, four arrows. With the prominent "✦" prefix on Ask Krishna, the row reads as `✦ Ask Krishna →` — three glyphs around two words. Premium apps use one ornament per row, not two.

---

## What's done well

1. **Color palette has unusually strong base contrast.** Ivory-on-indigo at 14.5:1 and gold-on-indigo at 7.4:1 mean the app is *naturally* compliant on its primary text — which is why the failures stand out so cleanly. Most app palettes start at 5-6:1 and degrade from there.
2. **`hitSlop={12}` everywhere on small targets.** Back buttons, menu buttons, action chips — all have hit-slop expanding the touch area to 56pt+. This is a pattern many apps forget. The visual size is sometimes wrong (P1 above), but the touch target is forgiving.
3. **Graceful SecureStore failure handling** (`_layout.tsx:43-47`, `index.tsx:76-92`). Every async storage call is wrapped, with sensible defaults if it fails. The user never gets trapped on a blank screen.
4. **Design-token system exists and is thoughtful** (`design-tokens.ts`). `Motion.reverent = 1000` for sacred moments is a beautiful detail. `Layout.minTapTarget = 44` is in the tokens. The system is here — it just isn't enforced everywhere.
5. **Haptics on every interactive Pressable** (`Haptics.selectionAsync()` before navigation, before save, before share). Almost every tap has feedback. This is the #1 thing that makes the app *feel* premium even when other details slip.

---

## Suggested fix order for one developer day

**Morning (3 hours, ship-blocker P0s):**
- Add `accessibilityRole="button"` to all 18 Pressables (30 min, find-replace)
- Fix the 3 contrast failures: settings version block, about version label, curator footer (15 min)
- Fix the heart active state: change icon color to gold when active, or move to ivory background (45 min)
- Add `accessibilityLabel`s to onboarding deity chips, skip buttons, primary button (45 min)
- Add `useReducedMotion()` gate to `Motion` durations centrally (45 min)

**Afternoon (4 hours, top P1+P2 polish):**
- Refactor onboarding to use `Spacing` + `Radius` tokens (90 min)
- Centralize top-bar component, replace in 4 screens (60 min)
- Wire up serif font for `TypeScale.verse` and `TypeScale.verseSanskrit` via `expo-font` (45 min)
- Replace emoji icons with @phosphor-icons/react-native thin-weight set (60 min)
- Add `accessibilityLanguage="sa"` (or hi-IN fallback) to all Devanagari `<Text>` (15 min)

That gets the app from "AA conditional pass with rough edges" to "AA pass with premium polish" in one focused day.
