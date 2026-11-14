# A. Planning Assumptions and Feasibility Assessment

> Part of the [BUSMORA 16-week implementation plan](./README.md). Planning only: nothing here asserts that code exists or that a test has passed.
> Product scope and acceptance come from [PRD v2.0](../core/prd.md). Approved technical decisions in `core/` stay binding.

## A.1 Verdict

**The whole of Phase 1 and Phase 2 is scheduled inside 16 weeks. Unscheduled overflow is zero.** The plan targets a release-ready Phase 2 at the end of Week 16, with Meta's review of Facebook production access as the one dependency outside the team's control.

| Quantity | Hours | Tasks |
|---|---:|---:|
| Estimated work for the full release contract (central estimate) | 1,950 | 99 |
| Scheduled in the 64 primary tasks (four per week) | 1,280 | 64 |
| Scheduled in the 35 additional tasks T65–T99 | 670 | 35 |
| **Total scheduled** | **1,950** | **99** |
| **Unscheduled overflow** | **0** | **0** |

**Basis: the plan owner's instructions of 2026-11-14.** The timeline stays at 16 weeks with no buffer weeks (DR-1, DR-8). Nothing is swapped out or deferred (DR-2). Every remaining commitment is scheduled, and hours are to be treated as unlimited and not as a limit on scope (DR-12, DR-13). The earlier figure of 20 productive hours per person per week no longer bounds the plan.

**What the schedule asks for, stated as numbers.** 1,950 hours across four people and 16 weeks is about 30.5 hours per person per week on average. Weeks 1–6 stay at 80 hours each (20 per person). Weeks 7–16 total 155, 100, 140, 165, 180, 155, 145, 154, 148 and 128 hours, which is 25 to 45 hours per person per week. By assignee over the 16 weeks: A 505, B 491, C 460, D 494.

**Range.** 1,950 hours is the central estimate. Thirty-five tasks were added from coarser estimates (±35%), 16 primary and additional tasks are low confidence, and reviewers judged six tasks (T12, T25, T35, T36, T42, T59) larger than one person-week. With realistic overruns the total is more likely 1,950 to 2,600 hours. Under the plan owner's assumption, overruns are worked as extra hours inside the same weeks.

**Why the PRD's numbers differ.** The PRD and decision D-05 assumed five people for 24 weeks (2,400 hours). The plan owner's instruction for this plan is four people for 16 weeks with no hour limit. The scope did not change.

**What "release-ready at Week 16" means here.** Every active requirement and all 57 active acceptance scenarios have scheduled tasks with testable acceptance criteria, all 57 scenarios are run on the release candidate in Week 16 (T98), and the Epic exit review issues the release statement. It is a planned outcome, not a record that anything has been built or has passed. AC-30 passes only after Meta approves production access; if approval has not arrived, the release statement says so and names Meta as the outstanding party.

## A.2 Capacity calculation

| Item | Value | Status |
|---|---|---|
| Team size | 4 people (temporary assignees A, B, C, D) | Planning decision |
| Duration | 16 weeks, relative Weeks 1–16 | Planning decision |
| Hours | Not limited. Weeks 1–6 are planned at 20 per person; Weeks 7–16 at 25 to 45 per person | Plan owner's instruction, 2026-11-14 |
| Total | 1,950 scheduled hours over 64 person-weeks (about 30.5 hours per person per week) | Derived |
| Primary tasks | 4 per week, 64 in total | Planning decision |
| Additional tasks | 35 (T65–T99), in Weeks 7–16, 670 hours, each assigned to a named person on top of that person's primary task | Plan owner's instructions, 2026-11-14 |

What each task estimate must contain: reading the requirement and approved design, any research, implementation decisions inside approved constraints, implementation, tests, peer review given and received, integration with earlier work, expected fixes, and the acceptance demonstration.

About 2 hours of each person's week go to routine coordination: a short weekly planning check, reviewing one peer change, and the epic gate review. That time is inside every estimate. No work is planned outside the 64 primary tasks and the 35 additional tasks.

