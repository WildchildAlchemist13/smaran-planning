# Smaran — Pre-Ship Architecture Audit

## Verdict
**SHIP-WITH-FIXES.** The codebase is clean, well-typed, and architecturally sound for a v0.1 MVP. No crash-class defects on the happy path, but **3 ship-blockers** will produce confusing failures on a meaningful slice of real Indian Android devices:

1. The streak `daysBetween` calculation uses UTC parsing of a local date string → streaks silently break across DST/timezone boundaries
2. `app.json` declares neither the `expo-image-picker` plugin nor any Android permission strings → gallery access on Android 13+ may be denied; Play Console data-safety review will flag the mismatch
3. `expo-notifications` is not added as a config plugin and there is no `POST_NOTIFICATIONS` declaration → Android 13+ notification permission flow can fail or display the system prompt at the wrong time

None large fixes (~2 hours collectively). Without them: broken streak counts, "gallery picker does nothing" complaints from Pixel/OnePlus 13+ users, Play Console reviewer flagging missing permissions.

## TypeScript & Doctor Status
- `tsc --noEmit`: **PASS** (strict mode on, typed routes on)
- `expo-doctor`: **PASS** (17/17 checks passed)
- `expo lint`: 5 cosmetic errors (unescaped quotes), 1 warning (unused `err` binding `app/index.tsx:76`)

## Critical Issues (must fix before Play Store)

| File:line | Issue | Risk | Fix |
|---|---|---|---|
| `lib/storage.ts:64-69` (`daysBetween`) | `new Date('2026-05-15')` parses as UTC midnight. Local-date strings get shifted to UTC on parse → streak resets near midnight or DST boundaries | **Critical** — streak is the #1 retention mechanic | Compute days from Y-M-D components: `const [y,m,d] = date.split('-').map(Number); Date.UTC(y, m-1, d)`. Or compare strings directly. |
| `app.json:30-46` (plugins) | Missing `expo-image-picker`, `expo-notifications`, `expo-file-system` config plugins. On Android 13+: gallery picker may fail silently, morning bell won't survive reboot | **Critical** | Add: `["expo-image-picker", { "photosPermission": "Smaran uses your gallery only to let you pick deity art for your home screen. Images stay on your device." }]`, `["expo-notifications", { "color": "#C9A961" }]`. Rebuild required. |
| `lib/deityImages.ts:84-125` (`pickAndSaveDeityImage`) | `new File(sourceUri)` expects `file://` but Android picker returns `content://` on API 30+ | **Deferred Critical** for v0.1 (curator hidden, only Rev uses it). **Tag TODO** before exposing to users in v0.2. | Use `fetch(sourceUri).blob()` or legacy `copyAsync` from expo-file-system. Test on real Android with content-URI source before v0.2. |
| `app/index.tsx:60-100` (mount effect) | `previousStreak = streak` reads `0` (initial state) → `streakState.count > previousStreak && previousStreak > 0` guard never fires → spring pulse animation is dead code | **Medium-High** — celebratory pulse never plays | Read previous streak from storage before calling `updateStreakOnAppOpen`, or have function return both old+new counts. |
| `app/_layout.tsx:35-50` | Initial route check happens after first render → one frame flash of `index.tsx` before `router.replace('/onboarding')` on fresh install | **Medium** — visual flash | Gate `<Stack>` render on `checked` state, or use `SplashScreen.preventAutoHideAsync()` + `hideAsync()` in finally. |
| `lib/notifications.ts:31-37` | If permission was previously denied, `requestPermissionsAsync` returns denied without re-prompting (Android). Onboarding screen says "Enable morning bell" with no feedback that it failed | **High** | After request, if status `denied`, show: "Notifications are off in system settings — you can enable them in Settings → Apps → Smaran." |
| `lib/storage.ts` (favorites) | SecureStore Android has ~2KB recommended cap. Power user with 100+ favorites silently fails to save | **Medium** v0.1, **High** v0.5 | Move to `expo-file-system` JSON or `expo-sqlite`. Defer for v0.1. |

## High Priority

| File:line | Issue | Impact | Fix |
|---|---|---|---|
| `app/index.tsx` (handleToggleSave, handleShare) | Errors swallowed silently; no error toast | UX confusion | Try/catch with optimistic UI + revert on failure |
| Haptics calls (12+ sites) | Not awaited, not try/caught — throws unhandled rejection on devices without haptics engine | Crash potential on budget Vivo/Realme | Wrap all `Haptics.selectionAsync()` with `.catch(() => {})` or `safeHaptic()` helper |
| `app/onboarding.tsx:54-59` (`handleEnableNotifications`) | If `requestNotificationPermissions` throws (rare on MIUI weird states), screen freezes | High on Xiaomi | Wrap in try/finally; always call `finishOnboarding()` |
| `lib/notifications.ts:65-91` | 7 weekly notifications scheduled. On Xiaomi/MIUI, killed when app force-closed by Security/battery optimization. **No re-schedule logic** | **High** for India — Xiaomi ~25% market share | In `app/index.tsx` mount: `if ((await getScheduledNotifications()).length < 7 && permission granted) await scheduleDailyDarshanNotification()` |
| `components/MenuSheet.tsx:43` | Bare `setTimeout(() => router.push, 80)` with no cleanup | Lingering reference | Track timer in ref, clear on unmount |
| `lib/share.ts:45` (catch handler) | `console.error` only — silent failure if share extension corrupt | UX confusion | `Alert.alert('Could not open share sheet', 'Please try again.')` |
| `app/_layout.tsx` (no error boundary) | Single uncaught render exception kills app to white screen | Catastrophic | Top-level `<ErrorBoundary>` with "Something went wrong — pull to retry" + `Updates.reloadAsync()` |

