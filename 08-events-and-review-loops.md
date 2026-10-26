# 08 — Events & Review Loops

> Observability and human approval: what the system records, which events
> trigger a review, and where a human must approve before the flow continues.
> *Skeleton — fill for your context.*

## Event journal

The system appends events to `events/` (one journal per period or stream). An
event is a small, timestamped record of something the agent did or observed. See
`events/` for the sample format.

## Hooks

Points in the workflow (`05-workflows.md`) where an event fires and is appended
to `events/`. Hooks are runtime-agnostic names; the runtime wires them to real
triggers under `integrations/<runtime>/`.

| Hook | Fires when | Records |
|---|---|---|
| `on-plan` | a plan is approved | scope, risks, acceptance check |
| `pre-code` | implementation starts | which plan, which scope |
| `post-code` | a change is complete | files touched, self-check result |
| `on-review` | a review verdict lands | pass/fail + findings |
| `pre-deploy` | a deploy is about to run | target env, change ref |
| `post-deploy` | a deploy finishes | env, outcome, rollback note |
| `on-escalation` | a boundary/approval is hit | the question sent to the operator |

## Review loops

The loop that keeps quality without a human in every step:

1. `reviewer` checks a change and emits a verdict (`on-review`).
2. **Pass** → the change proceeds to Deploy.
3. **Fail** → the findings route back to `Code`; the `implementer` addresses them
   and produces a new change, which triggers a *fresh* review run (never the same
   run — reviewer isolation, `07-agent-roles.yml`).
4. Repeat until the review passes or the agent escalates (`on-escalation`) —
   e.g. the same finding recurs, or a fix would exceed scope.

Every pass through the loop is journaled, so the operator can see how many
rounds a change took and why.

## Human approval gates

Gates where the flow **stops for a human** (from `09-safety-and-boundaries.md`):

- Before any irreversible or destructive action.
- Before deploying to a production environment.

Operator-specific gates (worked example — edit for your context):

- Before sending anything user-facing (email, public post, message to a
  customer) — the draft is shown, sending waits for approval.
- Before any paid action or spend.
- Before touching data classified as sensitive/personal.
- When a review loop exceeds a set number of rounds without passing — stop and
  ask rather than grind.
