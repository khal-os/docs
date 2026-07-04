# docs — khal.ai public documentation

> You are working inside **`repos/docs/`**, the public Mintlify documentation site for KhalOS. Pages are MDX with YAML frontmatter; navigation + branding live in `docs.json`. This is the public-facing surface — the only khal-os org repo that's truly public-public.

## The docs contract (v1) — READ FIRST

khal docs was reset to a deliberately small **v1**: the onboarding + `khal` CLI core only. Everything else lives in `_archive/` and is rebuilt **one page at a time**. Keep it small.

- **One page per PR**, reviewed by one human. No mega-PRs.
- **EN + pt-BR always in sync.** Every page exists in both languages and ships together — never one without the other (EN at `<path>`, pt-BR at `pt-BR/<path>`).
- **One thing per page.** If it does two things, it's two pages.
- **No repeated facts.** State a fact once, link to it. A reference page references; a guide narrates — never both.
- **Keep the agent blocks.** Every page an agent would act on carries a `🤖 copy → send to your agent` accordion.
- **Write for humans.** Minimal prose, no chatty asides. `khal <cmd> --help` is the local truth; docs are the map.
- **Grow from `_archive/`.** Bring an archived page back only when needed, tightened, in its own PR (EN+pt-BR).

**NO FUCKING BLOAT.**

## What lives here

```
docs/
├── docs.json              # Site config: navigation, branding, redirects
├── index.mdx              # Landing page
├── index.mdx              # START HERE landing page
├── fde-start-here/        # Day 0 enrollment/toolchain/access pages
├── dev/                   # FDE CLI, environment ladder, source control, local dev
├── khaw/                  # KHAW, Khortex, model credential docs
├── brain/                 # @khal-os/brain user docs
├── snippets/              # Reusable MDX snippets
├── images/                # Static images
├── logo/                  # Brand assets
├── favicon.svg
├── README.md              # Repo-level readme
├── CONTRIBUTING.md        # Contribution guide for doc PRs
└── LICENSE
```

## Quick start

```bash
mint dev              # local preview on http://localhost:3000
mint broken-links     # check for dead internal links before shipping
```

Install Mintlify CLI via npm: `npm i -g mint`.

## Related

- **Mintlify docs** → https://mintlify.com/docs (component reference, config schema)
- **Workspace manifest** → `../../.genie/MANIFEST.md`
