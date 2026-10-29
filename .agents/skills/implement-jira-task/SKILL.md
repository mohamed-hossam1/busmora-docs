---
name: implement-jira-task
description: "Execute Stage 1 (Implementation) of the task lifecycle: verify Jira status, read Jira, inspect Vault only if referenced or linked, map code, grill product decisions and evaluate benchmarks, save grilling/benchmark/plan to Initiatives (Write 1), implement test-first, verify, create canonical Feature documentation and task record in Features (Write 2), leave code uncommitted, and record stage completion."
---

# Implement Jira Task — Stage 1 Protocol

This skill executes **Stage 1 (Implementation)** of the engineering task lifecycle. Its core purpose is to eliminate ambiguity and prevent rework by gathering product context and developer alignment *before* writing code, documenting decisions and plans in the Vault across two distinct lifecycle writes, and delivering verified, uncommitted code ready for Stage 2 (Migration).

Context not gathered before code is context paid for in rework.

---

## 1. Core Principles & Governance

### 1.1 Generic & Reusable Structure
- This skill contains no hardcoded project names, organization names, or static paths. It operates generically across any project that follows this Vault structure.

### 1.2 Vault Path Discovery
Resolve `$VAULT` dynamically at the start of execution:
1. Locate the directory containing `00 - Home.md` and `01 - Initiatives/` within the repository workspace.
2. If multiple candidates exist, prefer the one under the active project's documentation folder or allow manual specification.

Canonical Vault hierarchy:
```text
$VAULT/
├── 00 - Home.md
├── 01 - Initiatives/
│   └── <Initiative Name>/
│       ├── PRD.md
│       ├── Benchmark.md              # Optional / Created when evaluating tools or algorithms
│       └── Slices/
│           └── <Feature Name>.md
├── 02 - Features/
│   └── <Feature Name>/
│       ├── Overview.md
│       ├── Requirements.md
│       ├── Technical Design.md
│       ├── Testing.md
│       └── Tasks/
│           └── <JIRA-KEY>.md
├── 03 - Skills/
├── 04 - Rules/
│   ├── Documentation Lifecycle.md
│   └── Vault Conventions.md
└── Templates/
```

### 1.3 When the Vault is Read During Discovery
Do **not** read the entire Vault or search all initiatives and features indiscriminately. The Vault is inspected during discovery **only** when:
1. **Explicit Jira Reference**: The Jira ticket mentions a previous ticket or explicitly instructs to check previous ticket work $\rightarrow$ Inspect that referenced ticket's task record or feature documentation in the Vault.
2. **Linked or Named Vault Resource**: The ticket links to or names a specific Vault directory, initiative, or document.
3. **Global Conventions**: Team rules located in `04 - Rules/` (such as conventions and coding standards).

Otherwise, the Vault is primarily used to **document** the developer's benchmark, grilling decisions, plan, and final implementation across the two lifecycle writes.

### 1.4 The Two Vault Writes
This skill writes to the Vault at two specific lifecycle moments:
1. **Write 1 (Post-Grill / Pre-Implementation $\rightarrow$ `01 - Initiatives/`)**:
   - After the grilling session and benchmark evaluation are aligned with the developer.
   - Saves the agreed initiative context, grilling outcomes, benchmark decisions (tools, libraries, services), and the approved developer plan into `01 - Initiatives/<Initiative Name>/`.
2. **Write 2 (Post-Implementation $\rightarrow$ `02 - Features/`)**:
   - Only *after* the implementation code is written and verified test-first.
   - Creates or updates the canonical feature folder in `02 - Features/<Feature Name>/` (`Overview.md`, `Requirements.md`, `Technical Design.md`, `Testing.md`).
   - Creates the task record in `02 - Features/<Feature Name>/Tasks/<JIRA-KEY>.md` documenting the execution log, actual code changes, test verification results, and Stage 1 completion.

### 1.5 No Committing During Implementation (HARD RULE)
- All code changes must remain **uncommitted** in the working tree for developer review through VS Code Source Control / Git Changes.
- Do not run `git commit`, `git commit --amend`, or create any commit. Committing and branch management belong exclusively to Stage 3 (`finalize-jira-task`).
- If the developer asks you to commit while this skill is active, remind them:
  > "Are you sure you want to commit? There is a strict rule that changes remain uncommitted in implement-jira-task for review in VS Code Source Control."
- Confirmation from the developer authorizes **one commit only**. Subsequent commits require separate confirmation.

