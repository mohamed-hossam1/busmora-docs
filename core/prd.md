# BUSMORA — Product Requirements Document

**Version:** 2.0 — approved product redesign\
**Date:** 30 October 2026\
**Product:** BUSMORA, an AI Marketing Director for Arabic-first business owners\
**Development planning period:** Approximately 24 weeks of development by five students using AI-assisted software development\
**Revision basis:** owner-approved redesign supplied 30 October 2026; this supersedes conflicting v1.1 scope.\
**Status:** Product and release requirements; not evidence of implemented capabilities or validated market demand

## 1. Purpose and decision status

This PRD defines BUSMORA's initial customer focus, owner workflows, capabilities committed to its first production-ready release (Phase 2), and later expansion. It specifies user-facing behavior, approval boundaries, evidence standards, and release acceptance. Detailed schemas, model/provider choices, frameworks, infrastructure, and platform API configuration belong in supporting technical designs.

The requirements marked **Phase 2** are binding release commitments in this revision. The approved redesign supersedes conflicting v1.1 requirements. Phase 3, Phase 4+ and Phase 5 remain future work with no assumed capacity in the approximately 24-week plan. An interface or design is not evidence of operational capability.

BUSMORA is a graduation project with four simultaneous goals: a polished and reliable product, meaningful technical innovation, real value for business owners, and commercial potential. Paid pilots and a fixed number of external testers are not graduation prerequisites. Continuous evaluation is required. Functional readiness, eligibility for use by ordinary business owners, and commercial validation are separate judgments; none may be inferred solely from the others.

### 1.1 Requirement terms

- **Must:** required for the stated phase to be accepted.
- **Should:** expected behavior when applicable; an exception requires a documented product or technical reason.
- **Planned:** part of the product vision, with implementation committed to a later phase.
- **Open technical decision:** a solution choice to resolve through architecture and evaluation work without changing the product requirement.
- **Planning assumption/target:** an estimate or evaluation objective, not a measured result. Assumptions must be checked against actual capacity and evidence; release gates cannot be silently weakened to conceal a failure.

### 1.2 Revision and source authority

This PRD is the product/release authority. The explicit owner-approved redesign supersedes v1.1 and conflicting supporting documents. Existing engineering choices remain binding where compatible. The repository filename is `prd.md`; the supplied request's `prd(1).md` refers to this same document, so its existing filename and links are preserved.

Stable FR-01–49 and NFR-01–09 identifiers are retained with the dispositions below. FR-47 and AC-26 are retired, never reassigned. FR-50–58 and AC-31–58 add coverage. AC-01–25, AC-27–58 are active. The original external planning/playbook files are historical rationale, not implicit requirements. Five diagnostic lenses remain useful guidance, not validated scores, mandatory worksheets, or an endorsement.

### 1.3 Revision ledger and cross-document change map

| Decision / requirement disposition | PRD | Design System | Frontend | Backend | System |
|---|---|---|---|---|---|
| FR-01 extended; FR-50 new: Google and verification links | §§4–5 | §10.2 | auth/session | identity, §8.2 | §4.7 |
| FR-03–06, FR-10 rewritten: essential draft intake and confirmation | §§4.1, 5.2 | §10.3 | unified form | identity draft, brain activation | §§3.3, 4.6 |
| FR-16, FR-45–46 rewritten; FR-51–54 new: Discovery and complete steps | §§4.2, 5.4, 5.12 | §§10.4–10.5 | roadmap feature | tasks models/APIs | §§3.4, 3.6–3.7 |
| FR-17–18, FR-22–23 clarified: independent Chat, choice-first forms, local Canvas, retry masking | §§4.2, 5.4–5.5 | §10.6 | employee/canvas | conversation/task linkage | §§5–6 |
| FR-29, FR-41 narrowed; FR-55, FR-57 new: documents vs Posts and calendar | §§5.7–5.10 | §§10.8–10.9 | deliverables/publishing | separate result contracts | §§3–6 |
| FR-02 rewritten; FR-47 and AC-26 retired; FR-58 new | §§5.1, 9, 12 | remove daily/performance screens | remove obsolete routes/modules | retire exclusive APIs/jobs/tables | replace flows and matrices |
| FR-38–40 narrowed: requested analysis with existing parsing/math safety | §5.9 | §10.14 | Chat attachment flow | generic analysis uploads | §3.7 |
| FR-48 retained and strengthened: current knowledge and targeted Post holds | §§5.3, 5.12 | §§10.7, 10.9 | invalidation/reapproval | dependency events/preflight | §§3.3, 4.5 |
| FR-43–44 retained; FR-56 new: Settings and contextual notifications | §§5.11, 5.14 | §§10.10–10.11 | settings/notifications | identity/connections/notifications | §§5.3, 7.5 |
| P5-01 new; P3/P4 retained and clarified | §§3, 6 | §10.15 | unavailable capability states | no active Ads endpoints | §7.9 |
| All safety NFRs retained; acceptance and crosswalk replaced | §§8–14 | §16 | §10 | §10 | Appendix A |

Removal of **Today Dashboard**, **Outcomes & Reviews**, and **Now/Next/Later** is complete in the active product contract. Historical names occur only in explicit retirement and negative acceptance records. No replacement daily dashboard or recurring performance-review workflow is authorized.

## 2. Product vision and boundaries

BUSMORA is an Arabic-first AI Marketing Director with minimal owner-confirmed business setup, persistent Business Brain, independent Chat, authentic conversation-linked Agent Canvas, an adaptive executable step-based Roadmap, document/report Deliverables, and a social-media Scheduling calendar with governed publishing. It helps owners choose and complete useful marketing work using available evidence without requiring marketing terminology or sophisticated prompts.

The Marketing Director understands products, services, customers, pricing, goals, constraints, and available performance information. This broader context informs its work; its recommendations and execution focus on marketing, customer acquisition, go-to-market, and marketing-driven growth. Experienced marketers may use BUSMORA for execution and time savings, but marketing expertise is not a prerequisite.

The core experience is **confirm essential context → choose Chat or Roadmap → perform bounded work → inspect outputs → authorize supported external actions**. The owner may ask for analysis of supplied evidence or a strategic revision at any time. There is no mandatory campaign-measurement phase, periodic results capture, recurring performance review, or automatic performance-triggered plan revision.

The product's differentiation hypothesis is better decisions, less owner effort, and continuity across tasks in a usable Arabic-first experience. Arabic text generation, persistent context, and multiple agents alone do not establish a competitive advantage. BUSMORA must not claim exclusive capabilities, superior marketing results, or regional product-market fit without evidence.

### 2.1 Primary and secondary users

| User | Need | Product response |
|---|---|---|
| Primary launch cohort: Arabic-first owner-operated service business selling through inquiries, consultations, or bookings | Choose feasible work to improve qualified demand, conversion, or follow-up | Explain the likely constraint, prepare useful work, with evidence and uncertainty visible |
| Secondary: owner or marketer with marketing experience | Reduce research, planning, content, coordination, and publishing effort | Direct chat and task execution using the same Business Brain and controls |

The launch focus is a product hypothesis selected for a coherent inquiry-to-customer workflow, not a verified market-size conclusion. Initial evaluation should begin with reachable Egyptian owners and separately identified GCC country cohorts when owner access and suitable native review are available. Discovery selects a service niche; the playbook's website-development studio example does not determine the niche.

The initial cohort should have a deliverable offer, control of its inquiry destination, willingness to review and act on relevant marketing work, and capacity to serve the intended demand. A website is optional. Native-publishing pilots should involve businesses for which Facebook is actually relevant; a connected Page does not establish channel fit.

Other business models may receive supported general analysis and drafts, with their limits explicit. Phase 2 does not promise equally deep e-commerce, SaaS, marketplace, enterprise, or regulated-sector workflows. The country/segment focus does not reduce the full Arabic/English UI or English, Egyptian Arabic, audience-specific Gulf-oriented Arabic, and Modern Standard Arabic (MSA) content commitments. Interface, conversation, and campaign audience may each use different languages or registers.

### 2.2 Primary owner jobs

| Owner job | Required product result |
|---|---|
| What should I do next with my time and budget? | A defensible priority, evidence and uncertainty, and a feasible next action |
| Help me get this work done | A usable artifact, an accurately labeled owner handoff, or verified supported execution |
| Help me understand evidence I provide | A scoped one-off analysis with limitations; any strategic revision remains a separate owner decision |

### 2.3 Product principles

1. **Useful direction from incomplete context.** BUSMORA may offer preliminary recommendations while distinguishing observations, conclusions, and hypotheses.
2. **Owner intent governs work.** It may perform internal steps needed for a requested task. Optional research or work beyond that request requires the owner's choice.
3. **Owner control of consequential actions.** Publishing, outreach, expenditure, live-site changes, and other external effects require explicit approval before execution.
4. **Persistent context with provenance.** Business knowledge, historical work, and preferences remain available without turning every inferred claim into a trusted fact.
5. **Inspectable systems.** The interactive Brain graph explains context and evidence; the authentic Agent Canvas explains actual work, failures, and recovery. Neither is a general visual programming tool.
6. **Accurate representation.** The platform must distinguish available, upcoming, partial, failed, and completed capabilities and actions.
7. **Capacity before volume.** Active work must fit owner time, budget, skills, assets, and fulfillment capacity, or expose the conflict. More posts or more traffic is not the default answer.
8. **Diagnosis before channel selection.** Consider offer/audience, destination, conversion, acquisition, and existing-customer opportunities. A missing integration must not bias strategic priority toward the only executable channel.
9. **Completion is not impact.** Draft completion, owner-reported implementation, verified publication, measured outcomes, and causal explanations are distinct. Unknown data is not zero; an inconclusive analysis is valid.

## 3. Phase plan and release contract

