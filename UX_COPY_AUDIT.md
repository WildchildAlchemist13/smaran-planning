# Smaran — UX Copy Audit

> Audit date: 2026-05-15. Scope: all user-facing strings in `Smaran-app/` v0.1.0. Reference: `BRAND.md` Section 7 (Voice + Tone) and Section 9 (Things to Never Do).

---

## Overall Voice Score: **6.5 / 10**

The foundation is strong — most copy is calm, concise, and reverent in spirit. Onboarding and About pages are close to brand-correct. **However**, the app has three systemic violations that a single afternoon of edits would resolve and that, left untouched, will quietly leak brand authority every time a user encounters them:

1. **The word "blessing"** appears in primary copy (home share button, share text, share sheet title) — a direct hit on the BRAND.md "Christianity-flavored language" anti-pattern. There is no Sanskrit-rooted swap currently used in product. Fix once, fix everywhere.
2. **The notification title `"Good morning ☀"`** uses an emoji where the brand explicitly only allows 🪔 (and even that "occasionally"). The sun emoji also reads as generic-cheery, not reverent.
3. **Multiple "coming soon" labels** read corporate/marketing rather than reverent. "COMING SOON" in caps is a startup-landing-page tic, not a devotional voice.

Beyond those three, the copy is generally good — but a dozen small sharpenings (listed below) would push the voice from "appropriate" to "ownable." The Ask Krishna sample exchange and the onboarding notification screen are already exemplary — that's the bar to bring everything else up to.

---

## Per-Screen Audit

### Screen: `app/index.tsx` — Home / Daily Darshan

| Current | Issue | Suggested |
|---|---|---|
| `accessibilityLabel="Open menu"` (line 156) | Fine, keep. | `"Open menu"` |
| `accessibilityLabel={saved ? 'Unsave verse' : 'Save verse'}` (line 238) | "Unsave" is mildly tech-flavored. | `saved ? 'Remove from saved' : 'Save this verse'` |
| `accessibilityLabel="Share blessing"` (line 245) | **"Blessing" violates BRAND.md §9.** Christianity-flavored. | `"Share this verse"` |
| `accessibilityLabel="Read full verse and commentary"` (line 251) | Fine. Slightly long; OK for a11y. | Keep, or trim to `"Read the full verse"`. |
| `accessibilityLabel="Ask Krishna"` (line 263) | Fine. | Keep. |
| `Today is {deity.weekdayName}` greeting small (line 211) | Reads well. | Keep. |
| `{deity.name} {deity.honorific}'s day` (lines 213-215) | Reverent and specific. | Keep. |
| `· {deity.energy} ·` (line 216) | `energy` values are single English words — `clarity / stillness / courage / beginning / preservation / abundance / discipline`. "Energy" itself is a Western pop-spirituality giveaway. | Change the design comment / label nothing visible — the bullet ornament is enough. The single word in caps reads fine on its own. (Underlying field: rename `energy` → `quality` in `deityCalendar.ts`. See "Anti-patterns" section below.) |
| `Bhagavad Gita {verse.chapter}.{verse.verse}` (line 225) | Fine. Could shorten in this context. | Keep on home; consider abbreviating to `BG {chapter}.{verse}` only on the saved-verses card (where it already is). |
| `&quot;{verse.oneLine}&quot;` (line 227) | Fine — quoted one-liner. | Keep. |
| `Read full →` button label (line 254) | Slightly abrupt. The "→" is doing the work. | `Read this verse →` (4 chars longer, sharper meaning) |
| `Bring a question to Krishna  →` (line 264) | This is the best line on the home screen. Has two spaces before arrow — likely a typo. | `Bring a question to Krishna →` (single space) |
| `· deity art coming ·` placeholder hint (line 196) | Fine — quiet and self-aware. | Keep. |

---

### Screen: `app/onboarding.tsx` — 4-step welcome

