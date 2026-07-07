# v2 Plan — Summary

> Compiled 2026-07-07, after v1 ships. v1 = the page live on GitHub Pages. v2 = what we build next, gated on signal.

---

## Where v1 ended

**Live:** `https://karthik-zoro-96.github.io/runner-portfolio/`

What's shipped:
- Single-file HTML, vanilla JS, no backend
- Real photos (hero race shot + Himalayan trek)
- 8 races across 4 years (Athlinks + STS + personal)
- PB ladder with both race + Strava training PBs
- Career stats (325 runs, 1,128.7 km, 155h 55m)
- Dynamic features: year filter, distance filter, sort, race expand
- Currently training for: first full marathon, sub-3:30
- CSV + build.js data pipeline (15 sec per new race)
- Deployed free on GitHub Pages
- Showcase post drafted, ready to ship

**What's working** → the page renders, the data is real, the photo + trek combo is unique, the share quote holds up.

**What's deferred** → all v2 features below.

---

## v2 has two layers

### Layer A — Page v2 (single-user, more features)
For Karthikeyan's own portfolio. Adds capabilities on top of the v1 page.

### Layer B — Product v2 (multi-tenant)
The community build we specced in `runners-portfolio-product-brief.md`. Triggered by signal, not schedule.

---

## Layer A — Page v2 (single-user, build when you feel like it)

These are the 10 features that earn their place in v2, in priority order:

### Tier 1 — ship in the next 1-2 months
| # | Feature | Why | Effort |
|---|---|---|---|
| 1 | **Year-in-review auto-card** (Dec 1) | Auto-generated "your 2026 in races" shareable image. Highest social value, lowest build cost | 4-6 hrs |
| 2 | **Splits per race** (when known) | When you add a Strava import, splits come free. Big emotional weight for the half + marathon rows | 2-3 hrs |
| 3 | **Goal-race countdown card** | "X days to first full marathon" — the empty Marathon row gets a date when you pick the race | 1-2 hrs |
| 4 | **Race-tag system** | Mark races as `tough`, `negative-split`, `weather-war`, `PR-streak` — adds color to the history | 3-4 hrs |
| 5 | **Race-photo gallery** | Multiple photos per race, not just one. From the camera roll dump | 2-3 hrs |

### Tier 2 — ship when you have 30+ races
| # | Feature | Why | Effort |
|---|---|---|---|
| 6 | **Strava OAuth** (auto-pull all-time bests, runs) | Replaces manual training PB entry. Saves you 5 min/month of editing | 1-2 days |
| 7 | **Search + advanced filters** | By name, time range, placement % | 4-6 hrs |
| 8 | **"Compare with me" widget** | Visitors enter their 10K time → see how they stack up vs your PBs | 1 day |
| 9 | **Personalized "currently training" sub-3:30 countdown** | Same as #3 but for any goal, with weekly target pace | 4-6 hrs |
| 10 | **Print-friendly single-page resume** | Already specced in v1 but not implemented. For the sponsorship pitch PDF | 1-2 hrs |

### What v2 page deliberately skips
- ❌ Comments / reactions
- ❌ Live data feed
- ❌ Anything that requires a backend
- ❌ Account system
- ❌ Multi-page (separate URL per race)

---

## Layer B — Product v2 (multi-tenant, gated on signal)

This is the community build. Don't start until the showcase post produces a real demand signal.

### Trigger conditions (any of these unlocks Layer B)

| Signal | When it counts |
|---|---|
| ≥3 distinct runners DM you "how do I get one?" | within 14 days of showcase post |
| A running club admin reaches out for a club-wide page | any time |
| A brand DMs about sponsorship or partnership | any time |
| ≥100 unique visitors to your GitHub Pages URL | within 30 days |

If 0 signals after 30 days: keep iterating on the post + the page, don't start the multi-tenant build.

### The Phase 1 (hand-curated batch) — quick to do, validates the community

**Before** building the SaaS, manually build 5-10 portfolios for runners who ask. Use the same `me.json` + `races.csv` + `index.html` template. Each takes ~30 min of your time.

**Why this matters:**
- You learn what data the 4 cohorts need (and where Athlinks / STS fails for them)
- You get real user stories for Phase 2
- You build the user base before building the product
- You validate the willingness to pay / share / be featured

### Phase 2 (the SaaS build) — only after 10+ are sharing

**Tech stack (recommended):**
- **Frontend**: Next.js (app router, React Server Components for SEO) — gives you the React ergonomics you wanted, with SSR for the public profile pages
- **Database**: PostgreSQL (via Supabase or Neon, free tier)
- **Auth**: Auth.js with magic link, single OAuth provider (Strava to start)
- **Hosting**: Vercel (free tier for hobby, scales cleanly)
- **Storage**: Vercel Blob for race photos
- **Data imports**: Athlinks CSV paste (v1), STS profile paste (v1.5), Strava OAuth (v2), UltraSignup + ITRA + Parkrun (v3)
- **Cost**: $0-25/mo for the first 100 users

**Build time**: 2-3 weeks of focused work, with the v1 + v1.1 + Phase 1 work as the foundation.

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

## The actual decision tree (so we know when to do what)

```
Week 1-4 post-launch:
  └─ Ship showcase post
  └─ Track DMs
  └─ Run races, add to CSV
  └─ Iterate page if needed (rare)

If 0 signals at week 4:
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
  └─ Note: brands pay $1-10k/year for an athlete's content deal at this tier
```

---

## v2 timeline (rough)

| Phase | When | What | Effort |
|---|---|---|---|
| **Page v2 Tier 1** | next 1-2 months | year-in-review, splits, countdown, tags, photos | 1-2 days total |
| **Page v2 Tier 2** | when you have 30+ races | Strava OAuth, advanced filters, compare widget | 1 week |
| **Phase 1 (hand batch)** | when 3+ DMs | 10 manual portfolios | 10 hours |
| **Phase 2 (SaaS)** | when 10+ sharing | Next.js + Postgres + Auth | 2-3 weeks |
| **v3 (B2B)** | when clubs/teams ask | club mode, coach mode, paid tiers | 1+ month |

---

## The 1 thing I want you to do right now

**Don't build anything yet.** v1 is live. v1 is good. The page is doing its job.

**The single most important thing you can do in the next 7 days:**

```
1. Run a race
2. Snap the photo
3. Add 1 row to races.csv
4. Run node build.js
5. git push
6. Watch the page update
```

Live, repeated, that's the muscle. The data pipeline is the only thing that needs to actually work in production. If adding a new race takes 30 seconds, you'll do it every time. If it takes 5 minutes, you won't.

**v1 is shipped. v2 is gated on signal. The discipline is to not over-build until the world tells you what to build.** That discipline is what makes the difference between an indie product that grows and one that dies of feature creep.

---

## What this is in one sentence

**v2 is a function of v1's signal: ship v1, run races, add to CSV, watch the DMs come in, build only when the world tells you what to build.**

End of plan. Now go run a race. 🏃
