# E. Task Descriptions — Phase 2 Verification, Stabilization and Release (Weeks 14–16, T53–T64 and additional tasks T92–T99)

> Execution authority: [file 11](./11-execution-contract.md). Estimate/Range values are manual-equivalent hours; use 3×–5× compression and file 01 allowances. These tasks expand testing already performed every week.
> Part of the [BUSMORA 16-week implementation plan](./README.md). Epics: [D](./03-epics.md). Dependencies and weekly independence checks: [F](./07-dependency-map.md).
> These are committed verification and stabilization tasks. Their results are recorded during execution.
> Every task also meets the shared Definition of Done in the [README](./README.md#definition-of-done-for-every-task). Each estimate includes research, implementation, tests, peer review, expected fixes and the demonstration.
> The release ledger and defect register were created in T07 and have been filled task by task since. Each task here adds its own results to its own rows.
> No new feature starts in this Epic. Each task extends the weekly quality discipline, verifies the full committed scope and routes findings into repair and rerun.
> Difficulty factors: **V**olume, **U**ncertainty, **I**ntegration, **S**ecurity/data risk, **T** verification difficulty — each Low, Med or High.

---

## Week 14 — P2-E07

### P2-E07-T53 — Security and isolation verification

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P2-E07 | 14 | A | 22 h | 18–30 h | Medium |

**User story.** As a business owner, I want the finished product checked for ways another person or a hostile web page could reach my data or act for me, so that I can rely on the controls and not only on their design.

**Problem and outcome.** Each feature carried its own checks. After this task the built product has been attacked as a whole across workspaces, credentials, sessions, approvals and untrusted input, with critical and high findings repaired or listed as blockers.

**Scope.** Cross-workspace attempts against every built module and store; credential handling for the Page connection; session and revocation behaviour; approval boundaries (an approval cannot be created or widened by model text, retrieved content or a replayed request); hostile website and research sources; unsafe content in documents; repair of critical and high findings within the estimate.
**Excludes.** Third-party penetration testing. The added features, which T94 covers.

**Acceptance criteria.**
1. For each built module, a request made as one workspace for another's record is refused, and the result is recorded.
2. Page credentials are not obtainable through any application response, log, trace or the AI runtime.
3. Approvals for a Post, a Roadmap, a Brain proposal and a budget extension cannot be produced by replaying an old request or by content the model wrote.
4. A hostile source cannot trigger a tool, raise an allowance, change knowledge or cause an internal fetch.
5. Every finding is classed by the frozen severity definitions; each critical or high finding is repaired and retested, or recorded as a release blocker with reproduction steps.

**Requirement references.** NFR-03, NFR-04; PRD §7, §10.2 (truthfulness/control, recovery/security); AC-17, AC-27, AC-31.
**Prerequisites.** All tasks T09–T52 and T65–T69; severity definitions from T07.
**Independence this week.** Verifies security behaviour; T55 tests publishing faults, T54 language and access, T56 quality.
**Estimate and difficulty.** 22 h. V High, U Med, I Med, S High, T High.
**Completion evidence.** The findings record with severity, status and retest result for each.
**Risks and assumptions.** A finite check cannot prove the absence of future risk; the ledger says so. Findings beyond the estimate go to T61 or T62.

### P2-E07-T54 — Arabic and English critical journeys on mobile and keyboard

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P2-E07 | 14 | B | 18 h | 15–24 h | High |

**User story.** As an Arabic-first business owner using my phone, I want every important action to be readable and usable in my language, so that I can run my marketing without a desktop or English.

**Problem and outcome.** Screens were built bilingual one by one. After this task the built critical journeys have been walked end to end in both languages on a phone-width screen and by keyboard, with blocking defects repaired.

**Scope.** Journeys: sign up and verify; onboarding by manual entry and by website; a Chat request with a clarification; reading the Canvas list; Discovery to an approved Roadmap and a run step; creating, approving and rescheduling a Post; reading a notification; the Brain List. Checks of direction, mixed-script text, dates, times with zone, currencies, forms and errors, focus and labels. Repair of blocking defects.
**Excludes.** Screens added by T65–T91, which T92 covers. Native review of generated content (T56).

**Acceptance criteria.**
1. Each listed journey is completed in Arabic and in English at phone width without a blocking defect, or the defect is recorded.
2. Each journey is completed by keyboard alone.
3. In the Post approval preview the owner can identify what will be posted, where and when, including the time zone, in both languages.
4. No untranslated text, clipped text or reversed number or link appears in the listed journeys.
5. Active interaction time to the first useful output is measured for five trial runs and reported separately from background waiting time.
6. Blocking defects are repaired and retested, or recorded as release blockers.

**Requirement references.** FR-43, FR-44; NFR-07; PRD §10.2 (usability); AC-11, AC-29; DS §11, §12.
**Prerequisites.** All owner-facing tasks T09–T52 and T65–T68.
**Independence this week.** Interface behaviour only.
**Estimate and difficulty.** 18 h. V Med, U Low, I Low, S Low, T Med.
**Completion evidence.** A journey-by-journey record in both languages with the timing measurements.
**Risks and assumptions.** Trial runs by team members are not owner evidence and are labelled as such.

### P2-E07-T55 — Publishing fault-injection evidence on a real Page

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P2-E07 | 14 | C | 22 h | 18–30 h | Medium |

**User story.** As a business owner, I want proof that publishing behaves safely when things go wrong, so that a network fault or a last-second change can never cause a duplicate, a late post or a post I did not approve.

**Problem and outcome.** Each publishing task demonstrated its own cases. After this task all of the PRD's publication fault scenarios have been run together against a real client-authorized browser Page, with results stored as release evidence.

**Scope.** Induced scenarios: retry after a proven pre-submit failure, two workers on one Post, delayed worker after timeout, crash before/after click, cancellation at dispatch, revoked authorization, concurrent/stale reauthentication, changed DOM and wrong adaptive matches, media replacement/expired access, a version changed after approval, a late job, an unknown result with delayed history or identical manual content, and a business-context change after approval (T68); both committed formats; checking the Page itself for duplicates; repair of defects found.
**Excludes.** Bulk approval (T82). Ordinary-owner access (T63).

**Acceptance criteria.**
1. Each scenario has a recorded run with the Post's state history and the resulting state of the real Page.
2. No scenario leaves two posts on the Page for one approved version.
3. No scenario publishes a version, destination or time the owner did not approve.
4. No dispatch is authorized outside the lateness window or after disconnection. An already authorized in-flight action follows the stop/reconciliation boundary in file 11; its external acceptance time is reported truthfully, never treated as cancelled without evidence.
5. Every unknown result ends reconciled or held.
6. Each failed scenario is repaired and rerun, or recorded as a release blocker.
7. The evidence identifies the authorized browser session, Page, build and runtime while redacting secrets.
8. No irreversible action is retried on an unknown result or lease timeout; no reauthentication callback, adaptive fingerprint or browser profile crosses workspace boundaries. Each contract case in file 11 has actual evidence or a visible unresolved test result.

**Requirement references.** FR-34, FR-35, FR-36; NFR-05; PRD §10.2 (publication); AC-12, AC-13, AC-28, AC-54.
**Prerequisites.** P2-E04-T31, T35; P2-E05-T38, T39, T43; P2-E06-T47, T68.
**Independence this week.** Publishing path only.
**Estimate and difficulty.** 22 h. V Med, U Med, I High, S High, T High.
**Completion evidence.** The scenario record with links to the real posts.
**Risks and assumptions.** The platform may limit how faults can be induced; scenarios that could not be induced are listed as not evidenced. Its cross-cutting effort is included in file 01's explicit allowance.

### P2-E07-T56 — Expanded quality review across four registers

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P2-E07 | 14 | D | 18 h | 14–28 h | Low |

**User story.** As the team and its supervisors, we want suitable people to score the product's diagnoses, plans and content in English, Egyptian Arabic, Gulf-oriented Arabic and Modern Standard Arabic, so that we know the real quality in each register instead of assuming it.

**Problem and outcome.** Weekly dogfooding and register evaluation have already produced feedback and refinements. After this task outputs for the available cases have been scored against the frozen rubric by reviewers, with results reported per register.

**Scope.** Running the 14 cases through the built product; preparing blind scoring packs; collecting weekly internal scores and optional independent blind scores, with adjudication when qualified reviewers are available; a grounding audit of consequential claims in a sample; reporting counts and denominators per register and overall; the most damaging defects repaired within the estimate; clarification frequency recorded.
**Excludes.** The remaining ten cases, the comparison against a document-only route, tuning and a second round (T95, T71, T93).

**Acceptance criteria.**
1. Every available case is run on the built product and its outputs are kept unchanged for scoring.
2. Scores are reported per register with the number of cases and reviewers; small-sample uncertainty is stated.
3. Scores identify team, automated and optional independent sources. Missing independent review is marked unmeasured and does not delay the core engine.
4. Held-out cases are reported separately and were not used for tuning.
5. In the audited sample, each consequential claim is marked supported, owner claim, hypothesis or unsupported; any fabricated citation is a critical defect.
6. The result is compared with the 80% gate and stated as met, not met or not measurable for each register.

**Requirement references.** FR-14, FR-26, FR-32; NFR-02, NFR-07; PRD §10.1–10.2 (usefulness, grounding); AC-11, AC-18, AC-19; strategy/evaluation.md §4.
**Prerequisites.** P1-E01-T07; P1-E03-T23 (cases and the reviewer list); P2-E04-T29, T36; P2-E05-T37; P2-E06-T48; weekly internal evaluation records.
**Independence this week.** Quality of outputs only.
**Estimate and difficulty.** 18 h. V Med, U High (model/register variance), I Low, S Low, T High.
**Completion evidence.** The scoring report with per-register counts.
**Risks and assumptions.** Optional outside dialect feedback runs in parallel. Report sample sizes and scorer provenance; ongoing team evaluation and tuning continue through release.

---

## Week 15 — P2-E07

### P2-E07-T57 — Durable-work recovery verification

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P2-E07 | 15 | A | 20 h | 17–27 h | Medium |

**User story.** As a business owner, I want my running work to survive failures behind the scenes and to be told the truth when it cannot, so that a server problem never silently loses or misreports my task.

**Problem and outcome.** Durability was shown per feature. After this task Tasks, research, website imports and waiting clarifications have been interrupted on purpose and their recovery and reported states checked.

**Scope.** Worker interruption during orchestrated work, research, a website import and a clarification wait; admission under simultaneous requests across conversations; cancellation during work; what the owner sees during recoverable failures and after recovery is exhausted; resumption that rechecks authorization and eligibility; repair of defects found.
**Excludes.** Publishing faults (T55). Hiding of retry attempts beyond what T33 shows (T80).

**Acceptance criteria.**
1. Each interrupted workflow completes or ends in a truthful terminal state; none is left shown as running with no work happening.
2. Under 20 simultaneous requests in one workspace, never more than two Tasks run and waiting Tasks start in order.
3. A Task resumed after an interruption does not use a Brain item removed or rejected while it was paused.
4. After recovery is exhausted the owner sees failed or partially completed with a reason and an available action.
5. A recoverable retry is not announced as a failure and creates no notification.
6. Each defect is repaired and retested, or recorded as a release blocker.

**Requirement references.** FR-23, FR-53; NFR-01, NFR-05; AC-17, AC-41, AC-43, AC-44.
**Prerequisites.** P1-E03-T22; P2-E04-T25, T28, T36; P2-E05-T40, T42; P2-E06-T45.
**Independence this week.** Uses its own test workspaces on staging, in time windows separate from T58's load runs.
**Estimate and difficulty.** 20 h. V Med, U Med, I High, S Med, T High.
**Completion evidence.** The interruption record with before and after states for each workflow.
**Risks and assumptions.** AC-43 will be reported as partly met if retry attempts are still visible in the Canvas.

### P2-E07-T58 — Runtime and cost ceilings at declared load

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P2-E07 | 15 | B | 18 h | 15–25 h | Medium |

**User story.** As an operator, I want measured response times, limits and cost per task at the load we declared, so that we know what the product can carry and what it costs before owners depend on it.

**Problem and outcome.** Limits were declared in T07 from early measurements. After this task they are measured on staging at the declared release load and compared with the ceilings.

**Scope.** Measurement of quick-reply time, Task acknowledgement, event delivery, content generation, research and import duration; behaviour at the concurrency limits; cost per accepted document and Task including failed and retried calls; comparison with the frozen ceilings; a load-test tool selected (OD-12).
**Excludes.** Scaling work. Tests against production.

**Acceptance criteria.**
1. Each frozen limit has a measured value at the declared load and a pass or fail.
2. Test conditions are recorded: load, data volume, environment size and which providers were real.
3. Cost per Task includes failed and retried calls and relevant service costs.
4. Any limit that fails is recorded as a release gap; it is not changed without the documented change rule.
5. The projected monthly running cost for the declared load is stated.

**Requirement references.** NFR-06; PRD §10.2 (runtime/resources), §10.3 (cost per accepted Task); SYS §2.2.
**Prerequisites.** P1-E01-T07; P1-E02-T12, T14; built workflows T18–T48.
**Independence this week.** Measures; changes no behaviour. Runs in time windows separate from T57.
**Estimate and difficulty.** 18 h. V Med, U Med, I Med, S Low, T Med.
**Completion evidence.** The measurement report against each frozen limit.
**Risks and assumptions.** Internal ops manages test infrastructure costs. Use real providers for acceptance evidence; label development stubs separately and never substitute them for production-path measurements.

### P2-E07-T59 — Acceptance evidence run and release ledger

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P2-E07 | 15 | C | 20 h | 17–25 h | High |

**User story.** As the team, its supervisors and you, we want one ledger that says for every requirement and acceptance scenario what is implemented, what evidence exists and what failed or is missing, so that the release decision is made on facts.

**Problem and outcome.** The ledger has been filled task by task since T07, with uneven evidence. After this task every row has been audited, each built scenario whose evidence is missing or older than its last change has been rerun against the real implementation, and absences are checked.

**Scope.** An audited ledger row per active FR-01–58 (not FR-47), NFR-01–09 and AC-01–25, AC-27–58 with implementation reference, accountable person, conditions, evidence and status; a run of each built acceptance scenario on staging; inventory checks that no Today, Outcomes and Reviews, recurring-review, Now/Next/Later, horizon or advertising resource exists in routes, interfaces, schemas, jobs or notifications; a check that Phase 3, 4 and 5 capabilities are shown as unavailable; a plain statement of Phase 2 status.
**Excludes.** Scenarios for unbuilt features, which are recorded as "not built". Repair work (T61, T62).

**Acceptance criteria.**
1. No active requirement or scenario is missing from the ledger; retired FR-47 and AC-26 are shown as retired.
2. Each row marked passed links to evidence from the real implementation; none relies on a mock, a design or a screenshot of an interface alone.
3. No row says "not built"; a row without evidence yet names the task that will supply it (T96 or T98).
4. The inventory checks for removed and deferred surfaces are recorded with their results.
5. Social Profile Import is disabled and labelled Coming Soon; Instagram, X, connected analytics, image generation, billing and Meta Ads are not presented as working.
6. The ledger's summary states whether each PRD §10.2 gate is met, not met or not measurable.

**Requirement references.** PRD §3.1, §9, §10.2, §13.3, §14; FR-37, FR-49, FR-58; AC-16, AC-56, AC-57, AC-58; SYS Appendix A.
**Prerequisites.** All tasks T01–T56 and T65–T69.
**Independence this week.** The ledger's structure has existed since T07, so T57, T58 and T60 write to their own rows without waiting for this task. Its scenario reruns use a separate set of test workspaces and the staging time windows agreed with T57 and T58.
**Estimate and difficulty.** 20 h. V High, U Low, I Med, S Low, T Med.
**Completion evidence.** The ledger and the summary statement.
**Risks and assumptions.** The ledger reports actual evidence, and repair tasks close any technical gaps before final certification.

### P2-E07-T60 — Production environment and release rehearsal

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P2-E07 | 15 | D | 22 h | 18–30 h | Medium |

**User story.** As an operator, I want a production environment that has been deployed, rolled back and restored at least once before any owner uses it, so that the first real incident is not also the first rehearsal.

**Problem and outcome.** Only staging exists. After this task a production environment runs the release candidate under the production configuration and the recovery procedures have been exercised there.

**Scope.** Production environment separate from staging with its own secrets and data, at the production addresses reserved in T12; deployment of the release candidate; a rollback; a restore followed by reapplying the protected record of disconnections and holds that T43 keeps and of removals that T69 adds to it; access controls for operators; the production configuration needed for an ordinary owner to connect a Page.
**Excludes.** Inviting owners. Building restore suppression for removed data (T69); this task repeats T69's drill on production.

**Acceptance criteria.**
1. Production is reachable securely and shares no data or secret with staging.
2. The release candidate is deployed by the pipeline, then rolled back, then redeployed, each recorded with its duration.
3. After a restore, a connection disconnected before the restore is still disconnected, an item removed before the restore is still absent, and no Post is published as a result of the restore.
4. Scheduled work that was in flight at the restore point is held or revalidated, not replayed.
5. Operator access is limited to named people and is recorded.
6. The rehearsal record lists problems and their owners.

**Requirement references.** NFR-03, NFR-05, NFR-09; PRD §3.1, §10.2 (recovery/security), §13.2; SYS §4.5 (restore), §4.9; OD-16, OD-21.
**Prerequisites.** P1-E02-T12, T16; P1-E03-T19; P2-E05-T43; P2-E06-T69; the built product as frozen at the end of Week 13.
**Independence this week.** A separate environment; T57, T58 and T59 use staging.
**Estimate and difficulty.** 22 h. V Med, U Med, I Med, S High, T Med.
**Completion evidence.** The rehearsal record with durations.
**Risks and assumptions.** Internal ops manages production purchasing in parallel. Rehearsals verify compatibility for long-running scheduled work and protected browser-session restoration (SYS R-05).

---

## Week 16 — P2-E07

### P2-E07-T61 — Defect repair: Chat, research, Roadmap and intake

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P2-E07 | 16 | A | 23 h | 23 h fixed | Medium |

**User story.** As a business owner, I want the serious problems found in testing fixed before I am asked to rely on the product, so that known critical faults do not reach me.

**Problem and outcome.** Weekly regression produces an ongoing defect record, expanded by the Week-14/15 checkpoints. After this task the critical and high defects recorded through the final Week-16 candidate in onboarding, Brain, Chat, orchestration, research and Roadmap are repaired and retested, in severity order, with reruns through final certification.

**Scope.** Defects in the named areas recorded in the defect register throughout Week 16, including T63/T98 discoveries, critical first, then high; a retest of each repair; a rerun of the acceptance scenarios the repair could affect; an update of the affected ledger rows and a note of any change to operator-facing behaviour.
**Excludes.** New features. Critical/high defects discovered during Week 16 are included; medium/low polish is prioritized after them.

**Acceptance criteria.**
1. Defects are taken strictly in severity order; the order worked is recorded.
2. Each repaired defect has a retest result from the real implementation.
3. Each critical or high defect not repaired is listed as a release blocker with reproduction steps and an estimate.
4. No repair changes approved product behaviour or removes a control.
5. The ledger rows for repaired items are updated with the new evidence.

**Requirement references.** PRD §10.2 (zero critical violations; no unresolved critical/high in recovery and security), §13.3.
**Prerequisites.** Findings from P2-E07-T53, T54, T56, T57, T59.
**Independence this week.** Starts in its owned areas independently of T62; final repair merges feed T98 certification.
**Estimate and difficulty.** 23 h; continues until critical and high defects in its area are closed. V unknown until Week 15, U High, I Med, S Med, T Med.
**Completion evidence.** The repaired-defect list with retests and the remaining-blocker list.
**Risks and assumptions.** Weekly regression reduces the repair backlog; file 01 reserves human effort for remaining repairs. Critical/high findings require fixes and reruns before promotion.

### P2-E07-T62 — Defect repair: publishing, Scheduling, connection and interface

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P2-E07 | 16 | B | 23 h | 23 h fixed | Medium |

**User story.** As a business owner, I want the serious problems found in publishing and in the screens I use fixed before release, so that a known fault cannot cause a wrong post or stop me completing an action.

**Problem and outcome.** As T61, for a different set of areas. After this task the critical and high defects recorded through the final Week-16 candidate in Posts, approval, publishing, connection, notifications, documents, files, accounts and bilingual interface behaviour are repaired and retested with reruns through final certification.

**Scope.** Defects in the named areas recorded in the defect register throughout Week 16, including T63/T98 discoveries, critical first, then high; retests; a rerun of the acceptance scenarios the repair could affect; ledger updates and a note of any change to operator-facing behaviour.
**Excludes.** New features. Critical/high defects discovered during Week 16 are included; medium/low polish is prioritized after them.

**Acceptance criteria.**
1. Defects are taken strictly in severity order; the order worked is recorded.
2. Each repaired publishing defect is retested against a real client-authorized browser Page.
3. Each critical or high defect not repaired is listed as a release blocker with reproduction steps and an estimate.
4. No repair weakens approval binding, authorization checks or duplicate prevention.
5. The ledger rows for repaired items are updated with the new evidence.

**Requirement references.** PRD §10.2 (publication, usability, recovery/security), §13.3.
**Prerequisites.** Findings from P2-E07-T53, T54, T55, T57, T59.
**Independence this week.** Starts in its owned areas independently of T61; accepts T63/T98 discoveries and feeds final T98 certification.
**Estimate and difficulty.** 23 h; continues until critical and high defects in its area are closed. V unknown until Week 15, U High, I Med, S High, T High.
**Completion evidence.** The repaired-defect list with retests and the remaining-blocker list.
**Risks and assumptions.** As T61.

### P2-E07-T63 — Client-session production publishing validation

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P2-E07 | 16 | C | 16 h | 8–30 h | Low |

**User story.** As a client, I want to authorize my own browser session and publish an approved Post to my Page through production, so that the full product works with my own account.

**Problem and outcome.** Earlier weekly checks exercise the publishing path. This task provides final client-session evidence under the production configuration for AC-30 and FR-33.

**Scope.** Client-controlled authentication, selected Page verification, exact-version approval, scheduled text/link and single-image publication through the pinned Scrapling/Playwright runtime, durable permalink/read-back, automatic dashboard reauthentication and disconnect checks, with evidence tied to the final build.
**Excludes.** Other platforms and unapproved client actions.

**Acceptance criteria.**
1. A client connects their own account/session and Page; no developer-app role or API approval is required.
2. An approved text/link Post and an approved single-image Post publish through the isolated browser context and have matching durable evidence.
3. Expiry or a login challenge automatically exposes the dashboard Re-authenticate action; the scoped flow verifies the intended account/Page and restores encrypted state without releasing held Posts. Disconnect prevents new dispatch authorization under the old generation; in-flight outcomes remain subject to reconciliation.
4. Evidence records build/configuration, client authorization, destination, approved version and verified outcome, with secrets redacted.
5. Failures route to T62/T99 for repair and rerun within Week 16; a relevant later build change invalidates and repeats the evidence.
6. The final AC-30/FR-33 ledger rows and T98 certification reference the same promotable build.
7. Evidence records the adapter, Scrapling, Playwright and browser-image versions and immutable media reference. T55's fault evidence remains valid for this candidate or is rerun; production read-back is attributable to each approved attempt, not merely matching text.

**Requirement references.** FR-33; AC-30 under [file 11](./11-execution-contract.md#publishing-through-authenticated-browser-sessions); AC-12, AC-54; NFR-03, NFR-05.
**Prerequisites.** P1-E01-T03; P2-E05-T38, T43; P2-E06-T47; P2-E07-T55, T60. Final certification consumes the Week-16 repaired candidate.
**Independence this week.** Can start on the current production candidate; completion coordinates explicitly with T62/T99 repairs and T98's final-build certification.
**Estimate and difficulty.** 16 manual-equivalent hours. V Low, U High (session/UI behavior), I Med, S High, T High.
**Completion evidence.** Verified client-session publication of both formats plus safe-session recovery on the final candidate.
**Risks and assumptions.** Session challenges require client reauthentication. Ambiguous publication stays held until reconciliation; no automatic resend.

### P2-E07-T64 — Operator runbooks and owner help

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P2-E07 | 16 | D | 18 h | 15–23 h | High |

**User story.** As an operator on call, I want step-by-step procedures for the failures we expect, and as an owner I want a short explanation of what the product does and does not do, so that problems are resolved correctly and owners are not misled about capabilities.

**Problem and outcome.** Knowledge of how to recover sits with whoever built each part. After this task written procedures exist for the expected incidents, tested by someone who did not write them, with plain owner help for the built workflows.

**Scope.** Runbooks for a stuck Task, an uncertain publication, a revoked Page authorization, a failed import, a cost spike, a failed deployment and a restore; how to find records for an owner's report; owner help covering setup, Chat, Roadmap, Posts and approval, what is owner-reported versus verified, data retention as it really is, and what is not available yet; the graduation evidence index pointing to the ledger.
**Excludes.** Marketing material. Help for unbuilt features.

**Acceptance criteria.**
1. A team member who did not write a runbook resolves a staged stuck Task and a staged uncertain publication using it alone.
2. Each runbook states how to confirm the problem, the safe actions, the actions that must never be taken (for example resending an uncertain post) and when to escalate.
3. No runbook step exposes a secret or owner content unnecessarily.
4. Owner help states retention and deletion limits accurately, including verified derived-copy cleanup, restore suppression and documented backup expiry.
5. Owner help lists Coming Soon and later-phase capabilities as unavailable.
6. Help is available in Arabic and English.

**Requirement references.** NFR-08, NFR-09; FR-49; PRD §10.2 (recovery/security: operator recovery instructions), §13.2 (weeks 23–24 evidence).
**Prerequisites.** P1-E02-T11, T16; P2-E07-T55, T57, T59, T60.
**Independence this week.** Describes behaviour as of the end of Week 15. If a Week 16 repair changes operator-facing behaviour, the repair's own record states the change; the final runbook update explicitly follows the completed repair records before release.
**Estimate and difficulty.** 18 h. V Med, U Low, I Low, S Med, T Med.
**Completion evidence.** The two staged incidents resolved by a second person from the runbooks.
**Risks and assumptions.** None material.

---

## Additional verification and release tasks from the former overflow register (Weeks 14–16, T92–T99)

> Added on 2026-11-14 by the plan owner's instruction to schedule every remaining commitment inside the 16 weeks under the approved AI-assisted capacity model. Each is carried by the named assignee in addition to that person's other task in the week. "Added features" below means the work of T65–T91.

### P2-E07-T92 — Arabic, English and access checks on the added screens

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P2-E07 | 14 | B | 30 h | 24–40 h | Medium |

**User story.** As an Arabic-first business owner, I want every screen, not only the main journeys, to work properly in my language, on my phone and with a keyboard, so that no part of the product feels unfinished.

**Problem and outcome.** T54 covers the critical journeys. After this task the added screens (Settings, Google sign-in, graph, calendar, Batch Review, analysis, export, revisions, campaign packages) meet the same standard.

**Scope.** Each added screen checked in Arabic right-to-left and English left-to-right, at phone width, by keyboard and with a screen reader at the level T07 sets; mixed-script content; dates, numbers and currency by region; repair of defects found.
**Excludes.** Review of generated content (T56, T95).

**Acceptance criteria.**
1. Every added screen has a recorded pass or a logged defect for each check.
2. No untranslated text, clipped text or wrongly mirrored control remains on a passed screen.
3. Every action on a passed screen can be done by keyboard.
4. Defects not repaired in the week are in the defect register with severity.

**Requirement references.** FR-43, FR-44; NFR-07; AC-11, AC-29.
**Prerequisites.** All owner-facing tasks T65–T91.
**Independence this week.** A different set of screens from T54.
**Estimate and difficulty.** 30 h. V High, U Low, I Low, S Low, T Med.
**Completion evidence.** The check record per screen.
**Risks and assumptions.** None material.

### P2-E07-T93 — Brain value comparison and held-out reporting

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P2-E07 | 14 | D | 20 h | 16–28 h | Medium |

**User story.** As a release decision-maker, I want to know whether the Brain makes the Director's work better than documents alone, so that the product's central claim is measured, not assumed.

**Problem and outcome.** Weekly model/context experiments and Brain comparisons already feed implementation refinements. After this task the same cases have been run with Brain context and with documents only, and held-out results are reported separately.

**Scope.** Consolidation of the document-only comparison route used in earlier weekly experiments; runs of the full case set both ways; scoring by the T07 rubric; a report with counts and denominators, overall and per register, with held-out cases separate.
**Excludes.** Human register review (T56, T95). Removing committed features. Findings drive implementation and prompt refinements through the weekly repair process.

**Acceptance criteria.**
1. Every case has a result for both routes, or a recorded reason why not.
2. The report gives counts and denominators, never percentages alone.
3. Held-out results are reported apart from the cases used in tuning.
4. A result that does not favour the Brain is reported as it is.

**Requirement references.** PRD §10.1, §10.2; D-23, D-38.
**Prerequisites.** P1-E03-T24; P2-E04-T25, T71; P2-E06-T91.
**Independence this week.** Automated runs; it does not use T56's reviewers.
**Estimate and difficulty.** 20 h. V Med, U Med, I Med, S Low, T Med.
**Completion evidence.** The comparison report.
**Risks and assumptions.** The result may be unfavourable; it is reported either way.

### P2-E07-T94 — Security and isolation verification of the added features

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P2-E07 | 14 | A | 24 h | 19–32 h | Medium |

**User story.** As a business owner, I want the newer parts of the product tested for safety as hard as the core, so that export, sign-in and file upload cannot expose my business.

**Problem and outcome.** T53 covers the core workflows. After this task the added features have the same verification.

**Scope.** Cross-workspace attempts on every store and screen the added features introduced; account takeover attempts through Google linking; hostile spreadsheets; export links and their expiry; bulk approval and revision approval boundaries; removal and restore suppression; repair of defects found.
**Excludes.** The core workflows (T53).

**Acceptance criteria.**
1. Every added store and module is in the cross-workspace check and passes.
2. No tested path links an account without proof of control.
3. No hostile file causes execution, an internal fetch or data from another workspace.
4. No bulk or revision action approves content the owner was not shown.
5. Each critical finding is repaired and rerun, or recorded as a release blocker.

**Requirement references.** NFR-03, NFR-04, NFR-09; PRD §10.2 (security); AC-17.
**Prerequisites.** All tasks T65–T91; severity definitions from T07.
**Independence this week.** A different feature set from T53.
**Estimate and difficulty.** 24 h. V High, U Med, I Med, S High, T High.
**Completion evidence.** The findings record with reruns.
**Risks and assumptions.** None material.

### P2-E07-T95 — Register tuning and held-out confirmation

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P2-E07 | 15 | A | 20 h | 16–28 h | Low |

**User story.** As an owner who writes to customers in my own dialect, I want the Director's Arabic to have been corrected after native review and checked again, so that I can trust it.

**Problem and outcome.** Weekly tuning records and the expanded T56 checkpoint supply the current findings. After this task the findings of T56 have been acted on and a held-out confirmation gives the per-register result with scorer provenance.

**Scope.** Tuning for each register from T56's findings; a second review round on held-out cases across the four registers; scores with counts and denominators; a statement per register of whether the 80% gate is met.
**Excludes.** Optional independent-reviewer recruitment, handled in parallel by internal quality operations.

**Acceptance criteria.**
1. Each T56 finding is addressed or recorded as not addressed with the reason.
2. Internal confirmation results exist for all four registers; optional independent scores identify their reviewers and coverage.
3. The gate statement per register is met, not met or not measurable, with the numbers.
4. Optional independent scores are labelled unmeasured when absent; team evaluation continues and no independent pass is fabricated.

**Requirement references.** FR-32; NFR-07; PRD §10.2.
**Prerequisites.** P2-E04-T71; P2-E07-T56; weekly internal evaluation records.
**Independence this week.** Content quality only.
**Estimate and difficulty.** 20 h. V Med, U High, I Low, S Low, T High.
**Completion evidence.** The second-round score sheet and gate statement.
**Risks and assumptions.** Optional external feedback (XD-8) runs in parallel; weekly internal evaluation and tuning own the delivery outcome.

### P2-E07-T96 — Acceptance evidence for the added scenarios

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P2-E07 | 15 | C | 24 h | 19–32 h | Medium |

**User story.** As a release decision-maker, I want every acceptance scenario touched by the added features run on the real product with recorded evidence, so that all 57 scenarios can be judged.

**Problem and outcome.** T59 audits the ledger for the primary work. After this task the scenarios the added features complete have evidence in their ledger rows.

**Scope.** Runs of AC-03, AC-04, AC-10, AC-11, AC-12, AC-14, AC-15, AC-23, AC-24, AC-25, AC-29, AC-32, AC-37, AC-38, AC-42, AC-45, AC-47, AC-50, AC-51, AC-52, AC-53 and AC-55 on staging; evidence linked in each row; the Bell showing revision notices; defects logged.
**Excludes.** Scenarios already evidenced by the primary tasks, which T59 audits.

**Acceptance criteria.**
1. Each listed scenario has a recorded run and a status of passed or failed with a defect reference.
2. No row is marked passed on a mock or a design.
3. A revision awaiting approval appears in the Bell and opens the revision.
4. Every failure is in the defect register with severity.

**Requirement references.** PRD §3.1, §9, §10.2; AC-17.
**Prerequisites.** All tasks T65–T94.
**Independence this week.** It fills its own ledger rows; T59 audits other rows. It uses its own test workspaces and the staging time windows agreed on day one.
**Estimate and difficulty.** 24 h. V High, U Low, I Med, S Low, T Med.
**Completion evidence.** The ledger rows with evidence links.
**Risks and assumptions.** Seven tasks share staging this week; use reserved windows and isolated test workspaces.

### P2-E07-T97 — Recovery and fault verification of the added workflows

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P2-E07 | 15 | A | 24 h | 19–32 h | Medium |

**User story.** As a business owner, I want exports, analysis, clean-up and bulk actions to survive interruptions as reliably as the core, so that a restart never leaves my data half-handled.

**Problem and outcome.** T57 covers core durable work. After this task the added background work has been interrupted on purpose and shown to recover.

**Scope.** Interruption of export, spreadsheet analysis, question research, derived-copy clean-up, bulk approval and revision approval; checks that each ends complete or truthfully failed, with no duplicate effect; repair of defects found.
**Excludes.** Core workflows (T57). Publishing faults (T55).

**Acceptance criteria.**
1. Each listed workflow has a recorded interruption and outcome.
2. No interruption leaves a partial approval, a partial revision or a clean-up shown as complete when it is not.
3. No interruption produces a duplicate export, Task or approval.
4. Each failed case is repaired and rerun, or recorded as a release blocker.

**Requirement references.** NFR-05; PRD §10.2 (recovery); AC-17.
**Prerequisites.** P2-E04-T65, T72, T75; P2-E05-T67, T79, T82; P2-E06-T89.
**Independence this week.** Separate workflows and test workspaces from T57.
**Estimate and difficulty.** 24 h. V Med, U Med, I High, S High, T High.
**Completion evidence.** The interruption record.
**Risks and assumptions.** Seven tasks share staging this week; use reserved windows and isolated test workspaces.

### P2-E07-T98 — Full regression of all 57 scenarios on the final repaired candidate

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P2-E07 | 16 | C | 24 h | 19–32 h | Medium |

**User story.** As a release decision-maker, I want all active scenarios evidenced on the final repaired build, so that the promoted product is the one that passed.

**Problem and outcome.** Weekly cumulative regression has already exercised implemented workflows. T98 consolidates and repeats all 57 scenarios after the last Week-16 repair merge.

**Scope.** Start on the available candidate; route discoveries immediately to T61/T62/T99; rerun affected checks after each repair; cut an immutable final candidate; run the full suite after its last merge; include T63 production evidence for the same build; certify the release ledger and removed-surface inventory.
**Excludes.** Implementing repairs, which remain assigned to T61/T62/T99. Their outputs are required for final certification.

**Acceptance criteria.**
1. Every active scenario has an actual result on the final candidate; AC-30 uses T63's client-session production evidence, not an external approval state.
2. Each new failure has a severity and repair owner, including discoveries during Week 16.
3. The full suite runs after all final repairs merge; unresolved critical/high findings prevent promotion until repaired and retested.
4. Build hash, configuration, fixtures and environment identifiers accompany the evidence.
5. A subsequent code or relevant configuration change invalidates certification and triggers a new final-candidate run.
6. The release statement reports observed results for every applicable technical gate; planning approval is not test evidence.

**Requirement references.** PRD §9, §10.2, AC-17 as governed by [file 11](./11-execution-contract.md#continuous-tests-and-final-artifact).
**Prerequisites.** P2-E07-T59, T60, T96, T97 for startup; completed T61/T62/T99 repairs and T63 evidence for final certification.
**Independence this week.** Startup is independent; completion explicitly follows same-week repair merges and production evidence. This replaces the previous blanket independence claim.
**Estimate and difficulty.** 24 manual-equivalent hours with file 01 repair/integration allowance. V High, U Low, I Med, S High, T High.
**Completion evidence.** All 57 scenario results and signed-off technical gate summary for one final candidate.
**Risks and assumptions.** Candidate drift invalidates old results; C owns certification and serializes final evidence collection with T63.

### P2-E07-T99 — Defect repair for the added features

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P2-E07 | 16 | D | 24 h | 19–40 h | Low |

**User story.** As a business owner, I want the problems found in the newer features fixed before release, so that every part of the product I can reach works.

**Problem and outcome.** T61 and T62 repair the core areas. After this task the critical and high defects recorded against the added features through the final Week-16 candidate are closed.

**Scope.** Repair, in severity order, of defects logged against T65–T91 by T92, T94, T96, T97 and Week-16 T63/T98 runs; a rerun of each affected scenario; an update of the ledger rows.
**Excludes.** Defects owned by T61/T62. Week-16 findings in added features, including T63/T98 discoveries, are included here.

**Acceptance criteria.**
1. Every critical and high defect in this task's area is closed with a rerun of its scenario.
2. Each repair is reviewed by a second person.
3. Any defect not closed is listed with severity and reproduction steps in the release summary.

**Requirement references.** PRD §10.2; AC-17.
**Prerequisites.** Findings of T92, T94, T96, T97.
**Independence this week.** Its area list does not overlap T61/T62; it accepts same-week findings and supplies final repair merges to T98.
**Estimate and difficulty.** 24 h. V unknown, U High, I Med, S Med, T Med.
**Completion evidence.** The closed defect list with reruns.
**Risks and assumptions.** Weekly regression and reserved integration effort support closure; critical/high findings are repaired and rerun before final certification.
