# AGENTS.md — dix-homeschool

Rules for any AI agent working in this repo:

- Read `docs/AI_HANDOFF.md` before making changes; update it when the state of the site changes.
- Append an entry to `docs/AI_CHANGELOG.md` for every change you make (date, what, why, commit, deploy).
- Show Randy a preview and get his OK before deploying to production or changing DNS.
- Never commit, print or log Cloudflare tokens or other secrets.
- Keep changes minimal: change only what was asked; do not restyle the page.
