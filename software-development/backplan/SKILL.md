---
name: backplan
description: "Use when starting any task: plan from the end state back."
version: 0.1.0
author: Nathan (nathanjstratusadv), Hermes Agent
license: MIT
platforms: [linux, macos, windows]
metadata:
  hermes:
    tags: [planning, workflow, execution, backplan]
---

# Backplan (Working-Backwards Task Planning)

Every task starts at its end. Before acting, derive the end state and its
completion test, walk backwards from it to the verified current state (step 0),
then execute the resulting chain forward. Full plans are documented in a
per-project `.backplan/` folder and committed alongside the work they planned.

## When to Use

- On every request, in one of two gears (choose the gear first, always):
  - **Fast gear** — single-step, local, nothing shared or remote is touched.
    One line: end state, completion test, next move. No doc.
  - **Full gear** — multi-step, or touches shared/remote systems. Full
    backwards-decomposition, sign-off, plan doc in `.backplan/`.
- Don't use for: pure conversation or questions — the answer *is* the end
  state, there is nothing to plan. Also not when the user explicitly says
  "just do it, no planning" — comply, but re-plan honestly if the job goes
  sideways.

## Choosing the Gear

Full gear when ANY of these holds:

- The work is multi-step (three or more distinct moves).
- It touches shared or remote state: `git push`, SSH to cluster nodes, deployed
  services, anything another person or machine can observe.
- It is expensive to reverse: data mutation, long-running jobs, force-pushes,
  reboots, destructive deletes.

Otherwise fast gear: state the end state, completion test, and next move in
one or two lines, then work. Same discipline, ceremony dropped.

## Procedure (full gear)

1. **Inventory step 0 — the current state.** Inspect, don't assume: read the
   relevant files (`read_file`, `search_files`), check running processes and
   versions (`terminal`), note the environment.
   *Done when:* a short list of facts you *verified*, not remembered.
2. **State the end state.** One sentence, observable from outside: a file that
   exists, a service answering, a test passing, a report with cited sources.
   Never "improved", "cleaned up", "works better".
3. **Write the completion test.** The concrete check that proves the end state
   holds: a command to run, a URL to hit, a file to read, output to see.
   *Done when:* the test is runnable *as written*, right now if the end state
   were true.
4. **Walk backwards.** Ask: "what must be true immediately before the end
   state?" — that is step N-1. Repeat until the chain hangs on step 0. Each
   step must depend on the one before it; if a step needs nothing from its
   predecessor it is parallel, not sequential — label it so.
   *Done when:* step 1 follows directly from verified step-0 facts.
5. **Reverse into a forward plan.** Each step: the action + a checkable
   completion criterion.
6. **Present for sign-off.** End state, completion test, plan (one line per
   step), and the riskiest step. Wait for a "yes" before executing. If the
   user redirects, re-derive only the part the change touches.
7. **Write the plan doc** (format below) to
   `<project-root>/.backplan/<short-goal>-<YYYY-MM-DD>.md` — *before* the first
   execution step.
8. **Execute, re-anchoring at each step.** Before each step ask: does this pull
   the end state closer, and is it still reachable? If a step's precondition
   turns out false, that is a deviation: record it, re-derive from that step
   forward, then continue. Never push through a false precondition silently.
9. **Close the loop.** Run the completion test and capture its real output.
   Report done only on that evidence. Commit the plan doc with the work.
   If the test cannot be run, report the blocker honestly — a pass that was
   never observed is worse than an admitted blocker.

## Plan Document

Location: `<project-root>/.backplan/` (create if missing). Name:
`<short-goal-description>-<YYYY-MM-DD>.md`, kebab-case, 3–6 words
(`serve-fp8-on-pve2-2026-10-05.md`). Same goal, same day: append `-2`, `-3`.

```markdown
# <short goal> — <YYYY-MM-DD>

## End state
<one observable sentence>

## Completion test
<concrete, runnable check(s)>

## Current state (step 0)
<verified facts: files, services, versions, environment>

## Plan
1. <step> — done when <criterion>
2. <step> — done when <criterion>

## Deviations
none yet
```

Living-doc rules:

- Update the doc the *moment* the plan shifts — never "later".
- `## Deviations` is append-only: one line per change,
  `- <HH:MM> — <what changed> — <why>`. Keep the original step visible
  (strike it through) so deviations have something to deviate *from*.
- On completion the doc shows the path actually taken, not the original dream.
- Commit the doc with the feature it planned.

## Pitfalls

- **Unobservable end state.** "The code is cleaner" ✗ — "clippy reports zero
  warnings on the touched modules" ✓. If you cannot point at something to
  inspect, it is not an end state.
- **Completion test that is a step, not a state.** "Wrote the test" ✗ — "the
  suite passes including the new case" ✓.
- **Step 0 from memory.** A stale assumption poisons every step that hangs on
  it. Inspect.
- **Dropping the gear to save time.** Full gear exists because shared/remote
  work is expensive to reverse; the sign-off is cheap insurance, not ceremony.
- **Silent plan drift.** A plan that changed without a deviations entry is a
  decision made by accident.
- **Fabricated completion.** If the completion test could not be run, say so.
- **Backwards-planning a question.** "What's 2+2" has no plan; fast gear only,
  or none at all.

## Verification

- Full gear: the doc exists at `.backplan/<short-goal>-<date>.md` and is
  committed; its completion test was run and its real output is quoted in the
  final report.
- Every mid-execution plan change has a matching deviations entry with a why.
- The final report states: end state, completion test + observed result,
  deviations (if any).
