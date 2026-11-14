# I. Review Findings and Revisions

> Part of the [BUSMORA 16-week implementation plan](./README.md).
> Method: the EGM brainstorm skill's Goldfish review. Seven reviewers, each in a fresh context with no access to the drafting conversation, read the plan files and the source documents. They were read-only.

## I.1 Reviewer status

| Reviewer lens | Status | Blockers | Majors | Nits |
|---|---|---:|---:|---:|
| Design and sequencing | Completed | 2 | 8 | 1 |
| Technical accuracy against sources | Completed | 0 | 9 | 3 |
| Scope and simplicity | Completed | 2 | 6 | 2 |
| Naive-reader comprehension | Completed (read 01–03, 09 and 39 of 64 tasks) | 1 | 10 | 1 |
| Implementation readiness | Completed | 2 | 9 | 1 |
| Document structure | Completed | 0 | 6 | 8 |
| Inventive catalyst | Completed (7 reshaping ideas) | — | — | — |

All seven first stopped on a usage limit before reporting and were resumed; each then returned a full report. Findings overlap across lenses; they are merged below.

Checks the reviewers confirmed: 64 tasks, 16 weeks, one task per person per week, 80 hours every week, 1,280 hours in total, every template field on every task and Epic, the 670-hour overflow sum, every cited decision identifier exists and means what the plan says, and no retired scope is reintroduced.

## I.2 Findings accepted and changes made

| # | Finding (lenses) | Severity | Change made |
|---|---|---|---|
| 1 | Isolation was to be "proven in every store" in Week 4, before the vector index, object storage and knowledge tables held data (design, readiness) | Blocker | T15 now delivers the mechanism and a check run on real account data and synthetic records; T17, T21 and T24 extend it to real data; Gate G1-4 cites all four |
| 2 | The production restore rehearsal relied on a record of disconnections and holds that no task built (design, readiness) | Blocker | T43 now keeps that protected record; T60 reapplies it and lists T19 and T43 as prerequisites; restore suppression for deleted knowledge is stated as not evidenced |
| 3 | Nobody owned opening accounts or confirming a payer; four early tasks each assumed it (readiness) | Blocker | T03 now lists every account with a named responsible person and needed-by week, and escalates a missing payer as a stop condition; new dependency XD-11 |
| 4 | Reviewer recruitment had no owner (readiness) | Blocker | T03 names the person responsible; T23 delivers a reviewer list by Gate G1 and reports any register with no reviewer |
| 5 | README missing; no glossary; "Task" versus "task"; "you" undefined; opaque identifier prefixes (comprehension, structure) | Blocker | README written with glossary, prefixes, the two meanings of "task" and "plan owner" |
| 6 | T51 hid notification writing for about ten event kinds across eight modules (design, readiness) | Major | Each feature task writes its own notification records under the shared Definition of Done; T51 is the Bell, links and a completeness check |
| 7 | Staging deployment and secrets for each new feature were unowned (design, readiness) | Major | Definition of Done item 2; T13 confirms accounts and email on staging |
| 8 | T12 too large and missing the key-management service and object storage that T19, T15 and T28 need (design, readiness) | Major | Both added to T12's scope, the storage vendor decision moved from T21, the product domain added; T12 reclassified Low confidence. **The size problem is not solved**, see I.3 |
| 9 | Workspace time zone was used by T31 but delivered by no task; T27 offered a target Page without depending on T19 (readiness) | Major | Planning zone added to T27 with a criterion; T19 added as a prerequisite |
| 10 | Meta clock started later than needed: business verification and policy pages need no working flow; submission was planned against staging addresses (design, readiness, inventive) | Major | Business verification filed in T03 (Week 1); policy pages in T16 (Week 4); production addresses reserved in T12; T38 submits with those addresses |
| 11 | Ledger built in Week 15 although the PRD says "maintain"; defect register undefined; T57, T58, T60 wrote to a structure T59 created the same week (comprehension, structure, inventive, scope) | Major | T07 creates the ledger and defect register; every task fills its rows (Definition of Done item 5); T59 becomes an audit |
| 12 | T33 cited ADR-024 but excluded retry masking, contradicting an approved decision (accuracy) | Major | Retry presentation moved into T33 with a criterion; AC-43 and FR-23 now Full; OV-07 narrowed |
| 13 | Coverage summary counts wrong; FR-14, FR-35, FR-51, FR-57 and AC-55 wrongly rated Full (accuracy, structure) | Major | Counts corrected; the five rows changed to Partial with their overflow items. FR: 25 full, 26 partial, 5 unplanned, 1 gated. AC: 33, 16, 7, 1 |
| 14 | Feasibility verdict understated and falsely precise: estimates fitted to 80 hours, zero float, range applied only to overflow (scope) | Blocker | A.1 now states 670 hours as the low end with a likely range of 670–1,300; A.6 states that weekly totals are 80 by construction; RK-1 updated |
| 15 | A.4 claimed nothing on the "do not cut" list was simplified while three such items sat in overflow (scope, design) | Blocker | A.4 corrected to say so plainly; DR-2 now carries the reviewers' alternative |
| 16 | Capacity basis (4 people, 16 weeks) departs from approved D-05 without a cited authority (accuracy) | Major | A.1 and README cite the plan owner's instruction |
| 17 | Three tasks share staging in Week 15, not two; T61, T62 and T64 share runbooks in Week 16 (design, readiness, structure) | Major | Dependency map updated for both weeks; F.3 no longer claims a single shared-resource case |
| 18 | Dependency table omitted prerequisites stated in task text; "longest chains" showed edges that are not dependencies (structure, accuracy) | Major | T27, T51, T60, T64 rows corrected; standing prerequisites noted; chains redrawn |
| 19 | T31 or T35 silently carried the release workflow start (design) | Major | T31 now owns starting the workflow at submission and the expiry of unapproved Posts |
| 20 | The bilingual rule lived only in a file header and would be lost on import (comprehension) | Major | Moved into the Definition of Done |
| 21 | T52 title conflicted with the Epic boundary on export (comprehension, structure) | Major | T52 retitled "document export"; the Epic boundary names Brain and history export |
| 22 | P1-E03 exit criteria only pointed at the gate; T24 had no exit criterion (structure) | Major | Three criteria added |
| 23 | Timing deviations from approved documents unstated (accuracy) | Nit | Table A.4.1 added |
| 24 | T63 "spare hours go to T61 and T62" contradicted their fixed time box (comprehension, accuracy) | Nit | Reworded |

