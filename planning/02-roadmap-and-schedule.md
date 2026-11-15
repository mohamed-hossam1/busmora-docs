# B. Phase and Epic Roadmap · C. 16-Week Schedule

> Authority: [execution contract](./11-execution-contract.md). All hours below are manual-equivalent baseline hours; apply the owner-confirmed, benchmarked 3×–5× multiplier and allowances in file 01. Accounts, keys and servers are provisioned progressively with their consuming milestones, not as kickoff gates.
> Part of the [BUSMORA 16-week implementation plan](./README.md). Task detail: [Phase 1](./04-tasks-phase-1.md), [Phase 2 build](./05-tasks-phase-2-build.md), [Phase 2 release](./06-tasks-phase-2-release.md).

## B.1 Phases

| Phase | PRD meaning (§3) | Weeks | Tasks | Hours |
|---|---|---|---:|---:|
| **Phase 1 — Foundation** | Platform, data, integration and operational foundations needed by Phase 2 | 1–6 | 24 | 480 |
| **Phase 2 — First production-ready release** | Owner confirms context and independently uses Chat, Roadmap, documents and governed publishing | 7–16 | 40 primary + 35 additional | 1,470 |

Phase 1 establishes the complete foundation over six weeks while every weekly increment is deployed and dogfooded. Phase 2 delivers all owner features, with expanded verification in Weeks 14–15 and stabilization/polish in Week 16. The 35 additional tasks are fully committed alongside the primary tasks.

**Interpretation recorded for your review.** The PRD lists "email/password verification links" under Phase 2 and "account foundation" under Phase 1. This plan builds the verification link with the account in Phase 1 (T09), because an unverified account is not a secure foundation. Google sign-in stays Phase 2 scope and is task T70 in Week 7.

## B.2 Sequential Epic chain

One scheduled Epic chain; every week belongs to one Epic. Validate exit criteria continuously and use the allocated weekly repair effort before activating dependent capabilities. Independent preparation can continue; a failed technical check is never marked passed to preserve a date.

| Order | Epic | Phase | Weeks | Tasks | Blocked by |
|---|---|---|---|---|---|
| 1 | **P1-E01** Delivery base and early evidence | 1 | 1–2 | T01–T08 | None |
| 2 | **P1-E02** Secure accounts, isolation and operable environment | 1 | 3–4 | T09–T16 | P1-E01 |
| 3 | **P1-E03** Durable records for knowledge, conversation, work, documents and connection | 1 | 5–6 | T17–T24 | P1-E02 |
| 4 | **P2-E04** First integrated owner slice | 2 | 7–9 | T25–T36, T65, T70–T75 | P1-E03 and the Phase 1 exit gate |
| 5 | **P2-E05** Recoverable publishing, bounded research and an approved Roadmap | 2 | 10–11 | T37–T44, T66, T67, T76–T84 | P2-E04 |
| 6 | **P2-E06** Step execution and owner control of knowledge, documents and notices | 2 | 12–13 | T45–T52, T68, T69, T85–T91 | P2-E05 |
| 7 | **P2-E07** Verification, stabilization and release readiness | 2 | 14–16 | T53–T64, T92–T99 | P2-E06 |

Phase 2 Epics are outcome slices, not technology layers. A slice mixes intake, Chat, Roadmap and publishing work across the settled domain/module boundaries and schema ownership, supporting parallel AI-assisted execution.

## B.3 Gates

### Gate G1 — Phase 1 exit (end of Week 6)

