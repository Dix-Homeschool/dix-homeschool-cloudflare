# AI changelog — dix-homeschool

## 2026-10-31 — Remove "Open Code Lab" button
- What: removed `<a href="https://code.dixhome.com" class="pill">Open Code Lab ↗</a>` from the
  header nav in `public/index.html`. Nothing else on the page changed.
- Why: Randy is using Visual Studio Code locally instead of the hosted coding space.
- Commits: ae10baa (button removal) and 61db331 (docs), on top of bd58283 "Link to family code lab".
- Preview: version 132bb183-7d55-4665-911b-d44fa40499c0
  (https://no-code-lab-dix-homeschool.randy-a03.workers.dev), approved by Randy.
- Production: pushing to GitHub main triggers an automatic Cloudflare build+deploy. The push of
  61db331 auto-deployed version 5b4ff2e5-28a0-40f0-98a3-b1c59bd9139b at 100% (identical index.html to
  the approved preview). Previous version for rollback: 52081844-4d37-4a38-8825-b9b60acd0400.
  CORRECTION (2026-10-31, later): 5b4ff2e5 (deployment 8e7560d0, 04:19 UTC) was replaced two minutes later
  by version c61a1b62-6921-49fe-807c-953528efdae2 (deployment 962ba860, 04:21 UTC), which is what is live.
- Also added AGENTS.md, docs/AI_HANDOFF.md and this changelog.

## 2026-10-31 — Optimization tweaks + NewEarth tie-in (PREVIEW ONLY)
- Why: Randy asked for "all optimization tweaks" and a NewEarth tie-in.
- What (`public/index.html`):
  - Contrast to WCAG AA 4.5:1: `--muted` #66746b→#5c6961; `.brand span` #829a57→#627442 (footer
    override #97ad6e on dark green); `.step b` #899b73→#5d6b4c; `.track .top` #8a9c76→#697958;
    `.step p,.about p` → `var(--muted)`.
  - Skip link ("Skip to content") → `<main id="main-content" tabindex="-1">`.
  - Head: canonical, Open Graph, twitter:card summary_large_image, JSON-LD (WebSite +
    EducationalOrganization; no address, phone or children's names).
  - Replaced 14 inline `style=` attributes with classes `.code`, `.heading-gap`, `.note-gap`.
  - Removed ↗ from the internal "Explore the Python path" link and the mailto link; decorative ↗
    spans marked `aria-hidden`. New-tab external links use `rel="noopener"`.
  - New NewEarth section (`#newearth`, also in nav) linking https://newearth.graphitenerd.com/ and
    https://newearth.graphitenerd.com/stats. No IPs, server addresses or player names.
- New files: `public/og-image.png`, `public/robots.txt`, `public/sitemap.xml`, `public/_headers`.
- Not done: dark mode (skipped), 404.html (another bot owns it), `_redirects` (not needed).
- Commit: on local branch `optimize-2026-10-31` (NOT pushed; pushing main auto-deploys production).
- Preview: version 37e90666-927c-4fb5-a224-57d1572d3dcb,
  https://optimize-dix-homeschool.randy-a03.workers.dev. Verified security headers and noindex.
- Production: NOT deployed. Live remains c61a1b62-6921-49fe-807c-953528efdae2 (rollback target).
- Backups of changed files: `backups/*.pre-optimize-2026-10-31` (untracked, not served).

## 2026-10-31 — Gorillas 404, favicon, remaining label contrast (PREVIEW ONLY)
- Why: Randy asked to add the Gorillas 404 page and finish the optimize pass.
- What:
  - `public/404.html`: Gorillas 404 built by another bot (reviewed: inline CSS/JS only, no outside
    requests, links only to `/`, localStorage key `dixhome-404-gorillas` for game prefs). Added only a
    favicon `<link>`.
  - `wrangler.jsonc`: `"not_found_handling": "404-page"` under `assets` (unknown paths → 404.html, status 404).
  - `public/favicon.svg`, `public/favicon.ico`, and `<link rel="icon">` tags in `public/index.html`.
  - Contrast: `.eyebrow` #708358→#5c6c48 (3.72→5.11 on #f5f3ea, 3.35→4.61 on #e5e9db);
    `.paper small` #84975f→#66754a (2.97→4.64 on #f8f7ef).
- Commit: local branch `optimize-2026-10-31` (NOT pushed).
- Commit db263f4. Preview: version ebc3df74-2983-43bf-9a57-2dc0a70de8ce, alias `optimize`
  (https://optimize-dix-homeschool.randy-a03.workers.dev). Verified: / 200; /does-not-exist and /a/b/c
  return 404 with the Gorillas page; robots, sitemap, og-image, favicon.svg/.ico 200; headers applied.
- Production: NOT deployed. Live remains c61a1b62-6921-49fe-807c-953528efdae2 (rollback target).
  Note: deploying this also changes `not_found_handling`, which ships with the version.
