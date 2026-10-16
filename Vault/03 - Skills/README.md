# Skills

Store reusable AI-agent skills and instructions here.

A skill explains **how to perform a repeatable task**. Keep skill instructions specific, actionable, and honest about unknowns. Skills should not replace the global rules that make a process mandatory.

## Task Implementation Lifecycle Skills

- [[implement-jira-task/SKILL|implement-jira-task]] — Stage 1: Discovery, grilling, benchmark evaluation, test-first implementation, and Vault documentation. *(Transitions Jira to "In Progress" right before implementation starts).*
- [[migrate-outdated-practices/SKILL|migrate-outdated-practices]] — Stage 2: Structural and architectural refactoring against Vault rules with zero business logic changes. *(Keeps Jira "In Progress").*
- [[finalize-jira-task/SKILL|finalize-jira-task]] — Stage 3: Task consolidation, commit, push, PR creation, and documentation sync. *(Transitions Jira to "Done").*

## PR Review Lifecycle Skills (Developer Peer Reviews)

- [[discuss-pr-comments/SKILL|discuss-pr-comments]] — PR Review Stage 1: Walk through developer PR comments, grill and extract preferences, codify new rules into `04 - Rules/`, and implement uncommitted code fixes. *(Transitions Jira to "In Review").*
- [[finalize-pr-comments/SKILL|finalize-pr-comments]] — PR Review Stage 2: Verification sweep (tests, types, build), reply to discussed comments on GitHub, resolve threads, commit, and push. *(Requires Stage 1 completion. Transitions Jira back to "Done").*

## Jira Status Transitions Summary

```text
Discovery & Grilling                   --> Initial state (To Do / Selected)
Right before Implementation (Phase 7)   --> In Progress  (implement-jira-task)
Migration                               --> In Progress  (migrate-outdated-practices)
Finalize Jira Task                      --> Done         (finalize-jira-task)
When PR comments arrive & Discuss runs  --> In Review    (discuss-pr-comments)
Finalize PR Comments                    --> Done         (finalize-pr-comments)
```

Add new skills in their own folders with a `SKILL.md` file.
