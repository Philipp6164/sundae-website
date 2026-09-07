# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

Marketing site for **Sundae**, a slow-dating mobile app (one match every Sunday, no
swiping/feed, personality-first, photos blurred until a conversation gets going, optional
Sundae+ subscription at 2,99&nbsp;€/month). The site is static HTML — one page per file,
no build. The app itself lives in a separate repo (React Native / Expo Router + Supabase).

## Files

- [index.html](index.html) — the landing page (nav, hero, statement, how-it-works, the
  week, personality/blur demo, learning loop, insights, Sundae+, safety, FAQ, CTA, footer).
- [privacy.html](privacy.html) — GDPR/BDSG privacy policy.
- [terms.html](terms.html) — Terms of Use (German law, consumer-facing).
- [imprint.html](imprint.html) — § 5 DDG legal notice.

All four are self-contained: markup + one `<style>` block + tiny inline `<script>`. No
bundler, framework, package manager, or tests. Preview by opening a file directly or
`python -m http.server` from the repo root.

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
- **Legal pages** share a narrower reading layout (760px), a `.toc`, and `.placeholder`
  spans (peach background) marking every value the operator must fill in before launch.

## Known gaps / to finish

- Legal pages contain `<span class="placeholder">` markers for the operator's real
  details (name, address, contact, hosting/email provider, Supabase region, retention
  periods, supervisory authority). Search `class="placeholder"` across the repo.
- Contact address `support@sundae.pro` and domain `sundae.pro` are assumed from the app
  source — confirm before publishing.
- "Join the waitlist" has no backend: the CTA form builds a `mailto:` to
  `support@sundae.pro` client-side.
- Legal texts are a solid starting draft, not a substitute for review by a lawyer.
