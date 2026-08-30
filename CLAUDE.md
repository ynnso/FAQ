# FAQ — Shoot2Sell Photography

Single-file FAQ support page for Shoot2Sell, a Texas real estate photography company. Self-serve answers, not client-facing marketing.

**Working file:** `index.html` (mirror `FAQ.html` locally, sync before every commit)
**Live:** https://ynnso.github.io/FAQ/
**Repo:** github.com/ynnso/FAQ

## Stack
Vanilla HTML/CSS/JS. No framework, no build step. One `<script>` IIFE.

## Behavior, as built
- 52 real questions, 6 categories, one section open at a time. Clicking outside, or opening another section, auto-collapses the current one.
- Opening a question isolates it — sibling questions hide until it's closed.
- Rows cascade in one at a time on open, JS-timed (not CSS animation-delay — that broke silently once and is not going back).
- Search bar: Airbnb-style pill, grey idle / white raised on focus. Red squircle button morphs into a labeled "Search" pill on focus, elastic overshoot. Mic sits left-inside the input; the search button owns the right edge alone.
- Search matching ignores filler words ("how/much/is") and maps real synonyms (cost↔price↔pricing, photo↔picture↔image). Exact match tried first, then synonym match, then typo-tolerant fallback.

## Communication
- Replies: terse bullet list of what changed, nothing else. No play-by-play, no narrated tool use, no re-explaining a prior answer at length — a one-line pointer back to it is enough.
- Skip restating context the user already has.

## Rules
- Verify every visual/behavior change live (screenshot or computed style) before calling it done. Never claim untested.
- Push only after local verification passes: sync `FAQ.html`→`index.html`, commit, push, poll the live URL.
- Make the exact edit asked for — no incidental scope, no revisiting things not flagged.

## Verification protocol — required before every push
1. Test at three widths, not one: mobile (375px), tablet (~768px), desktop (~1280px+). A fix confirmed only at one width is not confirmed.
2. Test the actual user flow, not just the new code path in isolation — open a question, close it, open a different one, search, clear search. Side effects on existing behavior are the real risk, not the new code itself.
3. New animation/motion work is the highest-risk category in this file's history — three separate regressions came from custom stagger/fade mechanisms that didn't re-measure a parent's height after finishing. Before shipping any new hide/show/collapse animation: confirm what re-measures the container afterward, not just that the new animation itself looks right.
4. State plainly what was and wasn't checked. "Confirmed via computed style, not a real screenshot" is a fine thing to say — a false "verified" is not.

Replaces `FAQ MD.txt`, the original design brief — now stale against the live build.

## Cutover, 2026-08-29

`index.html` now redirects (meta-refresh + JS) to `faq-v2.html` — Sonny's decision, done with the 5 open bugs below still unresolved (chose cutover now over fix-first). `faq-v2.html` is the only real version going forward; `index.html` is a redirect shell only, not a page to edit.

## v2 redesign (`faq-v2.html`) — open items log, 2026-08-29

**Reported by Sonny, status STILL OPEN as of his last message ("again same issues") — do not mark resolved without his confirmation:**
- H1 "FAQ" weight not reading as +35% heavier.
- Mic & search icon sized wrong — expanded the search bar container height, text floats.
- Search bar not staying pinned to the mobile screen top when scrolling.
- Padding issue: something hiding/clipping under the screen top.
- Top 5 Trending font not reading +30% bigger.

**What was tried, 2026-08-29:** `grep` on the source showed H1 weight/icon sizing/trending font values matching what was asked for, and an `IntersectionObserver` fix was pushed for a sticky-header/pinned-search overlap. None of this was confirmed against a real click/scroll in the browser tool (timed out repeatedly) — only computed DOM state was checked. Sonny reports the same issues persisting after that push, so the `grep`-based "already correct" read and the overlap fix are both unconfirmed in practice — treat as still broken, re-investigate from scratch rather than re-checking the same values.
