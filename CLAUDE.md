# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

Marketing site for **Sundae**, a slow-dating mobile app (one match every Sunday, no
swiping/feed, personality-first, photos blurred until a conversation gets going, optional
Sundae+ subscription at 6,99&nbsp;€/month). The site is static HTML — one page per file,
no build. The app itself lives in a separate repo (React Native / Expo Router + Supabase).

Hosted on **GitHub Pages** at **sundae.pro** (see `CNAME`).

## Files

- [index.html](index.html) — the landing page, English only. Redesigned Sep 2026 to be
  short and scannable: nav, hero, how-it-works (3 cards folding in the old
  personality/blur and Discover-postcard demos), Sundae+ (feature list + a small
  two-person compatibility-meter visual, echoing the app's own "How you two match"
  screen), safety, FAQ, CTA, footer. The old separate statement / the-week / learning-loop
  / insights sections were cut for being non-essential, not replaced — their key lines were
  folded into one-sentence asides instead (see `.how-note`, FAQ answers). Footer has an
  EN legal column, a DE legal column, and a Product column; each legal column also links
  the matching `delete-account.html`/`konto-loeschen.html` page.
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

- **Design system mirrors the app**, now including its accent colour, not just its
  neutrals. Palette on `:root`: `--bg:#F7F5F2`, `--ink:#171717`, `--muted*`, `--faint`,
  `--card`, `--line*`, and `--accent:#217885` / `--accent-tint` / `--accent-tint-border` —
  copied from the app's own `src/theme.ts`, which calls it "the single deliberate pop of
  colour" and says to use it sparingly (thin strokes, small fills, never on primary
  buttons or body text). On the site that means the `.dot` after "sundae", the
  `.sunday-dot`, postcard stamps, and the compatibility-meter dots — nothing else. Heading
  font is `Lora` (serif); the postcard/handwritten touches (`.pc-hand`, `.mini-pc span`)
  use `Caveat`. Small uppercase `.eyebrow` labels; pill buttons (`.btn`,
  `border-radius:999px`).
- **`index.html` is a stack of `<section>`s**, each with a `/* ===== NAME ===== */` CSS
  banner. In-page nav uses section `id`s (`#how`, `#plus`, `#safety`, `#faq`, `#join`) with
  smooth scroll — a new nav link needs the matching `id`. (`#week` and `#personality` were
  removed in the Sep 2026 redesign; don't re-add links to them.)
- **`.reveal`** elements fade in via one `IntersectionObserver`; gated by
  `prefers-reduced-motion`.
- **Responsive**: main breakpoint `@media (max-width:820px)` (plus 900/480px tweaks) —
  collapses the hero/Sundae+ two-column grids to one column, the how/safety card grids to
  fewer columns, and swaps in the `.nav-toggle` menu.
- **Legal pages** share a narrower reading layout (760px) and a `.toc`. The `.placeholder`
  CSS (peach background) is retained but no placeholders remain — all operator details
  are filled in: Philipp Ludwig Syring, Otto-Lauffer-Str. 3a, 37077 Göttingen; email
  `support@sundae.pro`; Kleinunternehmer § 19 UStG; Supabase region EU/West (Ireland);
  no Discord/Slack — reports are reviewed via a local admin tool that hits Supabase directly.

## Store distribution is deliberately restricted to the EU/Germany

The operator had ticked "available worldwide" in App Store Connect / Play Console. After
comparing Hinge's privacy policy (Sep 2026), we flagged that worldwide availability would
likely trigger US state privacy laws that have **no small-business threshold** — notably
Washington's My Health My Data Act and Nevada's consumer health data law (NRS 603A), both
written broadly enough to plausibly sweep in dating-preference data. Unlike CCPA/VCDPA/CPA/
CTDPA/UCPA (which all have revenue or record-count thresholds a pre-launch Kleinunternehmer
won't meet), WA/NV apply based on "conducting business" in the state, with no such floor.

**Decision: narrow store availability to the EU (or Germany) instead of adding a US
consumer-health-data supplement.** This also matches the site's own "launching city by
city" copy in the CTA section. Nobody has changed the actual App Store Connect / Play
Console setting yet as of this note — that's an action item for the operator outside this
repo, not something committable here. If distribution is ever widened to include US states,
revisit: Washington/Nevada consumer health data supplement, and re-check CCPA/state-law
thresholds against actual user/revenue numbers at that time.

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
- Expo is legally **650 Industries Inc., 140 2nd Street, Floor 4, San Francisco, CA
  94105, USA** — named with full entity/address in privacy §6, after comparing notes with
  another indie developer's privacy policy (Sep 2026).
- **Apple refund requests**: if a user disputes an App Store charge for Sundae+, Apple may
  ask the developer for a small amount of purchase info (transaction ID, confirmation the
  feature was delivered/worked) before deciding the refund. Documented in terms §9
  ("Refunds" bullet) and privacy §6 (Apple/Google entry) — worded generically (describes
  what *can* happen for any App Store subscription seller), not as a claim that this has
  already happened. Revisit if the operator's actual practice becomes more specific (compare
  how thorough other apps' policies get about this, e.g. what data they stopped sending
  Apple over time).
- Transfers §7 now names **Google, Apple and Microsoft (GitHub's parent)** specifically as
  EU–US Data Privacy Framework participants, based on well-established public knowledge —
  deliberately did **not** claim a specific transfer mechanism for Vonage or Expo (650
  Industries) since their DPF status couldn't be verified live; they stay under the
  generic "Standard Contractual Clauses" fallback. If you ever get a chance to check the
  official DPF participant list (dataprivacyframework.gov — it's a JS app, doesn't work
  via simple fetch) or confirm actual signed DPAs/SCCs with Vonage and Expo, tighten this.
- Considered (and deliberately skipped) adding an "EU AI Act" transparency section for the
  matching/quality-score model, after seeing another dev's app do this for image-recognition
  AI. Sundae's matching is a ranking algorithm, not one of the AI Act's Art. 50 transparency
  triggers or an Annex III high-risk category (dating/matchmaking isn't listed) — self-
  declaring an AI Act compliance conclusion in the public doc felt like exactly the kind of
  judgment call CLAUDE.md already flags for a lawyer, so it's not in privacy.html. The
  existing Art. 22 GDPR paragraph in §9 already covers "no legal/similarly significant
  automated decision" without needing AI Act framing.
