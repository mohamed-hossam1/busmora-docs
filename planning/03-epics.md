# D. Epic Descriptions

> Part of the [BUSMORA 16-week implementation plan](./README.md). Planning IDs are stable identifiers for import into Jira; they are not Jira issue keys.
> Sources: PRD = [core/prd.md](../core/prd.md), SYS = [core/SYSTEM-ARCHITECTURE.md](../core/SYSTEM-ARCHITECTURE.md), BTD = Backend Technical Design, FTD = Frontend Technical Design, DS = Design System.

---

## P1-E01 — Delivery base and early evidence

| Field | Value |
|---|---|
| **Planning ID** | P1-E01 |
| **Phase** | 1 — Foundation |
| **Scheduled weeks** | 1–2 |
| **Blocked by** | None (first Epic) |

**Intended outcome and value.** Both codebases exist and start on any team member's machine, the riskiest technical path (durable work across two languages) has run once from request to live event, and the three unknowns that could invalidate the plan are measured or named: Facebook production access, research source coverage and Arabic model quality. The evaluation standard is frozen so later work is judged against a fixed bar.

**Entry criteria.** Approved documentation in `core/`, `domain/` and `strategy/`. Four people available. Nothing else is assumed: no repository, environment, account or provider access.

**Scope.** Backend and frontend repositories with local run and automated checks; the published API contract with typed frontend access; the walking skeleton (SYS §9.4 spike 1); Facebook access-path investigation and the external dependency register; research provider baseline (OD-22); Arabic model baseline (OD-4, D-31); evaluation standard (PRD §10.2, OD-24).

**Boundaries.** No owner-facing feature. No staging environment. No account system. Spikes produce recorded decisions and measurements, not product features.

**Exit criteria.**
1. A person who did not write it starts the backend and the frontend from the written instructions and sees every health check pass.
2. Automated checks run on each proposed change in both repositories and block a failing change.
3. One request travels through the public interface, the durable workflow, both language runtimes and back to a live event stream, and the stream resumes after a dropped connection without losing events.
4. The frontend reads a live backend value through the generated typed client, and an incompatible contract change is rejected automatically.
5. A written Facebook record states the selected app configuration, required access, verification and review steps, supported formats and blockers; it states whether a tester-mode post was publicly visible.
6. The dependency register lists owner, legal entity, budget, payer, reviewers and availability, each as confirmed, unconfirmed or blocked.
7. Research and model baselines report measured quality, regional coverage, latency and cost for each candidate, with a recorded selection.
8. Rubrics, severity definitions, sampling, held-out rule, release load and task limits are frozen and dated.

**Requirement references.** PRD §§10.1–10.2, §11.1, §13.1; FR-28, FR-33; NFR-06; AC-30 (path only); SYS §2.4, §6, §9.2 (OD-4, OD-12, OD-15, OD-17, OD-22, OD-23, OD-24), §9.4; ADR-002, ADR-014, ADR-015, ADR-018.

**Tasks.**
- Week 1: P1-E01-T01, P1-E01-T02, P1-E01-T03, P1-E01-T04
- Week 2: P1-E01-T05, P1-E01-T06, P1-E01-T07, P1-E01-T08

---

## P1-E02 — Secure accounts, isolation and operable environment

| Field | Value |
|---|---|
| **Planning ID** | P1-E02 |
| **Phase** | 1 — Foundation |
| **Scheduled weeks** | 3–4 |
| **Blocked by** | P1-E01 |

**Intended outcome and value.** An owner can hold a verified account in an isolated workspace, the product runs in a deployed environment that can be restored and rolled back, and operators can trace what happened. Every later feature depends on these properties, and they are expensive to add afterwards.

**Entry criteria.** P1-E01 exit criteria met.

**Scope.** Email/password registration with verification link, sign-in, sign-out and session; password recovery and session security; shared bilingual components and the structured form renderer; request tracing and redacted logs; staging with automated deployment and a secrets store; audit, usage and notification records; workspace isolation across all stores; data lifecycle policy and the first restore and rollback drill.

**Boundaries.** No Google sign-in (T70, Week 7). No business data. No Notification Bell interface (T51). The lifecycle policy is written and the drill is run; deletion across derived stores is not built here (T65, Week 9).

