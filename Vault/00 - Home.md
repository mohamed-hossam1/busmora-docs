# Project Vault

The Vault is the team's central knowledge base and roadmap for developers and AI agents. It records product intent, requirements, technical decisions, applicable rules, and what was actually implemented.

The Vault is a **knowledge and navigation layer, not a mirror of the codebase**. Agents should inspect only the context needed for the task. Code remains the source of truth for current runtime behavior; the Vault explains intended behavior and the reasoning behind decisions.

## Start here

1. Read [[04 - Rules/Vault Conventions|Vault Conventions]] for the rules governing the Vault.
2. Read [[04 - Rules/Documentation Lifecycle|Documentation Lifecycle]] to understand when records are created and updated.
3. Follow the [[04 - Rules/End-to-End Task Execution Guide|End-to-End Task Execution Guide]] for the canonical step-by-step procedure from Jira ticket pickup to Pull Request creation using the unified Vault and Matt Pocock skills suite.
4. Check the Jira task: if it references a previous ticket or links to a Vault note, inspect those documents.
5. Read only the additional rules, decisions, and skill instructions relevant to the task.

## Vault map

- [[01 - Initiatives/README|Initiatives]] — larger product objectives, PRDs, optional benchmarks, and lightweight Slice overviews.
- [[02 - Features/README|Features]] — canonical documentation for each feature (`Overview.md`, `Requirements.md`, `Technical Design.md`, `Testing.md`) and per-task execution records (`Tasks/JIRA-123.md`).
- [[03 - Skills/README|Skills]] — unified catalog of AI-agent workflow and engineering skills (stored canonically in `.agents/skills/` and bridged into `03 - Skills/`).
- [[04 - Rules/README|Rules]] — global, team-wide conventions, lifecycle rules, and execution guides that apply across features.
- [[Templates/README|Templates]] — starting structures for new documentation.

## How the concepts relate

- **Initiative:** a larger objective, documented by a `PRD.md`; it also contains `Benchmark.md` when an algorithm, library, or tool comparison is conducted.
- **Slice:** a lightweight overview under an Initiative's `Slices/` folder. It links the Initiative to one canonical Feature and does not duplicate the full feature specification.
- **Feature:** the canonical home for a unit of functionality under `02 - Features/<Feature Name>/`.
- **Task record:** a per-Jira-task execution record at `02 - Features/<Feature Name>/Tasks/JIRA-123.md`. It records workflow progress, grilling decisions, approved plans, verification evidence, and stage completions without replacing the canonical feature documents.

## System responsibilities

- **Jira** tracks work items, assignments, and task status.
- **The Vault** stores product context, requirements, intended designs, decisions, applicable rules, and implementation records.
- **The code repository** stores the actual source code, tests, and code history.
- These systems are not assumed to synchronize automatically. Keep links and records accurate when work changes them.

## Documentation & Jira lifecycle

1. **Discovery & Grilling**: Read the Jira task. Inspect the Vault only if the ticket references a previous ticket, links to a Vault note, or for global rules. Conduct grilling and benchmarks.
2. **Pre-Implementation (Write 1)**: Save the PRD, Benchmark, and plan into `01 - Initiatives/<Initiative Name>/`.
3. **Implementation**: Move Jira ticket to **"In Progress"** (right before writing code). Implement test-first and verify. Keep code uncommitted.
4. **Post-Implementation (Write 2)**: Create or update canonical feature documents in `02 - Features/<Feature Name>/` and record task execution in `Tasks/<JIRA-KEY>.md`.
5. **Migration**: Perform structural refactoring against Vault rules (ticket remains **"In Progress"**).
6. **Task Finalization**: Consolidate records, commit, push, create PR, and transition Jira ticket to **"Done"**.
7. **PR Review Stage 1 (Discuss PR Comments)**: When peer developer comments arrive, transition Jira ticket to **"In Review"**, grill/clarify feedback, codify rules in `04 - Rules/`, and implement fixes.
8. **PR Review Stage 2 (Finalize PR Comments)**: Verify test suites/build, reply to comments on GitHub, resolve threads, push, and transition Jira ticket back to **"Done"**.

## General principles

- Prefer links to canonical documents over duplicated content.
- Distinguish intended behavior from observed implementation behavior.
- Record decisions and evidence; label unknowns rather than guessing.
- Create benchmark comparisons when choosing algorithms, libraries, or tools where the comparison could affect the decision—not for every routine choice.
- Keep secrets, credentials, tokens, and sensitive personal data out of the Vault.
