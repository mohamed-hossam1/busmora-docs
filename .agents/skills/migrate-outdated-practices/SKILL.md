---
name: migrate-outdated-practices
description: "Audit and fix concrete outdated patterns in a task's touched and directly related code while preserving observable behavior. Use when structural migration is requested or needed; it is not a mandatory stage for every ticket."
---

# Migrate Outdated Practices

Follow the [Task Lifecycle Protocol](../../../Vault/01%20-%20Rules/Task%20Lifecycle%20Protocol.md). Locate the project's Vault and `02 - Tasks/<JIRA-KEY>/Task.md`; read the implemented behavior, touched files, verification baseline, and relevant architecture rules. Missing stage checkboxes are not evidence that implementation is incomplete: establish the actual state from the record and code.

Inspect touched files and directly connected callers/callees for concrete violations of documented rules. Cite the rule and location for each finding. Do not impose generic architectural tastes, expand into an unrelated repository-wide refactor, or create a business feature folder for technical cleanup.

Explain the proposed changes and verification. Carry out migration already authorized by the request or agreed plan; discuss material scope expansion or behavior changes before proceeding. If no findings exist, record that briefly and finish without a ceremonial approval or migration pass.

Preserve public behavior, contracts, and side effects. Make incremental changes and run relevant regression checks against the known baseline; record failures, actual results, and justified skips. Do not change test expectations to hide a regression.

Append findings, changed files, deferred items, and verification to the existing `Task.md`. Update durable architecture only if its contract actually changes. Keep the current Jira status (**In Progress**, or **In Review** for an open PR); migration alone never closes a ticket. Leave changes uncommitted unless committing/delivery was requested. Report the result and remaining work without requiring numbered stage checkboxes.