| Current | Issue | Suggested |
|---|---|---|
| `Smaran` title (line 76) | Fine. | Keep. |
| `Daily darshan + Bhagavad Gita.\nA 30-second practice of remembrance.` (line 77-79) | "+" reads slightly product-spec-y; everything else is good. | `Daily darshan. Bhagavad Gita.\nA 30-second practice of remembrance.` |
| `· One deity each morning` (line 82) | Fine. | Keep. |
| `· One verse from the Gita` (line 83) | Fine. | Keep. |
| `· A streak that grows quietly` (line 84) | Excellent — already brand-perfect ("quietly" is the move). | Keep. |
| `Begin` button (line 86) | Slightly bare. "Begin" alone is fine; "Begin your practice" would be too on-the-nose. | Keep `Begin`. |
| `· Step 1 of 3 ·` (line 92) | Fine. | Keep. |
| `What may we call you?` (line 93) | Slightly antique. "May we" is a touch Victorian; "what shall we call you" is closer to Indian-English warmth. | `What shall we call you?` |
| `Your name will appear in your daily greeting. Used only on this device.` (line 95) | Two short sentences; functional. The second is reassuring. Could be one line. | `Used only in your morning greeting. Stored only on this device.` |
| `Your name` placeholder (line 99) | Fine. | Keep. |
| `Continue` button (line 110) | Fine. | Keep. |
| `Skip` text (line 115) | Slightly utilitarian for this voice. | `Skip for now` (warmer, signals "not forever") |
| `· Step 2 of 3 ·` (line 122) | Fine. | Keep. |
| `Choose your ishta-devata` (line 123) | Strong — uses Sanskrit term correctly. Should italicize per BRAND.md §7 ("Always italicize Sanskrit transliteration"). | `Choose your *ishta-devata*` (render with italic style on `ishta-devata`). |
| `The form of the divine you feel closest to. (You can change this anytime.)` (line 124) | Good. The parenthetical can stand alone for breathing room. | `The form of the divine you feel closest to.\nYou can change this anytime.` |
| `Continue` (line 145) | Fine. | Keep. |
| `Skip` (line 149) | Same as above. | `Skip for now` |
| `· Step 3 of 3 ·` (line 156) | Fine. | Keep. |
| `One quiet bell at sunrise` (line 157) | Already excellent — best heading in the app. | Keep. Frame it. |
| `Smaran sends a single, calm notification at 7 a.m. each morning — naming the day's deity and inviting you to your practice. No other notifications. Ever.` (line 158-161) | Strong. "inviting you to your practice" is slightly soft; "naming the day's deity" is the better half. | `One calm bell at 7 a.m. — naming the day's deity, opening your practice. No other notifications. Ever.` (tightens, removes redundancy with the heading, keeps the "Ever." landing) |
| `Enable morning bell` button (line 163) | Good — uses the brand metaphor. | Keep. |
| `Not now` skip text (line 165) | Fine. | Keep. |

---

### Screen: `app/ask-krishna.tsx` — Coming-soon placeholder

| Current | Issue | Suggested |
|---|---|---|
| `Ask Krishna` title (line 49) | Fine — this is the product name for this surface. | Keep. |
| `Bring your real life questions\nto the wisdom of the Gita.` (line 56) | "Real life" reads slightly self-help-y. Sharper to lean into the Gita-as-counsel framing. | `Bring the questions you carry\nto the wisdom of the Gita.` |
| `COMING SOON` (line 62) | **Anti-pattern.** Reads startup-landing-page, not devotional. | `· ARRIVING SOON ·` or, better, drop the label entirely and let the body copy carry the message. If kept, lowercase: `arriving soon`. |
| `Soon, you will be able to bring any question you carry — about anxiety, work, grief, love, purpose — and receive a response rooted in the Bhagavad Gita's wisdom.` (line 67-70) | Strong. "the Bhagavad Gita's wisdom" is slightly redundant after "rooted in." | `Soon, you'll bring any question you carry — anxiety, work, grief, love, purpose — and receive a response rooted in the Bhagavad Gita.` |
| `Every response will cite a real verse. Every response will speak from the text, never as the deity. Every response will honor the tradition.` (line 71-74) | Beautiful triple anaphora. Keep. | Keep. |
| `We're building this carefully because it deserves to be built carefully.` (line 75-77) | Strong, mature voice. | Keep. |
| `· EXAMPLE ·` label (line 82) | Fine. | Keep. |
| `"I'm anxious about a presentation tomorrow."` (line 84) | Fine — believable user prompt. | Keep. |
| `Your anxiety comes from a familiar place — wanting to control how others receive you. In Chapter 2, Verse 47, you are reminded:` (line 87-90) | Strong. "you are reminded" is perfectly reverent passive. | Keep. |
| `कर्मण्येवाधिकारस्ते मा फलेषु कदाचन` (line 92) | Sanskrit verse — should be followed by italic transliteration per BRAND.md §7 ("Always provide translation alongside untranslated Sanskrit"). | Add italic line: *karmaṇy-evādhikāras te mā phaleṣu kadācana* |
| `You have a right to action — never to its fruits. The presentation is your action. How they receive it is the fruit. The moment you release the second, the first becomes lighter.` (line 93-97) | Excellent — already brand-voice. | Keep. |
| `What would change if you gave only the first your full attention?` (line 99) | Excellent closing. | Keep. |
| `🪔` (line 108) | OK per BRAND.md §7 (occasional 🪔 acceptable in marketing/notifications). Use sparingly elsewhere. | Keep here. |

