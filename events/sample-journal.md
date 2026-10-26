# Sample Event Journal

> Format reference for `events/`. One entry per event, newest last. *Skeleton —
> illustrative entries only.*

---

**2026-01-01T09:00:00Z** · `plan` · project: `example`
Scoped request into a 3-step plan. Awaiting approval gate.

**2026-01-01T09:15:00Z** · `review` · project: `example`
Reviewer role flagged 1 boundary concern; routed back to plan.

**2026-01-01T10:00:00Z** · `deploy` · project: `example`
Human approved. Change shipped to staging.

<!-- Entry template:
**<ISO-8601 timestamp>** · `<stage>` · project: `<id>`
<one-line description of what happened>
-->
