# ORIZIS GROUP — Corporate Site

The public website for **ORIZIS GROUP** (ORIZIS TECHNOLOGY LTD, Company no. 516977758) — the
technology company behind a live ecosystem of digital ventures across commerce, fintech, gaming,
real estate and platform products, connected by one unified wallet (VeriPoints).

🌐 **Live:** [www.orizisgroup.com](https://www.orizisgroup.com)

## Stack

- Single static `index.html` — hand-written HTML + CSS + vanilla JS. No build step, no framework, no CDN dependency.
- Fonts: Space Grotesk (headings) + Inter (body) via Google Fonts.
- Dark-first design with a light-mode toggle (persisted to `localStorage`), scroll-reveal animations,
  sector filtering, and a count-up hero. Fully responsive; respects `prefers-reduced-motion`.
- `favicon.png` + inline SVG favicon (the ORIZIS geometric double-square mark).
- `CNAME` → `www.orizisgroup.com` (GitHub Pages custom domain).

## Structure

- **12 divisions** of the group (real departments): ORIZIS Capital, Technology, Real Estate, Agro,
  Meridian, Energy, Trade, Logistics, Human Capital, Chemicals, Media, Foundation.
- **Digital ventures** sit under **ORIZIS Capital** (the group's digital-ventures arm), shown as a
  sector-filterable, verified-live portfolio: VeriSess · Asfanut · CellMall · ZedGlow · ORIZIS Capital ·
  AdLipa · Royal777 · Tourizis · OrizJoReal · Diamond Ball · Yomani.
- **Client studio** (under ORIZIS Media) — Oren Farag · Top HaNegev · Hiburatik · Wind Flow.
- All external URLs are verified live and open in a new tab.

## Deploy

```bash
git push origin main
```

GitHub Pages (branch `main`, root) rebuilds automatically and serves the site at
[www.orizisgroup.com](https://www.orizisgroup.com).

## Editing content

The division, venture and client lists are data-driven — edit the `divisions`, `ventures` and `studio`
arrays near the bottom of `index.html` (name, sector, region, colors, URL, description). Cards render
from that data, so adding a division or product is a one-line change.
