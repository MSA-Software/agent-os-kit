---
name: daily-review
description: >
  Use when the operator asks "what should I do today?", starts a work session,
  or wants a status pass over their projects. Reads the persistent context and
  project registry, then proposes a short, prioritized set of next actions plus
  the risks and questions worth surfacing. Runtime-agnostic.
---

# Daily Review

Procedural memory for a recurring stand-up the agent runs *with* the operator.
No runtime-specific calls — this reads kit files and produces text.

## When to use

- The operator opens a session with "what's next", "where are we", "plan my day".
- On a schedule, if the runtime supports it (see `integrations/<runtime>/`).

## Inputs (read, do not guess)

- `01-operator-profile.yml` — time budget, focus, communication preferences.
- `02-operating-principles.md` — values and decision defaults (obey them).
- `03-life-goals.md` if present — longer-horizon goals to align against.
- `04-project-registry.yml` — the authoritative list of active projects.
- `09-safety-and-boundaries.md` — hard limits (read first, overrides all).

Treat any blank field as UNKNOWN. Ask rather than invent.

## Procedure

1. Load the inputs above. If `04-project-registry.yml` has `projects: []`, stop
   and ask the operator to add at least one project — there is nothing to review.
2. For each `active` project, read its `next_action`, `blockers`, and
   `next_checkpoint`.
3. Rank next actions by the operator's priorities and the values in
   `02-operating-principles.md` (focus over breadth; reversibility over
   cleverness). Respect the weekly `time_per_week_hours` budget — do not propose
   more than fits.
4. Flag every project whose `next_checkpoint` is past or whose `blockers` are
   non-empty.
5. Draft 2–3 review questions the operator should answer to unblock or re-focus.

## Output

Respond in the operator's `preferred_language`, concise, in this shape:

```
Today (fits your N h budget):
1. <project> — <small next action>   (why: <one line>)
2. ...

Needs a decision:
- <project> — <blocker or stale checkpoint> → <question>

Nothing else is active. Anything to add to the registry?
```

## Notes

- This is a proposal, never an action. Do not execute irreversible steps here
  (see `02-operating-principles.md` and `09-safety-and-boundaries.md`).
- If you notice you ran this same reasoning more than a couple of times, that is
  a signal to distil a sharper skill — see `distill-skill`.
