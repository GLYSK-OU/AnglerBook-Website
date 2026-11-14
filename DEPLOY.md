# AnglerBook.fun — deploy notes

Static site. No build step — these files serve as-is.

## Files
- index.html ............ landing page (brand crest, features, Pro, footer)
- terms.html ............ Terms of Use (GLYSK OÜ, Estonia governing law)
- privacy.html .......... Privacy Policy (App Store privacy-policy URL)
- catalog/index.html .... browsable gear catalog (fetches the canonical gear.json live, ./gear.json fallback)
- 404.html .............. branded not-found page
- og-image.png .......... social share card (1200×630)
- favicon.svg
- assets/ ............... style.css + self-hosted fonts (no third-party CDN)
- SETUP-statichost-infomaniak.md ... full GitHub → StaticHost → Infomaniak guide

## Catalog data source
/catalog is a browsable page that fetches the gear catalog live at runtime. The
catalog is maintained ONLY in GLYSK-OU/AnglerBook-catalog and published via
GitHub Pages. The page reads the canonical file directly:
  https://glysk-ou.github.io/AnglerBook-catalog/catalog/gear.json  (CORS: *)
`catalog/gear.json` in this repo is a same-origin FALLBACK only — served if the
canonical fetch fails, so /catalog can never go blank. To change the source, edit
the `SOURCE` constant in catalog/index.html.

Note: `catalog.anglerbook.fun` is NOT provisioned in DNS yet. Once that subdomain
+ custom domain are live on the AnglerBook-catalog Pages site, point `SOURCE` at
`https://catalog.anglerbook.fun/catalog/gear.json`.

## Heads-up: anglerbook.fun is served by GitHub Pages
The apex resolves to GitHub Pages (185.199.108–111.153) off this repo's `main`
branch, so **merging to `main` deploys immediately** — there is no manual step
gating a release. (The statichost/Infomaniak notes below are legacy.)

## Deploy to statichost (direct upload)
The statichost site must already exist in the dashboard, and anglerbook.fun must be
attached to it as a custom domain (Add site / Domains — there is no API for that yet).

    export STATICHOST_APIKEY='your_token'
    shcli <your-site-name> .       # run from inside this folder; uploads contents

## DNS for anglerbook.fun
Point the domain at the IP statichost shows for the site in the dashboard
(A record on the apex, or the CNAME they give you). First hit after attaching the
domain takes ~1–2s while the TLS cert provisions — expected.

## Before launch
- Contact address is support.anglerbook@glysk.eu (set in terms.html and privacy.html).
- Privacy Policy is live at /privacy.html — use this as the App Store privacy URL.
- Confirm the privacy.html assumptions still hold: no third-party advertising/analytics
  SDKs, GLYSK runs no content servers, weather source is Apple WeatherKit.
