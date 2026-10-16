---
name: finalize-jira-task
description: "Execute Stage 3 (Finalize) of the task lifecycle: verify Stage 1 and Stage 2 completion, consolidate the task record in the Vault, update canonical feature docs if durable behavior changed, commit uncommitted changes, push branch, create or update PR explaining any nearby legacy refactorings, post developer notes as PR comments, transition Jira task, and present completion evidence."
---

# Finalize Jira Task — Stage 3 Protocol

This skill executes **Stage 3 (Finalization)** of the engineering task lifecycle. It is the final stage that turns reviewed, migrated local code into a fully committed, pushed, pull-requested, documented, and completed Jira task.

Stage 3 **exclusively owns the Git commit and PR creation workflow**, ending the uncommitted code state maintained during Stage 1 and Stage 2.

---

## 1. Governance & Lifecycle Prerequisites

### 1.1 Generic & Reusable Structure
- This skill contains no hardcoded project names, organization names, or static paths. It operates generically across any repository that follows the canonical Vault conventions.

### 1.2 Prerequisite Gate: Stage 1 and Stage 2 Must Be Complete (HARD RULE)
- **Finalization must not begin until both Implementation (Stage 1) and Migration (Stage 2) are completed and recorded.**
- Inspect the task execution record in `$VAULT/02 - Features/<Feature Name>/Tasks/<JIRA-KEY>.md`:
  - Verify that `Stage 1: Implement Jira Task` is marked `[x]`.
  - Verify that `Stage 2: Migration` is marked `[x]`.
- If a completion record is missing, do not guess or silently assume it is finished. Inspect the working tree and Jira task, and clarify the status with the developer.
- If either stage is incomplete, halt immediately:
  > "Finalization cannot proceed because Stage [1 or 2] is not marked complete in the task record. Complete and record prior stages first."
- *Note*: Documented skipped tests do not block finalization as long as their omission and rationale are recorded.

### 1.3 Git & Commit Ownership (The Exception to No-Commit)
- The strict no-commit rule from Stage 1 and Stage 2 **does not apply here**. Finalize is explicitly assigned to commit the reviewed changes.
- Stage changes explicitly by file path. Never use `git add -A` or blind globbing that could catch unrelated files or secrets.
- Commits must be atomic, descriptive, and free of AI attribution footers (no `Co-Authored-By`, no "Generated with AI").

### 1.4 Jira Status Lifecycle Across Skills
Our engineering management system defines explicit Jira status transitions across skills:
- **`implement-jira-task`**: Moves ticket to **"In Progress"** (ONLY after grilling is complete and right before starting implementation code).
- **`migrate-outdated-practices`**: Keeps ticket **"In Progress"** (no status change).
- **`finalize-jira-task`**: Moves ticket to **"Done"** (in Phase 5).
- **`discuss-pr-comments`**: Moves ticket to **"In Review"** (or "Review") when PR review comments arrive.
- **`finalize-pr-comments`**: Moves ticket to **"Done"** again after review comments are resolved and pushed.

---

## 2. Execution Phases Overview

```text
Phase 0: Prerequisite Verification (Stage 1 & 2 Gate)
Phase 1: Task Consolidation & Vault Documentation Sync
Phase 2: Final Verification Sweep
Phase 3: Atomic Git Commit & Push
Phase 4: Create or Update PR & Post Developer Review Comments
Phase 5: Jira Completion Transition
Phase 6: Final Evidence Report & Closeout
```

---

## Phase 0 — Prerequisite Verification

1. Locate the task execution record:
   `$VAULT/02 - Features/<Feature Name>/Tasks/<JIRA-KEY>.md`
2. Check the **Stage Completion Status**:
   - `[x] Stage 1: Implement Jira Task`
   - `[x] Stage 2: Migration`
3. Inspect `git status`:
   - Identify uncommitted files resulting from Stage 1 implementation and Stage 2 migration.
   - Ensure no unrelated dirty files from other tasks are captured.

---

## Phase 1 — Task Consolidation & Vault Documentation Sync

Consolidate the entire task journey and ensure documentation accuracy:

### 1.1 Consolidate the Task Record
Update `$VAULT/02 - Features/<Feature Name>/Tasks/<JIRA-KEY>.md` to ensure it captures:
- Final implementation summary and touched files.
- Resolved grilling decisions and approved plan.
- Migration results, including any nearby legacy code refactored.
- Testing outcomes, passing test counts, and documented test skips with reasons.
- Any notes requested by the developer during the task lifecycle (under Section `Developer & PR Notes`).
- Mark **Stage 3 Complete**:
  ```markdown
  ## 8. Stage Completion Status
  - [x] Stage 1: Implement Jira Task (Completed: YYYY-MM-DD)
  - [x] Stage 2: Migration (Completed: YYYY-MM-DD)
  - [x] Stage 3: Finalize Jira Task (Completed: YYYY-MM-DD)
  ```

### 1.2 Sync Canonical Feature Documents
Review whether the implementation or migration revealed any durable architectural or behavioral updates:
- Inspect `$VAULT/02 - Features/<Feature Name>/`:
  - `Overview.md`: Update if feature scope or external interfaces evolved.
  - `Requirements.md`: Clarify actual final behavior and any agreed adjustments.
  - `Technical Design.md`: Reflect actual architecture, component interactions, and data models.
  - `Testing.md`: Record test strategy and long-term test coverage.