---

### Screen: `app/curator.tsx` — Admin / image manager

> Note: per the file header, this screen is hidden from production users before public launch. Voice still matters because Rev sees it daily during content management.

| Current | Issue | Suggested |
|---|---|---|
| `MANAGE DEITY ART` title (line 98) | Fine for admin. | Keep. |
| `Pick a beautiful image for each deity. The home screen will show your chosen image.` (line 104-106) | Functional. "Beautiful" carries weight. | `Choose an image for each deity. The home screen will show what you choose.` |
| `Recommended: classical Raja Ravi Varma paintings (public domain, search Wikipedia). Aspect 3:4 portrait works best.` (line 107-110) | Fine for admin. | Keep. |
| `'Picking…' / 'Replace' / 'Pick image'` (line 142) | Fine, functional. | Keep. |
| `Remove` button (line 150) | Fine. | Keep. |
| `Could not set image` Alert title (line 67) | Fine. | Keep. |
| `Remove ${name} image?` Alert title (line 76) | Fine — direct. | Keep. |
| `The card will return to the placeholder.` Alert body (line 77) | Fine. | Keep. |
| `Cancel` / `Remove` Alert buttons (line 79, 81) | Fine. | Keep. |
| `Images are stored only on this device. When cloud sync arrives, your images will sync to Smaran across devices.` (line 159-161) | Slightly long; "your images will sync to Smaran" is wordy. | `Images live only on this device. When cloud sync arrives, they'll travel with you.` |

---

### Screen: `app/settings.tsx` — Settings

| Current | Issue | Suggested |
|---|---|---|
| `SETTINGS` title (line 63) | Fine. | Keep. |
| `Your name` row label (line 69) | Fine. | Keep. |
| `Ishta-devata` row label (line 70) | Should italicize Sanskrit per BRAND.md. (`Ishta-devata` is Sanskrit-derived term used in English context.) | `Ishta-devata` (apply italic style — UI permitting). If italics aren't viable in row labels, accept as-is. |
| `Morning bell` row label (line 71) | Fine — uses brand metaphor. | Keep. |
| `Bell time` row label (line 72) | Fine. | Keep. |
| `On` value (line 71) | Fine. | Keep. |
| `7:00 AM` value (line 72) | Fine. Could read `7:00 a.m.` to match BRAND.md style elsewhere ("at 7 a.m. each morning"). | `7:00 a.m.` (consistency with onboarding copy) |
| `About Smaran` row (line 76) | Fine. | Keep. |
| `Privacy policy` row (line 77) | Fine. | Keep. |
| `Terms of service` row (line 78) | Fine. | Keep. |
| `Send feedback` row (line 79) | Slightly utilitarian for this voice. | `Write to us` (warmer; matches "small founder" feel) |
| `Manage deity art` row (line 84) | Fine for admin row. | Keep. |
| `Smaran v{APP_VERSION}` (line 89) | Fine. | Keep. |
| `Early access · with reverence` (line 90) | Strong. The "· with reverence" is a brand signature. | Keep. |
| Email subject `Smaran feedback` (line 22) | Fine. | Keep — or `A note from a Smaran user` for warmth. |

---

### Screen: `app/about.tsx` — About / Credits

> Already very close to brand-perfect. Light edits only.

