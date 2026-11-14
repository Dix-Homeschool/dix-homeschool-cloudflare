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
- Live: version `5661ac63-af39-4ff2-8517-5cc241c6783f` (deployment `307fbcdd-d458-4706-9a98-a530d715c804`,
  2026-10-31 10:08 PM ET), auto-built by Workers Builds from main at f9ceabf.
- Previous / rollback target: version `c61a1b62-6921-49fe-807c-953528efdae2` (deployment `962ba860`).
  Rollback: `npx --yes wrangler@latest versions deploy c61a1b62-6921-49fe-807c-953528efdae2@100% --yes 2>&1 | grep -v -i token`
  (then push a revert so main matches production).

## Current state (2026-10-31)
- Header nav: Python path, All tracks, NewEarth, Current projects, Our approach, About us. (Current projects is on the preview branch only; production nav does not have it yet.)
- The "Open Code Lab ↗" button (linking to https://code.dixhome.com) was removed on 2026-10-31
  because Randy now uses Visual Studio Code locally. `code.dixhome.com` itself and DNS were not touched.

## Optimize + NewEarth + Gorillas 404 (shipped 2026-10-31 ~10:07 PM ET on Randy's OK; rollback c61a1b62)
Local branch `optimize-2026-10-31` (not pushed). Preview alias `optimize`:
https://optimize-dix-homeschool.randy-a03.workers.dev (latest version `ebc3df74-2983-43bf-9a57-2dc0a70de8ce`).
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
  `versions deploy ebc3df74-2983-43bf-9a57-2dc0a70de8ce@100% --yes`. Rollback: `c61a1b62...@100%`.

## StinkDoku beta mention — PREVIEW ONLY (2026-11-14, ~2:47 AM ET)

- Why: Randy asked (Nov 14, 2:42 AM ET) to look at dixhome.com and mention StinkDoku as one of the current projects, currently in beta on iPhone and Android, with a link back to the game.
- Where: a new `#projects` section ("Current projects.") immediately after the existing NewEarth section, plus a "Current projects" item in the header nav. NewEarth is already a live family project in the same style, so this was the most natural place. There was no existing general projects list.
- What: one cream card matching the site's `.track` style. Copy: "A free, ad-free Sudoku game the family is building. It's currently in beta on iPhone and Android." No kids' names, no private details, no BrewDadLabs mention (Dix Homeschool is the voice of this site).
- Worded buttons: "Become a tester ↗" → https://stinkdoku.com/beta/ (live 200 at preview time); "Play on the web ↗" → https://play.stinkdoku.com/. Both open in a new tab with `rel="noopener"`.
- Files: `public/index.html` only. Small CSS added (`.project-links`, `.button-alt`). NewEarth spacing and `public/_headers` (security headers) were not changed.
- Branch / worktree (not pushed): `feature/stinkdoku-beta-mention` at `/Users/test/sites/dixhome-stinkdoku-mention`, based on `origin/main` (e1864c1). Do not use the takeover worktree for this.
- Preview (no production traffic):
  - Alias: https://stinkdoku-beta-dix-homeschool.randy-a03.workers.dev
  - Version ID: `5f4a610c-8791-4aad-848f-269317e14ee4`
  - Also: https://5f4a610c-dix-homeschool.randy-a03.workers.dev
- Production: **unchanged.** dixhome.com still does not mention StinkDoku.
- Checked: desktop 1280×800 and mobile 390×844 in headless Chrome; no console errors, no failed requests, both buttons return 200, nav jump to `#projects` works. Screenshots: `/workspace/dixhome-stinkdoku-preview/` on the Websites box.
- To go live, after Randy himself OKs this preview (another bot's say-so is not enough):
  1. From the worktree, with the token already in the environment (never printed):
     `npx --yes wrangler@latest versions deploy 5f4a610c-8791-4aad-848f-269317e14ee4@100% --yes 2>&1 | grep -v -i token`
  2. Verify https://dixhome.com/ and https://www.dixhome.com/ show the new section; unknown paths still 404 with the Gorillas page; security headers still present; no `X-Robots-Tag` on the real domain.
  3. Then, with his OK, merge this branch into `main` and push so the repo matches production. The Mac's `GITHUB_TOKEN` cannot write to the Dix-Homeschool org; ask the GitHub bot (or Randy) to push. Workers Builds auto-deploy may still be disconnected after the repo transfer — do not assume a push deploys. The wrangler versions deploy in step 1 is what actually ships it.
  4. Rollback if needed: `npx --yes wrangler@latest versions deploy <currently-live-version-id>@100% --yes 2>&1 | grep -v -i token` (then revert the commit so main matches). Check the live version with `npx wrangler deployments list 2>&1 | grep -v -i token` at go-live time; as of 2026-11-13 the live version was `cc6f4a2d-ba9d-4f1a-97ca-4df220d6e8ad` (site files identical to f9ceabf / e1864c1).
