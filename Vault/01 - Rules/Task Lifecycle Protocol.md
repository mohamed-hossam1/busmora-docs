# Task Lifecycle Protocol

The engineer drives; the AI challenges assumptions and executes the agreed approach. Use this protocol for Jira implementation work. Scale the conversation and evidence to the decision; do not manufacture paperwork or approval rounds for decisions already settled by the engineer.

## 1. Intake and shared understanding

Read the ticket, its acceptance criteria, and the engineer's initial technical hypothesis. Keep Jira **To Do** during intake, grilling, research, and benchmarking (or retain the project's equivalent pre-start status). For work already underway, inspect its actual state before changing status. A completed ticket needs an explicit follow-up or reopening decision.

Inspect relevant code, tests, dependencies, and linked prior task records. Ask about unresolved product decisions, failure states, permissions, and trade-offs; look up facts yourself. Recommend a direction with a concrete example. Use the existing grilling skill when a deeper discussion is useful; do not require an exhaustive interview for every ticket.

Challenge every tool, library, or algorithm prescribed by the ticket: why it fits, what alternatives exist, and what evidence supports it. Distinguish an architectural invariant from a proposed implementation choice. Surface conflicts with the engineer before changing an invariant.

## 2. Constraints before independent research

Before searching for candidates, read the relevant sections of `busmora-docs/core/`, the team rules here, and the target repository's instructions and runtime configuration. Record the constraints and source links in `Task.md` or, when needed, `research.md`.

Apply the project's architectural invariants, boundaries, and system constraints. Reject incompatible candidates before benchmarking them. Conflicts between the brief, code, and specifications must be surfaced, not silently resolved.

For a tool or approach choice, the engineer and AI search independently in parallel, then compare their candidate sets. Invite the engineer's findings while doing the AI's research. Preserve attribution; never invent the engineer's findings or label AI research as two independent searches. Mark missing input pending and agree the selection with the engineer; an explicit waiver can be recorded. Routine work with no meaningful selection does not require a research exercise.

Use current primary sources, record versions and source dates, and distinguish documented claims from measured results. Include existing stack options and a simple baseline when relevant. Keep brief findings in `Task.md`; create `research.md` only when the detail helps the decision.

## 3. Measure claims in an isolated sandbox

When selection depends on measurable claims such as latency, throughput, resource consumption, or domain-specific accuracy, run an actual comparison. A README claim or a weighted opinion table is not a benchmark. Agree the criteria and pass thresholds before running it.

Use `scratch/benchmarks/<JIRA-KEY>/` in the repository running the experiment, with isolated dependencies and configuration. Confirm the path is git-ignored there before creating experiment files; the docs repository's ignore rule does not cover sibling repositories. Do not alter production manifests or lockfiles for an experiment.

Use representative workloads and realistic, non-sensitive data samples. Record versions, environment, data provenance, commands, repetitions, warm-up conditions, metrics, failures, and limitations. Compare candidates under the same conditions. Label estimates, vendor claims, and measurements separately.

Save useful conclusions in `02 - Tasks/<JIRA-KEY>/benchmark.md`. Preserve the minimal sanitized harness, fixture or data retrieval recipe, and raw results needed to reproduce the decision in optional `evidence/`; disposable dependencies and full experiment output stay ignored. If measurement cannot run, record the blocker and uncertainty and agree the next step; do not claim benchmark verification.

## 4. Agree a compact plan and implement test-first

Keep acceptance criteria, decisions, rejected alternatives, and the agreed implementation plan in `02 - Tasks/<JIRA-KEY>/Task.md`. One record is sufficient. Load additional domain vocabulary and targeted code context as needed; do not reread entire specifications.

Immediately before the first implementation code, **including the first acceptance test**, transition Jira to **In Progress**. Isolated exploratory benchmark code belongs to discovery and does not start implementation. Use the available Jira transition and verify its result; if unavailable, report the transition as pending rather than claiming success.

For each observable acceptance criterion:

1. **Red:** Write a focused test and run it. Confirm it fails because the required behavior is absent or incorrect, not because imports, fixtures, or infrastructure are broken.
2. **Green:** Write the minimal production code to pass that test.
3. **Refactor:** Improve the implementation while preserving the tested behavior.

Tie tests to acceptance criteria rather than private methods or implementation shape. For documentation-only work or changes without an executable behavior, use relevant validation and record why Red/Green is not applicable; do not create artificial tests. Run the repository's required checks and checks appropriate to the changed surface. Record commands, actual outcomes, and justified skips; do not fabricate counts or evidence.

Continue routine implementation within the agreed plan. Bring material changes to behavior, architecture, or scope back to the engineer. Leave work uncommitted by default for review unless committing is already requested or authorized.

## 5. Record, review, and merge

Update `Task.md` with touched files, deviations, verification, and delivery state. Structural migration is conditional on concrete violations in touched or directly related code; it is not a mandatory separate ceremony for every task.

If a stable business capability changes, update `03 - Features/<feature-name>/Overview.md` with current behavior, business rules, boundaries, and related task links. Pure technical tasks create no feature folder. Keep proposed behavior clearly identified on a working branch until merge. Update governing specifications only when their durable contract changes.

When finalization is requested, prepare the task's code, tests, and documentation for review, commit the scoped files, push, and open or update the PR. Use `codex/<JIRA-KEY>-<description>` for a new branch unless the engineer specifies another convention. When code and docs live in different repositories, use linked PRs and record their merge dependency rather than claiming one atomic PR spans repositories.

| Verified milestone | Jira status |
|---|---|
| Intake, grilling, research, sandbox benchmarks | To Do |
| Immediately before writing implementation code/tests | In Progress |
| PR opened; review discussion and fixes underway | In Review |
| All task delivery PRs verified merged into `main` | Done |

Opening a PR, approval, passing CI, resolving comments, or closing an unmerged PR is not completion. Before Done, verify each required PR's merged state, base branch `main`, merge commit, and merge time from the hosting service. Record the evidence in `Task.md`; rerun finalization after merge to close the ticket. Never merge solely because finalization was requested. Failed or unavailable transitions stay explicitly pending, with the actual known Jira status retained.