| Current | Issue | Suggested |
|---|---|---|
| `A daily practice of remembrance.` tagline (line 39) | Good. | Keep. |
| `ABOUT` section title (line 44) | Fine. | Keep. |
| `Smaran (smah-RUN, स्मरण) is the Sanskrit word for "remembrance"...` (line 46-48) | Excellent — pronunciation guide is the right move. | Keep. |
| `Smaran is a daily practice. Every morning at sunrise, you receive one quiet bell. You open the app. The deity of the day greets you. You read one verse from the Gita. You sit with it for thirty seconds. You close the app.` (line 49-53) | Excellent — staccato sentences mirror the ritual. Brand-voice exemplar. | Keep. |
| `That's the whole practice.` (line 54) | Excellent landing. | Keep. |
| `Over 30 days, the practice becomes a thread. Over 100 days, it becomes part of who you are.` (line 56-58) | Excellent. | Keep. |
| `SANSKRIT RECITATIONS` section (line 61) | Fine. | Keep. |
| `Every Sanskrit chant in Smaran is recorded by my mother — the same way she taught me to say them as a child.` (line 62-65) | Beautiful, personal. The "my" is a deliberate founder voice — keep. | Keep. |
| `SCHOLAR ADVISOR` section (line 68) | Fine. | Keep. |
| `Verse commentary is written in the mainstream Vedanta tradition — universalist, accessible, rooted in the Gita's text. Scholar credit added when full commentary ships.` (line 69-73) | Strong. "Ships" reads slightly tech-flavored at the very end. | `...rooted in the Gita's text. Full credits added when the commentary arrives.` |
| `ART` section (line 76) | Fine. | Keep. |
| `Deity art is being commissioned from Indian devotional artists. We do not use AI-generated faces of deities. Artist credits added as art arrives.` (line 77-80) | Strong, principled. | Keep. |
| `PRIVACY` section (line 83) | Fine. | Keep. |
| `Smaran does not collect, store, or transmit any of your personal data to any server. Everything you create lives only on your device. There are no ads. There is no tracking. Ever.` (line 84-87) | Excellent — the "Ever." landing is brand voice. | Keep. |
| `MADE IN` section (line 91) | Fine. | Keep. |
| `India · with reverence` (line 92) | Brand signature. | Keep. |
| `🪔` footer (line 96) | OK per BRAND.md (occasional 🪔). | Keep. |
| `Smaran v{APP_VERSION}` (line 97) | Fine. | Keep. |

---

### Screen: `app/saved.tsx` — Saved verses

| Current | Issue | Suggested |
|---|---|---|
| `YOUR SAVED VERSES` title (line 64) | Slightly possessive-redundant; "Saved" alone would suffice in caps. | `SAVED VERSES` (the personal context is implicit; cleaner) |
| `accessibilityLabel={`Open Bhagavad Gita ${item.verseId}`}` (line 85) | Fine for a11y. | Keep. |
| `BG {item.verseId}` card ref (line 88) | Fine in card context. | Keep. |
| `&quot;{item.verse.oneLine}&quot;` (line 91) | Fine. | Keep. |
| Empty state ornament `·` (line 70) | Quiet. Fine. | Keep. |
| Empty state text `The verses that meet you,\nyou can save here.` (line 71) | Lovely line. "the verses that meet you" is brand-voice. Could be slightly tightened. | Keep. (Optional alternative: `The verses that meet you\nlive here.` — more declarative, less instructional.) |

---

### Screen: `app/verse-detail.tsx` — Single verse view

| Current | Issue | Suggested |
|---|---|---|
| `BHAGAVAD GITA {verse.chapter}.{verse.verse}` (line 52) | Fine. | Keep. |
| `TRANSLATION` label (line 76) | Fine. | Keep. |
| `REFLECTION` label (line 83) | Fine. | Keep. |
| `Scholar-written commentary on this verse arrives in the coming weeks. For now, sit with the translation and let it speak to you.` (line 84-87) | Strong — "let it speak to you" is brand voice. Could trim "this verse" (redundant in context). | `Scholar-written commentary arrives in the coming weeks. For now, sit with the translation and let it speak to you.` |
| `SIT WITH THIS` label (line 89) | Excellent — distinctive, brand-owned. | Keep. |
| `What in your life today does this verse speak to?` (line 91) | Slightly awkward inversion ("speak to" at the end). | `What in your life today does this verse touch?` (warmer) — or — `Where in your life today does this verse land?` |
| `· Sanskrit recitation coming soon ·` (line 97) | Fine — uses bullet ornament instead of "COMING SOON" caps. Already brand-voice. | Keep. |
| `Back` accessibility label (line 39) | Fine. | Keep. |