### 1.6 Fast Path Restrictions
- The agent must **never** decide on its own that a task is trivial to skip protocol.
- Only the developer can explicitly authorize bypassing protocol using specific phrases:
  - `"Skip grilling"`: Skip interactive grilling questions; proceed directly to consolidated understanding summary and plan.
  - `"Skip all protocol"`: Skip pre-implementation grilling, understanding summary, and formal plan approval.
- Phrases like "just fix it" or "do it quickly" do **not** authorize a bypass.
- Even on the Fast Path, the agent must inspect the Jira task, relevant code, run proper verification, leave changes uncommitted, and log what was done in the task record.

### 1.7 Jira Status Lifecycle Across Skills
Our engineering management system defines explicit Jira status transitions across skills:
- **`implement-jira-task`**: Moves ticket to **"In Progress"** (ONLY after grilling is complete and right before starting implementation code).
- **`migrate-outdated-practices`**: Keeps ticket **"In Progress"** (no status change).
- **`finalize-jira-task`**: Moves ticket to **"Done"**.
- **`discuss-pr-comments`**: Moves ticket to **"In Review"** (or "Review") when PR review comments arrive.
- **`finalize-pr-comments`**: Moves ticket to **"Done"** again after review comments are resolved and pushed.

---

## 2. Phase-by-Phase Lifecycle

```text
Phase 0: Jira Status Verification (Early Exit Check)
Phase 1: Jira Intake & Reference Detection
Phase 2: Targeted Vault Mapping (Conditional)
Phase 3: Related Work & History Sweep
Phase 4: Codebase & Dependency Mapping
Phase 5: Product Grilling & Benchmark Evaluation [GATE 1: Alignment]
Phase 6: First Vault Write — Initiative & Plan (01 - Initiatives/)
Phase 7: Test-First Implementation
Phase 8: Verification & Evidence Collection
Phase 9: Second Vault Write — Feature & Task Record (02 - Features/)
Phase 10: Uncommitted Review & Stage 1 Handoff
```

---

## Phase 0 — Jira Status Verification (Early Exit)

**Always check the Jira task status first.**

1. Retrieve the issue by key (e.g., `<PROJECT>-123`).
2. If the issue is already in a completed state (`Done`, `Closed`, `Resolved`):
   - Immediately halt execution.
   - Do not inspect code, make edits, or update documents.
   - Inform the developer: `"Task <JIRA-KEY> is already marked as <STATUS>. Halting implementation."`

---

## Phase 1 — Jira Intake & Reference Detection

Extract the full context of the active ticket:

1. **Query Issue Details**:
   - Fetch: `summary`, `description`, `status`, `issuetype`, `priority`, `parent`, `subtasks`, `issuelinks`, `labels`, `components`, `comments`, and custom fields.
   - Fetch remote links (e.g., links to documentation or pull requests).
2. **Analyze References**:
   - Note if the description or comments mention a **previous ticket** to check or build upon.
   - Note if the ticket links or points to a specific **Vault note or directory**.
   - Extract acceptance criteria verbatim (treat as critical guidance to be analyzed, not unquestionable gospel).
   - Identify stated requirements versus obvious unaddressed edge cases.

---

## Phase 2 — Targeted Vault Mapping (Conditional)

Inspect the Vault **only** when triggered by ticket references or global rules:

1. **If a Previous Ticket is Mentioned**:
   - Locate the referenced ticket's task record in `$VAULT/02 - Features/*/Tasks/<PREV-TICKET>.md`.
   - Read the previous decisions, implemented behaviors, and known follow-up notes.
2. **If a Specific Vault Document / Directory is Linked or Named**:
   - Read only the specific Initiative, Feature, or Architecture note referenced.
3. **Global Rules**:
   - Check `$VAULT/04 - Rules/` for universal conventions relevant to the active task (e.g., `Vault Conventions.md`, `Documentation Lifecycle.md`, or stack-specific rules).
4. **Identify Conflicts**:
   - If a referenced Vault note conflicts with the Jira ticket or existing code, surface the conflict for discussion during grilling.

---

## Phase 3 — Related Work & History Sweep

1. **Parent Epic & Sibling Subtasks**:
   - Inspect the parent Epic and sibling tasks to understand the broader feature boundary.
2. **Issue Links**:
   - Inspect `blocks`, `is blocked by`, and `relates to` links.
