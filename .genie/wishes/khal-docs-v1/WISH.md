# Wish: khal docs v1 — from-zero reset, survivors-only, EN+pt-BR in sync

| Field | Value |
|-------|-------|
| **Status** | DRAFT |
| **Slug** | `khal-docs-v1` |
| **Date** | 2026-07-04 |
| **Author** | cezar |
| **Appetite** | 1 focused docs build (single wave, 3 sequential groups) → one PR |
| **Branch** | `wish/khal-docs-v1` |
| **Repo** | khal-os/docs (GitHub, Mintlify; automagik-genie admin) |
| **Design** | [DESIGN.md](../../brainstorms/khal-docs-v1/DESIGN.md) |

## Summary

The docs ballooned to 60 pages / 9 tabs. Reset to a from-zero **v1** that keeps alive only the two correlated survivors — PR #39's tightened Access pages + the khal CLI reference — consolidated + de-duped, **EN + pt-BR always in sync**, delivered as ONE PR. Archive the rest (recoverable), and codify a "one page at a time, for humans, no bloat" contract so it never balloons again.

## Scope

### IN
- **Archive (git mv, not delete)** every non-survivor page — EN **and** pt-BR — to `_archive/`: KHAW, KHORTEX, Brain, Pack Development, SDK, patterns, Visual Proof, Reference, getting-started + all pt-BR mirrors.
- **docs.json v1 nav** = one "Start with khal" section + its pt-BR mirror ("Comece com o khal"), listing ONLY the 5 survivors, each in **both** languages. All other tabs removed from nav.
- **5 survivors** (each EN + pt-BR): `fde-start-here/access/{overview, engineer-guide, admin-guide, troubleshooting}` (from #39, kept) + `fde-engineer/khal-cli`. **Add khal-cli pt-BR** (currently EN-only).
- **De-dup:** trim `khal-cli` (EN + pt-BR) to a **pure reference** — drop the onboarding re-narration, point to the engineer-guide; **keep the agent paste-blocks**.
- **Docs contract** in `CONTRIBUTING.md`/`CLAUDE.md`: one page per PR, human-reviewed; EN+pt-BR always in sync; one thing per page; no repeated facts; keep agent blocks; for humans; page-budget.
- Minimal v1 landing (access/overview as entry) so the root resolves; docs.json redirects for high-value archived paths if trivial.

### OUT
- No hard-delete (archive only; git + `_archive/` preserve all).
- No rewriting #39 Access pages beyond the single de-dup touch to khal-cli.
- No new content beyond the survivors (KHAW/Brain/etc. rebuilt later, one page at a time).
- No EN-only or pt-BR-only pages (sync mandatory).
- No changes to the app-kit khal CLI itself.

## Decisions

| # | Decision | Rationale |
|---|----------|-----------|
| 1 | Archive to `_archive/`, not delete | Clean v1 site now; live-product docs recoverable; feeds the rebuild |
| 2 | Keep the 5 survivors, de-dup only | Respect #39's fresh tightening; remove only the flagged overlap |
| 3 | khal-cli → pure reference | Narrative lives once (engineer-guide); CLI references, never re-narrates |
| 4 | EN + pt-BR always in sync | Founder hard rule; no single-language pages |
| 5 | One-page-per-PR docs contract | Codifies no-bloat/for-humans so it can't regress |

## Success Criteria

- [ ] Mintlify nav shows ONLY the v1: one "Start with khal" section (+ pt-BR mirror) with the 5 survivors; no other tabs remain.
- [ ] Every v1 page exists in BOTH EN and pt-BR (incl. new khal-cli pt-BR); no single-language page.
- [ ] khal-cli is reference-only (no onboarding re-narration), keeps its agent paste-blocks.
- [ ] All non-survivor pages under `_archive/` (nothing deleted); `mint broken-links` passes; docs.json valid.
- [ ] Docs contract present in `CONTRIBUTING.md`/`CLAUDE.md`.
- [ ] Delivered as one PR to `main`; Mintlify preview renders the clean v1.

## Execution Strategy

| Wave | Group | Description |
|------|-------|-------------|
| 1 | 1 | Demolition — git mv non-survivors (EN+pt-BR) to `_archive/` + rewrite docs.json to the v1 two-language single-section nav |
| 1 | 2 | Survivors — de-dup khal-cli (EN) + author khal-cli pt-BR in sync + confirm the 4 Access pages render in both languages |
| 1 | 3 | Docs contract + full build gate + open the single consolidation PR |

Single wave, **sequential** (G1 → G2 → G3): the nav must exist before survivors are wired; the contract + PR come last.

---

## Execution Groups

### Group 1: Demolition — archive + v1 nav
**Goal:** A clean v1 site skeleton — only survivors in nav, everything else archived.

**Deliverables:**
1. `git mv` every non-survivor `.mdx` (EN + pt-BR) into `_archive/` preserving relative paths.
2. Rewrite `docs.json` navigation to one "Start with khal" section + a pt-BR mirror, listing only the 5 survivor pages (both languages). Remove all other tabs.
3. Redirects in docs.json for any high-value archived path (else accept internal 404s).

**Acceptance Criteria:**
- [ ] `docs.json` is valid JSON; nav contains only the "Start with khal" section (+ pt-BR mirror) with the 5 survivors.
- [ ] All non-survivor pages are under `_archive/`; `git status` shows moves (renames), no deletions.

**Validation:**
```bash
cd /tmp/khal-docs && python3 -c "import json;json.load(open('docs.json'))" && ls _archive/ | head
```

**depends-on:** none

### Group 2: Survivors — de-dup + khal-cli pt-BR in sync
**Goal:** The 5 survivors render in the new nav, in both languages, de-duped.

**Deliverables:**
1. Trim `fde-engineer/khal-cli.mdx` to a pure reference (drop onboarding re-narration → point to the engineer-guide); keep the agent paste-blocks.
2. Author the pt-BR translation of khal-cli (mirror path, in sync with the trimmed EN).
3. Confirm the 4 Access pages (EN + pt-BR) resolve under the new nav.

**Acceptance Criteria:**
- [ ] khal-cli exists in EN + pt-BR, both reference-only, both with agent paste-blocks, in sync.
- [ ] All 5 survivor pages resolve in the new nav in both languages.

**Validation:**
```bash
cd /tmp/khal-docs && test -f fde-engineer/khal-cli.mdx && test -f pt-BR/fde-engineer/khal-cli.mdx && grep -c 'send to your agent' fde-engineer/khal-cli.mdx
```

**depends-on:** khal-docs-v1#1

### Group 3: Docs contract + build gate + PR
**Goal:** Lock the ethos and ship the single consolidation PR.

**Deliverables:**
1. Add the docs contract (one-page-per-PR, EN+pt-BR-in-sync, no-bloat, for-humans, keep agent-blocks) to `CONTRIBUTING.md` and/or `CLAUDE.md`.
2. Run the full `mint broken-links` build gate; fix any break.
3. Open ONE PR to `main` (Mintlify preview for founder review).

**Acceptance Criteria:**
- [ ] `mint broken-links` passes; the docs contract is present.
- [ ] A single PR is open to `main` rendering the clean v1 preview.

**Validation:**
```bash
cd /tmp/khal-docs && npx -y mint@latest broken-links 2>&1 | tail -2
```

**depends-on:** khal-docs-v1#1, khal-docs-v1#2