| Phase | Product outcome | Included capabilities |
|---|---|---|
| **Phase 1 — Foundation** | Establish the necessary platform, data, integration, and operational foundations for Phase 2 | Account and single-owner workspace foundation; isolation; task and deliverable persistence; knowledge and approval states; event and audit records; secure integration and background-work foundations; evaluation and observability foundations |
| **Phase 2 — First production-ready release** | Confirm essential context and independently use Chat, Roadmap, documents and governed social publishing | Email/password verification links and Google; unified website/manual draft confirmation; Brain Graph/List; choice-first Director Chat; bounded research/Tasks and conversation-local Canvas; resumable adaptive Roadmap Discovery and complete actionable steps; owner completion, Skip/Revisit and approved revisions; document Deliverables; first-class Posts; Scheduling Month/Week/List and Unscheduled; real Facebook Page publishing and recovery; notifications; Settings, AR/EN and four content registers; scoped requested CSV/.xlsx analysis |
| **Phase 3 — Product expansion** | Broaden inputs, publishing reach, and automatic measurement | Operational social-profile-link onboarding; independently activated Instagram Business and X publishing integrations; connected-platform analytics where authorized and available |
| **Phase 4+ — Further expansion** | Extend creation and commercialization | Native AI image generation and later billing, subject to separate design and evaluation |
| **Phase 5 — Meta Ads Management** | Authorized in-product ad campaign management | Future eligible Meta Business/Ad Account connection, campaign/creative setup, explicit budgets/spend authorization, launch, supported metrics and controlled optimization; API/access validation required |

The roughly 24-week schedule and AI-assisted development do not automatically remove agreed capabilities. A handoff can complete a task designed for owner execution, but cannot replace the mandatory native Facebook integration, graph, Canvas, research, or another committed capability. Development milestones (§13) are distinct from customer roadmap stages; no business is forced through a universal 12-week growth program.

### 3.1 Phase 2 completion standard

A committed capability counts as complete only when its main workflow operates end to end with real data or genuine connected accounts as appropriate; integrates with the rest of BUSMORA; reports true execution state; handles visible failure and recovery; and passes relevant functional, security, integration, reliability, and end-to-end verification. Mocked operations, simulated agent activity, and interface-only demonstrations cannot establish completion.

Production readiness includes monitoring, access controls, data protection, error handling, recovery, bilingual usability, and the acceptance gates in §§9–10. Detailed test cases and task-specific service/resource targets must be documented and verified at the declared release load. No paid customer or fixed external-tester count is required.

Development/test-account publishing proves only the access path exercised. Phase 2 must also establish that an intended ordinary owner can connect and publish using the production app configuration without relying solely on a developer-role account. Exact platform requirements must be verified for that configuration. Unresolved production access blocks claiming the intended Phase 2 release is complete.

### 3.2 Bounded Phase 2 capability matrix

| Surface | Committed release behavior | Boundary |
|---|---|---|
| Diagnosis and tasks | Adaptive Discovery, complete actionable steps, a relevant constraint hypothesis and a small curated recipe set | No universal business audit, numeric health score, or mandatory worksheet sequence |
| Research | Dynamic search/retrieval and synthesis across distinct relevant public source types, including accessible websites and review/community material | No guarantee of every site, private-group access, authenticated social harvesting, paid databases, or comprehensive ad-library coverage |
| Creation | Editable text, campaign/offer/page-copy briefs, response/interview guides, visual briefs, and owner media | No native image/video generation, design editor, live-site editing, or message sending |
| Facebook publishing | Text/link posts and a single owner-supplied image with caption, with approval, scheduling, verification, and recovery | Verify API support early; no implicit video, story, reel, carousel, or every-format promise. A format change requires a documented contract decision |
| Requested analysis | Explicit Chat task using documented CSV and Excel (.xlsx) templates, recognized-field mapping and deterministic supported metrics | No arbitrary workbook interpretation, live analytics, CRM, cohort/LTV forecast, or multi-touch attribution |
| Brain and Canvas | Interactive typed entity/relationship graph and actual task execution visualization, with accessible detail/list views | No ontology designer, unrestricted graph workspace, or workflow builder |
| Calendar and export | Month/Week/List calendar and Unscheduled Posts; text/Markdown/CSV and applicable original media exports | No enterprise calendar or document-design suite |
| Unsupported external work | Clear owner-ready draft/instructions, responsibility, and owner-reported completion | Never represented as executed or independently verified by BUSMORA |

Supported limits, blocked states, and later-phase capabilities must be visible at the point of use. Failures outside supported limits require a useful explanation and fallback, not simulated success.

## 4. Core user journeys

### 4.1 Authenticate and confirm essential context

Register using email/password only, verify a secure email link, then enter Onboarding; or use secure Google signup/login. Registration does not ask for business facts. Onboarding presents Import from Website, Manual Setup, and disabled Import from Social Media — Coming Soon — Phase 3.

Website URL → permitted crawl → structured extraction → validation → editable draft form → owner confirmation → active Brain. Manual Setup uses the **same form and schema**. Crawl observations, extracted proposals, owner-confirmed values and activated records are distinct. Partial results survive and manual completion is immediately available. Confirmation covers the displayed draft version once; it does not create a second Pending Brain Updates queue.

Require name, description, at least one offering, country/service area, and a contact/conversion route state (a supported route or explicitly not ready). Pricing is optional and supports fixed, starting-from, quote-required and unknown values. Strategy, audience research, competitors, past campaigns, baseline, budget and weekly owner hours are not onboarding requirements.

After confirmation offer **Chat with Marketing Director** and **Create My Roadmap** prominently; Brain remains directly accessible. Useful first output within ten minutes of active owner interaction remains a usability target, with background latency measured separately, not a claimed result.

### 4.2 Independent Chat and Roadmap Discovery

Direct Chat reuses current eligible context and clarifies only material gaps, with three or four meaningful choices plus Other / Custom Answer where suitable. Quick acknowledgments do not fabricate specialist work. Substantive requests create authorized durable Tasks. Conversation → its Tasks → selected Task Canvas is the only primary Canvas relationship; switching conversations leaves other work running.

Create My Roadmap opens a saved, resumable Discovery draft. The first significant question selects the objective (first customer, more customers, better inquiry conversion, offering launch, existing-customer revenue, or custom). Questions adapt to the objective and known context. Unknown, Skip and Not Applicable remain distinct; explain indispensable questions. Research with Director saves the draft, creates a durable question-linked conversation/task, and returns through **Use in Roadmap** to that exact question for selected-answer confirmation. Direct Chat and step execution do not show that action.

Generation produces a complete proposed plan with actionable steps, evidence, uncertainty, outputs and real dependencies. Initial approval activates one Roadmap; saved drafts and archived versions remain inspectable. Approval of a plan grants no publishing or spending permission.

### 4.3 Execute, skip, revisit and revise

The full vertical plan is visible. Recommended sequence is not a universal blocker. Run with Director validates prerequisites, task/resource authorization and the hard limit of two running Tasks per workspace, creates an idempotently linked Task and opens its conversation/Canvas. Waiting for capacity is shown truthfully. Reopening a step never reruns work.

Unsupported external actions receive instructions, artifacts and a checklist. Mark as Done records **Owner-reported** completion and optional evidence. Skip records owner intent/time and dependency impact, never completion or cancellation; Revisit recomputes eligibility. Skipping running work does not stop its Task or undo a platform action. Discuss or Revise Roadmap produces an added/modified/removed-or-superseded/dependency diff. Material revisions apply only after exact-version owner approval; rejection preserves the active plan.

### 4.4 Create documents and Posts, then publish safely

Research/strategy outputs create versioned documents in Deliverables. Social copy creates Publishing-owned Posts in Scheduling. A mixed campaign has documents and independent Posts linked by campaign/task/conversation/step references, without duplicate authoritative Post content. Manual New Post works without a connection or publishing date; no date means Unscheduled.

Scheduling defaults to Month on desktop, with Week/List and Unscheduled. The editor shows content, media, Page, date/time, planning zone and lateness allowance. Draft save is not approval. Individual/bulk approval binds exact versions and parameters. Any approval-critical edit or reschedule requires fresh approval. Publication requires current authority, valid window, no unresolved holds and platform evidence. Uncertain acceptance is reconciled, never blindly resent. Historical published Posts use actual publication dates.

### 4.5 Return, correct and analyze on request

Return to the last valid accessible page, otherwise Marketing Director; preserve authorized deep links. Focused notifications open exact forms, documents, Posts, revisions, Brain proposals or connection settings. Read state is not subject approval.

Editing a current price updates future context, preserves history, flags only affected work and holds affected scheduled Posts. Resolve explicitly and reapprove changed details. On an explicit request, supported CSV/.xlsx analysis produces a one-off document with deterministic calculations and limitations; it creates no monitoring cadence, dashboard, review reminders or automatic roadmap revision.

## 5. Functional requirements — Phase 2

### 5.1 Accounts, workspaces, and navigation

**FR-01. Accounts and workspace.** The owner can register/login/logout, verify email, reset a local password and access one primary-owner Workspace. Isolate every workspace’s knowledge, credentials, files, Tasks and approvals. FR-50 defines secure Google identity and email-link flows; Google-only accounts have no assumed password.

**FR-02. Navigation.** Primary areas are Marketing Director, Roadmap, Business Brain, Deliverables, Scheduling and Settings, with a Notification Bell in the top bar. Auth/public pages and initial Onboarding are distinct. First confirmed setup offers Chat or Roadmap; returning login restores the last valid accessible page or Marketing Director and honors authorized deep links. Mobile offers all critical actions. No daily dashboard, team invitations, shared approvals or multi-user administration is included.

### 5.2 Onboarding and initial business context

**FR-03. Website import.** The owner can submit a business website. BUSMORA must access permitted content, extract useful business and marketing context, including available brand messaging and visual identity cues, and indicate real progress, completion, partial completion, or failure. Page selection must prioritize relevant business content; login/signup and other low-value pages are excluded by default unless relevant. The import must expose meaningful source gaps. An inaccessible or sparse site must promptly offer guided setup and must not block useful work. Extracted text alone cannot establish page speed, visual quality, mobile functionality, or successful form submission.

**FR-04. Confirmed intake.** Crawling stores raw observations and structured extraction separately from proposed onboarding field values. Neither is active Brain knowledge. The owner inspects/edits the unified validated draft and confirms its exact version before activation. Persist sources, dates, extraction status and owner decision separately; confirmation is not independent truth verification. No arbitrary confidence percentages or duplicate initial proposal-review queue.

**FR-05. Completion choice.** After successful confirmation offer Chat with Marketing Director and Create My Roadmap. Brain stays directly accessible. Chat does not require a Roadmap or strategic interview.

