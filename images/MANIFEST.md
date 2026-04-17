# Screenshot capture manifest — Khal-OS docs

Every `<Frame>` in the Khal-OS docs site renders a placeholder until the image it references is captured and added under `/images/`. This manifest lists every shot the docs currently point at, where to capture it from (`nmstx.khal.ai` unless noted), and a crisp caption brief.

Update this file in lock-step when you add / remove `<Frame>` calls — if an operator opens a random `.mdx` and sees a placeholder image that isn't listed here, that's a drift bug.

## How to use this file

1. Log into `nmstx.khal.ai` with a test account that has at least one pack installed.
2. For each entry, set up the scene described in "Setup", capture the region described in "Shot", and save at the path listed under "Save as".
3. PNG, 2x pixel density where possible. Tight crops — no browser chrome unless the shot is about the shell itself.
4. Open a PR targeting `dev` titled `docs(khal-os): capture screenshots — <batch>` and attach the captures.

## Capture list (as of 2026-04-17 sprint)

### Home + what-is

| Save as | Used in | Setup | Shot | Caption brief |
|---|---|---|---|---|
| `images/hero-desktop.png` | `index.mdx` (Home hero) | Log in; open 3–4 packs (Terminal, Settings, Notes, NATS Viewer). Arrange windows so each is visible. | Whole desktop region; 16:9. No browser chrome. | "The khal-os desktop shell with a handful of packs running" |
| `images/architecture.png` | `what-is-khal-os.mdx` | N/A — hand-drawn or Figma diagram | Diagram, not screenshot. Show: frontend package (React) + optional Bun service + Helm chart + khal-os shell consuming the pack. | "Pack architecture — frontend package + optional Bun service + Helm chart, consumed by the khal-os shell" |

### Getting started

| Save as | Used in | Setup | Shot |
|---|---|---|---|
| `images/your-first-pack-preview.png` | `getting-started/your-first-pack.mdx` | After scaffolding and installing the hello-world pack locally | The scaffolded pack's React component rendered inside a single shell window |

### Patterns

| Save as | Used in | Setup | Shot |
|---|---|---|---|
| `images/pack-settings-preview.png` | `patterns/frontend-only-pack.mdx` | Install `pack-settings` into `nmstx.khal.ai` | `pack-settings` window open with the user profile rendered |
| `images/pack-terminal-preview.png` | `patterns/full-stack-pack.mdx` | Install `pack-terminal`; open it; type a few commands (`ls`, `uname -a`) | `pack-terminal` window with an active PTY session showing output |

### Dev + testing

| Save as | Used in | Setup | Shot |
|---|---|---|---|
| `images/khal-dev-qa.png` | `dev/local-dev-and-testing.mdx` | From a terminal outside the shell, run `khal-dev qa health` against a local pack | The `khal-dev qa` CLI output — PASS/FAIL lines, with some variety if possible |

### Brain (pre-existing section, retained)

The `brain/*` pages likely also carry placeholder images. Audit separately after the FDE-manual captures are complete.

## Image quality bar

- 2x pixel density (Retina).
- PNG, no PNG optimization loss (`pngcrush -brute` OK).
- No user-identifying data in screenshots. Use a test account with a placeholder display name and generic org.
- Crop to the pack window plus a thin shell border — do not include the whole viewport unless the shot is about the desktop shell itself (see hero).

## Not-yet-captured alt text

Every `<Frame>` currently uses the `alt` attribute as the caption brief for this file. When you capture an image, keep the alt text describing the shot (good for accessibility and SEO); the image itself renders in the placeholder slot.