**Exit criteria.**
1. Registration with email and password asks for no business details, sends a single-use expiring link, and rejects an invalid, expired or replayed link.
2. A reused session credential revokes the whole session; a password reset invalidates existing sessions.
3. Staging is reachable over a secure connection and is updated by the pipeline without manual steps on a server.
4. Each cross-workspace access attempt in the isolation check fails, in every store in use.
5. A governed action writes one audit record, a costly call writes one usage record, and neither contains secrets.
6. An operator follows one request across all process roles using recorded identifiers.
7. A staging restore and a rollback have each been performed once, with timing and problems written down.
8. Arabic right-to-left and English left-to-right both render for every component in the shared kit.

**Requirement references.** FR-01, FR-18 (form renderer), FR-43, FR-50 (email link, reset, sessions); NFR-03, NFR-05, NFR-06, NFR-07, NFR-08, NFR-09; AC-31; SYS §4.7, §4.9, §7.1, OD-5, OD-10, OD-13, OD-16, OD-21; ADR-010, ADR-012, ADR-013; FTD-ADR-07, FTD-ADR-10, FTD-ADR-12.

**Tasks.**
- Week 3: P1-E02-T09, P1-E02-T10, P1-E02-T11, P1-E02-T12
- Week 4: P1-E02-T13, P1-E02-T14, P1-E02-T15, P1-E02-T16

---

## P1-E03 — Durable records for knowledge, conversation, work, documents and connection

| Field | Value |
|---|---|
| **Planning ID** | P1-E03 |
| **Phase** | 1 — Foundation |
| **Scheduled weeks** | 5–6 |
| **Blocked by** | P1-E02 |

**Intended outcome and value.** The records every Phase 2 feature writes to exist with their states, provenance and workspace attribution: knowledge, conversations, Tasks, documents, files and the Facebook connection. An owner gets a real reply and a Task survives a restart. This closes Phase 1.

**Entry criteria.** P1-E02 exit criteria met.

**Scope.** Knowledge records with epistemic status, lifecycle and history; conversations with routed quick replies; Facebook Page connection in tester mode with protected credentials; versioned document records; owner file storage; durable Task lifecycle with two-slot admission; 10 more authored evaluation cases; retrieval of eligible context only.

**Boundaries.** No orchestration, research, onboarding, Posts or Roadmap. The Task performs one real bounded step only. The Brain has a minimal list, not governance or graph. No publishing.

**Exit criteria.** Gate G1 in [B.3](./02-roadmap-and-schedule.md), all nine criteria, plus:
1. A retrieval query returns only current eligible knowledge from the asking workspace; an item rejected a moment earlier is absent (T24).
2. A valid owner image uploads and a disguised file is refused (T21).
3. A document shows two versions with the earlier one unchanged (T20).

**Requirement references.** FR-08, FR-09, FR-11, FR-17, FR-19, FR-23, FR-33 (connection), FR-41 (records), FR-42, FR-53 (admission), FR-56 (connection status); NFR-01, NFR-03, NFR-05, NFR-06; AC-41 (admission); PRD §10.1; SYS §3.4, §4.5, §4.6, §7.3; ADR-003, ADR-004, ADR-007, ADR-018; D-22, D-30, D-66.

**Tasks.**
- Week 5: P1-E03-T17, P1-E03-T18, P1-E03-T19, P1-E03-T20
- Week 6: P1-E03-T21, P1-E03-T22, P1-E03-T23, P1-E03-T24

---

## P2-E04 — First integrated owner slice

| Field | Value |
|---|---|
| **Planning ID** | P2-E04 |
| **Phase** | 2 — First production-ready release |
| **Scheduled weeks** | 7–9 |
| **Blocked by** | P1-E03 and Gate G1 |

**Intended outcome and value.** For the first time an owner completes the core experience on real records: confirm essential context, ask the Director for work, inspect what actually ran, and see an approved Post published to a real Page at its scheduled time. This is the earliest point at which the product can be shown to an owner and the earliest point at which a production-access submission to Meta can be prepared.

**Entry criteria.** Gate G1 passed.

