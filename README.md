# muse-skills

A snapshot of Muse's skill definitions, connector configurations, and CLI
tools — 87 skills, synced daily. Built so other AI agents can reuse them.

## What's here

- **87 skills** — each directory holds `SKILL.md` (the skill's instructions).
  Service-backed skills also carry `manifest.yaml` (capabilities, permissions)
  and `connector_auth_config.yaml` (auth schema — **no actual credentials**),
  plus supporting references and scripts.
- **`bin/`** — 66 CLI tools the skills invoke (Linux x86-64 ELF). Most connector
  skills are thin wrappers over these; without `bin/` you get the docs but not
  the working tools.
- **`AGENTS.md`** — operating notes for agents: how to use the skills, trust and
  verification rules, search conventions. Read this before wiring the skills in.

Some skills are symlinks to a sibling skill's files (e.g. `facebook/` →
`facebook-cli/`); git preserves these as symlinks.

## Reusing the skills (for LLMs)

1. Pick a skill directory and read its `SKILL.md` — that's the contract: what it
   does, how to invoke it, what "done" looks like.
2. Put `bin/` on your `PATH` (Linux x86-64 only), or rewrite the
   `/opt/hatch/bin/<name>` absolute paths the skills reference.
3. Re-authenticate each connector yourself. `connector_auth_config.yaml`
   documents the OAuth scopes and token placement; run the skill's status/auth
   check and complete its connect flow before promising results.
4. This layout is Muse's skill convention (`SKILL.md` + `manifest.yaml`). The
   markdown ports directly to other harnesses; manifests may need reshaping to
   fit the target format.

## Freshness and safety

- Synced daily from the live environment — each day's delta lands as one commit,
  so consumers can track skill and tool changes over time.
- Every delta is secrets-scanned before push (private-key blocks, known token
  formats, high-entropy secret-like values). A finding blocks the commit and the
  push — nothing sensitive leaves the source machine.
- Excluded: vendored `node_modules` trees under `spaces/ts-runtime/dist/`
  (build tooling, reproducible from `bun.lock`), and platform internals from
  `/opt/hatch/bin` (the `hatch*` runtime, `spawnd`, browser infra, etc.) —
  only the CLIs the skills actually invoke are vendored.

## Layout

```
<skill-name>/
  SKILL.md                   # skill instructions (the contract)
  manifest.yaml              # connector manifest (when applicable)
  connector_auth_config.yaml # connector auth schema (when applicable)
  references/                # extra docs
  scripts/                   # helper scripts
bin/                         # 66 skill CLI tools (Linux x86-64)
AGENTS.md                    # operating notes for agents
```
