# BUSMORA — 16-Week Implementation Plan (Phase 1 and Phase 2)

> **Status:** approved execution strategy, harmonized on 2026-11-15. Four people deliver all 99 tasks in 16 weeks using AI coding agents and LLM tooling. Full scope is committed; unscheduled overflow is zero.
> **Authority:** [Execution contract](./11-execution-contract.md) records the project owner's current directives and overrides conflicting delivery/publishing assumptions in older source documents. [PRD v2.0](../core/prd.md) remains the product-scope baseline subject to those explicit overrides. This is a plan, not a claim that implementation or tests are complete.

## Verdict in four lines

1. Phase 1 and Phase 2 are fully feasible and committed within 16 weeks under the owner-confirmed, benchmarked 3×–5× AI-assisted execution model: 99 tasks, seven Epics, four people, zero scope reduction.
2. The 1,950-hour traditional manual-effort baseline becomes 390–650 human developer hours. Additional DoD and human-review allowances bring the planning envelope to 577.4–877 hours, leaving capacity for repair and integration.
3. Publishing uses Scrapling adaptive parsing and Playwright with clients' encrypted browser sessions, dashboard reauthentication and durable duplicate-prevention/read-back controls. All 57 active scenarios, including AC-30, are fully scheduled without a Meta API approval gate.
4. Staging deployment, dogfooding, quality tuning and cumulative regression happen every week. Week 16 completes stabilization, polish and certification of the final repaired build.

## Problem, goals and phase outcomes

BUSMORA helps an owner turn business context into useful marketing work, inspect its evidence, approve content and publish it under their control. The plan delivers the complete Phase 1/2 feature set with bilingual operation, data controls and truthful execution states.

Phase 1 (Weeks 1–6) establishes delivery tooling, accounts, isolation, durable records, browser connection, model evaluation and retrieval. Its current increment is deployed and exercised every week. Phase 2 (Weeks 7–16) completes Director work, Brain governance and graph, Roadmap, research, documents, analysis, Scheduling, browser publishing, Settings and notifications, with continuous validation and final release certification.

Later-phase products already excluded by the PRD remain outside this plan. No Phase 1 or Phase 2 feature is removed, swapped out or deferred.

## Files

| Deliverable | File |
|---|---|
| A. Capacity, estimation and feasibility | [01-assumptions-and-feasibility.md](./01-assumptions-and-feasibility.md) |
| B–C. Roadmap, gates and schedule | [02-roadmap-and-schedule.md](./02-roadmap-and-schedule.md) |
| D. Seven Epics | [03-epics.md](./03-epics.md) |
| E. T01–T24 | [04-tasks-phase-1.md](./04-tasks-phase-1.md) |
| E. T25–T52 and T65–T91 | [05-tasks-phase-2-build.md](./05-tasks-phase-2-build.md) |
| E. T53–T64 and T92–T99 | [06-tasks-phase-2-release.md](./06-tasks-phase-2-release.md) |
| F. Dependencies and integration handoffs | [07-dependency-map.md](./07-dependency-map.md) |
| G. Full requirements mapping | [08-requirements-coverage.md](./08-requirements-coverage.md) |
| H. Technical risks and resolved decisions | [09-risks-overflow-decisions.md](./09-risks-overflow-decisions.md) |
| I. Review resolutions | [10-review-findings.md](./10-review-findings.md) |
| J. Authority, architecture, weekly validation and evidence rules | [11-execution-contract.md](./11-execution-contract.md) |
| Consolidated resolution report | [consolidated_review_report.md](../report/consolidated_review_report.md) |
| Final readiness and repository clearance | [final-readiness-and-execution-clearance.md](../report/final-readiness-and-execution-clearance.md) |

## Architecture and alternatives

The approved topology uses four repositories: `busmora-web` (Next.js), `busmora-api` (NestJS), `busmora-ai` (FastAPI, AI activities and Python workflow/publishing workers), and `busmora-docs` (documentation). The three code repositories produce four deployables (web, API, workers, AI) and eight process roles: web, api-http, api-activities, workers-workflows, workers-publish, ai-http, ai-agent and ai-crawl. Next.js calls the public NestJS API; Python Temporal workflows coordinate activities, and FastAPI remains internal AI/retrieval compute. NestJS is the sole domain writer. Only the publishing worker decrypts browser sessions and executes exact-version approved Posts through Scrapling/Playwright; the research crawler has no publishing credentials.

