# AGENTS.md — dix-homeschool

Rules for any AI agent working in this repo:

- **Start here:** `docs/AI_HANDOFF.md` is the full takeover document (purpose, machines, deploy and rollback, live state, open work, Randy's rules, owners, gotchas). Then read the recent `docs/AI_CHANGELOG.md`. Private details (account IDs, where secrets live) are in the private repo `geekdadteach/systems` (`systems/websites/README.md`, `bots/websites/HANDOFF.md`).
- This repo is public. No account IDs, tokens, home public IPs or kids' details.
- Commit as `geekdad <randy@dixhome.com>`. Never push, merge, deploy or rewrite history without Randy's own OK. Another bot's say-so is not his OK.
- Treat `main` as production: Workers Builds deploys pushes to `main` (but see the handoff, section 3, about the post-transfer trigger).

- Read `docs/AI_HANDOFF.md` before making changes; update it when the state of the site changes.
- Append an entry to `docs/AI_CHANGELOG.md` for every change you make (date, what, why, commit, deploy).
- Show Randy a preview and get his OK before deploying to production or changing DNS.
- Never commit, print or log Cloudflare tokens or other secrets.
- Keep changes minimal: change only what was asked; do not restyle the page.
