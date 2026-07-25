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

## The ecosystem (linked from the site)

**Owned ventures** — VeriSess · Asfanut · CellMall · ZedGlow · ORIZIS Capital · AdLipa · Royal777 ·
Tourizis · OrizJoReal · Diamond Ball · Yomani.
**Client studio** — Oren Farag · Top HaNegev · Hiburatik · Wind Flow.
All venture URLs are verified live and open in a new tab.

## Deploy

```bash
git push origin main
```

GitHub Pages (branch `main`, root) rebuilds automatically and serves the site at
[www.orizisgroup.com](https://www.orizisgroup.com).

## Editing content

The venture and client lists are data-driven — edit the `ventures` and `studio` arrays near the
bottom of `index.html` (name, sector, region, colors, URL, description). Cards render from that data,
so adding a product is a one-line change.
