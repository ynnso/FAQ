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

## Content fact-check flags (from the SEO audit, do not ship unverified numbers)
- Real photography pricing floor is **$139** (tiered: $139 home / $149 new construction / $159 rental / $199 commercial / $249 multifamily) — draft content and old `index.html` both understated this as $159.
- "Agent Lifestyle Video" is likely misnamed — real service is **"Agent Listing Video"**; no confirmed price or turnaround exists for it anywhere.
- Confirmed accurate: HD Video Tours $159–$319, Aerial Photography $79–$119, all 4 regional phone numbers (DFW/Houston/Austin/San Antonio).
- Not yet independently verified: Aerial Video, Matterport, Zillow Showcase, Virtual Staging, Floor Plans pricing rows, and the LocalBusiness schema's `$29` price-range floor.

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

`FAQ SEO.html`, `files.zip`, `.gitignore` (dead rule), `HANDOFF.md`, `PROGRESS.md`, `FAQ_SEO_AUDIT.md`, `FAQ_SEO_PLAN.md` were all deleted 2026-08-30 — fully migrated/superseded, findings captured above.

## Rules that still apply (from CLAUDE.md)
- Verify every visual/behavior change live before calling it done — computed-style checks alone are not sufficient, say explicitly when that's all that was possible.
- Terse replies: bullet list of what changed, nothing else.
- Push only after local verification passes.
