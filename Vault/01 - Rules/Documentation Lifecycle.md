# Documentation Lifecycle

The [[Task Lifecycle Protocol]] governs engineering work and Jira milestones. This note governs where the evidence lives.

- **During discovery:** Create or update `02 - Tasks/<JIRA-KEY>/Task.md` with acceptance criteria, decisions, and the agreed plan. This is the only required task document. Add `research.md`, `benchmark.md`, or `evidence/` only when useful findings or measurements exist.
- **During implementation:** Extend the same record with changed files, Red/Green evidence, relevant checks, deviations, and unresolved items. Do not create a separate PRD, slice, requirements, design, or testing document per ticket.
- **At PR preparation:** Update an existing business capability's `03 - Features/<feature-name>/Overview.md`, or create one if the capability is new. Pure technical work stays in Tasks. Keep durable architecture in `core/` and domain definitions in `domain/`.
- **During review:** Append decisions and verification to the same task record. Promote a preference to `01 - Rules/` only when the team agrees it is a reusable standard.
- **After merge into main:** Record verified merge evidence and actual Jira status. Feature overviews describe the merged capability; pending work stays in task records and working branches. For separate code and docs repositories, link the delivery PRs and their merge dependency.

Templates are starting points, not required paperwork. Remove unused sections and optional links. Never backfill invented research, measurements, test results, status transitions, or merge dates.

Workflow skills share this protocol: `implement-jira-task` handles discovery and implementation; `migrate-outdated-practices` handles needed structural cleanup; `finalize-jira-task` handles delivery and verified post-merge closure. The PR-comment skills handle discussion and publication of agreed review fixes. No separate migration pass is required when there is nothing to migrate.