3. **Duplicate Check**:
   - Check if any recently resolved ticket already shipped the requested functionality to prevent duplicate work.

---

## Phase 4 — Codebase & Dependency Mapping

Inspect existing code to understand current runtime behavior:

1. **Domain Mapping**:
   - Locate the existing modules, routes, handlers, services, and models for this domain.
2. **Conflict Scan**:
   - Check if current code already contains endpoints or schema fields that overlap with the request.
3. **Integration Points & Callers**:
   - Grep for callers of any function, schema, or component that will be modified.
4. **Existing Dependencies**:
   - Verify existing libraries in dependencies before suggesting or adding a new package.
5. **Existing Test Patterns**:
   - Review current test layout and fixtures in the area to align with repo testing patterns.

---

## Phase 5 — Product Grilling & Benchmark Evaluation (HARD GATE 1)

Grilling eliminates product ambiguity before implementation. Conducted in chat.

### 5.1 Grilling Rules
- **One decision per turn**: Present one product decision at a time.
- **Product & business focus**: Focus on user flows, edge cases, failure states, permission boundaries, data semantics, and observable outcomes.
- **Always provide a recommendation**: Present a recommended option with clear rationale.
- **Out of scope for grilling**:
  - *Test mechanics*: The agent owns test setup, mocking, and coverage. Do not ask how to write tests.
  - *Task slicing / boundaries*: Do not ask the developer to arbitrate task assignment boundaries. Complete the required slice cleanly.
  - *Code lookups*: If answerable by reading code, look it up yourself.

### 5.2 Benchmark Evaluation (When Applicable)
- When choosing a library, service, or core algorithm where alternatives materially matter:
  - Formulate a benchmark comparison (candidates, criteria, performance/cost, tradeoffs).
  - Present the comparison to the developer to choose the agreed direction.
  - This benchmark decision feeds directly into **Write 1** (`01 - Initiatives/<Initiative Name>/Benchmark.md`).

### 5.3 Developer Plan Alignment
- Review any plan or pre-pasted approach provided by the developer.
- Synthesize the developer's inputs, grilling outcomes, and benchmark results into an aligned, step-by-step implementation plan.

### 5.4 Grilling Question Format
```markdown
### [Question Title / Decision Fork]

**Context & Current Behavior**:
[Brief description of current system behavior and what this ticket introduces]

**The Question**:
[One clear sentence defining the product decision]

**Choices**:
- **Choice A**: [Description, UX impact, behavioral outcome, tradeoffs]
- **Choice B**: [Description, UX impact, behavioral outcome, tradeoffs]

**Concrete Example**:
[Walk one realistic scenario through Choice A vs Choice B using real data, names, and states]

**UI / Observability Guide**:
[Where this behavior surfaces in the UI or how it is observed (API/DB/logs) if backend-only]

**Recommendation**:
[Your proposed choice and the direct rationale]

**Seed Data Needed?**:
[Only if seeing real data on the dev environment is genuinely necessary to decide; otherwise omit or state "None needed"]
```

Wait for developer confirmation before moving to the first Vault write.

---

## Phase 6 — First Vault Write: Initiative & Plan (Write 1)

Once grilling, benchmark choices, and the developer plan are aligned:

1. **Locate or Create Initiative Directory**:
   - Path: `$VAULT/01 - Initiatives/<Initiative Name>/`
   - Use the initiative name derived from the Epic or feature group.
2. **Save Initiative PRD**:
   - Create or update `$VAULT/01 - Initiatives/<Initiative Name>/PRD.md` using `Templates/Initiative PRD Template.md`.
   - Document problem, goals, scope, agreed requirements, and the developer's approved plan.
3. **Save Benchmark (If Conducted)**:
   - If tools, libraries, or algorithms were evaluated, save `$VAULT/01 - Initiatives/<Initiative Name>/Benchmark.md` using `Templates/Benchmark Template.md`.
   - Document objective, evaluated candidates, criteria, and the chosen decision.
4. **Save / Update Slice Link**:
   - Ensure `$VAULT/01 - Initiatives/<Initiative Name>/Slices/<Feature Name>.md` exists and links the initiative to the intended feature.

Confirm to the developer that Write 1 is complete and specify the saved Vault paths.

---

## Phase 7 — Test-First Implementation

Now, and only now, write code.

