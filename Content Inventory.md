# Shoot2Sell Content Inventory — Phase 1

_Built 2026-09-02. Real inventory of every FAQ-bearing page across the Shoot2Sell ecosystem — this repo's hub plus every dedicated page on shoot2sell.com. Every page listed here was live-fetched and its real question titles pulled directly (not estimated, not assumed). Use this before deciding what the hub should keep, cut, or route to._

## The headline finding

**This hub has 82 questions. The rest of shoot2sell.com has ~250+ real questions across 28 dedicated pages, plus ~40-50 more on the dashboard/account support page.** The individual pages are not weak — collectively they're roughly 3x richer than this hub. The hub's job in a hub-and-spoke model is routing and cross-service decision support, not depth — the depth already exists, distributed across pages built specifically for each topic.

## This Hub (ynnso.github.io/FAQ)

| Metric | Count |
|---|---|
| Total accordion questions | 82 |
| Unique questions | 80 |
| Categories | 13 |
| In FAQPage schema | 80 |

## Residential Service Pages

| Page | URL | Real Qs |
|---|---|---|
| Real Estate Photography | `/real-estate-photography` | 8 |
| Aerial Photography | `/aerial-photography` | 11 |
| Aerial Video | `/aerial-video` | 9 |
| Matterport 3D | `/matterport` | 9 |
| Zillow Showcase | `/zillow-showcase` | 7 |
| Floor Plans | `/floor-plans` | 8 |
| Video Tours (HD) | `/video-tours` | 10 |
| Virtual Staging | `/virtual-staging` | 7 |
| Agent Listing Video | `/agent-listing-video` | 9 |
| Agent Lifestyle Video | `/agent-lifestyle-video` | 9 |
| **Subtotal** | | **87** |

## Commercial (PORT) Service Pages

| Page | URL | Real Qs |
|---|---|---|
| Commercial Photography | `/commercial-real-estate-photography-services` | 7 |
| Commercial Aerial Photo | `/commercial-aerial-photography` | 7 |
| Commercial Aerial Video | `/commercial-aerial-video` | 7 |
| Commercial 3D Tours | `/commercial-3d-tours` | 7 |
| Commercial Floor Plans | `/commercial-floor-plans` | 7 |
| Commercial Video Tours | `/commercial-video-tours` | 7 |
| Commercial Virtual Staging | `/commercial-virtual-staging` | 7 |
| **Subtotal** | | **49** |

## Enhancement Pages

| Page | URL | Real Qs |
|---|---|---|
| Blue Skies & Green Grass | `/blue-skies-green-grass` | 8 |
| Aerial Green Grass | `/aerial-green-grass` | 8 |
| ColorPop | `/colorpop-aerial-photos` | 6 |
| Digital Twilight | `/virtual-twilight` | 8 |
| Twilight Photography | `/twilight-photography` | 8 |
| Keepsake Album | `/keepsake-album` | 7 |
| **Subtotal** | | **45** |

## Regional Pages

| Page | URL | Real Qs |
|---|---|---|
| Dallas-Fort Worth | `/dallas-fort-worth-photography-services` | 7 |
| Houston | `/houston-real-estate-photography` | 5 |
| Austin | `/austin-real-estate-photography` | 5 |
| San Antonio | `/san-antonio-real-estate-photography` | 8 |
| **Subtotal** | | **25** |

## Hub / Support Pages

| Page | URL | Real Qs |
|---|---|---|
| Services (catalog/booking guide) | `/services` | 6 |
| Dashboard & Account Help | `/dashboard-help` | ~38-50+ across 6 sections (Booking 6, Services & Delivery 6, General 3, Payments & Copyright 3, Account & Support 1, Dashboard & Virtual Tour 19) |
| About (trust/facts, not Q&A) | `/about` | 0 (facts page — founding 2011, 300K+ properties, FAA certs, in-house editing) |

## Real total, ecosystem-wide

87 + 49 + 45 + 25 + 6 + ~40 (dashboard, low estimate) ≈ **~252 real questions across 29 dedicated pages** — not counting this hub's 82.

## Real, notable pattern found across the inventory

**"What areas does Shoot2Sell serve?" and "Do we need someone on-site during the shoot?" appear near-verbatim on the large majority of the 28 service pages** (roughly 18-20 of them). This is real, deliberate consistency — every page answers the same core logistics questions — but at this scale it's also a real thin-content/repetition risk worth knowing about: the same 2 boilerplate Q&As are duplicated dozens of times across the domain. Not something to fix today — flagged for the classification pass in Phase 2.

## Next: Phase 2

Classify every one of this hub's 82 questions against this inventory:
1. **True duplicate** of a service-page question → candidate to cut from the hub
2. **Hub-only, no service-page equivalent** → decide: push out, or genuinely belongs on a hub (cross-service comparison, booking policy)
3. Cross-reference which service pages have content **not yet reflected or linked from the hub** at all

Not run yet — this file is the real input for that pass, not the pass itself.
