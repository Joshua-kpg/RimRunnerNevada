# RimRunnerNevada

Unofficial Nevada HS boys basketball ratings. Not affiliated with the NIAA or MaxPreps.
Status: **HELD - waiting for hosting.** Planned home: https://rimrunnernevada.web.app/ (Firebase Hosting). The Firebase project could not be
created yet (Google project quota on the hosting account) and Cloudflare Pages is at its project limit. Working preview: this repo's GitHub Pages copy
(https://joshua-kpg.github.io/RimRunnerNevada/). Until the site's Firebase project exists, accounts, picks and follows save only in the visitor's browser.

## Files
- `index.html` — the whole app (data embedded). `logos/` — small WebP school logos (from MaxPreps school profiles), served with the site.
- `data/results.json` — 2026-27 finals ledger written by the nightly updater.
- `firebase/` — Firestore rules and auth config for the site's Firebase project (accounts, picks, follows).

## Teams
121 teams: the NIAA 2026-27 boys basketball alignment (5A 9, 4A 44, 3A 20, 2A 16, 1A 29, in 15 NIAA leagues/regions), including the NIAA members
just across the state line (Truckee, South Tahoe, North Tahoe, Coleville and Needles in California; Beaver Dam in Arizona), plus 3 Other Nevada
schools that played 8+ NIAA teams in 2025-26.

## Nightly scores

`.github/workflows/nightly-scores.yml` runs `scripts/update_scores.py` at 07:30 and 15:00 UTC
(11:30 pm / 7:00 am Pacific standard time) and on manual dispatch. Before the Nevada boys season opens
(`SEASON_START` in index.html = the first game on the MaxPreps schedules, on or after the NIAA first possible winter contest day, Thu Nov 26, 2026)
the script makes no requests. From then on it reads boys basketball finals for the last 3 days from the MaxPreps Nevada scores-by-date page
(team-page fallback; games between two out-of-state NIAA members are read from the home team's page), matches them to the schedule by MaxPreps
team id + date, records them in `data/results.json` and bakes scores and W-L records into `index.html`. The page recomputes Elo from those finals.
It commits only when data changed, then deploys (index.html + logos/) to Cloudflare Pages if the `CLOUDFLARE_API_TOKEN` repository secret is set.
