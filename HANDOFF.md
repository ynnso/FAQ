# FAQ Landing Page — Handoff

_Written 2026-08-30. Read this first in a new chat — it replaces re-deriving context from scratch._

---

## 1. Background — what this project is

Shoot2Sell (Texas real estate photography company) FAQ support page. Self-serve answers, not client-facing marketing. Two builds now exist side by side:

- **`index.html`** — the live, original FAQ. 6 sections (~52 Q&A), light/dark theme, Airbnb-style search pill. This is what's currently live at the public URL.
- **`faq-v2.html`** — an SEO-optimized redesign in progress, built per `FAQ_SEO_PLAN.md`. New content structure (11 sections, real content migrated from a boosted draft + fact-checked), bold "poster" visual style (inspired by a Google Stitch mockup Sonny shared), dark mode fully removed (light-only by decision), redesigned hero/search/pricing tiles. **This is the file almost all recent work has gone into.**

`faq-v2.html` is **also live** — GitHub Pages serves any file in the repo, not just `index.html`, so it's reachable directly (see Links). Sonny has been testing it live on his real phone this whole session, not a local preview.

---

## 2. Folders / files

All in `C:\FAQ\` (single flat folder, no subfolders):

| File | Role |
|---|---|
| `faq-v2.html` | **Active working file.** SEO redesign, poster style. All current edits go here. |
| `index.html` | Live production original. Tracked in git. Not touched during the v2 redesign — kept as-is on purpose. |
| `FAQ.html` | Local mirror of `index.html`, kept in sync before commits (per old workflow in `CLAUDE.md`). Currently **untracked** in git (not ignored, just never `git add`ed — check before assuming it's backed up). |
| `CLAUDE.md` | Project instructions, read automatically every session. Has the terse-communication rule, verification protocol, and the v2 open-issues log (section added 2026-08-29/30). **Keep this updated as issues are reported/resolved — Sonny checks it.** |
| `PROGRESS.md` | Stale — last updated 2026-08-25, before v2 work and before dark mode was removed. Documents the old dark/light theme system on `index.html`. Historical reference only, don't treat as current status. |
| `FAQ_SEO_PLAN.md` | The original 8-step plan for building v2. Steps 1–7 done; **Step 8 (side-by-side comparison, get Sonny's OK before going live anywhere real) was never explicitly closed out** — worth revisiting. |
| `FAQ_SEO_AUDIT.md` | Content audit comparing `index.html` vs. the boosted draft — structure/question-count diffs, fact-check flags (pricing numbers, phone number, "$999 video" claim). Reference doc, not meant to be edited. |
| `FAQ SEO.html` | The raw boosted SEO draft v2's content was migrated from. Untracked in git. Source material only, not served anywhere. |
| `files.zip` | 5 research docs behind the SEO audit. Untracked in git. Reference only. |
| `README.md` | 1 line, effectively empty. |
| `.gitignore` | Only ignores `FAQ MD.txt` (an old stale design brief, not present in the folder anymore). |

---

## 3. Links

- **Live (index.html):** https://ynnso.github.io/FAQ/
- **Live (faq-v2.html, in-progress redesign):** https://ynnso.github.io/FAQ/faq-v2.html
- **Repo:** https://github.com/ynnso/FAQ (public repo, GitHub Pages serves it directly — every push goes live within ~10 min, see caching note below)
- Local dev server config: `C:\Claude\.claude\launch.json` → `faq-server` config, serves `C:\FAQ` on `http://localhost:8934` (index.html at `/`, any file directly at its path).

---

## 4. Latest updates (this session)

- Sticky-header vs. mobile-pinned-search overlap: found via `getBoundingClientRect` that focusing the hero search (which pins it via `position:sticky` + `.mobile-pinned`) also caused the `IntersectionObserver` driving the compact `#stickyHeader` to show it at the same time — both rendered overlapping in the 0–64px band. Pushed a fix (observer now checks `.mobile-pinned` and suppresses the compact header while it's true). **Verified only via computed DOM state, not a real click** — real click automation timed out repeatedly in the browser tool this session (see §6).
- Confirmed via live fetch against the real deployed `faq-v2.html`: H1 weight (365), mic icon sizing (24px icon / 40px box) are correctly present in the deployed HTML right now.
- Found a real, previously-unlogged fact: GitHub Pages serves this repo with `Cache-Control: max-age=600` — a 10-minute browser cache window. Any "still looks wrong right after a push" report could genuinely be a stale cache on Sonny's device, not a code bug — **but this is not confirmed as the actual cause of his repeated reports, just a real contributing possibility.**
- Corrected a mistake in `CLAUDE.md`: an earlier update stated "already correct, likely cache" as if settled — Sonny flagged this explicitly ("md should reflect what i reported not what you assume"). The log now states his reports as still-open/unconfirmed, not my theory. **This distinction matters — don't repeat it.**

---

## 5. Next steps

1. Sonny reported (most recent message): **"again same issues"** — H1 weight, mic/search icon sizing, mobile search pin, padding/clipping under screen top, Top 5 Trending font size all still reported as wrong, despite code-level checks showing expected values. This needs a real resolution, not another round of `grep`/computed-state checks that don't match what he's seeing on his actual phone.
2. Get a real click/scroll test working in the browser tool (or find another way to reproduce his exact interaction — real device screenshot from Sonny, or a different automation approach) so the pin/overlap fix can actually be confirmed end-to-end, not just via forced DOM state.
3. Once the above is genuinely resolved and confirmed, revisit `FAQ_SEO_PLAN.md` Step 8 — side-by-side comparison against `index.html`, explicit sign-off before `faq-v2.html` replaces the live page (it currently does NOT replace `index.html` — both are live at their own URLs).

---

## 6. Lingering / current issues

- **Unresolved, reported twice by Sonny, not yet fixed for real:**
  - H1 "FAQ" weight not reading as heavier than it should.
  - Mic & search icon sizing — was reported as expanding the search bar container / causing floating text.
  - Mobile search bar not staying pinned to the screen top when scrolling.
  - Padding/clipping issue under the screen top (likely related to the sticky-header/pin overlap, but not confirmed fixed from his side).
  - Top 5 Trending font size not reading as bigger.
- **Automation limitation (real, environment-level, not a page bug):** the `computer` click tool has repeatedly timed out (30s) or silently failed to register clicks on this page's search-focus interactions, on both the real hero input and the sticky-header button. Verification this session fell back to computed DOM state (`classList`, `getBoundingClientRect`, `scrollY`) instead of true click-through — this is a weaker form of verification and should be flagged explicitly whenever used, per `CLAUDE.md`'s verification protocol.
- **`FAQ.html`/`files.zip`/`FAQ SEO.html` are untracked in git** — not backed up to the remote. Low risk (source/reference material, reproducible), but worth knowing.
- **`PROGRESS.md` is stale** and describes a dark/light theme system that has since been removed from `faq-v2.html` (light-only now, by explicit decision). Don't treat it as current.

---

## 7. Instructions for next session

- Read `C:\FAQ\CLAUDE.md` in full — it has the binding communication/verification rules (terse replies, no play-by-play, real verification required before calling anything done, push policy).
- Treat `faq-v2.html` as the active file. Do not touch `index.html` unless explicitly asked.
- Do not mark any of the §6 open issues resolved without either (a) a real click/scroll test that actually completes, or (b) Sonny's own confirmation. Code-level/computed-state checks alone have already produced a false "fixed" once this session — don't repeat that.
- When logging status in `CLAUDE.md`, log what Sonny actually reported, not a theory about why — he corrected this explicitly once already.
- Terse replies only: bullet list of what changed, nothing else.
