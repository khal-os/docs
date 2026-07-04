# Brainstorm DRAFT: khal docs v1 — from-zero reset

Slug: khal-docs-v1 · Date: 2026-07-04 · Repo: khal-os/docs (GitHub, Mintlify)

## Founder intent (Cezar, verbatim)
Rebuild the docs FROM ZERO. The ONLY survivors: (1) PR #39 tightened Access pages, (2) my
khal-cli reference. They ARE correlated → ONE new PR that consolidates them into v1.
Ethos: "NO FUCKING BLOAT, built one page at a time, for humans, reviewed by one."

## Grounding
- Bloat: 60 .mdx, 9 tabs (START HERE, FDE Engineer, KHAW, KHORTEX, Pack Dev, Visual Proof,
  Reference, Brain, COMECE AQUI).
- Survivor 1: fde-start-here/access/{overview,engineer-guide,admin-guide,troubleshooting} (266 EN + pt-BR).
- Survivor 2: fde-engineer/khal-cli.mdx (109) — command set + agent paste-blocks.
- Correlation: Access engineer-guide = the guided onboarding narrative; khal-cli = the reference. Same audience.

## Open decisions
- [ ] (1) BLAST RADIUS + mechanism: hard-delete 58 pages vs nav-reset+archive vs nav-only. "alive" = site-nav vs repo?
- [ ] (2) v1 STRUCTURE: how few pages (2-3?); how the 4 Access + 1 CLI consolidate/de-dup
- [ ] (3) ETHOS as a rule: one-page-at-a-time future PRs; a docs CONTRIBUTING contract (page budget, for-humans, keep agent-blocks, no repeated facts)
- [ ] (4) pt-BR + non-FDE tabs (KHAW/KHORTEX/Brain/PackDev) OUT of v1? EN-first?

## WRS
Problem ✅ | Scope ✅ | Decisions ✅ | Risks ✅ | Criteria ✅ = 100/100 → crystallized
