---
name: discuss-pr-comments
description: "Discuss PR review comments with the engineer, clarify the underlying decisions, implement agreed fixes, and record review outcomes without publishing replies or commits by default."
---

# Discuss PR Comments

Follow the [Task Lifecycle Protocol](../../../Vault/01%20-%20Rules/Task%20Lifecycle%20Protocol.md). Locate the PR, Jira ticket, and `02 - Tasks/<JIRA-KEY>/Task.md`. Inspect the current branch, diff, existing changes, review threads and resolution state. Keep an open PR's Jira ticket **In Review**; verify any needed transition and report unavailable updates as pending.

Read commented code and relevant rules. Group unresolved comments by the decisions they raise, and check the PR's changed files for other occurrences of each substantiated issue. Treat already agreed, resolved threads as settled unless new evidence changes them.

Discuss unresolved decisions one group at a time, with a recommendation and concrete trade-offs. Look up facts yourself; do not ask the engineer to repeat decisions already captured. Scale the discussion to uncertainty. A review comment is not automatically a team-wide rule.

Implement agreed fixes within the task scope. Keep affected contracts and callers consistent. For behavior changes, use acceptance-driven tests and confirm the expected Red failure before production changes; preserve regression coverage for structural changes. Bring material scope expansion back to the engineer.

Record the comment/thread IDs or links, decisions, changed files, and verification in the same task record. Codify a reusable standard under `01 - Rules/` only when agreed as a team rule, and index it in that directory's README. Update the business overview only if durable business behavior changes.

Leave work uncommitted and replies unpublished by default; honor explicit delivery authorization. Report agreed outcomes, unresolved threads, and checks. Use `finalize-pr-comments` when asked to publish the review fixes. No numbered stage checkbox is needed to transfer a clear, evidenced handoff.