**FR-06. Unified essential business form.** Both active intake paths use the same editable persisted schema: business name, description, one or more products/services, country and relevant market/service area, and conversion/contact route state. Accept website, WhatsApp, telephone, social page, booking, marketplace or supported other route, or explicit “I do not have a contact/conversion route ready yet.” Each offering has name/description, optional category/details, pricing model fixed/starting_from/quote_required/unknown, optional decimal amount, and currency if amount is supplied. Never infer missing prices as zero or require numeric pricing. Optional logo, colors, links, tone, languages, timezone/currency are progressive. No goal, audience/competitor discovery, prior results, baseline, budget, weekly owner hours or detailed strategy gate belongs here.

**FR-07. Social import visibility.** The onboarding interface must display Social Profile Import as clearly unavailable and **Coming Soon** in Phase 2. It must not begin a simulated import or imply access to private profile information. Its actual operation is a Phase 3 requirement.

### 5.3 Business Brain and interactive graph

**FR-08. Structured memory.** The Brain must represent business identity, model/stage, offerings, audience/market, objectives, constraints/capacity, conversion path, positioning, brand voice/visual identity, channels, competitors/alternatives, research, owner preferences, and decisions. It must also relate claims to proof, goals to metric definitions, and tasks/artifacts to the context versions they used. Business knowledge and Director working preferences remain distinct. The graph and other views must use the same stored records; storage/retrieval choices remain technical decisions.

**FR-09. Epistemic and lifecycle state.** The system must distinguish source observations, owner-provided claims, AI hypotheses/interpretations, and owner-confirmed decisions/preferences. Source type, verification status, approval, confidence rationale, and current/superseded status are separate dimensions. Conflicts, unknown values, and non-applicability must be explicit. Approving an item for persistent use does not independently verify its truth; neither a citation nor prominence on a website establishes the underlying claim or revenue contribution.

**FR-10. Update rules.** Initial onboarding knowledge activates only after FR-04 confirmation. Later explicit owner edits update current values with owner provenance. AI-proposed durable facts, interpretations and preferences enter Pending Brain Updates for individual approve, edit-and-approve or reject. Reimports and research reveal material conflicts rather than overwrite confirmed decisions. Every material edit creates a historical version and triggers FR-48; saving reports is not knowledge adoption.

**FR-11. Historical work.** Conversations, tasks, decisions, research, campaigns, and deliverables must remain retrievable and reusable as context when relevant. Saving a deliverable does not promote all its statements into approved persistent business knowledge. Personalization should improve through approved knowledge, owner corrections, and relevant prior work; it does not require retraining the underlying model after each interaction.

**FR-12. Interactive graph.** The owner must be able to explore relevant typed entities and relationships, inspect sources/status and related decisions, add or correct business information and relationships, delete information, and act on pending updates. Focused neighborhoods and conventional editing forms are sufficient; an unrestricted graph or ontology designer is not required. The graph must communicate uncertainty. An accessible entity/detail list supplements the interactive graph and does not replace it.

**FR-13. Owner data controls.** The owner must be able to inspect, edit, export, and delete Brain information and historical deliverables, and disconnect linked accounts. Changes and approvals must be recorded under the documented retention policy so the product can explain what changed and why. Deleted information must stop participating in retrieval, including derived records; related artifacts must be removed, redacted, or lose their evidentiary basis as appropriate. Disconnection prevents future external sends and is not reversed implicitly by a retry.

### 5.4 Marketing Director, diagnosis, and roadmap

**FR-14. Business-aware diagnosis.** The Director must assess relevant offer/audience, destination, conversion, acquisition, and existing-customer opportunities against the owner's goal and actual conversion path. Its compact diagnosis must state the primary constraint hypothesis, supporting observations with sources/dates, material alternatives and disconfirming evidence where available, the missing evidence most likely to change the choice, and a recommended next action with reasons to defer alternatives. It must separate observations, conclusions, and hypotheses. Sparse data permits a preliminary direction or a useful evidence-gathering task, not invented specificity. Do not require a full audit before a scoped request.

**FR-15. Marketing scope.** Recommendations and execution should remain within marketing, customer acquisition, go-to-market, and marketing-driven growth. Broader business facts can inform a recommendation without making BUSMORA a general business manager.

**FR-16. Complete actionable Roadmap.** Generate a personalized full step-based plan from separate Discovery (FR-51), current Brain and evidence. Show all steps in a vertical responsive view with optional plan-specific sections. Order is recommended; only real dependencies block eligible execution. Each proposed initial plan and material strategic revision is reviewed before activation. One active plan, saved drafts and archived history follow FR-54. No planning-column task limits: the independent runtime limit is two running Tasks per Workspace.

Prioritization must first identify critical observed failures and enforce authorization, capability, budget, dependency, and fulfillment constraints. It then favors the best-supported constraint on the goal; if uncertainty could change the decision, a cheap discriminating question or research task may take priority. Rank feasible alternatives by effort and time to useful feedback. Do not use the playbook's multiplicative priority index, aggregate maturity score, or mandatory universal sequence. The correct recommendation may be no new acquisition campaign.

**FR-17. Direct Chat.** Owners can ask, discuss, clarify or launch supported work without any Roadmap. Direct Chat, Wizard question research and existing-step execution have distinct validated origins. All reuse eligible Brain and history; only Wizard-linked research exposes Use in Roadmap (FR-52).

**FR-18. Choice-first clarification.** Ask only material missing questions; use current Brain facts for three or four meaningful options plus Other / Custom Answer. Radio for single choice, checkboxes for multiple, dropdowns when useful; text/number/link inputs when real values are required. Allow Unknown, Skip and Not Applicable where legitimate. Never fabricate choices. Optional expanded research needs bounded owner authorization; declining preserves a feasible result with explicit limitations.

### 5.5 Router, orchestration, agents, and Agent Canvas

**FR-19. Fast Router.** Incoming conversational interactions must pass through a fast, cost-conscious AI routing step. It may answer simple conversation directly and must send substantive tasks to the Orchestrator. Navigation, deterministic approval actions, and ordinary UI controls do not require an AI call. The chosen route must not fabricate agent work; model selection remains open and routing complexity must be justified by evaluation.

**FR-20. Dynamic Orchestrator.** The Orchestrator must interpret the objective, load relevant context, plan work, select tools/specialists, execute suitable work directly, adapt as evidence arrives, consolidate results, and report outcomes. Runs retain workspace/task identity, context snapshot, objective, inputs, permitted tools, evidence/output contract, review state, proposed knowledge updates, and iteration/time/usage/concurrency ceilings. Child work inherits the parent allowance. The system must support genuinely context-dependent plans, including selecting, skipping, or repeating authorized steps; no fixed sequence is required for every task. Unrestricted tool invention, recursive spawning, and arbitrary executable workflows are excluded. Authorization, budget checks, validated state transitions, and external execution must be enforced outside model discretion.

**FR-21. Specialist capabilities.** Phase 2 must support research, marketing analysis, campaign planning/copy, publishing, and requested data interpretation. The exact agent count, module boundaries, and model assignments remain technical choices; eleven playbook roles do not imply eleven independent agents. The Canvas must reflect whichever agents actually run and distinguish tool/service calls from agent work. Publishing uses a controlled execution service, not discretionary agent authority. Multiple specialists are used when the task justifies them, not to fill a visual sequence.

**FR-22. Authentic conversation-local Canvas.** Conversation → related Tasks → Canvas for selected Task. Default to the latest relevant active Task; multi-Task selection stays in that conversation’s cards/history, never a workspace-wide selector. Show only recorded agent/tool/service/workflow nodes, connections, evidence and safe details, plus an accessible chronological list. Planned/Active/Awaiting Input/Held/Completed/Error/Cancelled are real presentation states. No private reasoning, secrets or invented percentages. Answer in Chat opens the exact pending form.

**FR-23. Durable work and retry presentation.** Tasks survive navigation, browser closure and ordinary worker interruption. Recoverable internal failures stay Running / Processing while authorized recovery remains; omit attempt counters and transient failure incidents from normal Canvas. Retain diagnostics internally. Exhausted recovery yields truthful terminal failure or partial completion and available actions. Operation-specific retry policies apply; uncertain publication never follows generic retries. Cancellation stops future cancellable work without undoing accepted external effects.

### 5.6 Research Agent and evidence-backed intelligence

**FR-24. Dynamic multi-source research.** Research is a core Phase 2 capability. The agent must interpret the question, form/revise a plan, dynamically choose relevant available sources, examine distinct source types when justified, and identify gaps. The initial supported scope is public search and website retrieval, including accessible competitor, review/community, and relevant market material. Public social/ad-library material may be used when accessible, but comprehensive coverage or private/authenticated access is not promised. No fixed source list or sequence applies to every task. Stop when evidence supports the decision, further retrieval is repetitive, useful access is exhausted, or authorized time/usage is exhausted; explain partial results and remaining questions.

**FR-25. Research output.** Supported tasks include competitor analysis, audience/customer pain discovery, market opportunity research, positioning/messaging analysis, and strategic recommendations. Outputs must connect the question, evidence, limitations/contradictions, business implications, options, and recommended next task. Customer-language findings must retain original wording, source/date, market context, and separately labeled translation where used. Public comments are sampled observations, not representative survey results. Research must inform a decision rather than merely list results or maximize citations.

**FR-26. Evidence quality.** Material findings must retain inspectable source references, relevant supporting context, and access/publication dates where available. A citation must support its associated claim; source existence alone is insufficient. Compare sources, disclose stale/inaccessible evidence and consequential contradictions, and distinguish duplicated material from independent corroboration. Never invent citations, exclusive access, interviews, reviews, audience proportions, search volumes, competitor economics, or activities. Observations, owner claims, interpretations, and hypotheses stay separate. Source failures require an honest alternative or partial result, and regional coverage gaps must be explicit.

**FR-27. Per-task research budget.** Each research task must have an authorized budget covering applicable time, model, tool, and paid-source resources. Its initial allowance may reflect task complexity within the owner's authorized usage limits. The agent may reallocate work within that budget. **Any increase requires explicit Owner approval before further consumption.** The request must explain the additional resources, likely time or cost effect, and expected benefit. A refusal must result in work within the original budget or the strongest supported partial result; agents must not evade the limit by switching tools or creating equivalent tasks. Approval of computational usage must not imply consent to undisclosed customer charges.

All delegated work and retries share the parent budget. Independent controls must reserve/account for concurrent consumption before calls where necessary. A provider that cannot be adequately bounded must not be exposed to autonomous execution. The owner must be able to inspect the authorized allowance, consumption, and any requested extension; refusal does not erase useful partial work.

