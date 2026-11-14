# E. Task Descriptions — Phase 2 Build (Weeks 7–13, T25–T52 and additional tasks T65–T91)

> Part of the [BUSMORA 16-week implementation plan](./README.md). Epics: [D](./03-epics.md). Dependencies and weekly independence checks: [F](./07-dependency-map.md).
> These are proposed backlog items for review. Nothing here has been started.
> Each estimate includes research, decisions inside approved constraints, implementation, tests, peer review, integration with earlier work, expected fixes and the acceptance demonstration. Implementation choices belong to the assignee.
> Difficulty factors: **V**olume, **U**ncertainty, **I**ntegration, **S**ecurity/data risk, **T** verification difficulty — each Low, Med or High.
> Every task also meets the shared Definition of Done in the [README](./README.md#definition-of-done-for-every-task): bilingual and keyboard use, running on staging with its secrets, its own audit, usage and notification records, the cross-workspace check extended, and its ledger rows updated. Copy that list into each Jira item on import.

---

## Week 7 — P2-E04

### P2-E04-T25 — Director performs orchestrated work from Chat

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P2-E04 | 7 | A | 24 h | 20–36 h | Low |

**User story.** As a business owner, I want to ask the Marketing Director for a piece of marketing work in my own words and have it plan and carry out that work using what it knows about my business, so that I get a useful result without writing a detailed prompt or building a plan first.

**Problem and outcome.** A Task currently performs one fixed step. After this task the Director interprets the objective, loads eligible context, plans, uses drafting and analysis specialists as the request warrants, stays inside fixed ceilings and reports what it did.

**Scope.** Planning that varies with the request and context; delegation to drafting and marketing-analysis specialists; use of eligible Brain context and the conversation's history; enforced ceilings on iterations, time and concurrency that the model cannot change; a record of each piece of specialist work actually performed; a text result returned to the conversation.
**Excludes.** Research (T36), typed document and Post results (T29), clarification forms (T40), usage budgets and extensions (T42), diagnosis recipes (T48).

**Acceptance criteria.**
1. Two different requests for the same business produce different plans and different specialist work; a simple request uses no specialist.
2. The result reflects a confirmed Brain fact relevant to the request, and does not use a candidate or rejected item.
3. When a ceiling is reached, the Task ends with a partial result and a plain statement of what was not done; it never continues past the ceiling.
4. The recorded specialist work matches what ran: no entry exists for work that did not happen.
5. A specialist cannot call a tool outside its permitted set, and no specialist can start further specialists without limit.
6. Model output that does not match the expected structure is rejected and handled, not saved.

**Requirement references.** FR-15, FR-17, FR-20, FR-21, FR-42; NFR-01, NFR-04, NFR-06; AC-05, AC-06; SYS §4.6; BTD-ADR-09, BTD-ADR-13; D-44.
**Prerequisites.** P1-E03-T18, T22, T24.
**Independence this week.** Reads Brain records that exist from T17; it does not need T26's onboarding. Returns text, so it does not need T27's Posts.
**Estimate and difficulty.** 24 h. V High, U High (model behaviour, agent framework), I High, S Med, T High.
**Completion evidence.** Three contrasting requests run on staging with their recorded work shown, including one stopped by a ceiling.
**Risks and assumptions.** The most likely task to overrun. If it does, T29 and T36 slip by the same amount.

### P2-E04-T26 — Manual setup, confirmation and Brain activation

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P2-E04 | 7 | B | 20 h | 17–25 h | High |

**User story.** As a business owner without a useful website, I want to enter the few essential facts about my business in one form and confirm them, so that the Director starts with context I have approved and I am not asked strategy questions to get going.

**Problem and outcome.** A verified owner has an empty workspace. After this task the owner completes the essential form, confirms the exact version shown, the Brain is activated from it, and they are offered Chat or Create My Roadmap.

**Scope.** Onboarding entry with Manual Setup active and Social Media import shown disabled as Coming Soon — Phase 3; the essential form (name, description, one or more offerings, country and service area, contact or conversion route or "not ready yet"); pricing models fixed, starting from, quote required and unknown; saved editable draft with versions; confirmation of the displayed version; Brain activation with provenance in the same step; the Chat-or-Roadmap choice; sign-in routing by onboarding state.
**Excludes.** Website import (T28, T32). Goal, audience, competitor, budget or weekly-hours questions, which must not appear.

**Acceptance criteria.**
1. The form cannot be confirmed without the five essential groups; "not ready yet" is accepted as a conversion route state.
2. An offering can be saved with each pricing model; an amount requires a currency; a missing price is stored as unknown, never as zero.
3. A partly filled draft survives closing the browser.
4. Nothing from the draft is retrievable as Brain knowledge before confirmation; after confirmation the items are active, owner-confirmed and carry their source.
5. Confirming a version that is no longer the latest is refused and the latest is shown.
6. After confirmation the owner is offered Chat and Create My Roadmap; a later normal sign-in opens Chat, an owner with incomplete onboarding is sent to setup, and no last-visited page is restored.
7. The Social Media import option cannot be started and makes no claim of access.

**Requirement references.** FR-02, FR-04, FR-05, FR-06, FR-07, FR-10; AC-02, AC-16, AC-21, AC-33, AC-34; SYS §3.3, §4.6 (flow 5); ADR-021; D-62, D-63; DS §10.3.
**Prerequisites.** P1-E02-T09, T10, T14; P1-E03-T17, T18.
**Independence this week.** The entry screen does not offer Website import yet; T32 adds it. T28 has its own route.
**Estimate and difficulty.** 20 h. V Med, U Low, I Med, S Med, T Med.
**Completion evidence.** AC-02 and AC-33 demonstrated on staging, including a stale-version confirmation refused.
**Risks and assumptions.** Create My Roadmap opens the Roadmap area; Discovery arrives in T30.

### P2-E04-T27 — Manual Post drafts and Unscheduled list

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P2-E04 | 7 | C | 16 h | 13–21 h | High |

**User story.** As a business owner, I want to write a social post myself and keep it as a draft with or without a date, so that I can prepare content before I connect a Page or decide when to publish.

**Problem and outcome.** There is no home for Posts. After this task Scheduling holds Post drafts with versions, shown in a List and an Unscheduled view.

**Scope.** New Post with text or link and an optional validated owner image; optional date; optional target Page when one is already connected; the workspace's planning time zone, chosen explicitly by the owner the first time a date is entered and changeable afterwards; immutable Post versions on save; List and Unscheduled views; Post detail and editor; discarding a draft.
**Excludes.** Approval (T31), publishing (T35), Month and Week views and drag (T78, T81), Posts from Chat (T29).

**Acceptance criteria.**
1. A draft can be created and edited with no connected Page.
2. A draft with no date appears only in Unscheduled and is given no invented date.
3. Each save creates a new version; the previous version remains readable.
4. A draft with a proposed date is shown as proposed and unapproved.
5. An attached image that failed validation cannot be saved to the draft.
6. Saving a draft creates no approval and starts no release.
7. The time zone is stored as a named zone, is never inferred from language, and is shown beside every date.

**Requirement references.** FR-55, FR-57; NFR-01; AC-48, AC-49; SYS §3.4 (Post); ADR-023; D-67; DS §10.9.
**Prerequisites.** P1-E02-T10, T14; P1-E03-T19, T21.
**Independence this week.** Its own module and screen; no other Week 7 task creates Posts.
**Estimate and difficulty.** 16 h. V Med, U Low, I Low, S Low, T Low.
**Completion evidence.** AC-48 (draft part) and AC-49 demonstrated.
**Risks and assumptions.** None material.

### P2-E04-T28 — Website import crawl with truthful outcomes

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P2-E04 | 7 | D | 20 h | 17–28 h | Medium |

**User story.** As a business owner with a website, I want the product to read my site's relevant pages and tell me honestly how far it got, so that I do not retype what is already public and I am never left waiting on a failed import.

**Problem and outcome.** Onboarding needs observations from a website. After this task a submitted address is crawled within bounds, relevant pages are stored as observations with their source and date, and progress, completion, partial completion or failure is reported truthfully.

**Scope.** Address validation; permitted, bounded crawling that honours the site's stated access rules; preference for relevant business pages over login and low-value pages; stored page snapshots; real progress; complete, partial and failed outcomes with reasons; one crawl at a time per workspace; usage records.
**Excludes.** Extraction into form fields and the onboarding entry (T32). Any claim about page speed, visual quality or form submission.

**Acceptance criteria.**
1. A normal small business site yields stored snapshots of its relevant pages within the provisional limits, with source address and capture time.
2. An address that resolves to an internal or reserved network location, directly or through a redirect, is refused.
3. Pages the site disallows are not fetched.
4. An unreachable or blocked site ends as failed with a reason within the time limit; a site that stops responding part-way ends as partial with what was captured kept.
5. Progress shown reflects pages actually fetched; no percentage is invented.
6. A second import request while one is running in the same workspace is refused or queued, and said to be so.

**Requirement references.** FR-03; NFR-01, NFR-04, NFR-05; AC-01 (crawl part), AC-27 (internal fetch); SYS §4.6 (flow 5), §4.7 (SSRF), §7.3 (crawl retries); D-32.
**Prerequisites.** P1-E01-T06; P1-E02-T12, T14, T15.
**Independence this week.** Has its own route and stores observations only; it does not write to T26's draft.
**Estimate and difficulty.** 20 h. V Med, U Med (real sites vary), I Med, S High, T Med.
**Completion evidence.** Runs against three real sites (normal, sparse, unreachable) and the internal-address test.
**Risks and assumptions.** Sites that block automated access will produce partial or failed imports; this is expected behaviour, not a defect.

---

## Week 8 — P2-E04

### P2-E04-T29 — Task results become documents and Posts

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P2-E04 | 8 | A | 22 h | 18–30 h | Medium |

**User story.** As a business owner, I want work I request in Chat to land where I will use it — a document in Deliverables or a Post in Scheduling — so that one post is one post and I never hunt through a conversation for my output.

**Problem and outcome.** Tasks return text only. After this task a finished Task saves typed results: documents, Posts or both, each linked to its origin, with any knowledge the Director wants to remember held as a candidate.

**Scope.** Result assembly that creates documents and Posts through their own modules in one step with the Task's completion; origin references; Chat cards linking to the document or the Post; the owner's choice of content language and register for a request; visual briefs as documents; AI-proposed durable facts stored as candidates.
**Excludes.** Campaign packages and brand profile (T74, T76), proposal review (T46), research reports (T36).

**Acceptance criteria.**
1. A request for one Arabic post creates one Post draft in Scheduling and a Chat reference to it, with no document and no forced research or campaign.
2. A request for a plan or brief creates a document with its sources, assumptions and limitations, and no Post.
3. A request for both creates both, linked by shared origin references, with the Post text stored once.
4. If any part of saving fails, the Task does not show as completed and no partial result is left behind.
5. A request stating a register (for example Gulf-oriented Arabic for a named country) produces content in that register while the interface language stays unchanged.
6. A durable fact the Director proposes is stored as a candidate and is not used as current knowledge.

**Requirement references.** FR-10, FR-17, FR-29, FR-31, FR-32, FR-41, FR-44, FR-57; NFR-01; AC-05, AC-10 (single item), AC-22, AC-47; SYS §4.6 (flow 1); ADR-023.
**Prerequisites.** P1-E03-T20; P2-E04-T25, T27.
**Independence this week.** Creates Post drafts through T27's Week 7 behaviour; it does not need T31's approval.
**Estimate and difficulty.** 22 h. V Med, U Med, I High, S Med, T Med.
**Completion evidence.** AC-22 demonstrated, plus a mixed request and a forced save failure.
**Risks and assumptions.** Register quality is judged by reviewers in T56, not here.

### P2-E04-T30 — Roadmap Discovery draft saved and resumable

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P2-E04 | 8 | B | 18 h | 15–24 h | High |

**User story.** As a business owner, I want to start planning my marketing by choosing an objective and answering questions at my own pace, so that I can stop and come back without losing what I entered.

**Problem and outcome.** Create My Roadmap leads nowhere yet. After this task it opens a saved Discovery draft whose first significant question selects the objective, with every answer and its status kept.

**Scope.** Discovery draft per workspace; objective selection (first customer, more customers, better inquiry conversion, offering launch, existing-customer revenue, custom); a fixed starter set of questions; answer statuses unanswered, answered, skipped, unknown, not applicable and confirmed; versioned answers with provenance; resume; safe handling of two edits at once.
**Excludes.** Adaptive question selection (T34), question research (T79, T83), plan generation (T37).

**Acceptance criteria.**
1. Create My Roadmap opens the existing draft if one exists, otherwise a new one.
2. After closing the browser and returning, the objective, answers, statuses and current position are restored.
3. Unknown, Skip and Not Applicable are saved as distinct statuses and are never shown or stored as an answer.
4. If the same draft is edited in two tabs, the second save is told about the conflict and neither edit is silently lost.
5. Changing an answer keeps the earlier version.
6. No question asks for a standard weekly-hours figure.

**Requirement references.** FR-45, FR-51; AC-20 (no weekly-hours question), AC-36; SYS §3.6; ADR-021; BTD-ADR-22; D-64; DS §10.4.
**Prerequisites.** P1-E02-T10; P1-E03-T17; P2-E04-T26.
**Independence this week.** Uses a fixed question set; does not depend on Chat results or Posts.
**Estimate and difficulty.** 18 h. V Med, U Low, I Med, S Low, T Med.
**Completion evidence.** AC-36 demonstrated for the fixed question set, including the two-tab conflict.
**Risks and assumptions.** None material.

### P2-E04-T31 — Exact-version Post approval and invalidation

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P2-E04 | 8 | C | 20 h | 17–25 h | High |

**User story.** As a business owner, I want to see exactly what will be posted, where and when before I approve it, and to have my approval cancelled if any of that changes, so that nothing goes out that I did not agree to.

**Problem and outcome.** Drafts cannot be approved. After this task an owner submits a fully specified Post, reviews its exact content, destination and scheduled time, approves or rejects it, and any approval-critical change creates a new version that needs fresh approval.

**Scope.** Submission for approval with complete scheduling details (instant, planning zone, lateness allowance); the review preview usable on a phone; individual approve and reject; approval bound to the version, destination, scheduled time and current authorization generation; invalidation on change of content, media, destination, time, zone or allowance; rescheduling by form; a prompt to connect a Page only when needed; expiry of an unapproved Post when its proposed time passes, using the approved release workflow, which this task starts at submission (SYS §4.6 flow 4).
**Excludes.** Dispatch (T35), bulk approval (T82), drag rescheduling (T78, T81).

**Acceptance criteria.**
1. The preview shows the exact text, media, destination Page, date and time in the planning zone, and the lateness allowance.
2. A Post cannot be submitted for approval without a connected, authorized Page and complete scheduling details.
3. Approving records which version and which details were approved.
4. Editing any approval-critical detail of an approved Post creates a new version, clears the approval and shows the Post as needing approval.
5. Rescheduling requires the complete scheduling details and clears the approval.
6. Changing the workspace's default time zone does not move an approved Post's instant.
7. Approving a version that is no longer current is refused.

**Requirement references.** FR-34, FR-44 (time zones), FR-55; NFR-01, NFR-07; AC-13, AC-29, AC-48, AC-52 (form); SYS §3.4, §4.6 (flow 4); ADR-006; D-50.
**Prerequisites.** P1-E03-T19; P2-E04-T27.
**Independence this week.** Works on any draft regardless of origin; does not need T29.
**Estimate and difficulty.** 20 h. V Med, U Low, I Med, S High, T Med.
**Completion evidence.** AC-13 demonstrated for each approval-critical field.
**Risks and assumptions.** None material.

### P2-E04-T32 — Website findings prefill the editable draft

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P2-E04 | 8 | D | 20 h | 17–28 h | Medium |

**User story.** As a business owner with a website, I want what the product found on my site placed into the same setup form for me to correct and confirm, so that I review a draft instead of typing everything, and nothing is treated as true until I confirm it.

**Problem and outcome.** Crawl observations and the setup form are separate. After this task Import from Website is offered in onboarding, extracted proposals fill the draft with their sources, and the owner edits and confirms as in manual setup.

**Scope.** Import from Website in the onboarding entry; structured extraction of the essential fields and available brand cues from stored snapshots; merging proposals into the draft with per-field source and date; visible source gaps; immediate manual completion when the import is partial, sparse or failed.
**Excludes.** Any second review queue for initial intake. Confidence percentages.

**Acceptance criteria.**
1. After a successful import the draft shows proposed values for the fields the site supports, each with the page it came from.
2. A field the owner has already edited is not overwritten by a later extraction result.
3. Fields the site did not support are shown as gaps, not filled with guesses; a missing price is not zero.
4. A partial or failed import leaves any captured fields in place and offers manual completion at once.
5. Nothing extracted is retrievable as Brain knowledge until the owner confirms the displayed draft version.
6. Text on a page that reads as an instruction to the product has no effect on what is extracted or done.

**Requirement references.** FR-03, FR-04, FR-06, FR-30 (brand cues); NFR-02, NFR-04; AC-01, AC-27; SYS §3.3, §4.6 (flow 5); ADR-019, ADR-021; D-18.
**Prerequisites.** P1-E01-T08; P2-E04-T26, T28.
**Independence this week.** Builds on Week 7 outputs only.
**Estimate and difficulty.** 20 h. V Med, U Med (extraction quality), I Med, S Med, T Med.
**Completion evidence.** AC-01 demonstrated on a real site, with an owner-edited field preserved.
**Risks and assumptions.** Extraction quality on Arabic sites is unmeasured until this task.

---

## Week 9 — P2-E04

### P2-E04-T33 — Authentic conversation-local Canvas

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P2-E04 | 9 | A | 20 h | 17–27 h | Medium |

**User story.** As a business owner, I want to see what the Director is actually doing on my request, so that I can trust the result and understand a delay or failure.

**Problem and outcome.** Work is visible only as a status line. After this task the selected conversation's Task is shown as a live diagram and an accessible list built only from recorded events.

**Scope.** Live view of the latest relevant active Task in the selected conversation; nodes for agents, tools, services and workflow steps that actually ran; connections between them; the states Planned, Active, Awaiting Input, Held, Completed, Error and Cancelled; safe detail for a node; an accessible chronological list; catching up after a dropped connection; recoverable internal retries shown as Processing with no attempt counter (ADR-024).
**Excludes.** Choosing among several related Tasks and guards against late events from another conversation (T80). Replay of finished past runs (ADR-020).

**Acceptance criteria.**
1. Every node and connection shown corresponds to a recorded event; with recording paused in a test, nothing new appears.
2. The Canvas shows only the selected conversation's Task; selecting another conversation shows that conversation's Task.
3. No progress percentage, private reasoning or secret is shown.
4. The same information is available as a keyboard-operable list in time order.
5. After a dropped connection the view catches up without duplicating or losing steps.
6. A Task that failed shows Error with the recorded reason.
7. A step that fails once and then succeeds on retry is shown as Processing throughout, with no failure flash and no attempt count; the attempts remain in operator records.

**Requirement references.** FR-21, FR-22, FR-23; NFR-01, NFR-07; AC-07, AC-43, AC-44; SYS §6.4; ADR-020, ADR-024; BTD-ADR-20; D-17, D-68; DS §10.6.
**Prerequisites.** P1-E01-T06; P1-E03-T22; P2-E04-T25.
**Independence this week.** Renders events that Tasks already record; T36's research events use the same approved event contract and need no change here.
**Estimate and difficulty.** 20 h. V Med, U Med, I Med, S Low, T Med.
**Completion evidence.** A recorded Task shown live and as a list, with a forced disconnection.
**Risks and assumptions.** AC-42 remains partly unmet until T80.

### P2-E04-T34 — Adaptive Discovery questions

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P2-E04 | 9 | B | 16 h | 13–22 h | Medium |

**User story.** As a business owner, I want the planning questions to fit my objective and skip what the product already knows, so that I answer only what matters for my plan.

**Problem and outcome.** Discovery asks a fixed set. After this task questions follow from the chosen objective and current Brain context, and record the practical constraints a plan must respect.

**Scope.** Question selection driven by objective and eligible Brain facts; reuse of known facts with the owner able to correct them; questions about the conversion route and material constraints (assets, permissions, fulfilment capacity, relevant skills, business budget); an explanation for any question that cannot be skipped; optional success measure with its status and unit.
**Excludes.** Research on a question (T79, T83), reuse across a later second Discovery (T84).

**Acceptance criteria.**
1. Two objectives lead to visibly different question sets for the same business.
2. A fact already confirmed in the Brain is shown as known and is not asked again unless it is missing something material.
3. A question that cannot be skipped says why.
4. Constraints are recorded as stated; business budget is stored separately from any AI usage allowance.
5. A baseline or target can be recorded as observed, owner-reported, estimated, unknown or not applicable, with period and unit.
6. Returning to the draft restores the adapted question order and statuses.

**Requirement references.** FR-18, FR-45, FR-51; AC-20, AC-36, AC-38 (reuse of Brain facts); SYS §3.6; OD-26 (question policy); DS §10.4.
**Prerequisites.** P1-E01-T08; P1-E03-T24; P2-E04-T30.
**Independence this week.** Works on the Discovery draft only.
**Estimate and difficulty.** 16 h. V Low, U Med (question policy), I Med, S Low, T Med.
**Completion evidence.** Two objectives compared for one business; AC-36 repeated with adaptive branching.
**Risks and assumptions.** The question policy is an open item (OD-26); the first version is recorded as provisional.

### P2-E04-T35 — Approved Post published for real at its scheduled time

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P2-E04 | 9 | C | 22 h | 18–32 h | Low |

**User story.** As a business owner, I want a post I approved to appear on my Facebook Page at the time I chose, with proof that it did, so that I can rely on the schedule without watching it.

**Problem and outcome.** Approved Posts go nowhere. After this task an unchanged approved text or link Post is dispatched at its scheduled time through the approved deterministic path, and marked published only on platform evidence.

**Scope.** Release at the scheduled time; an atomic claim that rechecks version, destination, authorization, cancellation and window immediately before sending; an attempt recorded before the platform is called; dispatch of text and link posts with no AI involvement after approval; platform post identity and read-back stored; states submitted and published; the published Post shown at its actual publication time; cancellation before dispatch.
**Excludes.** Image posts (T47), uncertain outcomes (T39), overdue and hold handling (T43).

**Acceptance criteria.**
1. An approved text post appears on a tester-mode Page within its lateness allowance, and the Post shows published with the platform's identity and link.
2. A Post is shown as published only after the platform confirms it; before that it shows submitted.
3. A Post changed after approval is not dispatched.
4. A Post whose connection was disconnected after approval is not dispatched.
5. A Post cancelled before its time is not dispatched and shows cancelled.
6. Two workers releasing the same Post result in one attempt.
7. The published Post is listed at its actual publication time.

**Requirement references.** FR-33, FR-35, FR-36; NFR-01, NFR-05; AC-12 (text/link, individual approval), AC-53 (actual time); SYS §4.6 (flow 4), §7.3; ADR-005, ADR-006; BTD-ADR-14; D-49.
**Prerequisites.** P1-E01-T06; P1-E02-T12; P1-E03-T19; P2-E04-T31.
**Independence this week.** Its own path from approval to platform; no other Week 9 task touches publishing.
**Estimate and difficulty.** 22 h. V Med, U High (platform behaviour, timing), I High, S High, T High.
**Completion evidence.** A real scheduled publication on a tester-mode Page with stored evidence, and the changed-after-approval case.
**Risks and assumptions.** Evidence is at tester-mode access level only and is labelled so. Timeouts are not handled until T39; until then they are treated as failures needing manual review.

### P2-E04-T36 — Research report with inspectable citations

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P2-E04 | 9 | D | 22 h | 18–34 h | Low |

**User story.** As a business owner, I want the Director to research a question about my market or competitors across public sources and give me a report I can check, so that my decisions rest on evidence I can inspect.

**Problem and outcome.** The Director cannot look anything up. After this task a research request is planned, relevant public sources are searched and retrieved through the provider chosen in T04, and the findings are saved as a document whose claims link to their sources.

**Scope.** Research planning from the question; dynamic choice among available public source types; retrieval and evidence extraction; association of each material finding with its source and access date; a report connecting question, evidence, limitations, implications, options and a recommended next task; customer wording kept in its original language with source and date; honest partial results when sources fail.
**Excludes.** Usage budgets and extensions (T42); contradiction and duplication analysis, stopping rules and hostile-source handling (T44); private or signed-in sources.

**Acceptance criteria.**
1. A competitor question for a launch-cohort business returns a document citing at least two distinct source types where they are accessible.
2. Every material finding links to a source a reader can open, with the date accessed.
3. No citation is invented: each cited address was actually retrieved in that Task.
4. Customer wording is quoted as found, with any translation labelled separately.
5. When a source cannot be reached the report says so and names what remains unanswered.
6. The report ends with a recommended next task, not only a list of results.
7. Research runs inside the Task's fixed ceilings from T25.

**Requirement references.** FR-21, FR-24, FR-25, FR-26, FR-28; NFR-02, NFR-04; AC-08; SYS §4.8; D-45.
**Prerequisites.** P1-E01-T04; P2-E04-T25, T28, T29.
**Independence this week.** Saves documents through T29's Week 8 behaviour. T33 displays its events without change.
**Estimate and difficulty.** 22 h. V High, U High (source access, model behaviour), I High, S Med, T High.
**Completion evidence.** Two research Tasks on evaluation cases with every citation opened and checked.
**Risks and assumptions.** Arabic and regional source coverage may be thin (measured in T04). Thin coverage is reported, not hidden.

### P2-E04-T65 — Removed data is cleaned from every derived copy

> **Additional task above the 64-task baseline**, added by the plan owner's decision on DR-2. Carried by B as a second task this week (36 h in the week).

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P2-E04 | 9 | B (second task) | 20 h | 16–30 h | Low |

**User story.** As a business owner, I want something I remove to be gone from every place the product copied it to, so that removed information about my business cannot resurface in later work.

**Problem and outcome.** Removal stops an item taking part in retrieval, but copies remain in derived stores. After this task a removal reaches every derived copy named in the data lifecycle policy, within the period the policy sets, and the owner sees a truthful status.

**Scope.** Removal of a knowledge item, a document, an uploaded file and a website import, using the removal behaviour that exists at the start of the week; propagation to every derived copy the T16 policy lists (search index entries, stored files and their derivatives, cached copies, saved crawl observations); a record of each removal with its completion state; a truthful in-progress, complete or failed status with automatic retry of a failed clean-up; an audit record for each removal.
**Excludes.** Backups and restores (T69). Export (T67). The Brain List and Deliverables screens that later offer removal to the owner (T50, T52); they call this behaviour.

**Acceptance criteria.**
1. After a removal completes, a search of each derived store named in the policy finds no copy of the removed item.
2. A removed item is never returned to a Task that starts after the removal, including through a cached or indexed copy.
3. A clean-up that fails part-way is retried and shows failed, not complete, until every store is clean.
4. The removal record shows what was removed, when, and when each store was cleaned, without keeping the removed content itself.
5. Removing an item in one workspace changes nothing in another workspace.
6. Material that the policy says must be kept (for example audit and publication records) is kept and the owner is told so.

**Requirement references.** FR-13, FR-42; NFR-03, NFR-09; AC-03 (deletion part), AC-15 (remove); PRD §13.3 (data controls); SYS §4.5; the T16 lifecycle policy.
**Prerequisites.** P1-E02-T14, T15, T16; P1-E03-T17, T20, T21, T24; P2-E04-T28, T29.
**Independence this week.** Uses records and stores that existed at the end of Week 8. It does not need the Canvas (T33), Discovery (T34), publishing (T35) or research (T36).
**Estimate and difficulty.** 20 h. V Med, U High (every store), I High, S High, T High.
**Completion evidence.** One removal of each kind on staging, followed by a recorded search of every derived store.
**Risks and assumptions.** The number of derived stores is set by the T16 policy; if the policy lists more than this estimate assumes, the task overruns. It runs on top of B's primary task, so B needs about 36 hours this week.

---

## Week 10 — P2-E05

### P2-E05-T37 — Complete proposed Roadmap generated from Discovery

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P2-E05 | 10 | A | 22 h | 18–32 h | Low |

**User story.** As a business owner, I want my answers turned into a complete step-by-step plan that shows why each step is there and how sure the Director is, so that I can judge the plan before I commit to it.

**Problem and outcome.** Discovery collects answers but produces nothing. After this task generating from a Discovery draft yields a complete proposed plan the owner can read in full.

**Scope.** Generation as an authorized Task from the Discovery draft, current Brain and available evidence; a proposed plan with every step carrying title, objective, rationale, evidence, uncertainty, responsible party, execution mode, required inputs, real dependencies, expected output, completion condition and estimated effort; a proposal screen showing all steps vertically; the draft locked while generating; a preliminary plan when answers are unknown.
**Excludes.** Approval and activation (T41), step execution (T45), revisions (T88, T89).

**Acceptance criteria.**
1. Generation produces a plan in which every step has all the listed fields; a plan missing a field is rejected and not saved as proposed.
2. Steps designed for the owner to carry out are labelled as owner execution, and steps needing a capability the product lacks are labelled blocked or later phase, never as supported execution.
3. Dependencies listed are real prerequisites; recommended order alone is not recorded as a dependency.
4. Where a Discovery answer was unknown or skipped, the plan states the resulting uncertainty and does not invent a value.
5. The plan contains no Now/Next/Later grouping, horizon property or periodic review date.
6. The proposed plan grants no publishing or spending permission, and says so.
7. Generation that fails leaves the Discovery draft intact and editable.

**Requirement references.** FR-14, FR-16, FR-45, FR-46, FR-49, FR-51, FR-54; NFR-01; AC-04, AC-18, AC-20, AC-21, AC-57; SYS §3.4, §3.6; ADR-022; D-65; DS §10.5.
**Prerequisites.** P2-E04-T25, T34.
**Independence this week.** If information is missing it produces a preliminary plan; it does not need T40's clarification forms.
**Estimate and difficulty.** 22 h. V Med, U High (plan quality), I Med, S Low, T High.
**Completion evidence.** Plans generated for two contrasting evaluation cases, checked field by field.
**Risks and assumptions.** Plan usefulness is judged by reviewers in T56. Generic content-first plans are a named risk signal in PRD §11.1.

### P2-E05-T38 — Production access submission to Meta

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P2-E05 | 10 | B | 18 h | 14–30 h | Low |

**User story.** As the team, we want our request for production access filed with Meta as soon as the working flow can be shown, so that the external review has the most time possible before the release decision.

**Problem and outcome.** Only app-role users can connect a Page. After this task everything the team controls for production access is prepared and submitted, or the exact missing prerequisite is recorded.

**Scope.** The app's review details, registered with the product domain and the production addresses set up in T12 so they do not change after review; links to the public policy pages from T16; a demonstration of the real connect, approve and publish flow on staging; reviewer access instructions; the status of the business verification filed in T03; submission; a log of the platform's responses.
**Excludes.** The approval itself. Any change to committed formats. Validation with an ordinary owner (T63).

**Acceptance criteria.**
1. Each prerequisite identified in T03 is marked done or blocked, with evidence.
2. The demonstration shows the real flow on staging, not a mock.
3. The submission is filed and its reference recorded; or, if a prerequisite outside the team's control is missing, the record names it, its owner and the date it was escalated.
4. The public pages describe data handling consistently with the T16 policy.
5. A named person is responsible for checking and answering the platform's responses each week until a decision.

**Requirement references.** FR-33; AC-30; PRD §3.1, §11.1 (Facebook), §13.2; SYS ADR-018, R-02, OD-23.
**Prerequisites.** P1-E01-T03; P1-E02-T12, T16; P1-E03-T19; P2-E04-T35.
**Independence this week.** Shows the flow as it worked at the end of Week 9; needs nothing from T39.
**Estimate and difficulty.** 18 h. V Med, U High (external requirements, possible rejection), I Low, S Med, T Low.
**Completion evidence.** The submission reference, or the blocked record.
**Risks and assumptions.** **Outcome depends on Meta.** A rejection needs rework that is not in the baseline. No legal entity means no submission and a blocked AC-30.

### P2-E05-T39 — Uncertain publication reconciled, never resent

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P2-E05 | 10 | C | 20 h | 17–28 h | Medium |

**User story.** As a business owner, I want the product to find out what really happened when a publish attempt gets no clear answer, so that my post is never published twice and never reported as published when it was not.

**Problem and outcome.** A timeout after dispatch has no safe handling. After this task an attempt with unknown acceptance is marked uncertain, checked against the platform, and either confirmed, failed or held for the owner.

**Scope.** The uncertain state; reconciliation using platform evidence; holding when the outcome cannot be established; retries only for failures known to be safe to retry, never changing approved details and never beyond the window; owner-facing explanation and actions for an uncertain or failed Post.
**Excludes.** Overdue, disconnection and cancellation races (T43).

**Acceptance criteria.**
1. When the platform accepts a post but the response is lost, the Post becomes uncertain, then published after reconciliation, and exactly one post exists on the Page.
2. When the platform did not accept it, reconciliation leads to a failure the owner can act on, not to an automatic resend outside the safe cases.
3. When the outcome cannot be determined, the Post is held for the owner with an explanation.
4. A retry never alters the approved text, media, destination or time and never occurs after the lateness window.
5. Every attempt and its outcome is kept in the Post's history.

**Requirement references.** FR-36; NFR-01, NFR-05; AC-12 (no duplicate), AC-28 (timeout); SYS §7.3, R-09.
**Prerequisites.** P2-E04-T35.
**Independence this week.** Extends the publish path that existed at the end of Week 9.
**Estimate and difficulty.** 20 h. V Med, U Med, I Med, S High, T High (failures must be induced).
**Completion evidence.** An induced lost response on a tester-mode Page ending in exactly one post.
**Risks and assumptions.** Depends on the platform offering usable read-back; where it does not, the Post is held.

### P2-E05-T40 — Choice-first clarification pauses and resumes work

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P2-E05 | 10 | D | 20 h | 17–27 h | Medium |

**User story.** As a business owner, I want the Director to ask me only what it really needs, as a few meaningful choices, and then carry on, so that I am not interrogated and my work is not blocked by a question I cannot answer.

**Problem and outcome.** A Task cannot ask the owner anything. After this task a Task pauses with a structured question, the owner answers in Chat, and the Task resumes.

**Scope.** Clarification raised only for a material gap; three or four meaningful options drawn from current Brain facts plus Other/Custom; Unknown, Skip and Not Applicable where legitimate; the exact pending form in Chat and reachable from the Canvas; the Task releasing its running slot while waiting and reacquiring one in order when answered; expiry after seven days with cancellation; durable facts in answers stored as candidates.
**Excludes.** "Research this" on a field (T79, T83). Notifications (T51).

**Acceptance criteria.**
1. A request with a material gap produces one form whose options reflect the business's confirmed facts; no option is fabricated.
2. A request with no material gap produces no question.
3. While a Task waits, it does not count toward the two running Tasks, and a waiting Task starts in turn when answered.
4. Answering Unknown or Skip lets the Task continue with a stated limitation.
5. "Answer in Chat" from the Canvas opens that exact form.
6. An unanswered form expires after seven days and the Task shows cancelled with the reason.

**Requirement references.** FR-18, FR-22, FR-23, FR-53; NFR-01; AC-07 (form link), AC-35, AC-41; SYS §4.6 (flow 2); FTD-ADR-10; D-15.
**Prerequisites.** P1-E02-T10; P1-E03-T22; P2-E04-T25, T33.
**Independence this week.** T37 does not use it.
**Estimate and difficulty.** 20 h. V Med, U Med, I High, S Low, T Med.
**Completion evidence.** AC-35 demonstrated, with a third Task starting when a running Task pauses.
**Risks and assumptions.** Too many low-value questions is a named risk signal (PRD §11.1); question frequency is measured in T56.

### P2-E05-T66 — A change to business context flags the work that used it

> **Additional task above the 64-task baseline**, added by the plan owner's decision on DR-2. Carried by D as a second task this week (40 h in the week).

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P2-E05 | 10 | D (second task) | 20 h | 16–28 h | Medium |

**User story.** As a business owner, I want to be told which documents and posts were written from information I have since changed, so that I do not reuse or publish something that states an old price or offer.

**Problem and outcome.** Work does not record which version of the business context it used, so nothing can tell that it is out of date. After this task every piece of Director work records the context it used, and a change to a confirmed fact marks the affected work for the owner's review.

**Scope.** Recording, on each Task result, document version and Post version, which confirmed context items and versions were used, including for work created before this task where the information can be recovered; detecting a change, replacement or removal of a confirmed item; marking affected documents and unpublished Posts as possibly outdated, naming the changed item; an owner review that keeps the work as it is, or sends it back to be edited; excluding a replaced value from new work; showing when a time-limited fact (such as a dated offer) has passed its date.
**Excludes.** Holding a scheduled Post at dispatch (T68). Changes arriving through Pending Brain Updates and the Brain List, which are built later (T46, T50) and use this behaviour under the Definition of Done.

**Acceptance criteria.**
1. A document and a Post created by the Director each show which confirmed context items they used.
2. Changing a price that a document used marks that document as possibly outdated and names the price; a document that did not use it is not marked.
3. The owner can clear the mark by confirming the work is still right, and the decision is recorded.
4. Work started after a change uses the new value and never the replaced one.
5. A fact with an end date that has passed is shown as expired wherever it is displayed and is not used as current.
6. Work created before this task is either linked to the context it used or shown as "context not recorded"; it is never shown as current by default.

**Requirement references.** FR-10, FR-42, FR-48; NFR-01; AC-23 (detection part), AC-47 (staleness); SYS §3.4.
**Prerequisites.** P1-E03-T17, T24; P2-E04-T25, T26, T29, T31.
**Independence this week.** Changes are made through the confirmation and edit behaviour in place since T26. It does not need the proposed Roadmap (T37), the submission (T38), reconciliation (T39) or clarification forms (T40).
**Estimate and difficulty.** 20 h. V Med, U Med, I High (touches orchestration, documents and Posts), S Med, T Med.
**Completion evidence.** A price change on staging followed by the marked and unmarked work shown side by side.
**Risks and assumptions.** It changes work built in T25 and T29; that integration is inside the estimate. It runs on top of D's primary task, so D needs about 40 hours this week.

---

## Week 11 — P2-E05

### P2-E05-T41 — Roadmap approval, single active plan and step view

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P2-E05 | 11 | A | 18 h | 15–23 h | High |

**User story.** As a business owner, I want to approve the exact plan I reviewed and then see it as my one active Roadmap, so that I always know which plan is in force.

**Problem and outcome.** A proposed plan cannot be adopted. After this task the owner approves or rejects the displayed proposal, approval activates it as the single active Roadmap, and all its steps are visible with their status and dependencies.

**Scope.** Approve and reject bound to the displayed proposal version; activation that replaces any previous active plan and archives it in one step; saved drafts and archived plans remaining readable; the Roadmap view listing all steps vertically with status, real dependencies and blocked steps explained; step detail.
**Excludes.** Running steps (T45), owner completion and Skip (T49), revision diffs (T88, T89).

**Acceptance criteria.**
1. Approving the displayed proposal makes it the active Roadmap; rejecting it leaves any existing active plan unchanged.
2. Approving a proposal that is no longer the latest is refused.
3. Two approvals issued at the same moment never produce two active plans.
4. The previously active plan becomes archived and remains readable with its steps.
5. Every step is visible; a step blocked by an unmet prerequisite says which one.
6. Activation is recorded in the audit trail and grants no publishing or spending permission.

**Requirement references.** FR-16, FR-46, FR-54; NFR-01; AC-04, AC-46, AC-57; SYS §3.4; ADR-022; BTD-ADR-22; DS §10.5.
**Prerequisites.** P2-E05-T37.
**Independence this week.** Roadmap only.
**Estimate and difficulty.** 18 h. V Med, U Low, I Med, S Med, T Med.
**Completion evidence.** AC-46 demonstrated with simultaneous approvals.
**Risks and assumptions.** None material.

### P2-E05-T42 — Research budget with owner-approved extensions

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P2-E05 | 11 | B | 22 h | 18–32 h | Low |

**User story.** As a business owner, I want each research task to work within a limit I can see and to ask me before using more, so that the product never spends my allowance without my agreement.

**Problem and outcome.** Research is limited only by fixed ceilings. After this task each research Task has an authorized allowance covering time, model, tool and paid-source use, accounted for across all its work, with any increase requiring the owner's explicit approval.

**Scope.** An initial allowance per research Task; reservation before calls and settlement against actual use, covering child work, concurrent calls, failed calls and retries; a pause at the limit with a request stating the extra resources, the likely time or cost effect and the expected benefit; approve and decline; a bounded partial result on decline; the owner's view of allowance, consumption and requests.
**Excludes.** Customer charging or billing (Phase 4+). Business marketing budget, which is a separate fact.

**Acceptance criteria.**
1. Total use by a Task, including child and retried work, never exceeds its allowance, shown in a test that runs calls concurrently.
2. Reaching the allowance pauses the Task and shows a request with the three required explanations.
3. No further costly call is made until the owner approves.
4. Declining ends the Task with the strongest supported partial result and what remains unanswered.
5. After a decline the Director cannot continue the same work by switching tools or starting an equivalent Task.
6. The owner can see the allowance, what was used and any extension, and the decision is in the audit trail.
7. Approving more usage is described as internal resource use, not as a charge.

**Requirement references.** FR-20, FR-27; NFR-06; AC-09; SYS §7.6; D-55.
**Prerequisites.** P1-E02-T14; P2-E04-T36; P2-E05-T40.
**Independence this week.** Budget exhaustion is this task's concern; evidence-based stopping belongs to T44.
**Estimate and difficulty.** 22 h. V Med, U High (concurrent accounting), I High, S High, T High.
**Completion evidence.** AC-09 demonstrated with concurrent child work and a declined extension.
**Risks and assumptions.** A provider whose use cannot be reserved in advance must be excluded from autonomous use (FR-27).

### P2-E05-T43 — Overdue, held, cancelled and disconnected publishing

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P2-E05 | 11 | C | 22 h | 18–30 h | Medium |

**User story.** As a business owner, I want a post that missed its time, lost its authorization or was cancelled at the last second to stop and wait for me, so that nothing is published late or without my current consent.

**Problem and outcome.** Publishing handles the normal path and uncertainty only. After this task late, unauthorized and cancelled sends are held or stopped truthfully.

**Scope.** Holding an approved Post that passes its lateness allowance, requiring a new time and approval; holding on lost or revoked authorization; the cancel-versus-dispatch race with the true result shown; disconnect blocking future sends; reconnect releasing nothing without fresh checks and, where required, approval; owner actions on a held Post; a protected record of disconnections and of Posts held as uncertain or overdue, kept apart from the main data so that a later restore can reapply them (SYS §4.5).
**Excludes.** Holds caused by a change to business context (T68). Bulk actions (T82).

**Acceptance criteria.**
1. An approved Post not dispatched within its allowance becomes held and is not sent late.
2. Revoking the Page's authorization before dispatch results in a held Post and a reconnect-needed status, with no send.
3. Cancelling at the moment of dispatch ends either as cancelled with no post, or as published with a message that the platform had already accepted it and how to remove it manually.
4. After disconnecting and reconnecting, a previously overdue or uncertain Post is still held.
5. A pending Post whose proposed time passes shows expired and can be given a new time as a new version.
6. Resolving a hold with unchanged details keeps the approval; changing details requires new approval.
7. Each disconnection and each hold is written to the protected record, which survives a restore of the main data to an earlier point.

**Requirement references.** FR-35, FR-36, FR-56; NFR-01, NFR-05; AC-28, AC-54; SYS §3.4 (Post), §4.6 (flow 4), §7.3.
**Prerequisites.** P1-E03-T19; P2-E04-T35; P2-E05-T39.
**Independence this week.** Publishing only.
**Estimate and difficulty.** 22 h. V Med, U Med, I Med, S High, T High (races must be induced).
**Completion evidence.** AC-28 and AC-54 demonstrated on a tester-mode Page.
**Risks and assumptions.** None material.

### P2-E05-T44 — Research evidence quality, stopping and untrusted sources

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P2-E05 | 11 | D | 18 h | 15–25 h | Medium |

**User story.** As a business owner, I want research that tells me when sources disagree, repeat each other or are out of date, and that cannot be hijacked by a web page, so that I am not misled by weak or hostile evidence.

**Problem and outcome.** Research cites sources but does not judge them. After this task reports check that each citation supports its claim, expose contradictions and gaps, stop for stated reasons, and treat retrieved content strictly as data.

**Scope.** A check that a cited passage supports the claim attached to it; separation of observations, owner claims, interpretations and hypotheses; labelling of stale and inaccessible sources; duplicated material distinguished from independent corroboration; visible contradictions; regional coverage gaps; stopping when evidence is sufficient, retrieval is repetitive or useful access is exhausted, with the reason stated; boundaries that prevent retrieved text from triggering tools, changing knowledge, raising budgets, disclosing private data or directing fetches to internal locations.
**Excludes.** Budget accounting (T42).

**Acceptance criteria.**
1. On a fixed source set with a planted unsupported claim, the report omits or qualifies that claim.
2. Two sources copying the same text are reported as one piece of evidence.
3. A planted contradiction between two sources is shown to the owner, not resolved silently.
4. A source containing instructions (for example to reveal data or fetch an internal address) has no effect, and the attempt is recorded for operators.
5. Private business facts are not placed into public search queries because a source asked for them.
6. The report states why research stopped.
7. A claim with no source appears only as an explicitly labelled hypothesis.

**Requirement references.** FR-24, FR-25, FR-26; NFR-02, NFR-04; AC-08, AC-27; SYS §4.7 (prompt injection, SSRF).
**Prerequisites.** P2-E04-T36.
**Independence this week.** Evidence handling only; shares no behaviour with T42.
**Estimate and difficulty.** 18 h. V Med, U Med, I Med, S High, T High.
**Completion evidence.** AC-08 and AC-27 run on the fixed source snapshots from the case library.
**Risks and assumptions.** Passing a finite hostile-source set does not prove safety against all inputs; the limit is stated in the ledger.

### P2-E05-T67 — Owner exports the Brain and their history

> **Additional task above the 64-task baseline**, added by the plan owner's decision on DR-2. Carried by A as a second task this week (38 h in the week).

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P2-E05 | 11 | A (second task) | 20 h | 16–27 h | Medium |

**User story.** As a business owner, I want to take a copy of what the product knows about my business and of the work it did for me, so that my information is not locked inside the product.

**Problem and outcome.** The owner can read their data only on screen. After this task the owner can request an export and receive files they can open outside the product.

**Scope.** An export request covering Brain items with their status, source and history, conversations, documents with their versions, and Posts with their approval and publication records; preparation as durable background work that survives a restart; a notification when it is ready; a download that only the owner can open and that expires; content written so that a spreadsheet program cannot be made to run a formula from it; an audit record of each export.
**Excludes.** Export of a single document from the library (T52). CSV and original-media export of Deliverables (T86). Exporting credentials or another workspace's data, which must never happen.

**Acceptance criteria.**
1. An export contains every Brain item, conversation, document and Post of the workspace that the owner can see in the product, and nothing from another workspace.
2. Each Brain item in the export carries its status, source and change history.
3. Removed items are absent from an export requested after the removal completed.
4. The export opens outside the product in ordinary tools, in Arabic and English, with Arabic text intact.
5. The download link stops working after its stated period and never works for another account.
6. A value beginning with a formula character is exported as text.
7. No access token or secret appears anywhere in the export.

**Requirement references.** FR-13; NFR-03, NFR-04, NFR-09; AC-03 (export part), AC-15 (export); PRD §13.3 (data controls).
**Prerequisites.** P1-E02-T14, T15; P1-E03-T17, T18, T20, T21, T22; P2-E04-T27, T29, T35.
**Independence this week.** Reads records as they stood at the end of Week 10. It does not need Roadmap approval (T41), the research budget (T42), held states (T43) or evidence handling (T44).
**Estimate and difficulty.** 20 h. V Med, U Low, I Med, S High, T Med.
**Completion evidence.** An export of a staging workspace opened outside the product and compared with the screens.
**Risks and assumptions.** The export formats are those the T16 policy names. An active Roadmap is not in the export until T41 exists; adding it belongs to T41's owner under the Definition of Done only if the policy requires it. It runs on top of A's primary task, so A needs about 38 hours this week.

---

## Week 12 — P2-E06

### P2-E06-T45 — Run a Roadmap step with the Director

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P2-E06 | 12 | A | 20 h | 17–27 h | Medium |

**User story.** As a business owner, I want to start a step of my Roadmap with one action and follow the work in its own conversation, so that the plan turns into finished work without my re-explaining it.

**Problem and outcome.** Steps are visible but cannot be executed. After this task Run with Director launches exactly one linked Task for an eligible step and opens its conversation and Canvas.

**Scope.** Eligibility check on prerequisites and authorization; launch creating one linked Task, conversation and output references; truthful waiting when two Tasks are already running; reopening a step showing its existing work; step work status following the Task; for owner-execution steps, instructions, any prepared artifacts and a checklist with the responsible party stated.
**Excludes.** Owner completion and Skip (T49), revisions (T88, T89).

**Acceptance criteria.**
1. Run with Director on an eligible step creates one Task even under a double click or a repeated request.
2. Reopening a step that already has work shows that work and never starts it again.
3. A step with an unmet real prerequisite cannot be run and says which prerequisite is missing.
4. With two Tasks running, the launched step shows waiting and starts in turn.
5. The step's documents and Posts are linked from the step when the Task completes.
6. A step meant for the owner to carry out provides instructions and a checklist, names the owner as responsible, and is never shown as executed by the product.

**Requirement references.** FR-16, FR-46, FR-49, FR-53; NFR-01; AC-24, AC-39, AC-41; SYS §3.6; ADR-022.
**Prerequisites.** P1-E03-T22; P2-E04-T25; P2-E05-T41.
**Independence this week.** Uses orchestration as it stood at the end of Week 11; T48's diagnosis work is not required.
**Estimate and difficulty.** 20 h. V Med, U Med, I High, S Med, T Med.
**Completion evidence.** AC-39 demonstrated with a double click, and one owner-execution step.
**Risks and assumptions.** None material.

### P2-E06-T46 — Pending Brain Updates review

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P2-E06 | 12 | B | 20 h | 17–26 h | Medium |

**User story.** As a business owner, I want to approve, correct or reject each thing the Director wants to remember about my business, so that its memory contains only what I accept.

**Problem and outcome.** AI-proposed facts accumulate as candidates with no way to decide them. After this task the owner reviews each proposal and decides it individually.

**Scope.** A Pending Brain Updates view with each proposal's content, source and reason; approve, edit-and-approve and reject, one at a time; conflicts with current knowledge shown instead of overwriting it; approved items becoming current with history; rejected items excluded from retrieval.
**Excludes.** A single approve-everything action that would hide individual decisions. Working-preference rules (T90).

**Acceptance criteria.**
1. Each proposal shows what is proposed, where it came from and whether it is an observation, interpretation or hypothesis.
2. Approve makes the item current; edit-and-approve saves the owner's wording as current; reject keeps it out of current knowledge.
3. A rejected proposal is not returned by retrieval and is not proposed again from the same source.
4. A proposal that contradicts a confirmed item is shown as a conflict for the owner to resolve; the confirmed item is not changed automatically.
5. Each decision is in the audit trail.
6. Approval is described as acceptance for future use, not as verification of truth.

**Requirement references.** FR-09, FR-10, FR-42; NFR-01; AC-03 (approve, edit, reject); SYS §3.3; D-24; DS §9.5, §10.7.
**Prerequisites.** P1-E03-T17, T24; P2-E04-T29.
**Independence this week.** Works on candidates that Tasks have produced since Week 8.
**Estimate and difficulty.** 20 h. V Med, U Low, I Med, S Med, T Med.
**Completion evidence.** A proposal approved, one edited and one rejected, with retrieval checked after each.
**Risks and assumptions.** A growing queue of pending updates is a named risk signal (PRD §11.1).

### P2-E06-T47 — Single-image posts with checks before approval

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P2-E06 | 12 | C | 18 h | 15–26 h | Medium |

**User story.** As a business owner, I want to publish a post with one of my own images and be told before I approve if the image or format will not be accepted, so that a scheduled post does not fail at publishing time.

**Problem and outcome.** Only text and link posts publish. After this task a Post with a single owner-supplied image and caption publishes with evidence, and problems the product can detect are shown before approval.

**Scope.** Publishing of one owner image with caption; checks of media and format against the platform's rules before a Post can be marked ready; a written matrix of supported formats and limits; clear messages for unsupported content.
**Excludes.** Video, stories, reels, carousels, multiple images, generated images.

**Acceptance criteria.**
1. An approved single-image Post appears on a tester-mode Page with image and caption, and is confirmed with platform evidence.
2. A Post whose image is missing, removed or outside the platform's limits cannot be submitted for approval, and the reason is shown.
3. Replacing the image on an approved Post requires new approval.
4. A request for an unsupported format is refused before approval with a statement of what is supported.
5. The format matrix is recorded and matches what was actually tested.

**Requirement references.** FR-31, FR-33, FR-34; AC-12 (each committed format, media rejected before approval); PRD §3.2 (Facebook publishing); ADR-019.
**Prerequisites.** P1-E03-T21; P2-E04-T31, T35.
**Independence this week.** Publishing only.
**Estimate and difficulty.** 18 h. V Med, U Med (platform media rules), I Med, S Med, T Med.
**Completion evidence.** A real image post on a tester-mode Page and a rejected invalid image.
**Risks and assumptions.** If the platform no longer supports a committed format, changing it needs a documented release-contract decision (PRD §3.2).

### P2-E06-T48 — Constraint diagnosis and curated recipes

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P2-E06 | 12 | D | 22 h | 18–32 h | Low |

**User story.** As a business owner, I want the Director to tell me what is most likely holding my marketing back and what to do first, with its reasons and doubts, so that I spend limited time and money on the right thing.

**Problem and outcome.** The Director carries out requests but does not diagnose. After this task it gives a compact diagnosis and can run about six curated recipes.

**Scope.** A diagnosis that assesses offer and audience, destination, conversion, acquisition and existing-customer opportunities against the goal and actual conversion route; the output stating the primary constraint hypothesis, supporting observations with sources and dates, alternatives, the missing evidence most likely to change the choice, and a recommended next action; about six recipes chosen from PRD §5.13 for the launch cohort; staying within marketing scope.
**Excludes.** The remaining recipes (T91). Numeric health scores, priority indices or mandatory worksheets, which must not appear.

**Acceptance criteria.**
1. For a business with many inquiries and few bookings, the diagnosis examines qualification, offer and response process before recommending more posting or spending.
2. For a new business with unknown audience and baseline, unknowns stay explicit and no audience proportion, acquisition cost or growth forecast is invented.
3. For a contrast pair, the changed fact changes the primary hypothesis or the recommended action.
4. Each diagnosis separates observations, conclusions and hypotheses and names at least one alternative.
5. A scoped request (for example one post) is carried out without a full diagnosis being forced first.
6. The recommendation may be "no new acquisition campaign" when the evidence points elsewhere.
7. The list of delivered recipes and of those not delivered is recorded.

**Requirement references.** FR-14, FR-15, FR-21; PRD §5.13; AC-18, AC-19; D-44, D-59; DS §9.7.
**Prerequisites.** P2-E04-T25, T29, T36.
**Independence this week.** Changes the Director's analysis behaviour only; T45 does not depend on it.
**Estimate and difficulty.** 22 h. V Med, U High (quality depends on model behaviour), I Med, S Low, T High.
**Completion evidence.** Diagnoses for four evaluation cases including one contrast pair.
**Risks and assumptions.** Quality is judged by humans in T56; a failing result is tuned in T91 and T95.

### P2-E06-T68 — A context change holds affected scheduled Posts

> **Additional task above the 64-task baseline**, added by the plan owner's decision on DR-2. Carried by C as a second task this week (38 h in the week).

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P2-E06 | 12 | C (second task) | 20 h | 16–28 h | Medium |

**User story.** As a business owner, I want a post I already approved to wait for me if I change the price or offer it mentions, so that the product never publishes an offer that is no longer true.

**Problem and outcome.** Affected work is marked (T66), but an approved Post still goes out at its time. After this task an approved or scheduled Post whose context changed is held for the owner's review and is checked again at the moment of dispatch.

**Scope.** Placing an approved or scheduled Post on a context-review hold when an item it used changes; checking for that hold immediately before dispatch; the owner's choices on a held Post: confirm it unchanged, edit it into a new version that needs new approval, or cancel it; a notification that names the changed item and opens the Post; the hold written to the protected record that T43 keeps.
**Excludes.** Marking documents and drafts (T66). Other holds (T43). Bulk review of held Posts (T82).

**Acceptance criteria.**
1. Changing a price after a Post that states it was approved puts that Post on hold, and it is not published at its scheduled time.
2. A change made in the last moments before dispatch still results in a hold or a truthful published state; it never results in a silent send of the old content.
3. A Post that did not use the changed item stays scheduled.
4. Confirming a held Post unchanged follows the same approval rule as other holds resolved without changes; editing it requires new approval.
5. The owner receives one notification for the hold, which opens the exact Post.
6. After a disconnect and reconnect, a context-held Post is still held.

**Requirement references.** FR-35, FR-48; NFR-01, NFR-05; AC-23; SYS §4.6 (flow 4).
**Prerequisites.** P2-E04-T31, T35; P2-E05-T43, T66.
**Independence this week.** Accepted on text and link Posts, which exist since T35; it does not need image posts (T47). Context changes are made by the means that existed at the end of Week 11, so it does not need Pending Brain Updates (T46), step launch (T45) or diagnosis (T48).
**Estimate and difficulty.** 20 h. V Med, U Med, I High, S High, T High (the race at dispatch must be induced).
**Completion evidence.** AC-23 demonstrated on a tester-mode Page.
**Risks and assumptions.** It runs on top of C's primary task, so C needs about 38 hours this week.

---

## Week 13 — P2-E06

### P2-E06-T49 — Owner-reported Done, Skip and Revisit

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P2-E06 | 13 | A | 18 h | 15–23 h | High |

**User story.** As a business owner, I want to record that I did a step myself, skip a step, or come back to one, so that my Roadmap reflects what really happened without pretending the product did or verified it.

**Problem and outcome.** A step's status can only follow a Task. After this task the owner can attest completion, skip and revisit, and progress is counted truthfully.

**Scope.** Mark as Done with a checklist and optional evidence, recorded as owner-reported; Skip recording intent and time and showing its effect on dependents; Revisit recomputing readiness; Not Applicable; progress counts for completed, owner-reported, skipped, blocked and not applicable; skipping a step whose Task is running leaves the Task running and offers a separate Cancel.
**Excludes.** Strategic revisions (T88, T89).

**Acceptance criteria.**
1. A step marked Done by the owner shows as owner-reported and is never shown as verified by the product.
2. A skipped step is not counted as completed or cancelled and shows when it was skipped.
3. Skipping a step tells the owner which dependent steps are now blocked and suggests an alternative or a revision; no dependency is waived silently.
4. Revisit returns the step to planned, ready or blocked according to its prerequisites at that moment.
5. Skipping a step with a running Task leaves the Task running; Cancel is a separate action.
6. Progress shows the five counts separately and makes no statement that a business objective was achieved.

**Requirement references.** FR-46, FR-49, FR-53; NFR-01; AC-24, AC-40; SYS §3.4 (Roadmap Step).
**Prerequisites.** P2-E05-T41; P2-E06-T45.
**Independence this week.** Roadmap only.
**Estimate and difficulty.** 18 h. V Med, U Low, I Med, S Low, T Med.
**Completion evidence.** AC-40 demonstrated, including Skip on a running step.
**Risks and assumptions.** None material.

### P2-E06-T50 — Brain List governance

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P2-E06 | 13 | B | 22 h | 18–29 h | Medium |

**User story.** As a business owner, I want to browse everything the product knows about my business, see what is imported, confirmed or inferred, and correct or remove it, so that I stay in charge of my business's record.

**Problem and outcome.** The Brain has a minimal list. After this task the List is the complete accessible way to explore, correct, relate and remove knowledge.

**Scope.** Browsing by category with status and source labels; item detail with history, related items and related decisions; adding and correcting items and relationships; removing an item so that it immediately stops taking part in retrieval; retiring an item as a historical withdrawal, distinct from removal; a link to pending updates; the List as the default Brain view on phones.
**Excludes.** The interactive graph (T77). Export (T67), clean-up of derived copies (T65), restore suppression (T69) and flagging of dependent work (T66, T68) are separate tasks; this screen uses the first and fourth.

**Acceptance criteria.**
1. Every item shows its category, its epistemic status and where it came from.
2. An owner correction becomes the current value with the previous value kept in history.
3. A removed item is not returned by retrieval from the moment of removal, and no Task started afterwards uses it.
4. Retire and remove are separate actions with their different consequences explained before the owner confirms.
5. A relationship can be added and removed, and affects which related items retrieval returns.
6. Uncertain and conflicting items are visibly marked.
7. Removing an item starts the clean-up of derived copies (T65) and shows its status; correcting an item marks the work that used it (T66).

**Requirement references.** FR-09, FR-10, FR-12, FR-13, FR-42, FR-43; NFR-01, NFR-09; AC-03 (list part); DS §10.7.
**Prerequisites.** P1-E03-T17, T24; P2-E04-T26, T65; P2-E05-T66; P2-E06-T46.
**Independence this week.** Brain only. It does not need restore suppression (T69).
**Estimate and difficulty.** 22 h. V High, U Low, I Med, S High, T Med.
**Completion evidence.** Correction with history, and removal followed by a retrieval check.
**Risks and assumptions.** AC-03 stays partly unmet until the graph is built (T77).

### P2-E06-T51 — Notification Bell with exact links

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P2-E06 | 13 | C | 20 h | 17–26 h | High |

**User story.** As a business owner, I want to be told when something needs me or has finished, and to go straight to that item, so that I do not have to check every area to know what is waiting.

**Problem and outcome.** Notification records exist but the owner cannot see them. After this task the top bar shows unread notifications that open their exact subject.

**Scope.** Bell with unread count; list with read and unread controls and mark all read; display of the notification records that each earlier feature task already writes under the Definition of Done (Task completion and terminal failure, clarification needed, document or research ready, Post approval needed, publication, hold and uncertainty, reconnection needed, Brain proposal and Roadmap proposal); exact subject links with an access check; an honest unavailable state; a check that every listed kind is in fact being written, with any missing one recorded as a defect against its feature.
**Excludes.** Email or push delivery. Notifications for recoverable retries, which must not exist. Revision approval notices (with T88, T89).

**Acceptance criteria.**
1. Each listed event produces one notification that opens the exact form, Post, document, proposal or setting.
2. Opening a link checks that the owner may see the subject; a link to a removed subject shows unavailable, not an error or another item.
3. Read, unread, the count and mark all read behave consistently across reloads.
4. Marking a notification read approves nothing.
5. A recoverable internal retry produces no notification.
6. A link taken from another workspace's notification is refused.

**Requirement references.** FR-02, FR-56; NFR-01, NFR-03; AC-34 (deep links), AC-55; SYS §5.3; DS §10.11.
**Prerequisites.** P1-E02-T14 and the features whose notification records it shows (T22, T29, T31, T35, T36, T37, T39, T40, T43, T46).
**Independence this week.** Every subject it links to existed at the end of Week 12.
**Estimate and difficulty.** 20 h. V Med, U Low, I High (many subjects), S Med, T Med.
**Completion evidence.** AC-55 demonstrated for each notification kind, including an unavailable subject.
**Risks and assumptions.** None material.

### P2-E06-T52 — Deliverables library with versions and document export

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P2-E06 | 13 | D | 20 h | 17–26 h | High |

**User story.** As a business owner, I want to find, edit, copy, export and remove the documents made for me, so that I can use them outside the product and keep my library in order.

**Problem and outcome.** Documents can only be read. After this task Deliverables is a working library.

**Scope.** Search and filters by type and date with paging; editing that creates a new version of the same document; copy; export as text and Markdown; removal with a confirmation that explains the effect; version history; source, context and limitation display.
**Excludes.** Reuse and Continue with Director, CSV and original-media export (T86); whole-history export (T67); designed PDF or Word export, which is outside Phase 2.

**Acceptance criteria.**
1. A document can be found by a word in its title and by type and date.
2. An owner edit creates a new immutable version of the same document; earlier versions stay readable.
3. Export produces a text and a Markdown file containing the content, sources and date.
4. A removed document no longer appears in the library and cannot be opened by its address.
5. No Post appears in Deliverables.
6. With more documents than one page holds, paging returns each document once.

**Requirement references.** FR-13, FR-41, FR-57; NFR-09; AC-15 (history, delete), AC-47; DS §10.8; OD-19.
**Prerequisites.** P1-E03-T20; P2-E04-T29, T65; P2-E05-T66 (the possibly-outdated mark is displayed here).
**Independence this week.** Deliverables only.
**Estimate and difficulty.** 20 h. V Med, U Low, I Low, S Med, T Low.
**Completion evidence.** AC-47 demonstrated, with an export opened outside the product.
**Risks and assumptions.** Removal uses the clean-up of derived copies from T65.

### P2-E06-T69 — A restore never brings back removed data

> **Additional task above the 64-task baseline**, added by the plan owner's decision on DR-2. Carried by D as a second task this week (35 h in the week).

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P2-E06 | 13 | D (second task) | 15 h | 12–24 h | Low |

**User story.** As a business owner, I want information I removed to stay removed even if the product is restored from a backup, so that a technical recovery cannot undo my decision.

**Problem and outcome.** A restore to an earlier point would bring back data the owner removed after that point. After this task removals are kept in a protected record and are applied again after any restore, before the restored data can be used.

**Scope.** Writing each completed removal to the protected record that T43 keeps apart from the main data; reapplying those removals after a restore, across the main data and every derived store, before retrieval or any Task can use the restored data; one restore drill on staging that shows it; the operator steps for it.
**Excludes.** The production rehearsal (T60), which repeats this on production. Expiry of old backups, which follows the T16 policy and the storage provider.

**Acceptance criteria.**
1. An item removed after the restore point is absent from the main data and every derived store once the restore procedure finishes.
2. Between the restore and the reapplication, no Task and no retrieval can use the restored item.
3. The protected record identifies what to remove without holding the removed content.
4. The drill is recorded with its duration and is run by a person who did not write the procedure.
5. A restore in one workspace's favour changes nothing about another workspace's removals.

**Requirement references.** FR-13; NFR-05, NFR-09; PRD §13.3 (data controls, recovery); SYS §4.5.
**Prerequisites.** P1-E02-T16; P2-E05-T43; P2-E04-T65.
**Independence this week.** Uses removals made through T65's behaviour. It does not need the Brain List (T50), the Bell (T51), the library (T52) or step reporting (T49).
**Estimate and difficulty.** 15 h. V Low, U High, I High, S High, T High.
**Completion evidence.** The recorded staging drill.
**Risks and assumptions.** A staging restore takes the environment away from the other Week 13 tasks for its duration; the window is agreed on day one. It runs on top of D's primary task, so D needs about 35 hours this week.

---

## Additional build tasks from the former overflow register (Weeks 7–13, T70–T91)

> Added on 2026-11-14 by the plan owner's instruction to schedule every remaining commitment inside the 16 weeks with no hour limit. They are listed here by week, after the weekly sections above. Each is carried by the named assignee in addition to that person's other tasks in the week. The Definition of Done applies to each.

### P2-E04-T70 — Owner signs in with Google and accounts link safely

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P2-E04 | 7 | B | 30 h | 22–40 h | Medium |

**User story.** As a business owner, I want to sign in with my Google account, so that I do not need another password.

**Problem and outcome.** Only email and password work. After this task an owner can register and sign in with Google, and an existing email account and a Google identity are joined only when it is safe.

**Scope.** Registration and sign-in with Google; linking a Google identity to an existing account only after the owner proves control of that account; refusing to link on an unverified email; the same session rules as email sign-in; truthful errors when the provider refuses or is unavailable; audit records for sign-in and linking.
**Excludes.** Other identity providers. The Settings screen that lists providers (T73).

**Acceptance criteria.**
1. A new owner registers with Google and lands in onboarding with one workspace.
2. An owner who registered by email cannot be taken over by someone who signs in with a Google identity that shows the same address but is unverified.
3. Linking requires proof of control of the existing account, and both sign-in methods then open the same workspace.
4. A cancelled or failed provider step returns the owner to sign-in with a plain message and creates no account.
5. Sessions created through Google rotate and revoke exactly as email sessions do.

**Requirement references.** FR-50, FR-56; NFR-03; AC-32.
**Prerequisites.** P1-E02-T09, T13, T12.
**Independence this week.** Accounts only. It needs no Week 7 output.
**Estimate and difficulty.** 30 h. V Med, U Med (provider setup), I Med, S High, T Med.
**Completion evidence.** AC-32 demonstrated on staging, including the refused takeover.
**Risks and assumptions.** Needs a Google developer project and consent-screen approval; the person responsible is named in T03's register.

### P2-E04-T71 — Evaluation library completed to 24 cases with a held-out set

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P2-E04 | 7 | C | 20 h | 16–26 h | High |

**User story.** As a release decision-maker, I want the full set of evaluation cases the PRD describes, so that the usefulness gate rests on enough evidence.

**Problem and outcome.** Fourteen cases exist. After this task about 24 authored cases exist, with a held-out subset that is not used during tuning.

**Scope.** Ten more cases covering the launch cohort, the four registers and the PRD's named scenarios; fixed source snapshots for each; expected-behaviour notes and rubric references; marking of the held-out subset; a record of which case serves which acceptance scenario.
**Excludes.** Running the cases (T56, T93, T95).

**Acceptance criteria.**
1. About 24 cases exist, each with business context, request, sources and rubric reference.
2. Every register has at least the number of cases the T07 standard requires.
3. The held-out subset is marked and is not referenced by any tuning task before T95.
4. Each case names the acceptance scenarios it can evidence.

**Requirement references.** PRD §10.1, §10.2; D-23, D-38; AC-18, AC-19.
**Prerequisites.** P1-E01-T07; P1-E03-T23.
**Independence this week.** Documents only.
**Estimate and difficulty.** 20 h. V Med, U Low, I Low, S Low, T Low.
**Completion evidence.** The case library reviewed by a second person.
**Risks and assumptions.** Gulf cases need a named country and a reviewer (XD-8).

### P2-E04-T72 — Owner uploads a spreadsheet and it is read safely

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P2-E04 | 7 | D | 25 h | 20–34 h | Medium |

**User story.** As a business owner, I want to give the Director my own numbers in a CSV or Excel file, so that its analysis uses my real results.

**Problem and outcome.** Files can be stored but spreadsheets are refused. After this task a CSV or .xlsx file in a documented layout is accepted, checked and turned into a table the product can analyse.

**Scope.** Upload of CSV and .xlsx within the T21 limits; the documented layouts the PRD authorizes; safe reading that never runs a formula, macro or external link; recognition of columns, dates, currencies and Arabic or English headers; a preview the owner confirms; clear refusal of unsupported layouts with the reason; missing values kept as unknown.
**Excludes.** Interpretation (T75). Free-form layouts, which §13.3 allows to be bounded.

**Acceptance criteria.**
1. A file in a documented layout is accepted and previewed with its columns correctly typed.
2. A file with formulas, macros or external references is read as values only, or refused; nothing in it is executed.
3. An empty cell is shown as unknown, never as zero.
4. An unsupported layout is refused with a message that names what is missing.
5. The owner must confirm the preview before the data can be used.
6. Another workspace cannot read the file or its table.

**Requirement references.** FR-38; NFR-03, NFR-04; AC-14 (intake part); PRD §13.3.
**Prerequisites.** P1-E02-T14, T15; P1-E03-T21, T22.
**Independence this week.** Uses file storage from Week 6. It does not need orchestration (T25).
**Estimate and difficulty.** 25 h. V Med, U Med, I Med, S High, T Med.
**Completion evidence.** Four files on staging: valid CSV, valid .xlsx, hostile file, unsupported layout.
**Risks and assumptions.** None material.

### P2-E04-T73 — Owner changes account, region and language settings

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P2-E04 | 8 | B | 20 h | 16–26 h | High |

**User story.** As a business owner, I want to change my sign-in methods, region, currency, time zone and default languages after setup, so that the product keeps matching my business.

**Problem and outcome.** These values are set once in onboarding. After this task Settings lets the owner see and change them.

**Scope.** Account details and the list of linked sign-in providers with link and unlink; region, currency and planning time zone; separate defaults for interface language, conversation language and content register; the effect of each change explained before saving; audit records.
**Excludes.** The Facebook connection controls (T19, T43). Billing, which is outside Phase 2.

**Acceptance criteria.**
1. Each default can be changed separately, and changing one does not change another.
2. Changing the planning time zone does not move the instant of any already scheduled Post.
3. The last remaining sign-in method cannot be unlinked.
4. A changed default applies to new work and does not rewrite existing documents or Posts.
5. The screen works in Arabic and English, on a phone and by keyboard.

**Requirement references.** FR-44, FR-56; NFR-07; AC-11 (settings), AC-29 (independent language settings).
**Prerequisites.** P1-E02-T09, T13; P2-E04-T26, T27, T70.
**Independence this week.** Settings only; it reads values that existed at the end of Week 7.
**Estimate and difficulty.** 20 h. V Med, U Low, I Med, S Med, T Low.
**Completion evidence.** AC-29's settings part demonstrated on a phone.
**Risks and assumptions.** None material.

### P2-E04-T74 — Director produces an editable campaign package

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P2-E04 | 9 | A | 15 h | 12–22 h | Medium |

**User story.** As a business owner, I want to ask for a whole campaign and get its posts and supporting documents as one editable package, so that I can review it as a unit.

**Problem and outcome.** The Director returns single items. After this task a campaign request produces a grouped set of Posts and documents that stay linked.

**Scope.** A campaign result made of several Posts and its brief or plan document; a shared campaign reference on each item; a package view that lists the items with their states; editing or removing one item without losing the others; no forced campaign when the owner asked for one item.
**Excludes.** Brand profile and claim checks (T76). Bulk approval of the package (T82).

**Acceptance criteria.**
1. A campaign request produces Posts in Scheduling and documents in Deliverables that show the same campaign.
2. A request for one post still produces one Post and no campaign.
3. Editing one item creates a new version of that item only.
4. Each Post still needs its own approval unless the owner uses the bulk action.

**Requirement references.** FR-29, FR-57; AC-10.
**Prerequisites.** P2-E04-T25, T27, T29, T31.
**Independence this week.** Uses typed results from Week 8. It does not need publishing (T35) or research (T36).
**Estimate and difficulty.** 15 h. V Med, U Med, I Med, S Low, T Med.
**Completion evidence.** AC-10 demonstrated on an evaluation case.
**Risks and assumptions.** None material.

### P2-E04-T75 — Director interprets the owner's numbers against a goal

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P2-E04 | 9 | D | 25 h | 20–36 h | Low |

**User story.** As a business owner, I want the Director to tell me what my numbers mean for my goal, without inventing figures, so that I can decide what to do next.

**Problem and outcome.** A spreadsheet can be read but not analysed. After this task a requested analysis returns a document whose every figure is calculated by fixed arithmetic from the confirmed table.

**Scope.** An analysis request from Chat on a confirmed table; calculations done by deterministic arithmetic, not by the model; interpretation against the owner's stated goal; unknown and zero-denominator cases reported as "cannot be calculated"; each statement labelled as measured, derived or assumed; the result saved as a document with the table and method named.
**Excludes.** Connected analytics sources, which are a later phase. Charts beyond simple tables.

**Acceptance criteria.**
1. Every figure in the result can be reproduced by hand from the table.
2. A rate with a zero or unknown denominator is reported as not calculable, never as zero or one hundred percent.
3. A column with unknown values is not treated as zero in any total.
4. The result separates what the data shows from what the Director suggests.
5. With too little data the result says so and proposes what to collect.

**Requirement references.** FR-21, FR-38, FR-39, FR-40; NFR-02, NFR-04; AC-14, AC-25.
**Prerequisites.** P2-E04-T25, T29, T72.
**Independence this week.** Uses orchestration and documents from Weeks 7–8.
**Estimate and difficulty.** 25 h. V Med, U High, I High, S Med, T High.
**Completion evidence.** AC-14 and AC-25 demonstrated with a hand check of each figure.
**Risks and assumptions.** Model behaviour around numbers is the main uncertainty.

### P2-E05-T76 — Brand profile and claim checks on generated copy

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P2-E05 | 10 | A | 15 h | 12–22 h | Medium |

**User story.** As a business owner, I want copy written in my brand's voice and free of claims I cannot support, so that my posts do not mislead my customers.

**Problem and outcome.** Copy uses Brain facts but nothing checks its claims. After this task a confirmed brand profile guides copy, and a claim with no confirmed basis is flagged before approval.

**Scope.** A brand profile (voice, vocabulary to use and avoid, visual cues) kept as confirmed Brain knowledge; its use in generated copy; a check of each factual or comparative claim against confirmed facts; a visible flag with the unsupported claim named; the owner's choice to edit, confirm a basis or remove the claim.
**Excludes.** Legal review. Image generation.

**Acceptance criteria.**
1. Copy for the same request differs between two businesses with different brand profiles.
2. A planted claim with no confirmed basis is flagged and named.
3. A flagged Post cannot be marked ready for approval until the owner resolves the flag.
4. A claim supported by a confirmed fact is not flagged and shows its basis.

**Requirement references.** FR-30; NFR-02; AC-10 (claims).
**Prerequisites.** P1-E03-T17; P2-E04-T29, T32, T74.
**Independence this week.** Works on drafts; it does not touch publishing states (T39) or the Roadmap (T37).
**Estimate and difficulty.** 15 h. V Med, U Med, I Med, S Med, T Med.
**Completion evidence.** The planted-claim case on staging.
**Risks and assumptions.** None material.

### P2-E05-T77 — Interactive Brain graph of focused neighbourhoods

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P2-E05 | 10 | B | 20 h | 16–28 h | Medium |

**User story.** As a business owner, I want to see how the facts about my business connect, so that I can understand and question what the Director relies on.

**Problem and outcome.** Knowledge is shown as a list only. After this task the owner can open a graph view centred on one item and its neighbours, from the same records as the list.

**Scope.** A graph of one focused item and its related items, as §13.3 authorizes; status and source shown on each node; moving the focus to a neighbour; opening an item's detail; a list alternative for every graph action; keyboard and screen-reader access; a phone layout that falls back to the list.
**Excludes.** A whole-Brain graph. Editing in the graph beyond opening the item.

**Acceptance criteria.**
1. The graph and the list show the same items, statuses and relationships.
2. Candidate, rejected and removed items appear exactly as their status requires and never as confirmed.
3. Every action available in the graph can be done by keyboard and through the list.
4. With many neighbours the view stays usable and says how many are not shown.

**Requirement references.** FR-08, FR-12; NFR-01, NFR-07; AC-03 (graph).
**Prerequisites.** P1-E02-T10; P1-E03-T17, T24; P2-E04-T26.
**Independence this week.** Reads knowledge records as they stood at the end of Week 9.
**Estimate and difficulty.** 20 h. V Med, U Med, I Low, S Low, T Med.
**Completion evidence.** AC-03's graph part demonstrated in both languages.
**Risks and assumptions.** None material.

### P2-E05-T78 — Scheduling calendar in Month and Week views

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P2-E05 | 10 | C | 15 h | 12–22 h | Medium |

**User story.** As a business owner, I want to see my posts on a calendar, so that I can judge my week and month at a glance.

**Problem and outcome.** Scheduling has a list only. After this task Month and Week views show the same Posts as the list.

**Scope.** Month and Week views in the planning time zone; each Post shown with its true state; agreement between Month, Week and List; right-to-left calendars for Arabic; opening a Post from the calendar; a phone layout.
**Excludes.** Drag rescheduling and daylight-saving checks (T81).

**Acceptance criteria.**
1. The same Post appears on the same day and time in Month, Week and List.
2. A Post with no date appears only in Unscheduled.
3. A published Post is shown at its actual publication time.
4. The calendar reads right to left in Arabic with the correct first day of the week for the workspace region.

**Requirement references.** FR-55; NFR-01, NFR-07; AC-50.
**Prerequisites.** P2-E04-T27, T31, T35.
**Independence this week.** Displays Post states as recorded; it does not need reconciliation (T39).
**Estimate and difficulty.** 15 h. V Med, U Low, I Low, S Low, T Med.
**Completion evidence.** AC-50 demonstrated in both languages.
**Risks and assumptions.** None material.

### P2-E05-T79 — Research on an exact Discovery question

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P2-E05 | 10 | D | 15 h | 12–22 h | Medium |

**User story.** As a business owner, I want to ask the Director to research a question I cannot answer in the wizard, so that I do not have to guess.

**Problem and outcome.** A Discovery question can only be answered or skipped. After this task the owner can start research on that exact question and see that it is in progress.

**Scope.** A "research this" action on a Discovery question; a research Task whose question is the wizard question with its context; a truthful "Researching" status on the question; the Task counted under the two-slot admission; the wizard staying usable meanwhile.
**Excludes.** Bringing the result back into the wizard and the Roadmap (T83).

**Acceptance criteria.**
1. Starting research on a question creates exactly one Task tied to that question, even on a double click.
2. The question shows Researching until the Task ends and then shows the true outcome.
3. The owner can answer other questions while it runs.
4. If research fails, the question returns to unanswered with the reason.

**Requirement references.** FR-17, FR-51, FR-52; AC-37 (start).
**Prerequisites.** P2-E04-T34, T36.
**Independence this week.** It does not change plan generation (T37), which uses answers as saved.
**Estimate and difficulty.** 15 h. V Low, U Med, I Med, S Low, T Med.
**Completion evidence.** A wizard question researched on staging.
**Risks and assumptions.** None material.

### P2-E05-T80 — Canvas handles related Tasks and late events

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P2-E05 | 11 | B | 20 h | 16–28 h | Medium |

**User story.** As a business owner, I want the Canvas to show only the work of the conversation I am looking at, and to let me pick among its Tasks, so that I am never shown another conversation's activity.

**Problem and outcome.** The Canvas follows one Task. After this task it lists the conversation's related Tasks and ignores events that arrive late from another conversation.

**Scope.** Choosing among the Tasks of the selected conversation; history of finished related Tasks as recorded; discarding an event that belongs to a conversation the owner has left; correct state after fast switching; the list alternative kept in step.
**Excludes.** Replay of past runs beyond recorded events (ADR-020).

**Acceptance criteria.**
1. Switching conversations while a Task streams never shows the first conversation's events in the second.
2. The owner can open any related Task of the conversation and sees its recorded events.
3. Rapid switching ends with the Canvas matching the selected conversation.
4. A waiting Task shows its waiting state with the link to its form.

**Requirement references.** FR-22; NFR-01; AC-42.
**Prerequisites.** P2-E04-T33; P2-E05-T40.
**Independence this week.** Canvas only.
**Estimate and difficulty.** 20 h. V Med, U Med, I Med, S Med, T High (timing must be induced).
**Completion evidence.** AC-42 demonstrated with two concurrent Tasks.
**Risks and assumptions.** None material.

### P2-E05-T81 — Drag rescheduling and daylight-saving correctness

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P2-E05 | 11 | C | 15 h | 12–22 h | Medium |

**User story.** As a business owner, I want to move a post to another day by dragging it, and to trust the time across clock changes, so that rescheduling is quick and never off by an hour.

**Problem and outcome.** Rescheduling is by form only. After this task a Post can be dragged in the calendar, with the same approval rules, and times stay right across a daylight-saving change.

**Scope.** Drag to a new day or time in Month and Week; a keyboard equivalent; the same new-version and approval-clearing rule as the form; refusal to drag a published or in-flight Post; correct handling of times that do not exist or occur twice on a clock-change day.
**Excludes.** Bulk moves.

**Acceptance criteria.**
1. Dragging an approved Post creates a new version and clears its approval, exactly as the form does.
2. A published or dispatching Post cannot be dragged.
3. A time chosen on a clock-change day is stored and shown as the owner meant it, and the ambiguity is explained when it exists.
4. The same move can be made by keyboard.

**Requirement references.** FR-55; NFR-07; AC-50 (clock changes), AC-52.
**Prerequisites.** P2-E04-T31; P2-E05-T78.
**Independence this week.** It does not need the new held states (T43) or bulk approval (T82).
**Estimate and difficulty.** 15 h. V Low, U Med, I Med, S Med, T High.
**Completion evidence.** AC-52 demonstrated, with a clock-change day in a zone that has one.
**Risks and assumptions.** None material.

### P2-E05-T82 — Bulk approval and Batch Review

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P2-E05 | 11 | A | 20 h | 16–28 h | Medium |

**User story.** As a business owner, I want to review a batch of posts in one sitting and approve the ones I accept, so that approving a campaign does not take one screen per post.

**Problem and outcome.** Each Post is approved alone. After this task the owner can review a batch and approve several exact versions in one action, with every decision still recorded per Post.

**Scope.** A Batch Review session over a chosen set or a campaign; each Post shown in full before it can be included; approve selected and approve all, each recording one approval per exact version; exclusion of Posts that changed during the session; held and flagged Posts excluded with the reason.
**Excludes.** Approval without showing the content.

**Acceptance criteria.**
1. A bulk approval records a separate approval for each Post's exact version.
2. A Post edited after the session opened is not approved by the bulk action.
3. A Post with an unresolved flag or hold cannot be included.
4. The owner sees how many were approved and which were left out, with reasons.
5. Approving all never approves a Post the owner was not shown.

**Requirement references.** FR-34, FR-55; AC-12 (batch), AC-51.
**Prerequisites.** P2-E04-T29, T31, T74; P2-E05-T39.
**Independence this week.** Accepted on Post states that existed at the end of Week 10.
**Estimate and difficulty.** 20 h. V Med, U Low, I Med, S High, T Med.
**Completion evidence.** AC-51 demonstrated, including an edit made mid-session.
**Risks and assumptions.** None material.

### P2-E05-T83 — Research answer returns to the wizard and the Roadmap

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P2-E05 | 11 | D | 15 h | 12–22 h | Medium |

**User story.** As a business owner, I want the research I asked for to come back to the question I asked, so that I can accept it and move on with my plan.

**Problem and outcome.** Question research produces a report with no way back. After this task the finding is offered on the question, and the owner chooses whether to use it in the Roadmap.

**Scope.** The finding and its sources shown on the originating question; accept, edit or decline; an accepted answer saved as owner-confirmed with its research origin; "Use in Roadmap" on a research report made from Chat; the proposed plan showing which answers came from research.
**Excludes.** Starting the research (T79).

**Acceptance criteria.**
1. A finished question research appears on its question with its sources.
2. Nothing from the research is used as an answer until the owner accepts it.
3. An accepted answer keeps its research origin and is visible in the proposed plan's evidence.
4. Declining leaves the question unanswered and the report available in Deliverables.

**Requirement references.** FR-17, FR-52, FR-57; AC-37.
**Prerequisites.** P2-E05-T37, T79.
**Independence this week.** Works on a proposed plan; it does not need approval and activation (T41).
**Estimate and difficulty.** 15 h. V Low, U Med, I Med, S Low, T Med.
**Completion evidence.** AC-37 demonstrated end to end.
**Risks and assumptions.** None material.

### P2-E05-T84 — A later Discovery reuses earlier answers

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P2-E05 | 11 | A | 10 h | 8–15 h | High |

**User story.** As a business owner, I want a second Discovery to remember what I already decided, so that I am not asked the same things again.

**Problem and outcome.** Discovery reuses Brain facts but not earlier Discovery answers. After this task earlier accepted answers are offered again, marked if they are old or in conflict.

**Scope.** Offering earlier accepted answers in a new Discovery; a mark on an answer whose underlying fact changed or expired; a mark on answers that conflict; the owner's confirm or change on each.
**Excludes.** Adaptive question selection (T34).

**Acceptance criteria.**
1. A second Discovery prefills answers accepted in the first and says where each came from.
2. An answer based on a fact that has since changed is marked and not silently reused.
3. Conflicting earlier answers are shown together for the owner to settle.

**Requirement references.** FR-51; AC-38.
**Prerequisites.** P2-E04-T34; P2-E05-T37, T66.
**Independence this week.** Discovery only.
**Estimate and difficulty.** 10 h. V Low, U Low, I Med, S Low, T Low.
**Completion evidence.** AC-38 demonstrated with a changed fact.
**Risks and assumptions.** None material.

### P2-E06-T85 — Honest handoff when Chat work needs something unsupported

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P2-E06 | 12 | A | 15 h | 12–20 h | Medium |

**User story.** As a business owner, I want the Director to say plainly when a job needs a tool it does not have, and give me what I need to do it myself, so that I am never told something was done when it was not.

**Problem and outcome.** The handoff contract exists for Roadmap steps only. After this task a Chat request that needs an unsupported connector ends with the same honest handoff.

**Scope.** Recognizing that a request needs a capability the product lacks; doing the part it can; a handoff with instructions, prepared material and a checklist; the result labelled as owner-executed; no claim of completion for the unsupported part.
**Excludes.** Building any new connector.

**Acceptance criteria.**
1. A Chat request needing an unsupported platform returns prepared material and steps, and states what the product did not do.
2. Nothing in the Task's state or the Canvas shows the unsupported action as performed.
3. The owner can mark their own part done, and it is shown as owner-reported.

**Requirement references.** FR-49; NFR-01; AC-24.
**Prerequisites.** P2-E04-T25, T29, T33.
**Independence this week.** It does not use step launch (T45).
**Estimate and difficulty.** 15 h. V Low, U Med, I Med, S Med, T Med.
**Completion evidence.** AC-24 demonstrated from Chat.
**Risks and assumptions.** None material.

### P2-E06-T86 — Reuse of past work and remaining exports

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P2-E06 | 12 | D | 15 h | 12–20 h | High |

**User story.** As a business owner, I want to continue from a past document and export tables and media, so that earlier work keeps paying off.

**Problem and outcome.** Past documents can be read but not built on. After this task the owner can continue with the Director from a document, the Director can use past documents and Tasks as context, and tables and original media can be exported.

**Scope.** "Continue with Director" from a document, opening a conversation that carries it; retrieval of the workspace's past documents and Task results as context, with their source shown; export of a document's tables as CSV written as text only; download of original uploaded media.
**Excludes.** Whole-history export (T67). The library screen itself (T52), which shows these actions.

**Acceptance criteria.**
1. Continuing from a document starts a conversation in which the Director demonstrably uses that document.
2. A removed or possibly-outdated document is not used as current context.
3. A CSV export opens in a spreadsheet program with no value run as a formula.
4. Original media downloads only for its own workspace.

**Requirement references.** FR-11, FR-41; NFR-04; AC-15 (reuse, export).
**Prerequisites.** P1-E03-T20, T21, T24; P2-E04-T25, T29; P2-E05-T66.
**Independence this week.** Offered from the reading view in place since T29.
**Estimate and difficulty.** 15 h. V Med, U Low, I Med, S Med, T Low.
**Completion evidence.** AC-15's reuse and export parts demonstrated.
**Risks and assumptions.** None material.

### P2-E06-T87 — Duplicate a published Post and delete locally

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P2-E06 | 12 | C | 10 h | 8–15 h | High |

**User story.** As a business owner, I want to reuse a published post as a new draft and to remove a post from my list without confusion about Facebook, so that my history stays useful and honest.

**Problem and outcome.** A published Post can only be viewed. After this task it can be duplicated as a new draft, and deleting it in the product says exactly what does and does not happen on the Page.

**Scope.** Duplicate as a new unapproved draft with no schedule; local delete of a published Post's record with wording that the post stays on the Page; the publication record kept as the policy requires.
**Excludes.** Deleting the post on Facebook, which the product does not do.

**Acceptance criteria.**
1. A duplicate is a new draft with no approval, no schedule and a reference to its origin.
2. The delete confirmation states that the post remains on the Page and how to remove it there.
3. After local delete the Post no longer appears in Scheduling and its publication evidence is kept.

**Requirement references.** FR-55; NFR-01; AC-53.
**Prerequisites.** P2-E04-T27, T35; P2-E05-T39.
**Independence this week.** It does not need image posts (T47).
**Estimate and difficulty.** 10 h. V Low, U Low, I Low, S Med, T Low.
**Completion evidence.** AC-53 demonstrated on a tester-mode Page.
**Risks and assumptions.** None material.

### P2-E06-T88 — Strategic revision proposed with a clear diff

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P2-E06 | 12 | B | 15 h | 12–22 h | Medium |

**User story.** As a business owner, I want a proposed change to my active plan shown as a comparison with what I approved, so that I can see exactly what would change.

**Problem and outcome.** An active Roadmap can only be replaced. After this task the Director or the owner can propose a revision, shown as a diff against the active plan.

**Scope.** Proposing a revision of the active plan with its reason and evidence; a diff of added, removed and changed steps; the active plan unchanged while the proposal is pending; one pending revision at a time; discarding a proposal.
**Excludes.** Approving and applying it (T89).

**Acceptance criteria.**
1. A proposed revision lists every added, removed and changed step against the active plan.
2. The active plan and its step states are unchanged while a revision is pending.
3. A second proposal cannot be opened while one is pending.
4. Discarding leaves no trace on the active plan.

**Requirement references.** FR-16, FR-54; AC-04 (revision), AC-45 (proposal).
**Prerequisites.** P2-E05-T37, T41.
**Independence this week.** It does not need step launch (T45).
**Estimate and difficulty.** 15 h. V Med, U Med, I Med, S Med, T Med.
**Completion evidence.** A revision proposed on staging with its diff.
**Risks and assumptions.** None material.

### P2-E06-T89 — Revision approved and applied in one step

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P2-E06 | 13 | A | 15 h | 12–22 h | Medium |

**User story.** As a business owner, I want my approval of a revision to change the plan all at once or not at all, so that I never end up with half of an old plan and half of a new one.

**Problem and outcome.** A revision can be proposed but not applied. After this task approving it switches the active plan atomically and tells the owner what happened to work in progress.

**Scope.** Approval of the exact displayed revision; an all-or-nothing switch that keeps one active plan; carrying over the state of unchanged steps; a truthful outcome for steps that were running or done and were changed or removed; a notification record for a revision awaiting approval and its exact link.
**Excludes.** Proposing (T88).

**Acceptance criteria.**
1. After approval exactly one plan is active, and it is the approved revision.
2. Two simultaneous approvals result in one applied revision.
3. Unchanged steps keep their state; a removed step that was running is ended truthfully and the owner is told.
4. A revision changed after it was displayed cannot be approved without being shown again.
5. A pending revision produces one notification that opens it.

**Requirement references.** FR-16, FR-54, FR-56; NFR-01; AC-04, AC-45, AC-55 (revision notices).
**Prerequisites.** P2-E05-T41; P2-E06-T45, T88.
**Independence this week.** Accepted on step states that existed at the end of Week 12; it does not need owner-reported completion (T49). Its notification record is written here; its display in the Bell (T51, same week) is confirmed in T96.
**Estimate and difficulty.** 15 h. V Med, U Med, I High, S High, T High.
**Completion evidence.** AC-45 demonstrated with a running step.
**Risks and assumptions.** None material.

### P2-E06-T90 — Working preferences and proof and metric relations

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P2-E06 | 13 | B | 15 h | 12–20 h | Medium |

**User story.** As a business owner, I want the Director to notice how I like things done and to keep claims tied to their proof and goals tied to their measures, so that its work gets closer to mine over time.

**Problem and outcome.** The Brain holds facts but not working preferences or these two relations. After this task repeated owner edits lead to a proposed preference, and claims and goals can be linked to proof and metrics.

**Scope.** Proposing a working preference from repeated owner edits, as a candidate in Pending Brain Updates; using an approved preference in later work; claim-to-proof and goal-to-metric relationships as recorded knowledge; their use by claim checks and by plan evidence.
**Excludes.** The List screen's general relationship editing (T50).

**Acceptance criteria.**
1. After the owner makes the same kind of edit several times, a preference is proposed and is not used until approved.
2. An approved preference changes later output in the stated way; a rejected one never returns.
3. A claim linked to proof is not flagged by the claim check; removing the proof flags it again.
4. A goal shows the metric it is measured by.

**Requirement references.** FR-08, FR-10; AC-03.
**Prerequisites.** P1-E03-T17; P2-E04-T29; P2-E05-T76; P2-E06-T46.
**Independence this week.** Uses Pending Brain Updates from Week 12, not the Week 13 List screen.
**Estimate and difficulty.** 15 h. V Med, U Med, I Med, S Low, T Med.
**Completion evidence.** A preference proposed, approved and seen in later output.
**Risks and assumptions.** T50 also adds relationship editing this week; the two touch the same records and settle order at merge.

### P2-E06-T91 — Remaining recipes and diagnosis tuning

| Epic | Week | Assignee | Estimate | Range | Confidence |
|---|---|---|---|---|---|
| P2-E06 | 13 | C | 20 h | 16–28 h | Low |

**User story.** As a business owner, I want the Director to handle the full range of common marketing jobs well, so that its advice fits my situation and not a template.

**Problem and outcome.** About six recipes exist. After this task the ten to twelve recipes the PRD describes exist, and diagnosis has been tuned on the non-held-out cases.

**Scope.** The remaining recipes, including the one that uses requested analysis; tuning of diagnosis against the evaluation cases that are not held out; a record of each change and its effect on case results.
**Excludes.** The held-out cases and human review (T56, T95).

**Acceptance criteria.**
1. Ten to twelve recipes exist, each with its trigger, inputs, limits and expected output.
2. Each recipe runs on at least one evaluation case and stays inside its ceilings.
3. Tuning changes are recorded with before and after results on the same cases.
4. No held-out case was used.

**Requirement references.** FR-14, FR-15; PRD §5.13, §10.2.
**Prerequisites.** P2-E04-T36, T71, T75; P2-E06-T48.
**Independence this week.** Director configuration only.
**Estimate and difficulty.** 20 h. V Med, U High, I Med, S Low, T High.
**Completion evidence.** The recipe list with a run of each.
**Risks and assumptions.** Quality still depends on human review in T56 and T95.