| # | Criterion | Evidence |
|---|---|---|
| G1-1 | A developer new to the project starts the whole product locally from the repository instructions; automated checks run on every proposed change in all three code repositories | T01, T02, T05 |
| G1-2 | Staging runs every process role, deployed by the automated pipeline; one restore drill and one rollback drill passed with recorded timings | T12, T16 |
| G1-3 | An owner registers, verifies by link, signs in, recovers a password and signs out; a reused session credential revokes the session | T09, T13 |
| G1-4 | Cross-workspace access attempts fail in the database, vector index, object storage, cache/streams and the internal AI service, on real data | T15 (mechanism and check), extended by T17, T21, T24 |
| G1-5 | Knowledge, conversation, Task, document, file, connection, audit, usage and notification records persist with workspace attribution and truthful states | T14, T17, T18, T19, T20, T21, T22 |
| G1-6 | A quick message gets a real, metered model reply; a substantive request becomes a durable Task that survives a worker restart; a third concurrent Task waits | T18, T22 |
| G1-7 | A client-controlled Page is connected through an isolated encrypted Scrapling/Playwright session; expiry, disconnect and dashboard reauthentication behavior is verified | T03, T19 |
| G1-8 | Evaluation standard frozen; 14 authored cases exist; model and research-provider baselines recorded | T04, T07, T08, T23 |
| G1-9 | Weekly staging/dogfooding evidence, session ownership and technical integration responsibilities are recorded; internal ops proceeds in parallel | T03, T07, weekly task owners |

Gate checks run during the week so repairs and reruns use the allocated AI-assisted delivery capacity. Technical prerequisites must pass before dependent activation. The integration lead owns any within-week reassignment and records the result. The 16-week full-scope commitment remains unchanged.

### Gate G2 — Epic transitions inside Phase 2

Each Epic's exit criteria are in [D. Epics](./03-epics.md). The gate review is a 30-minute check held inside the coordination allowance on the last day of the Epic.

### Gate G3 — Phase 2 exit (PRD §3.1 and §10.2)

