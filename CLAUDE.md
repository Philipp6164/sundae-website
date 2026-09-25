# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

Marketing site for **Sundae**, a slow-dating mobile app (one match every Sunday, no
swiping/feed, personality-first, photos blurred until a conversation gets going, optional
Sundae+ subscription at 6,99&nbsp;€/month). The site is static HTML — one page per file,
no build. The app itself lives in a separate repo (React Native / Expo Router + Supabase).

Hosted on **GitHub Pages** at **sundae.pro** (see `CNAME`).

## Files

- [index.html](index.html) — the landing page, English only (nav, hero, statement,
  how-it-works, the week, personality/blur demo, learning loop, insights, Sundae+, safety,
  FAQ, CTA, footer). Footer has both an EN and a DE legal-links column.
- **Legal pages — three docs, each an English + German pair:**
  - privacy policy: [privacy.html](privacy.html) ↔ [datenschutz.html](datenschutz.html)
  - terms of use: [terms.html](terms.html) ↔ [nutzungsbedingungen.html](nutzungsbedingungen.html)
  - § 5 DDG legal notice: [imprint.html](imprint.html) ↔ [impressum.html](impressum.html)

- **Account deletion instructions** (required by Google Play's Data Safety form, linked from
  its "Konto-URL löschen" field): [delete-account.html](delete-account.html) ↔
  [konto-loeschen.html](konto-loeschen.html). Numbered in-app steps + the "without the app"
  email path + a short what's-deleted/what's-kept summary consistent with privacy.html §8/§11.
  Not part of the EN/DE legal-page pair above, but follows the same template/CSS.

  There is **no separate EULA**. It was dissolved into the Terms: Apple Guideline 1.2
  (zero tolerance for objectionable content / filter + report + block / act within 24h)
  lives in Terms §6 (`#objectionable` subsection) + §10 (moderation); the app licence +
  reverse-engineering restrictions are in Terms §14; the App Store points (Apple's
  Standard EULA applies, Apple not a party, Apple is a third-party beneficiary, sanctions
  rep) are in Terms §2 (`#stores` subsection). The store setup should link Apple's
  Standard EULA, not a custom one.

  The **German version is the authoritative one** (stated in each doc's governing-law /
  scope section); English is a convenience translation. A `.langswitch` pill in each
  legal page's `<nav>` links EN↔DE. Keep the two halves of a pair in sync when editing.

All files are self-contained: markup + one `<style>` block + tiny inline `<script>`. No
bundler, framework, package manager, or tests. Preview by opening a file directly or
`python -m http.server` from the repo root. The legal pages share a CSS block (kept in
sync by hand) and cross-link each other in their footers + TOCs.

## Conventions

- **Design system mirrors the app.** Palette on `:root` (`--bg:#F7F5F2`, `--ink:#171717`,
  `--muted*`, `--faint*`, `--card`, `--line*`, `--dark`). Dark sections
  (`.statement`, `.insights`, `.cta`, `footer`) invert to `#171717`/white explicitly.
  Type: system font stack, headings `font-weight:600` with tight negative
  `letter-spacing`; small uppercase `.eyebrow` labels; pill buttons (`.btn`,
  `border-radius:999px`).
- **`index.html` is a stack of `<section>`s**, each with a `/* ===== NAME ===== */` CSS
  banner. In-page nav uses section `id`s (`#how`, `#week`, `#personality`, `#plus`,
  `#safety`, `#faq`, `#join`) with smooth scroll — a new nav link needs the matching `id`.
- **`.reveal`** elements fade in via one `IntersectionObserver`; gated by
  `prefers-reduced-motion`.
- **Responsive**: main breakpoint `@media (max-width:820px)` (plus 900/480px tweaks) —
  collapses grids, swaps in the `.nav-toggle` menu, turns the week grid into rows.
