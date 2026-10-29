# Documentation Lifecycle

## 1. Before implementation (Write 1 — Initiatives)

When a developer or AI agent begins a Jira Epic or major feature task:

1. Identify or create the relevant Initiative folder under `01 - Initiatives/<Initiative Name>/`.
2. Inspect the Vault only if the Jira ticket references a previous ticket or links directly to Vault documentation.
3. Conduct product grilling to eliminate ambiguity and confirm understanding.
4. Conduct benchmark evaluations (`Benchmark.md`) when comparing algorithms, libraries, or tools where tradeoffs matter.
5. Align with the developer on the implementation plan.
6. Save the agreed context, grilling outcomes, benchmark decisions, and approved plan to `01 - Initiatives/<Initiative Name>/` (`PRD.md`, `Benchmark.md`, and Slices).

## 2. After implementation (Write 2 — Features)

After implementation code is written and verified test-first:

1. Create or update the canonical Feature folder under `02 - Features/<Feature Name>/`:
   - `Overview.md`: summary, value, scope, parent initiative link.
   - `Requirements.md`: actual implemented requirements and behavior.
   - `Technical Design.md`: architecture, data flow, and technical details.
   - `Testing.md`: test strategy, performed tests, and evidence.
2. Create or update the task execution record at `02 - Features/<Feature Name>/Tasks/<JIRA-KEY>.md`:
   - Record touched files, test outcomes, skipped checks with justification, and Stage 1 completion.

## 3. Subsequent lifecycle stages

1. **Stage 2 (Migration)**: Structural refactoring against Vault rules with zero business logic changes. Record migration outcomes and mark Stage 2 completed in `Tasks/<JIRA-KEY>.md`.
2. **Stage 3 (Finalize Jira Task)**: Consolidate records, update feature documents if needed, and execute commit and PR workflow.

For the end-to-end integration combining Jira, the Vault, and the Matt Pocock skill suite, see [[End-to-End Task Execution Guide|End-to-End Task Execution Guide]].

## 4. Jira Status Lifecycle Across Skills

The Jira ticket status transitions at strictly defined milestones:
1. **Pre-Implementation**: Ticket remains in its initial status (`Backlog`, `Selected`, or `To Do`) during discovery, research, and grilling.
2. **Right Before Implementation Starts**: `implement-jira-task` transitions the ticket to **"In Progress"** (ONLY after grilling is complete and Write 1 is saved, immediately before writing code).
3. **Migration**: `migrate-outdated-practices` keeps the ticket in **"In Progress"** (no status change).
4. **Finalize Jira Task**: `finalize-jira-task` transitions the ticket to **"Done"** after commit, push, PR creation, and documentation sync.
5. **PR Review Stage 1 (Discuss PR Comments)**: When peer developer comments arrive on the PR, `discuss-pr-comments` transitions the ticket to **"In Review"** (or **"Review"**).
6. **PR Review Stage 2 (Finalize PR Comments)**: After all review comments are addressed, verified, replied to, resolved on GitHub, and pushed, `finalize-pr-comments` transitions the ticket back to **"Done"**.

## 5. Separation of responsibilities

- **Jira** tracks work assignments, tickets, and status.
- **The Vault** stores enduring architectural knowledge, decisions, benchmarks, and task execution logs.
- **The code repository** stores source code, tests, and commit history.
- Jira and the Vault are not automatically synced; links and task records are maintained explicitly.
