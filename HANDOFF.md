# FAQ Landing Page — Handoff

_Written 2026-08-30. Read this first in a new chat — it replaces re-deriving context from scratch. `CLAUDE.md` and `REFERENCE.md` are the other two docs in this folder; read all three before touching code._

---

## 1. What this is — full context & northstar

**Shoot2Sell** is a Texas real estate photography company (DFW, Houston, Austin, San Antonio). This repo is their **self-serve FAQ support page** — a single-file HTML/CSS/JS site, not client-facing marketing, not the main shoot2sell.com site.

**Northstar:** one page that (a) genuinely answers real customer questions across every service line (photography, aerial/drone, Matterport/360, virtual staging, floor plans, video, pricing, delivery, dashboard/account support), (b) ranks well on Google and reads well to AI answer engines (AEO), and (c) routes real interest back out to shoot2sell.com's own per-service pages, which is where booking/conversion actually happens — this FAQ page is a hub, not the sales surface.

Sonny (the user) is hands-on, detail-oriented, and has zero tolerance for unverified "done" claims — see `CLAUDE.md`'s two binding rules (NEVER ASSUME / NEVER SUBMIT UNTIL VERIFIED), both written this session after real, repeated failures earned them.

---

## 2. Folders / files

All in `C:\FAQ\` (single flat folder, no subfolders):

| File | Role |
|---|---|
| `index.html` | **The entire site.** ~492KB single file — HTML, all CSS (one `<style>` block), all JS (one `<script>` IIFE). No build step, no framework, no external JS deps. |
| `CLAUDE.md` | **Read this first.** Binding project rules: two hard-won verification rules at the very top (NEVER ASSUME — TEST CODE; NEVER SUBMIT UNTIL VERIFIED — desktop+mobile, visual+functional, ≥95% confidence), then stack/behavior notes, communication style, and a dated pass log. |
| `REFERENCE.md` | Detailed status log — current state, what's fixed and how it was verified, content fact-check flags, decisions made, real lessons learned (the pricing-table saga specifically). Longer-form than `CLAUDE.md`. |
| `HANDOFF.md` | This file. |
| `faq-v2.html` | Thin redirect shell to `./` — kept only so old bookmarks/links to `/faq-v2.html` still land on the real page. Not the active file, don't edit. |
| `robots.txt` | `User-agent: * / Allow: /` + sitemap reference. Added 2026-08-30, didn't exist before. |
| `sitemap.xml` | Single-URL sitemap (`https://ynnso.github.io/FAQ/`). Added 2026-08-30, didn't exist before. |
| `README.md` | 1 line, effectively empty. |

**Not in the repo, deleted 2026-08-30 as fully migrated/superseded:** `FAQ.html` (old mirror), `FAQ SEO.html` (raw draft, content migrated into `index.html`), `files.zip` (research docs, findings captured in `REFERENCE.md`), `.gitignore` (dead rule), old `HANDOFF.md`/`PROGRESS.md`/`FAQ_SEO_AUDIT.md`/`FAQ_SEO_PLAN.md` (consolidated into `REFERENCE.md`).

---

## 3. Links

- **Live site:** https://ynnso.github.io/FAQ/ — confirmed matching local `HEAD` byte-for-byte as of this write.
- **Redirect:** https://ynnso.github.io/FAQ/faq-v2.html → redirects to the line above.
- **Repo:** https://github.com/ynnso/FAQ (public — required for GitHub Pages on the free plan; was briefly made private this session, which took the site offline, then reverted).
- **Local dev server:** `C:\Claude\.claude\launch.json` → `faq-server` config. Ad hoc PowerShell `HttpListener` serving `C:\FAQ` — root `/` maps to `index.html`. `autoPort:true` is set (the default port 8934 is often already in use by another session; the tool reassigns automatically).
- **Real business site (linked out to, not part of this repo):** shoot2sell.com — the pricing tiles and 6 definitional Q&A answers link to real pages there (`/aerial-photography`, `/matterport`, `/zillow-showcase`, `/floor-plans`, `/commercial-virtual-staging`, `/video-tours`, `/real-estate-photography`, `/commercial-real-estate-photography-services`, `/contact`).

---

## 4. Latest updates (this session, 2026-08-30 — very long session, many passes)

Roughly chronological, most recent last. Full detail and root-cause writeups are in the git log (`git log --oneline`) and `REFERENCE.md` — every commit message this session is a real postmortem, not a one-liner.

**Early passes:** H1 weight, Top 5 Trending font, MLS/PORT text color, active section (black fill)/active question (white fill), 360 icon (rebuilt 3 times — final version is pure vector strokes, zero font dependency), search bar white-at-rest, search/section alignment (fluid `clamp()` formula, not a fixed px guess), pricing table hides during search, buried-answer-after-picking-a-suggestion.

**Sticky search bar — real fix:** `position:sticky` structurally cannot survive scrolling past its own short containing block (proved via `rect.top` reaching `-760px`). Pivoted to `.sticky-header` (`position:fixed`), rebuilt as a real visual duplicate of the hero bar, hardened against two documented iOS Safari `position:fixed` failure modes.

**Full functional sweep:** all 83 questions across 13 sections opened/closed via real dispatched click events (the `computer` click tool hangs page-wide in this environment — worked around throughout via synthetic `PointerEvent`/`MouseEvent`). All search-dropdown paths tested individually. Zero failures.