**Sensitivity.** The plan no longer has a capacity line to be sensitive to. What still matters is elapsed time: a task that is not finished in its week delays the tasks that depend on it (69 of 94 dependent tasks start the week after their last prerequisite). T03 still records real availability in Week 1 so that the team knows who can carry which additional task.

## A.3 How the work was estimated

The PRD's own allocation (2,400 hours, §13.1) was treated as a claim to test, not as proof. The estimate here was built from the required outcomes and the approved architecture:

1. List each coherent outcome an owner, developer or operator needs. One outcome may satisfy several requirements. Requirement counts did not set task counts.
2. Shape each outcome to about one person-week, including its own tests, review, integration and fixes.
3. Apply the simplifications PRD §13.3 already authorizes (A.4).
4. Sum the units, then compare with the PRD figure as a cross-check.

| Block | Tasks | Hours | Basis |
|---|---:|---:|---|
| Phase 1 foundation | 24 primary tasks | 480 | Two repositories, four deployables, eight process roles, two languages, durable workflows, isolation in five stores, staging, evaluation standard (SYS §§4, 9.4) |
| Phase 2 feature outcomes | 28 primary tasks + 27 additional tasks (T65–T91) | 1,020 | Intake, Brain, Chat, orchestration, Canvas, research, Roadmap, documents, Scheduling, publishing, analysis, Settings, notifications |
| Verification, stabilization, release | 12 primary tasks + 8 additional tasks (T92–T99) | 450 | PRD §10.2 gates, fault injection, security, language review, load, restore, rehearsal, defect repair, completion of the evaluation set |
| **Total** | **99 tasks** | **1,950** | |

The independent result is about 19% below the PRD's 2,400 hours. Two things explain the difference: the §13.3 simplifications, and tasks that carry their own tests instead of a separate integration workstream. The two estimates agree on the main point: the work is well above the 1,280 hours of four people at 20 hours a week, which is why the schedule now asks for 1,950.

**Why AI-assisted development does not close the gap.** Much of the cost is not typing code. It is external access (Meta review, provider accounts), cross-service integration, real-platform verification, human language review, and fault and recovery testing. These do not compress with code generation. No AI speed-up factor is applied anywhere in this plan.

## A.4 Simplifications applied (authorized by PRD §13.3)

| Authorized simplification | How it is applied | What stays committed |
|---|---|---|
| Focused graph views | The Brain is delivered first as the accessible List with full governance. The interactive graph is one focused-neighborhood view (T77). | Typed entities, relationships, epistemic labels, one stored record set |
| Fewer recipes | T48 delivers about six recipes instead of ten to twelve (the rest in T91). | Diagnosis contract, honest limits |
| Simpler calendar | Scheduling ships List and Unscheduled first. Month and Week are one bounded task (T78, T81). | One home for all Posts, exact-version approval |
| Bounded import layouts | Requested analysis supports the documented templates only (T72, T75). | Deterministic maths, unknown is not zero |
| Reuse of established components | The UI kit builds on the approved component base (FTD-ADR-12). | Bilingual, accessible, owned components |

No item in the "do not cut" list of §13.3 has been simplified or removed, and none is deferred: owner export, deletion across derived copies, context-change holds and language coverage of every screen are scheduled (T65–T69, T92). The simplifications above are the ones §13.3 authorizes; the remainder of each (the other recipes, the calendar views) is also scheduled.

## A.4.1 Deviations from dates in approved documents

| Approved timing | In this plan | Reason |
|---|---|---|
| Quick-reply latency measured in the walking skeleton (SYS §9.4 spike 2) | T18, Week 5 | Needs the model chosen in T08 and accounts from T09 |
| Embedding model chosen in Weeks 1–2 (D-39) | T24, Week 6 | Chosen where it is first used |
| Object storage vendor chosen in Weeks 1–2 (D-41) | T12, Week 3 | Chosen with the cloud provider |
| Shared bilingual components in Weeks 1–2 (PRD §11.1) | T02 Week 1, T10 Week 3 | Kit split across two tasks |

