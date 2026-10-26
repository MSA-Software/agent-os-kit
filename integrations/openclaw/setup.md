# OpenClaw × the Kit — Setup

> How to feed this kit into **OpenClaw**. OpenClaw is treated here purely as a
> model-agnostic, self-hosted agent runtime with the capabilities the kit relies
> on: markdown + SQLite persistent memory, multi-agent routing, custom skills,
> and messaging channels. **Verify vendor specifics (config schema, CLI names,
> repo layout, version) against your own OpenClaw install before publishing or
> running** — the templates below are illustrative shapes, not a guaranteed
> schema.

## Prerequisites

- A self-hosted OpenClaw instance you can run and point at a local folder (this
  kit).
- A model/provider configured in OpenClaw (the kit is model-agnostic — any
  capable model works).
- Somewhere for OpenClaw's SQLite database and markdown memory to live (a data
  dir you control; keep it out of this repo).
- Credentials for at least one channel you want, stored in OpenClaw's own secret
  mechanism — **never** committed here (see `../../09-safety-and-boundaries.md`).
- Read `../../start.md` and `../../09-safety-and-boundaries.md` first; safety
  rules override anything a runtime setup implies.

## Steps

1. **Point OpenClaw at this repo and load `start.md`.** `start.md` is the single
   entrypoint; it makes the agent read `09-safety-and-boundaries.md` first, then
   the numbered core files, then enumerate `integrations/`.
2. **Map the numbered core files into OpenClaw's markdown + SQLite memory.** Load
   `00`–`10` as always-on context (the "context layer" in `03-system-map.yml`).
   The markdown files stay the source of truth; SQLite is an index over them, not
   a replacement.
3. **Register the `skills/` catalog as OpenClaw custom skills.** Each
   `skills/<name>/SKILL.md` maps to an OpenClaw skill. New skills distilled with
   `distill-skill` are written back as portable `SKILL.md` files so they also run
   on other runtimes.
4. **Configure multi-agent routing and channels.** Map the kit's roles
   (`07-agent-roles.yml`) onto OpenClaw's multi-agent routing so planner /
   implementer / reviewer / deployer stay separate — reviewer isolation is what
   makes the review loop in `../../08-events-and-review-loops.md` meaningful.
   Wire at least one messaging channel.
5. **Verify the loop.** Ask the agent to run `daily-review`; confirm it reads the
   registry, proposes next actions, and stops at approval gates instead of
   acting. If a single agent both writes and reviews, fix the routing in step 4.

## Config templates (this folder)

Runtime-specific config for OpenClaw belongs **here**, never in the kit root.
The shapes below are illustrative — confirm the real keys against your OpenClaw
version:

```yaml
# memory.example.yml — markdown + SQLite memory over the kit
memory:
  markdown_source: "../../"        # the kit root (source of truth)
  sqlite_path: "~/.openclaw/agent-os-kit.db"   # your data dir, NOT this repo
  always_on:
    - 09-safety-and-boundaries.md  # first, overrides everything
    - 00-system-brief.md
    - 01-operator-profile.yml
    - 02-operating-principles.md
    - 03-system-map.yml
```

```yaml
# routing.example.yml — map kit roles onto multi-agent routing
agents:
  - role: planner       # read-only
  - role: implementer   # writes within approved scope
  - role: reviewer      # isolated — never the same run as implementer
  - role: deployer      # gated + human sign-off
skills_dir: "../../skills/"
```

```yaml
# channels.example.yml — reference only; real secrets live in OpenClaw's store
channels:
  - type: telegram
    credentials_ref: "openclaw-secret://telegram"   # NOT a literal token
```

Keep credentials as references (`*-secret://…`) pointing at OpenClaw's secret
store. Never write a literal token into any file in this repo.
