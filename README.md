# Runner Portfolio

A single-file running portfolio for Karthikeyan Shanmugam — career stats, personal bests, race history, and training progress.

## Highlights

- **No backend, no build step.** Everything lives in [index.html](index.html): markup, styles, JS, and data.
- **Data baked in from Strava.** Career totals, year-to-date stats, and the weekly-mileage strip are computed from the full Strava activity history and embedded as a static JSON blob — refresh by re-pulling and re-aggregating, then re-editing the `DATA` object.
- **Interactive race history** — filter by year and distance, sort by recency/fastest/oldest, tap a row to expand details.
- **Personal-best ladder**, training focus quote, and a print stylesheet for a clean PDF export.

## Running locally

```bash
cd runner-portfolio
python3 -m http.server 4300
```

Then open `http://localhost:4300`.

## Updating with fresh Strava data

1. Pull activity history (paginated) from Strava.
2. Aggregate: career totals, year-to-date stats, last 8 weeks of running km.
3. Update the `DATA` JSON block in `index.html` (`career_stats`, `weekly_km`, `all_time_bests` if any PBs improved).
4. Reload and spot-check the numbers against the source data.

## Deployment

Static file — deploy via GitHub Pages, Netlify, Vercel, or any static host. No environment variables or server-side code required.
