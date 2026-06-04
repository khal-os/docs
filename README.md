# Khal OS public docs

This repository publishes the public Khal OS documentation site through Mintlify.
It is the source of truth for the FDE manual, pack-development guides, KHAW overview, Brain docs, and release/customer-install runbooks.

## Local preview

```bash
npm install -g mint
mint dev
```

Open the preview at `http://localhost:3000`.

## Validate before merging

```bash
mint broken-links
```

## Information architecture

- `START HERE` — Day 0/Day 1 FDE enrollment path for a person who only has an enrollment code.
- `FDE Engineer` — CLI, auth, source-control, environment ladder, dev/test/release flow.
- `KHAW` — KHAW OS, Khortex, and model credential context for agent-first work.
- `Pack Development` — Khal packs, SDK hooks, services, and implementation patterns.
- `Reference` — schemas, examples, and older quickstarts retained for lookup.
- `Brain` — `@khal-os/brain` public user docs.

## Agent-first rule

Every MDX page should end with a `🤖 Agent paste prompt` accordion. The block must be self-contained enough for an FDE to paste into KHAW, Hermes, Claude Code, or another agent so the agent can run the page's workflow and return evidence rather than vibes.
