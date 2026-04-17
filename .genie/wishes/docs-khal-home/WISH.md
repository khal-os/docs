# WISH: docs-khal-home — brand the docs, write the narrative entry, kill the Mintlify starter

## Context

`khal-os/docs` is currently an **unmodified Mintlify Starter Kit** (`docs.json` says `"name": "Mint Starter Kit"`, nav is boilerplate `essentials/*`, `api-reference/endpoint/*`, etc.). A VC technical interview happens TODAY (2026-04-17). The site must open with a coherent FDE-manual narrative.

This is the **foundation wish**. It does two things: replaces the Mintlify starter with the real khal-os brand + nav, and authors the first 2 pages (Home + What is Khal-OS).

Context docs:
- Canonical IA: `/home/genie/workspace/genie-docs/reports/khal-os-fde-ia-draft.md`
- Council verdict (cuts to 20 pages, 5 guardrails, cross-org narrative): `/home/genie/workspace/genie-docs/reports/khal-os-council-verdict.md`
- Ecosystem brain: `/home/genie/workspace/repos/khal-os/genie/README.md` + `.genie/MANIFEST.md`

**Audience:** Forward Deployed Engineers hired to build + deploy khal apps (packs) into customer orgs.

## Kernel-protection guardrails (APPLY EVERYWHERE)

1. **Kernel is a black box.** Never cite `core/`, `platform/`, `brain/`, or `deploy/` source paths. Only `app-kit/`, `pack-template/`, and reference `pack-*/` may be referenced.
2. **Expose contracts, hide mechanisms.** "`useKhalAuth()` returns the user" ✓. "JWT verified via WorkOS public key" ✗.
3. **Secrets: FDE declares, platform provides.** Never mention k8s secrets, Vault, EnvironmentSecrets, or internal CRDs. Only "declare env in khal-app.json; platform injects".
4. **Pack-authoring ≠ pack-deployment.** `release/*` pages are "prepare your pack"; customer-side deployment is a thin separate page.
5. **NATS surface only.** Document subjects FDEs publish/subscribe to. Never cluster topology, leaf-nodes, JetStream internals, or `khal.catalog.*`.

## Execution groups

### Group A — Rewrite `docs.json` (nav + branding)

**Deliverable:** Replace the Mintlify Starter Kit `docs.json` with a khal-os-branded navigation that matches the 20-page FDE manual IA. Navigation structure:

```
Tabs:
  - Getting Started
      - Introduction:      index, what-is-khal-os
      - Setup & Quickstart: getting-started/clone-workspace, getting-started/install-dependencies, getting-started/your-first-pack
  - Pack Development
      - Fundamentals:      packs/anatomy-of-a-pack, packs/khal-app-json
      - SDK:               sdk/hooks-reference, sdk/useNats, sdk/useKhalAuth, sdk/useService
      - Services:          services/backend-overview, services/environment-config
      - Patterns:          patterns/frontend-only-pack, patterns/full-stack-pack
  - Ship It
      - Dev:               dev/local-dev-and-testing
      - Release:           release/publish-your-pack, release/customer-install
  - Reference
      - Manifest:          reference/khal-app-json-schema
      - Packs:              reference/example-packs
```

Branding:
- `"name": "Khal OS"`
- Pick a fitting primary color (match a khal-os green/teal; check for brand assets in `logo/` dir — if none, use `#0D9488` teal or similar OS-y tone).
- Retain Mintlify `contextual` options and keep `"theme": "mint"`.
- Favicon + logo dark/light: if brand assets exist in the repo, wire them; if not, leave placeholders with a TODO comment referencing `nmstx.khal.ai` for future branding capture.

