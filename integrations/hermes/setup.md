# Hermes × the Kit — Setup

> How to feed this kit into **Hermes Agent**. Hermes is treated here purely as a
> model-agnostic agent runtime with four capabilities the kit relies on:
> persistent memory, a skill factory, messaging gateways, and cron scheduling.
> **Verify vendor specifics (config schema, CLI names, version) against your own
> Hermes install before publishing or running** — the templates below are
> illustrative shapes, not a guaranteed schema.

## Prerequisites

- A working Hermes install you can point at a local folder (this kit).
- A model/provider already configured in Hermes (the kit is model-agnostic — any
  capable model works).
- Credentials for at least one messaging gateway you want (kept in Hermes' secret
  store, **never** committed to this kit — see `../../09-safety-and-boundaries.md`).
- Read `../../start.md` and `../../09-safety-and-boundaries.md` first; safety
  rules override anything a runtime setup implies.

## Steps

1. **Point Hermes at this repo and load `start.md`.** `start.md` is the single
   entrypoint; it tells the agent to read `09-safety-and-boundaries.md` first,
   then the rest of the numbered core files, then enumerate `integrations/`.
2. **Map the numbered core files into Hermes' persistent memory.** Load
   `00`–`10` as always-on context (the "context layer" in `03-system-map.yml`).
   Keep them as the source of truth — Hermes' memory mirrors the files, it does
   not replace them.
3. **Register the `skills/` catalog with Hermes' skill factory.** Each
   `skills/<name>/SKILL.md` becomes a Hermes skill; new skills the factory
   distils should be written back as `SKILL.md` files (use `distill-skill`) so
   they stay portable to other runtimes.
4. **Configure gateways and cron.** Wire the operator's chosen channel(s) and a
   schedule for recurring skills (e.g. run `daily-review` each morning). The
   `flow` stage in `../../05-workflows.md` is what cron drives.
5. **Verify the loop.** Ask the agent to run `daily-review`; confirm it reads the
   registry, proposes next actions, and stops at approval gates rather than
   acting. If it acts irreversibly, stop and re-check step 1–2.

## Config templates (this folder)

Runtime-specific config for Hermes belongs **here**, never in the kit root. The
shapes below are illustrative — confirm the real keys against your Hermes
version:

```yaml
# memory.example.yml — map the always-on context layer into Hermes memory
memory:
  source_dir: "../../"          # the kit root
  always_on:
    - 09-safety-and-boundaries.md   # first, overrides everything
    - 00-system-brief.md
    - 01-operator-profile.yml
    - 02-operating-principles.md
    - 03-system-map.yml
```

```yaml
# skills.example.yml — register the portable skill catalog
skills:
  catalog: "../../06-skills-catalog.md"
  dir: "../../skills/"
  write_new_as_skill_md: true    # keep distilled skills portable
```

```yaml
# gateways.example.yml — reference only; real secrets live in Hermes' store
gateways:
  - type: telegram
    credentials_ref: "hermes-secret://telegram"   # NOT a literal token
cron:
  - skill: daily-review
    schedule: "0 8 * * *"        # every day 08:00, operator timezone
```

Keep credentials as references (`*-secret://…`) pointing at Hermes' secret store.
Never write a literal token into any file in this repo.
