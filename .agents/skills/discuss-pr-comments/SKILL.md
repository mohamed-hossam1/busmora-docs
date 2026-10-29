---
name: discuss-pr-comments
description: "Execute Stage 1 of the PR Review lifecycle: walk through developer PR comments, grill and extract underlying preferences and standards, codify new rules into Vault (04 - Rules/), implement agreed code fixes, leave code uncommitted, and record PR Review Stage 1 completion in the task record for Stage 2 (finalize-pr-comments)."
---

# Discuss PR Comments — PR Review Stage 1 Protocol

Use this skill when a Pull Request receives review comments from another developer. This skill executes **Stage 1 (Discussion & Implementation)** of our two-stage PR Review lifecycle:

```text
PR Review Stage 1: discuss-pr-comments   --> Grill, extract preferences, codify rules, implement fixes
PR Review Stage 2: finalize-pr-comments  --> Verify tests/types/build, reply to comments, resolve threads, commit & push
```

In our engineering system, reviews are always from **fellow human developers** (never automated AI reviewers). The purpose of Stage 1 is **mutual understanding, grilling, and preference extraction first, code fixes second**. 

---

## 1. Governance & Core Principles

### 1.1 Generic & Reusable Structure
- This skill contains no hardcoded project names, user handles, or static paths. It operates generically across any repository and Obsidian Vault following our standard conventions.

### 1.2 Vault Path Discovery
Resolve `$VAULT` dynamically at the start of execution:
```text
$VAULT = <workspace-root>/busmora-docs/Vault (or directory containing 00 - Home.md and 01 - Initiatives/)
```
Canonical destinations:
- **Codified Rules**: `$VAULT/04 - Rules/<rule-name>.md`
- **Task Record Sync**: `$VAULT/02 - Features/<Feature Name>/Tasks/<JIRA-KEY>.md`

### 1.3 Core Tenets of PR Review Stage 1
1. **Preferences First, Fixes Second**:
   - Understand the *why* behind every developer comment before proposing or making edits.
   - Do not save preferences or edit code mid-discussion; wait until alignment on each group is complete.
2. **Comments are Exemplars, Not Isolated One-Offs**:
   - When the developer flags an issue on a specific line, treat it as an exemplar. Sweep the entire PR diff to identify all other occurrences of the same pattern across all touched files.
3. **Paced Discussion (One Group at a Time, One Question per Turn)**:
   - Categorize comments internally into thematic groups.
   - Surface **only one group at a time** to the developer.
   - Inside the active group, ask **only one focused question per response**. Never bundle independent decisions into a single turn.
4. **The Long-Term Engineering Lens**:
   - Recommend the solution that is **architecturally and technically correct for the long term**.
   - Never recommend a solution merely because it is the "smallest diff", "simplest change", or "works today". Smaller blast radius is only a tiebreaker between equally sound long-term architectures.
5. **No Commits / No External GitHub Mutations in Stage 1**:
   - Committing, pushing, replying to GitHub review threads, and resolving threads belongs exclusively to **Stage 2 (`finalize-pr-comments`)**.
   - Code changes remain uncommitted for review.

### 1.4 Jira Status Lifecycle Across Skills
Our engineering management system defines explicit Jira status transitions across skills:
- **`implement-jira-task`**: Moves ticket to **"In Progress"** (ONLY after grilling is complete and right before starting implementation code).
- **`migrate-outdated-practices`**: Keeps ticket **"In Progress"** (no status change).
- **`finalize-jira-task`**: Moves ticket to **"Done"**.
- **`discuss-pr-comments`**: Moves ticket to **"In Review"** (or "Review") in Phase 0 when PR review comments arrive.
- **`finalize-pr-comments`**: Moves ticket to **"Done"** again after review comments are resolved and pushed.

---

## 2. Execution Phases Overview

```text
Phase 0: Grounding & Jira Transition to In Review
Phase 1: Fetch Developer PR Comments & Codebase Sweep
Phase 2: Thematic Grouping & Surface First Group
Phase 3: Interactive One-Question-at-a-Time Discussion (Grilling)
Phase 4: Codification into Vault Rules (04 - Rules/)
Phase 5: Implement Approved Code Fixes & Cross-Seam Changes
Phase 6: Update Task Record & PR Review Stage 1 Handoff
```

---

## Phase 0 — Grounding & Jira Transition to "In Review"

Before fetching PR comments:
1. Confirm the active PR number and corresponding Jira task key.
2. **Transition Jira Ticket to "In Review"**:
   - As PR review comments have arrived from the developer, transition the ticket status to **"In Review"** (or project equivalent **"Review"**):
     - Transition ticket `<JIRA-KEY>` -> **"In Review"**.
     - If automated transition is unavailable, note in chat that the ticket is now in review.
3. Ensure working tree clean state: verify which branch is checked out and that no unrelated dirty files interfere.
4. State your understanding in a brief introductory note:
   - You are running PR Review Stage 1 (`discuss-pr-comments`).
   - You will discuss the developer's comments to extract engineering preferences.
   - You will proceed group-by-group, one question per turn.
   - You will not mutate code or persist rules until alignment is reached.
5. **Immediately proceed to Phase 1 in the same response.**

---

## Phase 1 — Fetch Developer PR Comments & Codebase Sweep

