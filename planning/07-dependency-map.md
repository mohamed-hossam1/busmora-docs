# F. Dependency Map

> Part of the [BUSMORA 16-week implementation plan](./README.md). Task IDs are shortened; the full ID adds the Phase and Epic prefix.
> Rule checked here: a task may depend only on work completed in an earlier week or on approved documents. It may not depend on a task in its own week.

## F.1 Dependencies on earlier work

"Week gap" is the number of weeks between the latest prerequisite and the task. A gap of 1 means an overrun in the prerequisite delays this task directly.

| Task | Week | Depends on (all in earlier weeks) | Latest prerequisite week | Week gap |
|---|---:|---|---:|---:|
| T01 | 1 | — | — | — |
| T02 | 1 | — | — | — |
| T03 | 1 | — | — | — |
| T04 | 1 | — | — | — |
| T05 | 2 | T01, T02 | 1 | 1 |
| T06 | 2 | T01 | 1 | 1 |
| T07 | 2 | T04 | 1 | 1 |
| T08 | 2 | — | — | — |
| T09 | 3 | T01, T02, T05 | 2 | 1 |
| T10 | 3 | T02 | 1 | 2 |
| T11 | 3 | T06 | 2 | 1 |
| T12 | 3 | T01, T02, T06 | 2 | 1 |
| T13 | 4 | T09 | 3 | 1 |
| T14 | 4 | T09 | 3 | 1 |
| T15 | 4 | T06, T09 | 3 | 1 |
| T16 | 4 | T11, T12 | 3 | 1 |
| T17 | 5 | T09, T14, T15 | 4 | 1 |
| T18 | 5 | T06, T08, T09, T14, T15 | 4 | 1 |
| T19 | 5 | T03, T09, T12, T14 | 4 | 1 |
| T20 | 5 | T09, T10, T14 | 4 | 1 |
| T21 | 6 | T12, T14, T15 | 4 | 2 |
| T22 | 6 | T06, T08, T14, T18 | 5 | 1 |
| T23 | 6 | T07 | 2 | 4 |
| T24 | 6 | T08, T15, T17 | 5 | 1 |
| T25 | 7 | T18, T22, T24 | 6 | 1 |
| T26 | 7 | T09, T10, T14, T17, T18 | 5 | 2 |
| T27 | 7 | T10, T14, T19, T21 | 6 | 1 |
| T28 | 7 | T06, T12, T14, T15 | 4 | 3 |
| T29 | 8 | T20, T25, T27 | 7 | 1 |
| T30 | 8 | T10, T17, T26 | 7 | 1 |
| T31 | 8 | T19, T27 | 7 | 1 |
| T32 | 8 | T08, T26, T28 | 7 | 1 |
| T33 | 9 | T06, T22, T25 | 7 | 2 |
| T34 | 9 | T08, T24, T30 | 8 | 1 |
| T35 | 9 | T06, T12, T19, T31 | 8 | 1 |
| T36 | 9 | T04, T25, T28, T29 | 8 | 1 |
| T37 | 10 | T25, T34 | 9 | 1 |
| T38 | 10 | T03, T12, T16, T19, T35 | 9 | 1 |
| T39 | 10 | T35 | 9 | 1 |
| T40 | 10 | T10, T22, T25, T33 | 9 | 1 |
| T41 | 11 | T37 | 10 | 1 |
| T42 | 11 | T14, T36, T40 | 10 | 1 |
| T43 | 11 | T19, T35, T39 | 10 | 1 |
| T44 | 11 | T36 | 9 | 2 |
| T45 | 12 | T22, T25, T41 | 11 | 1 |
| T46 | 12 | T17, T24, T29 | 8 | 4 |
| T47 | 12 | T21, T31, T35 | 9 | 3 |
| T48 | 12 | T25, T29, T36 | 9 | 3 |
| T49 | 13 | T41, T45 | 12 | 1 |
| T50 | 13 | T17, T24, T26, T46, T65, T66 | 12 | 1 |
| T51 | 13 | T14, T22, T29, T31, T35, T36, T37, T39, T40, T43, T46 | 12 | 1 |
| T52 | 13 | T20, T29, T65, T66 | 10 | 3 |
| T53 | 14 | All built work T09–T52, T65–T69 | 13 | 1 |
| T54 | 14 | All owner-facing work T09–T52, T65–T68 | 13 | 1 |
| T55 | 14 | T31, T35, T39, T43, T47, T68 | 12 | 2 |
| T56 | 14 | T07, T23, T29, T36, T37, T48; external reviewers | 12 | 2 |
| T57 | 15 | T22, T25, T28, T36, T40, T42, T45 | 12 | 3 |
| T58 | 15 | T07, T12, T14, built workflows T18–T48 | 12 | 3 |
| T59 | 15 | All work T01–T56, T65–T69 | 14 | 1 |
| T60 | 15 | T12, T16, T19, T43, T69; the built product as frozen in Week 13 | 13 | 2 |
| T61 | 16 | Findings of T53, T54, T56, T57, T59 | 15 | 1 |
| T62 | 16 | Findings of T53, T54, T55, T57, T59 | 15 | 1 |
| T63 | 16 | T03, T38, T60; Meta's approval and an intended owner (external) | 15 | 1 |
| T64 | 16 | T11, T16, T55, T57, T59, T60 | 15 | 1 |
| **T65** (additional) | 9 | T14, T15, T16, T17, T20, T21, T24, T28, T29 | 8 | 1 |
| **T66** (additional) | 10 | T17, T24, T25, T26, T29, T31 | 8 | 2 |
| **T67** (additional) | 11 | T14, T15, T17, T18, T20, T21, T22, T27, T29, T35 | 9 | 2 |
| **T68** (additional) | 12 | T31, T35, T43, T66 | 11 | 1 |
| **T69** (additional) | 13 | T16, T43, T65 | 11 | 2 |
| **T70** (additional) | 7 | T09, T12, T13 | 4 | 3 |
| **T71** (additional) | 7 | T07, T23 | 6 | 1 |
| **T72** (additional) | 7 | T14, T15, T21, T22 | 6 | 1 |
| **T73** (additional) | 8 | T09, T13, T26, T27, T70 | 7 | 1 |
| **T74** (additional) | 9 | T25, T27, T29, T31 | 8 | 1 |
| **T75** (additional) | 9 | T25, T29, T72 | 8 | 1 |
| **T76** (additional) | 10 | T17, T29, T32, T74 | 9 | 1 |
| **T77** (additional) | 10 | T10, T17, T24, T26 | 7 | 3 |
| **T78** (additional) | 10 | T27, T31, T35 | 9 | 1 |
| **T79** (additional) | 10 | T34, T36 | 9 | 1 |
| **T80** (additional) | 11 | T33, T40 | 10 | 1 |
| **T81** (additional) | 11 | T31, T78 | 10 | 1 |
| **T82** (additional) | 11 | T29, T31, T39, T74 | 10 | 1 |
| **T83** (additional) | 11 | T37, T79 | 10 | 1 |
| **T84** (additional) | 11 | T34, T37, T66 | 10 | 1 |
| **T85** (additional) | 12 | T25, T29, T33 | 9 | 3 |
| **T86** (additional) | 12 | T20, T21, T24, T25, T29, T66 | 10 | 2 |
| **T87** (additional) | 12 | T27, T35, T39 | 10 | 2 |
| **T88** (additional) | 12 | T37, T41 | 11 | 1 |
| **T89** (additional) | 13 | T41, T45, T88 | 12 | 1 |
| **T90** (additional) | 13 | T17, T29, T46, T76 | 12 | 1 |
| **T91** (additional) | 13 | T36, T48, T71, T75 | 12 | 1 |
| **T92** (additional) | 14 | All owner-facing work T65–T91 | 13 | 1 |
| **T93** (additional) | 14 | T24, T25, T71, T91 | 13 | 1 |
| **T94** (additional) | 14 | All work T65–T91 | 13 | 1 |
| **T95** (additional) | 15 | T56, T71; external reviewers | 14 | 1 |
| **T96** (additional) | 15 | All work T65–T94 | 14 | 1 |
| **T97** (additional) | 15 | T65, T67, T72, T75, T79, T82, T89 | 13 | 2 |
| **T98** (additional) | 16 | T59, T60, T96, T97 | 15 | 1 |
| **T99** (additional) | 16 | Findings of T92, T94, T96, T97 | 15 | 1 |

