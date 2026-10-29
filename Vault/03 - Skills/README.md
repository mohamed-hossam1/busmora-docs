# Unified AI Skills Catalog

This directory serves as the **Vault's unified catalog and entry point for all AI-agent skills** used across the repository.

A skill defines **how to perform a repeatable task** with clear instructions, constraints, and actionable steps. Skills work hand-in-hand with global governance rules in `[[04 - Rules/README|04 - Rules/]]`.

> [!NOTE] Unified Architecture & Storage
> - **Canonical Storage**: All agent skills are stored canonically in `.agents/skills/`. This standard runtime directory is directly discovered and executed by AI coding environments (Antigravity, Claude Code, Cursor, Copilot, Cline, etc.).
> - **Vault & Tooling Bridges**: Both `Vault/03 - Skills/` and `.claude/skills/` use transparent relative symlinks pointing to `.agents/skills/`. This ensures Obsidian users have full navigation and wikilink connectivity without file duplication, while all AI engines share the exact same instructions.

For a complete walkthrough of how to use these skills end-to-end on any Jira ticket, consult:
👉 **[[04 - Rules/End-to-End Task Execution Guide|End-to-End Task Execution Guide: From Jira to PR]]**

---

## 1. Task Implementation Lifecycle Skills

The core engineering workflow orchestrating Jira state, uncommitted review, test-first delivery, and Obsidian Vault synchronization.

| Skill | Lifecycle Stage | Jira Status Transition | Purpose |
|---|---|---|---|
| **[[implement-jira-task/SKILL\|implement-jira-task]]** | **Stage 1**: Discovery & Implementation | Moves to **"In Progress"** *(only right before writing code)* | Verifies Jira status, selectively maps Vault & code, conducts grilling, saves Write 1 (`01 - Initiatives/`), writes test-first code, verifies, saves Write 2 (`02 - Features/`), leaves code uncommitted. |
| **[[migrate-outdated-practices/SKILL\|migrate-outdated-practices]]** | **Stage 2**: Structural Migration | Remains **"In Progress"** | Scans touched and adjacent code for deprecated patterns or Vault rule divergences; refactors structurally with zero business logic changes. |
| **[[finalize-jira-task/SKILL\|finalize-jira-task]]** | **Stage 3**: Finalization & PR | Transitions to **"Done"** | Verifies Stage 1 & 2 completion, consolidates task record, commits atomic changes, pushes branch, generates PR with Vault links, and completes Jira task. |

---

## 2. PR Review & Feedback Lifecycle Skills

Orchestrates peer developer feedback received on pull requests.

| Skill | Lifecycle Stage | Jira Status Transition | Purpose |
|---|---|---|---|
| **[[discuss-pr-comments/SKILL\|discuss-pr-comments]]** | **PR Review Stage 1**: Discuss & Fix | Transitions to **"In Review"** | Interactively walks through PR comments, extracts preferences, codifies new durable rules into `04 - Rules/`, and implements uncommitted fixes. |
| **[[finalize-pr-comments/SKILL\|finalize-pr-comments]]** | **PR Review Stage 2**: Resolve & Push | Transitions back to **"Done"** | Runs full test and build verification, replies to comments on GitHub, resolves threads, commits, pushes, and transitions Jira back to "Done". |

---

## 3. Architecture, Design & Grilling Skills (Matt Pocock Suite)

Specialized engineering and design skills for precision domain modeling, codebase design, and pre-implementation rigor.

| Skill | Area | Best Used During | Key Focus |
|---|---|---|---|
| **[[grilling/SKILL\|grilling]]** | Discovery / Grilling | Pre-Implementation (Gate 1) | Relentlessly stress-tests ideas, decisions, edge cases, and failure modes one question per turn with concrete examples and recommendations. |
| **[[grill-me/SKILL\|grill-me]]** | Interactive Alignment | Pre-Implementation (Gate 1) | Developer-facing interactive interview mode to resolve open design decisions. |
| **[[grill-with-docs/SKILL\|grill-with-docs]]** | Alignment with Repo Docs | Pre-Implementation (Gate 1) | Conducts grilling grounded against canonical repo docs, PRDs, and domain models. |
| **[[codebase-design/SKILL\|codebase-design]]** | Architecture & Interfaces | Pre-Implementation / Design | Enforces "Design It Twice", "Deep Modules" (small interface, deep capability), identifying seams, and testability. |
| **[[domain-modeling/SKILL\|domain-modeling]]** | Domain-Driven Design | Discovery / Design | Establishes and sharpens ubiquitous language, maintains `GLOSSARY.md`, bounded contexts, and records Architectural Decision Records (ADRs). |
| **[[to-spec/SKILL\|to-spec]]** | Specification | Intake / Discovery | Turns ambiguous user or issue requirements into a structured, testable engineering specification. |
| **[[implement-spec/SKILL\|implement-spec]]** | Test-Driven Development | Implementation (Stage 1) | Strict red-green-refactor TDD execution against an approved specification or plan. |
| **[[to-tickets/SKILL\|to-tickets]]** | Task Decomposition | Initiative / Planning | Breaks a comprehensive feature specification into atomic, independently shippable implementation tickets. |
| **[[code-review/SKILL\|code-review]]** | Code Quality | Pre-PR Finalization (Stage 3) | Runs dual sub-agents to review git diff against repository standards and ticket acceptance criteria. |
| **[[pr/SKILL\|pr]]** | Delivery & Communication | PR Creation (Stage 3) | Crafts an atomic, communicative pull request description linking Jira issues, Vault documentation, and verification evidence. |

---

## 4. Codebase Exploration & Guidance Skills (Matt Pocock Suite)

Exploration and guidance utilities for navigating unfamiliar parts of the repository and unblocking decisions.

| Skill | Purpose |
|---|---|
| **[[wayfinder/SKILL\|wayfinder]]** | Swift codebase exploration: identifies key files, callers, test layouts, and relevant schemas without reading unrelated directories. |
| **[[wait-what/SKILL\|wait-what]]** | Unravels confusing code, legacy traps, or hidden assumptions by explaining the mental model behind existing code. |
| **[[ask-matt/SKILL\|ask-matt]]** | Acts as an engineering mentor for design patterns, seam creation, and TypeScript/NestJS/Next.js idioms. |
| **[[setup-matt-pocock-skills/SKILL\|setup-matt-pocock-skills]]** | Initializer for repo configuration: issue tracker links, triage vocabulary, and domain doc layout. |

---

## 5. Jira Status Transitions Lifecycle

The engineering system strictly governs ticket status across the lifecycle:

```text
Intake & Grilling (Phases 0–5)           --> Initial State ("To Do" / "Selected")
Right before Implementation (Phase 7)   --> "In Progress"  (implement-jira-task)
Migration (Stage 2)                     --> "In Progress"  (migrate-outdated-practices)
Finalize Jira Task (Stage 3)            --> "Done"         (finalize-jira-task)
When PR comments arrive & Discuss runs  --> "In Review"    (discuss-pr-comments)
Finalize PR Comments                    --> "Done"         (finalize-pr-comments)
```

---

## 6. Adding New Skills

To add a new skill to the unified catalog:
1. Create a dedicated folder in `.agents/skills/<skill-name>/` containing a `SKILL.md` file with standard YAML frontmatter (`name`, `description`).
2. Create relative symlinks:
   - In `Vault/03 - Skills/`: `ln -s ../../.agents/skills/<skill-name> <skill-name>`
   - In `.claude/skills/`: `ln -s ../../.agents/skills/<skill-name> <skill-name>`
3. Add the skill entry and description to this `Vault/03 - Skills/README.md`.
