---
name: finalize-pr-comments
description: "Execute Stage 2 of the PR Review lifecycle: verify Stage 1 (discuss-pr-comments) completion prerequisite, run test suites, typechecks, and builds, reply to each discussed comment on GitHub and resolve threads, commit and push changes, and mark PR Review Stage 2 complete in the task record."
---

# Finalize PR Comments — PR Review Stage 2 Protocol

This skill executes **Stage 2 (Finalization & Closeout)** of our two-stage PR Review lifecycle:

```text
PR Review Stage 1: discuss-pr-comments   --> Grill, extract preferences, codify rules, implement fixes
PR Review Stage 2: finalize-pr-comments  --> Verify tests/types/build, reply to comments, resolve threads, commit & push
```

Stage 2 is the closeout sibling of `discuss-pr-comments`. It **cannot run before Stage 1 is finished, grilled, and recorded**. Its primary responsibility is running the verification suite, replying to each discussed review comment on GitHub, resolving review threads, and committing/pushing the finalized code.

---

## 1. Governance & Lifecycle Prerequisites

### 1.1 Generic & Reusable Structure
- This skill contains no hardcoded project names, user handles, or static paths. It operates generically across any Git repository and Obsidian Vault following our conventions.

### 1.2 Prerequisite Gate: Stage 1 Must Be Complete (HARD RULE)
- **Finalize PR Comments cannot run before Discuss PR Comments is finished, grilled, and recorded.**
- Inspect the task execution record in `$VAULT/02 - Features/<Feature Name>/Tasks/<JIRA-KEY>.md`:
  - Check `PR Review Status`:
  - Verify that `PR Review Stage 1: Discuss PR Comments` is marked `[x]`.
- If Stage 1 is not marked complete, halt execution immediately:
  > "Finalize PR Comments cannot proceed because PR Review Stage 1 (discuss-pr-comments) is not marked complete in the task record. You must run and complete Stage 1 first."

### 1.3 Scope of Responsibilities
Similar to `finalize-jira-task`, this skill owns the verification, commit, and push actions, with **one critical extra step**:
- Running test cases and test suites.
- Running typechecks and project build.
- **Replying to each comment on GitHub** that was discussed and agreed during Stage 1.
- **Resolving each review thread** on GitHub.
- Committing and pushing the branch.
- **Transitioning the Jira ticket status back to "Done"**.

### 1.4 Jira Status Lifecycle Across Skills
Our engineering management system defines explicit Jira status transitions across skills:
- **`implement-jira-task`**: Moves ticket to **"In Progress"** (ONLY after grilling is complete and right before starting implementation code).
- **`migrate-outdated-practices`**: Keeps ticket **"In Progress"** (no status change).
- **`finalize-jira-task`**: Moves ticket to **"Done"**.
- **`discuss-pr-comments`**: Moves ticket to **"In Review"** (or "Review") when PR review comments arrive.
- **`finalize-pr-comments`**: Moves ticket to **"Done"** again after review comments are resolved and pushed.

---

## 2. Execution Phases Overview

```text
Phase 0: Prerequisite Verification (Stage 1 Check)
Phase 1: Full Verification Sweep (Tests, Types, Build)
Phase 2: Reply to Comments on GitHub & Resolve Threads
Phase 3: Atomic Git Commit & Push
Phase 4: Update Task Record & Closeout Evidence
```

---

## Phase 0 — Prerequisite Verification (Stage 1 Check)

1. Locate the active task record:
   `$VAULT/02 - Features/<Feature Name>/Tasks/<JIRA-KEY>.md`
2. Check the **PR Review Status** section:
   - Ensure `PR Review Stage 1: Discuss PR Comments` is checked `[x]` with a completion date.
3. Review the discussed comments and decisions recorded in Section 7 (`Developer & PR Notes` / `PR Review Log`) to identify:
   - The exact review comments that were resolved.
   - The agreed resolutions and reasoning for each comment.
4. If prerequisites are not satisfied, halt and notify the developer.

---

## Phase 1 — Full Verification Sweep

Before committing or replying on GitHub, execute the full quality verification suite:

1. **Test Suites**:
   - Run unit and integration test suites covering the modified files.
   - Confirm all test cases pass cleanly with zero regressions.
2. **Typecheck**:
   - Run the TypeScript compiler (`tsc --noEmit` or repo equivalent). Confirm zero typing errors.
