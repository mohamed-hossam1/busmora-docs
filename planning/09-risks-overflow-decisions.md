# H. Technical Risks, Full Scope and Decisions

> Authority: [approved execution contract](./11-execution-contract.md), 2026-11-15. All decisions below apply to the complete 99-task, 16-week delivery plan.

## H.1 Former overflow register — fully scheduled

**Unscheduled overflow: 0 tasks and 0 hours.** All former items remain in T65–T99. Hours below are traditional manual-equivalent estimates; the approved AI multiplier and explicit allowances are in [file 01](./01-assumptions-and-feasibility.md).

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

Performance and browser-adapter tuning occur weekly, with expanded measurements in T58 and final repair in T61/T62/T99. Technical rework draws on the human-effort reserve in file 01; no task or feature is moved out of the 16 weeks.

## H.2 Resolved delivery decisions

| Decision | Approved outcome | Implementation in the plan |
|---|---|---|
| DR-1 — Timeline | All Phase 1/2 scope in 16 weeks | Same 99 task IDs and scheduled weeks |
| DR-2 — Scope allocation | No feature swaps or reductions | All former overflow tasks remain assigned |
| DR-3 — Verification | Weekly tests throughout all 16 weeks | Weeks 14–16 expand coverage; Week 16 stabilizes and polishes |
| DR-4 — Architecture | Use `busmora-web`, `busmora-api`, `busmora-ai` and `busmora-docs`; keep four deployables/eight roles; Python workers live in `busmora-ai` | Adopt/configure/build ownership in each task; alternatives in file 11 |
| DR-5 — Parallel delivery | Domain/module separation and schema ownership settled | Four humans supervise AI work; automated PR linting and owned schema changes prevent merge contention |
| DR-6 — Phase 2 order | Existing Epic sequence retained | Weekly staging validates each increment, including incomplete journeys |
| DR-7 — Publishing access | Scrapling adaptive parsing and Playwright with encrypted client sessions | T03/T19/T35/T38/T43/T47/T63; dashboard reauthentication, no developer-app prerequisite |
| DR-8 — Calendar | No extra buffer weeks | Repair/integration effort reserved inside the existing weeks |
| DR-9 — Gates | Technical criteria continuously checked | Immediate owned repair; no dependent activation against a failing prerequisite |
| DR-10 — Dependencies | Earlier-week startup prerequisites with explicit completion handoffs | Weekly integration and final repairs → T98; changes → T64 |
| DR-11 — Value and dialect review | Continuous weekly dogfooding and tuning | T08 onward, with T56/T93/T95 as expanded checkpoints |
| DR-12 — Overflow | Zero | T65–T99 complete the full Phase 1/2 commitment |
| DR-13 — Capacity | Owner-confirmed benchmarked 3×–5× AI acceleration; 16 weeks validated | 390–650 baseline human hours plus explicit DoD/human-review allowances |

These supersede earlier proposed scope cuts, API approval branches and manual-only capacity conclusions. No further architecture change or feature removal is implied.

## H.3 Operational ownership

Legacy XD identifiers are retained for cross-references; they now identify operational work, not external project blockers.

| ID | Activity | Owner / timing | Treatment |
|---|---|---|---|
| XD-1 | Map A–D to people and skills | Plan owner, kickoff | Assign human oversight and agent work |
| XD-2 | Calendar and weekly staffing | Integration lead, weekly | Rebalance within the committed schedule |
| XD-3 | Test accounts, LLM keys and infrastructure | Internal ops, week-by-week | Provision only the consuming milestone's inputs; no front-loaded baseline gate |
| XD-4 | Company/commercial setup | Internal ops | Separate from technical milestones |
| XD-5 | Browser runtime and secret-store setup | T03/T12/T19 owners | Technical configuration, no developer app |
| XD-6 | Client authorization and Page selection | T03/T19/T63 owners | Verify client-controlled session and destination |
| XD-7 | Browser adapter health and session recovery | T38/T43 owners, weekly | Engineering-owned tests and repair |
| XD-8 | Optional dialect feedback | Quality owner, weekly | Supplement team evaluation; no core-engine delay |
| XD-9 | Cross-stack support and peer review | Integration lead, weekly | Assign reviewer and integration owner explicitly |
| XD-10 | Dogfooding scenario and value notes | T07 owner and task owners | Record weekly evidence as functionality grows |
| XD-11 | Purchasing and payment administration | Internal ops, in parallel | Not a release or phase-entry condition |

