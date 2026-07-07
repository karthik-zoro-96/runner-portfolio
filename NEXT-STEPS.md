# Next Steps — the dumb simple version

> You are NOT building v2 anytime soon. That's fine. This file is what to do
> instead, in order. Read top to bottom, do the first unchecked thing, stop.

---

## Right now (no coding at all)

- [ ] **Post the showcase.** The page is live at
      https://karthik-zoro-96.github.io/runner-portfolio/ — share it
      (Strava club, Instagram, Twitter, running WhatsApp groups).
- [ ] **Watch for DMs.** Keep a note (literally a note on your phone) of every
      person who asks "how do I get one of these?"
- [ ] **Keep racing.** After every race, add it to the page (see below).

## After every race (~5 minutes, manual for now)

1. Open `index.html`, find `const DATA = {` (around line 533).
2. Copy an existing race entry in the `"races"` list, paste it, edit the
   date / name / distance / time.
3. Save. Open the page locally and check it renders (a missing comma = blank page).
4. `git add index.html && git commit -m "Add <race name>" && git push`
5. Done — GitHub Pages redeploys automatically in ~1 minute.

## The ONE small thing worth building whenever you feel like it (1–2 hrs)

**Goal-race countdown.** When you pick your marathon and it has a date:
- Add `"target_date": "YYYY-MM-DD"` to `currently_training_for` in the DATA JSON.
- Add a small "X days to go" line in the training-for card (one date-diff in JS).
- No other feature depends on anything. This one has zero prerequisites.

## The FIRST real coding task if v2 ever starts (do this before any feature)

**Tier 0 — the data pipeline (~6–7 hrs).** Makes adding a race take 20 seconds
instead of 5 minutes:
1. Make `races.csv` with the 8 current races (one row per race).
2. Write `build.js` (Node) that reads the CSV and writes `data.js`.
3. Swap the inline JSON in `index.html` for `<script src="data.js">`.
4. Add a `node build.js` step to `.github/workflows/static.yml` **before** the
   "Upload artifact" step (this site deploys with GitHub Actions — there is no
   Vercel here, ignore anything that says otherwise).
5. Check the built page matches the current one, then push.

## When to actually build v2 (don't jump the gun)

Build more only if one of these REAL signals happens:
- **3+ different runners DM you** asking for their own page → hand-build
  portfolios for them with your template (~30 min each). Ten of those before
  any SaaS.
- **A running club admin** asks for a club page → take it seriously.
- **A brand** DMs about sponsorship → take it very seriously.

Page traffic alone is NOT a signal. Ignore visitor counts.

If none of that happens in a month: rewrite the post, try another channel,
keep racing. Do not build the multi-tenant product on zero signal.

## Full detail

The complete roadmap (tiers, dependencies, tech stack for the eventual SaaS)
lives in [v2-plan-summary.md](v2-plan-summary.md).

---

**One sentence:** post the page, log your races, note every DM — and build
nothing until at least three strangers ask for one.