- *Rule*: Do not create competing "final" notes; update the canonical documents directly.

### 1.3 Sync Initiative PRD (Only When Scope Changed)
- If the feature implementation altered high-level initiative commitments or scope, update `$VAULT/01 - Initiatives/<Initiative Name>/PRD.md`. Otherwise, leave Initiative docs untouched.

---

## Phase 2 — Final Verification Sweep

Before committing, run a final pre-commit health check across all touched files:

1. **Test Suite**: Run the unit and integration test suites covering the touched domains. Confirm all tests pass.
2. **Typecheck & Linter**: Ensure compiler (`tsc` or equivalent) and linter report clean status.
3. **Receipt Capture**: Record exact command outputs (e.g., `24 passed, 0 failed`) for the final evidence table.

---

## Phase 3 — Atomic Git Commit & Push

1. **Stage Files Explicitly**:
   - Stage all implementation code, migration refactorings, tests, and documentation files belonging to this task.
   - Leave untracked/unrelated developer files alone.
2. **Commit Message Construction**:
   - Format messages clearly following project conventions (e.g., conventional commits: `feat:`, `fix:`, `refactor:`, `docs:`).
   - Reference the Jira key in the commit message:
     ```text
     feat(domain): [JIRA-KEY] concise summary of feature implementation

     - Implemented core domain functionality
     - Migrated nearby legacy patterns to match Vault architecture rules
     - Added test suite coverage
     ```
   - **No AI attribution**: Never include `Co-Authored-By` or AI model trailers.
3. **Push Branch**:
   - Push the commit(s) to the remote tracking branch:
     `git push origin <branch-name>`

---

## Phase 4 — Create or Update PR & Post Developer Review Comments

### 4.1 PR Title and Body
Create a new Pull Request or update the existing PR for this branch:
1. **PR Title**:
   Include the Jira key and clear feature summary: `[JIRA-KEY] <Summary>`
2. **PR Description Body**:
   Must include:
   - **Summary of Changes**: What was built and the problem it solves.
   - **Nearby Legacy Refactoring (MANDATORY if touched)**:
     - Clearly list and explain any nearby legacy files refactored during Stage 2 (Migration).
     - State the architectural rationale from the Vault rules so reviewers understand why legacy code was touched.
   - **Testing & Verification**:
     - Summary of passing test suites and test types (unit/integration/E2E).
     - Any skipped checks with documented reasons.
   - **Vault References**:
     - Link to canonical feature doc and task execution record.
   - **Jira Issue Link**:
     - Link to the active Jira ticket.

### 4.2 Post Developer Notes as Comments on the PR
During grilling, implementation, or migration, the developer may have asked the AI to record specific notes (e.g., implementation nuances, rationale for complex logic, reviewer context, follow-up flags).
- Retrieve all recorded notes from Section 7 (`Developer & PR Notes`) of the task record (`Tasks/<JIRA-KEY>.md`), as well as any notes requested during finalization.
- Post these notes as explicit comments on the Pull Request:
  - If a note is general (e.g., architectural decision context, trade-off explanation), post it as a PR discussion comment.
  - If a note is tied to a specific file or boundary, post it as a file/line comment or highlight it clearly in the PR comment thread.
- Ensure all comments are clear, professional, and directly reflect the developer's guidance.

---

## Phase 5 — Jira Completion Transition

Close out the task in Jira:

1. **Update Completion Details**:
   - Add a completion comment or update relevant fields summarizing the shipped deliverables and linking the PR.
2. **Transition Status**:
   - Transition the ticket to **Done** (or project equivalent terminal status, e.g., `Closed`, `Resolved`).
   - If Jira transitions are managed via CLI (`acli`) or integration tools, execute the transition and capture the confirmation receipt.
   - If automated transitions are not available, clearly instruct the developer to mark the ticket Done and provide the text.

---

## Phase 6 — Final Evidence Report & Closeout

Conclude the task by presenting a structured evidence table in chat:

```markdown
### Task Finalized: [JIRA-KEY] — [Summary]

**Deliverables Shipped**:
- [1-2 sentences summarizing the feature and architectural outcome]

---

### Verification & Artifact Evidence

| Artifact / Action | Receipt / Location | Status |
| :--- | :--- | :--- |
| **Git Commit** | `<commit-sha>` — `<commit-message>` | Committed |
| **Git Push** | `origin/<branch-name>` | Pushed |
| **Pull Request** | [PR #XX: Title](<PR-URL>) | Created / Updated |
| **PR Comments / Notes** | `<count>` review notes posted from task record | Posted |
| **Test Verification** | `<Suite-name>`: `X passed, 0 failed` | Verified |
| **Vault Task Record** | `02 - Features/<Feature>/Tasks/<JIRA-KEY>.md` | Stage 3 Completed |
| **Canonical Docs** | `02 - Features/<Feature>/[Overview/Design/Testing].md` | Synced |
| **Jira Ticket** | `[JIRA-KEY]` | Marked Done |

---

**Task Lifecycle Complete**: Stage 1 (Implementation) -> Stage 2 (Migration) -> Stage 3 (Finalize) successfully concluded.
```
