# BUSMORA 16-Week Implementation Plan (Phase 1 and Phase 2)

* **Author:** engakhattab
* **Date:** 2026-11-14

## Overview

A planning-only backlog for building BUSMORA, an Arabic-first AI Marketing Director, with four people over 16 weeks. The plan covers the PRD's Phase 1 (foundation) and Phase 2 (first production-ready release) as two phases, seven sequential Epics and 99 tasks: 64 primary tasks, four per week, and 35 additional tasks (T65–T99) that hold everything the first draft could not fit. No task depends on another in the same week. By the plan owner's instructions of 2026-11-14 hours are unlimited, nothing is deferred, and the schedule totals 1,950 hours with zero unscheduled overflow. The full plan is in [planning/README.md](./planning/README.md).

The target readers are the team, its supervisors and whoever imports the backlog into Jira. Its value is an honest answer to one question: what can four people credibly finish in 16 weeks, and what is left.

## Analysis

**The scope did not fit the first capacity assumption, and the plan owner removed that assumption.** A bottom-up estimate gives about 1,950 hours at best and more likely up to 2,600. Four people at 20 hours a week give 1,280. The plan owner kept the 16 weeks and the full scope and set hours as unlimited, so all 1,950 hours are scheduled: about 30.5 hours per person per week on average, up to 45 in Week 11. All 57 active acceptance scenarios are fully planned and the plan targets a release-ready Phase 2 at Week 16; the Facebook production-access gate (AC-30) still depends on Meta.

**The contradictions the plan accepts, and what review found about each.**

1. *Full scope versus fixed capacity.* Resolved by an overflow register, not by cutting. Reviewers found the gap understated because every week was shaped to exactly 80 hours with no float.
2. *One sequential Epic chain versus four independent tasks per week.* This forces mixed "outcome slice" Epics and one dependency hop per week; 43 tasks start the week after their last prerequisite. Reviewers called the rule a cost driver in its own right.
3. *External approval versus a fixed end date.* The first draft submitted to Meta in Week 10. After review, business verification is filed in Week 1 and policy pages and production addresses are ready by Week 4, so the Week 10 submission is only the demonstration.
4. *A heavy approved architecture versus early owner value.* Phase 1 takes 24 of 64 tasks. Reviewers judged about six of them deferrable; that change is left to the plan owner.
5. *Verification at the end versus evidence as work finishes.* After review, the release ledger starts in Week 2 and every task fills its own rows.

**Secondary trade-offs.** A shared Definition of Done now carries staging deployment, notification, audit and usage records, isolation checks and ledger rows for every task; this removed hidden integration work but the estimates were not raised. Data controls the PRD says must not be cut (export, deletion across derived copies, context-change holds) were in overflow in the first draft, which two reviewers rated a blocker. They are now scheduled as T65–T69. The price is that Weeks 9–13 hold five tasks each, one person carries two in each of those weeks at 35 to 40 hours, and the four-tasks-per-week rule holds only for the 64 primary tasks.

**Ideal final result.** A narrower product that is complete on one owner path including its data controls, with float in the schedule and the Meta decision already in hand before the final weeks. The plan now has the data controls. It has no float and is not narrower, by the plan owner's decision; it is instead complete, at 1,950 scheduled hours.

## Goals vs. Non-Goals

* **In-Scope Goals:** a complete 16-week schedule; seven Epic descriptions; 64 task descriptions with user story, scope, acceptance criteria, prerequisites, independence check, estimate and risks; a dependency map; a coverage matrix for every active FR, NFR and AC; an overflow register with hours and release consequences; a list of decisions for the plan owner; a record of the review.
* **Out-of-Scope Non-Goals:** implementing anything; provisioning services; creating Jira issues; Phase 3, 4 and 5 work; changing the approved architecture; removing PRD commitments; promising any external approval date.

## Alternatives Weighed

* **Fit all 57 requirements into 64 tasks by making tasks thinner.** Rejected by author: it hides the gap inside under-sized tasks, which the request forbids.
* **Remove requirements to fit.** Rejected by author: the plan owner instructed that the timeline does not authorize removing scope.
* **Thematic Epics (one feature area each).** Rejected by author: four tasks from one feature area cannot run in the same week without depending on each other.
* **Roadmap work before publishing in Phase 2.** Rejected by author: it delays the Meta submission by about three weeks (DR-6).
* **Four weeks of stabilization, matching the PRD's 25%.** Rejected by author for the draft, because each task carries its own tests; kept open as DR-3.
* **Swap Notification Bell, document library and image posts for export, deletion and context holds.** Raised by reviewers and recommended by the author. Rejected by the plan owner: both sets stay, and the data controls are added on top (DR-2).
* **Plan 56 tasks with two buffer weeks.** Raised by reviewers. Rejected by the plan owner: zero float (DR-8).
* **Extend the schedule by 7 to 9 weeks.** Rejected by the plan owner: the timeline stays at 16 weeks (DR-1).
* **Slim Phase 1 and start publishing in Weeks 5–6.** Raised by reviewers; not applied because it moves a Post task into Phase 1. Open as DR-9.
* **Relax the same-week independence rule.** Raised by reviewers; not applied because it is a stated constraint. Open as DR-10.

## Key Decisions & Staging for Next Steps

**Decided in this session**
1. Baseline is 4 people, 16 weeks, 64 tasks, sequential Epics, strict same-week independence, 20 provisional hours.
2. PRD hour allocations are estimates to test; requirement counts do not set task counts.
3. PRD §13.3 simplifications are applied; nothing is silently removed; the architecture is unchanged.
4. Work that does not fit is listed as unscheduled overflow with hours, requirements and release consequences.
5. Unconfirmed facts (owner eligibility, legal entity, Meta app, budget, payer, dates, skills, reviewers) are recorded as open dependencies with early validation, never guessed.
6. This stage is planning only.
7. (Plan owner, 2026-11-14) DR-1: no extension; the workload is absorbed inside 16 weeks. DR-8: zero float, no reduction in tasks. DR-2: keep the Notification Bell, the document library and image posts, and add data export, deletion clean-up and context-change holds. Applied as additional tasks T65–T69.

**Waiting for the plan owner** (detail in [planning/09-risks-overflow-decisions.md](./planning/09-risks-overflow-decisions.md))
- None that blocks the schedule. DR-12 and DR-13 were decided on 2026-11-14: absorb all remaining scope; treat hours as unlimited.
- DR-3, DR-4, DR-5, DR-6, DR-7, DR-9, DR-10, DR-11.

**Next steps after review:** confirm the open dependencies in T03's register; then either revise the plan once more or run `/egm:egm-draft` if a formal design document for the delivery approach is wanted. Jira creation and implementation are separate requests.
