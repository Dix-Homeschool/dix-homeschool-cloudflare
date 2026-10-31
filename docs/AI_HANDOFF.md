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

## Current state (2026-10-31)
- Header nav: Python path, All tracks, Our approach, About us.
- The "Open Code Lab ↗" button (linking to https://code.dixhome.com) was removed on 2026-10-31
  because Randy now uses Visual Studio Code locally. `code.dixhome.com` itself and DNS were not touched.
