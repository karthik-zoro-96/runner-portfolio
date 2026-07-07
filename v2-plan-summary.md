# v2 Plan — Summary (revised after the audit, 2026-07-07)

> v1 = the page live on GitHub Pages. v2 = what we build next, gated on signal.
> **Revised 2026-07-07 after the audit** — Tier 0 added, Tier 1 resequenced, Layer B thresholds tightened.

---

## What the audit fixed

I claimed "CSV + build.js pipeline (15 sec per new race)" was already shipped. **It wasn't — the pipeline was never built anywhere.** The real workflow is hand-editing the `DATA` JSON in `index.html:533`, refreshed by pulling from the Strava MCP. **The plan is now corrected: building the pipeline is the prerequisite for everything else in Layer A.**

---

## Tier 0 — Build the data pipeline (BLOCKING)

Before any Tier 1 feature, this exists. Real workflow today:
- Pull latest from Strava MCP
- Hand-edit `DATA` JSON in `index.html:533`
- Find/replace, save, push

That's ~3-5 minutes per race update, not 15 seconds. And it's error-prone (one missing comma breaks the page).

| Step | Effort | What |
|---|---|---|
| 0.1 | 30 min | Create `races.csv` with the 8 existing races (one row each) |
| 0.2 | 2-3 hrs | Write `build.js` — Node script, reads CSV, writes `data.js` |
| 0.3 | 30 min | Update `index.html` to load `data.js` instead of inline JSON (one `<script src>` swap) |
| 0.4 | 30 min | Add a `node build.js` step to `.github/workflows/static.yml` before the "Upload artifact" step (this repo deploys via GitHub Actions → GitHub Pages, not Vercel) |
| 0.5 | 30 min | Migrate the 8 existing races into the CSV (validate output matches current page) |
| 0.6 | 1 hr | Add race photos to `photos/{date}-{slug}.jpg` (auto-detected by build) |
| **Total** | **~6-7 hrs** | Pipeline that makes everything below possible |

**After Tier 0:** adding a race = open CSV → add 1 row → `git push` → live. ~20 seconds.

---

## Where v1 ended

**Live:** `https://karthik-zoro-96.github.io/runner-portfolio/`

What's shipped:
- Single-file HTML, vanilla JS, no backend
- Real photos (hero race shot + Himalayan trek)
- 8 races across 4 years (Athlinks + STS + personal + Strava PRs)
- PB ladder with both race + Strava training PBs
- Career stats (325 runs, 1,128.7 km, 155h 55m)
- Dynamic features: year filter, distance filter, sort, race expand
- Currently training for: first full marathon, sub-3:30 (no target date yet)
- Hand-embedded `DATA` JSON in `index.html:533` (the bottleneck)

**What's working** → the page renders, the data is real, the photo + trek combo is unique, the share quote holds up.

**What's deferred** → all v2 features below.

---

## Layer A — Page v2 (single-user, build when you feel like it)

### Tier 0 — Prerequisite (do this first, before Tier 1)
- **CSV + build.js pipeline** — see above, ~6-7 hours, blocks everything else

### Tier 1 — Ship in the next 1-2 months (in actual dependency order)
| # | Feature | Depends on | Why | Effort |
|---|---|---|---|---|
| 1 | **Goal-race countdown** (`currently_training_for` target date + day-diff calc) | nothing | The Marathon row is the most-photographed gap. Adding a date + "X days" line plugs it cleanly | 1-2 hrs |
| 2 | **Race photo gallery** (per race, not just one) | Tier 0 | Multiple photos per race, not just hero. From `photos/{date}-{slug}.jpg` | 2-3 hrs |
| 3 | **Race-tag system** (tough, negative-split, PR-streak) | Tier 0 | Manual CSV column. Adds color to the history | 3-4 hrs |
| 4 | **Year-in-review auto-card** (Dec 1) | Tier 0 | Auto-generated "your 2026 in races" shareable image. Highest social value, lowest build cost | 4-6 hrs |
| 5 | **Print résumé** (dedicated @media print layout) | nothing | The current @media print just hides sections. A proper resume layout for the sponsorship pitch PDF | 1-2 hrs |

### Tier 2 — Ship when you have 30+ races
| # | Feature | Depends on | Why | Effort |
|---|---|---|---|---|
| 6 | **Strava OAuth** (auto-pull all-time bests + races) | Tier 0 | Replaces manual training PB entry + saves splits for free | 1-2 days |
| 7 | **Splits per race** | #6 (Strava) | When Strava import lands, splits come free. Not before. **Was incorrectly listed as Tier 1 in the previous plan** | 2-3 hrs (post-Strava) |
| 8 | **Search + advanced filters** (by name, time range, placement %) | Tier 0 | By name, time range, placement % | 4-6 hrs |
| 9 | **"Compare with me" widget** | Tier 0 | Visitors enter their 10K time → see how they stack up vs your PBs | 1 day |
| 10 | **Personalized training calendar** | Tier 0 | Weekly target pace + countdown for any goal race, not just first marathon | 4-6 hrs |

### What v2 page deliberately skips
- ❌ Comments / reactions
- ❌ Live data feed
- ❌ Anything that requires a backend (until Phase 2)
- ❌ Account system
- ❌ Multi-page (separate URL per race)
- ❌ Splits before Strava OAuth (was wrongly in Tier 1)

