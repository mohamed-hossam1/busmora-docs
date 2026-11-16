---
name: implement-jira-task
description: "Implement a Jira task through engineer-led discovery, constraints-first research, conditional benchmarks, acceptance-driven TDD, and a compact Vault task record. Leave changes uncommitted unless delivery is requested."
---

# Implement Jira Task

Read the shared [Task Lifecycle Protocol](../../../Vault/01%20-%20Rules/Task%20Lifecycle%20Protocol.md) and [Vault Conventions](../../../Vault/01%20-%20Rules/Vault%20Conventions.md). When invoked from a code repository, locate the active project's docs Vault (containing `00 - Home.md` and `01 - Rules/`); do not create a competing Vault. Use only relevant rules and specifications.

## Discover and agree

1. Read the Jira ticket, actual status, acceptance criteria, and linked prior work. Inspect relevant code, dependencies, and tests. If the ticket is completed, establish whether the engineer intends a follow-up or reopening before implementation. If Jira is unavailable, report it and use supplied ticket content for independent preparation; never invent issue details or status changes.
2. Start from the engineer's hypothesis. Challenge prescribed tools, failure states, and material assumptions. Ask focused decision questions with recommendations; resolve factual questions by inspection. Reuse conclusions and authorization already supplied. Use `grilling` for a deeper discussion when needed, not as a mandatory second interview.
3. Before candidate research, check core architecture, team constraints, and target runtime configuration. For meaningful tool choices, invite the engineer's independent candidates while researching your own; compare attributed findings and agree the direction. Keep missing input explicit.
4. Benchmark measurable selection claims in the ignored, dependency-isolated sandbox defined by the protocol. Use representative workloads and real Arabic data where relevant. Do not substitute documentation claims for runs. If blocked, record the uncertainty and agree the next step.
5. Put acceptance criteria, decisions, and the agreed plan in `02 - Tasks/<JIRA-KEY>/Task.md`. Add lowercase `research.md`, `benchmark.md`, and `evidence/` only when useful. Do not create extra specification files as a precondition to coding.

## Implement and hand off

Transition Jira to **In Progress** immediately before the first implementation test/code and verify the transition. Record a failed or unavailable transition as pending. Apply the protocol's acceptance-driven Red → Green → Refactor loop, confirming the expected Red failure. Record honest check results and justified skips; documentation-only validation needs no artificial tests.

Continue routine work within the agreed scope. Discuss material changes to behavior, architecture, or scope with the engineer. Inspect touched code for concrete standards violations; use `migrate-outdated-practices` only if structural cleanup is needed.

Update the same task record with files, evidence, and outstanding work. Update `03 - Features/<feature-name>/Overview.md` only for an affected business capability, linking the task. Technical work creates no feature folder. Follow the protocol for linked docs/code PRs across repositories.

Leave changes uncommitted by default; honor an explicit delivery request without asking for the same permission again. Report what changed, verification, record paths, and unresolved items. Implementation completion is distinct from Jira Done; use `finalize-jira-task` when delivery or post-merge closure is requested.