---

### Component: `components/MenuSheet.tsx` — Bottom sheet menu

| Current | Issue | Suggested |
|---|---|---|
| `Ask Krishna` (line 29) | Fine. | Keep. |
| `Saved verses` (line 30) | Fine. | Keep. |
| `Settings` (line 31) | Fine. | Keep. |
| `About Smaran` (line 32) | Fine — full name is right for a credits link. | Keep. |
| `Close menu` accessibilityLabel (line 59) | Fine. | Keep. |

---

### File: `lib/share.ts` — Share-to-WhatsApp text

> This is high-stakes copy: every share is a marketing impression seen by a non-user. Currently uses "blessing" (an anti-pattern term) twice and an emoji on the final footer line.

| Current | Issue | Suggested |
|---|---|---|
| `For ${recipientName} 🪔\n\n` (line 24) | The 🪔 is BRAND.md-acceptable in marketing/share. The "For X" framing is good. | Keep — or drop emoji and use ornament: `For ${recipientName} ·\n\n` |
| `Today is ${weekdayName} — ${name} ${honorific}'s day.\n\n` (line 26) | Excellent — mirrors notification voice. | Keep. |
| `"${verse.oneLine}"\n— Bhagavad Gita ${chapter}.${verse}\n\n` (line 27) | Fine. | Keep. |
| `Sent with Smaran 🪔\n— a daily Gita practice` (line 28) | "Sent with" is fine but slightly utilitarian; "a daily Gita practice" is good. | `From Smaran — a daily Gita practice 🪔` (one line, lamp at end is calmer) |
| Share sheet `title: "Today's blessing from Smaran"` (line 41) | **"Blessing" violates BRAND.md §9.** This is the OS-level title for the share sheet — visible on Android. | `"Today from Smaran"` or `"A verse from Smaran"` |
| `accessibilityLabel="Share blessing"` (in `index.tsx` line 245) | Same anti-pattern. | `"Share this verse"` |

---

## Notification Copy Audit (7 weekday notifications — daily impressions)

> **These matter most.** They are seen daily by every retained user. Currently they use a `☀` emoji in the title (anti-pattern) and a serviceable but flat body. Below is a per-day rewrite.

The current code generates one shared title (`Good morning ☀`) and one templated body (`Today is ${weekdayName} — ${name} ${honorific}'s day`). Recommendation: **keep the templated body, fix the title, and add a per-deity 1-line body variant** so the daily bell carries more meaning over time.

### The shared title fix (line 79 of `lib/notifications.ts`)

| Current | Issue | Suggested |
|---|---|---|
| `'Good morning ☀'` | Sun emoji is anti-pattern (BRAND.md: only 🪔 is occasionally acceptable). Generic-cheery, not reverent. | `'Good morning'` — full stop. The body carries the specificity. |

### The per-day body — current + suggested

The current pattern `Today is Sunday — Surya Bhagavan's day` is correct and reverent — keep it. **Optional enhancement**: append a 4-6 word "quality" tag on each day so the notification teaches over weeks.

| Day | Current body | Suggested body |
|---|---|---|
| Sunday | `Today is Sunday — Surya Bhagavan's day` | `Today is Sunday — Surya Bhagavan's day. The day for clarity.` |
| Monday | `Today is Monday — Shiva ji's day` | `Today is Monday — Shiva ji's day. The day for stillness.` |
| Tuesday | `Today is Tuesday — Hanuman ji's day` | `Today is Tuesday — Hanuman ji's day. The day for courage.` |
| Wednesday | `Today is Wednesday — Ganesh ji's day` | `Today is Wednesday — Ganesh ji's day. The day for beginnings.` |
| Thursday | `Today is Thursday — Vishnu Bhagavan's day` | `Today is Thursday — Vishnu Bhagavan's day. The day for steadiness.` (note: "preservation" is too abstract for a notification — "steadiness" carries the same meaning warmer) |
| Friday | `Today is Friday — Lakshmi Devi's day` | `Today is Friday — Lakshmi Devi's day. The day for abundance.` |
| Saturday | `Today is Saturday — Shani Bhagavan's day` | `Today is Saturday — Shani Bhagavan's day. The day for discipline.` |

If implementing the longer bodies feels like scope creep for this audit, the minimum-viable fix is just the title (`'Good morning'`, drop the sun emoji).