**FR-28. Modular research capability.** Research planning, retrieval, evidence extraction, claim/source association, synthesis, and budget accounting should have separable interfaces, with BUSMORA-specific interpretation layered on them. Provider/model/tool choices require an early evidence-backed architecture and evaluation investigation, including quality, regional coverage, latency, cost, and stopping behavior. Reusability does not commit Phase 2 to a research SaaS, marketplace, public SDK, or custom agent framework.

### 5.7 Campaigns, content, and brand voice

**FR-29. Flexible output size.** BUSMORA must respond to the requested scope. A single-post request must not force a campaign. Campaign packages may contain objective, audience, strategy/hypothesis, messages, channels, duration/schedule, channel-specific copy, CTA, relevant hashtags, visual briefs/owner media, approval state, and success context where relevant. Link packages to their task and goal. Offer/page-copy briefs, interview guides, response drafts, and other owner-ready text use the same artifact/editor/export path, not separate applications. A planned channel does not imply a publishing integration.

**FR-30. Brand understanding and claim integrity.** Accessible website content is the primary committed import source for tone, messaging, positioning, and communication patterns; guided owner context also supports no-website businesses. Distinguish observed wording from inferred traits. A compact profile includes audience, tone, vocabulary to use/avoid, offer facts, prohibited/unsupported claims, and useful owner examples. Content supplied in chat can inform the task; persistent AI interpretations follow FR-10. A separate historical-content import system is not required.

Material claims such as quantified results, testimonials/customer logos, offer availability, deadlines, discounts, and comparative superiority must have an inspectable basis. Missing support requires omission, qualification, or a targeted owner question. A generated differentiator is a hypothesis, not a confirmed business capability. Approval of copy does not verify facts or supply permission to use a customer's identity or testimonial.

**FR-31. Text-first assets.** Phase 2 must generate editable text and visual briefs and support owner-provided media for publishing where needed. It must not mark a platform-specific post publish-ready if required media is missing or invalid. Native AI image generation is a planned future capability, not a Phase 2 feature.

**FR-32. Language and audience fit.** The owner can specify country/market, content language/register, audience, and brand direction for each task. BUSMORA must adapt across English, Egyptian Arabic, Gulf-oriented Arabic, and MSA; Gulf variation follows the named audience and country rather than one generic persona. Task instructions may override stored defaults. Regional/seasonal references and business claims must be supplied or checked, not guessed. Naturalness, regional fit, truthful wording, and brand consistency require suitable human evaluation for all four registers.

### 5.8 Publishing, scheduling, and approval

**FR-33. Connected Facebook Page.** Phase 2 must let an authorized intended owner connect a Facebook Page and perform genuine publishing for the formats in §3.2. Development/test Pages support verification but do not alone meet the ordinary-owner distribution gate (§3.1, AC-30). The connector must expose authorization and supported-format/precondition failures before approval where detectable. Instagram Business and X remain independently activated Phase 3 integrations. Connection alone does not authorize a post, account analytics, or another external action.

**FR-34. Review and approval.** The owner must see exact text, attached media, destination account, scheduled date/time, timezone, and the disclosed maximum delivery-lateness allowance. They can edit, approve selected posts, or use **Approve All** for the displayed set. Approval binds to each exact version and execution details. A change to content, media, destination, time/timezone, or delivery allowance invalidates the affected approval. The same clear preview must be usable on mobile; unsupported/missing media must not be marked publish-ready.

**FR-35. Scheduled execution.** Unchanged approved posts may publish without a second approval within the approved schedule semantics. Immediately before dispatch, the worker must recheck version/destination, current authorization, cancellation, and context-review holds. Owners can cancel undispatched work or modify and reapprove it. A cancellation cannot promise to reverse a request already accepted by the platform; show actual state and any necessary owner removal step. Disconnection blocks future sends; reconnection must not silently send overdue posts. After the disclosed lateness window, hold the post for a new time and approval. Platform-side delay must be reported accurately; the system cannot guarantee control of the platform's processing time.

**FR-36. Verification and recovery.** Publishing must use authorized access and platform preconditions, prevent duplicate external sends during retry/recovery, and preserve provider identities and available read-back evidence. Distinguish submitted, platform-confirmed, failed, uncertain, cancelled, and held states. A timeout after dispatch is uncertain until reconciled; if publication cannot be determined, hold for review instead of blindly retrying. A local idempotency key alone does not establish exactly-once external execution. Automated retries must not alter approved details or exceed the delivery window. Confirm publication only with adequate platform evidence and show actionable recovery for revoked tokens, failed verification, and unresolved outcomes.

**FR-37. Other external actions.** Sending outreach, spending money, modifying a live website, and any other externally consequential operation require separate explicit owner approval if a future capability enables them. These action classes are not implicitly authorized by approving research, a roadmap, or a campaign draft. Paid ad management and live website modification are not Phase 2 execution commitments.

### 5.9 Explicitly requested one-off analysis

**FR-38. Requested file analysis.** An explicit Chat request may attach CSV or supported Excel (.xlsx) using documented templates and recognized-field mapping. Preview metric definitions, period, granularity, units/currency and identifiers before confirmation. Flag duplicate/overlapping/incompatible/ambiguous data; unknown is not zero. Parse without executing macros/formulas; deterministic code computes supported metrics. Unsupported layouts offer a template or corrected owner input. The result is a one-off analytical Deliverable, with no recurring product workflow.

**FR-39. Goal-based interpretation.** The Director must compare available observations with the task/campaign goal, its baseline status, and period, then identify patterns, limitations, and recommendations. Keep business outcomes, funnel conversions, and channel/operating signals distinct. Supported measures may include reach, impressions, engagement, clicks, inquiries/leads, qualified inquiries, bookings/customers, costs, and financial ratios only when inputs and attribution support them. Rates require defined denominators; zero denominators and unknown inputs must not produce fabricated results. ROAS requires attributed revenue and ad spend; ROI requires a stated profit/cost basis. Application code calculates; the model explains. Engagement, completed tasks, and publication do not establish sales, profit, or causation. No forecast, LTV model, cohort engine, or causal-attribution platform is committed.

**FR-40. Evidence governance.** Requested analysis preserves sourced observations and limitations separately from interpretations. Durable AI interpretations enter Pending Brain Updates. A roadmap change requires separate owner direction and explicit revision approval; analysis completion cannot start monitoring, schedule recurring reviews, or change strategy automatically.

### 5.10 Deliverables and history

**FR-41. Document Deliverables.** Save research reports, analyses, strategies, positioning, campaign planning, briefs, visual/content strategy and other non-Post documents with stable identity and immutable historical versions, task/conversation/step/campaign refs, dates, context, sources, assumptions and limitations. Search/type/date filters and cursor pagination support the library. Edit, Copy, Export, Reuse, Continue with Director and governed Delete preserve lineage. Text/Markdown, suitable CSV and original-media exports remain supported; no Phase 2 PDF/Word design suite. Social Posts live only in Publishing/Scheduling (FR-57).

**FR-42. Context reuse.** The Director must retrieve relevant approved Brain information, current owner-confirmed intake/owner-provided context with their status labels, and prior work without treating historical deliverables as current authority. Deleted records and rejected updates must not re-enter active context through retrieval. Expose time-sensitive dates, limitations, supersession, and dependencies under FR-48. A proposed preference does not become an adopted preference merely because it appears in an old conversation.

### 5.11 Application language and accessibility of information

**FR-43. Full localization and responsive access.** Localize auth, onboarding, Director/Canvas, Wizard/Roadmap/steps, Brain Graph/List/proposals, documents, Scheduling/editor/calendar/Batch Review, Settings, notifications, scoped analysis and all loading/empty/error states in Arabic and English. Preserve keyboard access, RTL/LTR/mixed scripts, mobile actions and accessible graph/event lists. Brain List is the preferred mobile default.

**FR-44. Independent language choices.** Application interface language, conversation language, and marketing-content language must be separately selectable or inferable from the owner's request and context. A user may use an English interface, converse in Egyptian Arabic, and create Gulf-oriented campaign content.

Country, currency, and IANA timezone must be stored explicitly rather than inferred from language. Dates, numbers, currencies, names, and links must remain understandable in mixed-script views. Scheduling must use timezone rules, including seasonal clock changes, rather than a permanent offset for Cairo or another market.

### 5.12 Goal, step and context contracts

**FR-45. Goal and practical constraints.** Roadmap Discovery records owner-selected objective, conversion route and material constraints: assets, permissions, fulfillment/customer-service capacity, relevant skills or business budget. Do not restore a standard weekly-owner-hours question. Reuse eligible known facts. Relevant success measures and optional baseline/targets retain observed/owner_reported/estimated/unknown/not_applicable distinctions, source, period and unit; targets are not forecasts. Business budget, AI resource allowance and external spending authority remain independent.

**FR-46. Executable steps.** Each stable Roadmap step includes title/objective, rationale/evidence/uncertainty, responsible party, execution mode, required inputs, dependencies, expected output, completion condition, estimated effort, task/output refs, work status, owner attestation and external-approval requirements. Work state, platform action state and owner completion are independent. Generated documents do not prove implementation, Post approval does not prove publication, and publication does not prove commercial impact.

Step execution follows FR-53. No mandatory learning state or periodic review date belongs to the step model. Suggested owner checks can appear in task instructions without creating recurring jobs or approval authority.

**FR-47. Retired in v2.0.** The v1.1 aggregate-outcome capture and recurring performance-review requirement is removed, with no replacement under this identifier. Requested analysis retains FR-38–40 safety; Post Batch Review and Brain/revision approvals remain independent features.

**FR-48. Context impact and freshness.** Version decision-critical context and retain dependencies for active tasks/artifacts. Changes to offer, price, audience, service area, or claim support must flag affected work for review; hold conflicting scheduled posts pending owner resolution. Do not silently rewrite approved content, regenerate everything, spend new research resources, trigger external actions, or overwrite historical evidence. If approved publishing details change, FR-34 requires fresh approval; even if they do not change, record the owner's resolution before clearing a context hold. Unrelated work must remain usable. Conflicting/superseded records stay intelligible subject to data controls.

