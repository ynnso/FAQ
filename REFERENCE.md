# FAQ Project — Reference

_Consolidated 2026-08-30 from CLAUDE.md, HANDOFF.md, PROGRESS.md, FAQ_SEO_AUDIT.md, FAQ_SEO_PLAN.md._

## What this is
Shoot2Sell (Texas real estate photography) self-serve FAQ page. Not client-facing marketing.

## Current state
- **`faq-v2.html`** is the only live version. `index.html` is a redirect shell (meta-refresh + JS) pointing to it — kept only so the root URL still works.
- **`index.html`**'s old content (original 6-section/57-question build, dark/light theme) is gone from the live file but survives in git history. Local `FAQ.html` mirror was deleted 2026-08-30.
- Repo is public again on GitHub (`github.com/ynnso/FAQ`) after a brief private stint broke GitHub Pages (free plan doesn't serve Pages from private repos) — visibility flipped back, Pages source had to be manually re-enabled in Settings → Pages, confirmed live again.
- Live: https://ynnso.github.io/FAQ/ → redirects to https://ynnso.github.io/FAQ/faq-v2.html

## v2 redesign — still-open bugs (unconfirmed fixed)
Reported by Sonny, repeated twice, cutover to v2 happened anyway on his call:
1. H1 "FAQ" weight not reading heavier than it should
2. Mic & search icon sizing wrong — expands search bar container, text floats
3. Mobile search bar not staying pinned to screen top on scroll
4. Padding/clipping under the screen top
5. Top 5 Trending font not reading bigger

Prior fixes were verified only via computed DOM state, not real click/scroll — automation kept timing out. **Do not mark these resolved without either a real click/scroll test or Sonny's own confirmation.**

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
| `faq-v2.html` | Active, only real version |
| `index.html` | Redirect shell only, do not add content here |
| `FAQ SEO.html` | Source draft v2 content was migrated from — reference only |
| `files.zip` | Research docs behind the SEO audit — reference only |
| `CLAUDE.md` | Binding project rules (communication style, verification protocol) |
| `HANDOFF.md`, `PROGRESS.md`, `FAQ_SEO_AUDIT.md`, `FAQ_SEO_PLAN.md` | Superseded by this file — kept for history, not needed for day-to-day reference |

## Rules that still apply (from CLAUDE.md)
- Verify every visual/behavior change live before calling it done — computed-style checks alone are not sufficient, say explicitly when that's all that was possible.
- Terse replies: bullet list of what changed, nothing else.
- Push only after local verification passes.
