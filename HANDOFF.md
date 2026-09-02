# FAQ Landing Page — Handoff

_Last updated 2026-09-01. Read this first in a new chat — it replaces re-deriving context from scratch. `CLAUDE.md` and `REFERENCE.md` are the other two docs in this folder; read all three before touching code._

---

## 1. What this is — full context & northstar

**Shoot2Sell** is a Texas real estate photography company (DFW, Houston, Austin, San Antonio). This repo is their **self-serve FAQ support page** — a single-file HTML/CSS/JS site, not client-facing marketing, not the main shoot2sell.com site.

**Northstar:** one page that (a) genuinely answers real customer questions across every service line (photography, aerial/drone, Matterport/360, virtual staging, floor plans, video, pricing, delivery, dashboard/account support), (b) ranks well on Google and reads well to AI answer engines (AEO), and (c) routes real interest back out to shoot2sell.com's own per-service pages, which is where booking/conversion actually happens — this FAQ page is a hub, not the sales surface.

Sonny (the user) is hands-on, detail-oriented, and has zero tolerance for unverified "done" claims — see `CLAUDE.md`'s two binding rules (NEVER ASSUME / NEVER SUBMIT UNTIL VERIFIED). **As of 2026-09-01, treat every existing pricing/naming claim in this file as unverified too** — at least one prior session shipped real, confirmed wrong prices and conflated two distinct real products under one name. See §4 below and `REFERENCE.md`'s 2026-09-01 section.

---

## 2. Folders / files

All in `C:\FAQ\` (single flat folder, no subfolders):

| File | Role |
|---|---|
| `index.html` | **The entire site.** HTML, all CSS (one `<style>` block), all JS (one `<script>` IIFE). No build step, no framework, no external JS deps. |
| `CLAUDE.md` | **Read this first.** Binding project rules: verification rules at the top, stack/behavior notes, communication style, dated pass log. |
| `REFERENCE.md` | Detailed status log — current state, what's fixed and how it was verified, content fact-check flags, real lessons learned. Longer-form than `CLAUDE.md`. |
| `HANDOFF.md` | This file. |
| `FAQ Price Sheet.md` | **Added 2026-09-01. The real pricing source of truth** — master rate sheet for every shoot2sell service. Check any dollar figure in `index.html` against this before trusting or extending it. |
| `faq-v2.html` | Thin redirect shell to `./` — kept only so old bookmarks/links to `/faq-v2.html` still land on the real page. Not the active file, don't edit. |
| `robots.txt` | `User-agent: * / Allow: /` + sitemap reference. |
| `sitemap.xml` | Single-URL sitemap. |
| `README.md` | 1 line, effectively empty. |

---

## 3. Links

- **Live site:** https://ynnso.github.io/FAQ/
- **Redirect:** https://ynnso.github.io/FAQ/faq-v2.html → redirects to the line above.
- **Repo:** https://github.com/ynnso/FAQ (must stay public — GitHub Pages free plan doesn't serve from private repos; this broke once, was reverted).
- **Local dev server:** `C:\Claude\.claude\launch.json` → `faq-server` config. Ad hoc PowerShell `HttpListener` serving `C:\FAQ` — root `/` maps to `index.html`. `autoPort:true` is set (default port 8934 often already in use — the tool reassigns automatically). Serves the real file directly (unlike `file://` paths outside the project root, which some tooling only renders as static snapshots).
- **Real business site (linked out to, not part of this repo):** shoot2sell.com — pricing tiles and definitional Q&A answers link to real pages there, including `/real-estate-photography`, `/aerial-photography`, `/matterport`, `/zillow-showcase`, `/floor-plans`, `/commercial-virtual-staging`, `/video-tours`, `/commercial-real-estate-photography-services`, `/contact`, and (added 2026-09-01) `/agent-listing-video`, `/agent-lifestyle-video`.

---

## 4. Latest updates (2026-09-01 session — pricing fact-check pass)

Full detail and root-cause writeups in `git log --oneline` and `REFERENCE.md`'s 2026-09-01 section.

**Started from a real catch, not a routine review:** Sonny spotted the FAQ's "starting at $159" photography price didn't match the page's own pricing tile ($139). Pulling that thread surfaced a pattern of real errors — this wasn't one typo, it was systemic content that had never been checked against a real source.

