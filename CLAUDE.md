# docs — khal.ai public documentation

> You are working inside **`repos/docs/`**, the public Mintlify documentation site for KhalOS. Pages are MDX with YAML frontmatter; navigation + branding live in `docs.json`. This is the public-facing surface — the only khal-os org repo that's truly public-public.

## What lives here

```
docs/
├── docs.json              # Site config: navigation, branding, redirects
├── index.mdx              # Landing page
├── quickstart.mdx         # Getting-started walkthrough
├── development.mdx        # Local dev + contribution guide
├── ai-tools/              # AI-focused guides (Claude, etc.)
├── api-reference/         # API docs (OpenAPI-driven where possible)
├── essentials/            # Core concepts + how-tos
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
