---
name: finalize-pr-comments
description: "Verify and publish agreed PR review fixes, then reply to addressed comments and resolve their threads. Keep Jira In Review until all required task delivery PRs are verified merged into main."
---

# Finalize PR Comments

Follow the [Task Lifecycle Protocol](../../../Vault/01%20-%20Rules/Task%20Lifecycle%20Protocol.md). Locate the PR and the project's `02 - Tasks/<JIRA-KEY>/Task.md`. Inspect current threads, agreed decisions, branch, and changes. Establish that the fixes implement the agreed review outcomes; a missing stage checkbox alone does not require repeating discussion. Discuss genuinely unresolved decisions before publishing their resolution.

Explicit invocation of this skill authorizes publishing the agreed review fixes and their targeted replies. If this skill was selected implicitly, obtain authorization for external replies unless the user's request already includes them. Do all preparation and verification first. Do not merge the PR as part of this workflow.

1. Run the checks required for the changed surface (tests, types, lint, and build as applicable). Record actual results, failures, and skips. Update the task and any affected business overview.
2. Stage only explicit task files, commit with the Jira key and no AI attribution trailers, and push to the PR branch. Verify the remote head contains the fixes before replying or resolving threads. If push fails, leave those replies/resolutions pending.
3. For each addressed comment, post a concise reply linking the pushed fix or explaining the agreed rationale. Resolve only threads whose requested work is addressed; leave unanswered or deferred requests open. Check existing replies before retrying to prevent duplicates.
4. Record commit, reply, and resolution receipts in `Task.md`; persist scoped record updates through the normal docs workflow and disclose any pending publication. Keep the ticket **In Review** while its PRs remain open. If a Jira update is unavailable, report pending status rather than claiming success.

Resolving review comments does not mean Done. If all required task delivery PRs have already merged into **main**, use the verified post-merge closure procedure in `finalize-jira-task`: check merged state, base, merge commit/time, and documentation before transitioning Jira. Otherwise report review fixes delivered and merge still pending.