**Scope.** Manual setup and website import into one confirmed draft; Brain activation; orchestrated work from Chat; typed results as documents and Posts; manual Post drafts; exact-version approval; scheduled real publishing of text and link posts; research report with citations; the Canvas; Discovery draft and adaptive questions. Added by the plan owner's decision on DR-2: clean-up of removed data from every derived copy (T65).

**Boundaries.** Tester-mode Page only; no claim about ordinary-owner access. No image posts, no recovery from uncertain outcomes, no budget extensions, no Roadmap generation yet. Calendar Month and Week views come in the next Epic (T78, T81). Added on 2026-11-14 from the former overflow register: Google sign-in (T70), completion of the evaluation library (T71), spreadsheet intake and requested analysis (T72, T75), Settings (T73) and campaign packages (T74).

**Exit criteria.**
1. On staging, an owner confirms a draft from manual entry and another from a website; nothing reaches the Brain before confirmation of the exact displayed version.
2. A substantive Chat request runs as a Task that uses confirmed Brain context and saves a document; a one-post request creates a Post in Scheduling and no duplicate document.
3. The Canvas shows only events recorded for the selected conversation's Task.
4. An approved text post is published on a tester-mode Page within its allowed window, with the platform's post identity and read-back stored; editing an approved detail blocks dispatch until re-approval.
5. A research request returns a report whose material claims each link to an inspectable source.
6. A Discovery draft with adaptive questions survives closing and reopening the browser.
7. Every screen in the slice works in Arabic right-to-left and English left-to-right.
8. A removed knowledge item, document, file and website import is absent from every derived store the lifecycle policy names.

**Requirement references.** FR-13 (deletion), FR-42, NFR-09, FR-03–07, FR-17, FR-19–22, FR-24–25, FR-29 (single item), FR-33–35, FR-41, FR-45, FR-51, FR-55 (drafts, list), FR-57; NFR-01, NFR-02, NFR-04; AC-01, AC-02, AC-05, AC-06, AC-07, AC-13, AC-21, AC-22, AC-33, AC-34, AC-36, AC-48, AC-49; ADR-005, ADR-006, ADR-021, ADR-023, ADR-024.

**Tasks.**
- Week 7: P2-E04-T25, P2-E04-T26, P2-E04-T27, P2-E04-T28, and additional tasks P2-E04-T70, P2-E04-T71, P2-E04-T72
- Week 8: P2-E04-T29, P2-E04-T30, P2-E04-T31, P2-E04-T32, and additional task P2-E04-T73
- Week 9: P2-E04-T33, P2-E04-T34, P2-E04-T35, P2-E04-T36, and additional task P2-E04-T65, P2-E04-T74, P2-E04-T75

---

## P2-E05 — Recoverable publishing, bounded research and an approved Roadmap

| Field | Value |
|---|---|
| **Planning ID** | P2-E05 |
| **Phase** | 2 — First production-ready release |
| **Scheduled weeks** | 10–11 |
| **Blocked by** | P2-E04 |

**Intended outcome and value.** The three places where the product could mislead or harm an owner are made safe: a publication whose outcome is unknown, research that spends past its allowance or cites weakly, and a plan that changes what the owner does. The production-access request goes to Meta at the earliest point the working flow can be shown.

**Entry criteria.** P2-E04 exit criteria met, including a real tester-mode publication.

**Scope.** Reconciliation of uncertain publication; overdue, held, cancelled and disconnected states; the Meta submission package; clarification forms that pause and resume a Task; research budget with approved extensions; research evidence quality, stopping and untrusted-source handling; Roadmap generation, approval, single active plan and step view. Added by the plan owner's decision on DR-2: marking of work affected by a context change (T66) and owner export of the Brain and history (T67).

**Boundaries.** Meta's approval is not in scope and not scheduled. No step execution yet. No strategic revisions (T88, T89, next Epic). Added on 2026-11-14 from the former overflow register: brand profile and claim checks (T76), the Brain graph (T77), calendar views and drag rescheduling (T78, T81), question research and its return (T79, T83), Canvas depth (T80), bulk approval (T82) and reuse of earlier Discovery answers (T84). Holding a scheduled Post after a context change comes in the next Epic (T68).

