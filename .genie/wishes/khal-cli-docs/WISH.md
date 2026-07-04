# Wish: khal CLI docs — the whole command set, agent-first, minimal

| Field | Value |
|-------|-------|
| **Status** | DRAFT |
| **Slug** | `khal-cli-docs` |
| **Date** | 2026-07-04 |
| **Author** | cezar |
| **Appetite** | 1 small docs build (single wave, 2 groups) |
| **Branch** | `wish/khal-cli-docs` |
| **Repo** | khal-os/docs (GitHub, Mintlify) |
| **Design** | _No brainstorm — direct wish_ |

## Summary

FDE engineers (and their agents) have no single, concise map of the `khal` CLI. Add one minimal, agent-first reference page under the FDE Engineer tab that covers the entire command set grouped by intent, and — this is the point — puts a **"copy this → send to your agent"** block on every family so a human hands the context straight to their agent to act. Minimal prose, no flag dumps; `khal <cmd> --help` stays the local truth.

## Scope

### IN
- **`fde-engineer/khal-cli.mdx`** — one page, matching `khaw/cli-reference.mdx` style (frontmatter + one `<Callout>` "human map; `khal <cmd> --help` is local truth" + concise per-family sections).
- Cover **all 31 commands** in 4 families: **onboarding/identity** (fde, login, auth, whoami, logout, source, git, registry, permissions), **build & ship** (new, start, build, deploy, install, promote, route, router, lifecycle, link, pull), **ops & introspection** (list, logs, telemetry, infra, context, doctor, config, target, dev), **maintenance** (update). Each section: 1-2 sentences what+why + a bash code block of the key commands.
- A **"Copy this → send to your agent"** fenced code block per family (+ one for one-command onboarding `khal fde join`): agent-ready context — purpose, exact commands, gotchas — tight and self-contained (Mintlify's built-in copy button is the mechanism).
- **`docs.json`** nav registration under the FDE Engineer tab ("CLI & identity" group, or a new "khal CLI" group).
- Optional reusable **snippet** (`snippets/paste-to-agent.mdx`) wrapping the paste-to-agent block if it cuts repetition.

### OUT
- No changes to the khal CLI itself (docs only).
- No exhaustive per-flag reference — point to `--help`.
- No KHAW/KHORTEX/pack-dev doc changes.
- No pt-BR translation (English first; pt-BR is a follow-up).
- No `khal-admin` (operator-only, separate).
- Do not balloon into a full manual — minimalism is a requirement, not a nice-to-have.

## Decisions

| # | Decision | Rationale |
|---|----------|-----------|
| 1 | One page, family-grouped | Agents/humans want a fast map, not 31 pages; `--help` is the deep reference |
| 2 | Paste-to-agent block per family | Agent-first: the human copies context, the agent acts — the company's whole ethos |
| 3 | Match `khaw/cli-reference.mdx` style | Consistency + proven minimal format |
| 4 | Point to `--help`, no flag dumps | Live CLI help is truth; docs that duplicate flags rot |

## Success Criteria

- [ ] `fde-engineer/khal-cli.mdx` exists, renders in the Mintlify build, and is reachable in the FDE Engineer nav.
- [ ] Every one of the 31 `khal` commands appears under exactly one family; each family has a 1-2 sentence intro + a bash code block.
- [ ] Every family (and `khal fde join`) has a "Copy this → send to your agent" block with self-contained agent-ready context and a working copy button.
- [ ] No broken links; the Mintlify build passes; prose stays minimal (no flag dumps).

## Execution Strategy

| Wave | Group | Description |
|------|-------|-------------|
| 1 | 1 | Author `fde-engineer/khal-cli.mdx` — all families + per-family paste-to-agent blocks |
| 1 | 2 | Register in `docs.json` nav (+ optional snippet) + Mintlify build/link check |

Single wave, **sequential** (G1 → G2): the page must exist before it can be registered + build-checked.

---

## Execution Groups

### Group 1: Author the khal CLI reference page
**Goal:** One concise, agent-first page covering the whole command set.

**Deliverables:**
1. `fde-engineer/khal-cli.mdx`: frontmatter (title/description) + one `<Callout type="info">` ("human map; `khal <cmd> --help` is the local truth").
2. Four family sections (onboarding/identity · build & ship · ops & introspection · maintenance), each = 1-2 sentences + a bash code block of the key commands; all 31 commands placed.
3. A "Copy this → send to your agent" fenced block per family + one for `khal fde join` (one-command onboarding), each self-contained (purpose + commands + gotchas).

**Acceptance Criteria:**
- [ ] All 31 commands appear under exactly one family; no flag dumps (each defers to `--help`).
- [ ] Each family + `khal fde join` has a paste-to-agent block; prose is minimal.

**Validation:**
```bash
cd /tmp/khal-docs && grep -c 'send to your agent' fde-engineer/khal-cli.mdx   # >= 5 (4 families + onboarding)
```

**depends-on:** none

### Group 2: Register in nav + build check
**Goal:** The page is discoverable and the site builds clean.

**Deliverables:**
1. `docs.json`: add `fde-engineer/khal-cli` to the FDE Engineer tab (CLI & identity group, or a new "khal CLI" group).
2. Optional `snippets/paste-to-agent.mdx` if it de-duplicates the wrapper.
3. Mintlify build/link check.

**Acceptance Criteria:**
- [ ] The page is in the FDE Engineer nav; the Mintlify build passes with no broken links.

**Validation:**
```bash
cd /tmp/khal-docs && npx mint@latest broken-links 2>/dev/null || mint broken-links
```

**depends-on:** khal-cli-docs#1