**Nuke list:** delete these Mintlify starter pages and any references from nav:
- `essentials/settings.mdx`, `essentials/navigation.mdx`, `essentials/markdown.mdx`, `essentials/code.mdx`, `essentials/images.mdx`, `essentials/reusable-snippets.mdx`
- `ai-tools/cursor.mdx`, `ai-tools/claude-code.mdx`, `ai-tools/windsurf.mdx`
- `api-reference/introduction.mdx`, `api-reference/endpoint/get.mdx`, `api-reference/endpoint/create.mdx`, `api-reference/endpoint/delete.mdx`, `api-reference/endpoint/webhook.mdx`
- `development.mdx`, `quickstart.mdx` (they'll be replaced by the new Getting Started set)

KEEP the existing `brain/*` pages and the current `brain` tab (they were the only real content) — just move them to a Brain tab in the new nav if it makes sense, or park them temporarily and note in PR description that a separate wish will integrate them.

**Acceptance:** `docs.json` renders the new nav. No orphaned .mdx files referenced. All listed nuke-list files deleted.

### Group B — Author `index.mdx` (Home)

**Deliverable:** A Home page that opens with the cross-org narrative and leads a VC or FDE through a 60-second picture of the product.

Required sections:
1. **Hero** — title "Khal OS — the browser-based OS for agents and the humans who ship with them", with the cross-org narrative paragraph:
   > **Automagik builds the primitives. Khal-os runs on them.** `automagik-dev` ships the agentic development toolchain — `genie` orchestrates wishes into shipped code via autonomous teams, `omni` lets agents speak across every channel humans live in, and `rlmx` gives them a research-grade reasoning loop. `khal-os` is the operating system that hosts what those agents build — a browser-based, macOS-inspired shell where every human and every agent lives alongside the apps ("packs") they use together. Forward Deployed Engineers hired on khal-os don't rebuild the primitives; they reach for the automagik toolchain to author khal packs faster, then ship those packs into customer organisations via khal-os's deploy rails. **One story: Automagik is how you build. Khal-os is where it runs.**
2. **"Day 1 on khal-os" `<CardGroup>`** — 4 cards linking to: clone-workspace, your-first-pack, hooks-reference, publish-your-pack.
3. **"Learn by example" `<CardGroup>`** — cards linking to patterns/frontend-only-pack, patterns/full-stack-pack, reference/example-packs.
4. `<Frame>` placeholder for a hero screenshot of the khal-os desktop from `nmstx.khal.ai` — alt text: "The khal-os desktop shell with a handful of packs running".

### Group C — Author `what-is-khal-os.mdx`

**Deliverable:** One page, ~300–500 words, answering what khal-os is from an FDE POV.

Required sections (use `<Steps>`, `<CardGroup>`, `<Accordion>` as fits):
1. **What it is:** a browser-based OS shell where packs (apps) run for humans and agents side-by-side.
2. **What FDEs do:** author packs with the `@khal-os/*` SDK, wire them through the platform's NATS bus, ship them via CI → Helm → customer orgs.
3. **What the platform does (FYI only, no internals):** handles auth, window management, NATS bridging, secret injection, tenant isolation. One-liner each. Kernel is a black box.
4. **Where the automagik toolchain fits:** genie / omni / rlmx are how you BUILD packs faster. (Do not deep-link to automagik docs unless Mintlify cross-site linking is configured.)
5. **What's NOT in this manual:** kernel internals, deploy-platform internals — intentionally. This is the FDE path; other docs exist for maintainers.
6. `<Frame>` placeholder for an architecture diagram (alt: "Pack architecture: frontend package + optional Bun service + Helm chart, consumed by the khal-os shell").

**Source of truth:** Compose from `khal-os/genie/README.md`, `khal-os/genie/.genie/MANIFEST.md` (without listing all 15 repos by name — that's a kernel leak), and `app-kit/README.md`.

## Constraints

- Mintlify components throughout: `<Card>`, `<CardGroup>`, `<Steps>`, `<Frame>`, `<Tabs>`, `<CodeGroup>`, `<Note>`, `<Warning>`.
- No wall-of-text. Paragraphs 3–5 sentences max.
- Every screenshot = `<Frame>` placeholder with descriptive alt text. No fabricated UI.
- Narrative tone matches the SOUL.md voice of concrete, shipping-oriented language.

## Success criteria

- `mintlify dev` renders without errors.
- Nav shows the 20-page IA (even if subsequent wishes populate the content later).
- Home + What-is load cleanly; no Mintlify starter-kit residue visible.
- PR titled `docs(khal-os): home, narrative, IA rewrite — VC v1`.

## PR target

Branch: `docs-khal-home-20260417`. PR base: `main`.
