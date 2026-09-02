# FAQ Landing Page — Handoff

_Last updated 2026-09-02. Read this first in a new chat — it replaces re-deriving context from scratch. `CLAUDE.md` and `REFERENCE.md` are the other two docs in this folder; read all three before touching code._

## 2026-09-02 update: Commercial / PORT content added

The FAQ was ~98% MLS/residential content with zero PORT/commercial-specific Q&A (verified by searching all schema text — confirmed via live audit, not assumed). Added a new **"Commercial Properties"** section, placed near the top (right after Services Overview, per Sonny's request), covering the 5 main services: Photo, Aerial (photo+video combined), 360/Matterport, Floor Plans, Video.

- 11 new Q&As, real content pulled from shoot2sell.com's own commercial pages (`/commercial-real-estate-photography-services`, `/commercial-aerial-photography`, `/commercial-aerial-video`, `/commercial-3d-tours`, `/commercial-floor-plans`, `/commercial-video-tours`) — each already has its own real FAQPage schema, so this wasn't invented content.
- Real PORT pricing tiers from `FAQ Price Sheet.md`, presented in **plain language, not the internal BCD/CR/AMF shorthand** (per Sonny's explicit direction): "Builders & Contractors," "Commercial & Retail," "Apartments & Multi-Family."
- 96 total questions now (was 83), 75 in the `FAQPage` schema (was 64), 14 categories (was 13).
- Also fixed in the same pass: the Virtual Staging pricing tile + Q&A link both pointed to `/commercial-virtual-staging` despite the content being 100% residential-toned — real, separate `/virtual-staging` page exists and is now linked instead.
- Also: 14 new outbound links added earlier the same session (enhancements had zero despite 6 real dedicated pages existing; cost questions dead-ended with no next step; aerial comparison questions only linked one of two real pages) — see git log for the individual commits, each independently verified (live link check + screenshot + JSON-LD validation) before push.

**Not yet done:** Enhancements' PORT-side variants (if any exist — not checked), and no PORT-specific Zillow Showcase content (unclear if Zillow Showcase applies to commercial properties at all — not verified either way).

## 2026-09-02 (later same day): fresh-eyes review + fixes

A step-back "what stands out" review (not chasing bugs, judging the page as a whole) surfaced real items, fixed in order:

- **Citation fact-check:** checked ~24 statistical claims against real current sources. Fixed 6 confirmed-wrong ones (a "33% faster (NAR)" staging stat was actually RESA's number, not NAR's; "83%" should be 81%; a "85% prefer short-form video" HubSpot claim had no matching real figure and was replaced; "5.3x engagement" was really ~67% more; Zillow "2x more saves" was really 68%; "9% of agents use video" had no real source anywhere near that low). Added 2 missing citations to real-but-uncited aerial stats. ~9 claims remain unverified, flagged not assumed fine.
- **Per-question URL anchors:** all 97 (now 76) questions got a stable `id`, with real bidirectional deep-linking (land on a hash → opens and scrolls to that answer; open any answer manually → URL updates via `replaceState`). Was a real structural gap — zero anchors existed before, so nothing could be deep-linked or cited to a specific answer.
- **Core Web Vitals — measured, not assumed:** LCP 384-484ms, CLS 0.0000, FCP 368-468ms on the real live production URL. The 533KB single-file size (flagged earlier as a risk) turned out not to matter — one request, no render-blocking assets, GitHub Pages compression handles it. No fix needed; the original concern was wrong.
- **Title/meta description fixed:** was still describing the pre-Commercial-Properties page ("Real Estate Photography FAQ," no mention of commercial or pricing). Updated across all 6 locations (title, meta description, OG, Twitter) plus the H1's hidden keyword span. Found and fixed a stray embedded `\r` character on the title line as a side effect (harmless to rendering, would've broken future exact-string edits there).
- **Dashboard/Account section removed entirely** (21 questions) — real content already lives at `shoot2sell.com/dashboard-help` (verified: same exact question titles, live page). Replaced with one link-out line at the bottom of the page. **76 questions now** (was 96), **12 categories** (was 14), **75 unique in the FAQPage schema** (unchanged — these were always excluded from schema, so this was a pure page-weight/focus win with zero schema-side change).

**Still open, not started:** Trust/E-E-A-T signal gap (no visible authorship/credentials block) — named, not acted on, real project scope not a quick fix.

**Checked and resolved (no action needed):** `dashboard-help` does also cover Booking & Scheduling (5 of 6 questions closely match ours in topic) plus Services & Delivery and Payments & Copyright. But unlike Dashboard/Account, this is NOT a clean duplicate worth removing — `dashboard-help`'s versions are vaguer than ours (no dollar amounts on cancellation/weekend/rush fees, where ours has real numbers from the price sheet fixed earlier this session). Removing our content in favor of theirs would be a downgrade. Two real, separate findings from this check, noted but not acted on per Sonny:
- **Real conflict on the parent site itself:** `dashboard-help` states photography "ranges from $159–$549 per shoot." Our FAQ says $139 starting, verified three ways (live pricing widget, this page's own pricing tile, PriceApp's Golden Dataset) earlier this session. Two pages on shoot2sell.com now disagree with each other — not something this repo can fix, since it's the other page that's likely stale, not ours. Worth knowing if this surfaces again.
- **Real content gap:** `dashboard-help` has "Can I request a reshoot if I don't like the photos?" — not answered anywhere in our FAQ. Real reshoot fee exists in `FAQ Price Sheet.md` ($89 MLS). Could be added; wasn't, on request.

## 2026-09-02 (later still): added a real "Real Estate Photography" section

Sonny caught a real structural gap: every other core service (Aerial, 360, Staging, Floor Plans, Video) had its own dedicated section — the company's actual namesake service, base residential photography, had none. Its only mention was one pricing question buried inside "Pricing & Packages."

Sonny also pasted an unsourced claim that Google recommends "15-30 questions in 3-5 categories" for a main FAQ page. Checked it the same way as the earlier SEO-skills list: that specific figure isn't traceable to any real Google documentation. Real sources give a much looser range (3-8 for most pages, 5-10 for "pillar" content, some say no set ideal). The specific number was wrong, but the underlying concern was real and, if anything, understated -- even the most generous real range is far below where this page now sits.

Fixed the concrete, unambiguous part now (the missing section); the volume question (82 questions total) is a much bigger, separate content-strategy decision, not resolved here.

- New **"Real Estate Photography"** section, inserted between Booking & Scheduling and Aerial Photo & Video. 6 questions: the existing, already-verified pricing answer (cross-listed, same pattern as the Floor Plans duplicate), plus 5 new questions sourced from the real, live `shoot2sell.com/real-estate-photography` FAQ (shoot length, on-site requirement, free single-page website inclusion, homeowner-vs-agent booking, delivery turnaround) -- not invented content.
- **82 questions now** (was 76), **13 categories** (was 12), **80 unique in the FAQPage schema** (was 75).
- Caught and fixed a real gap while building this: the 6 new accordion items didn't get the `id="q-..."` deep-link anchors, since those were added by a one-time script run before this section existed. Added manually, verified unique against all 82 ids file-wide, verified live via cold-load deep-link.

**Still open:** the page-volume question (82 questions vs. the 3-10 range real sources suggest) -- named, not decided. Trust/E-E-A-T gap -- still open. `dashboard-help` reshoot-question gap and the $139/$159 site-wide pricing conflict -- still just noted, not acted on.

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
