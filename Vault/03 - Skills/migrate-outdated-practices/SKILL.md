---
name: migrate-outdated-practices
description: "Execute Stage 2 (Migration) of the task lifecycle: verify Stage 1 completion, audit touched and nearby code against Vault architecture rules, present violations in a consolidated approval report, execute approved structural refactorings with zero business logic changes, verify no test regressions, leave changes uncommitted, and record stage completion in the task record."
---

# Migrate Outdated Practices — Stage 2 Protocol

This skill executes **Stage 2 (Migration)** of the engineering task lifecycle. Its purpose is to bring the code produced in Stage 1 and relevant nearby legacy code into full compliance with the project's documented architectural rules and standards. 

This is a **structural and architectural refactoring stage**, not an opportunity to alter business logic.

---

## 1. Governance & Lifecycle Prerequisites

### 1.1 Generic & Reusable Structure
- This skill contains no hardcoded project names, organization names, or static paths. It operates generically across any repository that follows the canonical Vault conventions.

### 1.2 Prerequisite Gate: Stage 1 Must Be Complete (HARD RULE)
- **Do not begin Migration before Implementation (Stage 1) is finished and recorded.**
- Check the task execution record in `$VAULT/02 - Features/<Feature Name>/Tasks/<JIRA-KEY>.md`:
  - Verify that `Stage 1: Implement Jira Task` is marked complete.
  - If the completion record is missing, inspect the git working tree and Jira task, and clarify the status with the developer. Do not silently assume Stage 1 is complete.
  - If Stage 1 is still in progress, halt immediately:
    > "Stage 1 (Implementation) is not marked complete in the task record. Complete and record Stage 1 before running Migration."

### 1.3 Strict Scope & "No Business Logic Change" Rule
- **Structural only**: This stage updates patterns, enforces module boundaries, modernizes deprecated helpers, improves typing, and adds structured logging. It must **never** alter business logic, feature behavior, or API semantics agreed upon during Stage 1.
- If a structural fix has any chance of affecting business logic, or if an architectural rule conflicts with existing behavior, **stop and ask the developer** before touching the code.
- **Controlled scope**: Audit files touched during Stage 1, plus nearby legacy code directly connected to those files. Do not initiate an uncontrolled repo-wide refactor unless explicitly instructed.

### 1.4 No Committing During Migration (HARD RULE)
- All migration changes must remain **uncommitted** in the working tree for developer review through VS Code Source Control / Git Changes.
- Do not run `git commit`, `git commit --amend`, or create any commit. Committing belongs exclusively to Stage 3 (`finalize-jira-task`).
- If the developer asks you to commit while this skill is active, remind them:
  > "Are you sure you want to commit? There is a strict rule that changes remain uncommitted in migrate-outdated-practices for review in VS Code Source Control."
- Confirmation authorizes **one commit only**. Subsequent commits require separate confirmation.

### 1.5 Jira Status Lifecycle Across Skills
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
Phase 1: Vault Architecture Rules Intake (04 - Rules/)
Phase 2: Audit Touched Files & Nearby Legacy Code
Phase 3: Consolidated Migration Proposal [APPROVAL GATE]
Phase 4: Execute Approved Refactorings
Phase 5: Regression Verification & Evidence Check
Phase 6: Update Task Execution Record (Tasks/<JIRA-KEY>.md)
Phase 7: Uncommitted Review & Stage 2 Handoff
```

---

## Phase 0 — Prerequisite Verification

1. Locate the active task record:
   `$VAULT/02 - Features/<Feature Name>/Tasks/<JIRA-KEY>.md`
2. Inspect the **Stage Completion Status** section:
   - Ensure `Stage 1: Implement Jira Task` has a checked status `[x]` with a completion date.
3. Review the **Execution & Verification Log** in the task record to identify:
   - All files modified or created during Stage 1.
   - The test suites that passed at the end of Stage 1 (baseline for regression testing).
4. If prerequisites are not satisfied, halt and notify the developer.

---

## Phase 1 — Vault Architecture Rules Intake

Load the governing rules from `$VAULT/04 - Rules/`:

1. **Vault Conventions & Lifecycle**:
   - `04 - Rules/Vault Conventions.md`
   - `04 - Rules/Documentation Lifecycle.md`
2. **Technical & Architectural Standards**:
   - Ingest all active technical rules in `$VAULT/04 - Rules/` relevant to the touched technology stack (e.g., API contracts, error handling, typing standards, database operations, observability).
3. **Repository Analogs**:
   - Inspect existing modern references in the repository to match established naming, import grouping, and architectural layering.

---

## Phase 2 — Audit Touched Files & Nearby Legacy Code

Audit all files modified in Stage 1, plus closely related callers/callees in the same domain. Evaluate against standard architectural categories:

### Category 1: Module Boundaries & Architectural Layering
- Direct database calls from route handlers or UI components bypassing service/repository layers.
- Leaked internal abstractions or circular dependencies across domain boundaries.
- Tight coupling where dependency injection or clear interface contracts should exist.

### Category 2: API Contracts & Typing Standards
- Endpoints or methods using loose `any`, hand-rolled type casts (`as Type`), or unvalidated request bodies.
- Non-standard response envelopes (inconsistent `{ success, data, error }` wrappers or malformed pagination shapes).
- Timestamp fields lacking strict date typing or hydration helpers.

### Category 3: Error Handling & User-Facing Safety
- Empty or swallowed `catch` blocks.
- Unhandled promise rejections or raw database error objects leaking to the client.
- Exposing internal database keys, UUIDs, or sensitive stack traces in user-facing error messages.

### Category 4: Observability & Structured Logging
- Ad-hoc `console.log`, `console.warn`, or `console.error` in production code.
- Missing structured logging around critical operations, mutations, or external service calls.
- Inconsistent log formats lacking standard correlation IDs or context metadata.

### Category 5: Deprecated Helpers & Legacy Patterns
- Usage of deprecated project helpers or legacy utility functions where modern project standards exist.
- Write-then-dispatch patterns lacking transactional atomicity where background tasks could fail after state is committed.
- Unnecessary boilerplate that can be streamlined using the project's standard abstractions.

---

## Phase 3 — Consolidated Migration Proposal (APPROVAL GATE)

Before editing any code, compile all identified violations into a single structured report presented in chat.

### Proposal Format
Present findings grouped by category:

```markdown
## Stage 2 Migration Proposal: <JIRA-KEY>

