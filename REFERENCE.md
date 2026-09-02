# FAQ Project — Reference

_Consolidated 2026-08-30 from CLAUDE.md, HANDOFF.md, PROGRESS.md, FAQ_SEO_AUDIT.md, FAQ_SEO_PLAN.md._

## What this is
Shoot2Sell (Texas real estate photography) self-serve FAQ page. Not client-facing marketing.

## Current state
- **`index.html`** holds the real content (the v2 redesign) — swapped in 2026-08-30 so the short root URL works directly. `faq-v2.html` is now a redirect shell pointing to `./`, kept only for old bookmarks/links.
- **`index.html`**'s original content (old 6-section/57-question build, dark/light theme) is gone from the live file but survives in git history. Local `FAQ.html` mirror was deleted 2026-08-30.
- Repo is public again on GitHub (`github.com/ynnso/FAQ`) after a brief private stint broke GitHub Pages (free plan doesn't serve Pages from private repos) — visibility flipped back, Pages source had to be manually re-enabled in Settings → Pages, confirmed live again.
- Live: https://ynnso.github.io/FAQ/ (real page, short URL) — https://ynnso.github.io/FAQ/faq-v2.html redirects here.
- **Primary test viewport is 393×852** (iPhone 15/16/17), not 375×667 — 375×667 is the stress-test floor only, not the design target. Corrected 2026-08-30 after testing at the wrong baseline.

## v2 redesign — status as of 2026-08-30 (multiple passes)

**Resolved and verified (real click/scroll dispatch or DOM measurement, not just code review):**
- H1 weight, Top 5 Trending font, MLS/PORT text color, active section (black fill) / active question (white fill) two-tone pair, 360 icon (rebuilt twice — final version is pure vector strokes, zero font dependency, to eliminate cross-platform font-metric differences as a variable), search bar is white at rest (was black-85%, made `.search-clear`'s hardcoded-black X icon invisible), search/section left-right alignment (real root cause: hero padding was a fixed px number, `.main`'s is a fluid `clamp()` — now both use the same formula, verified 0px gap at 375/393/440/768px), pricing table hides during an active search, buried-answer-after-picking-a-suggestion (re-scrolls after the accordion's expand transition finishes, not before).
- **Sticky/pinned search bar — real root cause found via an actual scroll test, not assumption:** `position:sticky` on the hero search bar structurally cannot survive scrolling past its own short containing block (`.hero`) — confirmed live, `rect.top` reached `-760px` instead of staying at `0`. Reverted the hero bar to normal flow; the always-pinned affordance is `.sticky-header` (`position:fixed`, no containing-block limit), rebuilt to be a real visual duplicate of the hero bar (was still black-85%/white-icon from before the hero bar itself went white) and hardened against two documented iOS Safari `position:fixed` failure modes (dropped `backdrop-filter`, added `translateZ(0)` + `will-change:transform` for its own GPU layer).
- **Full functional sweep, 2026-08-30:** all 83 questions across 13 sections opened/closed via real dispatched click events — 0 truncated, 0 empty, 0 console errors. All search-suggestion-dropdown paths tested individually (trending click, matching-questions click, fuzzy-typo fallback, no-results state, arrow-key nav + Enter, Escape, Clear-recent button, submit button, input's own X) — all correct.

**Real environment limitations hit repeatedly, not page bugs:**
- The `computer` click tool hangs page-wide (30s timeout, no console errors) — worked around throughout via dispatched `PointerEvent`/`MouseEvent`, which does exercise real listeners.
- `document.hasFocus()` is false in this automation context, so real `:focus`/`:focus-within` CSS never engages even when `document.activeElement` is correct — anything gated on real focus rests on weaker (but still real) verification here.
- Screenshots render corrupted/duplicated at tablet (768px) and desktop (1280px) specifically, and can show a stale frame for 1-2s after a JS-triggered DOM change — confirmed via direct DOM measurement that the underlying page is correct both times; only the screenshot capture itself is broken. Always double-check via `getBoundingClientRect`/computed style before concluding something is broken from a screenshot alone at those widths.
- **None of the above has been confirmed against a real iPhone.** Everything in this section is either verified in this environment or targets a specific documented real-device failure mode — neither is a substitute for Sonny's own phone.

## Pricing table (`.pricing-tile`) fixes, 2026-08-30 — multiple passes, real lesson in it

Long back-and-forth on this section specifically — worth recording the actual lesson, not just the final state:

- Real bugs found and fixed, in order: divider lines merging into one (negative-margin bleed removed), arrow too small/grey → bigger/red, arrow overlapping the name text (moved to price row), `1fr 1fr` not actually equal columns (`minmax(0,1fr)` needed), arrow anchored to the tile's corner instead of the content (dislocated at wide desktop tiles — moved into the price/contact row's own flex flow so it follows content width at any size), dollar-sign superscript broken by the `inline-flex` change (`vertical-align` silently no-ops on flex children — needs `align-self` instead), Virtual Staging/Zillow 360 wrong links and icons.
- **The actual lesson:** "Agent Brand Video" wrapping to 3 lines got "fixed" twice and stayed broken both times, because the verification method itself was wrong — first a stale screenshot (real tool quirk, screenshots can lag a DOM change), then height ÷ line-height arithmetic (silently wrong once the row is `flex`/`inline-flex`, since the row's height is set by its tallest child — an icon or arrow — not by text line count). The fix that actually held: `range.selectNodeContents(textNode); range.getClientRects().length` — measures real line breaks directly, can't be fooled by either failure mode. Root cause once measured correctly: the 44px icon was eating too much of a 155px mobile tile's width for two words to ever share a line at any font size. Shrunk the icon itself (44px→32px, universal), not just the text.
- Second lesson, from the same stretch: don't swing to the biggest fix that's already proven — the Contact-us arrow got shrunk to 14px everywhere when only mobile (155px tiles) actually needed it; desktop (245px+ tiles) had real room for the standard 22px. Fixed by scoping the smaller size inside the existing mobile media query instead of picking one size globally.
- **`CLAUDE.md` now opens with a NEVER ASSUME / TEST CODE rule** capturing both lessons — read it before trusting a screenshot or a line-count calculation on this file again.

## SEO/AEO pass, 2026-08-30
Ran a hard critique grounded in the live deployed page (fetched fresh, not assumptions) against `REFERENCE.md`'s own history. Shipped the top 6 fixes:
- Title: generic `FAQ | Shoot2Sell Photography` → keyword+geo `Real Estate Photography FAQ | Shoot2Sell Texas`.
- Added `<link rel="canonical">`, Open Graph + Twitter Card tags (none existed before).
- H1 stays visually just "FAQ" (the giant display type is untouched) but now carries a real, accessible (clip-based, not `display:none`) keyword-rich span — H1 is the highest-weight on-page signal and was carrying zero keywords before.
- `FAQPage` schema: added `dateModified`/`url`. Added a separate `Organization`/`WebSite`/`BreadcrumbList` schema block — deliberately **no logo/sameAs/telephone fields**, since no real verified URL or number for any of those exists anywhere in this repo; fabricating one would hurt trust signals more than an absent one.
- Removed the 21 Dashboard/Account questions from the `FAQPage` schema's `mainEntity` (62 remain, verified via JSON parse) — that content stays fully on the page for real users, it's just internal support content diluting the schema's topical focus alongside 62 real commercial questions.
- Added `robots.txt` + `sitemap.xml` (neither existed before).

**Not done, held for a later decision (from the same critique):** splitting the highest-intent service clusters (Aerial, Matterport/360, Floor Plans, Virtual Staging) into their own dedicated URLs — the single biggest remaining structural gap, but a real project scope, not a quick fix. Also not done: separating the inline `<style>`/`<script>` into external cacheable files (540KB single file, low urgency, no images so no waterfall problem).

## Content fact-check flags — status as of 2026-09-01 pricing pass (see full section below)
- ~~Real photography pricing floor is $139~~ — **resolved 2026-09-01**, real floor is $139 (25-image base, under-2,000-sqft tier) but the real "starting from" figure across all packages is **$119** (Mini, 12 images). Both now correctly represented.
- ~~"Agent Lifestyle Video" is likely misnamed — real service is "Agent Listing Video"~~ — **this flag was itself wrong.** Confirmed 2026-09-01 directly from Sonny (real product screenshots): these are **two separate, real products** — Agent Listing Video ($499) and Agent Lifestyle Video Package ($999) — not one misnamed thing. A prior session had conflated them under one name/price; fixed.
- Confirmed accurate: HD Video Tours $159–$319, Aerial Photography $79–$119, all 4 regional phone numbers (DFW/Houston/Austin/San Antonio).
- **Resolved 2026-09-01** (was "not yet independently verified"): Floor Plans (2D $69 / 3D $99, was showing $79/$199), ColorPop ($39 first/$15 additional, was flat $39/image), Blue Skies & Green Grass (two separate $29 add-ons, was merged into one). See dated section below.
- **Still not independently verified:** Aerial Video, Matterport, Zillow Showcase pricing rows beyond the single tile figure, and the LocalBusiness schema's `$29` price-range floor. `FAQ Price Sheet.md` (added 2026-09-01) has the real tier ladders for Matterport and Zillow 360 — not yet built into the FAQ's Q&A text.

## Pricing fact-check pass, 2026-09-01

Sonny handed over `FAQ Price Sheet.md` (the real master rate sheet) plus real product screenshots mid-session, prompted by catching a mismatch between two numbers already on the same page. Cross-verified against the price sheet, live shoot2sell.com embedded JSON data, and (where relevant) the separate PriceApp project's own Golden Dataset — all three agreed, confirming the price sheet as reliable ground truth. Fixed, real, confirmed errors:

- **ColorPop:** was flat "$39 per image" everywhere (6 locations) — real structure is $39 first + $15 each additional, matching PriceApp's own Golden Dataset (ADD-03: 4 images = $84, not $156).
- **Floor Plan 2D/3D:** 2D was $79 (real $69), 3D was $199 (real $99 — that $199 figure was actually the PORT/commercial rate, misapplied to the residential FAQ).
- **Photography:** was flat "$159–$549" range not matching the live pricing tile's own $139. Replaced with the real 7-tier sqft breakdown from the price sheet ($139/$166/$189/$214/$279/$339/$399 at the 25-image base), plus the real $119 Mini-package floor.
- **Blue Skies & Green Grass:** was one $29 service; real price sheet shows these as two separate $29 add-ons (Blue Sky Replacement, Green Grass Enhancement). Split across all 9 locations that referenced it (overview table, dedicated Q&A, comparison Q&A, turnaround/compliance/reorder/pricing-summary lists).
- **Agent Brand Video → two real products, not one:** confirmed via Sonny's own product-page screenshots that "Agent Listing Video" ($499, shoot2sell.com/agent-listing-video) and "Agent Lifestyle Video Package" ($999, shoot2sell.com/agent-lifestyle-video) are separate real products with separate real specs — a prior session had conflated them under one FAQ entry named "Agent Brand Video," with the $999 product's content wrongly attached to a "$499 or price-on-request" label, and a wrong 5–7 business day turnaround (real: Agent Listing Video is 24-hour, only the Lifestyle Package takes 5–7 days). Renamed to match the real site, added the missing Lifestyle Package as its own Q&A, fixed the mismatched delivery times and file-format specs that resulted from the conflation.
- **Duplicate schema question:** "How much do floor plans cost?" appeared twice as separate `Question` nodes in the same `FAQPage` block (legitimately shown twice in the visible UI, across two categories — but Google flags duplicate `name` fields in one schema block). Removed the duplicate schema entry, kept both visible accordions.

**Verification:** JSON-LD re-validated after every edit (62 questions, no duplicate names). All 9+ outbound shoot2sell.com links checked live (200 OK, no redirects). Pricing tile screenshot-confirmed clean at 393×852 (primary target, matches CLAUDE.md) and 440×956 (ceiling) after the Agent Listing Video rename — no clipping, tile row height auto-equalizes with its neighbor. At 375×667 (stress floor only) the longer name wraps to 3 lines instead of 2 but does not clip or overflow (measured via `getClientRects()` per the NEVER ASSUME rule, not eyeballed) — accepted by Sonny as-is, not worth trading off the 393×852 primary target for.

**Real lesson, matches this file's existing pattern:** this is the second documented case (after the Aug 30 pricing-table saga) of a real defect surviving specifically *because* a prior session's content was trusted at face value instead of fact-checked against a real source. The "Agent Lifestyle Video is likely misnamed" flag two sections up was itself wrong — written from inference, not verification, and would have led a future session toward the wrong fix if acted on directly. Treat every existing pricing claim in this file as unverified until checked, including ones that look authoritative.

## Decisions made along the way
- Structure: granular per-service sections (11ish), not the audit's leaner 7-section alternative — long-tail search favors per-service pages.
- Dashboard/Account (tech-support content, ~45% of the old FAQ, zero search value) was folded into v2 rather than cut — see Step 2 of the original plan.
- Dark mode fully removed in v2 — light-only by explicit decision.
- Bolded-lead-sentence answer pattern (from the boosted draft) adopted for AEO/featured-snippet value.

## Files still in the folder
| File | Status |
|---|---|
| `index.html` | Active, real content — the only version that matters |
| `faq-v2.html` | Redirect shell to `./`, kept only for old bookmarks/links |
| `CLAUDE.md` | Binding project rules (communication style, verification protocol) |
| `REFERENCE.md` | This file |
| `README.md` | 1 line, effectively empty |
| `FAQ Price Sheet.md` | Added 2026-09-01. Real master rate sheet — the pricing source of truth, see the 2026-09-01 section above |
| `HANDOFF.md` | Re-added 2026-08-30 (was deleted earlier that day, reinstated as the per-session onboarding doc) — read first in a new chat |

`FAQ SEO.html`, `files.zip`, `.gitignore` (dead rule), `PROGRESS.md`, `FAQ_SEO_AUDIT.md`, `FAQ_SEO_PLAN.md` were all deleted 2026-08-30 — fully migrated/superseded, findings captured above.

## Rules that still apply (from CLAUDE.md)
- Verify every visual/behavior change live before calling it done — computed-style checks alone are not sufficient, say explicitly when that's all that was possible.
- Terse replies: bullet list of what changed, nothing else.
- Push only after local verification passes.