**FR-49. Honest capability and handoff contract.** Recommendations and tasks must distinguish supported analysis/drafting, supported connected execution, owner execution, blocked work, and later-phase capability. Unsupported execution must produce useful instructions/artifacts and a completion checklist when feasible, with an explicit responsible party and unblock conditions. Owner-reported results remain labeled. Never claim external work was completed by BUSMORA merely because a draft, recommendation, or integration screen exists.

### 5.13 Curated task recipes

Use approximately ten to twelve relevant recipes: goal discovery; audience/alternative research; claim-proof inspection; offer/message drafting; destination/CTA observations; inquiry qualification; response drafts; campaign preparation; governed Facebook publishing; requested file analysis; existing-customer/referral drafts. These are configurations, not mandatory steps or new product modules. Verify external measurements or provide an honestly limited owner checklist.

### 5.14 Additional redesign requirements

**FR-50. Authentication.** Email/password registration collects no business details and sends a secure single-use expiring verification link with resend and expiry handling. Preserve reset, logout, session rotation/revocation. Google signup/login uses authorization code OIDC with state, PKCE S256 and nonce; validate issuer, audience, signature, expiry, subject and verified email. Provider identity is issuer + subject, not an email string. Never silently merge matching emails; require sign-in and recent reauthentication of the existing account plus a separate explicit linking intent. Cancellation/failure creates no partial authenticated account. Use the existing session architecture; provider tokens do not become browser session tokens.

**FR-51. Roadmap Discovery.** Persist draft, adaptive question identities/order, answers and status (unanswered/answered/skipped/unknown/not_applicable/researching/confirmed) with provenance and versions. Goal selection drives questions. Reuse currently eligible Brain context and accepted previous decisions, asking again only for a material gap/conflict/staleness. Save and resume across browser closure. An indispensable answer has an explanation; otherwise a preliminary plan exposes uncertainty. Draft editing and generation are separate from initial activation.

**FR-52. Exact-question research.** Save the draft before opening Director. Backend validates Workspace, draft, question, conversation, Task, result and return destination. Research stays within task authority. Only valid active linkage enables Use in Roadmap; owner selects/edits a source-aware result, returns to the exact question, then confirms. Persist accepted decisions/context with provenance for future reuse. Query parameters alone are never linkage or authority; skipped/unknown cannot become positive facts.

**FR-53. Step control.** Run with Director is idempotent, creates linked Task/conversation/output refs, checks dependencies/authority and queues truthfully at two running Tasks. Mark as Done records owner-reported checklist/evidence, never verification. Skip records intent/time and impact; Revisit recomputes readiness. Skipped is not Completed or Cancelled. Skipping running work leaves its Task active and offers a distinct Cancel action. Block dependents whose actual prerequisite is unmet; suggest alternatives/revision, never silently waive a dependency. Progress separates completed, owner-reported, skipped, blocked and inapplicable counts; no inference of achieved business objective.

**FR-54. Plan lifecycle and revision.** Support saved drafts, proposed initial plans, one active plan, versioned proposed revisions and archived historical plans. Initial/revision approval binds displayed proposal and active base version. Activation atomically archives the previous active version/plan under a workspace lock. Rejection keeps active plan intact. Revisions show added/modified/removed-or-superseded steps, dependency changes, rationale, evidence and constraints. In-flight Tasks/results retain original step/version references; no silent restart, cancellation or publishing approval.

**FR-55. Scheduling.** Single home for every Post, with desktop Month default, Week/List alternatives and Unscheduled. Month remains available on mobile. Calendar shows proposed/unapproved times distinctly from authorized scheduled sends, plus held/failed and historical published Posts at their actual dates. New Post accepts draft text/link or validated owner image, optional target/date; connect a Page only when needed. Editor/detail supports versioned edit, schedule/reschedule, timezone, preview and approvals. Drag reschedule has an accessible form alternative and requires fresh approval. Bulk decisions enumerate exact displayed versions and report partial failures. Duplicate published content creates a new Draft; history is never rewritten or falsely removed from Facebook.

**FR-56. Settings and notifications.** Account shows providers and verification; password controls only when applicable. Region stores country, currency and IANA zone independently. Connection status is connected/reconnect_needed/disconnected with consequences explained before disconnect; reconnect never auto-releases overdue sends. In-app notifications include unread/read controls and exact subject links for completion, terminal failure, clarification, document/research ready, Post approval/publication/hold/uncertainty, reconnection, Brain proposal and revision approval. No transient retry notification. Unavailable subjects show an honest unavailable state; read state grants no approval.

**FR-57. Output ownership.** Publishing owns Posts/Post Versions, Campaign grouping, approval, schedule, attempts and outcomes. Deliverables owns non-Post documents and applicable upload/export storage. Task result assembly creates either/both through their public contracts; a Post never requires a document origin. Chat retains eight baseline kinds and adds typed `post_reference` for a Scheduling link, never a disguised Deliverable. Origin refs distinguish direct conversation, Wizard research, roadmap execution and manual creation.

**FR-58. Removed surface exclusion.** No active Today Dashboard, Outcomes & Reviews, replacement daily dashboard, recurring marketing-performance review or Now/Next/Later roadmap model is shipped. Remove exclusive routes, APIs, view models, jobs, notifications and acceptance obligations. Retain legitimate Post Batch Review `ReviewSession` records, knowledge/revision review and requested analysis. Historical records follow safe compatibility/deletion policy, not destructive undocumented migration.

## 6. Future-phase requirements

Later phases retain their own acceptance standards and receive no assumed delivery capacity before the binding Phase 2 release is secure. Order additional integrations by observed owner friction; a read-only measurement source may be more valuable than another publisher. Do not enable a feature merely because its interface is present.

### 6.1 Phase 3 — Social Profile Import

**P3-01.** The owner can provide a supported social page or profile URL to establish or enrich the Brain using accessible public or appropriately authorized business content. One supported link is enough to start. BUSMORA should extract relevant offerings, messages, historical communication patterns, and brand voice where available and ask concise follow-up questions if the source is sparse. A URL alone must not be treated as permission to access private content or analytics. The **Coming Soon** control becomes enabled only when this workflow functions end to end.

### 6.2 Phase 3 — Additional publishing and connected analytics

**P3-02.** Instagram Business and X can each be connected, approved, scheduled, published, verified, monitored, and disconnected independently. Each must meet the same approval, version binding, authorization, idempotency, failure visibility, and release-quality principles as Facebook Pages before being labelled available.

**P3-03.** Where the owner authorizes it and the platform makes metrics available, BUSMORA can retrieve connected-platform performance data and support a separately designed future performance-analysis experience. Direct analytics access is distinct from publishing authorization. It does not feed an active recurring-review subsystem in Phase 2.

### 6.3 Phase 4+ — Creation and commercial expansion

**P4-01.** Native AI image generation may create visual assets from a campaign brief and approved brand context. Its eventual agent or Skill design, provider, evaluation, rights, and cost handling require separate design before commitment. Phase 2 continues to support visual briefs and owner media.

**P4-02.** A later commercial phase may add subscriptions, payment collection, plan-based usage quotas, and billing administration. No payment provider, pricing structure, or mandatory paid pilot is selected by this PRD. Phase 2 still requires internal usage controls and cost monitoring.

### 6.4 Phase 5 — Meta Ads Management

**P5-01. Planned, not implemented.** The owner connects an eligible Meta Business/Ad Account, inspects permissions/constraints, prepares strategy and creatives, configures supported campaigns/targeting, reviews setup, explicitly authorizes budgets/spending, launches through verified APIs, inspects supported metrics and discusses controlled optimization with Director. Routine supported management is intended inside BUSMORA. Account eligibility, production API access/permissions, formats and measurement must be independently verified; none is guaranteed. Meta Pixel / Conversions API or another permitted measurement integration may be needed for conversion metrics. Never invent attribution. Campaign creation approval does not grant unbounded future spend; material changes need appropriate new authorization. This is separate from Phase 2 organic Page publishing and adds no current routes, tools or credentials.

## 7. Approval and knowledge-control matrix

| Activity | Phase 2 rule |
|---|---|
| Internal steps necessary for an explicit owner request | Proceed within task authorization and resource limits; clarify only essential gaps |
| Optional new research or a substantial workflow outside the request | Present the choice and wait for owner authorization |
| Initial website/manual intake | Persist an editable draft; confirm the displayed version before Brain activation; preserve source labels |
| Owner edits to business information | Apply as owner-provided information; version material changes and flag dependencies, without treating the edit as verification or publication approval |
| AI-proposed persistent fact, interpretation, or preference | Hold as Pending Brain Update until owner approves or edits and approves |
| Saving a historical document or requested analysis | Save with source and date; do not automatically promote its interpretations to trusted Brain knowledge |
| Actual task/execution status | Update from recorded events or explicitly labeled owner reports; do not require approval for every internal transition or equate completion with growth |
| Initial Roadmap activation | Review the exact proposed plan and source context before explicit approval; atomically preserve one active plan |
| Proposed strategy/roadmap revision | Show the versioned diff, rationale, dependencies and practical impact; apply only after explicit approval |
| Artifact acceptance | Accept a draft for its task; do not infer approval to publish, spend, send, or change persistent knowledge |
| Research-budget increase | Obtain explicit owner approval before any increase; no automatic extensions |
| Publish or schedule external content | Obtain exact-version approval for text, media, destination, time/timezone, and disclosed delivery allowance; check current authority immediately before dispatch |
| Changed approved publication details | Invalidate the affected approval and require fresh approval; batch approval does not cover later versions |
| Material context change affecting scheduled content | Hold affected work for owner resolution; do not silently rewrite, regenerate, or publish; changed execution details require reapproval |
| Unknown publication outcome or overdue job | Reconcile before retry; hold unresolved or out-of-window work; no blind duplicate or silent late send |
| Account disconnection/reconnection | Stop future sends when disconnected; reconnecting does not authorize overdue work or new actions |
| Owner execution outside BUSMORA | Supply draft/instructions and record attested completion accurately; do not claim execution or independent verification |
| Other external side effects | Require separate explicit approval before execution if the capability exists |

Task authorization, artifact acceptance, knowledge approval, roadmap approval, and external-action approval must remain distinct stored decisions. One screen may group reviewable proposals with clear choices and diffs; a single ambiguous approval must not silently grant all authorities. Model text and retrieved material cannot create or expand authority.

## 8. Quality, security, and operational requirements