1. **Fetch Pull Request Review Threads**:
   - Using the GitHub CLI (`gh`):
     ```bash
     gh api repos/<owner>/<repo>/pulls/<PR-NUMBER>/comments --paginate
     ```
   - Fetch thread resolution statuses via GraphQL to identify open vs. resolved threads.
2. **Filter Out Already-Settled Comments**:
   - Threads explicitly agreed and marked resolved by the developer are treated as settled.
   - Unreplied or unilaterally resolved threads are **re-surfaced** to ensure substantive alignment was not missed.
3. **Full File Inspection**:
   - Read each commented file in full—not just the diff hunk—to understand the surrounding architectural context.
4. **Exemplar Sweep**:
   - For every flagged issue, extract the underlying anti-pattern or rule violation.
   - Grep the PR diff and all changed files for any other instances of the same issue.
   - Compile a complete list of affected locations across the PR.
5. **Vault Rules Cross-Reference**:
   - Check `$VAULT/04 - Rules/` to see if an existing rule already governs the pattern. Flag whether the comment aligns with an existing rule or introduces a new standard.

---

## Phase 2 — Thematic Grouping & Surface First Group

1. **Internal Thematic Grouping**:
   - Group all open comments into logical themes (e.g., *API Envelope & Pagination*, *Error Handling & Logging*, *Type Strictness*, *Database Transactions*, *UI State Management*).
   - Order groups by architectural priority (core domain/data model first, peripheral/cosmetic last).
2. **Initial Presentation to Developer**:
   - State the total count: `"Found N open review threads across G thematic groups."`
   - Provide a high-level list of the group themes (one line per group).
   - **Surface ONLY the first thematic group with full context.**
   - Include:
     - Verbatim developer comments with file:line citations.
     - Relevant code snippets.
     - Additional instances found during the exemplar sweep across the PR.
     - Your architectural analysis.
     - Concrete options with pros/cons.
     - Your recommended direction backed by the long-term engineering lens.
     - One focused question to confirm direction.

---

## Phase 3 — Interactive Discussion Within a Group (Grilling)

Inside the active group, conduct a structured dialogue. Every response adheres to this format:

```markdown
### Group [N]: [Theme Title]

**Settled in Prior Turns**:
- [Summary of points already agreed upon]

**Emerging Preferences (Sharing for calibration, not saving yet)**:
- [Clear statements of preferences emerging from the developer's feedback]

---

### Question [X]: [Specific Decision Point]

**Context & Current Code**:
[File:line and snippet of what currently exists]

**Reviewer Comment**:
> "[Developer's comment text]"

**Exemplar Sweep Findings**:
[Other locations in the PR exhibiting the same pattern]

**Options Considered**:
- **Option A**: [Description, benefits, long-term architectural impact]
- **Option B**: [Description, benefits, long-term architectural impact]

**Recommendation**:
[Your proposed choice grounded in long-term engineering correctness and Vault conventions]

**The Question for You**:
**[One clear, direct question to lock in the decision]**
```

### Discussion Rules:
- **One question per turn**: Do not ask secondary or side questions in the same message.
- **Support reversals**: If downstream implications prompt the developer to reverse a previous decision, adjust immediately and update the settled summary.
- **Push back constructively**: If a proposed fix violates existing Vault rules, introduces security risks, or breaks cross-seam integrity, explain the concern professionally before accepting.
- **Progress to Next Group**: Once all questions for the current group are resolved, summarize the decisions and surface the next group alone.

---

## Phase 4 — Codification into Vault Rules (04 - Rules/)

When the discussion concludes across all groups:

1. **Synthesize Agreed Standards**:
   - Present a crisp summary of all decisions reached during the walkthrough.
2. **Create or Update Vault Rules**:
   - For every new engineering standard or pattern preference extracted from the comments:
     - Create or update a rule file in `$VAULT/04 - Rules/<rule-name>.md`.
     - Update `$VAULT/04 - Rules/README.md` to index the new rule.

---

## Phase 5 — Implement Approved Code Fixes & Cross-Seam Changes

Now, and only now, apply code edits:

1. **Apply Fixes Across All Exemplar Locations**:
   - Apply the agreed patterns to the originally commented lines **and** all sibling instances identified during the PR sweep.
2. **Enforce Cross-Seam Bundling**:
   - If an API response shape, error format, or data contract was changed, update both backend route handlers and frontend callers in the same changeset.
3. **Leave Changes Uncommitted**:
   - Keep all changes uncommitted in the working tree for review.

---

## Phase 6 — Update Task Record & PR Review Stage 1 Handoff

1. **Update Task Execution Record**:
   - In `$VAULT/02 - Features/<Feature Name>/Tasks/<JIRA-KEY>.md`:
   - Under Section 7 (`Developer & PR Notes` / `PR Review Log`), document:
     - All comments discussed and agreed resolutions.
     - Files modified.
     - New Vault rules codified in `04 - Rules/`.
   - Record PR Review Stage 1 completion:
     ```markdown
     ### PR Review Status
     - [x] PR Review Stage 1: Discuss PR Comments (Completed: YYYY-MM-DD)
     - [ ] PR Review Stage 2: Finalize PR Comments
     ```
2. **Handoff Announcement**:
   - Report to the developer:
     > "PR Review Stage 1 (discuss-pr-comments) is complete. Code changes are implemented and uncommitted. Ready for PR Review Stage 2 (finalize-pr-comments)."
