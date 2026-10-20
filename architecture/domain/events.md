# Key Domain Events

> Facts that something happened, named in the past tense and owned by one context. These are **domain events** (durable, meaningful to the business). They are distinct from **Canvas progress signals** (§9), which are live-only UI feed (01 RT-1).
> Every event carries the Workspace identity and its time (01 DT-4). Payloads list only the essentials.
> Consumers in *italics* are cross-cutting: Audit Trail (**A**), Usage Metering (**U**), Notifications (**N**).

## 1. Identity & Workspace

| Event | When | Essential data | Consumers |
|---|---|---|---|
| **UserRegistered** | Sign-up completed | User | Identity (sends verification) |
| **EmailVerified** | User confirms email | User | — |
| **WorkspaceCreated** | Automatically on registration (FR-001) | Workspace, Business name | Business Brain (empty Brain), Market Intelligence (awaits website URL) |
| **BusinessTimeZoneChanged** | User edits Settings | old zone, new zone | Governed Publishing (affects display only; **never** moves approved or scheduled Posts, 01 PB-12) |
| **WorkspaceDeletionRequested** | User asks; operator-run (01 DT-1) | requested-by | All contexts (stop work); *A* |
| **WorkspaceDeleted** | All data removed | minimal deletion record | Operators |

## 2. Market Intelligence

| Event | When | Essential data | Consumers |
|---|---|---|---|
| **OnboardingCrawlStarted** | User submits a valid website URL | URL | Notifications (progress indicator) |
| **OnboardingCrawlCompleted** | Crawl ends, fully or partially (≤ 10 pages / ≤ 5 min) | pages read, partial flag, Snapshots | Business Brain (extract Candidates); *N* ("N items ready for review") |
| **OnboardingCrawlFailed** | Site unreachable after retries | reason | *N* ("could not access your website") |
| **BrandKitDetected** | Logo, colors or font found during the Onboarding Crawl | detected elements, not-detected elements | Business Brain (Brand Kit facts; "logo not detected" flag, 01 BA-3) |
| **ResearchCrawlCompleted** | Gathering for a Task ends | sources used, sources skipped with reason, Research Signals, partial flag | Marketing Employee (continue Task); *U* |
| **SourceSkipped** | CAPTCHA, persistent rate limit or block on a source | source, reason | Marketing Employee (try alternatives); *N* (transparent notice, PRD §13.1) |

## 3. Business Brain

| Event | When | Essential data | Consumers |
|---|---|---|---|
| **KnowledgeCandidateProposed** | A crawl, Task, Conversation or edit feedback proposes an item | Knowledge Item (or Procedural Rule), Provenance, confidence | *N* (badge, "3 new knowledge items") |
| **KnowledgeItemApproved** | Review approves (possibly after editing) | item, final content | Marketing Employee (now usable); *A* |
| **KnowledgeCandidateRejected** | Review rejects | item | *A* |
| **KnowledgeItemAddedManually** | User adds on the Brain page (approved immediately, 01 BG-1) | item | Marketing Employee; *A* |
| **KnowledgeItemEdited** | User edits an approved item | item, previous and new version | Marketing Employee; *A* |
| **KnowledgeItemRetired** | User "deletes" or supersedes an item (01 BG-3) | item, reason | Marketing Employee (stop using it); *A* |
| **RelationshipAdded / RelationshipRetired** | User draws or removes a link, or an approved Candidate brings one | from, to, type | *A* |
| **KnowledgeConflictDetected** | New knowledge contradicts approved knowledge | items involved | *N* ("which is correct?", PRD §13.4) |
| **ProceduralRuleApproved** | Review approves a Procedural Rule | rule, scope | Marketing Employee (applies to future generation); *A* |

## 4. Marketing Employee

| Event | When | Essential data | Consumers |
|---|---|---|---|
| **TaskRequested** | User sends a message that needs work, or Router classifies it complex | Task, request, Conversation | Canvas (acknowledgement ≤ 2 s, 01 DL-3) |
| **TaskStarted** | Capacity available (≤ 2 running per Workspace) | Task, frozen configuration | — |
| **ConstraintDiagnosed** | Orchestrator settles the commercial bottleneck | discovery / positioning / conversion | Task record |
| **ClarificationRequested** | Missing information found (PRD §9.3) | form, expires-at (+7 days) | Conversation (form message) |
| **ClarificationAnswered** | User submits the form (any field may be "Search Yourself") | answers | Task resumes; Business Brain (answers about the Business → Candidates, 01 BG-2) |
| **ClarificationExpired** | 7 days unanswered | Task | Task cancelled; *N* |
| **TaskCompleted** | All steps done | Task, Deliverables | Deliverables; Business Brain (Knowledge Extraction → Candidates); *N* (follow-up suggestions, PRD §8.2) |
| **TaskPartiallyCompleted** | Time or cost limit reached (01 SC-10) | what's missing | Deliverables (labelled partial); *N* |
| **TaskFailed** | Retries and alternatives exhausted (PRD §13.2) | user-facing explanation | *N* (transparent error) |
| **TaskCancelled** | User cancels or Clarification expires | reason | — |
| **ResourceConsumed** | Any model call, crawl, image or post with a cost | resource kind, quantity, cost | *U* (Usage Record, 01 SC-3) |

