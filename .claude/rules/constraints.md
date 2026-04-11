# Constraints — docs

- **NEVER** commit marketing copy, names, or claims that haven't been approved by the human. If in doubt, ask.
- **NEVER** publish documentation for features that don't exist in the actual product. "Coming soon" sections drift and mislead — keep them out.
- **NEVER** leak internal paths, internal agent naming, `.genie/wishes/` content, or customer names into public docs.
- **NEVER** link to private repos (`khal-os/core`, `khal-os/desktop`, `khal-os/deploy`, etc.) — they 404 for readers without org access.
- **NEVER** embed secrets, API tokens, or credentials in example code. Use placeholders (`YOUR_API_KEY`).
- **NEVER** ship a PR with broken internal links. Run `mint broken-links` before committing.
- **NEVER** modify `docs.json` navigation structure without a reason — changes affect SEO, inbound links, and user muscle memory.
- **NEVER** rename or move a page without adding a redirect in `docs.json`. Broken URLs hurt search ranking.
- **NEVER** push directly to `main`. This repo has no `dev` branch — open a feature branch (`docs/*`) and PR against `main`. Humans merge.
- **NEVER** copy a large block of code from a private repo. Write a minimal example that demonstrates the concept.

## Authority matrix

### Can do without approval

- Fix typos, grammar, formatting
- Add missing examples to existing pages
- Reorganize sections within a page
- Add new pages under existing sections (update `docs.json` navigation accordingly)
- Update outdated command output / screenshots
- Fix broken internal links
- Run `mint dev` + `mint broken-links` locally

### Requires human approval

- New top-level navigation sections
- Changes to `docs.json` branding (colors, logo, favicon)
- New product claims / positioning copy
- Pages about unreleased features
- API reference changes (usually auto-generated from OpenAPI — check the source)
- Redirects that change URL structure at scale
- Merging to `main`
