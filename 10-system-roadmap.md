# 10 — System Roadmap

> Where this AI operating system is going. Living document the agent updates as
> the system grows. *Skeleton — fill for your context.*

The agent keeps this file current: after each real cycle (`05-workflows.md`),
move shipped items out of **Next** into **Now**, and promote **Later** items as
they come into focus. The entries below are a worked example — replace them with
your own.

## Now

- Core context layer in place (`00`–`03`): the agent reads the operator profile,
  principles, and system map on every run.
- Project registry (`04`) drives what is active and what the next action is.
- Dev-flow (`05`) runs plan → code → review → deploy with reviewer isolation and
  a production sign-off gate.
- Two portable skills shipped (`daily-review`, `distill-skill`); the skill
  factory can grow more.
- Two runtimes documented (`integrations/hermes/`, `integrations/openclaw/`).

## Next

- Fill the operator profile and project registry with real data (they ship
  intentionally empty).
- Distil the operator's 2–3 most repeated tasks into new skills.
- Wire one always-on schedule (e.g. `daily-review` every morning) on the chosen
  runtime.
- Add operator-specific approval gates to `08`/`09` as real edge cases appear.

## Later

- A second channel and/or a second runtime, added as a new `integrations/<name>/`
  folder with no change to the core.
- Richer event analytics over `events/` (trends, recurring blockers).
- Multi-operator or team variant of the profile/registry, if the operator grows
  from an individual into a company.

## Add a runtime

The system is pluggable. Adding a runtime means adding one
`integrations/<name>/` folder (with `setup.md` + config templates) — without
touching the numbered core files.
