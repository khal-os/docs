# WISH: docs-khal-sdk — SDK hooks + services (6 pages)

## Context

The SDK is the FDE's primary surface. These 6 pages teach the three hooks (`useNats`, `useKhalAuth`, `useService`) and the backend-services surface they connect to.

Context: IA at `/home/genie/workspace/genie-docs/reports/khal-os-fde-ia-draft.md`; council verdict at `/home/genie/workspace/genie-docs/reports/khal-os-council-verdict.md`.

## Kernel-protection guardrails (APPLY — read carefully)

1. Never cite `core/`, `platform/`, `brain/`, `deploy/` source paths.
2. **Expose contracts, hide mechanisms.** This is the wish where this guardrail matters most. You will document what hooks RETURN, not how they ARE IMPLEMENTED.
3. Secrets: FDE declares, platform provides.
4. Pack-authoring ≠ pack-deployment.
5. NATS surface only — document subjects an FDE publishes/subscribes to. Never cluster topology, leaf nodes, JetStream internals, or `khal.catalog.*`.

## Source of truth

- `/home/genie/workspace/repos/khal-os/app-kit/packages/os-sdk/` — the SDK source. Read `src/` to extract hook signatures, return types, contract.
- `/home/genie/workspace/repos/khal-os/pack-terminal/` — the reference full-stack pack using all three hooks. Cite its `package/src/` for code examples.
- `/home/genie/workspace/repos/khal-os/pack-settings/` — reference frontend-only pack using `useKhalAuth`.

## Pages to author

### `sdk/hooks-reference.mdx`

A single landing page for all SDK hooks. Table at top:

| Hook | Purpose | Returns | Full docs |
|------|---------|---------|-----------|
| `useNats` | NATS client for pub/sub/RPC | `{ publish, subscribe, request }` | `/sdk/useNats` |
| `useKhalAuth` | Current user + auth state | `{ user, loading, signOut }` | `/sdk/useKhalAuth` |
| `useService` | Call YOUR own backend service | `{ call, stream }` | `/sdk/useService` |

Below: 1-paragraph intro, then `<CardGroup>` with cards linking to each hook's page. Note: this is the one page that acts as an index; deep docs live on the per-hook pages.

VERIFY return-type signatures against actual `os-sdk/src/` exports before writing.

### `sdk/useNats.mdx`

- **Signature:** `const nats = useNats()` — include real TS signature from source.
- **What it returns:** methods + their signatures (code block).
- **Basic example:** publish + subscribe (from pack-terminal's frontend).
- **RPC example:** request/reply pattern (from pack-terminal).
- **Subject naming:** how FDE forms subjects. (DO NOT expose internal subjects like `khal.catalog.*` — only the subjects FDE-owned packs use, typically scoped by app id + org id.)
- **Error handling:** one short section.
- **`<Warning>` callout:** never subscribe to `khal.internal.*` or `khal.catalog.*`. Those are platform plumbing.

### `sdk/useKhalAuth.mdx`

- **Signature:** `const { user, loading, signOut } = useKhalAuth()` (verify against source).
- **User object shape:** fields an FDE can read (id, email, orgId, role). DO NOT expose JWT structure, claim internals, or the platform's WorkOS integration.
- **Typical use:** conditional render while `loading`, show user name, handle `signOut` in UI.
- **Permissions gate:** mention `user.role` briefly — link forward to permissions overview (out of scope this wish, leave as TBD placeholder).
- **`<Note>`:** "The platform handles sign-in. Your pack reads the authenticated user."

### `sdk/useService.mdx`

- **Signature:** how to call your pack's own backend service.
- **Example:** call a service endpoint, handle response.
- **When to use vs `useNats`:** useService for direct backend calls your service exposes; useNats for broadcast / pub-sub across packs.
- **Example from pack-terminal service/src/** showing matched frontend call + backend handler.

### `services/backend-overview.mdx`

- **What a pack service is:** optional Bun-based backend, runs as a k8s pod via its Helm sub-chart.
- **Tree:** `service/src/index.ts`, `service/Dockerfile`, `service/healthcheck.sh`.
- **Talking to the frontend:** NATS is the primary transport (via `useNats` on the frontend, subscribing on the service).
- **Talking to external APIs:** regular `fetch` or whatever you like.
- **Lifecycle:** don't document k8s pod lifecycle, just "the platform runs your service alongside your pack frontend and terminates it when the pack is uninstalled".
- **`<Warning>`:** "Services should be stateless or externalize state. See environment-config for env secrets."
- Link forward to pack-terminal full-stack pattern.

### `services/environment-config.mdx`

- **How FDE declares env vars:** `khal-app.json` `env[]` entries with `{name, type}`. `type: 'secret'` for secrets, `type: 'string'` for plain config.
- **What the platform does:** injects values at runtime. Hard stop. Don't explain k8s secrets, Vault, CRDs, rotation. Sentinel guardrail.
- **Accessing env vars:** from service code, `process.env.MY_VAR`. From frontend: DO NOT read secrets in the frontend. If a frontend needs config, it must come via the service.
- **Example:** `khal-app.json` env[] block for an API key + a non-secret feature flag.

## Constraints + PR

- VERIFY every hook signature against source before writing. Do not guess.
- Mintlify components; `<ParamField>` for hook argument/return documentation.
- One PR titled `docs(khal-os): SDK hooks + services — 6 pages`.
- Branch: `docs-khal-sdk-20260417`. Base: `main`.
