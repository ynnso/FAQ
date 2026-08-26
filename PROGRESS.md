# FAQ Project — Progress

_Last updated: 2026-08-25_

## Status

Dark/light theme system is **shipped and live** at https://ynnso.github.io/FAQ/
(commit `7d2aa12`). PriceApp (C:\Claude) work is closed — do not touch unless
reopened.

## What shipped this pass

- Brand red swapped: `#C8102E` → `#e60000` (light) / `#ff2a2a` (dark)
- 5 visual bug fixes on the accordion/section UI (all confirmed live):
  - Removed red glow on active section header
  - Section icon stays black/neutral when active, not red
  - Removed red left border on open answer
  - Removed grey line at bottom of open answer
  - Answer close (X) icon is black/neutral, not red
- Full dark/light theme system:
  - 3-state token pattern: `:root` (light default) → `@media (prefers-color-scheme: dark)` guarded by `html:not([data-theme="light"])` → `html[data-theme="dark"]` override for explicit toggle
  - Flash-of-wrong-theme prevention via early inline `<script>` in `<head>` reading `localStorage` before body paints
  - Real toggle button (`#themeToggle`, sun/moon icon swap), persists to `localStorage` under `faqTheme`
  - `.search-backdrop` dark override (`rgba(0,0,0,0.55)` vs light's weaker `rgba(10,10,10,0.28)`)
- Fixed real bugs found during the work:
  - `.hero` gradient had a hardcoded `rgba(200,16,46,...)` red (can't be split from a CSS var into rgba channels) — missed by the token-only swap, fixed by hand
  - `.sticky-header` had two colliding `transition:` shorthand declarations (second silently overwrote the first) — merged into one, plus added a dark-mode background override (was hardcoded white, stayed white in dark mode)
  - **Footer bug**: `footer { background: var(--black) }` — `--black` is a semantic token that flips to near-white (`#f2f2f2`) in dark mode, so the footer would've gone light while its text stayed white (invisible). Fixed by pinning footer to a literal `#0a0a0a`, independent of theme.

## Verification performed

- Real dark-mode screenshots at mobile width (375px) — accordion open state, footer, header all confirmed visually correct
- Computed-style checks (via live JS in the actual rendered page, not guesses) at tablet (768px) and desktop (1280px) confirming: `data-theme` toggles correctly, body/header/footer background and text colors flip as expected, `.main` max-width behaves correctly (860px cap on desktop)
- **Known gap**: tablet/desktop screenshots came out visually corrupted (squished into a narrow column) due to a Browser-pane tool rendering artifact when serving from a local ad hoc PowerShell HTTP listener — NOT a real page bug. Confirmed via DOM measurement (`getBoundingClientRect`, `window.innerWidth`) that actual layout width was correct at both sizes. Real screenshots at 768/1280 widths were not obtained — computed-style/DOM checks stood in for them this round.
- Brace-balance check (Perl, counts `{`/`}` in the `<script>` block) passed before commit
- Synced `FAQ.html` → `index.html`, committed, pushed, polled live URL to confirm deploy

## Architectural notes (theming-specific)

- CSS custom properties can't be decomposed into r/g/b channels for `rgba()` — any hardcoded `rgba(R,G,B,alpha)` tied to a brand/theme color must be manually kept in sync by hand whenever that token changes. Two such spots existed (`.hero`, now fixed) — worth grepping for `rgba(` literals again if the palette changes further.
- Any element meant to be **intentionally theme-invariant** (like the footer) must use a literal color, never a semantic token — semantic tokens (`--black`, `--white`) are defined as "text/background roles," not literal colors, and they intentionally invert in dark mode.
- Shorthand CSS properties (`transition`, `background`, etc.) silently overwrite rather than merge when declared twice in the same rule — this has bitten the project multiple times (`.hero`, `.sticky-header`) and is worth a final grep pass if more theme work is done.

## Pending / not yet done

- FAQ SEO content-migration plan (from `FAQ SEO.html` / audit research in `files.zip`) — **paused, not cancelled**, awaiting explicit user go-ahead to resume. Do not restart without the user bringing it back up.

## Verification round — 2026-08-25

Both pending visual-confirmation items closed out with real screenshots.

- **Tablet (768px) and desktop (1280px), dark mode:** confirmed clean via real screenshots. The corrupted-render tool artifact from last round did not recur once a tall viewport (height set to 2000–2400px) was used instead of scrolling — the local PowerShell listener + normal-height scroll combo has a scroll/coordinate desync bug in the Browser pane tool (screenshots showed stale scroll position vs. actual DOM). Workaround: render at a tall fixed height so the whole page fits with no scroll needed, screenshot, done. Worth remembering if this comes up again.
- **Mic-error bubble:** background/text correctly invert together in dark mode (light bubble, dark text) — readable, not the footer-style bug.
- **Back-to-top button:** visible bottom-right, dark circle, white icon, correct.
- **Isolate mode:** confirmed on Company & Policies — opening one question hides its siblings, no dark-mode issue.
- **Search dropdown:** dark rows, white text, red match-highlight, dimmed backdrop behind it — all correct.

No visual bugs found in this pass.