**Fixed, verified against `FAQ Price Sheet.md` / live shoot2sell.com / Sonny's own product screenshots:**
- ColorPop: was flat $39/image, real is $39 first + $15 each additional (6 locations fixed)
- Floor Plan 2D/3D: $79/$199 → real $69/$99 (the $199 was the PORT/commercial rate, wrongly on the residential FAQ)
- Photography: real 7-tier sqft breakdown added, $119 Mini floor called out
- Blue Skies & Green Grass: was one $29 service, split into two real $29 add-ons (9 locations)
- **Agent Brand Video → two real products:** "Agent Listing Video" ($499) and "Agent Lifestyle Video Package" ($999) are separate, real products (confirmed via Sonny's own screenshots) — a prior session had merged them into one FAQ entry under a name neither product actually uses, with the $999 product's content wrongly attached and a wrong delivery estimate. Renamed to match the real site, added the missing product as its own Q&A, fixed delivery times and file-format specs.
- Removed a duplicate `Question` node in the `FAQPage` JSON-LD schema (legitimate UI duplicate, illegitimate schema duplicate)

**Verified:** JSON-LD valid after every edit (62 questions, no duplicate names), all outbound links live-checked (200 OK), pricing tile screenshot-confirmed at 393×852 (primary target) and 440×956 (ceiling) — clean, no clipping. 375×667 stress floor wraps the renamed tile to 3 lines instead of 2 (measured via `getClientRects()`, not eyeballed) but doesn't clip; Sonny explicitly accepted this rather than trade off the primary target.

**Also fixed this pass (doc accuracy, not code):** `CLAUDE.md` had stale facts — "52 real questions, 6 categories" (real: 83 questions, 13 categories) and a reference to a `FAQ.html` mirror file that was deleted weeks ago. `REFERENCE.md`'s own "content fact-check flags" list had a wrong entry (claimed Agent Lifestyle Video was a naming mistake — it's real). Both corrected.

**Not done this pass — was the original plan, got pre-empted by the fact-check chain above:** Matterport and Zillow Showcase have real tier ladders now available in `FAQ Price Sheet.md` but still show zero pricing detail in their Q&A text (only a single number on the pricing tile). This is the natural next task.

---

## 5. Next steps

1. ~~Matterport / Zillow Showcase enrichment~~ — **done 2026-09-01**, real tier data added to both Q&As.
2. ~~Booking/cancellation fee answers~~ — **done 2026-09-01**, real numbers applied ($29 weekend, $49 cancellation, $49/$79 rush).
3. ~~Aerial bundle combo pricing~~ — **done 2026-09-01**, all 4 real combo prices added.
4. ~~"Biggest remaining strategic item" — splitting service clusters into dedicated shoot2sell.com URLs~~ — **this was already done, on the real business site, before this doc was ever written.** Verified 2026-09-01: `shoot2sell.com/aerial-photography`, `/aerial-video`, `/matterport`, `/floor-plans`, `/commercial-virtual-staging` all already have their own dedicated pages with real, distinct per-service `FAQPage` schemas (confirmed via live fetch — e.g. `/aerial-video` has "How much does aerial video cost for real estate agents in Texas?" as its own question, not shared with this repo's FAQ). **This item never should have been on this list.** Sonny caught it by asking a plain question about the site's actual structure — a third confirmed case (after the Agent Video and question-count flags) of this doc carrying a claim that was never checked against reality. If a future session sees a "not yet started" item here, verify against the live site before trusting it.
5. Low-priority: splitting the inline `<style>`/`<script>` into external cacheable files — a "someday" item, not a bug.

---

## 6. Lingering / current issues — real, unresolved, can't fix from here

- **Nothing has been confirmed on a real iPhone by any session yet.** Every fix is verified via Chrome-based emulation or direct DOM/CSS measurement. Sonny does his own real-device check after each push — don't tell him to hard-refresh/cache-bust, he already does this thoroughly before reporting back.
- **`document.hasFocus()` is `false`** in this automation context — real `:focus`/`:focus-within` CSS doesn't reliably engage even when `document.activeElement` is correct.
- **Screenshots render corrupted/duplicated at tablet (768px) and desktop (1280px)** in the Browser pane tool specifically — use DOM measurement at those widths instead.
- **Screenshots can show a stale render for 1-2 seconds right after a DOM change** — re-screenshot after a wait, or measure directly.
- **The `computer` click tool hangs (30s timeout) page-wide on this page** — use dispatched `PointerEvent`/synthetic events instead (confirmed working again 2026-09-01).
- **`file://` paths outside the project's own working directory only render as static snapshots** in the Browser pane (no JS execution, no interaction) — use the `faq-server` dev-server config from `.claude/launch.json` instead when you need a live, interactive render of this repo.

---

## 7. Instructions for next session

1. **Read `CLAUDE.md` in full first.**
2. **Read `REFERENCE.md`**, especially the 2026-09-01 section — it documents a real case of a prior session's content being wrong and a fact-check flag *in this very doc* also being wrong. Don't trust either file's older claims without a source check when it matters.
3. **Before touching any price, check `FAQ Price Sheet.md` first**, or ask Sonny for the real numbers if the sheet doesn't cover it. Don't extend or "clean up" existing pricing text on the assumption it's already correct.
4. **Verify against the live site first** (`curl -s "https://ynnso.github.io/FAQ/?nocache=$(date +%s)"`) before assuming local state matches deployed.
5. **Push policy:** commit locally freely; push only after real verification passes — desktop AND mobile, visual AND functional, checked for side effects.
6. **Terse replies once work is underway** — bullet list of what changed, nothing else. This handoff doc is the exception.
7. **Don't append routine reminders** (cache-busting, hard-refresh, "test before reporting") to responses — Sonny already does this rigorously every time.