Phase 2 completion requires real evidence for all 57 active scenarios and the applicable technical quality controls. AC-30 uses the client-session browser interpretation in [file 11](./11-execution-contract.md#publishing-through-authenticated-browser-sessions). T98 certifies all scenarios on the final repaired build, with T63 production evidence for that same build. No API-access decision or internal commercial activity gates completion.

### Browser publishing milestones

| Milestone | Owner | Week |
|---|---|---:|
| Scrapling/Playwright adaptive baseline, session workflow and authorized experiment | T03 | 1 |
| Encrypted connection and Page selection | T19 | 5 |
| Scheduled text/link publication with read-back | T35 | 9 |
| Production browser/session hardening | T38 | 10 |
| Recovery, reconnect and holds | T39, T43 | 10–11 |
| Single-image publishing | T47 | 12 |
| Browser fault and isolation verification | T53, T55 | 14 |
| Client-session production verification | T63 | 16 |

### Weekly staging and validation

Every week ends with staging deployment, dogfooding of the available increment, recorded value feedback and cumulative regression. Bootstrap staging is part of T01/T02; T12 hardens it. The weekly focus and evidence fields are defined in [file 11](./11-execution-contract.md#weekly-staging-and-value-validation). Each outcome in the schedule below includes this recurring activity.

## C. Complete 16-week schedule

Task IDs are shortened here; the full ID adds the Phase and Epic prefix (for example `P1-E01-T01`). Hours are traditional manual-effort point estimates, not human-hour requirements.

**Additional tasks T65–T99.** By the plan owner's decisions of 2026-11-14, every commitment that was in the overflow register is in the schedule as an additional task, on top of the four primary tasks of its week. Each is marked **+** in the cell of the person who carries it. Weekly totals are: Weeks 1–6, 80 hours each; Week 7, 155; Week 8, 100; Week 9, 140; Week 10, 165; Week 11, 180; Week 12, 155; Week 13, 145; Week 14, 154; Week 15, 148; Week 16, 128. The schedule totals 1,950 hours and there is no unscheduled overflow.

| Week | Phase | Epic | Task A | Task B | Task C | Task D | Weekly Outcome |
|---|---|---|---|---|---|---|---|
| 1 | 1 | P1-E01 | **T01** Backend runs locally with automated checks (22 h) | **T02** Bilingual app shell in its own repository (20 h) | **T03** Browser-session publishing baseline and operational ownership (18 h) | **T04** Research source and provider baseline (20 h) | All three code repositories supply the bootstrap staging increment; browser/session and research experiments begin |
| 2 | 1 | P1-E01 | **T05** Published API contract with typed frontend access (16 h) | **T06** Durable cross-language work path proven end to end (24 h) | **T07** Evaluation standard frozen (20 h) | **T08** Arabic model baseline across four registers (20 h) | The riskiest technical path works once; quality gates and model choice are fixed |
| 3 | 1 | P1-E02 | **T09** Owner account with email verification and session (22 h) | **T10** Shared bilingual components and structured forms (18 h) | **T11** Operator can trace a request across services (18 h) | **T12** Staging environment with automated deployment (22 h) | An owner can sign in; bootstrap staging is hardened; failures are traceable |
| 4 | 1 | P1-E02 | **T13** Account recovery and session security (20 h) | **T14** Audit, usage and notification records (18 h) | **T15** Workspace isolation proven in every store (22 h) | **T16** Data lifecycle policy and first restore and rollback drill (20 h) | Accounts are safe to build on; tenant isolation and recovery are demonstrated |
| 5 | 1 | P1-E03 | **T17** Knowledge records with provenance and lifecycle (20 h) | **T18** Conversations with real quick replies (22 h) | **T19** Facebook Page connection with protected browser sessions (20 h) | **T20** Versioned document records (18 h) | The owner gets a real reply; knowledge, documents and a Page connection persist |
| 6 | 1 | P1-E03 | **T21** Owner file storage with validation (18 h) | **T22** Durable Task lifecycle with two-slot admission (24 h) | **T23** Evaluation case library, first 14 cases (18 h) | **T24** Retrieval of eligible context only (20 h) | Work survives restarts; context can be retrieved safely. **Gate G1** |
| 7 | 2 | P2-E04 | **T25** Director performs orchestrated work from Chat (24 h) | **T26** Manual setup, confirmation and Brain activation (20 h) **+** T70 Google sign-in and safe linking (30 h) | **T27** Manual Post drafts and Unscheduled list (16 h) **+** T71 Evaluation library completed to 24 cases (20 h) | **T28** Website import crawl with truthful outcomes (20 h) **+** T72 Spreadsheet upload read safely (25 h) | First owner features: confirmed context, real Director work, Post drafts |
| 8 | 2 | P2-E04 | **T29** Task results become documents and Posts (22 h) | **T30** Roadmap Discovery draft saved and resumable (18 h) **+** T73 Account, region and language settings (20 h) | **T31** Exact-version Post approval and invalidation (20 h) | **T32** Website findings prefill the editable draft (20 h) | Website and manual intake share one form; Chat produces documents and Posts; approval binds exact versions |
| 9 | 2 | P2-E04 | **T33** Authentic conversation-local Canvas (20 h) **+** T74 Editable campaign package (15 h) | **T34** Adaptive Discovery questions (16 h) **+ T65** Removed data cleaned from every derived copy (20 h) | **T35** Approved Post published for real at its scheduled time (22 h) | **T36** Research report with inspectable citations (22 h) **+** T75 Owner numbers interpreted against a goal (25 h) | Scheduled browser publication to a client-authorized Page; research and Canvas work on real records |
| 10 | 2 | P2-E05 | **T37** Complete proposed Roadmap generated from Discovery (22 h) **+** T76 Brand profile and claim checks (15 h) | **T38** Production browser-session hardening (18 h) **+** T77 Interactive Brain graph (20 h) | **T39** Uncertain publication reconciled, never resent (20 h) **+** T78 Calendar Month and Week views (15 h) | **T40** Choice-first clarification pauses and resumes work (20 h) **+ T66** Context change flags the work that used it (20 h) **+** T79 Research on an exact Discovery question (15 h) | Browser-session production behavior verified; uncertain publishes are safe; a full plan can be proposed; outdated work is marked |
| 11 | 2 | P2-E05 | **T41** Roadmap approval, single active plan and step view (18 h) **+ T67** Owner exports the Brain and history (20 h) **+** T82 Bulk approval and Batch Review (20 h) **+** T84 Later Discovery reuses earlier answers (10 h) | **T42** Research budget with owner-approved extensions (22 h) **+** T80 Canvas related Tasks and late events (20 h) | **T43** Overdue, held, cancelled and disconnected publishing (22 h) **+** T81 Drag rescheduling and daylight-saving correctness (15 h) | **T44** Research evidence quality, stopping and untrusted sources (18 h) **+** T83 Research answer returns to wizard and Roadmap (15 h) | Publishing recovery complete for text and link posts; research is bounded and grounded; one Roadmap is active |
| 12 | 2 | P2-E06 | **T45** Run a Roadmap step with the Director (20 h) **+** T85 Honest handoff from Chat (15 h) | **T46** Pending Brain Updates review (20 h) **+** T88 Strategic revision proposed with diff (15 h) | **T47** Single-image posts with checks before approval (18 h) **+ T68** Context change holds affected scheduled Posts (20 h) **+** T87 Duplicate published Post, local delete (10 h) | **T48** Constraint diagnosis and curated recipes (22 h) **+** T86 Reuse of past work and remaining exports (15 h) | Roadmap steps execute; AI proposals need owner approval; both committed post formats publish |
| 13 | 2 | P2-E06 | **T49** Owner-reported Done, Skip and Revisit (18 h) **+** T89 Revision approved and applied atomically (15 h) | **T50** Brain List governance (22 h) **+** T90 Working preferences and proof and metric relations (15 h) | **T51** Notification Bell with exact links (20 h) **+** T91 Remaining recipes and diagnosis tuning (20 h) | **T52** Deliverables library with versions and document export (20 h) **+ T69** A restore never brings back removed data (15 h) | Owner controls steps, knowledge, documents and notices. **Feature freeze for the baseline** |
| 14 | 2 | P2-E07 | **T53** Security and isolation verification (22 h) **+** T94 Security and isolation verification of added features (24 h) | **T54** Arabic and English critical journeys on mobile and keyboard (18 h) **+** T92 Language and access checks on added screens (30 h) | **T55** Publishing fault-injection evidence on a real Page (22 h) | **T56** Expanded quality review across four registers (18 h) **+** T93 Brain value comparison and held-out reporting (20 h) | Built workflows tested for safety, language and publishing faults; defects recorded and the worst repaired |
| 15 | 2 | P2-E07 | **T57** Durable-work recovery verification (20 h) **+** T95 Register tuning and held-out confirmation (20 h) **+** T97 Recovery verification of added workflows (24 h) | **T58** Runtime and cost ceilings at declared load (18 h) | **T59** Acceptance evidence run and release ledger (20 h) **+** T96 Acceptance evidence for added scenarios (24 h) | **T60** Production environment and release rehearsal (22 h) | Evidence exists for every built scenario; production exists; every gap is written down |
| 16 | 2 | P2-E07 | **T61** Defect repair: Chat, research, Roadmap, intake (23 h) | **T62** Defect repair: publishing, Scheduling, connection, interface (23 h) | **T63** Client-session production publishing validation (16 h) **+** T98 Full regression on the final repaired candidate (24 h) | **T64** Operator runbooks and owner help (18 h) **+** T99 Defect repair for added features (24 h) | Final repaired build regressed; client-session AC-30 verified; full-scope release evidence assembled |

### Manual baseline and AI-assisted hours by assignee

| Assignee | Manual-equivalent hours | Stacks touched |
|---|---:|---|
| A | 505 | API, platform services, agents, Roadmap, verification |
| B | 491 | Frontend, workflows, Chat, Brain, publishing access, load |
| C | 460 | Integration, security, evaluation, publishing, notifications |
| D | 494 | Research, models, operations, documents, intake, release |
| **Total** | **1,950** | |

The hours above 1,280 are the 35 additional tasks (T65–T99, 670 hours). Each is assigned to the person shown in the schedule, in addition to that person's primary task. Apply the approved 3×–5× multiplier; weekly human effort includes the separate allowances in file 01. The team may move an additional task to another person in the same week; it may not move it to another week without rechecking [F](./07-dependency-map.md).

A, B, C and D are placeholders. The team maps them to real people at kickoff by strength and availability. Same-week task startup is independent; swaps preserve named module/schema owners and the explicit weekly integration/certification handoffs.

Baseline human effort by assignee at 5×–3× is A: 101–168.3 hours; B: 98.2–163.7; C: 92–153.3; D: 98.8–164.7. Add weekly human-review and DoD allowances from file 01. These are allocations derived from the owner-confirmed benchmark, not a new benchmark run by this audit.
