# E. Task Descriptions — Phase 1 (Weeks 1–6, T01–T24)

> Part of the [BUSMORA 16-week implementation plan](./README.md). Epics: [D](./03-epics.md). Dependencies and weekly independence checks: [F](./07-dependency-map.md).
> Each estimate includes research, decisions inside approved constraints, implementation, tests, peer review, integration with earlier work, expected fixes and the acceptance demonstration. Implementation choices belong to the assignee.
> These are proposed backlog items for review. Nothing here has been started.
> Every task also meets the shared Definition of Done in the [README](./README.md#definition-of-done-for-every-task).
> Difficulty is rated on five factors: **V**olume, **U**ncertainty, **I**ntegration, **S**ecurity/data risk, **T** verification difficulty — each Low, Med or High.

---

## Week 1 — P1-E01

### P1-E01-T01 — Backend runs locally with automated checks

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P1-E01 | 1 | A | 22 h | 18–28 h | Medium |

**User story.** As a developer on the team, I want to start every backend service and data store on my own machine with one documented procedure, so that I can build and verify features without waiting for anyone else's environment.

**Problem and outcome.** No repository or environment exists. After this task the backend repository holds the three approved backend deployables in runnable skeleton form with their local stores, and every proposed change is checked automatically.

**Scope.** The backend repository; skeleton services for the API, workers and AI deployables that start and report health; local instances of each approved store and of the durable workflow service; automated checks on proposed changes; written start-up instructions; test tooling selected (OD-12).
**Excludes.** Any domain feature, staging, the frontend, the cross-language work path (T06).

**Acceptance criteria.**
1. A team member who did not write the instructions starts all backend process roles and stores on a clean machine by following them, without help.
2. Every process role answers a health check and reports that it can reach the stores it is allowed to use.
3. A proposed change that fails a check cannot be merged; a passing change can.
4. One example test runs in each language runtime as part of the automated checks.
5. No secret is stored in the repository; the instructions say how a developer supplies local secrets.

**Requirement references.** SYS §2.4, §4.3, §4.9; ADR-002, ADR-003, ADR-008; OD-12, OD-15, OD-17; K-02, K-03.
**Prerequisites.** None.
**Independence this week.** Creates its own repository. T02 builds a separate repository and shows no backend data. T03 and T04 work outside both repositories.
**Estimate and difficulty.** 22 h. V High (three services, several stores, two languages), U Med, I Med, S Low, T Low.
**Completion evidence.** A second team member records a clean-machine start and a blocked failing change.
**Risks and assumptions.** A machine with too little memory for all stores slows every later task; record the minimum specification.

### P1-E01-T02 — Bilingual app shell in its own repository

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P1-E01 | 1 | B | 20 h | 17–24 h | High |

**User story.** As a developer building owner screens, I want an application shell that already handles Arabic right-to-left and English left-to-right with the approved visual tokens, so that every screen I add is bilingual and consistent from its first version.

**Problem and outcome.** No frontend exists. After this task the frontend repository serves a shell with the six primary areas as empty destinations, a language switch and the design tokens, with automated checks.

**Scope.** Frontend repository and checks; approved design tokens; shell and navigation for Marketing Director, Roadmap, Business Brain, Deliverables, Scheduling and Settings; top bar; language switch; direction handling; base text, button and input components; translation mechanism selected (OD-7).
**Excludes.** Any data from the backend, authentication, the full component kit (T10), the landing page.

**Acceptance criteria.**
1. Switching language changes direction, alignment and text for every shell element without a reload losing the choice.
2. Each of the six areas opens at its approved route and shows an honest empty state, not sample data.
3. No route, menu entry or label exists for Today, Outcomes & Reviews or any daily dashboard.
4. The shell is usable at phone width and by keyboard alone; focus is visible.
5. Mixed Arabic and English text, numbers and a link display in a readable order in both directions.
6. A failing check blocks a proposed change.

**Requirement references.** FR-02, FR-43, FR-44, FR-58; NFR-07; AC-56; DS §§2–4, §10.1, §11, §14.2; FTD §2.3, FTD-ADR-07, FTD-ADR-12; OD-7.
**Prerequisites.** None.
**Independence this week.** Separate repository; consumes no backend output.
**Estimate and difficulty.** 20 h. V Med, U Low, I Low, S Low, T Med (two directions, phone width).
**Completion evidence.** Screen recording of the shell in both languages on a phone-width and a desktop window, operated by keyboard.
**Risks and assumptions.** The translation mechanism chosen here binds all later screens; the choice and reason are recorded.

### P1-E01-T03 — Facebook access path, owner eligibility and dependency register

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P1-E01 | 1 | C | 18 h | 14–26 h | Low |

**User story.** As the team's integration lead, I want written evidence of what Meta requires before an ordinary owner can connect a Page and publish, and of whether we can meet it, so that the release gate AC-30 is planned on facts instead of hope.

**Problem and outcome.** Production access, the intended owner, the legal entity, budget, payer, reviewers and real availability are all unconfirmed. After this task each is recorded as confirmed, unconfirmed or blocked, with the next action and its owner.

**Scope.** Investigation of the selected Meta app configuration, required permissions and access levels, business verification, review steps and committed formats; a first real test post from a tester role with a public-visibility check; a dependency register covering the intended owner, legal entity, existing Meta app, service budget and payer, language reviewers, team availability and calendar constraints; a list of every account the project needs (source hosting, Meta app with tester roles and a test Page, model and search providers, email delivery, cloud, durable workflow service, tracing, product domain) with a named person responsible for opening each and the week it is needed; filing business verification with Meta in this week if a legal entity exists, because it needs no working product; a named person responsible for finding the intended owner and the language reviewers.
**Excludes.** Product code, the in-product connection (T19), the submission itself (T38). This task does not obtain any approval and does not itself pay for or open the accounts.

**Acceptance criteria.**
1. The record names each permission and access level needed for text/link and single-image posts, and the review and verification steps for each, with a link to the platform's own statement.
2. The record states whether a post published by a tester role was visible to a person with no role on the app, with a screenshot.
3. Every dependency in the register has a status, an owner and a date by which it must be resolved to avoid delaying a named task.
4. Items nobody could confirm are marked unconfirmed; none is filled with a guess.
5. The record lists what blocks submission today and which planned task removes each blocker.
6. Team availability for Weeks 1–16 is collected from all four people and compared with the 20-hour assumption.
7. Every needed account has a named responsible person and a needed-by week; the record states plainly whether a payer is confirmed. If none is confirmed by the end of Week 1, this is escalated to the plan owner as a stop condition for Weeks 2–3.
8. Business verification is filed, or the record says why it could not be (for example no legal entity) and who was told.

**Requirement references.** FR-33; AC-30; PRD §3.1, §11.1 (Facebook, capacity, launch cohort); SYS ADR-018, R-01, R-02, OD-23; D-54.
**Prerequisites.** None.
**Independence this week.** Works in the platform's developer tools and with people. Needs no repository.
**Estimate and difficulty.** 18 h. V Low, U High (external rules, unknown eligibility), I Low, S Med, T Med.
**Completion evidence.** The access record and the dependency register, reviewed by the team at the end of Week 1.
**Risks and assumptions.** If no legal entity can pass verification, AC-30 cannot pass and Phase 2 cannot be declared complete; this is escalated to you the same week. Developer-account access proves nothing about ordinary owners.

### P1-E01-T04 — Research source and provider baseline

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P1-E01 | 1 | D | 20 h | 16–26 h | Medium |

**User story.** As the person who will build research, I want measured results for how well candidate search and retrieval providers find and return Arabic and regional business sources, so that the research capability is built on a provider that can meet its commitments.

**Problem and outcome.** Provider choice for public search and retrieval is open (OD-22). After this task one provider path is selected from measurements of access, regional relevance, cost, latency and how reliably usage can be bounded.

**Scope.** A fixed set of about 20 research questions drawn from the launch cohort in Egyptian and Gulf markets; trials of candidate providers including the retained retrieval adapter; measurement and a written selection with a fallback.
**Excludes.** Product code, the research feature (T36), model selection (T08).

**Acceptance criteria.**
1. For each candidate the record reports how many of the fixed questions returned at least one relevant, accessible source, split by market and language.
2. Cost and latency per query are measured, not quoted from price lists alone.
3. The record states whether each provider's usage can be capped before a call; a provider that cannot be capped is marked unsuitable for autonomous use.
4. Source types that could not be reached (for example private groups) are listed as gaps.
5. One provider path and one fallback are selected with the reason; unpaid trial limits that affected the result are stated.

**Requirement references.** FR-24, FR-27, FR-28; NFR-06; PRD §11.1 (research); OD-22; D-32.
**Prerequisites.** None.
**Independence this week.** Standalone experiments with their own notes; no repository needed.
**Estimate and difficulty.** 20 h. V Med, U High, I Low, S Low, T Med.
**Completion evidence.** The baseline record with raw results for every question and candidate.
**Risks and assumptions.** Assumes trial or paid access can be obtained in Week 1 (unconfirmed budget). If not, the record states which candidates could not be tested.

---

## Week 2 — P1-E01

### P1-E01-T05 — Published API contract with typed frontend access

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P1-E01 | 2 | A | 16 h | 13–20 h | High |

**User story.** As a developer working on either side of the public interface, I want the backend's published contract to generate the types the frontend uses and to reject incompatible changes, so that a change on one side cannot silently break the other.

**Problem and outcome.** Two repositories need one agreed contract (ADR-015). After this task the frontend reads a live backend value through generated access, and an incompatible change fails a check.

**Scope.** Publication of the contract from the backend; generated typed access in the frontend; an automated incompatibility check; the documented way a developer runs both repositories together.
**Excludes.** Contracts between the language runtimes inside the backend (T06). Any business endpoint.

**Acceptance criteria.**
1. The shell displays a value fetched from the running backend using only generated types.
2. Removing or renaming a published field causes the check to fail with a message naming the field.
3. Adding an optional field passes.
4. A developer runs both repositories together locally by following the written procedure.

**Requirement references.** SYS §6.1–6.3; ADR-015; OD-17; D-51.
**Prerequisites.** P1-E01-T01, P1-E01-T02.
**Independence this week.** Uses only endpoints that existed at the end of Week 1. T06 adds its own endpoint and does not need this pipeline.
**Estimate and difficulty.** 16 h. V Low, U Low, I Med, S Low, T Low.
**Completion evidence.** A recorded failing check for a deliberate incompatible change.
**Risks and assumptions.** None material.

### P1-E01-T06 — Durable cross-language work path proven end to end

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P1-E01 | 2 | B | 24 h | 20–34 h | Low |

**User story.** As the team, we want one request to travel the whole approved path once — public interface, durable workflow, both language runtimes, live event stream — so that we know the architecture works before building features on it.

**Problem and outcome.** The polyglot design is the largest technical risk (SYS R-11, §9.4 spike 1). After this task a stub piece of work runs through every hop, survives a worker restart, and its events reach a client that reconnects.

**Scope.** A request that starts durable work; a workflow in one runtime calling an activity in the other; a stub AI step that emits progress events; delivery of those events to a client with resume after disconnection; validated payloads between runtimes with a drift check.
**Excludes.** Real model calls, Tasks as a domain concept (T22), the Canvas interface (T33), authentication.

**Acceptance criteria.**
1. Starting the stub work returns an acknowledgement within the provisional two-second target on a developer machine.
2. Stopping and restarting a worker mid-run does not lose the work; it completes.
3. A client that disconnects and reconnects receives the events it missed, in order, with none repeated.
4. A payload that does not match the agreed shape is rejected at the boundary, and a mismatch between the two runtimes' definitions fails an automated check.
5. The record states measured acknowledgement and event-delivery times.

**Requirement references.** FR-23; NFR-05; SYS §4.6, §6.4, §9.4 (spike 1), R-04, R-11; ADR-003, ADR-014; D-27, D-34.
**Prerequisites.** P1-E01-T01.
**Independence this week.** Adds its own entry point; does not need T05's generated client. Uses a stub, so does not need T08's model choice.
**Estimate and difficulty.** 24 h. V Med, U High (first use of the durable workflow service across languages), I High, S Low, T Med.
**Completion evidence.** A recorded run with a worker killed mid-way and a client reconnecting.
**Risks and assumptions.** If the path cannot be made to work in the week, the architecture risk is confirmed and DR-4 becomes urgent.

### P1-E01-T07 — Evaluation standard frozen

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P1-E01 | 2 | C | 20 h | 17–24 h | High |

**User story.** As the team and its supervisors, we want the quality bar written down and dated before features are built, so that nobody can lower it later to make a failing release pass.

**Problem and outcome.** PRD §10.2 requires the rubric, sampling, severity definitions and runtime limits frozen by the end of Week 2. After this task they exist, with four exemplar cases and one supported owner scenario.

**Scope.** Human rubric for decision and content usefulness and for the four registers; critical and high defect definitions; sampling and held-out rule; declared release load; provisional task-specific latency, timeout, usage and concurrency limits; the change rule; one launch-cohort owner scenario; four authored exemplar cases, one per register; reviewer needs per register; the release ledger created with one empty row per active FR, NFR and AC, and a defect register with the severity scale, both to be filled by each later task as it finishes (PRD §13.3 says "maintain a ledger").
**Excludes.** The remaining cases (T23, T71, T93), running any evaluation, recruiting reviewers (tracked in the dependency register).

**Acceptance criteria.**
1. Each rubric dimension has a scale, a pass threshold and a worked example of a pass and a fail.
2. "Critical" and "high" each have a definition a second person applies to five sample defects with the same result as the author.
3. The held-out rule says which cases may never be used for prompt tuning and how that is enforced.
4. Each numeric limit names its source: a measurement from T04 (search cost and latency), or an approved provisional target from SYS §2.2; none is invented. Limits that await a later measurement (for example quick-reply time in T18) are marked provisional with the task that will confirm them.
5. The standard is dated, and the rule for changing it requires a written reason.
6. Each exemplar case states the business, the request, the material facts and what a defensible answer must contain.

**Requirement references.** PRD §10.1–10.2, §11.1 (evaluation); SYS §2.2, §7.4, OD-24; strategy/evaluation.md §§3–4, §6; D-37, D-60.
**Prerequisites.** P1-E01-T04 (measurements, already available). Approved documents.
**Independence this week.** Limits use T04 results and approved provisional targets, so it does not wait for T08's model measurements.
**Estimate and difficulty.** 20 h. V Med, U Med, I Low, S Low, T Med.
**Completion evidence.** The dated standard and four cases, with the second-person severity check.
**Risks and assumptions.** Limits set before real slices exist may need a documented revision at Gate G1.

### P1-E01-T08 — Arabic model baseline across four registers

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P1-E01 | 2 | D | 20 h | 16–26 h | Medium |

**User story.** As the person who will build the Director, I want measured quality, cost and speed for candidate models in English, Egyptian Arabic, Gulf-oriented Arabic and Modern Standard Arabic, so that routing and writing use models chosen from evidence.

**Problem and outcome.** Model providers are open (OD-4, D-31). After this task a light model for routing and a heavier model for planning and writing are selected, each with a fallback and measured cost.

**Scope.** A fixed prompt set covering routing, short replies, a diagnosis and one post per register; trials of candidate models through the approved gateway approach; measurement of quality, latency and cost; a selection record with cost ceilings.
**Excludes.** Product integration (T18), embedding choice (T24), human native review (T56).

**Acceptance criteria.**
1. Every candidate is run on every prompt; outputs are kept for inspection.
2. Latency and cost per call are measured for each candidate.
3. Quality notes are recorded per register; a register nobody on the team can judge is marked "not judged", not scored.
4. The selection names a light and a heavy model with fallbacks and states the expected cost of a quick reply and of a typical task.
5. The record states that the selection is provisional until native reviewers score outputs in T56.

**Requirement references.** FR-19, FR-32; NFR-06, NFR-07; PRD §11.2; SYS §4.8, OD-4; D-31.
**Prerequisites.** None.
**Independence this week.** Standalone; uses its own prompt set and does not need T07's rubric.
**Estimate and difficulty.** 20 h. V Med, U High, I Low, S Low, T High (quality judgement without native reviewers).
**Completion evidence.** The selection record with all outputs and measurements.
**Risks and assumptions.** Needs provider accounts and a payer (unconfirmed). Gulf quality cannot be judged without a suitable reviewer.

---

## Week 3 — P1-E02

### P1-E02-T09 — Owner account with email verification and session

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P1-E02 | 3 | A | 22 h | 18–28 h | Medium |

**User story.** As a business owner, I want to register with only an email and password, confirm my email from a link, and sign in and out, so that I have a private workspace before I enter anything about my business.

**Problem and outcome.** No account exists. After this task an owner can register, verify, sign in, stay signed in across page reloads and sign out, and a workspace is created for them.

**Scope.** Registration, verification link with resend, sign-in, sign-out, session continuity, automatic single-owner workspace, the auth screens in both languages, email delivery provider selected (OD-10).
**Excludes.** Password reset and session hardening (T13), Google sign-in (T70), onboarding (T26).

**Acceptance criteria.**
1. Registration asks for email and password only and creates a workspace with no business facts.
2. The verification link works once; an invalid, expired or already-used link fails with a clear message and offers resend.
3. An unverified owner cannot reach the application areas.
4. After sign-in, reloading a page keeps the owner signed in; sign-out ends the session on that device.
5. Public responses do not reveal whether an email is registered.
6. Every screen and error works in Arabic and English.

**Requirement references.** FR-01, FR-50 (email link); NFR-03; AC-31; SYS §4.7, §4.7.1; ADR-012; D-29, D-61; OD-10.
**Prerequisites.** P1-E01-T01, T02, T05.
**Independence this week.** Uses the Week 1 base components, not T10's kit. Demonstrated locally, so it does not need T12's staging.
**Estimate and difficulty.** 22 h. V Med, U Med, I Med, S High, T Med.
**Completion evidence.** Demonstration of AC-31 including a replayed and an expired link.
**Risks and assumptions.** Email delivery needs a provider account (unconfirmed payer).

### P1-E02-T10 — Shared bilingual components and structured forms

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P1-E02 | 3 | B | 18 h | 15–24 h | High |

**User story.** As a developer building owner screens, I want one set of accessible bilingual components and a form renderer that turns a described question into the right control, so that onboarding, clarifications and Discovery all behave the same way without each being built from scratch.

**Problem and outcome.** Several features need choice-first forms and the same cards, overlays and status indicators. After this task they exist once, documented and tested in both directions.

**Scope.** Form controls, cards, overlays, navigation elements, data display, status indicators; a structured form renderer supporting single choice, multiple choice, dropdown, text, number and link inputs, an Other/Custom answer, and Unknown, Skip and Not Applicable where a question allows them.
**Excludes.** Any feature screen, calendar and graph components.

**Acceptance criteria.**
1. Each component renders in both directions and is operable by keyboard with visible focus and an accessible name.
2. A question described as single choice renders as radio options; multiple choice as checkboxes; a required number refuses non-numeric input with a message.
3. Unknown, Skip and Not Applicable are returned as distinct answers, never as empty or zero.
4. The renderer never adds an option that was not in the question it was given.
5. Colour contrast for text and controls meets the approved accessibility level.
6. A catalogue page shows every component in both languages.

**Requirement references.** FR-18, FR-43; NFR-07; AC-35 (controls); DS §8, §9, §12; FTD-ADR-10, FTD-ADR-12.
**Prerequisites.** P1-E01-T02.
**Independence this week.** Frontend only; T09's screens use the Week 1 base components.
**Estimate and difficulty.** 18 h. V Med, U Low, I Low, S Low, T Med.
**Completion evidence.** The catalogue page reviewed in both languages with keyboard only.
**Risks and assumptions.** None material.

### P1-E02-T11 — Operator can trace a request across services

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P1-E02 | 3 | C | 18 h | 15–24 h | Medium |

**User story.** As an operator, I want to follow one request through every service and see errors in one place without reading secrets, so that I can find out what failed when an owner reports a problem.

**Problem and outcome.** Logs, metrics and error tracking beyond model tracing are undecided (OD-13, OD-9). After this task one identifier links a request across all process roles and errors are collected centrally with redaction.

**Scope.** Structured logs with a shared identifier across roles and through the durable workflow; central error collection; the model-call tracing service set up and shown on a test call (real model calls start in T18); redaction of secrets and private content; a short operator guide.
**Excludes.** Dashboards, alerts, the runbooks (T64), enabling it on staging (done in T16).

**Acceptance criteria.**
1. Given one identifier, an operator finds the matching records in every process role the request touched.
2. A deliberately triggered error appears in the central collection with its identifier within one minute.
3. A test secret and a test private message placed in a request do not appear in any log or error record.
4. The guide lets a second person repeat criterion 1 without help.

**Requirement references.** NFR-08; SYS §7.4, OD-9, OD-13; D-33.
**Prerequisites.** P1-E01-T06.
**Independence this week.** Verified in the local environment; does not need T12's staging or T09's accounts.
**Estimate and difficulty.** 18 h. V Med, U Med, I Med, S Med, T Low.
**Completion evidence.** A second person traces a request and confirms the redaction test.
**Risks and assumptions.** Tool choice may add cost (unconfirmed budget).

### P1-E02-T12 — Staging environment with automated deployment

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P1-E02 | 3 | D | 22 h | 18–34 h | Low |

**User story.** As the team, we want every merged change deployed to a shared environment automatically, so that we verify features where they will really run and external platforms can reach us.

**Problem and outcome.** The cloud provider, secrets store and deployment path are open (OD-2, OD-5, OD-16). After this task a staging environment runs all process roles on managed stores, updated by the pipeline, reachable securely.

**Scope.** Provider selection inside the approved single-region decision; managed stores including object storage (vendor chosen here, D-41) and the key-management service later used to protect Page credentials; all eight process roles deployed; the product domain with the hostnames staging uses now and production will use later; secure public access to the web and API only; secrets store; automated deployment on merge; a one-step rollback to the previous version.
**Excludes.** Production (T60), backup drill (T16), load testing.

**Acceptance criteria.**
1. A merged change reaches staging with no manual step on a server.
2. Only the web application and the public API are reachable from outside; a request to any other role from outside fails.
3. Secrets are read from the secrets store; none is in a repository or an image.
4. Rolling back to the previous version takes one documented action.
5. Monthly running cost of staging is estimated from the provider's own figures and recorded.
6. The provider and region choice is recorded with its reason.

**Requirement references.** NFR-03, NFR-05; SYS §4.9, OD-2, OD-5, OD-16; ADR-011, ADR-016; K-11.
**Prerequisites.** P1-E01-T01, T02, T06.
**Independence this week.** Deploys what existed at the end of Week 2. T09 and T11 are verified locally and arrive on staging through the pipeline afterwards.
**Estimate and difficulty.** 22 h. V Med, U High (provider choice, eight roles), I Med, S High, T Med.
**Completion evidence.** A merge observed arriving on staging, and an outside request to an internal role refused.
**Risks and assumptions.** Needs a paying account in Week 3 (unconfirmed). Without it the task cannot complete and Gate G1 slips. Reviewers judged this task larger than one person-week; if it overruns, T16 and T19 slip and the overrun is recorded against DR-8.

---

## Week 4 — P1-E02

### P1-E02-T13 — Account recovery and session security

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P1-E02 | 4 | A | 20 h | 16–25 h | High |

**User story.** As a business owner, I want to recover a forgotten password and know that a stolen session is shut down, so that my workspace stays mine.

**Problem and outcome.** T09 gives basic sign-in. After this task password reset works, a reused session credential revokes the session, several tabs stay signed in together, and public endpoints are rate-limited.

**Scope.** Password reset by single-use link; revocation of sessions on reset; reuse detection; behaviour across tabs; idle and absolute session limits per the approved design; rate limiting for public auth endpoints (OD-14); authentication of live event streams by the approved short-lived stream credential (ADR-013), with streams closing on revocation.
**Excludes.** Google sign-in and account linking (T70).

**Acceptance criteria.**
1. A reset link works once and expires; after a reset, all earlier sessions are ended.
2. Presenting an already-used session credential ends that whole session family.
3. With two tabs open, both remain signed in after the session renews.
4. Repeated failed sign-ins or reset requests are slowed or refused, with a message that reveals nothing about the account.
5. A live event stream cannot be opened without the stream credential, and one opened before sign-out stops delivering after sign-out.
6. Registration, verification email, sign-in and reset are shown working on staging with their secrets in the secrets store.

**Requirement references.** FR-01, FR-50; NFR-03; SYS §4.7; ADR-012, ADR-013; OD-14.
**Prerequisites.** P1-E02-T09.
**Independence this week.** Changes only account behaviour. T15 covers service-to-service identity, not owner sessions.
**Estimate and difficulty.** 20 h. V Med, U Low, I Med, S High, T High (timing and multi-tab cases).
**Completion evidence.** Demonstration of reuse detection and multi-tab renewal.
**Risks and assumptions.** None material.

### P1-E02-T14 — Audit, usage and notification records

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P1-E02 | 4 | B | 18 h | 15–23 h | High |

**User story.** As an operator, I want every governed action, costly call and attention-worthy event recorded in a uniform way from the first feature onward, so that I can explain what happened and what it cost.

**Problem and outcome.** Later features must write audit, usage and notification records in the same transaction as the change they describe. After this task the mechanism exists and is shown on the account events that already happen.

**Scope.** Append-only audit records with minimal metadata; usage records by resource kind, quantity, estimated cost, workspace and task; notification records with kind, exact subject reference and read state; the internal event mechanism features use to write them; an operator query.
**Excludes.** The Notification Bell interface (T51), budgets (T42), any dashboard.

**Acceptance criteria.**
1. Registration and email verification each produce one audit record naming the action, actor, subject and time.
2. An audit record cannot be changed or removed through any application path.
3. A usage record written for a test call is retrievable by workspace and by task and includes failed calls.
4. A record and the change it describes are saved together or not at all.
5. Records contain no passwords, tokens or message content.
6. A request for another workspace's records is refused.

**Requirement references.** FR-13 (change record), FR-56 (records); NFR-06, NFR-08, NFR-09; SYS §3.5, §4.5; BTD-ADR-04; D-19, D-35.
**Prerequisites.** P1-E02-T09.
**Independence this week.** Demonstrated on events that existed at the start of the week. Carries its own workspace check, so it does not need T15.
**Estimate and difficulty.** 18 h. V Med, U Low, I Med, S Med, T Low.
**Completion evidence.** Operator query showing audit and usage records for a test workspace.
**Risks and assumptions.** Minimal audit fields follow the approved design; T16's policy may add a retention duration later.

### P1-E02-T15 — Workspace isolation proven in every store

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P1-E02 | 4 | C | 22 h | 18–28 h | Medium |

**User story.** As a business owner, I want certainty that no other workspace can read my knowledge, files, work or credentials, so that I can trust the product with my business.

**Problem and outcome.** Isolation must hold in five stores and across the internal AI service (NFR-03, G2). After this task the mechanisms exist and a repeatable check proves that cross-workspace attempts fail for everything that exists so far. Stores that hold no product data yet are checked with synthetic test records; the tasks that later put real data in them (T17, T21, T24) extend the check.

**Scope.** Workspace scoping for the database including the read-only access used by the AI runtime; vector index filters; object storage paths; cache and stream keys; the short-lived service identity between the API and the AI service; a reusable cross-workspace check, run on real account data and on synthetic records in the vector index and object storage, that later tasks extend.
**Excludes.** Feature-level authorization rules (each feature task), penetration testing (T53).

**Acceptance criteria.**
1. With two test workspaces, an attempt by one to read the other's data fails in each store, shown by the check.
2. The AI runtime cannot write domain data and cannot read rows outside the workspace named in its verified identity.
3. A call to the AI service without a valid service identity, or with an expired one, is refused.
4. The check runs automatically and fails the build if any cross-workspace read succeeds.
5. A written checklist tells the author of a new store or module what to add to the check.

**Requirement references.** FR-01; NFR-03; AC-17 (isolation); SYS §4.5, §4.7, §7.1, R-07; ADR-010; BTD-ADR-06, BTD-ADR-08; G2.
**Prerequisites.** P1-E02-T09, P1-E01-T06.
**Independence this week.** Covers stores that existed at the start of the week. T14's new records follow the approved scoping rule and carry their own check.
**Estimate and difficulty.** 22 h. V Med, U Med, I High, S High, T High.
**Completion evidence.** The automated check output for both workspaces across all stores.
**Risks and assumptions.** A gap found later in a new module is a critical defect by the T07 definition.

### P1-E02-T16 — Data lifecycle policy and first restore and rollback drill

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P1-E02 | 4 | D | 20 h | 16–26 h | Medium |

**User story.** As an operator, I want a written policy for how long data is kept and how it is removed, and proof that we can restore and roll back, so that recovery is a practised procedure and owners are told the truth about retention.

**Problem and outcome.** PRD §11.1 requires the data governance design by Week 4 (OD-21). After this task the policy exists and staging has been restored and rolled back once.

**Scope.** Policy for active-data deletion, derived records, backup expiry, export, logs and traces, token lifecycle and the minimum audit data kept; a staging restore from backup; a staging rollback; enabling request tracing and error collection on staging; the public privacy, data-deletion and terms pages that state the policy, which Meta's review also requires.
**Excludes.** Building deletion across derived stores (T65), restore suppression (T69) or export (T67). Production rehearsal (T60).

**Acceptance criteria.**
1. The policy states, for each kind of data, how long it is kept, what deletion removes, and what cannot be removed immediately (for example backups) and why.
2. The policy states how a restore must respect earlier deletions and disconnections.
3. A restore of the staging database to a chosen earlier point succeeds; time taken and data lost are recorded.
4. A rollback to the previous deployed version succeeds; time taken is recorded.
5. During the drill an operator sees staging errors and traces using T11's guide.
6. Problems found are listed with an owner.
7. The public pages are reachable at the product domain and say the same as the policy.

**Requirement references.** FR-13; NFR-05, NFR-09; PRD §11.1 (data governance); SYS §4.5 (retention), OD-21; G3.
**Prerequisites.** P1-E02-T12, P1-E02-T11.
**Independence this week.** Uses staging as it stood at the start of the week. The policy describes approved design; it does not change T14's records.
**Estimate and difficulty.** 20 h. V Med, U Med, I Med, S High, T Med.
**Completion evidence.** The policy and the drill record with timings.
**Risks and assumptions.** Provider backup features may limit the restore point; the limit is recorded, not hidden.

---

## Week 5 — P1-E03

### P1-E03-T17 — Knowledge records with provenance and lifecycle

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P1-E03 | 5 | A | 20 h | 17–26 h | High |

**User story.** As a business owner, I want each thing the product knows about my business stored with where it came from and whether I confirmed it, so that a guess is never presented to me as a fact.

**Problem and outcome.** The Brain needs one record set that later features read and write. After this task knowledge items and relationships persist with category, epistemic status, lifecycle, provenance and history, and an owner can see and add one in a minimal list.

**Scope.** Knowledge items in the nine approved categories; typed relationships; the four epistemic statuses; lifecycle including candidate, active, rejected, superseded and retired; provenance; version history on change; explicit unknown and not-applicable values; a minimal accessible list with add and edit.
**Excludes.** Onboarding activation (T26), proposal review (T46), governance and deletion (T50), graph (T77), retrieval (T24).

**Acceptance criteria.**
1. An item added by the owner is stored as owner-provided and active, with time and provenance.
2. Editing an item keeps the earlier version readable and marks the new one current.
3. A candidate item is not shown as current knowledge and can move only to active or rejected.
4. Unknown and not-applicable are stored and shown as such, never as empty or zero.
5. A relationship can only join two items in the same workspace.
6. Each change writes an audit record; the list works in both languages and by keyboard.
7. The cross-workspace check from T15 is extended to knowledge records and the AI runtime's read-only access to them.

**Requirement references.** FR-08, FR-09, FR-10 (owner edits), FR-12 (list); NFR-01; SYS §3.3, §3.4, §4.5; ADR-007; D-20, D-21, D-24.
**Prerequisites.** P1-E02-T09, T14, T15.
**Independence this week.** Its own module and screen. T18's replies do not read the Brain yet.
**Estimate and difficulty.** 20 h. V Med, U Low, I Med, S Med, T Med.
**Completion evidence.** Demonstration of add, edit with history, and a candidate that is not treated as current.
**Risks and assumptions.** Category and relationship lists follow the approved design; changing them later needs a decision.

### P1-E03-T18 — Conversations with real quick replies

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P1-E03 | 5 | B | 22 h | 18–30 h | Medium |

**User story.** As a business owner, I want to write to the Marketing Director and get a real, direct answer to a simple message, so that I can start talking without setting up a plan first.

**Problem and outcome.** There is no conversation yet. After this task the owner can hold a saved conversation, simple messages get a routed model reply through the approved synchronous path, and substantive requests are recognised honestly as work that will need a Task.

**Scope.** Conversations and messages; the Chat screen at its approved route; the routing step using the model selected in T08; direct replies; honest handling of substantive requests before Tasks exist; usage recorded per call; measured reply time.
**Excludes.** Task launch (T22), orchestration (T25), Brain context (T24), clarification forms (T40).

**Acceptance criteria.**
1. A simple message receives a complete reply; the 95th-percentile reply time over 50 messages is measured against the provisional three-second target and recorded.
2. A substantive request is not answered with invented specialist work; the reply says the request needs a Task.
3. Conversations and messages persist across sign-out and sign-in and are listed for the owner.
4. Every model call writes a usage record, including a failed call.
5. If the AI service is unavailable, the owner sees a clear failure and can resend; nothing is lost.
6. The reply follows the conversation's language; the screen works in both interface languages.

**Requirement references.** FR-02, FR-17, FR-19; NFR-01, NFR-06; AC-06 (simple input); SYS §4.6, §9.4 (spike 2), R-06; ADR-004, ADR-020; D-48.
**Prerequisites.** P1-E01-T06, T08; P1-E02-T09, T14, T15.
**Independence this week.** Does not read knowledge, documents or connections.
**Estimate and difficulty.** 22 h. V Med, U Med (latency), I High, S Med, T Med.
**Completion evidence.** The latency measurement and a transcript showing a direct reply and an honest handling of a substantive request.
**Risks and assumptions.** If the three-second target is missed, the result is recorded for T58 and the limit is reviewed under the change rule.

### P1-E03-T19 — Facebook Page connection with protected credentials

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P1-E03 | 5 | C | 20 h | 16–30 h | Low |

**User story.** As a business owner, I want to connect my Facebook Page when I choose to, see its connection status, and disconnect it, so that the product can act for my Page only with my authorization.

**Problem and outcome.** Publishing needs an authorized Page and safely stored credentials. After this task a Page can be connected in tester mode, credentials are encrypted and usable only by the publishing role, and status is truthful.

**Scope.** Owner-initiated connection and Page selection; encrypted credential storage with decryption limited to the publishing role; status connected, reconnect needed and disconnected; disconnect with consequences explained first; authorization generation advancing on disconnect; audit records.
**Excludes.** Any publishing (T35), production access (T38), other platforms.

**Acceptance criteria.**
1. An owner with a tester role connects a Page from staging and sees it as connected with its name.
2. Cancelling or failing the platform's consent leaves no partial connection.
3. Stored credentials are unreadable in the database and never appear in logs; the AI runtime cannot obtain them.
4. Disconnecting shows what will stop, then removes usable credentials and advances the authorization generation.
5. A credential the platform has revoked shows as reconnect needed after the next check, not as connected.
6. Connecting a Page creates no Post, approval or publication.

**Requirement references.** FR-33, FR-56; NFR-03; AC-54 (connection states); SYS §4.6 (flow 6), §4.7; ADR-018; D-30, D-54.
**Prerequisites.** P1-E01-T03; P1-E02-T09, T12, T14.
**Independence this week.** Its own module; no other Week 5 task uses a connection.
**Estimate and difficulty.** 20 h. V Med, U High (platform consent and token behaviour), I High, S High, T Med.
**Completion evidence.** Connection, revocation and disconnection demonstrated on staging with a tester Page.
**Risks and assumptions.** Works only for app-role users until Meta grants production access. Platform rules may change during the project.

### P1-E03-T20 — Versioned document records

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P1-E03 | 5 | D | 18 h | 15–23 h | High |

**User story.** As a business owner, I want each document the product writes for me kept with its versions, sources and origin, so that I can return to it and see how it changed.

**Problem and outcome.** Research and planning outputs need a durable home that is not a Post. After this task documents persist with immutable versions and can be read in a minimal Deliverables screen.

**Scope.** Document records with type, title, origin references, sources, assumptions and limitations; immutable versions; safe display of formatted text; a minimal list and reading view.
**Excludes.** Creation from Tasks (T29), editing, search, export and deletion (T52), Posts.

**Acceptance criteria.**
1. Saving a change to a document creates a new version; earlier versions stay readable and unchanged.
2. A document shows its sources, date and stated limitations.
3. Formatted text containing a script or unsafe link is displayed without executing it.
4. A document from another workspace cannot be opened by guessing its address.
5. The list and reading view work in both languages; long Arabic and English text is readable.

**Requirement references.** FR-41 (records, versions); NFR-01, NFR-04; AC-47 (versions); SYS §3.7, §4.5; OD-19.
**Prerequisites.** P1-E02-T09, T10, T14.
**Independence this week.** Its own module; documents are created by a test path until T29.
**Estimate and difficulty.** 18 h. V Med, U Low, I Low, S Med, T Low.
**Completion evidence.** A document with two versions shown, and the unsafe-content test.
**Risks and assumptions.** None material.

---

## Week 6 — P1-E03

### P1-E03-T21 — Owner file storage with validation

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P1-E03 | 6 | A | 18 h | 15–23 h | High |

**User story.** As a business owner, I want to upload an image safely and know immediately if it cannot be used, so that I never discover at publishing time that my media was rejected.

**Problem and outcome.** Posts need owner media and later features need uploads. After this task an owner can upload a file that is validated, stored under their workspace and reachable only through expiring access.

**Scope.** Upload with type and size validation; image validity check; workspace-scoped storage on the vendor chosen in T12; expiring access; removal; documented limits; the cross-workspace check extended to real stored files.
**Excludes.** Attaching media to Posts (T27), platform-specific format rules (T47), spreadsheet uploads (T72, T75).

**Acceptance criteria.**
1. A valid image uploads and can be viewed by its owner; an unsupported type or oversize file is refused with the limit stated.
2. A file with an image extension that is not a real image is refused.
3. Access links expire; an expired link or a link used from another workspace fails.
4. Removing a file makes it unreachable.
5. Upload, failure and removal states are shown in both languages.

**Requirement references.** FR-31 (owner media); NFR-03, NFR-04; SYS §4.5 (object storage); ADR-019; D-41; OD-18.
**Prerequisites.** P1-E02-T12, T14, T15.
**Independence this week.** Its own capability; nothing else in Week 6 uploads files.
**Estimate and difficulty.** 18 h. V Med, U Low, I Med, S Med, T Low.
**Completion evidence.** Demonstration of an accepted image, a disguised file refused and an expired link.
**Risks and assumptions.** Size limits are provisional until T47 checks platform rules.

### P1-E03-T22 — Durable Task lifecycle with two-slot admission

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P1-E03 | 6 | B | 24 h | 20–34 h | Low |

**User story.** As a business owner, I want work I ask for to keep going if I close the browser or a server restarts, and to be told truthfully when it is waiting, so that I can rely on what the product says about my work.

**Problem and outcome.** Substantive requests need durable Tasks with truthful states and a hard limit of two running per workspace. After this task a request from a conversation becomes a Task that runs one real bounded step, survives interruption and reports its true state.

**Scope.** Task records linked to a conversation; the approved states; atomic first-in-first-out admission at two running Tasks; cancellation; survival of worker interruption; one real bounded model step whose result returns to the conversation; usage and audit records; Task status shown in Chat.
**Excludes.** Planning and delegation (T25), clarifications (T40), budgets (T42), the Canvas (T33), document or Post results (T29).

**Acceptance criteria.**
1. A substantive request creates one Task in its conversation and is acknowledged within the provisional two-second target.
2. With two Tasks running in different conversations, a third shows as waiting, and starts when a slot is released, in request order.
3. Stopping a worker during a Task does not lose it; the Task finishes and its state was never shown as failed.
4. Cancelling stops further work and shows cancelled; a finished Task cannot be cancelled.
5. A step that cannot recover ends as failed with a reason the owner can read; it is never left as running.
6. Under simultaneous requests, no more than two Tasks are ever running in one workspace.

**Requirement references.** FR-17, FR-20 (run identity), FR-23, FR-53 (capacity); NFR-01, NFR-05, NFR-06; AC-41, AC-44; SYS §3.4, §4.6 (flow 1), §7.3; BTD-ADR-05, BTD-ADR-12; D-66.
**Prerequisites.** P1-E01-T06, T08; P1-E02-T14; P1-E03-T18.
**Independence this week.** The single step uses no retrieval (T24) and no files (T21).
**Estimate and difficulty.** 24 h. V High, U High (concurrency and interruption), I High, S Med, T High.
**Completion evidence.** Recorded run of three simultaneous requests with a worker stopped mid-Task.
**Risks and assumptions.** Admission races are hard to reproduce; T57 re-tests them under load.

### P1-E03-T23 — Evaluation case library, first 14 cases

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P1-E03 | 6 | C | 18 h | 15–24 h | High |

**User story.** As a reviewer of the product's quality, I want a set of authored business cases with fixed sources, so that outputs are judged against the same facts each time.

**Problem and outcome.** The PRD asks for about 24 cases. T07 wrote four. After this task ten more exist, giving 14, with contrast pairs and a held-out subset.

**Scope.** Ten authored cases across sparse and conflicting evidence, a direct request, a capacity limit, a no-website business, a many-inquiries-few-bookings business and a weak-repeat-demand business; at least two contrast pairs; fixed source snapshots; held-out marking; a coverage table by register and category; a reviewer list naming, for each of the four registers, the confirmed reviewers or stating that none is confirmed.
**Excludes.** The remaining cases and any automated runner (T71, T93). Running the cases (T56).

**Acceptance criteria.**
1. Each case has a business profile, the request, the material facts, fixed sources and what a defensible answer must and must not contain.
2. Each contrast pair differs in exactly one material fact and states how the answer should change.
3. At least four cases are marked held-out and stored where prompt authors are told not to use them.
4. The coverage table shows cases per register and category, and names what the missing ten cases must cover.
5. Every case is labelled synthetic.
6. The reviewer list exists; a register with no confirmed reviewer is reported to the plan owner at Gate G1 as an open gap.

**Requirement references.** PRD §9 (closing paragraph), §10.1; AC-18, AC-19, AC-21 (case basis); strategy/evaluation.md §3; D-60.
**Prerequisites.** P1-E01-T07.
**Independence this week.** Authored documents only.
**Estimate and difficulty.** 18 h. V Med, U Low, I Low, S Low, T Med.
**Completion evidence.** The 14 cases and the coverage table.
**Risks and assumptions.** Fourteen cases cannot support the per-register 80% gate with confidence; the gap is T71, T93.

### P1-E03-T24 — Retrieval of eligible context only

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P1-E03 | 6 | D | 20 h | 16–27 h | Medium |

**User story.** As a business owner, I want the Director to use what I have confirmed about my business and never something I rejected, removed or have not approved, so that its work reflects my current business.

**Problem and outcome.** The Director needs relevant context without ineligible items leaking back. After this task current eligible knowledge is indexed and can be retrieved by meaning and by relationship, with eligibility rechecked against the source of truth.

**Scope.** Indexing of eligible knowledge when it is added or changed; retrieval by similarity followed by one to two relationship steps; a recheck of eligibility before results are returned; each result labelled with its epistemic status and provenance; embedding model selected (D-39).
**Excludes.** Use by the Director (T25), retrieval of documents and past conversations (partly OV), the document-only comparison route (T71, T93), deletion clean-up (T65).

**Acceptance criteria.**
1. A query returns relevant active items from the asking workspace with their status labels.
2. Candidate, rejected, superseded and retired items are never returned, including in the minute after their state changed.
3. An item related to a retrieved item is included through its relationship, and not if the relationship is retired.
4. A query from one workspace never returns another's items; the T15 cross-workspace check is extended to real indexed data.
5. The index can be rebuilt from the source of truth and gives the same eligible results.

**Requirement references.** FR-08, FR-11, FR-42; NFR-01, NFR-03; SYS §4.4, §4.5 (derived index); ADR-004, ADR-007; D-22, D-39.
**Prerequisites.** P1-E01-T08; P1-E02-T15; P1-E03-T17.
**Independence this week.** Reads knowledge records from Week 5; T22's step does not call it.
**Estimate and difficulty.** 20 h. V Med, U Med, I High, S High, T Med.
**Completion evidence.** Test showing a rejected item absent from results immediately after rejection, and a rebuild.
**Risks and assumptions.** Arabic retrieval quality with the chosen embedding model is unmeasured until this task.
