# BUSMORA — 16-Week Implementation Plan (Phase 1 and Phase 2)

> **Status:** proposed plan for review, revised once after a seven-lens Goldfish review and then on 2026-11-14 for the plan owner's decisions (DR-1, DR-2, DR-8, DR-12, DR-13): full scope in 16 weeks, zero overflow, hours unlimited. The 2026-11-14 revisions have not been reviewed by a fresh reviewer. Planning only: no code, infrastructure or Jira item has been created, and no task has started.
> **Authority:** [PRD v2.0](../core/prd.md) governs scope and acceptance. Approved technical decisions in `core/` are binding. The staffing and schedule in this plan (4 people, 16 weeks, hours not limited) replace the PRD's 5 people and 24 weeks by the plan owner's instruction.
> **"Plan owner"** in these files means the person who commissioned this plan and decides its open trade-offs.

## Verdict in four lines

1. All of Phase 1 and Phase 2 is scheduled inside 16 weeks: 99 tasks, 1,950 hours, zero unscheduled overflow.
2. The schedule is 64 primary tasks (four per week, 1,280 hours) plus 35 additional tasks T65–T99 (670 hours) that hold everything formerly in overflow. By the plan owner's instruction hours are unlimited; the schedule asks for about 30.5 hours per person per week on average and up to 45 in Week 11.
3. All 57 active acceptance scenarios are fully planned, and the plan targets a **release-ready Phase 2 at the end of Week 16**. The one dependency outside the team is Meta's approval of Facebook production access (AC-30).
4. Decided by the plan owner: no extension, no float, no swaps, absorb all scope, treat hours as unlimited. Still open: eight decisions (DR-3 to DR-7 and DR-9 to DR-11), none of which blocks the schedule.

## Files

| Deliverable | File |
|---|---|
| A. Assumptions and feasibility | [01-assumptions-and-feasibility.md](./01-assumptions-and-feasibility.md) |
| B. Phase and Epic roadmap, gates · C. 16-week schedule | [02-roadmap-and-schedule.md](./02-roadmap-and-schedule.md) |
| D. Epic descriptions (7) | [03-epics.md](./03-epics.md) |
| E. Task descriptions T01–T24 (Phase 1) | [04-tasks-phase-1.md](./04-tasks-phase-1.md) |
| E. Task descriptions T25–T52 and additional T65–T91 (Phase 2 build) | [05-tasks-phase-2-build.md](./05-tasks-phase-2-build.md) |
| E. Task descriptions T53–T64 and additional T92–T99 (Phase 2 verification and release) | [06-tasks-phase-2-release.md](./06-tasks-phase-2-release.md) |
| F. Dependency map and same-week independence checks | [07-dependency-map.md](./07-dependency-map.md) |
| G. Requirements coverage matrix | [08-requirements-coverage.md](./08-requirements-coverage.md) |
| H. Risks, former overflow register (now fully scheduled), decisions | [09-risks-overflow-decisions.md](./09-risks-overflow-decisions.md) |
| I. Review findings and revisions | [10-review-findings.md](./10-review-findings.md) |
| Skill brief | [../concepts_brief.md](../concepts_brief.md) |

## Definition of Done for every task

Applies to every task in addition to its own acceptance criteria. Copy it into each Jira item on import. The time is inside each estimate.

1. **Both languages.** Any owner-facing screen, message or error works in Arabic right-to-left and English left-to-right, at phone width and by keyboard.
2. **On staging.** From Week 4 on, the result runs on staging with its own secrets in the secrets store.
3. **Its own records.** A governed action writes its audit record, a costly call its usage record, and an event the owner must know about its notification record, using the T14 mechanism.
4. **Isolation.** A new store or module extends the T15 cross-workspace check.
5. **Ledger.** The task updates its rows in the release ledger and logs defects it leaves open in the defect register (both created in T07).
6. **Second person.** One teammate reviews the change and witnesses the completion evidence.
7. **No mock as evidence.** Acceptance is shown on the real implementation.

## Reading aids

**Two meanings of "task".** A capital-T **Task** is a product record: one unit of marketing work the owner asks the Director to do. A lowercase task, or an ID like T25, is a backlog item in this plan.

