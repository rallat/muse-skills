# muse-skills

A snapshot of Muse's skill definitions, connector docs, and CLI tools —
synced daily. Built so other AI agents can reuse them.

> **Layout change (2026-10-15):** the platform restructured its skills. The old
> per-service skill directories (87 of them: `gmail/`, `spotify/`, …) were
> retired; skills are now workflow-oriented and the per-service knowledge lives
> in connector docs under `connectors/`. The retired tree is preserved in this
> repo's git history (commits up to 2026-10-14).

## What's here

- **53 skills** — each directory holds `SKILL.md` (the skill's instructions)
  plus supporting references and scripts. These are workflow skills
  (documents, email-management, flight-booking, shopping, …).
- **`connectors/`** — 63 connector docs. Each `connectors/<name>/CONNECTOR.md`
  documents a service integration: capabilities, how to connect, and operating
  instructions. **No actual credentials** are included anywhere.
- **`bin/`** — 66 CLI tools the connectors invoke (Linux x86-64 ELF). Most
  connectors are thin wrappers over these; without `bin/` you get the docs but
  not the working tools.
- **`AGENTS.md`** — operating notes for agents: how to use the skills, trust and
  verification rules, search conventions. Read this before wiring things in.

## Reusing the skills and connectors (for LLMs)

1. For a workflow, pick a skill directory and read its `SKILL.md` — that's the
   contract: what it does, how to invoke it, what "done" looks like.
2. For a service, read `connectors/<name>/CONNECTOR.md` for its capabilities
   and connection flow.
3. Put `bin/` on your `PATH` (Linux x86-64 only), or rewrite the
   `/opt/hatch/bin/<name>` absolute paths the docs reference.
4. Re-authenticate each connector yourself — the docs describe the auth flow
   and scopes; run the connector's status check and complete its connect flow
   before promising results.
5. The markdown ports directly to other harnesses; manifests and config files
   may need reshaping to fit the target format.

## Freshness and safety

- Synced daily from the live environment — each day's delta lands as one commit,
  so consumers can track changes over time.
- Every delta is secrets-scanned before push (private-key blocks, known token
  formats, high-entropy secret-like values). A finding blocks the commit and the
  push — nothing sensitive leaves the source machine.
- Excluded: platform internals from `/opt/hatch/bin` (the `hatch*` runtime,
  `spawnd`, browser infra, etc.) — only the CLIs the connectors invoke are
  vendored.

## Layout

```
<skill-name>/
  SKILL.md                   # skill instructions (the contract)
  references/                # extra docs
  scripts/                   # helper scripts
connectors/<name>/
  CONNECTOR.md               # service capabilities + connection flow
bin/                         # 66 connector CLI tools (Linux x86-64)
AGENTS.md                    # operating notes for agents
```
