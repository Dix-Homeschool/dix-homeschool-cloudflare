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
- Also added AGENTS.md, docs/AI_HANDOFF.md and this changelog.