## I.3 Findings not applied — they need the plan owner's decision

These would change scope priority, the phase boundary, the 64-task baseline or the same-week rule. They are recorded as decisions in [H.2](./09-risks-overflow-decisions.md).

| Finding (lenses) | Why it was not applied | Decision |
|---|---|---|
| The baseline funds a Notification Bell, a document library and image posts while export, deletion clean-up and context holds are in overflow. Two reviewers rated this a blocker and proposed swapping them (scope, design) | Changes which commitments are built first | DR-2, option (c). I recommend accepting it |
| Plan about 56 tasks with two buffer weeks instead of 64 with zero float (scope) | Changes the 64-task baseline you set | DR-8 |
| Phase 1 is heavier than needed; pull the publishing chain one to two weeks earlier and relax the strict Gate G1 (scope, design, inventive) | Puts a Post task in Phase 1 and changes the gate rule | DR-9 |
| The same-week independence rule manufactures splits and one hop per week (scope) | It is a rule you set | DR-10 |
| Start human review in Week 2 and around Week 10 instead of once in Week 14 (scope, inventive) | Depends on reviewers who are unconfirmed | DR-11 |
| No branch if no legal entity exists for Meta verification (scope) | Needs your choice of branch | DR-7 |
| Verification weeks spend about 94 hours on release ceremony for a release that cannot happen, and only 46 on repair (scope) | Trading T58 and T60 for repair and data controls is part of DR-2 and DR-3 | DR-2, DR-3 |

## I.4 Unresolved issues

