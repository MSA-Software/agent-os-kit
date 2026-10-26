# Agent OS Kit

A **model-agnostic** methodology + template kit for building a personal or
company **AI operating system**. You clone it, fill in the numbered files, and
hand `start.md` to your agent runtime — **one entrypoint → the system stands up.**

> *Skeleton scaffold (Tier 1). File structure and contracts are in place;
> the durable content is authored in later tiers.*

## Idea

The kit is the **70% agent-ready artifact** of the *AI Operating System Kit*
course: durable `.md` / `.yml` files an agent reads to plan, code, review,
deploy, and run your work. The runtime (Hermes, OpenClaw, …) is **pluggable** —
it *consumes* what this kit offers.

## Layout

```
agent-os-kit/
  start.md                    # entrypoint you hand to your agent
  README.md                   # this file
  00-system-brief.md          # what the system is, who it serves
  01-operator-profile.yml     # who the operator is
  02-operating-principles.md  # values and rules
  03-system-map.yml           # how components fit together
  04-project-registry.yml     # registry of projects (not memory chaos)
  05-workflows.md             # dev-flow: plan → code → review → deploy
  06-skills-catalog.md        # skills as procedural memory
  07-agent-roles.yml          # subagents, reviewers, boundaries
  08-events-and-review-loops.md  # events, hooks, human approval gates
  09-safety-and-boundaries.md    # safety gates
  10-system-roadmap.md           # where this system is going
  skills/                     # agent-agnostic SKILL.md files
  integrations/
    hermes/                   # Hermes setup.md + config templates
    openclaw/                 # OpenClaw setup.md + config templates
  scripts/                    # python helpers
  events/                     # sample event journal
```

## Design rules

- **Runtime-agnostic core.** The numbered files `00`–`10` are plain prose/YAML.
  No dotfiles, no Claude-specific defaults.
- **Runtime config is scoped.** Anything specific to a runtime (memory config,
  gateways, channels, cron) lives under `integrations/<runtime>/` — never at the
  root.
- **Pluggable integrations.** Adding a new runtime = one new section in the
  course + one new `integrations/<name>/` folder, without touching the core.

## Usage

1. Clone the repo.
2. Fill in the numbered files for your context.
3. Hand `start.md` to your agent.
4. Follow `integrations/<runtime>/setup.md` for your chosen runtime.
