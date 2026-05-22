# KHAL OS public docs

This repository contains the public Mintlify documentation site for KHAL OS. It is the public-facing docs surface for product concepts, app/pack authoring, SDK usage, and release/customer handoff guidance.

## Local development

Run commands from the repository root, where `docs.json` lives:

```bash
npx --yes mint@latest dev
npx --yes mint@latest broken-links
```

If you prefer a global CLI, install Mintlify with `npm i -g mint` and run `mint dev` or `mint broken-links`.

## Public/private boundary

These docs are public. Do not add secrets, tokens, credentials, customer identifiers, private hostnames, internal IPs, live monitoring URLs, admin usernames, migration/cutover notes, or operator-only runbook commands. Keep production operations and private topology in internal Hermes/KHAL runbooks unless Felipe explicitly approves publication.

## Pack distribution policy

Pack/app repos are git-native: Marketplace/App Store installation reads the git repo and `khal-app.json`, then builds/loads the app from that source. Do not document `pack-*` repos as npm or GitHub Packages artifacts.

The npm packages that belong in public docs are the app-kit/framework packages only:

- `@khal-os/sdk`
- `@khal-os/ui`
- `@khal-os/types`
- `@khal-os/dev-cli`

## Review requirement

Open a PR for documentation changes and request review before publishing or merging to the default branch. Public docs changes need product/schema review for technical truth and public/private boundary review for safety.