Actual service outages, invalid sessions and incorrect product behavior remain technical incidents with named repair owners. Internal administrative activity is not converted into an engineering gate.

## H.4 Technical risk register

RK-1 (capacity uncertainty) and RK-12 (developer collaboration/merge contention) are **closed** by the confirmed benchmark and delivery controls. Their identifiers are retained here for traceability, not as active project risks. Remaining entries are runtime failure cases with implementation and test owners; they do not reopen the schedule or reduce scope. The precise safety contracts are in [file 11](./11-execution-contract.md#runtime-boundaries-and-durable-handoffs).

| ID | Technical risk | Response and verification | Owner |
|---|---|---|---|
| RK-1 | CLOSED — benchmarked AI throughput | Accepted owner confirmation; execution accounting continues | Integration lead |
| RK-2 | Prerequisite defects affect integration | Check continuously; repair within the week and rerun before dependent activation | Epic lead |
| RK-3 | Orchestration produces wrong or unbounded work | Fixed ceilings, typed outputs, adversarial fixtures and weekly regression | T25/T42 |
| RK-4 | Cross-language durable workflow fails | Early T06 exercise, contract checks and interruption tests | T06/T57 |
| RK-5 | Adaptive matching is missing, ambiguous or semantically wrong | Scrapling fingerprints plus live Playwright checks; isolated adaptive storage; wrong-target and changed-DOM fixtures fail closed | T03/T38/T55 |
| RK-6 | Session expires, account challenges login or Page access changes | Automatic dashboard recovery, single-use scoped reauth operation, generation checks and isolated encrypted state | T19/T43 |
| RK-7 | Arabic/regional sources have gaps | Measure coverage, preserve source provenance and report partial evidence | T04/T44 |
| RK-8 | Grounding or register quality varies | Weekly comparisons, fixed rubric, tune against training cases and report held-out outcomes | T08/T56/T93/T95 |
| RK-9 | Evaluation scores lack independent coverage | Label score provenance and sample size; gather optional feedback alongside engineering | Quality owner |
| RK-10 | A repair causes another regression | Per-change reruns, weekly cumulative suite and final full T98 run after repairs | T61/T62/T98/T99 |
| RK-11 | Runtime/token/browser-resource usage exceeds limits | Meter usage, enforce per-task limits and test load incrementally | T07/T14/T42/T58 |
| RK-12 | CLOSED — parallel developer collaboration | Strict domain/module separation, automated PR linting and clear schema ownership | Integration lead |
| RK-13 | Evidence refers to an obsolete build | Build/configuration IDs in ledger; invalidate after changes; certify final build | T59/T98 |
| RK-14 | Deploy/restore resumes obsolete publishing authority | Reapply holds, revocations, unresolved attempts, publication identities and deletion records before resuming work | T43/T60/T69 |
| RK-15 | Removed data survives in derived stores | Cleanup plus restore suppression and repeated isolation/deletion tests | T65/T69/T94 |
| RK-16 | Session secrets leak through traces or model access | Least privilege, encryption, redaction, no model access and negative tests | T19/T53 |
| RK-17 | Shared staging or browser sessions interfere | Named reservations, isolated workspaces/contexts, serial live publishing and controlled load windows | T12/T55/T58 |
| RK-18 | Ambiguous browser outcome causes duplicates | Durable version key and attempt before action; no automatic dispatch retry or lease takeover; attributable permalink/read-back, otherwise hold | T35/T39/T55 |
| RK-19 | Reauthentication completion or dashboard event is stale | Single-use expiry/generation checks, monotonic state revisions, authorized snapshot refresh; no automatic release on reconnect | T06/T19/T43 |
| RK-20 | Image bytes change or access expires after approval | Immutable media version/digest, fresh scoped download reference, pre-submit validation and cleanup | T21/T47/T55 |

## H.5 Execution records

Maintain weekly build and dogfooding records, effort actuals, defect/rerun evidence and the release ledger. T07 records provisional numeric limits and their confirmation tasks; T16/T21 finalize lifecycle/upload values before dependent acceptance. Missing measurements stay visibly pending until run. The [resolution report](../report/consolidated_review_report.md) closes outdated planning findings without fabricating runtime evidence.