---

## Layer B — Product v2 (multi-tenant, gated on signal)

### Trigger conditions (revised, no traffic vanity)

| Signal | When it counts |
|---|---|
| **≥3 distinct runners DM you "how do I get one?"** | within 14 days of showcase post — **real trigger** |
| **A running club admin reaches out for a club-wide page** | any time — **real trigger** |
| **A brand DMs about sponsorship or partnership** | any time — **real trigger** |
| ~~≥100 unique visitors~~ | ~~don't count on its own — pair with intent signal~~ |

If 0 signals after 30 days: keep iterating on the post + the page, don't start the multi-tenant build.

### The Phase 1 (hand-curated batch) — quick to do, validates the community

**Before** building the SaaS, manually build 5-10 portfolios for runners who ask. Use the same `me.json` + `races.csv` + `index.html` template (after Tier 0 is built). Each takes ~30 min of your time.

**Why this matters:**
- You learn what data the 4 cohorts need (and where Athlinks / STS fails for them)
- You get real user stories for Phase 2
- You build the user base before building the product
- You validate the willingness to share / be featured

### Phase 2 (the SaaS build) — only after 10+ are sharing

**Tech stack (recommended):**
- **Frontend**: Next.js (app router, React Server Components for SEO)
- **Database**: PostgreSQL (via Supabase or Neon, free tier)
- **Auth**: Auth.js with magic link + Strava OAuth to start
- **Hosting**: Vercel (free tier for hobby, scales cleanly)
- **Storage**: Vercel Blob for race photos
- **Data imports**: Athlinks CSV paste, STS profile paste, Strava OAuth
- **Cost**: $0-25/mo for the first 100 users

**Build time**: 2-3 weeks of focused work.

**Features in v2 product:**
1. Auth (magic link)
2. Claimable slugs (`ladder.run/{your-name}`)
3. Auto-import from Strava (training + races)
4. Manual race entry form
5. Athlinks CSV paste import
6. STS profile paste import
7. OG card auto-generator (per profile)
8. Year-in-review auto-card (Dec 1 for everyone)
9. Public + private mode toggle
10. Sponsorship CTA (visible only on public mode)

### What v2 product deliberately skips
- ❌ Live tracking integrations
- ❌ Real-time anything
- ❌ Paid features (defer to v3 — sponsorship-funded model first)
- ❌ Mobile app (web-only forever)

---

## The actual decision tree

```
Week 1-4 post-launch:
  └─ Ship showcase post
  └─ Track DMs
  └─ Run races, add to JSON (manually) — 3-5 min per race
  └─ Iterate page if needed (rare)

Tier 0 (within 2 weeks):
  └─ Build the CSV + build.js pipeline (~6-7 hrs)
  └─ New race add drops to ~20 sec

Tier 1 (next 1-2 months after Tier 0):
  └─ Goal countdown (no deps, do first)
  └─ Photo gallery
  └─ Race tags
  └─ Year-in-review
  └─ Print résumé

If 0 Layer B signals at week 4:
  └─ Re-write the post, try different channels
  └─ Don't scale

If 3+ DMs at any point:
  └─ Phase 1: hand-build 10 portfolios (10 hours of your time)
  └─ Document what you learned
  └─ THEN consider Phase 2

If 10+ Phase 1 portfolios are sharing and 1+ wants multi-user:
  └─ Phase 2 build (2-3 weeks)
  └─ Launch with the 10 Phase 1 users as your first cohort
  └─ Open to public after they share

If a brand DMs:
  └─ Take the conversation seriously (this is real revenue)
  └─ Iterate the page to optimize for the brand pitch
```

---

## v2 timeline (rough)

| Phase | When | What | Effort |
|---|---|---|---|
| **Tier 0** | this week, before anything else | CSV + build.js + photo auto-detect | 6-7 hrs |
| **Tier 1** | next 1-2 months after Tier 0 | countdown, photos, tags, year-review, print résumé | 1-2 days total |
| **Tier 2** | when you have 30+ races | Strava OAuth, advanced filters, compare widget | 1 week |
| **Phase 1 (hand batch)** | when 3+ DMs | 10 manual portfolios | 10 hours |
| **Phase 2 (SaaS)** | when 10+ sharing | Next.js + Postgres + Auth | 2-3 weeks |
| **v3 (B2B)** | when clubs/teams ask | club mode, coach mode, paid tiers | 1+ month |

---

## The single most important next step

**Don't build v2 features yet.** Build Tier 0 first.

**The order that matters:**

```
1. Tier 0 (CSV + build.js pipeline)        6-7 hrs
2. Run a race, add to CSV, push             20 sec
3. Tier 1 #1: Goal-race countdown          1-2 hrs
4. Run another race, add to CSV, push       20 sec
5. Show the post, watch the DMs
6. Tier 1 features as needed
7. Layer B when signal arrives
```

If you skip Tier 0, every Tier 1 feature is more painful than it needs to be. The pipeline is the product. Build it once. Use it forever.

---

## What v2 is in one sentence

**v2 starts with Tier 0 (the data pipeline) — not the features — and stays gated on real signal for Layer B.**

End of corrected plan. The pipeline first. The features second. Signal-gated scaling. Now go run a race. 🏃
