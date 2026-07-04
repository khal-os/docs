# Design: khal docs v1 — from-zero reset, survivors-only, EN+pt-BR in sync

| Field | Value |
|-------|-------|
| **Slug** | `khal-docs-v1` |
| **Date** | 2026-07-04 |
| **WRS** | 100/100 |
| **Repo** | khal-os/docs (GitHub, public, Mintlify) |

## Problem
The docs ballooned to 60 pages / 9 tabs; reset to a from-zero **v1** that keeps alive only the two correlated survivors (PR #39's tightened Access pages + the khal CLI reference), consolidated and de-duped, with **EN + pt-BR always in sync**, delivered as one PR — and codify a "one page at a time, for humans, no bloat" contract so it never balloons again.

## Scope
### IN
- **Archive, don't delete.** Move all non-survivor pages (KHAW, KHORTEX, Brain, Pack Development, SDK, patterns, Visual Proof, Reference, and every non-survivor page in both EN and pt-BR) to `_archive/` — out of the nav, recoverable, rebuilt later one page at a time.
- **v1 nav (docs.json) = one "Start with khal" section**, mirrored in pt-BR ("Comece com o khal"), listing only the survivor pages — each present in **both** languages.
- **The 5 survivor pages** (each EN **and** pt-BR):
  1. `fde-start-here/access/overview` — the v1 landing / what+why + the one command.
  2. `fde-start-here/access/engineer-guide` — the guided onboarding narrative (enroll → workstation → connect).
  3. `fde-engineer/khal-cli` — the command reference (families + agent paste-blocks). **Add the pt-BR translation** (currently EN-only).
  4. `fde-start-here/access/admin-guide` — granting access (admin audience).
  5. `fde-start-here/access/troubleshooting` — fixing access.
- **De-dup:** trim `khal-cli` to a **pure reference** — drop the re-narration of onboarding; point to the engineer-guide for the guided flow (kills the correlation overlap Cezar flagged). Keep the agent paste-blocks.
- **Docs contract** (add to the repo's `CONTRIBUTING.md` / `CLAUDE.md`): one page per PR, human-reviewed; **EN + pt-BR ship together, always in sync**; each page does one thing; no repeated facts; keep the "copy → send to your agent" blocks; write for humans.
- **One consolidation PR** off `main`; Mintlify build clean, nav shows only the v1.

### OUT
- No hard-delete (archive only; git history + `_archive/` preserve everything).
- No rewriting the #39 Access pages beyond the single de-dup touch to `khal-cli`.
- No new content beyond the survivors — KHAW/KHORTEX/Brain/Pack-Dev/etc. are rebuilt **later**, one page at a time, from the archive.
- **No EN-only or pt-BR-only pages** — language sync is mandatory, not optional.

## Approach
A single PR: (1) `git mv` every non-survivor page (both languages) into `_archive/`; (2) rewrite `docs.json` to a two-language mirror of one "Start with khal" section holding only the 5 survivors; (3) translate `fde-engineer/khal-cli` to pt-BR and trim both language versions to pure reference (de-dup vs the engineer-guide); (4) codify the docs contract. Archive-not-delete respects the blast radius (KHAW/Brain may document live products) while giving a clean v1 site now and raw material for the one-page-at-a-time rebuild. Keeping the 5 survivors (vs consolidating to 2-3) respects Cezar's freshly-tightened #39 work and only removes the one overlap he flagged. EN+pt-BR-in-sync is enforced by the contract so the parallel surface never drifts.

## Decisions
| Decision | Rationale |
|----------|-----------|
| Archive to `_archive/`, not delete | Clean v1 site now; KHAW/Brain docs (possibly live products) stay recoverable; feeds the rebuild |
| Keep the 5 survivors, de-dup only | Don't rewrite Cezar's just-tightened #39; remove only the flagged overlap |
| khal-cli → pure reference | Onboarding narrative lives once (engineer-guide); the CLI page references, never re-narrates |
| EN + pt-BR always in sync (contract) | Founder hard rule; a page never exists in one language only |
| One-page-per-PR docs contract | Codifies "no bloat / for humans / reviewed by one" so it can't regress |

## Risks & Assumptions
| Risk | Severity | Mitigation |
|------|----------|------------|
| Archiving docs for LIVE products (KHAW/Brain) hides real info | Medium | Archive (not delete) keeps them reachable in-repo; rebuild plan brings the needed ones back one page at a time |
| Mintlify i18n nav wiring (EN + pt-BR mirror) breaks | Medium | Follow the existing "COMECE AQUI" pt-BR-tab pattern; `mint broken-links` gate on the PR |
| khal-cli pt-BR translation drifts from EN | Medium | The contract makes sync mandatory + reviewed together; translate in the same PR |
| Redirects: archived URLs 404 for existing links | Low | Add `redirects` in docs.json for any high-value archived path, or accept 404 on internal-only docs |

## Success Criteria
- [ ] The Mintlify nav shows **only** the v1: one "Start with khal" section (+ its pt-BR mirror) with exactly the 5 survivor pages; no KHAW/KHORTEX/Brain/Pack-Dev/Visual/Reference tabs remain in nav.
- [ ] Every v1 page exists in **both** EN and pt-BR (incl. the new `khal-cli` pt-BR); no page is single-language.
- [ ] `khal-cli` is reference-only (no onboarding re-narration) and keeps its agent paste-blocks; the engineer-guide owns the narrative.
- [ ] All non-survivor pages live under `_archive/` (nothing deleted); `mint broken-links` passes.
- [ ] The docs contract (one-page-per-PR, EN+pt-BR-in-sync, no-bloat, for-humans, keep agent-blocks) is codified in `CONTRIBUTING.md`/`CLAUDE.md`.
- [ ] Delivered as one PR to `main`; Mintlify preview renders the clean v1.