**SEO/AEO pass:** title/H1 keyword content, canonical tag, OG/Twitter tags, `Organization`/`WebSite`/`BreadcrumbList` schema (deliberately no fabricated logo/phone/social — none exist anywhere in the repo), removed the 21 Dashboard/Account questions from the `FAQPage` schema (still on the page, just not diluting the commercial-intent signal), `robots.txt`/`sitemap.xml` added.

**Outbound linking:** all pricing tiles already linked to real shoot2sell.com pages (confirmed, not built new); added a visual arrow signal; fixed one wrong link (Virtual Staging → was `/blue-skies-green-grass`, corrected to `/commercial-virtual-staging`); added matching outbound links inside the 6 definitional Q&A answers (Zillow Showcase, Aerial, Matterport, Virtual Staging, Floor Plans, Video).

**Pricing table — the long saga, worth reading in full in `REFERENCE.md`:** a chain of real, sequential bugs, each one only visible after the previous fix shipped: divider lines merging → arrow too small → arrow overlapping name text → columns not actually equal (`1fr` needs `minmax(0,1fr)`) → arrow dislocated at wide desktop tiles → dollar-sign superscript broken by a flex conversion → Agent Brand Video/Contact-us line-wrap regressions (twice, from unreliable verification methods) → Agent Brand Video tile height collapsing (alone in its grid row, nothing to stretch against) → Contact-us arrow disconnected from its text (a `space-between` overcorrection, reverted) → **zig-zag arrow alignment on price tiles** (arrow position followed each price's own digit width instead of a fixed column — fixed last, confirmed via real screenshot + exact x-coordinate measurement across every tile).

**Two new binding rules added to `CLAUDE.md`** as a direct result of this saga — read them before making any layout change to this file.

---

## 5. Next steps

1. **Nothing is currently known-broken.** Everything above is either verified via real measurement in this environment or explicitly flagged as unverifiable here (see §6). If Sonny reports something new, don't guess — ask for a screenshot, then measure, per `CLAUDE.md`.
2. **The biggest remaining strategic item from the SEO critique, not yet started:** splitting the highest-intent service clusters (Aerial, Matterport/360, Floor Plans, Virtual Staging) into their own dedicated URLs on shoot2sell.com's own domain (not this repo — this repo just links out). This is a real project scope, not a quick fix — hold until Sonny explicitly wants to pick it up.
3. **Content fact-checks still open** (see `REFERENCE.md` §"Content fact-check flags"): Aerial Video, Matterport, Zillow Showcase, Virtual Staging, Floor Plans pricing rows haven't been independently verified against the live shoot2sell.com pricing pages. Don't ship new pricing numbers without checking.
4. Low-priority, not urgent: splitting the inline `<style>`/`<script>` into external cacheable files (492KB single file, but no images, so no real waterfall cost — this is a "someday" item, not a bug).

---

## 6. Lingering / current issues — real, unresolved, can't fix from here

- **Nothing has been confirmed on a real iPhone this entire session.** Every fix is either verified via Chrome-based emulation at specific pixel widths, or via direct DOM/CSS measurement. Real device font rendering, real iOS Safari `position:fixed`/`backdrop-filter` behavior, real notch/safe-area clearance — none of it has ever been seen on an actual phone. This is a permanent gap of this environment, not something to keep re-attempting.
- **`document.hasFocus()` is `false`** in this automation context, so real `:focus`/`:focus-within` CSS and focus/blur event listeners don't reliably engage even when `document.activeElement` is correct. Anything gated on real focus rests on weaker verification here — computed DOM state, not a true interaction test.
- **Screenshots render corrupted/duplicated at tablet (768px) and desktop (1280px)** specifically, in this Browser pane tool. DOM measurement is used instead at those widths — confirmed reliable, but it means "I took a screenshot" isn't possible to claim honestly at those two breakpoints.
- **Screenshots can show a stale render for 1-2 seconds right after a DOM change** — a real, repeated tool quirk this session, not a page bug. Always re-screenshot after a wait, or better, measure directly (`getBoundingClientRect`, `getClientRects()` on text nodes) — this exact quirk caused a false "confirmed 2-line" report earlier this session that turned out to still be broken.
- **The `computer` click tool hangs (30s timeout) on this page specifically**, page-wide, not just on one element. Worked around via dispatched `PointerEvent`/`MouseEvent`, which does exercise real event listeners — but it's a workaround, not a fix, and every future session will likely hit this again.

---

## 7. Instructions for next session

1. **Read `CLAUDE.md` in full first**, especially the two rules at the very top. They exist because of real, repeated failures this session — don't treat them as boilerplate.
2. **Read `REFERENCE.md`** for the detailed fix history and the specific measurement techniques that actually work in this environment (text-node `getClientRects()` for line-wrap, exact `getBoundingClientRect()` comparisons for alignment) versus the ones that quietly lie (screenshot-right-after-a-change, height÷line-height arithmetic near any flex row).
3. **Verify against the live site first** (`curl -s "https://ynnso.github.io/FAQ/?nocache=$(date +%s)"`) before assuming local state matches what's deployed — confirmed identical as of this write, but re-check at the start of a new session regardless.
4. **Push policy:** commit locally freely; push only after real verification passes (per the new `CLAUDE.md` rule) — desktop AND mobile, visual AND functional, ≥95% confidence, explicitly checked against side effects on the rest of the page.
5. **Terse replies only** once work is underway: bullet list of what changed, nothing else — per `CLAUDE.md`'s existing Communication section. This handoff doc is the exception, not the norm.
6. If Sonny sends a screenshot showing something wrong: don't theorize from the screenshot alone. Reproduce it locally, measure precisely (not by eye), find the actual root cause, and say plainly what's verified versus what isn't — the whole point of this session's two new rules.
