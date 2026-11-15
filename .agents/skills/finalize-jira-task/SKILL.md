---
name: finalize-jira-task
description: "Prepare and deliver an implemented Jira task through scoped commits and PRs, or close it after verified merge into main. PR opening sets In Review; Done requires merge evidence for every required delivery PR."
---

# Finalize Jira Task

Read the [Task Lifecycle Protocol](../../../Vault/01%20-%20Rules/Task%20Lifecycle%20Protocol.md) and [Documentation Lifecycle](../../../Vault/01%20-%20Rules/Documentation%20Lifecycle.md). Locate the active project's Vault, task record, repositories, branches, and PRs. Follow the delivery or post-merge path according to actual state.

## Prepare delivery

- Check implementation against acceptance criteria and the record's verification evidence. Resolve unfinished work within scope. Migration is conditional on findings; do not require a separate migration checkbox or rerun an already completed interview.
- Inspect working trees and diffs. Preserve unrelated changes; stage only explicit task paths. Use an existing suitable branch, or the protocol's branch convention when creating one. Do not rewrite shared history.
- Consolidate `02 - Tasks/<JIRA-KEY>/Task.md`. Update only affected business overviews in `03 - Features/`; technical tasks create no feature folder. Across repositories, prepare linked code/docs PRs and document the merge dependency.
- Complete required checks and relevant acceptance verification. Reuse valid evidence for unchanged files; rerun checks when changes or failures invalidate it. Record skipped or unavailable checks with reasons, not invented success.

## Commit, push, and open review

When invoked to deliver the task, commit the scoped code, tests, and documentation with the Jira key and repository conventions. Use no AI attribution trailers. Push and open or update the PR, applying the `pr` skill when available. Explain the resulting behavior, verification, relevant migration rationale, Jira link, and task/feature references; omit irrelevant sections.

At PR opening, transition Jira to **In Review** and verify it. If a PR already exists, inspect and reconcile the actual status. Put reviewer context in the PR body; post separate comments only when explicitly requested. Record verified commit/PR receipts in `Task.md`; never pre-mark delivery successful. If receipt updates leave local docs changes, commit/push the scoped update and disclose any remaining unpublished record rather than claiming a clean delivery.

Before retrying a failed publication, inspect remote state to avoid duplicate PRs or commits. If credentials or tools block progress, report the concrete pending action. Do not claim Jira changed when the transition is unavailable.

## Close only after merge

For an open PR, report **In Review / awaiting merge** and finish. Approval, green CI, resolved threads, or a closed-unmerged PR do not qualify for Done. Do not merge automatically as part of finalization.

On post-merge finalization, fetch hosting-service evidence that every required task delivery PR is merged with base branch **main**. Record PR URL, merge commit, and merge time. Verify the task's linked documentation is synchronized. Only then transition Jira to **Done**, confirm the result, and record its receipt. If a required PR is open, unmerged, targets another branch, or cannot be verified, leave closure pending and state why. Persist post-merge receipt changes through the repository's ordinary documentation workflow; never push directly to main to bypass review.

Report the actual PR/merge state, checks, documentation, and Jira status. Distinguish implementation delivered for review from task completed after merge.
