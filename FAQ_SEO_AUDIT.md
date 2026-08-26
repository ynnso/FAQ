# FAQ SEO Audit — Current Build vs. Boosted Version

_Written 2026-08-25. Source materials: `files.zip` (5 research docs) + `FAQ SEO.html` (boosted draft), both already in this folder before this audit. Content/structure only — design and colors in `FAQ SEO.html` are explicitly out of scope per instruction._

Nothing in `index.html` (the live build) was changed to produce this file. This is analysis only.

---

## 1. What's actually different — hard counts

| | **Current (`index.html`)** | **Boosted (`FAQ SEO.html`)** |
|---|---|---|
| Sections | 6 | 11 |
| Total questions | ~57 | 67 |
| Section grouping | By department (Services, Staging, Booking, Company, **Dashboard**, **Account**) | By service type (Booking, Aerial, 360 Tours, Staging, Floor Plans, Video, Enhancements, Pricing, Delivery, Regional) |
| Question markup | `<span class="accordion-q">` inside a button — not a heading | Same pattern (`<span>` inside button) — **also not a heading** |
| Section markup | Real `<h2>` per section | Real `<h2>` per section |
| FAQPage schema | Present and in sync — 52 questions in the JSON-LD, 52 on the page (an earlier pass in this audit miscounted the page total; corrected here) | Present, one `Question` entry per accordion item, appears current |
| LocalBusiness schema | Absent | Present — includes a phone number and areaServed list (**needs a fact-check before reuse, see §4**) |
| Meta description / keywords | Absent | Present |
| Pricing | Not shown as a table anywhere; pricing lives in the app, not the FAQ | Opens with a pricing table (Services Overview) — **numbers need a fact-check, see §4** |

**Correction to the research docs:** `Shoot2Sell_Quick_Reference.md` says "no schema markup" and "questions are bold text, not headings." That was true of the *live production site* (shoot2sell.com/faq) the audit was run against — it is **not** fully true of this repo's `index.html`, which already has section `<h2>`s and a FAQPage schema block. The schema being stale (52 vs. 57+) and the questions still not being real headings are both still live problems here, just narrower than the research docs describe.

---

## 2. The technical-bloat problem, quantified

Per-section question counts in the current build:

| Section | Questions |
|---|---|
| Services & Pricing | 9 |
| Virtual Staging | 5 |
| Booking & Scheduling | 10 |
| Company & Policies | 7 |
| **Your Dashboard** | **12** |
| **Account & Login** | **14** |

**Dashboard + Account = 26 of ~57 questions — about 45% of the entire FAQ** is login help, password resets, headshot uploads, 403 errors, editing listing info. None of it is a query a prospective client would search. This is the research docs' Issue #2/#3 (technical bloat, diluted commercial intent), and it's the single biggest structural problem, worse than the docs' own estimate of "19 Q&As."

`FAQ SEO.html` handles this by dropping it entirely — its closest analog, "Delivery & Technical," is about file formats and usage rights, not login support. It does not attempt to preserve dashboard/account content anywhere.

---

## 3. Content pattern in the boosted version (worth keeping regardless of final structure)

Every answer in `FAQ SEO.html` opens with a **bolded, self-contained direct-answer sentence** ("Yes, next-day availability is our standard operation..."), then a bullet list of concrete specifics. Example:

> **Yes, next-day availability is our standard operation** across all major Texas metro areas.
> - Local Dispatch Crews: ...
> - Same-Day Openings: ...

