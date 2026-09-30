# muse-skills-connectors

Snapshot of Muse's skill definitions and connector configurations, taken 2026-09-30.

## What's here

- **87 skills** — each directory holds a `SKILL.md` (the skill's instructions) plus supporting files (references, scripts, templates).
- **Connector configs** — skills backed by a connected service also carry `manifest.yaml` (connector capabilities, permissions) and `connector_auth_config.yaml` (auth schema; no actual credentials).
- Some skills are symlinks to a sibling skill's files (e.g. `facebook/` → `facebook-cli/`); git preserves these as symlinks.

## What's excluded

- Vendored `node_modules` trees under `spaces/ts-runtime/dist/` (build tooling, reproducible from `bun.lock` / `deps-manifest.json`).
- Tool binaries (`/opt/hatch/bin`, ~1.4GB of executables) — not source, not pushed.
- No secrets: scanned for API keys, tokens, and private keys before snapshotting; none found.

## Layout

```
<skill-name>/
  SKILL.md                  # skill instructions
  manifest.yaml             # connector manifest (when applicable)
  connector_auth_config.yaml# connector auth schema (when applicable)
  references/               # extra docs
  scripts/                  # helper scripts
```