## Medium Priority

| File:line | Issue | Fix |
|---|---|---|
| `app/onboarding.tsx:107` `<TextInput autoFocus>` | Keyboard pops before mount transition completes → layout shift | Defer via `setTimeout(() => ref.focus(), 250)` |
| `app/onboarding.tsx:127-141` (deity grid 30%/gap 12) | On <360dp Android (older Samsung J-series, ~3-5% IN market), 7 chips wrap into messy rows | Use `width: '31%'` with calculated gap, or 2-column on small screens |
| `lib/storage.ts:155-158` (`resetAllStorage`) | Doesn't clear deity image keys or delete saved image files | Iterate full prefix, also delete files in document directory |
| `app/about.tsx:16` & `app/settings.tsx:23` | `APP_VERSION = '0.1.0'` hardcoded twice → drift risk | Read from `Constants.expoConfig?.version` |
| `app.json` Android | No `softwareKeyboardLayoutMode` declared | `windowSoftInputMode: adjustResize` (Expo default — verify) |

## Specific Concern: First-Launch Behavior

**Crash points on first launch: zero on the happy path.** Failure modes handled with try/catch fallbacks. Only real concern: brief one-frame flash of home before onboarding redirects (fix by gating render on `checked` state).

## Specific Concern: Notification Reliability on Indian Android Phones

Indian distribution dominated by Xiaomi (MIUI), Samsung (One UI), Vivo (Funtouch), Realme (Realme UI), OnePlus (OxygenOS). All five aggressively kill background apps and de-schedule local notifications:

- **Xiaomi (MIUI 14+)**: Auto-start permission **off by default**. Without it, scheduled notifications killed within hours of last app open. Security app force-stops apps with "high battery use." **Mitigation**: in onboarding step 3, after scheduling, show one-time tip about MIUI auto-start. Recommended for v0.2.
- **OnePlus / Oppo / Realme (OxygenOS / ColorOS)**: Default battery optimization kills scheduled notifications after ~2 days of no app open. **Mitigation**: re-schedule on app open (single most important fix for OEM reliability).
- **Vivo (Funtouch)**: Similar to MIUI. Same mitigation.
- **Samsung (One UI 6+)**: Generally reliable for `WEEKLY` triggers. Lowest risk.
- **Pixel (stock Android)**: Reliable. Lowest risk.

**Universal mitigations to add:**
1. Re-schedule on every app open if `getScheduledNotifications().length < 7` (most impactful single fix)
2. Add `BOOT_COMPLETED` permission via `expo-notifications` config plugin so notifications survive reboots (currently missing)
3. Keep Android channel at `AndroidImportance.DEFAULT` (HIGH would heads-up — fights "quiet bell" brand)

**Realistic expectation**: even with all mitigations, expect ~70% bell delivery rate on Xiaomi/Vivo without manual auto-start enablement. Build a one-screen "if your bell goes silent, here's why" help screen for v0.2.

## Specific Concern: Sharing Failure Modes

- WhatsApp not installed → share sheet still opens with available apps
- User cancels via backdrop → Android often returns `sharedAction` even on cancel (unreliable). You don't use the return value, so doesn't matter.
- Emoji 🪔 in shared text → all modern Android share targets handle UTF-8 fine
- Special chars in user's name → plain text share, no escaping needed
- One miss: if share extension genuinely fails (corrupt custom ROM), `console.error` only → user taps share button, nothing happens. Add `Alert.alert` in catch.

## Top 10 Fixes Ordered by Risk × Effort

1. **Fix `daysBetween` UTC parsing** in `lib/storage.ts:64-69`. ~10 min. Replace `new Date(date1)` with Y-M-D component parsing.
2. **Add config plugins to `app.json:30`** for `expo-image-picker` and `expo-notifications` with permission strings. ~5 min + rebuild.
3. **Re-schedule notifications on app open** in `app/index.tsx` mount. ~15 min. Single biggest fix for OEM bell reliability.
4. **Fix dead streak-pulse animation** in `app/index.tsx:60-100`. ~10 min.
5. **Wrap all `Haptics.selectionAsync()` calls in `.catch(() => {})`** or `safeHaptic()` helper. ~15 min.
6. **Add top-level `<ErrorBoundary>`** in `app/_layout.tsx`. ~20 min. Biggest insurance policy.
7. **Gate `<Stack>` render in `_layout.tsx`** on `checked` state. Use `SplashScreen.preventAutoHideAsync()` + `hideAsync()`. ~15 min.
8. **Show feedback on notification permission denial** in `app/onboarding.tsx:54-60`. ~10 min.
9. **Hide curator from production build.** Wrap in `if (__DEV__ || Constants.expoConfig?.extra?.showCurator)`. ~5 min.
10. **Read `APP_VERSION` from `expo-constants`** in `app/about.tsx:16` and `app/settings.tsx:23`. ~5 min.

**Total time for top 10: ~2 hours.** All low-risk edits, no architectural change required. Ship after these to internal testing.

## Critical Files for Implementation

- `/Users/revanthsharma/Claude Code Desktop/Smaran-app/lib/storage.ts`
- `/Users/revanthsharma/Claude Code Desktop/Smaran-app/app.json`
- `/Users/revanthsharma/Claude Code Desktop/Smaran-app/lib/notifications.ts`
- `/Users/revanthsharma/Claude Code Desktop/Smaran-app/app/index.tsx`
- `/Users/revanthsharma/Claude Code Desktop/Smaran-app/app/_layout.tsx`