**NFR-01. Truthful state.** Brain graph, Canvas, tasks, calendar, and approval information must derive from stored knowledge and actual events. Planned, running, awaiting input/review, completed, failed, held, cancelled, and uncertain states require corresponding evidence. Drafted, owner-implemented, published, and commercially effective are distinct. Do not simulate agents, tool calls, sources, progress percentages, metrics, or customer results.

**NFR-02. Source integrity.** Retain enough source identity, date, and supporting context to inspect consequential claims. Source validity and actual claim support must be evaluated separately. Inaccessibility, staleness, contradiction, uncertain independence, and limited geographic coverage must be visible. Cache reuse must preserve freshness labels. Generated claims and owner approvals must never be substituted for independent evidence.

**NFR-03. Workspace security.** Authentication and authorization must isolate each owner's context, retrieval, files, tasks, credentials, approvals, exports, and execution. Store secrets/tokens securely, restrict tool access to the current workspace, and enforce current authorization in backend services independently of the model or UI. Test tenant isolation, credential handling, access revocation, and approval boundaries. Implementation choices belong in technical design.

**NFR-04. Untrusted input handling.** Pages, research, uploads, and tool results are data, not instructions. They must not authorize tools, knowledge overwrites, budget increases, or disclosure. Validate fetch URLs and guard against internal-network requests and malicious redirects. Do not copy private business data into public searches because a source requests it. Parse spreadsheets without executing macros/formulas; protect exported spreadsheets from formula injection. Validate structured model outputs before persistence or action.

**NFR-05. Durable execution and recovery.** Research, imports, tasks, and scheduled publishing must remain traceable through ordinary navigation and worker interruptions. Preserve approvals and attempt/outcome history; test retries, concurrent workers, cancellation races, revocation, uncertain provider responses, and overdue work. Do not blindly replay external operations after recovery or backup restore. Recovery must revalidate authority and reconcile uncertain outcomes. Backup/restore and deployment/rollback procedures must be exercised before release.

**NFR-06. Resource accountability.** Monitor actual model/tool/search/integration usage per task and workspace, including failed calls, retries, and child work. Enforce limits and concurrency reservations independently of agents; record budget approvals. Declare task-specific latency, timeout, usage, and concurrency ceilings at a stated release load, then measure them (§10). Internal resource authorization is distinct from customer charging. Cost per useful task must include repeated work and relevant service costs, not only successful model calls.

**NFR-07. Accessible bilingual operation.** Critical mobile and keyboard journeys, RTL/LTR/mixed-script layouts, forms/errors, graph/list interactions, Canvas/events, dates, currencies, and approvals must be understandable in the selected language. Verify country/timezone handling, seasonal clock changes, independent language settings, and all four content registers. The owner must be able to identify precisely what will happen, where, and when. English or Egyptian Arabic success does not establish Gulf-oriented quality.

**NFR-08. Observability.** Operators must be able to investigate failures, source gaps, stuck jobs, approval transitions, publishing attempts, revocation, and cost spikes using real records. Provide actionable owner errors and recovery runbooks, with a small internal view/logging path sufficient for the release. Logs and user-facing activity must redact secrets and avoid unnecessary private data or private model reasoning. Operational telemetry must remain separate from claimed customer business performance.

**NFR-09. Data control.** Export, deletion, correction, and disconnection must cover product data, integrations, derived retrieval records, and affected artifacts. Define active-data deletion, backup expiry, sensitive-data handling, token lifecycle, and the minimal retained audit metadata before release. Explain retention limits accurately; do not promise immediate deletion from every backup if unavailable. Restore procedures must respect prior deletion/disconnection decisions. Prefer aggregate outcome data over storing customer contacts.

## 9. Phase 2 acceptance scenarios

The following are mandatory product-level release checks. Detailed fixtures and test cases belong in the evaluation/engineering plans and use the gates in §10. A claimed capability must be exercised against its real implementation, not a visual stand-in. Preserve evidence against each applicable FR/NFR using the crosswalk in §14.

| ID | Scenario | Observable pass condition |
|---|---|---|
| AC-01 | Website onboarding | Crawl prefills a validated editable draft; no Brain eligibility before exact-version owner confirmation; sources and partial fields survive. |
| AC-02 | Manual onboarding | The same form accepts essential fields only, supports a not-ready conversion route and leads to Chat/Roadmap choice without strategy questions. |
| AC-03 | Brain governance | The owner explores and edits the graph, distinguishes imported from confirmed or inferred items, and approves, edits, rejects, exports, or deletes applicable information |
| AC-04 | Diagnosis and Roadmap | Separate Discovery produces a complete actionable proposed plan; evidence, uncertainty, real dependencies and completion conditions are visible; activation/revision require approval. |
| AC-05 | Direct work | The owner launches a supported task from chat without completing preceding roadmap tasks; the task reuses relevant Brain context and history |
| AC-06 | Dynamic orchestration | Simple input avoids fabricated specialist work; complex tasks vary planning/delegation with context inside shared tool/iteration/time/concurrency limits; ordinary UI actions do not require AI routing |
| AC-07 | Authentic Canvas | Only the selected conversation’s actual Task nodes/events appear, with safe detail/list and exact form links; no fictional progress or history. |
| AC-08 | Multi-source research | Research dynamically examines distinct relevant supported source types, verifies claim support, distinguishes duplicated sources, exposes gaps/contradictions, and links implications to an actionable next task; access/stopping limits produce honest partial results |
| AC-09 | Research budget | Parent, child, concurrent and retried work stay within the authorized allowance; attempted increases pause for owner approval; refusal preserves a bounded result without tool/task budget evasion |
| AC-10 | Campaign creation | The owner can request either a single item or an editable text-first campaign, with brand-aware copy, channel details, visual briefs, and owner media when needed |
| AC-11 | Language experience | The complete application works in Arabic RTL and English LTR; separate interface, conversation, and content-language choices work across the agreed registers |
| AC-12 | Facebook publishing | An authorized Page accepts each committed format after exact individual/batch approval; a scheduled job publishes for real, retains platform evidence, and handles retries without duplicate publication; unsupported/missing media is rejected before approval |
| AC-13 | Approval invalidation | Editing approved content, media, destination, time/timezone, or delivery allowance prevents dispatch under the old approval until the new details are approved |
| AC-14 | Requested file analysis | Explicit Chat tasks accept valid CSV and .xlsx through mapping/validation and deterministic math; result is a one-off document, with no recurring jobs or automatic revision. |
| AC-15 | History and control | Deliverables and task history are retrievable and reusable; the owner can export or delete them and disconnect a linked account |
| AC-16 | Honest availability | Social Profile Import is visibly disabled and labelled Coming Soon; Instagram Business, X, connected analytics, native image generation, and billing are not represented as operational until implemented |
| AC-17 | Operational readiness | Security, isolation, integration, failure and recovery, observability, and end-to-end checks pass against the implemented workflows |
| AC-18 | New service business with unknown audience/baseline | Unknown/not-applicable remains explicit; propose useful evidence or offer work without invented audience proportions, CAC, or growth forecasts |
| AC-19 | Many inquiries, few qualified conversations | Investigate qualification, offer, response process, and alternative explanations before automatically recommending more posting or spend |
| AC-20 | Material practical constraints | Discovery respects relevant assets, permissions, budget and fulfillment; no standard weekly-hours onboarding question or unsupported paid execution. |
| AC-21 | No website but working messaging/marketplace route | The actual route informs the plan; no unnecessary site requirement or simulated social import |
| AC-22 | One scoped Arabic Post | Direct Chat creates a Publishing Post visible in Scheduling without forced research/campaign or a duplicate Deliverable. |
| AC-23 | Offer/price changes after approval | Affected work becomes stale and conflicting scheduled posts are held; owner resolution is recorded; edits/rescheduling require fresh approval; unrelated work remains usable |
| AC-24 | Best task needs an unsupported connector | Supply usable owner instructions/artifacts and a completion checklist; distinguish attested implementation from system-verified execution |
| AC-25 | Unknown outcomes, zero denominator, duplicated or incompatible data | Unknown never becomes zero; invalid ratios are withheld; repeated/overlapping totals, currencies and metric definitions are flagged before combining; provenance/period remain inspectable |
| AC-26 | Retired | Removed v1.1 recurring work/review-cycle acceptance; identifier reserved, not a release test. |
| AC-27 | Contradictory or malicious public source | Conflict remains visible; source instructions cannot authorize tools, override knowledge, raise budgets, leak private data, or redirect retrieval to internal resources |
| AC-28 | Timeout, revocation, cancellation race, or overdue publish | Reconcile or hold uncertain outcomes; no blind duplicate, unauthorized late dispatch, or silent send after reconnection; cancellation state and platform acceptance remain honest |
| AC-29 | Mobile mixed-script review and approval | Text/media, destination, date/time/timezone, status and controls are legible and usable in Arabic RTL and English LTR; keyboard/detail alternatives and independent language settings work |
| AC-30 | Intended ordinary owner connects and publishes | The production configuration supports the owner's actual access path and genuine publication; developer-role account success alone cannot satisfy the gate |

