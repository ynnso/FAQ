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

## Rules
- Verify every visual/behavior change live (screenshot or computed style) before calling it done. Never claim untested.
- Push only after local verification passes: sync `FAQ.html`→`index.html`, commit, push, poll the live URL.
- Make the exact edit asked for — no incidental scope, no revisiting things not flagged.

Replaces `FAQ MD.txt`, the original design brief — now stale against the live build.