### Android channel name + description (lines 46-48 of `lib/notifications.ts`)

| Current | Issue | Suggested |
|---|---|---|
| `name: 'Daily Darshan'` | Fine. | Keep. |
| `description: 'Your morning notification with today's deity and verse'` | Functional. Could be brand-voice. | `'One quiet bell each morning, naming the day's deity.'` |

---

## Onboarding Copy Audit (highest stakes — first impression)

The onboarding is the closest-to-finished area of the app and the place a user forms their voice impression. Most needed changes are tiny — a comma here, a "for now" there — but they raise the ceiling.

| Surface | Current | Suggested |
|---|---|---|
| Welcome tagline | `Daily darshan + Bhagavad Gita.\nA 30-second practice of remembrance.` | `Daily darshan. Bhagavad Gita.\nA 30-second practice of remembrance.` (drop the +, which reads spec-sheet) |
| Welcome bullets | `· One deity each morning / · One verse from the Gita / · A streak that grows quietly` | Keep — already brand-perfect. |
| Welcome CTA | `Begin` | Keep. |
| Name screen heading | `What may we call you?` | `What shall we call you?` (warmer, less Victorian) |
| Name screen subtext | `Your name will appear in your daily greeting. Used only on this device.` | `Used only in your morning greeting. Stored only on this device.` (tighter, parallel) |
| Skip links | `Skip` | `Skip for now` (signals reversibility) |
| Ishta heading | `Choose your ishta-devata` | Keep, italicize `ishta-devata` |
| Ishta subtext | `The form of the divine you feel closest to. (You can change this anytime.)` | `The form of the divine you feel closest to.\nYou can change this anytime.` (drop parens) |
| Notifications heading | `One quiet bell at sunrise` | Keep — exemplary. |
| Notifications subtext | `Smaran sends a single, calm notification at 7 a.m. each morning — naming the day's deity and inviting you to your practice. No other notifications. Ever.` | `One calm bell at 7 a.m. — naming the day's deity, opening your practice. No other notifications. Ever.` |
| Notifications CTA | `Enable morning bell` | Keep. |
| Notifications skip | `Not now` | Keep. |

---

## Top 10 Changes That Most Improve Voice Consistency

Ranked by impact × frequency-of-encounter:

1. **Drop "blessing" everywhere.** Affects: home share button a11y label, share text footer/title, share sheet OS title. Replace with `verse` or remove. Highest-leverage single change — kills the largest BRAND.md anti-pattern in the product.
2. **Fix the daily notification title** from `'Good morning ☀'` to `'Good morning'`. This is seen daily, by every retained user, on their lock screen. The sun emoji is the most visible brand-voice violation in the product.
3. **Add per-day quality tag** to the notification body (`The day for stillness.`, `The day for courage.`, etc.). Teaches the calendar over weeks; transforms the notification from utility to ritual.
4. **Replace `COMING SOON` on Ask Krishna** with `· arriving soon ·` (lowercase, ornament). Removes the loudest startup-marketing tic in the app.
5. **Italicize Sanskrit** consistently: `ishta-devata` in onboarding, add transliteration `karmaṇy-evādhikāras te mā phaleṣu kadācana` under the Sanskrit verse on Ask Krishna. Both are explicit BRAND.md §7 rules being missed.
6. **Change `Send feedback` → `Write to us`** in Settings. Five-letter-shorter and the difference between corporate and personal.
7. **Change `What may we call you?` → `What shall we call you?`** — small, but "may" reads stiff and "shall" reads warm-Indian-English.
8. **Change `Read full →` → `Read this verse →`** on home. The current button leans on the arrow to do the meaning. Specificity is brand-voice.
9. **Change `Your saved verses` title → `Saved verses`.** The "Your" is implicit; cleaner, less corporate.
10. **Change `Bring your real life questions` → `Bring the questions you carry`** on Ask Krishna. "Real life" is self-help vocabulary; "the questions you carry" is brand vocabulary.

---

## Brand Voice Anti-patterns Found (specific, with locations)

1. **Christianity-flavored vocabulary — "blessing"** (BRAND.md §9 explicit ban).
   - `lib/share.ts:28` — footer prose: `"Today's blessing from Smaran"`
   - `lib/share.ts:41` — share-sheet title prop
   - `app/index.tsx:245` — accessibility label `"Share blessing"`

