# H. Risks, Former Overflow and Decisions

> Part of the [BUSMORA 16-week implementation plan](./README.md). Planning only. DR-1, DR-2, DR-8, DR-12 and DR-13 were decided by the plan owner on 2026-11-14 and are applied. Every other trade-off below still waits for a decision.

## H.1 Former overflow register — now fully scheduled

**Unscheduled overflow: 0 hours.** On 2026-11-14 the plan owner instructed that every remaining commitment be scheduled inside the 16 weeks, with hours treated as unlimited. Each former overflow item is now one or more additional tasks. The item numbers are kept so that earlier references can be traced.

| Former ID | Work | Hours | Now scheduled as | Week | Requirements it completes |
|---|---|---:|---|---|---|
| OV-01 | Google sign-in and safe account linking | 30 | T70 | 7 | FR-50, FR-56; AC-32 |
| OV-02 | Interactive Brain graph (focused neighbourhoods) | 20 | T77 | 10 | FR-08, FR-12, NFR-01; AC-03 |
| OV-03 | Owner export of Brain and history | 20 | T67 | 11 | FR-13, NFR-04, NFR-09; AC-03, AC-15 |
| OV-04 | Deletion across derived copies, with restore suppression | 35 | T65, T69 | 9, 13 | FR-13, FR-42, NFR-05, NFR-09; AC-03, AC-15 |
| OV-05 | Context impact and freshness holds | 40 | T66, T68 | 10, 12 | FR-10, FR-35, FR-42, FR-48; AC-23, AC-47 |
| OV-06 | Working-preference proposals and claim/metric relations | 15 | T90 | 13 | FR-08, FR-10 |
| OV-07 | Canvas depth: related-Task history and late-event guards | 20 | T80 | 11 | FR-22; AC-42 |
| OV-08 | Remaining recipes and diagnosis tuning | 20 | T91 | 13 | FR-14, PRD §5.13 |
| OV-09 | Honest handoff contract for Chat requests | 15 | T85 | 12 | FR-49; AC-24 |
| OV-10 | Campaign packages, brand profile and claim-basis checks | 30 | T74, T76 | 9, 10 | FR-29, FR-30; AC-10 |
| OV-11 | Register tuning and second review round | 20 | T95 | 15 | FR-32, NFR-07; PRD §10.2 |
| OV-12 | Deliverables reuse, Continue with Director, CSV and media export | 15 | T86 | 12 | FR-11, FR-41; AC-15 |
| OV-13 | Calendar Month and Week, drag reschedule, daylight-saving checks | 30 | T78, T81 | 10, 11 | FR-55, NFR-01, NFR-07; AC-50, AC-52 |
| OV-14 | Bulk approval and Batch Review sessions | 20 | T82 | 11 | FR-34, FR-55; AC-12, AC-51 |
| OV-15 | Duplicate published Post and local-delete handling | 10 | T87 | 12 | FR-55; AC-53 |
| OV-16 | Exact-question research and Use in Roadmap | 30 | T79, T83 | 10, 11 | FR-17, FR-52; AC-37 |
| OV-17 | Strategic revisions with diff and atomic approval | 30 | T88, T89 | 12, 13 | FR-16, FR-54; AC-04, AC-45 |
| OV-18 | Knowledge reuse across a later Discovery | 10 | T84 | 11 | FR-51; AC-38 |
| OV-19 | Requested CSV and .xlsx analysis | 50 | T72, T75 | 7, 9 | FR-21, FR-38–40, NFR-04; AC-14, AC-25 |
| OV-20 | Settings: account and providers, region, language defaults | 20 | T73 | 8 | FR-44, FR-56; AC-11, AC-29 |
| OV-21 | Localization and accessibility for the added screens | 30 | T92 | 14 | FR-43, NFR-07; AC-11 |
| OV-22 | Evaluation completion: 10 more cases, Brain versus document-only runs, held-out reporting | 40 | T71, T93 | 7, 14 | PRD §10.1–10.2; D-23, D-38 |
| OV-23 | Release verification of the added features, final regression, repair of remaining critical and high defects | 120 | T94, T96, T97, T98, T99 | 14–16 | PRD §10.2; AC-17 |
| | **Total moved into the schedule** | **670** | 35 additional tasks, T65–T99 | | |
| | **Total unscheduled** | **0** | | | |