This is the right shape for featured snippets / AI Overviews / voice answers (research docs' AEO/GEO sections) — the first sentence alone is a complete, quotable answer. Current `index.html` answers are conversational paragraphs without a bolded lead claim. This pattern is copyable independent of anything else in this audit — it's about answer construction, not page structure.

---

## 4. Fact-check flags — do not ship as-is

`FAQ SEO.html` was written as a demo/draft and contains claims that need verification against real Shoot2Sell data before any of it goes live:

- **Phone number** in the LocalBusiness schema (`+1-214-272-3200`) — confirm this is real or remove it.
- **Pricing table** ($159–$299 photography, $79–$119 aerial, etc.) — confirm against actual current rates. These do not obviously reconcile with PriceApp's own verified numbers (different products/scale, but worth a side-by-side before publishing a number publicly).
- **"5-7 days" for Agent Lifestyle Video at $999+** — not a service mentioned anywhere in the current FAQ; confirm it's real before including it.
- General tone of the boosted answers is more definitive/promotional ("standard operation," "case-by-case") than the current FAQ's phrasing — read each one against how the company actually operates before reusing verbatim.

---

## 5. Two different target structures — the docs don't agree with each other

- The audit report's own **Appendix (recommended structure)** proposes a lean **7 sections / ~26-32 Q&As** (Booking, Services & Delivery, Pricing, Regional, Image Rights, Payments, General).
- `FAQ SEO.html`, the actual boosted draft, went a different direction: **11 sections / 67 Q&As**, split finer by individual service (Aerial gets its own section, 360 Tours gets its own section, etc.) rather than one combined "Services & Delivery" bucket.

These are two legitimately different bets — lean-and-consolidated vs. granular-per-service — not one plan. Worth deciding explicitly rather than defaulting to whichever file happens to get copied from.

---

## 6. Recommended course

1. **Structure:** lean toward `FAQ SEO.html`'s granular per-service sections over the audit's own leaner appendix. Long-tail search traffic (the whole point of this exercise) tends to search per-service ("drone photography FAQ," "Matterport tour cost"), not a generic "Services & Delivery" bucket — granular sections give each service its own indexable target.
2. **Drop Dashboard + Account wholesale** from the SEO-facing page. That's 26 of ~57 questions with zero search value. If Photogs/clients still need that self-serve content somewhere, it belongs on a separate, non-indexed or `noindex`'d support page — not mixed into a page trying to rank.
3. **Reuse the bolded-lead-sentence answer pattern** from `FAQ SEO.html` regardless of which structure wins — it's the cheapest, lowest-risk win here (Part 3 above).
4. ~~Fix the schema drift~~ — corrected: the current build's schema is actually in sync (52/52). No action needed here after all.
5. **Fact-check everything in §4** before any of `FAQ SEO.html`'s numbers or claims get reused.
6. **Mechanically:** matches what you proposed — duplicate the current page as a new file, build the boosted content structure there, leave `index.html` (the live FAQ) untouched until the new version is fully reviewed and approved. Two live surfaces, zero risk to the current page while this is worked out.

---

## 7. Step 5 fact-check — checked against the real live site (2026-08-25)

Checked `FAQ SEO.html`'s numbers against shoot2sell.com directly (contact page, homepage, pricing page, video page, aerial page). Results:

**Confirmed real, safe to keep:**
- All 4 phone numbers in the LocalBusiness schema (DFW, Houston, Austin, San Antonio) — match the live contact page exactly.
- HD Video Tours: $159–$319 — matches the live video page exactly ($159 HD Highlight, $319 Full HD Walkthrough).
- Aerial Photography: $79–$119 — matches the live aerial page exactly, including the Mini (2 shots/$79) vs Standard (5 shots/$119) split.

**Contradicted — needs a fix, not just a shrug:**
- **Photography starting price is wrong.** `FAQ SEO.html`'s table says Photography (Standard) $159–$299. The live pricing page says photography actually starts at **$139** for homes, and is tiered by property type: $139 homes, $149 new construction, $159 rentals, $199 commercial, $249 multifamily. This isn't a rounding difference — $159 is presented as the floor when the real floor is $139.
- **This same wrong number is already live** — the current `index.html` FAQ says "between $159 and $549 per shoot." That floor is also stale against the real $139 starting price. Worth fixing on the *current* live FAQ regardless of what happens with v2.

**Unconfirmed — likely invented for the draft, don't ship as-is:**
- **"Agent Lifestyle Video" — probably the wrong name.** The real service on the live site's video page is called **"Agent Listing Video"** (agent on-camera intro to the property), not "Agent Lifestyle Video." No price is shown for it on the live site at all — the $999+ and "5-7 days" in the draft could not be confirmed anywhere.
- BuzzReels — the service itself is real ("3 different 10-20 second social media branded reels," per the live video page), but the $159 price for it isn't shown on the live site — unconfirmed.
- Aerial Video ($119–$249), Matterport ($149–$249), Zillow Showcase ($160–$299), Virtual Staging ($60–$220), Floor Plans ($79–$199) — not independently checked against a live page this round. Not contradicted, just not verified either.
- The LocalBusiness schema's `"priceRange": "$29-$999"` — the $999 lines up with the Agent video's unconfirmed price; nothing found matching a $29 floor.

**Bottom line:** don't copy `FAQ SEO.html`'s pricing table into v2 verbatim. Swap in the real photography tiers ($139/$149/$159/$199/$249), rename "Agent Lifestyle Video" to "Agent Listing Video" and drop its price/turnaround until you confirm real numbers, and get the remaining unconfirmed rows (aerial video, Matterport, Zillow Showcase, staging, floor plans) checked before this goes live.

---

## 8. Not decided yet — needs your call

- Final section structure: granular-per-service (11ish sections) vs. the audit's leaner 7-section version, vs. something in between.
- Where Dashboard/Account content goes (own page? stays gated? cut entirely?).
- URL/filename for the new duplicate page.
- Whether geo-targeted city pages (research docs' Issue #14, 4 market-specific pages) are in scope for this round or a later one — it's the most expensive item in the docs (4-6 hours) and wasn't part of what you asked me to audit yet.

Nothing here has been implemented. Say the word on structure and I'll duplicate the page and start building.