**Exit criteria.**
1. A publish attempt whose acceptance is unknown is reconciled against the platform or held; it is never sent twice.
2. An approved post past its lateness allowance is held for a new time and approval; disconnecting blocks sends and reconnecting releases nothing by itself.
3. The submission to Meta is filed, or the exact missing prerequisite is recorded with its owner.
4. A Task that needs an answer shows the exact form in Chat, releases its running slot and resumes when answered.
5. Research stays inside its allowance across child, concurrent and retried work; a requested increase pauses for the owner; a refusal returns the strongest partial result.
6. A source that contains instructions cannot trigger a tool, raise a budget or cause an internal fetch.
7. Discovery produces a complete proposed plan; only one plan can be active even under concurrent approvals.
8. Changing a confirmed fact marks the documents and unpublished Posts that used it, and only those.
9. An owner export contains the workspace's Brain, conversations, documents and Posts, and nothing from another workspace.

**Requirement references.** FR-10, FR-13 (export), FR-42, FR-48, NFR-09, AC-47, FR-14 (plan evidence), FR-16, FR-18, FR-24–28, FR-33, FR-35–36, FR-46, FR-54 (activation), FR-56 (connection states); NFR-02, NFR-04, NFR-05, NFR-06; AC-04 (activation), AC-08, AC-09, AC-20, AC-27, AC-28, AC-30 (submission), AC-35, AC-46, AC-54; ADR-018, ADR-022.

**Tasks.**
- Week 10: P2-E05-T37, P2-E05-T38, P2-E05-T39, P2-E05-T40, and additional task P2-E05-T66, P2-E05-T76, P2-E05-T77, P2-E05-T78, P2-E05-T79
- Week 11: P2-E05-T41, P2-E05-T42, P2-E05-T43, P2-E05-T44, and additional task P2-E05-T67, P2-E05-T80, P2-E05-T81, P2-E05-T82, P2-E05-T83, P2-E05-T84

---

## P2-E06 — Step execution and owner control of knowledge, documents and notices

| Field | Value |
|---|---|
| **Planning ID** | P2-E06 |
| **Phase** | 2 — First production-ready release |
| **Scheduled weeks** | 12–13 |
| **Blocked by** | P2-E05 |

**Intended outcome and value.** The owner can act on the plan and stay in control of what the product remembers and tells them: run a step, report their own work, skip and return, approve or reject what the Director wants to remember, correct the Brain, manage documents and follow notices to the exact item. Both committed Facebook formats publish. The baseline feature set freezes at the end of this Epic.

**Entry criteria.** P2-E05 exit criteria met.

**Scope.** Run with Director; owner-reported Done, Skip and Revisit; Pending Brain Updates; Brain List governance; single-image posts with checks before approval; constraint diagnosis with about six recipes; Notification Bell; Deliverables library. Added by the plan owner's decision on DR-2: context-change holds on scheduled Posts (T68) and restore suppression for removed data (T69).

**Boundaries.** No new feature after the end of this Epic. Added on 2026-11-14 from the former overflow register: honest handoff from Chat (T85), reuse of past work and remaining exports (T86), duplicate and local delete of published Posts (T87), strategic revisions (T88, T89), working preferences and proof and metric relations (T90), and the remaining recipes with diagnosis tuning (T91). The graph, bulk approval, requested analysis and Settings were delivered in earlier Epics (T77, T82, T72, T75, T73).

**Exit criteria.**
1. Launching an eligible step creates exactly one Task under a double click or retry; reopening the step does not relaunch it.
2. Skip records intent and time, is never shown as completed or cancelled, and Revisit recomputes readiness.
3. An AI-proposed fact reaches the Brain only after the owner approves or edits and approves it; a rejected proposal never returns through retrieval.
4. A post with missing or invalid media cannot be marked ready for approval; a valid single-image post publishes with platform evidence.
5. Two contrasting evaluation cases produce different primary constraint hypotheses, each with evidence and a stated alternative.
6. A notification opens the exact subject after an access check; a removed subject shows an honest unavailable state.
7. Editing a document creates a new version and keeps the old one readable.
8. A Post approved before a price change is held and not published at its scheduled time.
9. After a staging restore, an item removed after the restore point is absent before any work can use the restored data.

