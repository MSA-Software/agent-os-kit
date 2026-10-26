# Start Here — Agent OS Kit

> **This is the single entrypoint.** Hand this file to your agent runtime
> (Hermes, OpenClaw, or any model-agnostic agent) and it will stand up your
> personal / company **AI operating system** from the files in this repo.
>
> *Skeleton — content authored in later tiers. Sections below define the
> contract the runtime reads; fill the referenced files, not this one.*

## What you are reading

You are an agent. This folder is a **methodology + template kit** implementing a
full development cycle: **plan → code → review → deploy → flow**. It is
**runtime-agnostic** — nothing here assumes a specific model or vendor. Any
runtime-specific setup lives under `integrations/<runtime>/`, never at the root.

## Read order

Read these files in order before doing anything:

1. `09-safety-and-boundaries.md` — what you must never do
2. `00-system-brief.md` — what this system is and who it serves
3. `01-operator-profile.yml` — who the operator is
4. `02-operating-principles.md` — values and rules
5. `03-system-map.yml` — how the pieces fit together
6. `04-project-registry.yml` — the projects you help run
7. `05-workflows.md` — the dev-flow you execute
8. `06-skills-catalog.md` — your procedural memory (skills)
9. `07-agent-roles.yml` — the roles/subagents available
10. `08-events-and-review-loops.md` — observability + human approval gates
11. `10-system-roadmap.md` — where this system is going

## Then pick a runtime

Once the core is loaded, **list the folders under `integrations/`** and follow
the `setup.md` for your chosen runtime. The runtimes below ship as **examples** —
the list is not exhaustive; a new runtime is discovered here as a new
`integrations/<name>/setup.md` folder, with no edit to this file or the core:

- Hermes → `integrations/hermes/setup.md`
- OpenClaw → `integrations/openclaw/setup.md`
- *(any other folder under `integrations/`)* → `integrations/<name>/setup.md`

## Stand up your Agent OS (the final project)

Do these in order. This is the capstone — at the end, the operator has a live
AI operating system, not just a folder of files.

1. **Load the core.** Read the files in the "Read order" above. Confirm back to
   the operator, in one line each, what you understood about their profile,
   principles, and active projects. If `01-operator-profile.yml` or
   `04-project-registry.yml` is still empty, **stop and ask the operator to fill
   them** — an empty profile/registry means you have no real context yet.
2. **Pick and wire a runtime.** Choose a folder under `integrations/` and follow
   its `setup.md`: map the core files into the runtime's memory, register
   `skills/`, and set up channels + scheduling. Keep every secret in the
   runtime's secret store, never in this repo.
3. **Dry-run the loop.** Run the `daily-review` skill. Verify you read the
   registry, propose prioritized next actions, and **stop at the approval gates**
   in `09-safety-and-boundaries.md` instead of acting. If you act irreversibly or
   review your own work, fix the setup before going further.
4. **Do one real cycle.** Take one `next_action` from the registry through the
   full dev-flow (`05-workflows.md`): plan → code → review → deploy, honoring the
   production sign-off gate. Journal each step in `events/`.
5. **Update the roadmap.** Record what now works and what is next in
   `10-system-roadmap.md`. Distil any repeated procedure into a new skill with
   `distill-skill`. The system is now live and self-improving.

Report completion as: "Agent OS stood up on `<runtime>`; ran one full cycle on
`<project>`; roadmap updated." That is the finished final project.