### 7.0 Transition Jira to "In Progress"
- **Timing Rule (STRICT)**: Transition the ticket to **"In Progress"** only after grilling is completed and the plan/Write 1 are saved, **right before writing the first line of code**.
- Never transition to "In Progress" during discovery or grilling.
- Execute the transition via CLI or Jira integration tool:
  - Transition ticket `<JIRA-KEY>` -> **"In Progress"**.
  - If automated transition is unavailable, note in chat that the ticket is now in progress.

### 7.1 Pre-Coding Standards Check
- Check relevant rules in `$VAULT/04 - Rules/` and follow closest in-repo analogs.

### 7.2 Red-Green-Refactor Loop
   - **Red**: Write a focused, failing test for the next observable behavior from the approved plan. Confirm it fails for the expected reason.
   - **Green**: Write the minimal production code necessary to make the test pass.
   - **Refactor**: Clean up and optimize while keeping tests green.
3. **Mid-Flight Deviations**:
   - If implementation uncovers an unexpected roadblock or the agreed plan no longer fits:
   - **Stop coding immediately.**
   - Escalate to the developer with the discovery and proposed adjustment.
   - Resume only upon explicit approval.

---

## Phase 8 — Verification & Evidence Collection

1. **Test Execution**:
   - Run unit, integration, and applicable end-to-end tests for all touched areas.
   - Run type checks and linters.
2. **Evidence Recording**:
   - Capture passing test counts, command outputs, and verified behaviors.
3. **Document Skipped Checks**:
   - If any test is deliberately skipped (e.g., requires external environment not available locally), record the skip and its explicit justification.

---

## Phase 9 — Second Vault Write: Feature & Task Record (Write 2)

After implementation is complete and verified:

1. **Create or Update Canonical Feature Folder**:
   - Path: `$VAULT/02 - Features/<Feature Name>/`
   - Create or update documents using templates from `Templates/`:
     - `Overview.md`: Purpose, value, scope, and parent initiative link.
     - `Requirements.md`: Actual functional requirements and behavior.
     - `Technical Design.md`: Architecture, data flow, components, and technical decisions.
     - `Testing.md`: Test strategy, performed tests, coverage, and test fixtures.
2. **Create Task Record**:
   - Path: `$VAULT/02 - Features/<Feature Name>/Tasks/<JIRA-KEY>.md`
   - Structure:
     ```markdown
     # Task: <JIRA-KEY> — <Summary>

     **Feature**: [[02 - Features/<Feature Name>/Overview|Feature Overview]]
     **Initiative**: [[01 - Initiatives/<Initiative Name>/PRD|Initiative PRD]]
     **Jira Key**: <JIRA-KEY>
     **Status**: Stage 1 Completed

     ---

     ## 1. Ticket Overview
     - **Summary**: <Summary>
     - **Issue Type**: <Type>
     - **Original Acceptance Criteria**:
       <Verbatim criteria>

     ## 2. Grilling Decisions & Alignment
     - **Decision 1**: <Summary of decision and rationale>
     - **Decision 2**: <Summary of decision and rationale>

     ## 3. Approved Implementation Plan
     <Paste the approved plan executed during Phase 7>

     ## 4. Execution & Verification Log
     - **Touched Files**:
       - `<file-path>`: <summary of change>
     - **Tests Performed**:
       - `<test-file>`: <passing count / status>
     - **Skipped Tests & Justifications**:
       - <None or detail of skipped test with reason>
     - **Deviations from Plan**:
       - <None or documented deviation with approval context>

     ## 5. Developer & PR Notes
     <!-- Notes requested by developer during grilling or implementation to be posted as comments on the PR in Stage 3 -->
     - <Developer note 1>

     ## 6. Stage Completion Status
     - [x] Stage 1: Implement Jira Task (Completed: YYYY-MM-DD)
     - [ ] Stage 2: Migration
     - [ ] Stage 3: Finalize Jira Task
     ```

Confirm to the developer that Write 2 is complete and specify the updated feature and task paths.

---

## Phase 10 — Uncommitted Review & Stage 1 Handoff

1. **Verify Uncommitted State**:
   - Ensure all code changes remain **uncommitted** in the working directory for developer review in VS Code Source Control.
2. **Handoff Report**:
   - Summarize implemented functionality.
   - Summarize test verification evidence.
   - State updated Vault files:
     - Write 1: `$VAULT/01 - Initiatives/<Initiative Name>/`
     - Write 2: `$VAULT/02 - Features/<Feature Name>/`
   - Announce readiness: **Stage 1 (Implementation) is complete. Ready for Stage 2 (Migration).**