| AC-31 | Email verification link | Email/password-only signup sends a single-use expiring link; invalid/expired/replayed tokens fail, resend works, verification continues to Onboarding. |
| AC-32 | Google identity | Signup/login validate state/PKCE/nonce/claims; cancellation is safe, matching email never silently links, explicit reauthenticated link succeeds without takeover. |
| AC-33 | Optional offering pricing | Multiple offerings accept fixed, starting-from, quote-required and unknown models; optional amounts require currency and missing values never become zero. |
| AC-34 | Completion and return routing | After confirmation Chat and Create My Roadmap are available; later login restores an authorized last page/deep link or Director. |
| AC-35 | Choice-first forms | Current Brain personalizes meaningful choices plus Other; legitimate unknown/skip/not-applicable and real numeric/text fields work. |
| AC-36 | Wizard persistence | Close/reopen after edits and adaptive branching: saved draft/questions/statuses restore; conflicts do not overwrite another edit. |
| AC-37 | Question research and return | Save → linked conversation/task → selected evidence-aware result → exact question → confirmation survives switching/reconnect; forged/cross-tenant origins fail. |
| AC-38 | Knowledge reuse | Later Discovery reuses confirmed current answers/provenance; stale/conflicting facts are flagged and skipped/unknown answers never become positive knowledge. |
| AC-39 | Execute step | Eligible step launch checks authority/dependencies and creates one Task under double click/retry; reopening does not relaunch. |
| AC-40 | Skip and Revisit | Record intent/time, preserve non-completion, explain blocked dependents and alternate paths; Revisit recalculates readiness without cancelling running work. |
| AC-41 | Task capacity across conversations | Two running Tasks in different conversations continue; third remains truthfully requested/waiting, with atomic admission on release. |
| AC-42 | Conversation Canvas switching | Switching conversation changes selected Canvas; late old-stream events cannot contaminate it; conversation-local history selects other related Tasks. |
| AC-43 | Recoverable retry UX | Authorized transient retries remain Processing with no failure flash/attempt counter/notification; internal diagnostics retain causes. |
| AC-44 | Terminal failure | After operation-specific recovery is exhausted, show truthful failed or partial result and supported recovery; never conceal it as Processing. |
| AC-45 | Strategic revision | Diff includes added/modified/removed-or-superseded/dependency changes; stale approval fails, reject preserves active plan, accepted revision is atomic. |
| AC-46 | One active Roadmap | Concurrent activations cannot create two active plans; drafts survive, archived versions and original execution references remain inspectable. |
| AC-47 | Document lineage | Research saves a non-Post Deliverable; owner/Chat edits create a new immutable version of the same artifact with sources/context and staleness labels. |
| AC-48 | Manual New Post | Create/edit a draft in Scheduling without a connected Page; subsequent publishing requires connection and exact-version approval. |
| AC-49 | Unscheduled | Null publishing date appears only in Unscheduled; no invented calendar date or release workflow dispatch. |
| AC-50 | Calendar views | Month/Week/List and range/filter pagination agree; proposed dates are visibly unapproved; mobile and keyboard alternatives work across DST. |
| AC-51 | Bulk approval | Only enumerated displayed versions are decided; stale/missing/ineligible items report per-item failure and retry cannot approve a newer unseen version. |
| AC-52 | Rescheduling | Drag/form change of an approved Post creates a new version and invalidates approval; timezone default change never moves an approved instant. |
| AC-53 | Historical publication | Confirmed Posts appear at actual publishedAt, not an obsolete proposed date; duplicate creates a new Draft and local deletion claims no platform removal. |
| AC-54 | Connection recovery | Disconnect blocks sends and advances authorization generation; reconnect does not release overdue/uncertain work without fresh checks/approval as required. |
| AC-55 | Contextual notifications | Read/unread/count and mark-all-read work; exact form/Post/document/revision/proposal/settings links validate access and deleted subjects show unavailable. |
| AC-56 | Removed route absence | Active router/OpenAPI/module/job inventories contain no Today or Outcomes/recurring-review resources, including renamed substitutes. |
| AC-57 | Step model absence checks | Active schemas/components/DTOs/events contain no Now/Next/Later or horizon properties, board controls or planning-column limits. |
| AC-58 | Deferred Meta Ads | Phase 5 vision is consistent across documents/UI; no current Ads connector/tool/spend permission is advertised or executable. |

Evaluation must also include an established business with healthy acquisition but weak repeat demand, and low-volume/long-cycle cases. Different evidence must change priorities appropriately without implying a full e-commerce lifecycle or statistical-testing platform.

## 10. Evaluation approach

Evaluation runs throughout development, combining functional, integration, security, reliability, and end-to-end checks with human judgment. Owner sessions may be unpaid; no fixed external-tester count or paid pilot is required. Technical success alone does not establish willingness to pay, customer growth, or product-market fit.

### 10.1 Case design and judging

Build an initial set of approximately 24 authored business cases spanning sparse/conflicting evidence, direct requests, owner overrides, capacity/fulfillment limits, different conversion problems, source failures, and all four content registers. Include contrast pairs where a material fact changes, such as unknown versus measured baseline or no customers versus repeat customers. Keep held-out cases unused for prompt tuning and report their coverage. Synthetic fixtures must be labeled and excluded from customer-success evidence.

Use fixed source snapshots for reproducible grounding tests and a small live set for freshness/access. Judge diagnosis calibration, evidence support, relevance, alternative explanations, task feasibility, owner effort/comprehension, language/brand fit, and continuity after corrections/results. Correct answers need not match one prescribed strategy, but must be defensible from evidence and constraints. LLM judging can screen outputs; source inspection and suitable native human review decide material factual, cultural, and strategic questions.

### 10.2 Release gates and targets

The following gates adopt plan.md's proposals as the revision's evaluation baseline. They are not achieved results. Freeze the case rubric, sampling, critical/high defect definitions, and task-specific runtime/resource limits by the end of the first two project weeks, informed by technical slices. Changes require documented rationale and cannot silently lower the standard to pass a failed release.

| Area | Gate or target | Required interpretation/evidence |
|---|---|---|
| Required behavior | All active AC-01–25 and AC-27–58 pass under declared conditions, with detailed tests covering applicable FRs/NFRs | Native publishing, graph, Canvas, research, imports, and data controls use their real implementations |
| Truthfulness/control | Zero critical violations in the release suite: invented evidence/results, unauthorized action, budget bypass, cross-workspace disclosure, or fake execution | Any observed violation blocks release until repaired and retested; passing a finite suite is not proof of zero future risk |
| Decision/content usefulness | At least 80% of held-out cases pass the prewritten human rubric without major strategic/factual correction, overall and within each content register | Report counts, denominators, case coverage, reviewer basis, and small-sample uncertainty; do not hide failed language coverage in an aggregate |
| Grounding | All audited consequential factual claims have inspectable support or explicit owner-claim/hypothesis designation; no fabricated citation | Check source existence and actual support, not citation count |
| Publication | Retry, concurrency, cancellation, revocation, version/context change, late-job, and uncertain-result scenarios pass | Retain real platform evidence and production-owner access evidence; local idempotency alone is insufficient |
| Calculation/imports | Supported calculations match deterministic fixtures; unknown, zero, duplicate, period, unit/currency, and parsing cases pass | Models explain values rather than calculate or fabricate them |
| Recovery/security | No unresolved critical/high defect in scoped access, credential, deletion, backup/restore, durable-job, and rollback checks | Record test results and operator recovery instructions |
| Usability | No blocking defect in required Arabic/English critical journeys; measure the ten-minute first-value target | For supported cases report median/distribution of active interaction time, background latency separately, completion, and assistance; do not advertise achievement without data |
| Runtime/resources | Meet documented task-specific latency, timeout, concurrency/load, and cost ceilings | Establish numeric values from early slices, document release load and test conditions, and verify before release |

Gulf-oriented content needs appropriate regional review; a missing assessment is an unresolved quality gap. The ten-minute first-value objective is a usability target to measure and improve, not evidence of current performance. Evidence for mandatory behavior and truthfulness cannot be traded for a favorable average score.

### 10.3 Product measures and baseline comparison

Track confirmed setup → chosen Chat or Roadmap → requested Task → accepted useful output → recorded execution or owner attestation. Measure return use, correction effort, clarification burden, failed intake, uncertain publication and actual cost per accepted artifact/Task. There is no required review-cycle funnel or periodic performance dashboard. Compare equivalent work with owners’ existing tools using the same facts/evidence, and assess reuse after corrections. Report denominators, assistance and active interaction time separately from background latency. Small samples and repeated use do not establish revenue causality, regional demand or willingness to pay.

## 11. Dependencies, risks, and technical investigations

### 11.1 Early decisions and risk owners

Assign actual team members to these workstream roles at kickoff. Deadlines are project-relative and do not imply that an investigation or approval has already occurred.

| Area / accountable role | Decision or evidence due | Required response / boundary |
|---|---|---|
| Launch cohort / product and evaluation lead | Weeks 1–2: reachable service niches, conversion paths, owner capacity, Facebook relevance, and separate GCC evaluation needs | Narrow the first-use promise without claiming validated demand or reducing language commitments |
| Facebook / integration lead | Start week 1; by week 2 document selected app configuration, required access, verification/review path, supported formats, and blockers; publish a real test as early as access permits | Submit for production access at the earliest eligible point; no promised approval duration; developer-only evidence cannot pass AC-30 |
| Research / AI lead with evaluation partner | By week 2: evaluated baseline for source access, regional relevance, grounding, stopping, provider cost/latency, and budget control | Preserve dynamic multi-source research inside supported access; do not promise private-source workarounds; finalize bounded tool/provider choices from evidence |
| Shared data and state / platform lead | Weeks 1–2: entity/evidence/task/metric/approval/version contracts and transition rules; integrate through weeks 3–4 | Graph, Canvas, chat, Roadmap and publishing share records; no competing sources of truth or silent knowledge writes |
| Execution / platform and integration leads | Weeks 1–4: durable jobs/events, budget reservations, action preflight, retry/reconciliation, cancellation and context-hold design | Model decisions cannot override controls; ambiguous outcomes remain held rather than blindly repeated |
| Evaluation / evaluation lead | By week 2: rubrics, held-out coverage, severity definitions, sampling, load and numeric runtime limits; extend evidence with each slice | Do not invent success rates or lower gates to mask failures; arrange relevant native reviewers early |
| Data governance / platform lead | Initial design by week 4: retention, deletion/derived records, backup expiry/restore, export, logs, and token lifecycle | Test owner controls before release; retain only justified audit data and explain actual retention |
| Localization / UX lead | Weeks 1–2: shared bilingual components and review plan; continuous checks thereafter | Egyptian/MSA success does not establish GCC quality; missing review or mobile approval failures remain explicit gaps |
| Capacity / whole team | Week 1 availability check; week 2 and milestone re-estimates | Use actual productive hours and throughput; stop expansion and simplify within the contract before accepting a release shortfall |

Risk signals requiring corrective work include generic content-first plans across contrasting cases, repeated low-value clarifications/approvals, growing pending-update queues, weak Arabic source coverage, costly research loops, and graph/Canvas embellishment delaying an integrated owner journey. Address the underlying workflow, evaluation, or implementation limit before adding more generators or integrations.

### 11.2 Architecture and implementation boundaries

The default approach is one modular application with a shared system of record, file storage, and durable background execution, using established managed components where suitable. A relational store with explicit relationships can support the graph; a dedicated graph database, microservice fleet, custom agent framework, or streaming platform is not required. Provider/framework/database selections remain technical decisions.