**Standing prerequisites not repeated in every row.** From Week 3 on, every task uses the release ledger and defect register created in T07. T36, T37, T44, T48 and T56 take their demonstration cases and fixed source snapshots from T07 and T23. T53 uses the severity definitions from T07.

**Reading the table.** 69 of the 94 tasks with prerequisites have a week gap of 1. The plan has almost no float: a one-week overrun in a prerequisite usually delays its dependants by one week and pushes later work into overflow.

### Longest chains

| Chain | Tasks in order | Weeks |
|---|---|---|
| Publishing | T01 → T06 → T12 → T19 → T31 → T35 → T39 → T43 → T55 → T62 | 1 → 16 |
| Facebook access | T03 → T19 → T35 → T38 → (Meta's decision, external) → T63; T60 must also be complete before T63 | 1 → 16 |
| Director work | T01 → T05 → T09 → T14 → T18 → T22 → T25 → T29 → T36 → T42 → T57 | 1 → 15 |
| Roadmap | T17 → T26 → T30 → T34 → T37 → T41 → T45 → T49 | 5 → 13 |
| Intake | T26, T28 → T32 | 7 → 8 |

## F.2 Same-week independence checks

For each week: what exists at the start, why all four tasks can start on day one, the six task pairs, and any shared area that two tasks change and how blocking is avoided.

### Week 1 — T01, T02, T03, T04

**Available at start.** Approved documentation only. No repository, environment, account or provider access.
**Why all four can start.** T01 and T02 each create their own repository from nothing. T03 works in the platform's developer tools and with people. T04 runs standalone experiments.

| Pair | Dependency? | Reason |
|---|---|---|
| T01–T02 | None | Separate repositories; the shell shows no backend data |
| T01–T03 | None | T03 needs no code |
| T01–T04 | None | T04's experiments live outside the product repository |
| T02–T03 | None | — |
| T02–T04 | None | — |
| T03–T04 | None | T04 uses trial access it obtains itself; it does not wait for T03's budget finding |

**Shared changes.** None. If the team wants one source-hosting organization, whoever starts first creates it; that takes minutes and is not a deliverable of either task.

### Week 2 — T05, T06, T07, T08

**Available at start.** Runnable backend with checks (T01); bilingual shell (T02); Facebook record and dependency register (T03); research baseline (T04).
**Why all four can start.** Each builds on Week 1 outputs or on none.

| Pair | Dependency? | Reason |
|---|---|---|
| T05–T06 | None | T05 uses endpoints that existed in Week 1; T06 adds its own and does not use generated frontend types |
| T05–T07 | None | — |
| T05–T08 | None | — |
| T06–T07 | None | — |
| T06–T08 | None | T06 uses a stub step, not a real model |
| T07–T08 | None | T07's limits come from T04 and approved provisional targets; T08 uses its own prompt set, not T07's rubric |

**Shared changes.** T05 and T06 both touch the API's start-up code. Each change is additive; whichever merges second rebases. Neither needs the other's result.

### Week 3 — T09, T10, T11, T12

**Available at start.** Week 1 outputs; typed contract (T05); proven work path (T06); evaluation standard (T07); model selection (T08).
**Why all four can start.** T09 needs the repositories and contract; T10 the shell; T11 and T12 the work path.

| Pair | Dependency? | Reason |
|---|---|---|
| T09–T10 | None | T09's screens use the Week 1 base components |
| T09–T11 | None | — |
| T09–T12 | None | T09 is verified locally; it reaches staging later through the pipeline |
| T10–T11 | None | — |
| T10–T12 | None | — |
| T11–T12 | None | T11 is verified locally; enabling it on staging is in T16's scope |

**Shared changes.** T09 and T12 both add configuration for secrets. Under the shared Definition of Done, each task puts its own feature on staging with its own secrets once staging exists. T09 finishes before staging may exist, so T13, the next account task, confirms accounts and email on staging in Week 4. No task waits.

### Week 4 — T13, T14, T15, T16

**Available at start.** Accounts and workspaces (T09); component kit and forms (T10); tracing (T11); staging (T12).
**Why all four can start.** All build on accounts or staging from Week 3.

| Pair | Dependency? | Reason |
|---|---|---|
| T13–T14 | None | T14 is demonstrated on registration and verification events from T09, not on T13's new events |
| T13–T15 | None | T13 changes owner sessions; T15 covers service-to-service identity and stores |
| T13–T16 | None | — |
| T14–T15 | None | T14's records follow the approved scoping rule and carry their own cross-workspace check |
| T14–T16 | None | T16 writes policy from approved design; it does not change T14's records |
| T15–T16 | None | The drill restores staging as it stood at the start of the week |

**Shared changes.** T14 and T15 both add database changes to the single migration history. One schema per module (BTD-ADR-07) keeps them apart; ordering is settled at merge.

### Week 5 — T17, T18, T19, T20

**Available at start.** Phase 1 E01 and E02 complete: secure accounts, isolation, records mechanism, staging, policy.
**Why all four can start.** Each creates one new record family in its own module.

| Pair | Dependency? | Reason |
|---|---|---|
| T17–T18 | None | Replies do not read the Brain yet (T24, T25) |
| T17–T19 | None | — |
| T17–T20 | None | — |
| T18–T19 | None | — |
| T18–T20 | None | — |
| T19–T20 | None | — |

**Shared changes.** Four modules add database changes and one screen each under navigation destinations that already exist from T02. Migration ordering is settled at merge.

### Week 6 — T21, T22, T23, T24

**Available at start.** Knowledge records (T17), conversations and replies (T18), Page connection (T19), documents (T20).
**Why all four can start.** T21 needs staging and isolation; T22 needs conversations; T23 needs the standard; T24 needs knowledge records.

| Pair | Dependency? | Reason |
|---|---|---|
| T21–T22 | None | The Task's single step uses no files |
| T21–T23 | None | Case sources are stored with the cases, not through owner uploads |
| T21–T24 | None | — |
| T22–T23 | None | Cases are documents; nothing runs them yet |
| T22–T24 | None | The Task's single step uses no retrieval; context use starts in T25 |
| T23–T24 | None | — |

**Shared changes.** None of substance.

### Week 7 — T25, T26, T27, T28

**Available at start.** Gate G1 passed: all Phase 1 foundations.
**Why all four can start.** T25 needs Tasks and retrieval; T26 accounts, forms and knowledge records; T27 files and components; T28 the work path and isolation.

| Pair | Dependency? | Reason |
|---|---|---|
| T25–T26 | None | T25 reads eligible Brain items however they were created (T17's list is enough); it does not need onboarding |
| T25–T27 | None | T25 returns text; Posts from Chat come in T29 |
| T25–T28 | None | The crawl is not yet a research tool (T36) |
| T26–T27 | None | — |
| T26–T28 | None | **Reshaped to remove a dependency.** The onboarding entry does not offer Website import this week; T28 has its own route and stores observations only. T32 joins them in Week 8 |
| T27–T28 | None | — |

**Shared changes.** T26 and T25 both touch the Brain module (activation versus reading). Different behaviours; merge order only.

### Week 8 — T29, T30, T31, T32

**Available at start.** Orchestrated work (T25), confirmed manual setup (T26), Post drafts (T27), crawl observations (T28).
**Why all four can start.** Each extends one Week 7 output.

| Pair | Dependency? | Reason |
|---|---|---|
| T29–T30 | None | — |
| T29–T31 | None | T29 creates drafts through T27's Week 7 behaviour; approval applies to any draft whatever its origin |
| T29–T32 | None | — |
| T30–T31 | None | — |
| T30–T32 | None | Discovery reads Brain facts that exist at the start of the week |
| T31–T32 | None | — |

**Shared changes.** T29 and T31 both change the Post module (origin references versus approval). Additive on both sides; merge order only.

### Week 9 — T33, T34, T35, T36 and additional T65

**Available at start.** Typed results (T29), Discovery draft (T30), approval (T31), website prefill (T32).
**Why all four can start.** T33 needs recorded Task events; T34 the draft; T35 approval and connection; T36 orchestration and documents.

| Pair | Dependency? | Reason |
|---|---|---|
| T33–T34 | None | — |
| T33–T35 | None | — |
| T33–T36 | None | Both use the event contract approved in SYS §6.4 and in place since T06 and T22. T33 is accepted on drafting Tasks; research events need no Canvas change |
| T34–T35 | None | — |
| T34–T36 | None | Question-linked research is overflow (T79, T83) |
| T35–T36 | None | — |
| T65–T33 | None | Additional task. Clean-up acts on stores; the Canvas is not involved |
| T65–T34 | None | Same carrier (B). Discovery answers are not removed by T65 this week |
| T65–T35 | None | Posts and publication records are kept under the policy, not cleaned |
| T65–T36 | None | T65 is accepted on documents that existed at the end of Week 8; research reports made later are documents and follow the same behaviour without change |

**Shared changes.** T65 adds clean-up behaviour to the knowledge, document, file and crawl modules. No Week 9 task changes removal in those modules; merge order only.

### Week 10 — T37, T38, T39, T40 and additional T66

**Available at start.** P2-E04 exit met: real tester-mode publication, research, Canvas, adaptive Discovery.
**Why all four can start.** T37 needs adaptive Discovery; T38 the working flow; T39 the publish path; T40 Tasks and the Canvas.

| Pair | Dependency? | Reason |
|---|---|---|
| T37–T38 | None | — |
| T37–T39 | None | — |
| T37–T40 | None | With missing information T37 produces a preliminary plan; it does not raise clarification forms |
| T38–T39 | None | The submission shows the flow as it worked at the end of Week 9 |
| T38–T40 | None | — |
| T39–T40 | None | — |
| T66–T37 | None | Additional task. A proposed Roadmap is not marked by T66 this week; T66 is accepted on documents and Posts from Week 8 |
| T66–T38 | None | — |
| T66–T39 | None | T66 marks unpublished Posts and never changes a Post's publishing state; holds are T68 |
| T66–T40 | None | Same carrier (D). The review of marked work is an ordinary screen action, not a clarification form |

**Shared changes.** T66 adds a context record to Task results, documents and Posts. T39 changes the Post's publishing states and T40 the Task's waiting states: different fields; merge order only. Three tasks touch the Post or Task modules this week, which makes merges slower than in other weeks.

### Week 11 — T41, T42, T43, T44 and additional T67

**Available at start.** Proposed plan (T37), submission filed or blocker recorded (T38), reconciliation (T39), clarifications (T40).
**Why all four can start.** Each extends one Week 9 or Week 10 output.

| Pair | Dependency? | Reason |
|---|---|---|
| T41–T42 | None | — |
| T41–T43 | None | — |
| T41–T44 | None | — |
| T42–T43 | None | — |
| T42–T44 | None | **Split to avoid overlap.** T42 owns stopping because the allowance is used up; T44 owns stopping because evidence is sufficient, repetitive or unreachable. Neither needs the other |
| T43–T44 | None | — |
| T67–T41 | None | Additional task, same carrier (A). The export covers records that existed at the end of Week 10; an active Roadmap is not in it this week |
| T67–T42 | None | — |
| T67–T43 | None | The export shows a Post's state as recorded; it does not need the new held states |
| T67–T44 | None | — |

**Shared changes.** T42 and T44 both change the research capability. They touch different behaviours (accounting versus evidence handling); merge order only. T67 only reads other modules' records.

### Week 12 — T45, T46, T47, T48 and additional T68

**Available at start.** P2-E05 exit met: active Roadmap, bounded research, publishing recovery.
**Why all four can start.** T45 needs the active plan; T46 candidates from Task results; T47 the publish path; T48 orchestration and research.

| Pair | Dependency? | Reason |
|---|---|---|
| T45–T46 | None | — |
| T45–T47 | None | — |
| T45–T48 | None | Step execution uses orchestration as it stood at the end of Week 11 |
| T46–T47 | None | — |
| T46–T48 | None | Diagnosis proposes knowledge through the candidate behaviour in place since T29 |
| T47–T48 | None | — |
| T68–T45 | None | Additional task |
| T68–T46 | None | T68 is accepted on context changes made by the means in place at the end of Week 11. Changes that arrive through T46's approvals use the same behaviour and are checked in T55 |
| T68–T47 | None | Same carrier (C). T68 is accepted on text and link Posts; the hold does not depend on the Post's format |
| T68–T48 | None | — |

**Shared changes.** T47 and T68 both change the publishing path (media checks versus a hold check before dispatch), and one person carries both. T45 and T48 both touch how the Director is configured. T45 adds a launch origin; T48 adds analysis behaviour. Merge order only.

### Week 13 — T49, T50, T51, T52 and additional T69

**Available at start.** Step launch (T45), proposal review (T46), image posts (T47), diagnosis (T48).
**Why all four can start.** T49 needs step launch; T50 proposal review; T51 the events of earlier features; T52 documents.

| Pair | Dependency? | Reason |
|---|---|---|
| T49–T50 | None | — |
| T49–T51 | None | T51 lists no event introduced by T49 |
| T49–T52 | None | — |
| T50–T51 | None | Brain proposal notices link to T46's screen from Week 12 |
| T50–T52 | None | — |
| T51–T52 | None | "Document ready" links to the reading view in place since T20 and T29 |
| T69–T49 | None | Additional task |
| T69–T50 | None | T69 uses removals made through T65's behaviour, not through the new List screen |
| T69–T51 | None | — |
| T69–T52 | None | Same carrier (D). As T69–T50 |

**Shared environment.** T69's restore drill takes staging away from the other four tasks for its duration. The window is agreed on day one; the other tasks demonstrate before or after it.

**Shared changes.** T51 displays notification records that each earlier feature task wrote under the Definition of Done. If T51 finds a kind that is not being written, it records a defect against that feature; it does not build the missing behaviour. **Review finding accepted:** the first draft hid this as retrofit work inside T51.

### Week 14 — T53, T54, T55, T56

**Available at start.** P2-E06 exit met: the full baseline feature set, frozen.
**Why all four can start.** Each verifies a different property of the same built product.

| Pair | Dependency? | Reason |
|---|---|---|
| T53–T54 | None | Security versus language and access |
| T53–T55 | None | T53 covers credential and approval boundaries; T55 covers publishing faults |
| T53–T56 | None | — |
| T54–T55 | None | — |
| T54–T56 | None | T54 checks the interface; T56 checks generated content |
| T55–T56 | None | — |

**Shared changes.** Repairs by different tasks may touch the same screen or module. These are ordinary merge conflicts. Each task records its own findings in its own section of the findings record.

### Week 15 — T57, T58, T59, T60

**Available at start.** Week 14 findings and repairs.
**Why all four can start.** T57, T58 and T59 use staging; T60 builds production from T12 and T16.

| Pair | Dependency? | Reason |
|---|---|---|
| T57–T58 | None | **Shared environment.** Both stress staging. They use separate test workspaces and agreed time windows so that fault injection does not distort load measurements |
| T57–T59 | None | **Shared environment.** T59's reruns use their own test workspaces and time windows on staging. T57 adds its results to its own ledger rows; the ledger's structure dates from T07 |
| T57–T60 | None | Different environments |
| T58–T59 | None | T58 adds its results to its own ledger rows |
| T58–T60 | None | Load runs on staging, not production |
| T59–T60 | None | Acceptance runs on staging |

**Shared changes.** The ledger is one document edited by several tasks. Its structure was fixed in T07. T59 audits it; T57, T58 and T60 own the rows for their own results. Three tasks share staging this week: the time windows are agreed on day one inside the coordination allowance. This is the tightest week for shared resources in the plan.

### Week 16 — T61, T62, T63, T64

**Available at start.** Ledger with all findings to the end of Week 15; production environment rehearsed; Meta's decision if it has arrived.
**Why all four can start.** T61 and T62 work fixed, non-overlapping areas of the defect list; T63 uses production; T64 documents behaviour as of Week 15.

| Pair | Dependency? | Reason |
|---|---|---|
| T61–T62 | None | Area lists do not overlap |
| T61–T63 | None | — |
| T61–T64 | None | T64 writes runbooks for behaviour as of Week 15. A repair records any operator-facing change in its own record; T64 folds those notes in as its last step and does not wait for a repair to finish |
| T62–T63 | None | T62 works defects recorded by the end of Week 15; a defect found by T63 is recorded as a blocker, not added to T62 |
| T62–T64 | None | As T61–T64 |
| T63–T64 | None | — |

**Shared changes.** The ledger and runbooks, handled by row and entry ownership as in Week 15.

## F.2b Same-week checks for the additional tasks T70–T99

These tasks were added on 2026-11-14. Each row checks one additional task against **every** other task in its week (primary and additional) as a group; the pairs are not listed one by one. Every prerequisite of every task below is in an earlier week (see F.1).

| Week | Tasks in the week | Additional task | Depends on a same-week task? | Reason |
|---|---|---|---|---|
| 7 | T25–T28, T70, T71, T72 | T70 | No | Accounts only; prerequisites end in Week 4 |
| 7 | | T71 | No | Documents; nothing runs them this week |
| 7 | | T72 | No | Stores and reads a file; no Director work, so it does not need T25 |
| 8 | T29–T32, T73 | T73 | No | Reads values set by T26, T27 and T70 in Week 7 |
| 9 | T33–T36, T65, T74, T75 | T74 | No | Builds on typed results from Week 8; packages are drafts, so it does not need T35 |
| 9 | | T75 | No | Builds on T72 (Week 7) and T29 (Week 8); it is not a research Task, so it does not need T36 |
| 10 | T37–T40, T66, T76–T79 | T76 | No | Checks drafts; uses packages from T74 (Week 9); does not touch publishing states (T39) or T66's context record |
| 10 | | T77 | No | Reads knowledge records as of Week 9 |
| 10 | | T78 | No | Displays Post states as recorded; does not need T39's new states |
| 10 | | T79 | No | Starts a research Task from a question; T37 uses answers as saved and is unaffected |
| 11 | T41–T44, T67, T80–T84 | T80 | No | Canvas only; builds on T33 and T40 |
| 11 | | T81 | No | Builds on T78 (Week 10); does not need T43's held states or T82 |
| 11 | | T82 | No | Accepted on Post states as of Week 10; does not need T43 or T81 |
| 11 | | T83 | No | Works on a proposed plan (T37, Week 10); does not need approval (T41) |
| 11 | | T84 | No | Builds on T34, T37 and T66; independent of T83, which concerns research answers |
| 12 | T45–T48, T68, T85–T88 | T85 | No | Chat handoff; does not use step launch (T45) |
| 12 | | T86 | No | Offered from the reading view in place since T29 |
| 12 | | T87 | No | Published-Post actions; does not need image posts (T47) or T68's hold |
| 12 | | T88 | No | Builds on the active plan from T41 (Week 11); does not need step launch (T45) |
| 13 | T49–T52, T69, T89–T91 | T89 | No | Builds on T88 and T45 (Week 12); accepted on step states as of Week 12, so it does not need T49. Its notice is displayed by the Bell built this week; that display is confirmed in T96 |
| 13 | | T90 | No | Uses Pending Brain Updates (T46, Week 12), not the List screen (T50) |
| 13 | | T91 | No | Director configuration; builds on T48 (Week 12) |
| 14 | T53–T56, T92–T94 | T92 | No | Different screens from T54 |
| 14 | | T93 | No | Automated runs; does not use T56's reviewers or results |
| 14 | | T94 | No | Different feature set from T53 |
| 15 | T57–T60, T95–T97 | T95 | No | Builds on T56's findings (Week 14) |
| 15 | | T96 | No | Fills its own ledger rows; T59 audits other rows |
| 15 | | T97 | No | Different workflows and test workspaces from T57 |
| 16 | T61–T64, T98, T99 | T98 | No | Runs on the candidate as of the end of Week 15; repairs rerun the scenarios they touch |
| 16 | | T99 | No | Its defect area does not overlap T61 or T62 |

**Pairs among additional tasks in the same week** were also checked: no additional task needs another additional task from its own week. T76 uses T74, T81 uses T78, T83 uses T79, T89 uses T88, T93 uses T91, and each of those is one week earlier.

**Shared areas that need coordination, not sequencing.**
- Week 10: six tasks touch Posts, Tasks or the Brain. Changes are to different behaviours; merges will be frequent.
- Week 11: ten tasks run. T81 and T82 both change Scheduling; T83 and T84 both change Discovery.
- Week 13: T50 and T90 both change relationships in the Brain; T89 writes a notice kind the same week the Bell is built.
- Weeks 14 and 15: seven tasks share staging each week. Time windows are agreed on day one.
- Week 16: three repair tasks and a regression run share the candidate. T98 runs on the Week 15 candidate so that it does not wait for repairs.

## F.3 Result of the check

- 96 pairs among the primary tasks and 20 pairs involving T65–T69 were checked one by one. The 30 additional tasks T70–T99 were each checked against all other tasks of their week as a group (F.2b). With 99 tasks the weeks hold 4, 4, 4, 4, 4, 4, 7, 5, 7, 9, 10, 9, 8, 7, 7 and 6 tasks, which is 290 same-week pairs in all. No task depends on a same-week output.
- Two weeks needed reshaping to reach that result: Week 7 (onboarding entry versus website import) and Week 11 (budget stopping versus evidence stopping). Both are noted above.
- Same-week interactions that remain and are not deliverable dependencies: T57, T58 and T59 share the staging environment in Week 15; T61, T62 and T64 touch the same runbooks in Week 16.
- Weeks 7 to 16 hold more than four tasks, and each person carries two or three tasks in most of those weeks. A person's tasks in a week are independent of each other, so they may be worked in any order.
- Same-week interactions added by the additional tasks: three tasks touch the Post or Task modules in Week 10; one person changes the publishing path twice in Week 12; T69's restore drill takes staging in Week 13.
- No integration task exists outside the 64 primary and 35 additional tasks. Where two earlier outputs must be joined, a named later task does it: T05 (repositories), T16 (tracing on staging), T29 (Tasks with documents and Posts), T32 (crawl with the form). Cross-cutting joins (staging deployment, notification, audit and usage records, isolation checks, ledger rows) belong to each task through the shared Definition of Done.
- **Limit of this check.** It shows that no task needs a same-week output. It does not show that the rule is cheap: reviewers pointed out that it forces splits (T28/T32, T42/T44) and one dependency hop per week, which is why the Roadmap chain takes seven weeks. Relaxing the rule is your decision (DR-10).
