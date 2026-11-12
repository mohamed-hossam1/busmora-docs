# Key Domain Events

> Facts that something happened, named in the past tense and owned by one context. These are **domain events** (durable, meaningful to the business). They are distinct from **Canvas progress signals** (§9), which are a live-only UI feed.
> Aligned with PRD v2.0; the canonical owner/consumer list is [SYSTEM-ARCHITECTURE.md](../core/SYSTEM-ARCHITECTURE.md) §3.5.
> Every event carries the Workspace identity and its time. Payloads list only the essentials.
> Consumers in *italics* are cross-cutting: Audit Trail (**A**), Usage Metering (**U**), Notifications (**N**).

## 1. Identity & Workspace

| Event | When | Essential data | Consumers |
|---|---|---|---|
| **UserRegistered** | Email/password sign-up completed (no business details) | User | Identity (sends single-use verification link) |
| **EmailVerified** | User opens a valid verification link | User | Identity (continue to Onboarding) |
| **AuthIdentityLinked** | Google identity bound after reauthentication and explicit intent | issuer, subject | *A* |
| **SessionRevoked** | Logout, reuse detection or access revocation | session family | SSE streams close |
| **WorkspaceCreated** | Automatically on registration | Workspace | — |
| **WorkspaceSettingsChanged / BusinessTimeZoneChanged** | User edits Settings | changed fields | Governed Publishing (display only; **never** moves approved or scheduled Posts) |
| **OnboardingDraftUpdated** | Draft edited or extraction merged | draft, version | — |
| **OnboardingConfirmed** | Owner confirms the exact draft version | draft, version, confirmed fields | Business Brain (activates in the same transaction); *A* |
| **WorkspaceDeletionRequested / WorkspaceDeleted** | Owner asks; cleanup completes | requested-by | All contexts (stop work, suppress retrieval); *A* |

## 2. Market Intelligence

| Event | When | Essential data | Consumers |
|---|---|---|---|
| **OnboardingCrawlStarted** | User submits a website URL | URL | Onboarding progress |
| **OnboardingCrawlCompleted** | Crawl ends, fully or partially | pages read, partial flag, Snapshots | Identity (merge proposals into the draft) |
| **OnboardingCrawlFailed** | Site unreachable or unsupported | reason | Onboarding (offer manual completion) |
| **BrandKitDetected** | Logo, colors or tone cues found | detected / not-detected elements | Identity draft (optional fields) |
| **ResearchCrawlCompleted** | Gathering for a Task ends | sources used, sources skipped with reason, Research Signals, partial flag | Marketing Employee (continue Task); *U* |
| **SourceSkipped** | CAPTCHA, rate limit or block on a source | source, reason | Marketing Employee (try alternatives, disclose gap) |

## 3. Business Brain

| Event | When | Essential data | Consumers |
|---|---|---|---|
| **OnboardingKnowledgeActivated** | Confirmed intake becomes current context | items, provenance | Marketing Employee; *A* |
| **KnowledgeOwnerProvided** | Owner adds or edits a fact directly | item, previous and new version | Marketing Employee; *A* |
| **KnowledgeCandidateProposed** | A Task, Conversation or edit feedback proposes a durable fact or rule | item / rule, provenance | *N* (Brain proposal) |
| **KnowledgeItemApproved / KnowledgeCandidateRejected** | Owner reviews a Candidate | item | *A* |
| **KnowledgeItemRetired** | Historical withdrawal (not deletion) | item, reason | Marketing Employee (stop using it); *A* |
| **KnowledgeDeletionRequested / KnowledgeDeletionCompleted** | Owner deletes; cleanup of derived records completes | item | Retrieval suppression; affected artifacts lose that evidence; *A* |
| **RelationshipAdded / RelationshipRetired / RelationshipDeleted** | Owner edits links, or an approved Candidate brings one | from, to, type | *A* |
| **KnowledgeConflictDetected** | New information contradicts current context | items involved | *N* |
| **ContextVersionChanged** | Material change to offer, price, audience, service area or claim support | changed item, dependent work | Tasks / Deliverables (flag stale); Governed Publishing (hold conflicting scheduled Posts, FR-48); *N* |
| **ProceduralRuleApproved** | Owner approves a Procedural Rule | rule, scope | Marketing Employee (applies to future generation); *A* |

## 4. Marketing Employee

| Event | When | Essential data | Consumers |
|---|---|---|---|
| **TaskRequested** | A substantive request, Wizard question research or step launch | Task, origin, conversation | Conversation (creates linked conversation atomically when needed); Canvas |
| **TaskStarted** | Atomic admission (≤ 2 running per Workspace) | Task | — |
| **ConstraintDiagnosed** | Orchestrator settles the primary constraint hypothesis | lens, evidence, alternatives | Task record |
| **ClarificationRequested / ClarificationAnswered / ClarificationExpired** | Missing information; answer; 7 days unanswered | form, answers | Conversation; durable facts → Candidates; expiry cancels the Task (*N*) |
| **TaskBudgetExtensionRequested / Approved / Rejected** | Task reaches its authorized allowance | current usage, requested extension, reason, decision | Owner decision; *A*, *U* |
| **TaskCompleted / TaskPartiallyCompleted / TaskFailed / TaskCancelled** | Terminal outcome | typed result refs or explanation | Deliverables / Publishing (results); Step Execution; *N* (no notice for recoverable retries) |
| **ResourceConsumed** | Any model call, search, crawl or platform call with a cost | resource kind, quantity, cost | *U* |
| **RoadmapDraftUpdated / DiscoveryAnswerSaved / DiscoveryAnswerConfirmed** | Discovery progresses | draft, question, answer, status | Brain (confirmed owner decisions); — |
| **QuestionResearchStarted / ResearchResultSelected** | "Research this" on a Discovery question; owner uses a result | link, Task, result | Discovery draft (exact-question return) |
| **RoadmapProposed / RoadmapActivated / RoadmapRejected** | Generation saves a plan; owner decides | roadmap, version | *N*; *A* |
| **StepLaunched / StepSkipped / StepRevisited / StepNotApplicable** | Owner acts on a step | step, Task ref, decision | Step Execution; *A* |
| **OwnerImplementationAttested** | Owner marks external work as Done | step, checklist, evidence refs | Step Execution (labelled Owner-reported); *A* |
| **StrategicRoadmapRevisionProposed / Approved / Rejected** | Owner-directed or context-driven revision | revision, diff, decision | Roadmap; *N*; *A* |