Keep clear logical boundaries for context/evidence, tasks/artifacts/versions, proposals/decisions, approvals/integrations/attempts, deterministic requested analysis, and execution events/usage. Graph and Canvas render those records. A small set of research/analysis/drafting specialists may be selected dynamically; deterministic services own permissions, calculations, scheduling, imports, and side effects. Start with a small evaluated model/tool path and add complexity only for demonstrated quality/cost/latency benefit.

Supporting engineering designs must record concrete schemas, state transitions, source/format limits, deletion behavior, and failure tests. This section does not add public APIs, user-programmable workflows, or a separate research product.

## 12. Scope exclusions and unresolved commercial hypotheses

### 12.1 Explicit Phase 2 non-goals

| Excluded work | Phase 2 alternative / disposition |
|---|---|
| Operational Social Profile Import; Instagram Business/X publishing; connected analytics | Remain Phase 3; use guided context, owner examples, export, and explicitly supplied evidence in Chat |
| Native AI image generation and subscription/billing infrastructure | Remain Phase 4+; use owner media, internal quotas, task allowances, and real cost monitoring |
| Full CRM, contact database, lead enrichment, cold-outreach sequencer | Aggregate results, qualification definitions, and owner-sent drafts |
| WhatsApp/DM/social inbox reading, sending, or automation | Owner-supplied conversion destination and response/follow-up drafts; no implied connector |
| Paid campaign management, autonomous optimization, or spending | Bounded experiment recommendations and briefs; owner executes in another system |
| Website/CMS builder or live deployment | Ready-to-paste copy, instructions, and owner QA checklist |
| Full SEO/ranking/backlink/technical-audit platform | Bounded intent/content research and page observations with declared access/measurement limits |
| All-network publishing and all-format parity | One dependable Facebook connector with a declared format matrix |
| Video generation, design editor, or document-layout/export suite | Text, visual briefs, original owner media, and simple exports |
| Generic workflow/agent builder, marketplace, ontology designer, public research SaaS/SDK | Curated recipes, bounded dynamic planning, focused graph and execution views |
| Enterprise organizations, team roles, client portals, multi-user approval systems | Single-owner workspace with strong tenant isolation |
| Numeric business-health scores, predicted revenue lift, autonomous A/B system, full cohort/LTV or attribution engine | Qualitative diagnosis, deterministic supported metrics, small explicit experiments, inconclusive results where warranted |
| Universal business-model depth, all playbook worksheets/recipes, large free-tool acquisition portfolio | A narrow launch cohort, a small task library, and evaluation of recurring usefulness |

Recommending work does not grant permission or create a connector for it. Owner handoff cannot replace any existing Phase 2 native commitment. No lifetime-unlimited pricing promise, dual-credit storefront, payment provider, or price is selected; internal cost controls remain mandatory before billing.

### 12.2 Commercial learning during development

Evaluate whether owners need direction more than additional content volume; whether the chosen service cohort shares a useful workflow; where qualified demand actually originates; whether Discovery and task interactions are useful; whether Arabic flows improve comprehension; whether corrected context improves later work; and whether repeated value earns willingness to pay. Use real owner sessions when available, initially assisted and later without explanation, and compare with current workflows.

These are commercial hypotheses, not asserted facts or paid-pilot graduation prerequisites. Small samples can expose usability problems but cannot establish regional demand, retention, or causal growth. Choose packaging only after observing useful repeat behavior and service costs, including failed/repeated calls, infrastructure, and support. Weak repeat use should trigger examination of recommendations, feasibility, and task friction before adding channels or notifications.

## 13. Delivery and scope control

### 13.1 Capacity assumptions

Five people over 24 weeks provide 120 person-weeks of calendar allocation, not necessarily full-time engineering. The initial scenario is **20 productive hours per person per week**, or 2,400 hours; actual availability must be checked. At 15 hours the total is 1,800; at 25 it is 3,000. These are planning calculations, not verified estimates of implementation effort.

Reserve the final six weeks, approximately 600 hours in the 20-hour scenario, for integration failures, language/usability repair, security/recovery testing, and stabilization. The first 18 weeks have a 1,800-hour allocation including ongoing tests and supporting design work. No Phase 3/4/5 work is funded by this envelope.

| Shared workstream | Initial allocation | Accountable role |
|---|---:|---|
| Platform, evidence/Brain, approvals, data controls | 320 hours | Platform lead |
| Director, research, recipes, grounding, resource controls | 400 hours | AI lead with evaluation partner |
| Bilingual responsive UX, tasks, graph, Canvas | 380 hours | UX lead |
| Facebook access, scheduling, publishing and recovery | 260 hours | Integration lead |
| Confirmed intake, Discovery and requested analysis | 220 hours | Evaluation/data lead |
| Cross-system integration, tests, deployment, runbooks | 220 hours | Shared; rotating integration owner |
| **First 18 weeks** | **1,800 hours** | Shared allocation, not solo-person estimates |
| **Final six weeks** | **600 hours** | Stabilization/contingency, not new features |

Each of the five students can own a workstream, but ownership is not a claim that they perform every allocated hour. Share implementation and review across modules without exceeding individual availability. Re-estimate at week 2 and each milestone using actual throughput. If availability is lower, the schedule is not automatically feasible; expose the gap and apply §13.3.

### 13.2 Connected delivery milestones

Weeks below are relative to the project's development schedule, not dates implied by this document. The current PRD revision supplies the product baseline; its technical investigations, schemas, case sets, and runtime targets still need execution and verification.

| Weeks | Focus | Required milestone evidence |
|---|---|---|
| **1–2** | Confirm cohort/capacity; freeze supporting contracts and evaluation; source/cost research spike; Facebook access investigation; bilingual base components | One supported owner scenario, declared quality/resource gates, access path or named blocker, and measured provider costs/latency |
| **3–4** | Usable Phase 1 foundation: isolation, guided/URL intake, provenance, task/artifact persistence, first Director response, early real publishing spike | Real input produces saved context and useful draft; actual events only; development publishing evidence labeled at the access level exercised |
| **5–8** | First Phase 2 slice: confirmed intake/Discovery/step Roadmap, direct chat, claim review, bounded graph/Canvas, exact approval, publishing worker | Owner corrects context, chooses a task, inspects work, reviews a draft and executes an approved supported test post; production-access submission starts as soon as eligible |
| **9–12** | Research quality/stopping, dynamic specialists, campaign/calendar review, owner handoffs, initial recipes, cost/error visibility | Research informs an actionable decision; budget/source failures return honest partial results; shared context works across views; early usability findings addressed |
| **13–16** | CSV/Excel mapping for requested analysis, deterministic metrics, Wizard research return, step execution/revisions, context staleness, deletion/export, publishing edge cases | Persisted Discovery, direct work and requested analysis; controlled revisions; no fabricated result or automatic stale-content publication |
| **17–18** | Complete committed language/control paths; full acceptance suite; intended-owner access validation; feature freeze | Requirement ledger records implemented behavior and every unresolved failure; no feature inferred complete from UI alone |
| **19–22** | Reliability/fault injection, isolation/security, native review, mobile accessibility, cost/load checks, backup restore | Critical/high defects resolved; acceptance evidence and operator recovery paths complete |
| **23–24** | Release rehearsal, deployment/rollback, final relevant regressions, user/operator documentation and graduation evidence | Release only after binding gates pass; unmet production access or native capability remains a blocker |

External approvals have no guaranteed duration. Investigate from week 1 and submit at the earliest eligible point; do not schedule an approval outcome as predictable engineering work. Integrate weekly around one schema and end-to-end owner tasks. Five isolated subsystems do not constitute a completed release.

### 13.3 Release ledger and change control

Maintain a ledger for every FR/NFR and acceptance scenario with implementation reference, accountable person, verification evidence, and current status. Delivery priority is order of work, not permission to omit a commitment. Any new feature must identify its capacity source.

If the estimate exceeds capacity: stop later-phase/cosmetic work; simplify within the supported contract (focused graph views, fewer recipes, simpler calendar, bounded import layouts); reuse established components; re-estimate and expose the gap. Do not cut truthful state, authorization, data controls, recovery, required languages, or real native workflows. Changes to committed formats or behavior require an explicit documented release-contract decision. If a required capability remains unavailable, mark Phase 2 incomplete rather than labeling it done, beta, or replaced by manual handoff.

## 14. Requirement-to-acceptance crosswalk

This maps design commitments, not executed tests. Every release ledger entry needs an implementation reference, accountable person, fixture/load conditions, evidence and status. Retired FR-47/AC-26 cannot become active obligations. System Architecture Appendix A provides the five-document implementation map and the 40 approved redesign checks.

| Requirements | Principal acceptance scenarios |
|---|---|
| FR-01–02, FR-50 | AC-17, AC-29, AC-31–32, AC-34 |
| FR-03–07 | AC-01–02, AC-16, AC-21, AC-27, AC-33–34 |
| FR-08–13, FR-42, FR-48 | AC-03, AC-15, AC-23, AC-27, AC-38 |
| FR-14–16, FR-45–46, FR-51–54 | AC-04, AC-18–21, AC-24, AC-36–40, AC-45–46, AC-57 |
| FR-17–18 | AC-05, AC-22, AC-35, AC-37 |
| FR-19–23 | AC-06–07, AC-09, AC-17, AC-41–44 |
| FR-24–28 | AC-08–09, AC-18, AC-27, AC-37 |
| FR-29–32 | AC-10–11, AC-22, AC-24 |
| FR-33–37, FR-55 | AC-12–13, AC-23, AC-28–30, AC-48–54 |
| FR-38–40 | AC-14, AC-25, AC-27 |
| FR-41, FR-57 | AC-15, AC-22, AC-47–49 |
| FR-43–44 | AC-11, AC-29, AC-50 |
| FR-49 | AC-16, AC-24, AC-58 |
| FR-56 | AC-15, AC-34, AC-54–55 |
| FR-58 | AC-56–57 |
| NFR-01–02 | AC-03, AC-07–08, AC-16, AC-23, AC-27, AC-42–44 |
| NFR-03–06 | AC-09, AC-12–13, AC-17, AC-25, AC-27–28, AC-31–32, AC-37, AC-41, AC-51–54 |
| NFR-07 | AC-11, AC-29, AC-35, AC-50 |
| NFR-08–09 | AC-15, AC-17, AC-23, AC-28, AC-43–44 |
| P3-01–03, P4-01–02, P5-01 | AC-16, AC-58 verify honest current availability; future operation needs separate real evidence |
