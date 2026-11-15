# Tasks

Each Jira ticket has one execution folder. It answers: what did we do, why, and what evidence supports it?

```text
02 - Tasks/<JIRA-KEY>/
├── Task.md          # Required: acceptance criteria, plan, decisions, changes, verification
├── research.md      # Optional: independent engineer and AI findings
├── benchmark.md     # Optional: empirical comparison and conclusions
└── evidence/        # Optional: sanitized logs, screenshots, reproducible harness/data recipe
```

Start with [[Templates/Task Record Template]]. Keep brief research or verification directly in the record; create extra artifacts only when the detail warrants them. Use [[01 - Rules/Task Lifecycle Protocol]] for the workflow and Jira milestones.

All technical tasks live here too. Link to [[03 - Features/README|a business capability]] only when the task changes one; never invent a feature to file a technical ticket.