## 5. Deliverables

| Event | When | Essential data | Consumers |
|---|---|---|---|
| **DeliverableCreated / DeliverableVersionCreated** | A document is saved or edited | type, origin refs, Source Citations | Conversation (document card); reusable facts → Candidates |
| **AnalysisUploadCreated / AnalysisUploadRejected** | CSV / `.xlsx` attached to an explicit Chat request passes or fails validation | template, period, reason | Task (mapping confirmation or template fallback) |
| **AnalysisMappingConfirmed** | Owner confirms field mapping | mapping | Deterministic metric computation |
| **DataExportRequested / DataExportCompleted** | Owner exports Brain / history | scope | *A* |

## 6. Governed Publishing

| Event | When | Essential data | Consumers |
|---|---|---|---|
| **PostDrafted** | Manual New Post or a typed Task result | Post, first Post Version, origin | — |
| **PostVersionCreated** | Owner edits, Director revises, or reschedule | Post, new version, author | Approval cleared; edit diffs → Procedural Rule Candidates; *A* |
| **PostSubmittedForApproval** | Complete parameters sent for review | Post | Channel Connections (connect prompt if needed) |
| **PostApproved / PostRejected** | Individual or bulk decision on exact displayed versions | Post Version | *N*; *A* |
| **PostScheduled / PostRescheduled** | Release registered or moved | ScheduledTime (`{ instant, zone, latenessAllowanceMinutes }`) | — |
| **PostHeldOnStaleness / PostHoldResolved** | Context change or authority/window problem; owner resolves | Post, reason, resolution | *N*; *A* |
| **PostCancelled / PostExpired** | Cancel before dispatch; pending past its time | Post | *N* |
| **PostSubmitted / PostOutcomeUncertain** | Provider acknowledged; acceptance unknown | attempt | Reconciliation; *N* for uncertainty |
| **PostPublished / PostPublishFailed** | Platform evidence confirms; confirmed failure | platform post id and URL, or reason | *A*; *N* |
| **ReviewSessionStarted / ReviewSessionEnded** | Batch Review opened / finished | session | Evaluation timing |

## 7. Channel Connections

| Event | When | Essential data | Consumers |
|---|---|---|---|
| **AccountConnected** | Owner completes authorization | Platform, account | Governed Publishing; *A* |
| **AccountReconnectNeeded** | Token or permission failure | Platform, account | Governed Publishing (hold future sends); *N*; *A* |
| **AccountDisconnected** | Owner disconnects; authorization generation advances | Platform, account | Governed Publishing (block future sends); *A* |

## 8. Evaluation (internal)

| Event | When | Essential data | Consumers |
|---|---|---|---|
| **ScenarioRun** | A Scenario executes in Treatment or Control | configuration, outputs, clarification and correction counts | Evaluation Round |
| **OutputScored** | A Reviewer scores one output | rubric scores | Evaluation Round (tie-break if two scores differ by more than 1 point) |

## 9. Canvas progress signals (not domain events)

Live, ephemeral signals for the selected conversation's Task, replayable only during the run plus a short grace window. Five baseline agent/handoff signals — `agent_started`, `agent_tool_used`, `agent_completed`, `agent_error`, `flow_connection` — plus `execution_event` for actual tool/service/workflow/input/hold transitions. Recoverable retries stay diagnostic only and appear as Processing (FR-23).

## 10. Key event chains

1. **Onboarding:** UserRegistered → EmailVerified → OnboardingCrawlStarted → OnboardingCrawlCompleted → OnboardingDraftUpdated → OnboardingConfirmed → OnboardingKnowledgeActivated.
2. **Direct Chat task:** TaskRequested → TaskStarted → ConstraintDiagnosed → ClarificationRequested → ClarificationAnswered → TaskStarted (slot reacquired) → ResearchCrawlCompleted → TaskCompleted → DeliverableCreated / PostDrafted → KnowledgeCandidateProposed.
3. **Roadmap:** RoadmapDraftUpdated → QuestionResearchStarted → ResearchResultSelected → DiscoveryAnswerConfirmed → RoadmapProposed → RoadmapActivated → StepLaunched → TaskCompleted (or OwnerImplementationAttested).
4. **Publish:** PostDrafted → PostSubmittedForApproval → (AccountConnected) → PostApproved → PostScheduled → PostSubmitted → PostPublished **or** PostOutcomeUncertain → reconciliation → PostPublished / held.
5. **Context change:** KnowledgeOwnerProvided (price edit) → ContextVersionChanged → PostHeldOnStaleness → PostHoldResolved or PostVersionCreated → PostApproved.
6. **Learning loop:** PostVersionCreated (owner edit) → KnowledgeCandidateProposed (Procedural Rule) → ProceduralRuleApproved → future Post Versions follow it.
