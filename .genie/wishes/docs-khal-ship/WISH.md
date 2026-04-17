# WISH: docs-khal-ship — patterns + dev + release + reference (7 pages)

## Context

Ship-side of the FDE manual: reference implementations, local dev loop, how a pack gets published, how a customer installs it, and the canonical reference pages.

Context: IA at `/home/genie/workspace/genie-docs/reports/khal-os-fde-ia-draft.md`; council verdict at `/home/genie/workspace/genie-docs/reports/khal-os-council-verdict.md`.

## Kernel-protection guardrails (APPLY)

1. Never cite `core/`, `platform/`, `brain/`, `deploy/` source paths.
2. Expose contracts, hide mechanisms.
3. Secrets: FDE declares, platform provides.
4. **Pack-authoring ≠ pack-deployment.** The `release/*` pages describe what FDE does to prepare a pack. `customer-install` is thin — it names what a customer admin runs, without exposing platform internals.
5. NATS surface only.

## Source of truth

- `/home/genie/workspace/repos/khal-os/pack-settings/` — frontend-only pattern.
- `/home/genie/workspace/repos/khal-os/pack-terminal/` — full-stack pattern (PTY service).
- `/home/genie/workspace/repos/khal-os/pack-notes/` — public reference pack.
- `/home/genie/workspace/repos/khal-os/pack-template/README.md` + `.github/workflows/` — CI pipeline the FDE gets for free.
- `/home/genie/workspace/repos/khal-os/app-kit/packages/dev-cli/` — the `khal-dev` CLI (QA + scaffolding).
- `/home/genie/workspace/repos/khal-os/app-kit/packages/types/src/manifest.ts` — KhalAppManifest schema.

## Pages to author

### `patterns/frontend-only-pack.mdx`

Walk the reader through `pack-settings` as the canonical frontend-only pattern.

- Tree: no `service/` dir.
- Show the `khal-app.json` (no services[] entry).
- Highlight SDK usage: `useKhalAuth` for user context, React state for local UI.
- Publishing: npm-only (no Docker, no Helm).
- `<Frame>` placeholder — alt: "pack-settings running inside the khal-os shell".

### `patterns/full-stack-pack.mdx`

Walk `pack-terminal` as the canonical full-stack pattern.

- Tree: both `package/` and `service/`.
- `khal-app.json` with services[] entry.
- Frontend: `useNats` subscribes to the PTY stream; xterm.js renders it.
- Service: Bun server, subscribes to NATS from the frontend, spawns PTY, publishes stdout.
- Deploy shape: the service runs as a pod via Helm; the frontend is an npm package consumed by the shell.
- `<Frame>` placeholder — alt: "pack-terminal with an active PTY session inside khal-os".
- Explicit note: don't reveal kernel-side PTY lifecycle — just "your service owns the PTY".

### `dev/local-dev-and-testing.mdx`

The unified dev + test loop — council collapsed 10 candidate pages into this one.

Sections:
1. **Build + watch:** `bun run build`, `bun run watch` (verify commands from pack-template).
2. **Typecheck + lint:** `bun run typecheck`, `bun run lint`.
3. **Unit tests:** vitest + mock NATS client example (short).
4. **Integration tests:** one paragraph — hit your service with a test NATS subject.
5. **`khal-dev` QA CLI:** list the key commands from `app-kit/packages/dev-cli/src/commands/` (e.g., `khal-dev qa look`, `khal-dev qa assert`, `khal-dev qa health`) — one line each, plus an example invocation.
6. **Debug tips:** browser DevTools (F12 in Tauri), service logs (`docker logs` while dev-running).
7. **`<Frame>` placeholder** — alt: "khal-dev qa output during a local test run".

VERIFY `khal-dev` subcommands against `app-kit/packages/dev-cli/src/commands/` — list only real ones.

### `release/publish-your-pack.mdx`

The CI-driven publish flow from `pack-template/.github/workflows/`. NOT the platform's deploy.

Sections (use `<Steps>`):
1. **Push to `dev` branch** → CI publishes `@khal-os/pack-<name>@next` to GitHub Packages + tags Docker image `:next`.
2. **Merge to `main`** → CI publishes `@latest` + tags Docker image `:latest`.
3. **Tag `v*`** → CI packages the Helm chart and pushes to `oci://ghcr.io/khal-os/charts`.
4. **Table of CI workflows:** from pack-template/README.md — ci.yml, publish-npm.yml, docker-build.yml, helm-release.yml, release.yml. One line each.
5. **What this means:** after tag, your pack is ready to install. The customer-install page covers the customer side.
6. **`<Note>`:** FDE never SSHes into a cluster. Publishing is a git push.

### `release/customer-install.mdx`

Intentionally THIN. Customer admins (not FDE) install packs. Keep FDE-relevant:

- **What the customer needs:** your pack's Helm chart name + version (`ghcr.io/khal-os/charts/pack-<name>:<version>`).
- **What the FDE hands off:** the Helm chart URL + a minimal `values.yaml` example (with env vars declared but secrets marked as customer-supplied).
- **What happens next:** customer runs their install process (Helm or platform UI). Don't document the platform's install internals.
- **When to update:** bumping the pack version on main triggers a new chart; customers opt in via the platform's upgrade flow.
- `<Warning>`: "If you need environment-specific config per customer, declare it in `khal-app.json` `env[]`. Never hardcode customer URLs / tokens in source."

### `reference/khal-app-json-schema.mdx`

The exhaustive manifest reference. Source: `/home/genie/workspace/repos/khal-os/app-kit/packages/types/src/manifest.ts`.

Approach: a `<ParamField>` table for every top-level field and nested field. Include types, defaults, required/optional, and a 1-line description.

Include a complete example showing every field populated.

DO NOT copy JSDoc verbatim — paraphrase, and link to source for the canonical truth.

### `reference/example-packs.mdx`

A matrix of reference packs with a one-line pitch per pack:

| Pack | Pattern | What to learn here |
|------|---------|-------------------|
| pack-settings | Frontend-only | useKhalAuth, React + SDK basics |
| pack-notes | Frontend-only (public) | Clean data-model example |
| pack-terminal | Full-stack (PTY) | useNats + service + Dockerfile + Helm |
| pack-files | Full-stack | File manager patterns |
| pack-genie | Full-stack with AI | Embedding agent features |
| pack-nats-viewer | Dev tool pattern | Introspecting messages |
| pack-hello | Hello-world | Minimum viable pack |

Each row links to the pack's GitHub repo (`github.com/khal-os/pack-<name>`) — do NOT deep-link into private pack-* source files.

## Constraints + PR

- Mintlify components; `<Steps>` for the release flow; `<ParamField>` for the schema.
- `<Frame>` placeholders everywhere a screenshot would go.
- One PR titled `docs(khal-os): patterns + dev + release + reference — 7 pages`.
- Branch: `docs-khal-ship-20260417`. Base: `main`.
