# AI handoff — dix-homeschool

## What this is
Static family homeschool website ("dix.homeschool"), a single page at `public/index.html`.

## Hosting
- Cloudflare Worker (static assets only): `dix-homeschool`
  (account `a03ababb0169cb601b61cecbadd5c3eb`, config `wrangler.jsonc`, assets dir `./public`).
- Custom domains: `dixhome.com`, `www.dixhome.com`.
- Preview URLs are enabled: `https://<alias>-dix-homeschool.randy-a03.workers.dev`.
- Only `public/` is served; repo docs (this file, AGENTS.md) are not published.

## How to deploy
Pushing to GitHub `main` automatically builds and deploys to production (Cloudflare Workers Builds).
Only push to `main` after Randy approves a preview.

From the repo root on the Mac mini (`/Users/test/dix-homeschool-cloudflare`), with the Cloudflare
API token loaded from the environment (never printed):

- Preview (no production traffic):
  `npx --yes wrangler@latest versions upload --preview-alias <alias> 2>&1 | grep -v -i token`
- Production (after Randy approves the preview):
  `npx --yes wrangler@latest versions deploy <version-id>@100% --yes 2>&1 | grep -v -i token`
- Rollback: `npx --yes wrangler@latest versions deploy <previous-version-id>@100% --yes`

Note: Homebrew Node on the Mac may be broken; a working Node is at
`/Users/test/sites/stinkydragon/.node/bin` (prepend it to PATH).

## Live production
- Live: version `c61a1b62-6921-49fe-807c-953528efdae2` (deployment `962ba860-a5c1-48c5-bd77-b5703399eebc`,
  2026-10-31 04:21 UTC). This is the rollback target for the optimize work below.
  (Version 5b4ff2e5 was deployed two minutes earlier and replaced; it is not live.)

## Current state (2026-10-31)
- Header nav: Python path, All tracks, NewEarth, Our approach, About us.
- The "Open Code Lab ↗" button (linking to https://code.dixhome.com) was removed on 2026-10-31
  because Randy now uses Visual Studio Code locally. `code.dixhome.com` itself and DNS were not touched.

## Pending: optimize + NewEarth (preview only, NOT in production)
Local branch `optimize-2026-10-31` (not pushed). Preview alias `optimize`:
https://optimize-dix-homeschool.randy-a03.workers.dev (latest version: see AI_CHANGELOG; first upload was `37e90666`).
- `public/index.html`: AA contrast fixes (brand span, step labels, track labels, muted text), skip link
  to `<main id="main-content">`, canonical/OG/Twitter tags, JSON-LD (no address/phone/names),
  inline `style=` replaced by classes (`.code`, `.heading-gap`, `.note-gap`), ↗ removed from same-page
  and mailto links, new `#newearth` section linking https://newearth.graphitenerd.com/ and `/stats`.
- New files in `public/`: `og-image.png` (1200x630, no people), `robots.txt`, `sitemap.xml`,
  `_headers` (security headers; `X-Robots-Tag: noindex` on workers.dev preview URLs only).
- `public/404.html`: Gorillas game 404 page (built by another bot; self-contained, no outside requests,
  only links to `/`). Served for any unknown path with HTTP 404 via `"not_found_handling": "404-page"`
  in `wrangler.jsonc`. Only change made to it: a `<link rel="icon" href="/favicon.svg">`.
- `public/favicon.svg` (green rounded square, cream "D", lime leaf) and `public/favicon.ico` (16/32/48).
- Faint labels darkened for AA: `.eyebrow` #708358→#5c6c48, `.paper small` #84975f→#66754a.
- Intentionally not added: `_redirects`, dark mode.
- Cloudflare managed robots.txt (content signals) is enabled for the zone and may prepend to ours.
- To ship after Randy approves: merge to main and push (auto-deploys), or
  `versions deploy <latest optimize version>@100% --yes`. Rollback: `c61a1b62...@100%`.