**Performance tuning.** Runtime and cost ceilings at the declared load are measured in T58 (Week 15). Tuning needed to meet a ceiling is repair work in T61, T62 and T99 (Week 16), which under the unlimited-hours assumption continue until critical and high defects are closed.

**What the estimates still do not contain.** Rework after a Meta rejection; overruns of low-confidence tasks; defects beyond the hours in T61, T62 and T99; a landing page (in the Design System, not a PRD requirement). Under the plan owner's assumption these are worked as extra hours inside the 16 weeks; they are not deferred.

**Hours this schedule asks for.** 1,950 hours in total against the earlier provisional 1,280 (4 people × 16 weeks × 20 hours). That is about 30.5 hours per person per week on average: 20 in Weeks 1–6, and between 25 and 45 in Weeks 7–16, with the peak in Week 11 (180 hours across four people). These are the plan's numbers, recorded as facts, not as a reason to remove anything.

## H.2 Decisions required from you

| ID | Decision | Options | What the draft assumes until you decide |
|---|---|---|---|
| **DR-1** | How to handle the 670-hour gap | (a) Extend by about 8–9 weeks at the same staffing. (b) Add capacity: more hours per person or more people; 25 hours a week over 16 weeks closes about half. (c) Remove named commitments through a documented release-contract decision (PRD §13.3). (d) Accept Week 16 as a verified partial increment and do not claim a Phase 2 release | **Decided 2026-11-14.** No extension: the timeline stays at 16 weeks, and extra workload is absorbed inside it. Applied so far to the 95 hours of data controls (T65–T69). Then, by the decision on DR-12, applied to all remaining scope: the schedule holds 1,950 hours and targets a release-ready Phase 2 at Week 16 |
| **DR-2** | Which work is in the baseline and which is overflow | (a) The draft: one owner path (context → Chat work → documents and Posts → governed publishing) plus a thin Roadmap loop; data controls in overflow. (b) Swap the six Roadmap tasks (112 h) for OV-03, OV-04, OV-05 and OV-01 (125 h). (c) Reviewers' proposal: keep the Roadmap loop and fund OV-03, OV-04 and OV-05 (95 h) by dropping from the baseline T51 (Notification Bell), T52 (library), T47 (image posts) and by trimming release ceremony (T58, owner help in T64). Reviewers argued (c) gives a narrower product that is complete on the "do not cut" items | **Decided 2026-11-14: neither swap.** The Notification Bell (T51), the library (T52) and image posts (T47) stay, and OV-03, OV-04 and OV-05 are added to the 16 weeks as T65–T69 (95 h). The reviewers' blocker is answered: the "do not cut" data controls are scheduled. The cost is 95 hours above capacity (RK-16) |
| **DR-7** | What happens if T03 finds no legal entity for Meta verification | AC-30 would be unreachable. Options: continue publishing work to tester-mode depth only and move T39, T43, T47, T55 behind data controls; or stop and resolve the entity first | Publishing continues as drafted |
| **DR-8** | Whether to build float into the baseline | The draft fills all 64 slots with zero float. Reviewers recommend planning about 56 tasks and leaving two buffer weeks (after Gate G1 and before feature freeze), which moves about eight more tasks to overflow but makes the plan survivable | **Decided 2026-11-14: zero float.** No buffer weeks and no reduction in tasks |
| **DR-12** | How far "absorb the workload" goes | — | **Decided 2026-11-14: absorb all of it.** Every remaining overflow item is scheduled inside the 16 weeks as T70–T99. Unscheduled overflow is 0 hours. Capacity is not to be treated as a limit |
| **DR-13** | Whether the additional hours are available | — | **Decided 2026-11-14: assume unlimited hours.** The plan assigns every additional task to a named assignee and does not cap weekly hours |
| **DR-9** | Slimmer Phase 1 and earlier publishing | Reviewers judged Phase 1 heavier than needed: T13, T16's drill, T21 and T23 do not block Week 7. Moving T21 to Week 5 and T27 to Week 6, and letting unaffected tasks pass a partial Gate G1, would bring the Meta submission forward to Week 9 or earlier. It puts a Post task inside Phase 1, which changes the phase boundary | Phase 1 as drafted; strict gate |
| **DR-10** | Whether to keep strict same-week independence | The rule forces splits and one dependency hop per week. Allowing pairing, two-week tasks or mid-week handoffs would shorten the Roadmap and publishing chains | Strict rule, as you set |
| **DR-11** | When human review starts | The plan has two review rounds, in Week 14 (T56) and Week 15 (T95). Reviewers recommended a small early round on T08 outputs in Week 2 and on T29 outputs around Week 10, if reviewers can be found that early | Two rounds, Weeks 14 and 15 |
| **DR-3** | Share of the schedule for verification and stabilization | Weeks 14–16 now hold 20 tasks and 430 of 1,950 hours (22%). The PRD reserved 25% (6 of 24 weeks) | 22%, with every build task also carrying its own tests |
| **DR-4** | Whether to request an architecture simplification | Phase 1 costs 24 tasks (37.5% of capacity) because of the approved architecture. A separate decision could consider fewer deployables or deferring a store. Savings are unestimated and unproven | Architecture unchanged, as you instructed |
| **DR-5** | Phase boundary for email verification | Built in Phase 1 with the account (T09), though the PRD lists verification links under Phase 2 | Phase 1 |
| **DR-6** | Order of Phase 2 Epics | The draft starts publishing in Week 7 so the Meta submission happens in Week 10. Starting Roadmap first would delay submission by about three weeks | Publishing early |

