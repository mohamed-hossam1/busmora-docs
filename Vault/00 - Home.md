# BusMora Engineering Vault

Start with the ticket and the engineer's hypothesis. Challenge assumptions, check constraints, and agree the approach before implementation. The Vault keeps the execution history and current business capabilities easy to find.

## Navigation

| Need | Go to |
|---|---|
| How we work, including Jira milestones | [[01 - Rules/Task Lifecycle Protocol|Task Lifecycle Protocol]] |
| Documentation ownership and conventions | [[01 - Rules/README|Rules]] |
| What happened on a Jira ticket | [[02 - Tasks/README|Tasks]] |
| What a business capability does now | [[03 - Features/README|Features]] |
| A compact starting record | [[Templates/README|Templates]] |

## Workflow

1. **To Do:** Intake and interactive grilling; check constraints before independent engineer/AI research. Benchmark measurable selection claims in an ignored sandbox when needed.
2. **In Progress:** Transition immediately before the first implementation code/test. Use acceptance criteria to drive Red → Green → Refactor.
3. **In Review:** Open the PR with the task record and any affected business overview; discuss and verify review fixes.
4. **Done:** Only after verifying all required delivery PRs merged into `main`.

`Task.md` is the only required task document. Research, benchmarks, and evidence are conditional. Pure technical work creates no business feature folder.

## Canonical specifications

- [Core specifications](../core/README.md): product requirements, system architecture, backend/frontend design, and design system.
- [Domain](../domain/README.md): glossary, contexts, models, and events.
- [Planning](../planning/README.md): epics and schedules.

Jira owns assignments and workflow status. Task records hold decisions and evidence. Feature overviews describe current business behavior. Code and tests establish what is implemented; specifications define the agreed contracts.
