# I. Review Findings and Resolutions

> Updated 2026-11-15 against the project owner's [execution contract](./11-execution-contract.md).
> Status: **PASS — planning alignment / RESOLVED**. This is an authored resolution of previous findings, not a new independent Goldfish review or software test result.

## I.1 Review basis

Earlier reviews used a manual-effort capacity model, API-mediated publishing, late formal quality checkpoints and external administrative prerequisites. The approved execution strategy replaces those premises with 3×–5× AI-assisted engineering, client-session browser automation, weekly staging/dogfooding and continuous tests. All 99 tasks and the 16-week commitment remain.

Historical reviewer output is superseded for current planning decisions. The reports in [report/](../report/consolidated_review_report.md) retain finding IDs and explain the resolutions.

## I.2 Blocker resolutions

| Finding | Resolution | Governing artifacts |
|---|---|---|
| B1 — Capacity and fixed schedule | 1,950 manual-equivalent hours become 390–650 human hours; explicit DoD, human-review and repair allowances; continuously evaluated gates | Files 01, 02, 07, 11 |
| B2 — Final repaired build regression | Per-change/weekly tests; Week-16 discoveries enter repair; T98 reruns all 57 scenarios after the final merge and T63 verifies the same build | Files 06, 07, 11 |
| B3 — Value and Brain validation timing | Weekly staging and dogfooding begin in Week 1; comparative model/context experiments start early and continue on the implemented Brain; T93 consolidates results | Files 02, 04, 06, 11 |
| B4 — Architecture and open decisions | Existing topology retained; browser-session publishing replaces the API path; alternatives and all DR-1–DR-13 decisions recorded | Files 09, 11 |

## I.3 Major resolutions

| Finding | Resolution |
|---|---|
| M1 — Coverage and overflow | 57 FR, 9 NFR and 57 AC fully scheduled; no external AC-30 gate; planned coverage distinct from actual pass evidence |
| M2 — Definition of Done | Explicit allowance, applicability definitions, T14/T15 integration of earlier records and isolation checks |
| M3 — Integration contention | RESOLVED — confirmed domain/module separation, automated PR linting, schema ownership and explicit completion handoffs; not an active collaboration risk |
| M4 — External prerequisites | Browser login/session recovery replaces app approval; internal legal, purchasing and optional dialect review run in parallel |
| M5 — Limits and risks | T07/T16/T21 set values before dependent acceptance; weekly measurement, T58 confirmation and browser-specific risks |
| M6 — Documentation clarity | README includes phase outcomes, architecture links, definitions, authority and evidence semantics |
| N1 — Traceability/editorial details | Ledger creation T07, weekly maintenance and T59/T98 audits distinguished; stale claims replaced |

## I.4 Retained technical improvements

Cross-workspace checks grow as stores gain real data. Restore replays revocations, holds and removal records before resuming work. Features write their own applicable audit, usage and notification records; T51 verifies display. T07 creates the evidence structures, T12 hardens staging, and every task includes bilingual operation and peer review. These controls remain part of full-scope delivery.

## I.5 Evidence boundaries

The project owner confirms the 3×–5× multiplier is benchmarked and the full 16-week schedule validated. This audit accepts that evidence statement without independently rerunning the benchmark. Client-session publication results, register scores and regression results are produced during execution. A planning PASS never substitutes for a failed or unrun technical check. All Phase 1/2 functionality stays committed while defects are corrected.

## I.6 Final contract hardening

The [final readiness report](../report/final-readiness-and-execution-clearance.md) records the repository-scaffolding clearance. File 11 and the owning tasks now explicitly cover Scrapling/Playwright adaptive matching, durable dispatch/reconciliation boundaries, secure dashboard reauthentication, immutable approved media, progressive provisioning and Week-1 staging evidence. These are refinements to existing work, not extra tasks or scope reductions.
