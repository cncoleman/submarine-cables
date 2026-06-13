# The Shape of the Undersea Internet

A static, single-page network analysis of the world's submarine fiber-optic cables.
Built from TeleGeography's v3 GeoJSON API (community mirror), analyzed with Python + NetworkX,
visualized with Chart.js (bundled locally — no runtime CDN dependency).

## What it shows
- **Two ways to be important** — country connectivity (raw connections) vs. betweenness centrality (transit role)
- **The busiest corridors** — country pairs by number of distinct cables
- **Built in waves** — cable deployments by ready-for-service year
- **The chokepoint map** — interactive world choropleth shaded by betweenness; hover for per-country scores
- **What breaks first** — node-removal resilience simulation; drag the slider to remove countries (targeted by betweenness vs. random) and watch the largest connected cluster collapse

## Deploy to GitHub Pages
1. Create a new repository on GitHub.
2. Add these files to the repo root (keep the `vendor/` folder and `.nojekyll`):
   ```
   git init
   git add .
   git commit -m "Submarine cable network analysis"
   git branch -M main
   git remote add origin https://github.com/<you>/<repo>.git
   git push -u origin main
   ```
3. On GitHub: **Settings → Pages → Source: Deploy from a branch → main / (root) → Save.**
4. Your site goes live at `https://<you>.github.io/<repo>/` within a minute or two.

No build step. It's plain HTML/CSS/JS.

## Files
- `index.html` — the entire page (styles inline)
- `vendor/chart.umd.js` — Chart.js 4.4.1 (bar/line charts)
- `vendor/d3.min.js` + `vendor/topojson-client.min.js` — D3 7 and topojson for the map
- `vendor/countries-110m.json` — world topology (Natural Earth, via world-atlas)
- `.nojekyll` — tells Pages to serve files as-is (needed so `vendor/` is published)

Everything is bundled locally — the page has no runtime CDN dependency and works offline,
**as long as it is served over http(s)**. Opening `index.html` directly via `file://` will load
everything except the map (browsers block `fetch()` of the topology JSON on the file protocol).
Use Pages, or run `python3 -m http.server` in the folder to preview locally.

## Caveats
Figures come from a community mirror of the API (TeleGeography discontinued its public repo),
so totals lag the live map. Landing-point edges use an all-pairs-within-a-cable model;
country-level results and betweenness rankings are robust to that choice.