**Requirement references.** FR-13, FR-35, FR-48, NFR-05, NFR-09, AC-23, FR-08–12, FR-14–15, FR-31, FR-33–34, FR-41, FR-46, FR-49, FR-53, FR-56; PRD §5.13; NFR-01; AC-03 (list governance), AC-12 (formats), AC-18, AC-19, AC-39, AC-40, AC-41, AC-47, AC-55.

**Tasks.**
- Week 12: P2-E06-T45, P2-E06-T46, P2-E06-T47, P2-E06-T48, and additional task P2-E06-T68, P2-E06-T85, P2-E06-T86, P2-E06-T87, P2-E06-T88
- Week 13: P2-E06-T49, P2-E06-T50, P2-E06-T51, P2-E06-T52, and additional task P2-E06-T69, P2-E06-T89, P2-E06-T90, P2-E06-T91

---

## P2-E07 — Verification, stabilization and release readiness

| Field | Value |
|---|---|
| **Planning ID** | P2-E07 |
| **Phase** | 2 — First production-ready release |
| **Scheduled weeks** | 14–16 |
| **Blocked by** | P2-E06 |

**Intended outcome and value.** Everything built is tested the way the PRD's release gates require, the worst defects are repaired, a production environment exists and has been rehearsed, and a ledger states for every requirement what is implemented and evidenced. Since 2026-11-14 the Epic also verifies the added features, so that all 57 active scenarios are run on the release candidate and the team and its supervisors can make the Phase 2 release decision on facts, with Meta's approval as the one external dependency.

**Entry criteria.** P2-E06 exit criteria met. No new feature starts in this Epic.

**Scope.** Security and isolation verification; Arabic and English critical journeys on mobile and keyboard; publishing fault injection on a real Page; first human quality review across four registers; durable-work recovery verification; runtime and cost at declared load; acceptance evidence run and release ledger; production environment and rehearsal; two defect-repair tasks for the core areas and one for the added features; verification of the added features for language and access, security and recovery; the Brain value comparison; register tuning with a second review round; acceptance evidence for the added scenarios; a full regression of all 57 scenarios; intended-owner Facebook validation; operator runbooks and owner help.

**Boundaries.** No new feature. It verifies every built workflow, including the added features (T65–T91), completes the evaluation evidence (T93), runs the second review round (T95) and repairs defects until critical and high ones are closed. It cannot produce Meta's approval, which is the only release gate outside the team's control.

**Exit criteria.**
1. Every active FR, NFR and AC has a ledger row with status, evidence reference and owner; rows for unbuilt work say "not built".
2. No critical truthfulness or control violation is open in the built workflows, or each one is listed as a release blocker with reproduction steps.
3. Fault scenarios for publishing (retry, concurrency, cancellation race, revocation, version change, late job, uncertain result) each have a recorded real-platform result.
4. Review scores exist per register with counts and denominators; a register with no reviewer is recorded as an unresolved gap.
5. Production deploy, rollback and restore have each been rehearsed once.
6. AC-30 is recorded as passed with ordinary-owner evidence, or as blocked with the reason and the external party.
7. An operator who did not write the runbook resolves a stuck job and an uncertain publication using it alone.
8. All 57 active acceptance scenarios have a result on the release candidate (T98). A written release statement says each PRD §10.2 gate is met, or names the gate that is not met and why; AC-30 may be stated as waiting on Meta.

**Requirement references.** PRD §3.1, §10.2, §13.3; NFR-01–09; AC-11, AC-12, AC-13, AC-16, AC-17, AC-27, AC-28, AC-29, AC-30, AC-41, AC-43, AC-44, AC-56, AC-57, AC-58.

**Tasks.**
- Week 14: P2-E07-T53, P2-E07-T54, P2-E07-T55, P2-E07-T56, and additional tasks P2-E07-T92, P2-E07-T93, P2-E07-T94
- Week 15: P2-E07-T57, P2-E07-T58, P2-E07-T59, P2-E07-T60, and additional tasks P2-E07-T95, P2-E07-T96, P2-E07-T97
- Week 16: P2-E07-T61, P2-E07-T62, P2-E07-T63, P2-E07-T64, and additional tasks P2-E07-T98, P2-E07-T99
