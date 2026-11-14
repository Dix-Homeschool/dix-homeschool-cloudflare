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

## Site refresh — PREVIEW ONLY (2026-11-14, ~3:10 AM ET)

- Why: Randy (2:59 AM ET) asked to drop "family" project wording, rewrite the curriculum to match the real STEM work, make the site look like a complete school program, and state the school's legal status. He later added NewEarth Wildlife and the autonomous AI player research (3:02 AM), then replaced the legal wording (3:07 AM) and asked that each ORC citation be linked (3:08 AM).
- Branch / worktree (not pushed): `feature/site-refresh`, branched from `feature/stinkdoku-beta-mention` in `/Users/test/sites/dixhome-stinkdoku-mention`.
- Files: `public/index.html` rewritten (pretty-printed). `public/_headers` and `public/404.html` were not changed. There is still no Content-Security-Policy on this site; outbound links are ordinary `<a href>` anchors and do not need a CSP change.
- Legal / school-status wording is kept easy to change. Canonical instructions are in an HTML comment at the top of `index.html`.
  - Official text (exact, hero status band and About), class `school-status-text`:
    "Dix Homeschool is an Ohio home education program operating under Ohio Revised Code 3321.042. We currently teach grades 7-12, and issue high school diplomas to graduates under ORC 3313.6110. This site covers the STEM portion of our curriculum."
  - Short footer, class `school-status-footer`:
    "Ohio home education program under ORC 3321.042. Grades 7-12; high school diplomas issued under ORC 3313.6110. This site covers the STEM portion of our curriculum."
  - Links (new tab, `rel="noopener"`): "Ohio Revised Code 3321.042" and footer "ORC 3321.042" → https://codes.ohio.gov/ohio-revised-code/section-3321.042 ; "ORC 3313.6110" → https://codes.ohio.gov/ohio-revised-code/section-3313.6110. Meta description, og:description and JSON-LD keep the official sentences as plain text (no HTML).
  - Do not use "accredited", "K-12", or "in accordance with Ohio law" / "because of Ohio law" in visible copy.
- Public-facing "family" wording was removed (CSS `font-family` only). NewEarth is described as a private, approved-player STEM world, not a family project.
- Curriculum is organized as a grades 7-12 STEM program with strands (no invented course names, credits, teachers or stats): NewEarth building and history; game and app development (Scratch, StinkDoku); web development and Cloudflare hosting; GitHub workflow; Raspberry Pi and home networking; AI tools and the AI-player research; Python (existing 12-step path kept); Minecraft plugin development (NewEarth Wildlife).
- Current projects (three cards): StinkDoku (beta, tester and web-play buttons; "will be released as open source when ready," no repo link); NewEarth Wildlife (in development, will be made public once working, no download link); Bob, an autonomous AI player (research in progress, not a finished product).
- Other polish: CSS-only mobile Menu, tighter nav, school-status band, green info strip, About and footer restated in the new legal frame. The 12 Python steps remain, with "playtest with family" changed to "playtest and revise it."
- Preview (no production traffic), updated 3:12 AM ET after Randy's copy answers:
  - Alias: https://site-refresh-dix-homeschool.randy-a03.workers.dev
  - Version ID: `50cb47b3-867e-4404-8f98-d71d14890a69`
  - Also: https://50cb47b3-dix-homeschool.randy-a03.workers.dev
- Earlier preview versions on the same alias (`45a28aed-...`, `9974345f-...`, `04bdca50-...`) are superseded by `50cb47b3-...`.
- Production: **LIVE as of 2026-11-14 ~3:17 AM ET**, after Randy said "Looks good." Version `50cb47b3-867e-4404-8f98-d71d14890a69` is at 100% on dixhome.com and www.
- Rollback target (previous live version): `cc6f4a2d-ba9d-4f1a-97ca-4df220d6e8ad`
  `npx --yes wrangler@latest versions deploy cc6f4a2d-ba9d-4f1a-97ca-4df220d6e8ad@100% --yes 2>&1 | grep -v -i token`
  Then revert or reset the repo so main matches, with Randy's OK.
- Checked: desktop 1280×800 and mobile 390×844 in headless Chrome; no console errors; StinkDoku, NewEarth, stats, and both ORC links return 200; Gorillas 404 still works; no CSP header (same as before). Screenshots refreshed in `/workspace/dixhome-refresh-preview/` on the Websites box.
- Shipped: `versions deploy 50cb47b3-867e-4404-8f98-d71d14890a69@100% --yes` from this worktree after Randy's OK. Verified dixhome.com and www (200, new copy, ORC links 200, Gorillas 404, security headers present, no X-Robots-Tag). Next: push `feature/site-refresh` and open a PR to main (no merge in this step). Do not assume Workers Builds will auto-deploy; this ship was a wrangler versions deploy. The Mac's GITHUB_TOKEN cannot write to the Dix-Homeschool org, so the push/PR may need the GitHub bot or Randy's account.

### Answered by Randy, 3:12 AM ET, 14 Nov 2026
- NewEarth history work is formal history study: history lessons taught through building in the world. (Copy updated on this preview.)
- Name the AI player Bob on the site. (Copy updated; still described as research in progress.)
- StinkDoku: say it will be released as open source when ready, with no repo link. (Copy updated.)
- "Get in touch" stays a general contact. Do not mention enrollment. (Unchanged; no enrollment language added.)

### Still open
- Raspberry Pi and home networking are included as a strand from Randy's list and existing home systems, but there is no written syllabus in the repo. Any detail to add or trim?
