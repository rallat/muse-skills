# AGENTS.md

Operating notes for an AI agent using the skills in this repo. Distilled from the
working manual of the Muse instance these skills were snapshotted from — the
lessons that survived daily use.

## Using a skill

- Read the skill's `SKILL.md` fully before using it. It is the skill's contract:
  what it can do, how to invoke it, and what "done" looks like. Don't skim it.
- Resolve relative paths inside a skill against the skill's own directory, not
  your current working directory.
- When a skill wraps a connected service, run its status/auth check first and
  confirm the connection is healthy before promising an outcome. If it isn't
  connected, hand over the connect flow and stop — never commit to a result you
  can't produce.
- Pick the most specific skill for the task. If no skill fits, say so instead of
  improvising a connection flow, settings page, or pairing screen that doesn't
  exist.

## Trust and verification

- Never invent what you can't read. If a menu, photo, listing, or document can't
  be reliably read, mark the uncertainty explicitly instead of filling the gaps.
- Before sending a product or listing link to a user, open the URL yourself and
  confirm it resolves to the right item. Never send a link you haven't opened.
- Ground facts in tool output, not memory. A price, time, address, or booking
  reference someone will act on must come from a fresh tool result — or be
  flagged as unverified.

## Searching

- For camera and vintage-item hunts in Japan, search in Japanese first: Japanese
  queries (中古, model names in katakana/kanji) on Japanese site versions.
  English queries miss most listings.
- Prefer each shop's own on-site search over aggregators, and verify a shop's
  domain actually resolves before relying on it — dead domains rot fast.

## Editing state

- After editing a JSON state file, validate it immediately, e.g.
  `python3 -c "import json; json.load(open(path))"`. One malformed edit can
  silently corrupt the whole file.

## The binaries in `bin/`

- `bin/` holds the CLI tools the connector skills invoke. They are **Linux
  x86-64** ELF executables — they will not run on macOS or Windows.
- Skills call them as bare commands (expected on `PATH`) or as absolute
  `/opt/hatch/bin/<name>` paths. To reuse: add `bin/` to your `PATH`, symlink
  it to `/opt/hatch/bin`, or rewrite the paths.
- Each CLI manages its own auth (OAuth tokens, API keys). **No credentials ship
  in this repo** — re-authenticate every connector yourself before use.
- This snapshot is refreshed daily from the live environment, so `bin/` tracks
  the current tool versions.
