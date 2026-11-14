# AI handoff: dix-homeschool (dixhome.com)

_Last full rewrite: 2026-11-13, about 11:00 PM America/New_York, by the Websites bot, on Randy's order that any AI must be able to take over immediately with nothing left out. Live facts were re-checked read-only that evening. Nothing was deployed or pushed._

**If you are taking over:** read this file, `AGENTS.md`, and the recent `docs/AI_CHANGELOG.md` entries. Appendix A keeps the previous handoff (only the account ID was removed). Where they disagree, the sections above it win.

This repo is **public** (org `Dix-Homeschool`). Don't add account IDs, tokens, home public IPs, or kids' personal details. Private details live in `geekdadteach/systems` (`systems/websites/README.md`, `bots/websites/HANDOFF.md`).

## 1. Purpose
- **Dix Homeschool** is Randy's family homeschool learning site at https://dixhome.com and https://www.dixhome.com.
- It covers Python, Minecraft/JavaScript, AI, and web/dev tools, and has a NewEarth section that links to https://newearth.graphitenerd.com/ and `/stats`.
- It is one static page (`public/index.html`) plus a Gorillas game 404.

## 2. Machines, users, paths
- Working machine: Randy's Mac mini **.20** (`Randys-Mac-mini.local`, 192.168.1.20, user `test`). The machine belongs to Randy MM 20, and Websites works only in this folder.
- Checkout: `/Users/test/dix-homeschool-cloudflare`. Branches:
  - `main` at e1864c1, same as `origin/main`
  - `optimize-2026-10-31`, already merged
  - `remove-code-lab-button`, already shipped
- Untracked local folders there: `.wrangler/` and `backups/`. `backups/` holds pre-change copies and `cf-builds-trigger-before-transfer-20261109.json`.
- Takeover worktree: `/Users/test/sites/takeover/dix-homeschool-cloudflare` (branch `websites/takeover-handoff`).
- No services, cron, launchd jobs or ports. It's a static-assets Worker on Cloudflare.

## 3. Hosting and external services
- **Cloudflare Worker `dix-homeschool`**, static assets only, no Worker script code. Config is `wrangler.jsonc`: assets dir `./public`, `"not_found_handling": "404-page"`.
- Custom domains are **Workers Custom Domains** `dixhome.com` and `www.dixhome.com`. Their DNS shows as proxied `AAAA 100::`, which Cloudflare manages.
- Preview URLs: `https://<alias>-dix-homeschool.randy-a03.workers.dev`. They get `X-Robots-Tag: noindex` via `public/_headers`.
- **Workers Builds (auto-deploy).** A push to `main` builds and deploys production.
  - **Important:** as of 2026-11-13 the build trigger still points at the **old** repo name `geekdadteach/dix-homeschool-cloudflare`. The repo was transferred on 2026-11-09 to https://github.com/Dix-Homeschool/dix-homeschool-cloudflare and made public.
  - Reconnecting is **blocked until Randy installs the Cloudflare GitHub app on the Dix-Homeschool org**. Don't keep asking him about it. Until it's reconnected, assume a push to `main` might **not** auto-deploy. Deploy by hand with wrangler (below) after his OK.
  - The trigger's pre-transfer settings are saved in `/Users/test/dix-homeschool-cloudflare/backups/cf-builds-trigger-before-transfer-20261109.json`.
