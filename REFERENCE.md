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

## v2 redesign — bug status, 2026-08-30 pass
1. **H1 "FAQ" weight — fixed, root cause found.** The compounding "+35%" chain (200→270→365) never crossed 400 (CSS "normal"), so it could never read as heavier no matter how many +35% steps stacked on a 200 starting point — confirmed via computed style + a real screenshot. Now 493 (one more +35% step, first one past regular). Google Fonts import updated to load 400/493/800 instead of 200/270/365/400/800 (200/270 were dead weight, unused elsewhere).
2. **Mic & search icon sizing — already correct**, confirmed via real screenshot at 393×852 (static and simulated-focus state). No further change made.
3. **Top 5 Trending font — fixed, wrong element was targeted.** A prior pass applied the "+30%" bump to `.search-suggest-row` (the 5 list rows underneath, 14px→18px) instead of the "Top 5 Trending" header label itself — confirmed by reading the CSS, this is exactly why it looked like "the list got bigger, not the header" (Sonny's own read was correct). Reverted the list rows to 14px, added a dedicated `.search-suggest-label--trending` class (15.6px, +30% over the label's 12px) scoped to just that one label — "Matching Questions" (same base class, different search state) is untouched.
4. **Mobile search pin / padding-under-top-clipping — real root cause found and fixed, NOT confirmed on a real device yet.** No `safe-area-inset` usage existed anywhere in the file (a known gap flagged since the original build) — the pinned search bar and the compact sticky header both sat flush at `top:0` with zero clearance and, for the pinned bar, no background fill, so on any notched/Dynamic-Island phone real content could show through and/or get clipped under the status bar. Added `viewport-fit=cover` to the meta viewport tag, `env(safe-area-inset-top, 0px)` padding to both `.sticky-header` and `.search-wrap.mobile-pinned`, plus a solid white background on the pinned search bar so nothing shows through the new padding strip. `env()` resolves to `0px` with no inset (this dev environment, older phones) — verified no layout regression there via computed style. **Cannot verify the actual safe-area clearance in this environment — no real notched device available. Needs Sonny's confirmation on his phone.**

**Verification limitation hit again this session, same as before:** the `computer` click tool times out on this page's search-focus interaction specifically (30s timeout, no console errors — confirmed it's this exact interaction, not the page or the tool generally). Worked around it by dispatching synthetic PointerEvent/MouseEvent + `.focus()` via JS — but discovered `document.hasFocus()` is false in this automation context, so real `:focus`/`:focus-within` CSS and `focus`/`blur` event listeners don't reliably engage even when `document.activeElement` correctly shows the input focused. This is an environment-level limitation, not a page bug — computed DOM state (classes, computed styles) is the strongest verification obtainable here for anything gated on real focus/click. Item 4 above is the one still resting on that weaker verification.

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
