---
name: faq-audit
description: Audit, fix, or extend the Shoot2Sell FAQ (index.html in this repo) — pricing accuracy, AEO/search-lead-sentence structure, outbound link health, and JSON-LD schema hygiene. Use before shipping any pricing, naming, or content change to this file, or when asked to review/fact-check/optimize the FAQ.
---

# FAQ Audit — Shoot2Sell FAQ (C:\FAQ)

This skill captures a verification methodology proven on this exact file on 2026-09-01/02, after a prior session shipped real, confirmed wrong prices and conflated two distinct products under one name. Read `CLAUDE.md`, `HANDOFF.md`, and `REFERENCE.md` in this repo for full project context — this skill is the *procedure*, those files are the *state*.

## The one rule that matters most

**Never trust existing FAQ content at face value — verify every price, name, and fact against a real source before shipping it, even content that's already live.** This isn't hypothetical caution: this repo has three confirmed cases of a doc or a prior session's content being wrong (a flat ColorPop price that contradicted the project's own Golden Dataset, an "Agent Lifestyle Video is a naming mistake" claim that was itself the mistake, and a "not yet started" strategic item that was already done on the live site). Real sources, in priority order:

1. `FAQ Price Sheet.md` in this repo — the master rate sheet for every service
2. The live shoot2sell.com page for that service (fetch it, don't assume)
3. Cross-check against the separate PriceApp project's Golden Dataset when a number should match (same company, same math contract)

If none of these confirm a number, say so explicitly and ask — don't guess a plausible-looking figure.

## AEO check: does every answer lead with the actual answer?

A real, repeated bug this session: several "How much does X cost?" answers led with a generic sentence ("Enhancement pricing by service:") and never stated a number until deep in a bulleted list. Someone searching that exact question — or an AI answer engine quoting the first sentence — got nothing.

**Before calling any answer done, check its first sentence states the direct answer:**
- A "how much" question → the first sentence contains a real dollar figure
- A yes/no question → the first sentence is Yes/No, not "quality is important to us"
- A "what is X" question → the first sentence defines X, not a teaser

Run this check across every question after any pricing pass, not just the one you touched — a script like this catches it fast:
```js
const obj = JSON.parse(/* parse the FAQPage <script type="application/ld+json"> block */);
obj.mainEntity.filter(q => /^how much/i.test(q.name)).forEach(q => {
  const lead = q.acceptedAnswer.text.split(/(?<=[.:])\s/)[0];
  console.log(/\$\d|\d+%/.test(lead) ? 'OK  ' : 'MISS', q.name, '->', lead);
});
```

## Outbound links: intent, not volume

Link at genuine decision points — a definitional question and (this was a real gap found and fixed) the cost/pricing question for that same service, since that's the moment someone convinced by the price needs a next step. Don't force a link onto content with no natural single-service target (booking policy, delivery formats, dashboard/account, regional coverage) — that's the spam pattern to avoid, not a coverage goal to hit.

Before adding any link:
1. Confirm the target URL is real and live — `curl -s -o /dev/null -w "%{http_code}" -L <url>` should return 200. A soft-404 catch-all can also return 200 with a generic title — check the `<title>` too if the code alone looks suspicious.
2. Confirm the link content actually matches — a mismatch happened here once (a 100%-residential-toned Virtual Staging answer linked to the `/commercial-virtual-staging` page instead of the real, separate `/virtual-staging` page).
3. Match existing link markup exactly: `<p><a class="qa-link" href="..." target="_blank" rel="noopener">Link text<svg viewBox="0 0 24 24" fill="none" stroke-width="2.4" stroke-linecap="round" stroke-linejoin="round"><path d="M7 17 17 7M8 7h9v9"/></svg></a></p>`

## Schema hygiene — after every edit touching index.html

```js
const fs = require('fs');
const html = fs.readFileSync('index.html','utf8');
const m = html.match(/<script type="application\/ld\+json">([\s\S]*?)<\/script>/);
const obj = JSON.parse(m[1]); // throws if malformed — that's the check
const names = obj.mainEntity.map(q=>q.name);
console.log('count:', obj.mainEntity.length, 'duplicates:', names.filter((n,i)=>names.indexOf(n)!==i));
```
A duplicate `name` in one `FAQPage` block is a real Google Rich Results flag, not just untidy — even if the same text is legitimately shown twice in the visible UI across two categories.

## Verification before calling anything done

1. **Primary target: 393×852** (iPhone 15/16/17) — this is the design target per `CLAUDE.md`, not 375×667.
2. Use the `faq-server` dev config (`.claude/launch.json` in the sibling `C:\Claude` project) via `preview_start`, not a raw `file://` path — local files outside the active project root only render as static snapshots (no JS, no interaction) in the Browser pane.
3. This page's accordions need dispatched synthetic events, not the `computer` click tool (documented environment quirk — it hangs 30s page-wide on this file):
   ```js
   const btn = [...document.querySelectorAll('.accordion-trigger')].find(b => b.textContent.includes('...'));
   ['pointerdown','mousedown','pointerup','mouseup','click'].forEach(t => btn.dispatchEvent(new PointerEvent(t, {bubbles:true, cancelable:true})));
   ```
4. Check `read_console_messages` for errors after any interaction.
5. Stop the preview server when done.

## Commit & push

Commit locally after every verified change with a real postmortem-style message (what was wrong, what's now right, how it was verified) — matches this repo's existing git log convention, not a one-line summary. Push once link-health + JSON-LD + screenshot are all confirmed — this repo's own `CLAUDE.md` rule is: never claim "done" without real verification, state plainly what wasn't checked if something couldn't be.

## Communication

Terse bullet summary of what changed once work is underway — no narrated step-by-step. Don't append reminders about things Sonny already does reliably (cache-busting, hard-refreshing before reporting back) — he's asked explicitly not to be told this.
