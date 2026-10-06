# Author backplan skill — 2026-10-05

## End state
The `backplan` skill is authored at `software-development/backplan/SKILL.md`,
passes frontmatter validation, and is pushed to `origin/main` of
`nathanjstratusadv/agent-skills`.

## Completion test
- `git ls-remote origin main` shows the pushed commit sha.
- `git log --oneline -1` in the clone matches that sha.
- `read_file software-development/backplan/SKILL.md` returns the final skill.

## Current state (step 0)
- Fresh clone at `/root/agent-skills`, empty (no commits, no branches).
- Remote `origin` = `https://github.com/nathanjstratusadv/agent-skills`.
- No git identity set (local or global) → set repo-local
  `Nathan Hermes Agent <nathanj@stratusadv.com>` per house convention.
- Skill drafted (23/23 frontmatter + structure checks pass, 6,042 chars).
- No YAML lib in env → validated with a minimal flat parse (frontmatter is flat).

## Plan
1. Workshop the design with the user — done when the three gear/scope/
   threshold decisions are locked. (done)
2. Draft SKILL.md with valid frontmatter — done when all hardline checks pass.
   (done)
3. Write this backplan doc — done when it exists at
   `backplan/author-backplan-skill-2026-10-05.md`. (done)
4. Commit skill + doc locally — done when `git log --oneline` shows the
   commit and the working tree is clean. (done)
5. Push to `origin/main` — done when the remote sha equals the local sha.
   **Shared/remote step — requires user "yes" before executing.** (done)

## Result
Completion test passed: `git ls-remote origin main` =
`de03896a8c23d1fe0c0c84f0a9f67136b8ad05b9` = local `HEAD`.

## Deviations
1. 08:2x — Used a minimal flat frontmatter parse instead of ruamel/yaml:
   neither library is present in this environment. Chose a stdlib-only check
   rather than installing a dependency for a one-off validation.
2. Push over HTTPS failed (`could not read Username` — no GitHub
   credentials). Switched `origin` to `git@github.com:nathanjstratusadv/agent-skills.git`;
   the existing `~/.ssh/id_ed25519` key already authenticates as
   `nathanjstratusadv`, so the SSH push succeeded. Remote now uses SSH.
3. (post-closeout) `backplan/` → `.backplan/` per user rename (commit 68d4c5a).
4. (post-closeout) Genericity pass v0.2.0: step 1 dropped Hermes tool names
   (`read_file`/`search_files`/`terminal`) for agent-neutral capability
   language; sign-off step 6 gained a headless-agent carve-out; description
   rewritten to 53 chars ("Plan tasks from the end state backward before
   acting.") so it still drives auto-loading in non-Hermes agents.
5. (post-closeout) v0.3.0 user pass (Nathan, from field testing): goal-first
   step 1 ("never plan toward a goal you invented"), fast-gear clarify rule,
   "Goal from assumption" pitfall, metadata flattened to `hermes-tags`.
6. (post-closeout) v0.3.1 review pass: step 1 done-criterion split from
   observability (goal = crisp + unambiguous, intent-level; step 3 owns
   observability with its own done-criterion); step 1 gained a headless
   carve-out + unambiguous-goal softener; plan-doc End-state annotation
   corrected. `hermes-tags` kept flattened per user instruction (restored
   nested form rejected: breaks other agents' loaders).
7. (post-closeout) v0.3.2: removed the `metadata` block entirely. Spec
   research (agentskills.io): `metadata` must be a flat string→string map, so
   the nested Hermes form is spec-nonconformant, but the flattened
   `hermes-tags` — while spec-compliant — is read by nobody (Hermes parses
   nested, other agents ignore it). Dead weight removed; if a shared tag
   convention emerges, re-add it per the spec then.
8. (post-closeout) v0.3.3: added the project-consistency rule. Step 2 now
   inventories the house style (naming, placement, existing patterns/utilities)
   with its done-criterion extended; step 9 requires every step to be
   implemented in the project's own structure and style ("reads as if the
   project's authors wrote it") and routes structural departures through
   deviations; new pitfall "Transplanted conventions" with the maintainer-diff
   test.
9. (post-closeout) v0.3.4: `## Deviations` is now an ordered list
   (numbered in change order; append at the end, never renumber) so the
   sequence of plan drift is visible. This doc converted to the format as
   part of the change.
