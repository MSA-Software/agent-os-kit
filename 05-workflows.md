# 05 — Workflows

> The ARIS-style development flow the agent executes: **plan → code → review →
> deploy → flow**. Runtime-agnostic. Each stage names the role that runs it
> (`07-agent-roles.yml`) and the gate that must pass before the next stage.

## Overview

Every unit of work moves through the same stages. Each stage has an entry
condition, an action, and an exit gate (see `08-events-and-review-loops.md` for
where human approval is required). No stage may skip its exit gate — a failed
gate routes the work back, it does not pass through.

## Stages

### 1. Plan
- **Role:** `planner` (read-only).
- **Entry:** a request, or a `next_action` pulled from `04-project-registry.yml`.
- **Action:** scope the work into a short, ordered plan — files/systems touched,
  risks, and the acceptance check. Obey the values in `02-operating-principles.md`.
- **Exit gate:** plan is concrete and within `09-safety-and-boundaries.md` scope.
  Anything irreversible or out of scope stops here for operator approval.

### 2. Code
- **Role:** `implementer` (write within approved scope).
- **Entry:** an approved plan.
- **Action:** implement the smallest coherent change that satisfies the plan;
  run build/tests in a non-production environment.
- **Exit gate:** change is complete and self-checks pass. The implementer never
  reviews its own work — it hands off to a separate reviewer run.

### 3. Review
- **Role:** `reviewer` (isolated, read-only — a *different* run than the coder).
- **Entry:** a completed change.
- **Action:** check the change against the plan's acceptance criteria, the
  operating principles, and the safety boundaries. Findings are logged, not
  silently fixed.
- **Exit gate:** review passes → proceed to Deploy. Review fails → back to Code
  with the findings (see the review loop in `08`).

### 4. Deploy
- **Role:** `deployer` (deploy capability only).
- **Entry:** a passing review.
- **Action:** ship the verified change to its target environment.
- **Exit gate:** dev/staging may proceed automatically; **production always
  stops for human sign-off** (`09-safety-and-boundaries.md`). Record the deploy
  in the event journal.

### 5. Flow
- **Role:** any, on a schedule the runtime provides.
- **Entry:** the system is live and idle between explicit requests.
- **Action:** run recurring skills (e.g. `daily-review`), watch for events worth
  surfacing, and propose the next `next_action`. Never act irreversibly here.
- **Exit gate:** anything actionable re-enters at Plan.

## Handoffs

Each stage passes a small, explicit artifact to the next — never hidden state:

| From → To | Handoff |
|---|---|
| Plan → Code | the approved plan (scope, steps, acceptance check) |
| Code → Review | the change + which acceptance criteria it claims to meet |
| Review → Deploy | a pass verdict tied to the reviewed change |
| Review → Code | findings to fix (on failure) |
| Deploy → Flow | a journal entry recording what shipped where |
| Flow → Plan | a proposed next action |