- **Legal pages** share a narrower reading layout (760px) and a `.toc`. The `.placeholder`
  CSS (peach background) is retained but no placeholders remain — all operator details
  are filled in: Philipp Ludwig Syring, Otto-Lauffer-Str. 3a, 37077 Göttingen; email
  `support@sundae.pro`; Kleinunternehmer § 19 UStG; Supabase region EU/West (Ireland);
  no Discord/Slack — reports are reviewed via a local admin tool that hits Supabase directly.

## Known gaps / to finish

- Imprint shows a private residential address (legally required for this kind of service;
  a P.O. box is not enough). Operator may later switch to a *ladungsfähige Anschrift*
  service and update `imprint.html` + the address lines in privacy/terms/eula.
- "Join the waitlist" has no backend: the CTA form builds a `mailto:` to
  `support@sundae.pro` client-side.
- Legal texts are a thorough draft, not a substitute for review by a lawyer.

## Legal texts mirror specific app behaviour

The privacy policy / terms were written against the app's SQL (in the app repo,
`sql/`). If these change in the app, update **both language versions** of the doc:
- date-of-birth age gate (`birth_date.sql`) → privacy "Age verification" row, terms §3
- Sunday drop **plus a Tuesday second-chance drop** (`reset_schedule.sql`) → "one per
  cycle, tries again mid-week" wording everywhere (not literally "every Sunday" in legal text)
- internal quality score from partner feedback + engagement (`quality_score.sql`) →
  privacy "Match quality signal" row + §9 (Art. 22 note)
- match history, Sundae+ vs free (`match_history.sql`) → privacy + terms §9 + index Sundae+ feature 04
- partner can read your derived personality dimensions (`match_personality.sql`) →
  privacy "Other users"
- conversation snapshots + 90-day post-ban erasure (`sql/moderation/01,03,06`) → privacy §8
- `delete_my_account` = anonymise + keep messages as "Deleted user" → privacy §8/§11
- `sql/moderation/05` webhook (Discord/Slack) is NOT used — reports reviewed via a local
  admin tool; privacy §6 says exactly that. Revisit if the operator ever wires the webhook.
- phone/SMS OTP sign-in (Vonage) + Sign in with Apple / Google, **no passwords**, verified
  phone required before onboarding (`require_phone_for_onboarding.sql`) → privacy Account
  row, §4 legal basis, §6 processors (Vonage/Apple/Google), §7, security text; terms §3/§5
- required device location (no "approximate/optional" wording anywhere) → privacy Location
  row, terms §3/§4
- optional politics/religion + notes (Art. 9 special categories) → privacy Profile row, §5
- **Discover & postcards** (`nearby_top_matches` / `postcards.sql`): top-12 nearby,
  **the same twelve, sharp photo, for everyone now** — the old 3-free/12-Plus split is
  gone. Instead you write a postcard (≤100 chars, delivered next day) to whoever you
  like; free = 1 new thread/day, Sundae+ = 3/day + 2 reserved delivery spots in an
  otherwise-full inbox (5/day shared cap). Postcards ARE shown to their recipient (not
  private) and DO feed the matching model (`interest_mutual_boost`/`interest_oneway_boost`)
  — don't write "not shown to the other person" or "not used for matching". Two people who
  each send enough cards and both agree get a guaranteed "sure match" the next Sunday,
  placed before the model runs. → privacy "Discover & postcards" row + "People near you" h3
  + §9 matching-model paragraph, terms §4/§9, index step 02 + Sundae+ section + FAQ.
- voice messages (≤2 min audio, private bucket) + typing indicator + push previews →
  privacy Matches row, §6 Expo, §8; terms §4/§7
- Sundae+ = every match photo (not just the first — the profile TEXT itself, bio/prompts/
  basics/interests, is free for any match, always has been), more postcards + reserved
  spots, compatibility breakdown, match history, deeper insights → terms §9, index
  Sundae+ list. Discover itself (the twelve people) is NOT a Sundae+ feature anymore.
- app UI is en/de/fr; legal pages exist in EN + DE only (French optional)