Authority links: [System Architecture](../core/SYSTEM-ARCHITECTURE.md), [Backend Design](../core/BACKEND-TECHNICAL-DESIGN.md), [Frontend Design](../core/FRONTEND-TECHNICAL-DESIGN.md), [Design System](../core/DESIGN-SYSTEM.md). The [execution contract](./11-execution-contract.md#architecture-and-resolution-status) records the alternatives and current decision provenance, including the replacement of API-gated publishing.

## Definition of Done for every task

Baseline task estimates cover implementation and ordinary tests; the [capacity model](./01-assumptions-and-feasibility.md) adds an explicit allowance for cross-cutting work and weekly human review.

1. **Both languages.** Each owner-facing screen, message and error works in Arabic RTL and English LTR, at phone width and by keyboard.
2. **Weekly staging.** Every week's completed increment deploys to staging and is dogfooded. T01/T02 provide bootstrap deployment; T12 hardens it.
3. **Applicable records.** Actions changing owner data, authorization or execution state are audited; metered provider/tool calls record usage; actionable owner events record notifications. [T14](./04-tasks-phase-1.md#p1-e02-t14--audit-usage-and-notification-records) supplies the shared mechanism and integrates the earlier account paths in Week 4.
4. **Isolation.** Every new store or module extends [T15](./04-tasks-phase-1.md#p1-e02-t15--workspace-isolation-proven-in-every-store). Earlier account work includes local isolation checks that T15 incorporates.
5. **Evidence.** Each task updates the release ledger and defect register established in [T07](./04-tasks-phase-1.md#p1-e01-t07--evaluation-standard-frozen). T07 imports earlier notes. Evidence names the build, cases, results and follow-up owner.
6. **Second person.** A teammate reviews the AI-assisted change and its evidence. The weekly plan assigns that reviewer.
7. **Real implementation.** Mocks support development but do not certify end-to-end acceptance. Each repair reruns affected checks; the final build receives all 57 scenarios under T98.
8. **Feedback.** Weekly dogfooding and language/content checks drive refinements inside the full committed scope.

The [Week-1 completion contract](./11-execution-contract.md#week-1-completion-contract) makes T01–T04's deployment, experiment evidence and applicability rules explicit. Inputs are provisioned progressively by milestone, not as a front-loaded kickoff checklist. The [runtime and publishing contracts](./11-execution-contract.md#runtime-boundaries-and-durable-handoffs) govern recovery, duplicate prevention and session ownership across services.

## Reading aids

A capital-T **Task** is one product record of marketing work. A lowercase task or T01–T99 is a backlog item. The **plan owner** approves delivery directives; a product **owner** controls a business account and authorizes its work. A **workspace** is the isolated business/tenant boundary for data, credentials, usage and Task admission.

| Term | Meaning |
|---|---|
| Brain | Stored business knowledge with source, epistemic status and lifecycle |
| Director | AI Marketing Director that responds to owner requests |
| Candidate | Proposed knowledge awaiting owner confirmation |
| Canvas | View of recorded activity for the selected conversation's Tasks |
| Discovery / Roadmap | Saved planning questions / approved actionable marketing plan |
| Deliverable / Post | A versioned document / social content managed in Scheduling |
| Register | English, Egyptian Arabic, Gulf-oriented Arabic or Modern Standard Arabic |
| Browser session | Encrypted owner-authorized authentication state isolated to a workspace |
| Authorization generation | Counter advanced on disconnect to invalidate earlier dispatch authority |
| Planning zone / lateness allowance | Scheduling time zone / allowed delay before a Post must be held |
| Two-slot admission | At most two running product Tasks per workspace; others wait |
| Release ledger | Requirement/scenario status with actual evidence and build reference |
| Additional task | T65–T99, already assigned inside the 16-week schedule |
| Manual-equivalent hour | Traditional effort unit retained in the task tables; not a human-hour demand |

FR/NFR/AC identifiers refer to the PRD. ADR/OD identifiers refer to the linked architecture and technical designs; DR/XD/RK/OV identifiers are defined in [file 09](./09-risks-overflow-decisions.md).

## Final validation against the planning constraints

| Constraint | Result |
|---|---|
| Two phases, 16 weeks, four people | PASS — unchanged |
| All 99 tasks T01–T99, all Phase 1/2 scope | PASS — fully committed, zero unscheduled overflow |
| 64 primary plus 35 additional tasks | PASS — same task identifiers, owners and scheduled weeks |
| AI-assisted capacity | PASS — owner-confirmed benchmarked 3×–5× throughput; full schedule validated |
| Parallel delivery | PASS — domain/module isolation, schema ownership and automated PR linting are settled |
| Publishing delivery | PASS — Scrapling/Playwright, encrypted sessions, dashboard recovery, fail-closed deduplication and verified read-back; no API-approval dependency |
| Epic sequencing and dependencies | PASS — earlier-week startup dependencies plus explicit same-week integration/certification handoffs |
| Staging and value validation | PASS — every week from Week 1, with growing functionality and recorded feedback |
| Quality and regression | PASS — per-change checks, weekly cumulative suites and final post-repair T98 certification |
| Full requirements mapping | PASS — 57 FR, 9 NFR and 57 AC fully planned; retired IDs excluded |
| Internal operations | PASS — accounts, keys and servers provisioned week-by-week; commercial setup and optional dialect feedback remain parallel |
| Release evidence | Required at execution — planning PASS is not a record of successful software tests |
