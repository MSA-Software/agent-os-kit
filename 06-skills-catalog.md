# 06 — Skills Catalog

> Skills are **procedural memory** — reusable procedures the agent runs, and
> the seed of a self-improving skill factory. Each skill is a `SKILL.md` file in
> `skills/`, portable across runtimes (Hermes, OpenClaw). *Skeleton — the
> catalog is a table; individual skills live in `skills/`.*

## How skills work

- A skill is a single `skills/<name>/SKILL.md` file describing when to use it and
  the procedure to follow.
- Skills are **agent-agnostic**: no runtime-specific calls in the skill body.
- Successful task patterns are distilled into new skills (the skill factory).

## Catalog

| Skill | When to use | File |
|-------|-------------|------|
| `daily-review` | Operator asks "what's next" / starts a work session — reads context + registry, proposes prioritized next actions and risks | `skills/daily-review/SKILL.md` |
| `distill-skill` | A task succeeded and will recur — turn it into a new reusable skill (the skill factory) | `skills/distill-skill/SKILL.md` |

These two ship as worked examples. Add your own with `distill-skill`, then list
them here (one row per skill).