2. **Emoji in primary copy** (BRAND.md §7: only occasional 🪔; never others).
   - `lib/notifications.ts:79` — `'Good morning ☀'` — sun emoji in daily notification title (worst location).

3. **"COMING SOON" caps label** — generic startup-marketing, not devotional voice.
   - `app/ask-krishna.tsx:62` — `COMING SOON` in caps.
   - (Already handled correctly elsewhere: `· deity art coming ·` on home, `· Sanskrit recitation coming soon ·` on verse-detail. Bring Ask Krishna in line.)

4. **Western pop-spirituality vocabulary** (BRAND.md §9: avoid "manifestation," "the divine feminine," etc.).
   - `lib/deityCalendar.ts` field name `energy` — used in code; surfaces as caps tag on home (`· CLARITY ·`, etc.). The single rendered word is fine; the *field name* in the codebase reads pop-spirituality. Suggest renaming `energy` → `quality` in `deityCalendar.ts` for internal consistency. Visible copy is unaffected.
   - `app/ask-krishna.tsx:56` — `real life questions` reads self-help.

5. **Sanskrit not italicized** (BRAND.md §7 explicit rule).
   - `app/onboarding.tsx:123` — `ishta-devata` rendered in plain text.
   - `app/ask-krishna.tsx` — Sanskrit Devanagari verse has no transliteration; should add italic Latin transliteration on next line.

6. **Slightly tech-flavored verbs** in places that should be quiet.
   - `app/about.tsx:73` — `Scholar credit added when full commentary ships.` — "ships" is software vocabulary.
   - `app/onboarding.tsx:115,149` — `Skip` alone reads utilitarian; `Skip for now` matches voice.
   - `app/index.tsx:238` — `Unsave verse` a11y — "unsave" is product-tech.

7. **Stiff/archaic phrasing** in place of warm-Indian-English.
   - `app/onboarding.tsx:93` — `What may we call you?` reads Victorian.
   - `app/verse-detail.tsx:91` — `What in your life today does this verse speak to?` is grammatically inverted.

8. **Missing spaces / typos.**
   - `app/index.tsx:264` — `"Bring a question to Krishna  →"` has a double space before the arrow.

9. **Redundant possessives in titles.**
   - `app/saved.tsx:64` — `YOUR SAVED VERSES` — drop `YOUR`.

10. **Functional-but-flat prose** that could be brand-voice with one edit.
    - `lib/notifications.ts:48` — Android channel description.
    - `app/curator.tsx:159` — Cloud-sync footer note.
    - `app/about.tsx:73` — "ships" line above.

---

## Implementation Notes (for whoever applies these)

- **Single highest-ROI commit**: open `lib/share.ts`, `app/index.tsx`, `lib/notifications.ts`, `app/ask-krishna.tsx`, `app/onboarding.tsx`. Apply the changes in "Top 10" plus the "blessing" sweep. Roughly 15 string changes, maybe 30 minutes of work, fixes ~80% of the brand-voice gap.
- **Sanskrit italicization** in React Native: wrap in a `<Text style={{ fontStyle: 'italic' }}>` span where needed; or define an `Italic` styled component for consistency.
- **Notification body length**: per-day quality tags push body over 60 chars on some Android lock screens — verify on device. If truncated, drop the period after "day" and run as a single clause: `Today is Tuesday — Hanuman ji's day, the day for courage`.
- **Field rename `energy` → `quality`** in `deityCalendar.ts`: pure internal, no string change visible to the user. Touches `index.tsx:216` only at the type level. Do this when next touching the file; not urgent.
- **Re-audit at v0.2** when commentary, premium, and Ask Krishna real-AI screens land. Each of those will introduce 30+ new strings — bake the voice in at the start, not retrofit.

---

## Final note on what the app is doing right

The bones are good. `app/about.tsx`, the onboarding-notifications screen, the Ask Krishna sample exchange, and the empty state on `saved.tsx` are all already at the brand-voice ceiling — they read like Smaran, not like a generic devotional template. The fixes above are not a teardown; they're tightening the seams so every surface reads with the same calm. Once the "blessing" sweep and the notification-emoji fix are in, the app will feel meaningfully more composed — not because users will consciously notice the changes, but because nothing will be quietly subtracting from the reverence the rest of the product is carefully building.
