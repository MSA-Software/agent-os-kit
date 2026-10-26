---
name: distill-skill
description: >
  Use after finishing a task that went well and is likely to recur — the agent
  turns that successful run into a new, reusable skills/<name>/SKILL.md. This is
  the skill-factory procedure: procedural memory grows itself. Runtime-agnostic.
---

# Distill Skill

The skill factory in one file. When a task succeeds and its shape will repeat,
capture the *procedure* — not the one-off answer — as a new skill.

## When to use

- The operator says "remember how you did that" / "make this repeatable".
- You just completed a multi-step task you expect to be asked for again.
- `daily-review` (or any skill) has you re-deriving the same steps repeatedly.

## When NOT to use

- One-off tasks unlikely to recur — a skill would be clutter.
- Anything requiring secrets or runtime-specific calls in the body (keep those
  in `integrations/<runtime>/`, not in a portable skill).

## Procedure

1. Restate the task you just did and confirm with the operator that it is worth
   keeping. If not, stop.
2. Extract the **generalizable procedure**: the ordered steps that would work
   next time with different inputs. Strip the specific data from this run.
3. Identify the trigger — the "when to use" phrasing a future agent would match.
4. Write `skills/<kebab-name>/SKILL.md` using this repo's skill shape:
   - YAML frontmatter: `name`, `description` (the description starts with
     "Use when …" so trigger-matching is obvious).
   - Sections: When to use / When NOT to use / Inputs / Procedure / Output / Notes.
   - **No runtime-specific calls** in the body — it must run on any runtime.
5. Add a row to `06-skills-catalog.md` (Skill / When to use / File).
6. Show the operator the new skill and the catalog entry for approval before
   treating it as active.

## Output

- A new `skills/<name>/SKILL.md`.
- A new row in `06-skills-catalog.md`.
- A one-line summary: "Added skill `<name>` — use it when <trigger>."

## Notes

- Keep skills small and single-purpose; if a skill tries to do two jobs, split it.
- A skill is agent-agnostic memory. Never bake a model name, API key, or channel
  into it — that belongs under `integrations/`.
