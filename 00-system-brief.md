# 00 — System Brief

> One-page statement of what this AI operating system is, who it serves, and
> what "good" looks like. This is one of the first files your agent reads via
> `start.md` (right after the safety boundaries). The content below is a
> **worked example** — replace it with your own, keeping the same section shape.

## What this is

This is an **AI operating system**: a durable set of plain files — context,
principles, a project registry, skills, agent roles, and review loops — that
any model-agnostic agent runtime reads to plan, code, review, deploy, and run
the operator's work. It is not a chatbot transcript and not a single clever
prompt; it is a versioned system that outlives any one conversation or model.
Hand the folder to a capable agent and it should understand the operator well
enough to propose the right next actions.

## Who it serves

The **operator** — the individual or company whose goals, projects, and
constraints this system encodes. In this worked example the operator is a solo
technical founder running a small SaaS: limited hours, many competing
priorities, and a need for an agent that remembers context between sessions and
turns vague goals into concrete next steps. Replace this with your own operator
in `01-operator-profile.yml`.

## Runtime

This system is **runtime-agnostic**. It runs on any model-agnostic agent
runtime (e.g. Hermes, OpenClaw). Nothing in the numbered core files (`00`–`10`)
depends on a specific model, vendor, or tool. Runtime-specific setup —
credentials, memory backends, channels, scheduling — lives under
`integrations/<runtime>/` and never leaks into the core.

## What "good" looks like

- The operator can hand this folder to a fresh agent and get useful,
  context-aware next actions **without re-explaining themselves**.
- Every recurring task the operator does becomes a skill (`skills/`) instead of
  a repeated prompt.
- Work moves through an explicit loop — plan → code → review → deploy — with a
  human approval gate before anything irreversible.
- Adding a new runtime means adding one `integrations/<name>/` folder; the core
  files do not change.

## Non-goals

- **Not** a place to store secrets, credentials, or sensitive records — those
  stay in the runtime's secret store (see `09-safety-and-boundaries.md`).
- **Not** an autonomous decision-maker for irreversible actions — it proposes,
  the operator approves.
- **Not** tied to one model or vendor — any capability claim here must hold for
  a generic agent runtime, not a specific product.