### Summary
[Brief statement of audited scope: number of files audited, total violations found, and overall risk assessment.]

---

### Category: [Category Name]

- **Finding**: [Short descriptive title]
  - **Location**: `path/to/file.ts:line`
  - **Current Pattern**: [Description or brief snippet of the outdated/violating code]
  - **Target Standard**: [Governing rule from 04 - Rules/ or established project pattern]
  - **Proposed Fix**: [Precise refactoring action]
  - **Risk / Blast Radius**: [Low/Medium/High + rationale; confirm no business logic impact]
  - **Verification Plan**: [Specific test suite or typecheck that verifies the fix]

*(If a category has no findings, state: "No violations detected in audited scope.")*

---

### Approval Request
**Would you like me to proceed with migrating these approved findings?**
*(You may approve all at once or specify any items to omit).*
```

### Approval Rules:
- **One batch approval**: A single developer confirmation authorizes the execution of the selected proposed migrations.
- If the developer excludes certain findings, mark them as skipped with the developer's rationale in the task record.
- **Never guess on ambiguous rules**: If a proposed fix could touch business logic or edge-case behavior, highlight the tradeoff and seek explicit guidance.

---

## Phase 4 — Execute Approved Refactorings

Once approved:

1. **Refactor Incrementally**:
   - Apply structural fixes cleanly to the approved files.
   - Refactor nearby legacy code only where approved and strictly necessary.
2. **Preserve Business Logic**:
   - Keep inputs, outputs, and side effects byte-compatible with the intent established in Stage 1.
3. **Keep Diffs Disciplined**:
   - Avoid gratuitous reformatting or unrelated file touches. Maintain clean, reviewable diffs.

---

## Phase 5 — Regression Verification & Evidence Check

Ensure that structural refactoring introduced zero regressions:

1. **Run Full Test Suite on Touched Domains**:
   - Execute all unit, integration, and E2E tests covering the modified files.
   - **Zero-Regression Rule**: Every test that passed at the end of Stage 1 **must continue to pass**.
2. **Handle Regressions Immediately**:
   - If any previously passing test fails, investigate immediately.
   - Determine whether the test failure reveals an unintended change to business logic or an outdated test assertion.
   - If the refactoring introduced a regression, fix the implementation to restore passing status.
3. **Compile & Lint Verification**:
   - Run typecheckers (`tsc` or equivalent) and linters to confirm no syntax or typing violations remain.

---

## Phase 6 — Update Task Execution Record

Document all migration outcomes in the task's execution record:

**Path**: `$VAULT/02 - Features/<Feature Name>/Tasks/<JIRA-KEY>.md`

Append a dedicated section to the record:

```markdown
## 6. Migration Log (Stage 2)

- **Audited Scope**:
  - Touched files and nearby legacy components audited.
- **Approved & Applied Refactorings**:
  - `<file-path>`: [Refactoring applied and rule referenced from 04 - Rules/]
- **Nearby Legacy Code Refactored**:
  - `<file-path>`: [Rationale and scope of legacy cleanup]
- **Skipped Findings**:
  - [Any proposed finding that was deferred or omitted, with developer rationale]
- **Regression Verification**:
  - All Stage 1 tests re-verified: **Passed**
  - Typecheck and linter status: **Clean**

## 7. Developer & PR Notes
<!-- Additional notes requested by developer during migration to be posted as comments on the PR in Stage 3 -->
- [Developer notes if requested, otherwise omit or keep existing]

## 8. Stage Completion Status
- [x] Stage 1: Implement Jira Task (Completed: YYYY-MM-DD)
- [x] Stage 2: Migration (Completed: YYYY-MM-DD)
- [ ] Stage 3: Finalize Jira Task
```

---

## Phase 7 — Uncommitted Review & Stage 2 Handoff

1. **Verify Uncommitted State**:
   - Ensure all changes (from both Stage 1 and Stage 2) remain **uncommitted** in the working tree for developer review in VS Code Source Control.
2. **Handoff Report**:
   - Summarize the structural improvements applied.
   - Highlight any nearby legacy code cleaned up.
   - Confirm all tests pass without regressions.
   - Confirm task record updated with Stage 2 completion.
   - Announce readiness: **Stage 2 (Migration) is complete. Ready for Stage 3 (Finalize Jira Task).**