**Identifier prefixes.** FR, NFR, AC: PRD requirements and acceptance scenarios. ADR, OD, R, K, G: decisions, open decisions, risks, constraints and gates in SYSTEM-ARCHITECTURE. BTD-ADR, FTD-ADR: Backend and Frontend Technical Design decisions. D: decision registry. DS: Design System. OV, DR, XD, RK: overflow items, decisions required, external dependencies and risks in file 09.

**Glossary.**

| Term | Meaning |
|---|---|
| Director | The AI Marketing Director the owner talks to |
| Brain | The stored knowledge about the owner's business, each item labelled with its source and status |
| Candidate | A fact the Director proposes to remember; not used until the owner approves it |
| Canvas | The live view of what a Task is actually doing |
| Discovery | The saved set of questions that precedes Roadmap generation |
| Roadmap | The owner's approved step-by-step marketing plan |
| Deliverable | A document the product writes (report, plan, brief). Never a social post |
| Post | A piece of social content, kept in Scheduling |
| Register | A language variety for content: English, Egyptian Arabic, Gulf-oriented Arabic, Modern Standard Arabic |
| Recipe | A pre-configured kind of marketing task the Director can run |
| Launch cohort | The first target users: Arabic-first owner-run service businesses |
| Deployable / process role | The product ships as four deployables (web, API, workers, AI) running eight process roles: web, api-http, api-activities, workers-workflows, workers-publish, ai-http, ai-agent, ai-crawl |
| Tester mode | Meta's development access level; only people with a role on the app can connect a Page |
| Ordinary owner | A person with no role on BUSMORA's Meta app |
| Planning zone | The named time zone a Post's schedule was planned in |
| Lateness allowance | How long after its scheduled time a Post may still be sent |
| Authorization generation | A counter that advances when a Page is disconnected, so old approvals stop working |
| Two-slot admission | At most two Tasks run at once per workspace; others wait in order |
| Release ledger | One row per requirement and scenario with status and evidence |
| Overflow | Committed work that is not in the schedule. There is none since 2026-11-14; former items are listed in file 09 with their tasks |
| Additional task | One of T65–T99: work added on top of the four primary tasks of its week, assigned to a named person |

## Final validation against the planning constraints

| Constraint | Result |
|---|---|
| Exactly 2 phases | Met: Phase 1 Weeks 1–6, Phase 2 Weeks 7–16 |
| Exactly 16 weeks, 4 people | Met |
| 4 primary tasks per week, 64 in total, one per person per week | Met for the 64 primary tasks. **Not met as a whole:** Weeks 7–16 also hold 35 additional tasks, so those weeks have 5 to 10 tasks and each person carries two or three. This follows the plan owner's instruction to schedule all scope in 16 weeks |
| No Epic starts before its predecessor is complete | Met: 7 Epics in one chain, week-aligned |
| No task depends on another task in the same week | Met. 116 pairs checked one by one; the 30 tasks T70–T99 each checked against the rest of their week as a group (290 same-week pairs exist in all). Shared staging and shared code areas are coordinated, not dependencies |
| No hidden setup, integration, review or release work | Met in assignment: every piece of work is a task or a Definition of Done item. The Definition of Done hours were not added to the estimates |
| Comparable weekly difficulty supported by estimates | **Not met.** Weeks 1–6 total 80 hours each; Weeks 7–16 total 155, 100, 140, 165, 180, 155, 145, 154, 148 and 128 |
| User story and testable acceptance criteria for every task | Met in form. Some criteria refer to limits that T07, T16 and T21 will set |
| No implementation instructions inside tasks | Met |
| Full coverage of active in-scope commitments | **Met.** FR: 56 full, 1 externally gated. NFR: 9 full. AC: 56 full, 1 externally gated (AC-30). No partial, no unplanned |
| Explicit stabilization and release capacity | Met: 20 tasks and 430 hours in Weeks 14–16 (22%) |
| Honest treatment of external approvals | Met: Meta's approval is never scheduled; T63 records a blocked gate if it has not arrived |

**How the earlier conflict was settled.** Full scope, four tasks a week and 20 hours a week could not all hold. The plan owner kept full scope and the 16 weeks and removed the hour limit. The cost is recorded, not hidden: 1,950 scheduled hours, 5 to 10 tasks a week from Week 7, zero float, and 35 tasks whose estimates are coarser and unreviewed (risks RK-1, RK-2, RK-16, RK-17, RK-18 in file 09).