## 5. Deliverables

| Event | When | Essential data | Consumers |
|---|---|---|---|
| **DeliverableCreated** | Task output is finalized | type, Task, Source Citations | Conversation (Deliverable card); Governed Publishing (offers its posts for Batch Review, as drafts); Business Brain (reusable facts → Candidates) |
| **PerformanceReportRejected** | Uploaded file fails validation | reason (unrecognized columns) | Conversation (clear error, FR-095) |

## 6. Governed Publishing

| Event | When | Essential data | Consumers |
|---|---|---|---|
| **PostDrafted** | Employee proposes a Post inside a Content Package or Content Calendar | Post, first Post Version, target Platform | — |
| **PostSubmittedForApproval** | User sends Posts to Batch Review (01 PB-10) | Posts | Channel Connections (lazy connection prompt if no Connected Account, PB-11) |
| **PostVersionCreated** | User edits, or Employee regenerates with feedback | Post, new Post Version, author | Approval cleared → pending approval; Business Brain (User edit diffs → Procedural Rule Candidates, PRD §10.2); *A* |
| **PostApproved** | User approves one or many ("approve all remaining") | Post, approved Post Version | *A* |
| **PostRejected** | User rejects | Post | *A* |
| **PostScheduled** | Approved Post has its release time | Scheduled Time (instant + zone) | — |
| **PostCancelled** | User cancels before publishing (01 PB-6) | Post | *A* |
| **PostExpired** | Pending when its Scheduled Time passed (01 PB-7) | Post | *N* |
| **PostPublished** | Publish Attempt succeeded **and** Read-back Verification confirmed | platform post id, URL | *A*; *N* (per-post result); *U* (X post cost) |
| **PostPublishFailed** | Retries exhausted, Platform rejection, or Reconnect Needed | reason (platform message) | *A*; *N* (per-post status + suggested fix) |

## 7. Channel Connections

| Event | When | Essential data | Consumers |
|---|---|---|---|
| **AccountConnected** | User completes authorization for a Platform | Platform, account | Governed Publishing (pending connection resolved); *A* |
| **AccountReconnectNeeded** | Silent renewal fails (01 PB-9) | Platform, account | Governed Publishing (attempts fail with "reconnect needed"); *N*; *A* |
| **AccountDisconnected** | User disconnects (PRD §12.3) | Platform, account | Governed Publishing; *A* |

## 8. Evaluation (internal)

| Event | When | Essential data | Consumers |
|---|---|---|---|
| **ScenarioRun** | A Scenario executes in Treatment or Control | configuration, Deliverable, clarification and correction counts | Evaluation Round |
| **DeliverableScored** | A Reviewer scores one Deliverable | rubric scores | Evaluation Round (tie-break if two scores differ by more than 1 point) |

## 9. Canvas progress signals (not domain events)

Live, ephemeral signals produced by the Marketing Employee while a Task runs. They are replayable only during the run plus a short grace window (01 RT-1). They are never used as a record. Their names and fields come from PRD FR-045:

`agent_started {agent_id, agent_name, task_summary}`, `agent_tool_used {agent_id, tool_label}`, `agent_completed {agent_id, result_summary}`, `agent_error {agent_id, error_summary}`, `flow_connection {from_agent, to_agent, direction}`.

## 10. Key event chains

1. **Onboarding:** WorkspaceCreated → OnboardingCrawlStarted → OnboardingCrawlCompleted → BrandKitDetected + KnowledgeCandidateProposed ×N → (Review) KnowledgeItemApproved / KnowledgeCandidateRejected.
2. **Task with clarification:** TaskRequested → TaskStarted → ConstraintDiagnosed → ClarificationRequested → ClarificationAnswered → ResearchCrawlCompleted → TaskCompleted → DeliverableCreated → KnowledgeCandidateProposed.
3. **Publish:** DeliverableCreated → PostDrafted → PostSubmittedForApproval → (AccountConnected) → PostApproved → PostScheduled → PostPublished **or** PostPublishFailed → (manual retry) → PostPublished.
4. **Learning loop:** PostVersionCreated (User edit) → KnowledgeCandidateProposed (Procedural Rule) → ProceduralRuleApproved → future Post Versions follow it.