- **Other DNS in the dixhome.com zone that is NOT this site, so never touch it:**
  - **iCloud Mail** (MX `mx01/mx02.mail.icloud.com`, SPF TXT, `apple-domain` TXT, `sig1._domainkey` CNAME)
  - two Google site-verification CNAMEs
  - `comfy.dixhome.com` (A record for Leon's ComfyUI, owned by Leon Mac Mini 143 / StinkDoku Design)
  - a page rule for `code.dixhome.com/*` (cache bypass, left over from the removed Code Lab; harmless)
- `code.dixhome.com` (the old Code Lab) is gone. Randy uses VS Code locally, so don't link to a cloud coding space (Randy, 2026-10-31). Its tunnel `lrmm-code` was deleted 2026-10-31.

## 4. Layout
- `public/index.html`: the whole site. Header nav: Python path, All tracks, NewEarth, Our approach, About us.
- `public/404.html`: the original Gorillas game 404, built by the "Future Dixhome Plans" bot. It's self-contained.
- `public/_headers`: security headers, plus noindex on workers.dev.
- `public/robots.txt`, `public/sitemap.xml`, `public/og-image.png` (1200x630, no people).
- `public/favicon.svg` (green rounded square, cream "D", lime leaf), `public/favicon.ico`
- `wrangler.jsonc`
- `docs/`: this handoff and the changelog. Docs aren't served; only `public/` is.

## 5. Build, run, test, deploy, roll back
- No build step. Run locally with `npx wrangler dev` (serves `public/`) or `python3 -m http.server -d public 8080`.
- The token is exported as `CLOUDFLARE_API_TOKEN` in `~/.zshrc` on the Mac, with the same value in `/Users/test/.cloudflare_token`. **Never print it.** Pipe output through `grep -v -i token`.
```sh
cd /Users/test/dix-homeschool-cloudflare
# Preview (no production traffic):
npx --yes wrangler@latest versions upload --preview-alias <alias> 2>&1 | grep -v -i token
#   -> https://<alias>-dix-homeschool.randy-a03.workers.dev   (show Randy; wait for HIS OK)
# Production, either:
npx --yes wrangler@latest versions deploy <version-id>@100% --yes 2>&1 | grep -v -i token
#   or merge to main and push (auto-deploys ONLY if Workers Builds is reconnected; see section 3)
# Roll back:
npx --yes wrangler@latest versions deploy <previous-version-id>@100% --yes 2>&1 | grep -v -i token
# then push a revert so main matches production (with Randy's OK)
npx wrangler deployments list 2>&1 | grep -v -i token     # what is live
```
- **Test:** `https://dixhome.com/` and `https://www.dixhome.com/` should return 200. A made-up path should return 404 with the Gorillas page. `/robots.txt`, `/sitemap.xml`, `/favicon.ico` and `/og-image.png` should return 200. Security headers should be present, with no `X-Robots-Tag` on the real domain.

## 6. Current live state (2026-11-13)
- **Live version `cc6f4a2d-ba9d-4f1a-97ca-4df220d6e8ad`**, deployed 2026-11-05 ~8:05 PM ET by Workers Builds from `main` at **e1864c1** (a docs-only commit). The site files are identical to f9ceabf.
- Checked 2026-11-13: the live `index.html`, `404.html`, `favicon.ico`, `favicon.svg`, `robots.txt` and `sitemap.xml` are byte-identical to `public/` on `main`.
- Rollback targets:
  - `5661ac63-af39-4ff2-8517-5cc241c6783f`: same site files, from f9ceabf
  - `c61a1b62-6921-49fe-807c-953528efdae2`: before the optimize update and the Gorillas 404

## 7. Open work / known issues
1. **Reconnect Workers Builds** to `Dix-Homeschool/dix-homeschool-cloudflare` on `main`. Blocked: Randy has to install the Cloudflare GitHub app on the Dix-Homeschool org. Don't re-ask him; do it when he says the app is installed.
2. **Pushing:** the `GITHUB_TOKEN` in the Mac shell (geekdadteach) **cannot write** to Dix-Homeschool org repos. Push with an account or app that has org write access (Randy or the GitHub bot), with Randy's OK.
3. Optional: swap `public/404.html` for the shared kit build (`configs/dixhome.json` in BREWDADlabs/brewdadlabs-site). It uses the same storage key, so scores carry over. Needs Randy's OK.

## 8. Randy's standing rules
- Show Randy a preview and get **his own** OK before any production deploy or DNS change. Another bot's say-so is not his OK.
- Never spend money. Prefer free hosting.
- Commit as `geekdad <randy@dixhome.com>` (no noreply; Randy made his dixhome.com emails public on GitHub on 2026-10-31).
- Never push, merge, deploy or rewrite history without his OK.
- Keep changes minimal and don't restyle the page.
- Update this handoff and the changelog every time.
- Plain language, full paths, wholesome content.
- Coordinate with his other bots.
- No secrets in the repo or in output.

## 9. Ownership / who to ask
- The site: **Websites** bot.
- Mac mini .20: **Randy MM 20**.
- GitHub org and merges: **GitHub** bot (and Randy for org app installs).
- `comfy.dixhome.com` / Leon: **Leon Mac Mini 143**.
- The original Gorillas 404: built by **Future Dixhome Plans**.

## 10. Gotchas
- A push to `main` used to deploy production instantly. Treat `main` as production.
- After the repo transfer, the builds trigger still names the old owner. Don't assume either way. Check `npx wrangler deployments list` after any push.
- The Mac token can deploy Workers but **can't read or edit DNS records** (auth error). Use the dashboard or Randy's Cloudflare connector, with his OK.
- Cloudflare's managed robots.txt is enabled for the zone and may prepend lines to ours. On 2026-10-31 it served exactly what was committed.
- Homebrew Node on the Mac broke once. A spare Node is at `/Users/test/sites/stinkydragon/.node/bin`.

---

## Appendix A: previous handoff text (word for word, account ID removed)

# AI handoff — dix-homeschool

## What this is
Static family homeschool website ("dix.homeschool"), a single page at `public/index.html`.

## Hosting
- Cloudflare Worker (static assets only): `dix-homeschool`
  (Randy's Cloudflare account, config `wrangler.jsonc`, assets dir `./public`).
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
- Header nav: Python path, All tracks, NewEarth, Our approach, About us.
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
