**NEVER ASSUME — TEST CODE.** Verify claims against real measurements, not screenshots or arithmetic. This session hit both failure modes directly: (1) screenshots taken right after a DOM change can show a stale render — a real, repeated tool quirk, not a page bug; re-screenshot after a wait, or better, measure. (2) height ÷ line-height "line count" math silently breaks the moment a row becomes `display:flex`/`inline-flex` — the row's height is set by its tallest child (e.g. an icon), not by how many lines the text wraps to. The one method that can't lie about line-wrapping: `range.selectNodeContents(textNode); range.getClientRects().length`. When a fix doesn't land, don't re-guess at a bigger number — measure what's actually happening first. Also: prefer the smallest change that verifiably works over a big swing (e.g. shrinking one element's spacing before resizing something universal); when the user offers alternatives, pick one — don't add an extra unrequested twist on top.

**NEVER SUBMIT UNTIL VERIFIED.** Do not commit/push/report a fix as done until test and verification pass at real ≥95% confidence, covering BOTH desktop and mobile, BOTH visual and functional — a fix confirmed on one axis (e.g. mobile-only, or functional-but-not-visual) is not confirmed. "95% confidence" means: measured (not eyeballed), checked at more than one width per breakpoint class where geometry is width-dependent, and explicitly re-checked against everything else on the page that the same change could plausibly disturb — not just the one thing it was meant to fix. If real confidence is below that bar, say so plainly instead of reporting "done."

# FAQ — Shoot2Sell Photography

Single-file FAQ support page for Shoot2Sell, a Texas real estate photography company. Self-serve answers, not client-facing marketing.

**Working file:** `index.html` (the only version that matters — `FAQ.html` mirror was deleted 2026-08-30 as fully migrated)
**Live:** https://ynnso.github.io/FAQ/
**Repo:** github.com/ynnso/FAQ
**Pricing source of truth:** `FAQ Price Sheet.md` (added 2026-09-01) — verify any dollar figure against it (or live shoot2sell.com) before shipping. This repo has real, confirmed history of a prior session shipping wrong prices and conflating two distinct real products under one name — see the 2026-09-01 entry in `REFERENCE.md`.

## Stack
Vanilla HTML/CSS/JS. No framework, no build step. One `<script>` IIFE.

## Behavior, as built
- 82 questions across 11 categories, 80 unique ones represented in the `FAQPage` JSON-LD (82 HTML rows = 80 unique questions — 2 legitimate cross-listed duplicates: "How much do floor plans cost?" and "How much does real estate photography cost?", each shown in two categories, same content reused not restated). One section open at a time. Clicking outside, or opening another section, auto-collapses the current one. Includes a "Commercial Properties" section (added 2026-09-02) covering PORT/commercial pricing for Photo, Aerial, 360/Matterport, Floor Plans, and Video, and a "Real Estate Photography" section (added 2026-09-02) — the company's core namesake service previously had no dedicated section at all, just one pricing question inside Pricing & Packages.
- **Hub-and-spoke reorg, stage 1 of 4 (2026-09-02):** Booking & Scheduling, Delivery & Technical, and Regional Coverage merged into one section, "Booking, Delivery & Regional" (`id="block-support"`, `data-cat="support"`, pill label "Support") — all three were cross-cutting logistics digests with no per-product content to lose. Each question keeps its original `qa-tag` label (Booking & Scheduling / Delivery & Technical / Regional Coverage) so provenance stays visible even though they now share one physical section. Individual question anchors (`id="q-..."`) are untouched — every deep link built earlier still resolves. Cat-pill row updated to match (delivery/regional pills removed, booking2 pill renamed to "support"/"Support"); search indexing derives `cat` from each block's `data-cat` dynamically, so no separate list needed updating. **Stage 2 (2026-09-02):** 360 Media Tours + Floor Plans merged into "3D Tours & Floor Plans" (`id="block-tours-floorplans"`, `data-cat="tours-floorplans"`, pill label "3D Tours"). Virtual Staging sits directly after it, untouched — reserved for its own merge with Enhancements in stage 3. Same pattern as stage 1: original `qa-tag` labels kept per question, question anchors untouched, cat-pill row updated (floorplans pill removed, tours360 pill renamed).

**Stage 3 (2026-09-02):** Virtual Staging + Enhancements merged into "Virtual Staging & Photo Enhancements" (`id="block-staging-enhancements"`, `data-cat="staging-enhancements"`, pill label "Staging"). Video & Reels sits between the two former section slots and stayed in place, untouched. Same pattern as stages 1-2: original `qa-tag` labels kept per question, question anchors untouched, cat-pill row updated (enhancements pill removed, staging2 pill renamed).

**Stage 4 (2026-09-02) — reorg complete:** Real Estate Photography + Aerial merged into "Real Estate Photography & Aerial" (`id="block-photography-aerial"`, `data-cat="photography-aerial"`, pill label "Photos" — the old "Aerial" pill renamed, since Real Estate Photography never had its own pill to begin with, a pre-existing gap now folded away rather than fixed separately). Saved for last as planned since this is the section carrying the earlier live weather-trip-fee fix — re-verified word-for-word intact after the merge. Same pattern as stages 1-3: original `qa-tag` labels kept, question anchors untouched, cat-pill row updated.