## A.5 Major estimation assumptions

| # | Assumption | If wrong |
|---|---|---|
| EA-1 | Hours are unlimited (plan owner's instruction). The schedule needs about 30.5 hours per person per week on average and up to 45 in Week 11 | Tasks slip past their week and the Week 16 release target is missed |
| EA-2 | Each person can work across stacks with ramp-up inside the task estimate. Expertise is **not** assumed equal. Assignments are provisional. | Tasks marked low confidence overrun first; the team should map A–D to people by strength at kickoff |
| EA-3 | The approved architecture is built as specified (SYS §4, ADR-002/003) | A simpler architecture would need its own decision (DR-4) |
| EA-4 | Paid service accounts and a payer exist from Week 1 | T04, T08, T12 and T18 stall; see dependency register in H |
| EA-5 | A legal entity able to pass Meta business verification exists or can be arranged | AC-30 cannot pass; Phase 2 stays blocked (PRD §3.1) |
| EA-6 | Reviewers for Egyptian, Gulf, MSA and English can be recruited by Week 13 | T56 records an unresolved quality gap for the missing register |
| EA-7 | A task that overruns is finished with extra hours in its own week | Dependants start late; there is zero float (DR-8) |
| EA-9 | Each person can carry two or three tasks in most of Weeks 7–16, including the second-person reviews the Definition of Done requires | Reviews are skipped or tasks slip (RK-16) |
| EA-8 | Requirements stay at PRD v2.0 | New scope must name its capacity source (PRD §13.3) |

## A.6 Estimate confidence

Each task carries a point estimate, a range and a confidence level. The primary tasks of every week total 80 hours; the additional tasks bring Weeks 7–16 to between 100 and 180 hours.

| Confidence | Meaning | Tasks |
|---|---|---|
| High | Familiar work, clear acceptance | 26 tasks |
| Medium | Some integration or research uncertainty | 53 tasks |
| Low | New integration, external party, or quality that depends on model behaviour | 20 tasks: T03, T06, T12, T19, T22, T25, T35, T36, T37, T38, T42, T48, T56, T63, and additional T65, T69, T75, T91, T95, T99 |

The 20 low-confidence tasks are where overruns are most likely. If half of them overrun by 50%, that is about 100 more hours, worked inside the same weeks.

**Estimates for T65–T99 are coarser.** They come from the former overflow register's central values (±35%), split into week-sized pieces. They were not shaped with the same care as the primary tasks and have not been reviewed by a fresh reviewer.

**Primary weekly totals equal 80 hours by construction.** Tasks were shaped to fit a week, so the point estimates are targets, not independent measurements. The range on each task is the honest part. Re-estimate from actual hours at Gate G1.

**Cross-cutting work inside every estimate.** The shared Definition of Done (staging deployment, notification, audit and usage records, isolation check, ledger rows, both languages) adds an estimated 2 to 3 hours to each feature task. It was added after review and the point estimates were not raised, which makes overruns more likely.

## A.7 Material uncertainties

1. **Hours actually worked.** The plan assumes unlimited hours (EA-1); the schedule's weekly totals show what that means in practice.
2. **Facebook production access** depends on Meta. The team controls the submission, not the approval or its timing.
3. **Intended-owner eligibility** is unconfirmed: no named owner, no confirmed legal entity, no confirmed existing Meta app.
4. **Budget and payment ownership** for managed services, model usage and search providers are unconfirmed.
5. **Team skill limits** are unconfirmed. The plan rotates people across stacks, which costs ramp-up time.
6. **Language reviewers**, especially a Gulf reviewer, are unconfirmed.
7. **Model behaviour** for diagnosis, research and four-register content is unmeasured until T04, T08 and T56 run.
8. **Exact calendar dates**, exams and holidays are unknown. Any lost week delays every task scheduled in it.

Each uncertainty has an early validation task and a stated consequence in [H](./09-risks-overflow-decisions.md).