1. **Several tasks are probably larger than one person-week**: T12, T25, T35, T36, T42 and T59 were each named by at least one reviewer. They were not split, because splitting adds tasks and the baseline is fixed at 64. Their ranges and Low confidence state the risk; the real fix is DR-8.
2. **The Definition of Done adds work that the point estimates do not include** (about 2 to 3 hours per feature task).
3. **Acceptance criteria still point at limits that do not exist yet** (latency targets, upload limits, session limits, the accessibility level). They are set in T07, T16 and T21. Criteria that say "provisional target" refer to SYS §2.2.
4. **Some wording remains hard to test without a fixture**: "simple request" (T25), "normal small business site" (T28), "material finding" (T36). The evaluation cases from T07 and T23 are the intended fixtures; the tasks do not yet name which case.
5. **User stories for T53, T55, T57, T61 and T62 name the business owner.** One reviewer argued the true beneficiary is whoever makes the release decision. Left as written: the owner is who is harmed if the check is skipped. Your call.
6. **Retrofit risk for context holds.** T25, T29 and T35 do not record which context version work used, so OV-05 will have to add it later.
7. **The comprehension reviewer read 39 of 64 tasks**; the other 25 were covered by the structure, accuracy and readiness reviewers but not for readability.
8. **The plan was reviewed once.** The revisions in I.2 have not themselves been reviewed by a fresh reviewer.

## I.5 Plan owner's first decisions applied after review (2026-11-14) — superseded in part by I.6

| Decision | What the plan owner decided | Change made |
|---|---|---|
| DR-1 (the gap) | Keep 16 weeks; do not extend; absorb the workload inside the 16 weeks | Timeline unchanged. Absorption applied to the work named under DR-2 (95 h). The other 575 hours stay unscheduled; whether they are also to be absorbed is new decision DR-12 |
| DR-8 (float) | Zero buffer weeks; do not reduce tasks | Schedule unchanged. RK-2 updated |
| DR-2 (baseline content) | No swap. Keep the Notification Bell, the document library and image posts, and add data export, deletion clean-up and context-change holds | OV-03, OV-04 and OV-05 moved into the schedule as five additional tasks: T65 (Week 9), T66 (Week 10), T67 (Week 11), T68 (Week 12), T69 (Week 13). T50, T52, T55 and T60 now use or verify them. Nine coverage rows changed to Full |

**What this resolved.** Review findings I.3 row 1 and I.2 row 15: the "do not cut" data controls are no longer in overflow. Unresolved issue 6 (retrofit risk for context holds): T66 adds the context record in Week 10.

**What this cost, stated so it is not hidden.**
1. The schedule is 1,375 hours against a stated capacity of 1,280. Weeks 9–13 hold five tasks each, and one person carries two.
2. The rules "exactly four tasks per week" and "one task per person per week" hold for the 64 primary tasks only.
3. The estimates for T65–T69 come from the overflow register and are coarser than the others.
4. Verification tasks T53, T55, T59 and T60 gained scope without added hours (RK-17).
5. Unresolved issue 1 is worse, not better: the six oversized tasks are unchanged and there is still no float.
6. **These changes have not been reviewed by a fresh reviewer.** The five new task descriptions, the 20 new same-week pair checks and the changed coverage ratings are the author's work only.

## I.6 Plan owner's instruction of 2026-11-14 (second): full scope, zero overflow, hours unlimited

| Instruction | Change made |
|---|---|
| Do not treat hours or capacity as a limit; assume unlimited hours | The 20-hour figure no longer bounds the plan. Weekly totals and hours per assignee are recorded as facts in files 01 and 02 |
| Move every remaining overflow item into the 16 weeks | The 20 remaining items (575 h) became 30 additional tasks, T70–T99: 22 build tasks in Weeks 7–13 and 8 verification and release tasks in Weeks 14–16. With T65–T69 the plan has 35 additional tasks and 99 in all. Unscheduled overflow is 0 |
| All 57 acceptance scenarios fully planned; Week 16 release-ready, external approvals noted | Every FR, NFR and AC row is Full; FR-33 and AC-30 remain "Full — externally gated" on Meta. Gate G3, the Epic exit criteria and T98 now target a release statement at Week 16 |
| Update all planning files | Files 01 to 09 and the README updated; overflow references in task text now point to task IDs |

**Stated so it is not hidden.**
1. The schedule totals 1,950 hours: about 30.5 per person per week on average, 25 to 45 in Weeks 7–16.
2. Weeks 7–16 hold 5 to 10 tasks. "Four tasks per week, one per person" holds only for the 64 primary tasks.
3. T70–T99 were shaped from coarse overflow estimates. Their same-week independence was checked by group (file 07, F.2b), not pair by pair.
4. "Fully planned" and "release-ready" are planned outcomes. Nothing has been built or tested.
5. **None of the 2026-11-14 changes has been reviewed by a fresh reviewer.** Unresolved issues 1, 2, 3, 4 and 7 in I.4 still stand; issue 1 (oversized tasks, no float) is unchanged.
