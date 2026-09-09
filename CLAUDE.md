# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

Marketing site for **Sundae**, a slow-dating mobile app (one match every Sunday, no
swiping/feed, personality-first, photos blurred until a conversation gets going, optional
Sundae+ subscription at 2,99&nbsp;€/month). The site is static HTML — one page per file,
no build. The app itself lives in a separate repo (React Native / Expo Router + Supabase).

Hosted on **GitHub Pages** at **sundae.pro** (see `CNAME`).

## Files

- [index.html](index.html) — the landing page (nav, hero, statement, how-it-works, the
  week, personality/blur demo, learning loop, insights, Sundae+, safety, FAQ, CTA, footer).
- [privacy.html](privacy.html) — GDPR/BDSG privacy policy.
- [terms.html](terms.html) — Terms of Use (German law, consumer-facing).
- [eula.html](eula.html) — End User License Agreement: app licence + the zero-tolerance /
  objectionable-content / 24h-response clauses Apple (Guideline 1.2) and Google require
  for UGC apps, including Apple's mandated App Store schedule.
- [imprint.html](imprint.html) — § 5 DDG legal notice (operator is a Kleinunternehmer
  per § 19 UStG, uses a delivery-address service, no phone line).

All are self-contained: markup + one `<style>` block + tiny inline `<script>`. No
bundler, framework, package manager, or tests. Preview by opening a file directly or
`python -m http.server` from the repo root. The four legal pages share a CSS block (kept
in sync by hand) and cross-link each other in their footers + TOCs.

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
  are filled in: Philipp Ludwig Syring, Lange-Geismar-Str. 62, 37073 Göttingen; email
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

The privacy policy / terms / EULA were written against the app's SQL (in the app repo,
`sql/`). If these change in the app, update the docs:
- date-of-birth age gate (`birth_date.sql`) → privacy "Age verification" row, terms §3, eula §9
- Sunday drop **plus a Tuesday second-chance drop** (`reset_schedule.sql`) → "one per
  cycle, tries again mid-week" wording everywhere (not literally "every Sunday" in legal text)
- internal quality score from partner feedback + engagement (`quality_score.sql`) →
  privacy "Match quality signal" row + §9 (Art. 22 note)
- match history, Sundae+ vs free (`match_history.sql`) → privacy + terms §9 + index Sundae+ feature 04
- partner can read your derived personality dimensions (`match_personality.sql`) →
  privacy "Other users"
- report → Discord/Slack webhook (`sql/moderation/05`) → privacy processors list
- conversation snapshots + 90-day post-ban erasure (`sql/moderation/01,03,06`) → privacy §8
- `delete_my_account` = anonymise + keep messages as "Deleted user" → privacy §8/§11