## H.3 Open assumptions and external dependencies

None of these is confirmed. Each has an early validation task. "Latest safe date" is the week after which a named task is delayed.

| # | Dependency | Validated by | Latest safe date | If unresolved |
|---|---|---|---|---|
| XD-1 | Real weekly availability of each person | T03, Week 1 | Week 1 | Capacity changes by 16 tasks per 5 hours |
| XD-2 | Exact start and end dates, exams, holidays | T03, Week 1 | Week 1 | Each lost week removes four tasks |
| XD-3 | Budget and payer for cloud, durable workflow service, vector index, model and search usage, email delivery, tracing | T03; needed by T04, T08 (Weeks 1–2), T12 (Week 3) | Week 2 | T12 cannot complete; Gate G1 slips |
| XD-4 | A legal entity able to pass Meta business verification | T03, Week 1 | Week 9 | T38 cannot submit; AC-30 blocked; Phase 2 cannot be complete |
| XD-5 | Whether a Meta developer app already exists and who administers it | T03, Week 1 | Week 4 | T19 delayed |
| XD-6 | An intended ordinary owner with a real Page and no app role | T03 identifies; T63 uses | Week 15 | AC-30 cannot be evidenced even with approval |
| XD-7 | Meta production access approval | Submitted in T38, Week 10. **Not controlled by the team; no date promised** | — | AC-30 fails; FR-33 not met; Phase 2 incomplete |
| XD-8 | Language reviewers for four registers, including a Gulf reviewer for a named country | T03 identifies, T07 states the need | Week 13 | T56 records the register as an unresolved gap |
| XD-9 | Team skill limits across the two languages, the frontend and operations | T03 (availability and skills collected), then the plan owner maps A–D to people | Week 1 |
| XD-11 | A named person to open and pay for each account | T03 lists them; the plan owner confirms the payer | Week 1 | Low-confidence tasks overrun; cross-stack rotation costs more than estimated |
| XD-10 | Launch cohort and one reachable owner scenario | T07, Week 2 | Week 6 | Cases and recipes target the wrong business type |

**Affected release gates.** XD-4, XD-6 and XD-7 gate AC-30 and FR-33. XD-8 gates the per-register usefulness gate. XD-3 gates Gate G1 and the load measurement in T58.

## H.4 Risk register