**Reorg summary, all 4 stages:** 13 sections → 8 (Services Overview, Commercial Properties, Real Estate Photography & Aerial, 3D Tours & Floor Plans, Virtual Staging & Photo Enhancements, Video & Reels, Pricing & Packages, Booking/Delivery/Regional support). All 82 questions and 80 unique schema entries preserved throughout — zero content lost, only regrouped. Every stage independently verified (JSON-LD, deep-link, pill-click, qa-tag provenance, screenshot + console at 393×852) before commit.

**Known remaining gap, not addressed by the reorg:** the pill row still carries dead "Dashboard"/"Account" pills (see stage 1 note above) — a leftover from the 2026-08-30-ish Dashboard/Account content removal, unrelated to this reorg. Flagged for a future pass.
- **Known pre-existing gap, not touched by the reorg:** the cat-pill row still carries "Dashboard" and "Account" pills left over from the 2026-09-02 Dashboard/Account removal below — those pills have no matching section and are dead. Also, "Real Estate Photography" and "Commercial Properties" sections have never had a cat-pill at all. Neither is new; flagged for a future pass.
- **Dashboard/Account support content was removed entirely 2026-09-02** (21 questions, was already excluded from schema) — it duplicated real, live content at `shoot2sell.com/dashboard-help`, and account/login help isn't the same searcher intent as prospect research. Replaced with a single link-out at the bottom of the page. This FAQ is now 100% prospect-facing, matching its own stated northstar.
- Opening a question isolates it — sibling questions hide until it's closed.
- Rows cascade in one at a time on open, JS-timed (not CSS animation-delay — that broke silently once and is not going back).
- Search bar: Airbnb-style pill, grey idle / white raised on focus. Red squircle button morphs into a labeled "Search" pill on focus, elastic overshoot. Mic sits left-inside the input; the search button owns the right edge alone.
- Search matching ignores filler words ("how/much/is") and maps real synonyms (cost↔price↔pricing, photo↔picture↔image). Exact match tried first, then synonym match, then typo-tolerant fallback.

## Communication
- Replies: terse bullet list of what changed, nothing else. No play-by-play, no narrated tool use, no re-explaining a prior answer at length — a one-line pointer back to it is enough.
- Skip restating context the user already has.

## Rules
- Verify every visual/behavior change live (screenshot or computed style) before calling it done. Never claim untested.
- Push only after local verification passes: commit, push, poll the live URL.
- Make the exact edit asked for — no incidental scope, no revisiting things not flagged.

## Verification protocol — required before every push
1. Test at three widths, not one: mobile (375px), tablet (~768px), desktop (~1280px+). A fix confirmed only at one width is not confirmed.
2. Test the actual user flow, not just the new code path in isolation — open a question, close it, open a different one, search, clear search. Side effects on existing behavior are the real risk, not the new code itself.
3. New animation/motion work is the highest-risk category in this file's history — three separate regressions came from custom stagger/fade mechanisms that didn't re-measure a parent's height after finishing. Before shipping any new hide/show/collapse animation: confirm what re-measures the container afterward, not just that the new animation itself looks right.
4. State plainly what was and wasn't checked. "Confirmed via computed style, not a real screenshot" is a fine thing to say — a false "verified" is not.

Replaces `FAQ MD.txt`, the original design brief — now stale against the live build.

## Cutover, 2026-08-29/30

`index.html` originally redirected to `faq-v2.html`; **swapped 2026-08-30** so the short root URL (`https://ynnso.github.io/FAQ/`) serves the real content directly. `index.html` is now the active file — edit it. `faq-v2.html` is a thin redirect to `./`, kept only for old bookmarks/links.

## v2 redesign (`index.html`) — open items log, 2026-08-30 pass

**Primary test viewport: 393×852** (iPhone 15/16/17), not 375×667 — that's the stress-test floor only.

**Fixed this pass, code-verified + real screenshot, not yet confirmed on Sonny's real device:**
- H1 "FAQ" weight — root cause was the +35% chain never crossing 400 (CSS normal), so it stayed visually thin no matter how many steps stacked on a 200 start. Now 493, first step past regular. Font import trimmed to 400/493/800.
- Top 5 Trending font — a prior pass bumped the wrong element (`.search-suggest-row`, the list, not the header label). List reverted to 14px, header gets its own `.search-suggest-label--trending` class at 15.6px (+30%), scoped so "Matching Questions" is untouched.
- Mic & search icon sizing — checked again, already correct at 393×852 (static + simulated-focus). No change made, no bug found this round.

**Fixed this pass, code-only — needs Sonny's real-device confirmation, could not be verified further in this environment:**
- Mobile search pin / padding-under-screen-top clipping — root cause: no `safe-area-inset` usage anywhere in the file (flagged as a known gap since the original build). Added `viewport-fit=cover` + `env(safe-area-inset-top)` padding to `.sticky-header` and `.search-wrap.mobile-pinned`, plus a solid background on the pinned bar so nothing shows through. Resolves to a no-op on non-notched screens (verified via computed style) — cannot verify actual notch clearance without a real device.

**Automation limitation, confirmed again this pass:** the `computer` click tool times out specifically on this page's search-focus interaction (30s, no console errors). Worked around via synthetic PointerEvent/MouseEvent + `.focus()`, but `document.hasFocus()` is false in this environment, so real `:focus`/`:focus-within` CSS doesn't reliably engage even when `document.activeElement` is correct — computed DOM state is the strongest verification available here for anything gated on real focus.

Full detail in `REFERENCE.md`.