3. **Linter & Formatter**:
   - Run repo linters to ensure compliance with project formatting standards.
4. **Project Build**:
   - Run the production build command (`npm run build` or repo equivalent). Confirm build artifacts compile without warnings or errors.
5. **Capture Receipts**:
   - Record exact command output tails (e.g., `All 48 tests passed`, `Build completed successfully in 3.4s`).

---

## Phase 2 — Reply to Comments on GitHub & Resolve Threads

This is the key extra step of PR Review Stage 2:

1. **Review Resolved Comments from Stage 1**:
   - Ingest the agreed decisions from `Tasks/<JIRA-KEY>.md`.
2. **Post Targeted Reply to Each Comment**:
   - For every discussed review comment, post a clear, professional reply on GitHub via `gh api`:
     - State the action taken (e.g., fix applied, contract aligned, pattern updated).
     - Reference the new or updated Vault rule from `04 - Rules/` if applicable.
     - If a comment was intentionally dismissed during the Stage 1 grilling session, explain the agreed architectural rationale.
3. **Resolve GitHub Review Threads**:
   - Using GitHub GraphQL API, resolve each review thread whose comments have been addressed:
     ```bash
     gh api graphql -f query='mutation { resolveReviewThread(input: {threadId: "<THREAD-ID>"}) { thread { id isResolved } } }'
     ```
   - Confirm that all discussed review threads are cleanly marked resolved on GitHub.

---

## Phase 3 — Atomic Git Commit & Push

1. **Stage Files Explicitly**:
   - Stage all implementation fixes, tests, and documentation updates resulting from the PR review.
   - Never use blind staging (`git add -A` or `git add .`).
2. **Commit Message Construction**:
   - Format commit message clearly, referencing the PR review and Jira key:
     ```text
     fix(pr-review): [JIRA-KEY] address developer PR review comments

     - Implemented agreed fixes from Stage 1 discussion
     - Codified architectural standards in Vault 04 - Rules/
     - Verified all test suites, typechecks, and build pass cleanly
     ```
   - **No AI attribution**: Never include `Co-Authored-By` or AI model trailers.
3. **Push Branch**:
   - Push commit(s) to the remote branch:
     `git push origin <branch-name>`

---

## Phase 4 — Jira Transition, Task Record Sync & Evidence

1. **Transition Jira Ticket Back to "Done"**:
   - Once review comments are replied to, threads resolved, and code committed and pushed, transition the Jira ticket status back to **"Done"**:
     - Via CLI / integration tool: `acli jira workitem transition --key <JIRA-KEY> --status "Done" --yes` (or project equivalent).
     - If automated transition is unavailable, note in chat that the ticket is marked Done.
2. **Update Task Execution Record**:
   - In `$VAULT/02 - Features/<Feature Name>/Tasks/<JIRA-KEY>.md`:
   - Under `PR Review Status`, mark Stage 2 complete:
     ```markdown
     ### PR Review Status
     - [x] PR Review Stage 1: Discuss PR Comments (Completed: YYYY-MM-DD)
     - [x] PR Review Stage 2: Finalize PR Comments (Completed: YYYY-MM-DD)
     ```
3. **Present Final Evidence Table**:

```markdown
### PR Review Finalized: [JIRA-KEY] — PR #[PR-NUMBER]

| Item | Receipt / Outcome | Status |
| :--- | :--- | :--- |
| **Prerequisite Gate** | Stage 1 (discuss-pr-comments) verified | Passed |
| **Tests & Suites** | `<Suite-name>`: `X passed, 0 failed` | Verified |
| **Typecheck** | Zero errors reported | Clean |
| **Production Build** | `Build succeeded` | Clean |
| **GitHub Comments** | Replied to all `<N>` discussed comments | Replied |
| **GitHub Threads** | `<N>` review threads resolved | Resolved |
| **Git Commit** | `<commit-sha>` — `<commit-message>` | Committed |
| **Git Push** | `origin/<branch-name>` | Pushed |
| **Jira Ticket** | `[JIRA-KEY]` | Marked Done |
| **Vault Task Record** | `02 - Features/<Feature>/Tasks/<JIRA-KEY>.md` | Stage 2 Completed |

The Pull Request comments are fully addressed, replied to, verified, and resolved. PR is ready for approval / merge.
```