| # | Risk | Likelihood | Effect | Response in the plan |
|---|---|---|---|---|
| RK-1 | The schedule needs 1,950 hours, about 30.5 per person per week on average and up to 45 in Week 11. The estimate could reach 2,600 with overruns | Certain (it is the plan) | If the hours are not worked, tasks slip past Week 16 and the release target is missed | The plan owner has set hours as unlimited. Hours worked are recorded weekly against this schedule so that a slip is visible in the week it happens |
| RK-2 | No slack: 69 of 94 dependent tasks start the week after their last prerequisite, and there is zero float | High | One late prerequisite delays its dependants | Low-confidence tasks named; a late task is finished with extra hours in its own week, by the plan owner's assumption |
| RK-3 | Orchestration (T25) overruns | High | T29, T36 and everything after slip | Scope held to drafting and analysis specialists with fixed ceilings |
| RK-4 | The cross-language durable path (T06) fails in Week 2 | Medium | Phase 1 extends; DR-4 becomes urgent | Scheduled as early as the repository allows |
| RK-5 | Meta rejects or delays the submission | High | AC-30 blocked | Path found in Week 1; submission in Week 10; honest blocked record in T63 |
| RK-6 | No legal entity or intended owner | Unknown | AC-30 cannot pass | Escalated from T03 in Week 1 |
| RK-7 | Arabic and regional source coverage is thin | Medium | Research gives partial results | Measured in T04; gaps reported to the owner by design |
| RK-8 | Model quality misses 80% in a register | Medium | Usefulness gate fails | Measured in T56; tuned and measured again in T95; a miss after T95 is stated in the release summary |
| RK-9 | Reviewers unavailable | Medium | Register quality unmeasured | Recorded as an unresolved gap, never as a pass |
| RK-10 | Unknown defect volume in Week 16 | High | Critical defects stay open | Time-boxed repair by severity; remainder listed as blockers |
| RK-11 | Running cost of managed services and model usage | Unknown | Work stops if unpaid | Cost recorded in T12, T08, T58 |
| RK-12 | Rotating people across stacks slows work | Medium | Estimates low | Assignments are provisional; swap within a week is free |
| RK-13 | Verification is compressed into three weeks for 99 tasks of work | Medium | Evidence thin or late; defects found in Week 16 have little time | 20 verification and repair tasks in Weeks 14–16; every build task carries its own tests; DR-3 |
| RK-14 | Month-long scheduled work breaks on an incompatible deploy | Low in 16 weeks | Held or lost scheduled Posts | Rehearsed in T60 (SYS R-05) |
| RK-15 | Data removed by an owner persists in derived copies if T65 or T69 is late | Medium | A "do not cut" data control unmet | Both scheduled (Weeks 9 and 13) and verified in T94 and T60 |
| RK-16 | Sustained load in Weeks 7–16: each person carries two or three tasks in most weeks; Weeks 10–12 hold 9, 10 and 9 tasks | High | Quality and review suffer; second-person review (Definition of Done item 6) is the first thing squeezed | Assignees named for every task; weekly check that each finished task had its second-person review; no task is dropped silently |
| RK-17 | Many tasks change the same areas in the same week (Posts, Scheduling, Discovery, the Brain), and seven tasks share staging in Weeks 14 and 15 | High | Merge conflicts and blocked demonstrations | Same-week tasks are independent by design; staging windows agreed on day one of each week |
| RK-18 | The 30 tasks added on 2026-11-14 were shaped from coarse overflow estimates (±35%) and have not been reviewed by a fresh reviewer | Certain | Estimates and same-week independence for T70–T99 are less reliable than for T01–T64 | Re-estimate at Gate G1; a review of the revised plan can be run before Jira import |

## H.5 Remaining planning gaps

1. Estimates are not calibrated against this team's real throughput. Re-estimate at Gate G1 using six weeks of actuals.
2. The estimates for the additional tasks T65–T99 are coarser than those for T01–T64; they were derived from the former overflow register.
3. Numeric limits (uploads, rate limits, retention durations, latency ceilings) are set inside T07, T16 and T21 and do not exist yet.
4. Reviewer time is outside the planned hours; T03 names who recruits reviewers.
5. The plan does not schedule owner sessions for commercial learning (PRD §12.2); they are not a graduation prerequisite.
