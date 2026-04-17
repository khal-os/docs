# WISH: docs-khal-packs — Getting Started + Pack Fundamentals (5 pages)

## Context

Khal-os docs greenfield authoring. This wish covers **Setup & Quickstart** and **Pack Fundamentals** — the FDE's first physical steps after they click into the docs.

Context docs:
- Canonical IA: `/home/genie/workspace/genie-docs/reports/khal-os-fde-ia-draft.md`
- Council verdict: `/home/genie/workspace/genie-docs/reports/khal-os-council-verdict.md`

**Depends on:** `docs-khal-home` should land first (it wires `docs.json`). If it hasn't, create the pages anyway and `docs-khal-home` will wire them up.

## Kernel-protection guardrails (APPLY)

1. Never cite `core/`, `platform/`, `brain/`, or `deploy/`.
2. Expose contracts, hide mechanisms.
3. Secrets: FDE declares, platform provides.
4. Pack-authoring ≠ pack-deployment.
5. NATS surface only.

## Execution groups — author these 5 pages

### `getting-started/clone-workspace.mdx`

- Shows `gh auth login` + `git clone --recurse-submodules https://github.com/khal-os/genie.git khal-os` + `cd khal-os` + `./init.sh`.
- Explain that `init.sh` is the single bootstrap and is idempotent.
- Mention: `./init.sh --help` for options.
- DO NOT list all 15 submodule repo names (kernel-architecture leak — per sentinel guardrail). Just say "the workspace materializes every khal-os repo so you can jump between SDK, reference packs, and your own pack freely."
- Prereq box: `git ≥ 2.30`, `gh CLI` authenticated, Node + pnpm + bun installed.
- Source of truth: `/home/genie/workspace/repos/khal-os/genie/README.md`.

### `getting-started/install-dependencies.mdx`

- pnpm for `app-kit/` (workspace root uses pnpm).
- bun for `pack-*/` repos (pack-template uses bun).
- Node version expectations.
- One-shot: `pnpm install` at workspace root (if that bootstraps submodules — verify from `init.sh`).
- Source: `/home/genie/workspace/repos/khal-os/app-kit/README.md` (mentions `pnpm install; pnpm build; pnpm typecheck; pnpm lint`), `/home/genie/workspace/repos/khal-os/pack-template/README.md` (uses `bun install; bun run build`).
- `<CodeGroup>` with separate pnpm / bun blocks.

### `getting-started/your-first-pack.mdx`

The flagship "hello world" page. An FDE lands here and walks out with a running pack.

Sections:
1. **Scaffold from template:** `gh repo create khal-os/pack-<name> --template khal-os/pack-template --private` + `cd pack-<name>`.
2. **Rename the scaffold:** the 5-file rename list from `pack-template/README.md` (package.json, khal-app.json, helm/Chart.yaml, helm/values.yaml, root package.json).
3. **Edit the React component:** point to `package/src/index.tsx`. Show a minimal "Hello FDE" component.
4. **(Optional) edit the service:** `service/src/index.ts` — or delete `service/` if frontend-only.
5. **Build + typecheck:** `bun run build && bun run typecheck`.
6. **See it in the shell:** `<Frame>` placeholder — alt: "The scaffolded pack running inside the khal-os desktop shell". Instructions: install into a local khal-os instance or publish and install into `nmstx.khal.ai`. Keep this section short — link forward to `release/publish-your-pack.mdx` for the full CI-driven path.
7. **Next steps CardGroup:** links to packs/anatomy, sdk/hooks-reference, patterns/full-stack-pack.

Source of truth: `/home/genie/workspace/repos/khal-os/pack-template/README.md` + `pack-template/khal-app.json`.

### `packs/anatomy-of-a-pack.mdx`

Map the pack-template directory tree to its purpose. Use the exact tree from `pack-template/README.md`:

```
.
├── khal-app.json     # Manifest (validated by @khal-os/types)
├── package/          # Frontend npm package (@khal-os/pack-<name>)
│   ├── src/index.tsx # Default export: React component
│   ├── tsup.config.ts
│   └── package.json
├── service/          # (Optional) backend pod
│   ├── src/index.ts  # Bun HTTP server + NATS subscription
│   ├── Dockerfile
│   └── healthcheck.sh
├── helm/             # Helm sub-chart for the service
│   ├── Chart.yaml
│   ├── values.yaml
│   └── templates/
└── .github/workflows/ # CI + npm publish + Docker build + Helm release
```

Explain each dir in ≤3 sentences. Link `khal-app.json` → `packs/khal-app-json.mdx`. Link CI workflow table → `release/publish-your-pack.mdx`.

`<Frame>` placeholder — alt: "Pack directory tree labeled with runtime role of each file".

### `packs/khal-app-json.mdx`

Canonical manifest reference. The schema is defined in `/home/genie/workspace/repos/khal-os/app-kit/packages/types/src/manifest.ts` (Zod-validated `KhalAppManifest`). Read that file and reflect its fields.

Sections:
- **Required fields:** id, name, description, frontend.package (the npm name your pack publishes).
- **Optional fields:** services[], windows[], permissions[], env[] — one-line each, plus link to deep dives in reference pages.
- **Full example:** pull the `khal-app.json` from `pack-template/khal-app.json` and `pack-notes/khal-app.json` side-by-side in `<Tabs>` (frontend-only vs full-stack).
- **Validation:** mention `khal-dev` CLI will catch manifest errors at build time (don't deep-link to dev-cli internals — just that "the toolchain validates your manifest").
- Link forward to `reference/khal-app-json-schema.mdx` (written in docs-khal-ship) for the exhaustive schema.

## Constraints + PR

- Mintlify components throughout.
- `<Frame>` placeholders for every screenshot opportunity with descriptive alt text.
- DO NOT document the platform's deploy flow here — this wish is authoring, not deployment.
- One PR titled `docs(khal-os): getting started + pack fundamentals — 5 pages`.
- Branch: `docs-khal-packs-20260417`. Base: `main`.
