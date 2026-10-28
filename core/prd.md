# BUSMORA — Product Requirements Document

**Version:** 1.1 — product foundations revised from plan.md  
**Date:** 28 October 2026  
**Product:** BUSMORA, an AI Marketing Director for Arabic-first business owners  
**Planning horizon:** Approximately 24 weeks of development by five students using AI-assisted software development  
**Revision basis:** [Independent PRD Improvement Plan](/home/mohamed/Mohamed/test/plan.md), 27 October 2026  
**Status:** Product and release requirements; not evidence of implemented capabilities or validated market demand

## 1. Purpose and decision status

This PRD defines BUSMORA's initial customer focus, owner workflows, capabilities committed to its first production-ready release (Phase 2), and later expansion. It specifies user-facing behavior, approval boundaries, evidence standards, and release acceptance. Detailed schemas, model/provider choices, frameworks, infrastructure, and platform API configuration belong in supporting technical designs.

The requirements marked **Phase 2** are binding release commitments. All version 1.0 Phase 2 capabilities remain committed; this revision bounds their implementation and strengthens the decision and learning cycle. **Phase 3** and **Phase 4+** remain in the product vision, but receive no assumed capacity in the 24-week delivery plan. They may proceed only when Phase 2 commitments and stabilization capacity are secure. A later-phase label does not permit presenting an unimplemented feature as operational.

BUSMORA is a graduation project with four simultaneous goals: a polished and reliable product, meaningful technical innovation, real value for business owners, and commercial potential. Paid pilots and a fixed number of external testers are not graduation prerequisites. Continuous evaluation is required. Functional readiness, eligibility for use by ordinary business owners, and commercial validation are separate judgments; none may be inferred solely from the others.

### 1.1 Requirement terms

- **Must:** required for the stated phase to be accepted.
- **Should:** expected behavior when applicable; an exception requires a documented product or technical reason.
- **Planned:** part of the product vision, with implementation committed to a later phase.
- **Open technical decision:** a solution choice to resolve through architecture and evaluation work without changing the product requirement.
- **Planning assumption/target:** an estimate or evaluation objective, not a measured result. Assumptions must be checked against actual capacity and evidence; release gates cannot be silently weakened to conceal a failure.

### 1.2 Revision and source authority

Version 1.1 preserves FR-01–44, NFR-01–09, AC-01–17, and later-phase identifiers, adds FR-45–49 and AC-18–30, and adopts the bounded scope and delivery approach in plan.md. This PRD is the release contract. The plan supplies rationale; the [Growth Roadmap Playbook](/home/mohamed/Mohamed/test/BUSMORA_Universal_Growth_Roadmap_Playbook.md) supplies diagnostic concepts and task recipes, not additional implicit feature commitments.

The five STAR lenses are adapted from the playbook's course interpretation. Its reconstructed worksheets, illustrative schedules, maturity scores, and priority formula are not validated instruments or mandatory product features. Course attribution belongs in design documentation; the UI uses plain-language questions and must not imply endorsement or access to original worksheets.

## 2. Product vision and boundaries

BUSMORA is an AI Marketing Director that helps an owner decide what marketing work deserves attention this week, prepare useful assets, complete supported actions with approval, and use actual results to choose what comes next. It combines guided planning with direct conversation. The owner should not need marketing terminology, a self-diagnosed problem, or sophisticated prompts.

The Marketing Director understands products, services, customers, pricing, goals, constraints, and available performance information. This broader context informs its work; its recommendations and execution focus on marketing, customer acquisition, go-to-market, and marketing-driven growth. Experienced marketers may use BUSMORA for execution and time savings, but marketing expertise is not a prerequisite.

The core experience is an **adaptive marketing cycle**: understand; diagnose; choose feasible work; research or draft; review and execute; observe; learn and propose revisions. It supports both native Facebook execution and accurately labeled owner execution outside BUSMORA. Both return to the same review process. The owner can request a competitor analysis, a single post, a strategy discussion, or another supported task at any time without completing a prescribed sequence.

The product's differentiation hypothesis is better decisions, less owner effort, and continuity across cycles in a usable Arabic-first experience. Arabic text generation, persistent context, and multiple agents alone do not establish a competitive advantage. BUSMORA must not claim exclusive capabilities, superior marketing results, or regional product-market fit without evidence.

### 2.1 Primary and secondary users

| User | Need | Product response |
|---|---|---|
| Primary launch cohort: Arabic-first owner-operated service business selling through inquiries, consultations, or bookings | Choose feasible work to improve qualified demand, conversion, or follow-up | Explain the likely constraint, prepare useful work, and review actual observations |
| Secondary: owner or marketer with marketing experience | Reduce research, planning, content, coordination, and publishing effort | Direct chat and task execution using the same Business Brain and controls |

The launch focus is a product hypothesis selected for a coherent inquiry-to-customer workflow, not a verified market-size conclusion. Initial evaluation should begin with reachable Egyptian owners and separately identified GCC country cohorts when owner access and suitable native review are available. Discovery selects a service niche; the playbook's website-development studio example does not determine the niche.

The initial cohort should have a deliverable offer, control of its inquiry destination, willingness to act on a small weekly plan and record aggregate results, and capacity to serve the intended demand. A website is optional. Native-publishing pilots should involve businesses for which Facebook is actually relevant; a connected Page does not establish channel fit.

Other business models may receive supported general analysis and drafts, with their limits explicit. Phase 2 does not promise equally deep e-commerce, SaaS, marketplace, enterprise, or regulated-sector workflows. The country/segment focus does not reduce the full Arabic/English UI or English, Egyptian Arabic, audience-specific Gulf-oriented Arabic, and Modern Standard Arabic (MSA) content commitments. Interface, conversation, and campaign audience may each use different languages or registers.

### 2.2 Primary owner jobs

| Owner job | Required product result |
|---|---|
| What should I do next with my time and budget? | A defensible priority, evidence and uncertainty, and a feasible next action |
| Help me get this work done | A usable artifact, an accurately labeled owner handoff, or verified supported execution |
| What happened, and should we keep doing it? | Observations, a calibrated interpretation, and an owner-reviewed next decision |

### 2.3 Product principles

1. **Useful direction from incomplete context.** BUSMORA may offer preliminary recommendations while distinguishing observations, conclusions, and hypotheses.
2. **Owner intent governs work.** It may perform internal steps needed for a requested task. Optional research or work beyond that request requires the owner's choice.
3. **Owner control of consequential actions.** Publishing, outreach, expenditure, live-site changes, and other external effects require explicit approval before execution.
4. **Persistent context with provenance.** Business knowledge, historical work, and preferences remain available without turning every inferred claim into a trusted fact.
5. **Inspectable systems.** The interactive Brain graph explains context and evidence; the authentic Agent Canvas explains actual work, failures, and recovery. Neither is a general visual programming tool.
6. **Accurate representation.** The platform must distinguish available, upcoming, partial, failed, and completed capabilities and actions.
7. **Capacity before volume.** Active work must fit owner time, budget, skills, assets, and fulfillment capacity, or expose the conflict. More posts or more traffic is not the default answer.
8. **Diagnosis before channel selection.** Consider offer/audience, destination, conversion, acquisition, and existing-customer opportunities. A missing integration must not bias strategic priority toward the only executable channel.
9. **Completion is not impact.** Draft completion, owner-reported implementation, verified publication, measured outcomes, and causal explanations are distinct. Unknown data is not zero; an inconclusive review is valid.

## 3. Phase plan and release contract

| Phase | Product outcome | Included capabilities |
|---|---|---|
| **Phase 1 — Foundation** | Establish the necessary platform, data, integration, and operational foundations for Phase 2 | Account and single-owner workspace foundation; isolation; task and deliverable persistence; knowledge and approval states; event and audit records; secure integration and background-work foundations; evaluation and observability foundations |
| **Phase 2 — First production-ready release** | An intended owner can choose useful work, complete a native or owner-executed task, and review results in a repeatable cycle | Website/guided intake; goal and capacity context; Business Brain and interactive graph; evidence-backed diagnosis and executable Now/Next/Later roadmap; Director Chat; Fast Router and bounded dynamic multi-agent orchestration; authentic Canvas; dynamic multi-source research; text-first campaigns and owner media; real Facebook connection, approval, scheduling and verified publishing; CSV/Excel and manual outcome capture; recurring review and controlled updates; context staleness; saved/exportable deliverables; full Arabic/English responsive interface and four content registers; operational safeguards |
| **Phase 3 — Product expansion** | Broaden inputs, publishing reach, and automatic measurement | Operational social-profile-link onboarding; independently activated Instagram Business and X publishing integrations; connected-platform analytics where authorized and available |
| **Phase 4+ — Further expansion** | Extend creation and commercialization | Native AI image generation and a later subscription/billing capability, subject to separate design and evaluation |

The roughly 24-week schedule and AI-assisted development do not automatically remove agreed capabilities. A handoff can complete a task designed for owner execution, but cannot replace the mandatory native Facebook integration, graph, Canvas, research, or another committed capability. Development milestones (§13) are distinct from customer roadmap stages; no business is forced through a universal 12-week growth program.

### 3.1 Phase 2 completion standard

A committed capability counts as complete only when its main workflow operates end to end with real data or genuine connected accounts as appropriate; integrates with the rest of BUSMORA; reports true execution state; handles visible failure and recovery; and passes relevant functional, security, integration, reliability, and end-to-end verification. Mocked operations, simulated agent activity, and interface-only demonstrations cannot establish completion.

Production readiness includes monitoring, access controls, data protection, error handling, recovery, bilingual usability, and the acceptance gates in §§9–10. Detailed test cases and task-specific service/resource targets must be documented and verified at the declared release load. No paid customer or fixed external-tester count is required.

Development/test-account publishing proves only the access path exercised. Phase 2 must also establish that an intended ordinary owner can connect and publish using the production app configuration without relying solely on a developer-role account. Exact platform requirements must be verified for that configuration. Unresolved production access blocks claiming the intended Phase 2 release is complete.

### 3.2 Bounded Phase 2 capability matrix

| Surface | Committed release behavior | Boundary |
|---|---|---|
| Diagnosis and tasks | Five diagnostic lenses, a primary constraint hypothesis, capacity-aware priorities, and a small curated recipe set | No universal business audit, numeric health score, or mandatory worksheet sequence |
| Research | Dynamic search/retrieval and synthesis across distinct relevant public source types, including accessible websites and review/community material | No guarantee of every site, private-group access, authenticated social harvesting, paid databases, or comprehensive ad-library coverage |
| Creation | Editable text, campaign/offer/page-copy briefs, response/interview guides, visual briefs, and owner media | No native image/video generation, design editor, live-site editing, or message sending |
| Facebook publishing | Text/link posts and a single owner-supplied image with caption, with approval, scheduling, verification, and recovery | Verify API support early; no implicit video, story, reel, carousel, or every-format promise. A format change requires a documented contract decision |
| Measurement | Documented CSV and Excel (.xlsx) templates, recognized-field mapping, manual aggregate observations, and deterministic supported metrics | No arbitrary workbook interpretation, live analytics, CRM, cohort/LTV forecast, or multi-touch attribution |
| Brain and Canvas | Interactive typed entity/relationship graph and actual task execution visualization, with accessible detail/list views | No ontology designer, unrestricted graph workspace, or workflow builder |
| Calendar and export | List/week publishing view; text/Markdown/CSV and applicable original media exports | No enterprise calendar or document-design suite |
| Unsupported external work | Clear owner-ready draft/instructions, responsibility, and owner-reported completion | Never represented as executed or independently verified by BUSMORA |

Supported limits, blocked states, and later-phase capabilities must be visible at the point of use. Failures outside supported limits require a useful explanation and fallback, not simulated success.

## 4. Core user journeys

### 4.1 Establish context and reach first value

1. The owner creates an account and single-owner business workspace.
2. They choose website import or guided manual setup. The onboarding screen also displays Social Profile Import as **Coming Soon** and disabled until Phase 3.
3. Website import extracts accessible business and brand information and stores it as immediately usable, source-labelled **imported** context. It does not demand field-by-field approval before use.
4. After import, the owner chooses **Explore Your Business Brain** or **Meet Your Marketing Director** and can return to the Brain later.
5. Guided setup collects enough context to start: offer, market/country, stage, goal, actual inquiry/conversion destination, language, and rough owner capacity. Unknown audience, baseline, or economics are valid and clarified progressively.
6. The Director presents a short direction card: goal, likely limiting factor, relevant evidence, material uncertainty, and one recommended next action. It offers an editable Now/Next/Later plan without requiring a complete audit.
7. The owner selects the next action and receives a relevant initial artifact, such as offer copy, a post, an inquiry-response draft, a measurement template, or an interview guide. The system records responsibility and an appropriate review date.

The first-value target is an understandable next action and usable initial artifact within ten minutes of active owner interaction for a typical supported case. This is an evaluation target, not a measured or advertised result. Background latency is measured separately (§10).

### 4.2 Get direction and work directly

1. The Director considers the goal, evidence, conversion path, and constraints; it explains its main hypothesis, material alternatives, and what would change the recommendation.
2. Each task states why now, responsibility, execution mode, effort, dependencies, expected output, missing inputs, completion condition, metric/review need, and any required approval.
3. The owner may approve, reject, postpone, reprioritize, challenge, or revise activities. They may also request a supported task directly in chat.
4. New evidence, completed work, changed goals, and performance results can prompt proposed roadmap changes. The owner reviews changes before they are applied.
5. A direct request for one post or answer remains scoped. It does not require a full strategy interview, campaign, research run, or roadmap rewrite. Direct work shares context and history with planned work.

### 4.3 Research, create, approve, publish, learn

1. The owner requests research, a campaign, one post, or another supported marketing task.
2. BUSMORA reuses relevant Brain context, clarifies only essential missing details, and offers a choice when optional extra research would improve the result.
3. The Router answers simple interactions directly or sends substantive work to the Orchestrator. The Orchestrator plans and delegates dynamically within its authorization and resource limits.
4. The owner can inspect real work in the Agent Canvas and receives a source-supported deliverable or an honest partial result.
5. The owner reviews exact post text, media, destination, time/timezone, and disclosed delivery allowance. Selected or batch approvals bind to those versions. Unchanged approved details may execute without a second approval, subject to current authorization, cancellation, and context checks.
6. The system records actual dispatch and verification, or a failed/uncertain outcome with a recovery path. It holds affected posts when context becomes stale, authorization fails, or the approved delivery window expires.
7. The owner imports a CSV/Excel file or enters aggregate observations. BUSMORA interprets them against the task/campaign objective and proposes review decisions and separate roadmap/Brain changes.

### 4.4 Complete work outside BUSMORA

When the best action needs an unsupported connector, the Director prepares a ready-to-use artifact, instructions, completion checklist, and observation plan. The owner performs the work and may attest to completion or attach evidence. BUSMORA labels that report accurately; draft acceptance does not prove external implementation. Website changes, inquiry responses, follow-up, and paid experiments use this path in Phase 2 and do not acquire new execution permissions.

### 4.5 Return and review

The Today view shows the current goal, next action, owner blockers, approvals, and review due. The review identifies completed/not-completed work, observations and their period/source, missing data, interpretation limits, and a proposed decision: continue, change, stop, gather evidence, or propose scale. Scale remains a recommendation; it never authorizes spend.

Review cadence defaults to weekly where useful and adapts to the sales cycle and data volume. A result may be inconclusive. The owner reviews strategic revisions and persistent knowledge proposals separately, even when displayed together. Changes to business facts flag dependent work without automatic regeneration. Phase 2 needs an in-app return/review experience, not notification channels or autonomous background market monitoring.

## 5. Functional requirements — Phase 2

### 5.1 Accounts, workspaces, and navigation

**FR-01.** The owner must be able to create and access a business workspace. Each workspace has one primary Owner with control of its Brain, integrations, approvals, deliverables, and data. The system must isolate one business workspace's information and credentials from every other workspace.

**FR-02. Navigation and Today view.** The default returning-owner view must show the current goal, next action, blockers, approvals, and review due, with clear access to Director Chat, roadmap, Business Brain, deliverables, campaign/publishing work, and settings. Related tasks, artifacts, sources, and decisions must be reachable in context. The calendar is a list/week view of publishing records. Critical flows must work responsively on mobile. Team invitations, organization hierarchy, shared approvals, and multi-user role management are outside Phase 2.

### 5.2 Onboarding and initial business context

**FR-03. Website import.** The owner can submit a business website. BUSMORA must access permitted content, extract useful business and marketing context, including available brand messaging and visual identity cues, and indicate real progress, completion, partial completion, or failure. Page selection must prioritize relevant business content; login/signup and other low-value pages are excluded by default unless relevant. The import must expose meaningful source gaps. An inaccessible or sparse site must promptly offer guided setup and must not block useful work. Extracted text alone cannot establish page speed, visual quality, mobile functionality, or successful form submission.

**FR-04. Imported knowledge.** Website-derived items enter the Brain automatically as usable imported context with source/page where available, import time, extraction or verification status, and an uncertainty indicator when meaningful. Confidence in the extraction process must remain distinguishable from confidence that the underlying website claim is true. The product must not show arbitrary numerical confidence percentages.

**FR-05. Post-import choice.** The owner can inspect and correct the Brain immediately or start with the Marketing Director, using the imported context. Initial manual review is optional.

**FR-06. Guided setup.** With or without a useful website, the owner can provide business name/description, offer, target market/country, known or unknown audience, business model/stage, primary goal, conversion destination, language, and rough weekly capacity in plain language. Use conversational groups and progressive clarification; unknown fields are valid. The owner may ask for audience help. Logo, colors, tone, positioning, price/economics, channels, skills/assets, and budget are collected only as needed. Missing details must not prevent preliminary guidance or the first-value journey. A functioning messaging or marketplace route must be considered before recommending a new website.

**FR-07. Social import visibility.** The onboarding interface must display Social Profile Import as clearly unavailable and **Coming Soon** in Phase 2. It must not begin a simulated import or imply access to private profile information. Its actual operation is a Phase 3 requirement.

### 5.3 Business Brain and interactive graph

**FR-08. Structured memory.** The Brain must represent business identity, model/stage, offerings, audience/market, objectives, constraints/capacity, conversion path, positioning, brand voice/visual identity, channels, competitors/alternatives, research, owner preferences, and decisions. It must also relate claims to proof, goals to metric definitions, and tasks/artifacts to the context versions they used. Business knowledge and Director working preferences remain distinct. The graph and other views must use the same stored records; storage/retrieval choices remain technical decisions.

**FR-09. Epistemic and lifecycle state.** The system must distinguish source observations, owner-provided claims, AI hypotheses/interpretations, and owner-confirmed decisions/preferences. Source type, verification status, approval, confidence rationale, and current/superseded status are separate dimensions. Conflicts, unknown values, and non-applicability must be explicit. Approving an item for persistent use does not independently verify its truth; neither a citation nor prominence on a website establishes the underlying claim or revenue contribution.

**FR-10. Update rules.** Initial website imports are active with an **imported/unverified** designation. Owner edits take effect as owner-provided information. AI-proposed persistent facts, interpretations, or preferences from conversations, research, deliverables, or analysis enter **Pending Brain Update**; the owner can approve, modify, or reject them. Re-imports and reports must not silently overwrite established information. Material changes create a version and trigger the dependency/staleness behavior in FR-48; conflicting evidence must remain intelligible until resolved or handled under data controls.

**FR-11. Historical work.** Conversations, tasks, decisions, research, campaigns, and deliverables must remain retrievable and reusable as context when relevant. Saving a deliverable does not promote all its statements into approved persistent business knowledge. Personalization should improve through approved knowledge, owner corrections, and relevant prior work; it does not require retraining the underlying model after each interaction.

**FR-12. Interactive graph.** The owner must be able to explore relevant typed entities and relationships, inspect sources/status and related decisions, add or correct business information and relationships, delete information, and act on pending updates. Focused neighborhoods and conventional editing forms are sufficient; an unrestricted graph or ontology designer is not required. The graph must communicate uncertainty. An accessible entity/detail list supplements the interactive graph and does not replace it.

**FR-13. Owner data controls.** The owner must be able to inspect, edit, export, and delete Brain information and historical deliverables, and disconnect linked accounts. Changes and approvals must be recorded under the documented retention policy so the product can explain what changed and why. Deleted information must stop participating in retrieval, including derived records; related artifacts must be removed, redacted, or lose their evidentiary basis as appropriate. Disconnection prevents future external sends and is not reversed implicitly by a retry.

### 5.4 Marketing Director, diagnosis, and roadmap

**FR-14. Business-aware diagnosis.** The Director must assess relevant offer/audience, destination, conversion, acquisition, and existing-customer opportunities against the owner's goal and actual conversion path. Its compact diagnosis must state the primary constraint hypothesis, supporting observations with sources/dates, material alternatives and disconfirming evidence where available, the missing evidence most likely to change the choice, and a recommended next action with reasons to defer alternatives. It must separate observations, conclusions, and hypotheses. Sparse data permits a preliminary direction or a useful evidence-gathering task, not invented specificity. Do not require a full audit before a scoped request.

**FR-15. Marketing scope.** Recommendations and execution should remain within marketing, customer acquisition, go-to-market, and marketing-driven growth. Broader business facts can inform a recommendation without making BUSMORA a general business manager.

**FR-16. Adaptive roadmap.** The Director must produce an editable, versioned Now/Next/Later roadmap aligned with goals, evidence, dependencies, and owner capacity, using FR-45–46. Default to one to three active tasks, fewer when capacity requires; expose blocked, deferred, and not-applicable work distinctly. Explain why each priority deserves attention now and why alternatives are deferred. Owners can approve, change priority, reject, postpone, or revisit tasks. Record overrides and their consequences; rejection is a planning decision, not a permanent market fact. Strategic revisions require owner review before application; actual task status may update from execution evidence.

Prioritization must first identify critical observed failures and enforce authorization, capability, budget, dependency, and fulfillment constraints. It then favors the best-supported constraint on the goal; if uncertainty could change the decision, a cheap discriminating question or research task may take priority. Rank feasible alternatives by effort and time to useful feedback. Do not use the playbook's multiplicative priority index, aggregate maturity score, or mandatory universal sequence. The correct recommendation may be no new acquisition campaign.

**FR-17. Direct chat.** The owner can ask questions, challenge assumptions, change priorities, or launch a supported task without following the roadmap in order. Roadmap-driven and direct work must share the same Business Brain, decisions, and deliverable history.

**FR-18. Intelligent clarification.** BUSMORA must reuse available context and ask targeted questions only when missing information materially affects the requested work. If optional research goes beyond the owner's request, it must offer the owner a choice to authorize that research or proceed with transparent assumptions when feasible.

### 5.5 Router, orchestration, agents, and Agent Canvas

**FR-19. Fast Router.** Incoming conversational interactions must pass through a fast, cost-conscious AI routing step. It may answer simple conversation directly and must send substantive tasks to the Orchestrator. Navigation, deterministic approval actions, and ordinary UI controls do not require an AI call. The chosen route must not fabricate agent work; model selection remains open and routing complexity must be justified by evaluation.

**FR-20. Dynamic Orchestrator.** The Orchestrator must interpret the objective, load relevant context, plan work, select tools/specialists, execute suitable work directly, adapt as evidence arrives, consolidate results, and report outcomes. Runs retain workspace/task identity, context snapshot, objective, inputs, permitted tools, evidence/output contract, review state, proposed knowledge updates, and iteration/time/usage/concurrency ceilings. Child work inherits the parent allowance. The system must support genuinely context-dependent plans, including selecting, skipping, or repeating authorized steps; no fixed sequence is required for every task. Unrestricted tool invention, recursive spawning, and arbitrary executable workflows are excluded. Authorization, budget checks, validated state transitions, and external execution must be enforced outside model discretion.

**FR-21. Specialist capabilities.** Phase 2 must support research, marketing analysis, campaign planning/copy, publishing, and performance interpretation. The exact agent count, module boundaries, and model assignments remain technical choices; eleven playbook roles do not imply eleven independent agents. The Canvas must reflect whichever agents actually run and distinguish tool/service calls from agent work. Publishing uses a controlled execution service, not discretionary agent authority. Multiple specialists are used when the task justifies them, not to fill a visual sequence.

**FR-22. Authentic Agent Canvas.** For tasks benefiting from visualization, the Canvas must show actual plan/execution states, agents involved, tool/source activity, useful evidence/findings, handoffs, retries, failures, and owner-input points. A focused task diagram with expandable details and an accessible event list is sufficient; no workflow builder is required. Events and progress must correspond to recorded execution. Use percentages only when total work is known; otherwise show actual steps/activity. Action summaries must not expose secrets, sensitive raw prompts, or private model reasoning.

**FR-23. Recoverable work.** Longer tasks must have durable identities and retrievable status after navigation or ordinary interruptions. Partial results, failures, retries, and next recovery steps must be visible. Owners must be able to stop further internal work; cancellation cannot claim to undo an already completed tool action. Publishing cancellation follows FR-35–36. Execution and event-delivery mechanisms remain technical choices.

### 5.6 Research Agent and evidence-backed intelligence

**FR-24. Dynamic multi-source research.** Research is a core Phase 2 capability. The agent must interpret the question, form/revise a plan, dynamically choose relevant available sources, examine distinct source types when justified, and identify gaps. The initial supported scope is public search and website retrieval, including accessible competitor, review/community, and relevant market material. Public social/ad-library material may be used when accessible, but comprehensive coverage or private/authenticated access is not promised. No fixed source list or sequence applies to every task. Stop when evidence supports the decision, further retrieval is repetitive, useful access is exhausted, or authorized time/usage is exhausted; explain partial results and remaining questions.

**FR-25. Research output.** Supported tasks include competitor analysis, audience/customer pain discovery, market opportunity research, positioning/messaging analysis, and strategic recommendations. Outputs must connect the question, evidence, limitations/contradictions, business implications, options, and recommended next task. Customer-language findings must retain original wording, source/date, market context, and separately labeled translation where used. Public comments are sampled observations, not representative survey results. Research must inform a decision rather than merely list results or maximize citations.

**FR-26. Evidence quality.** Material findings must retain inspectable source references, relevant supporting context, and access/publication dates where available. A citation must support its associated claim; source existence alone is insufficient. Compare sources, disclose stale/inaccessible evidence and consequential contradictions, and distinguish duplicated material from independent corroboration. Never invent citations, exclusive access, interviews, reviews, audience proportions, search volumes, competitor economics, or activities. Observations, owner claims, interpretations, and hypotheses stay separate. Source failures require an honest alternative or partial result, and regional coverage gaps must be explicit.

**FR-27. Per-task research budget.** Each research task must have an authorized budget covering applicable time, model, tool, and paid-source resources. Its initial allowance may reflect task complexity within the owner's authorized usage limits. The agent may reallocate work within that budget. **Any increase requires explicit Owner approval before further consumption.** The request must explain the additional resources, likely time or cost effect, and expected benefit. A refusal must result in work within the original budget or the strongest supported partial result; agents must not evade the limit by switching tools or creating equivalent tasks. Approval of computational usage must not imply consent to undisclosed customer charges.

All delegated work and retries share the parent budget. Independent controls must reserve/account for concurrent consumption before calls where necessary. A provider that cannot be adequately bounded must not be exposed to autonomous execution. The owner must be able to inspect the authorized allowance, consumption, and any requested extension; refusal does not erase useful partial work.

**FR-28. Modular research capability.** Research planning, retrieval, evidence extraction, claim/source association, synthesis, and budget accounting should have separable interfaces, with BUSMORA-specific interpretation layered on them. Provider/model/tool choices require an early evidence-backed architecture and evaluation investigation, including quality, regional coverage, latency, cost, and stopping behavior. Reusability does not commit Phase 2 to a research SaaS, marketplace, public SDK, or custom agent framework.

### 5.7 Campaigns, content, and brand voice

**FR-29. Flexible output size.** BUSMORA must respond to the requested scope. A single-post request must not force a campaign. Campaign packages may contain objective, audience, strategy/hypothesis, messages, channels, duration/schedule, channel-specific copy, CTA, relevant hashtags, visual briefs/owner media, approval state, and metric/review context. Link packages to their task and goal. Offer/page-copy briefs, interview guides, response drafts, and other owner-ready text use the same artifact/editor/export path, not separate applications. A planned channel does not imply a publishing integration.

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

### 5.9 Performance analysis and continuous improvement

**FR-38. Performance-file import.** Phase 2 must accept CSV and Excel (.xlsx) campaign-performance data using documented templates and recognized-field mapping. Before analysis, show a mapping/validation preview establishing metric definitions, date range, granularity, units/currency, and identifiers. Flag repeated imports, overlapping totals, malformed values, incompatible currencies/definitions, and ambiguous identifiers; do not silently combine them. Preserve observations with source, import date, and measurement period. Explain unsupported files/layouts and offer the template or manual capture (FR-47). Supported metrics must be computed deterministically from supplied values, without inventing measurements or executing spreadsheet macros/formulas.

**FR-39. Goal-based interpretation.** The Director must compare available observations with the task/campaign goal, its baseline status, and period, then identify patterns, limitations, and recommendations. Keep business outcomes, funnel conversions, and channel/operating signals distinct. Supported measures may include reach, impressions, engagement, clicks, inquiries/leads, qualified inquiries, bookings/customers, costs, and financial ratios only when inputs and attribution support them. Rates require defined denominators; zero denominators and unknown inputs must not produce fabricated results. ROAS requires attributed revenue and ad spend; ROI requires a stated profit/cost basis. Application code calculates; the model explains. Engagement, completed tasks, and publication do not establish sales, profit, or causation. No forecast, LTV model, cohort engine, or causal-attribution platform is committed.

**FR-40. Controlled learning.** Store sourced observations as historical data. A review presents observations first, interpretations second, and a proposed decision third, including inconclusive findings and attribution/sample limitations. AI interpretations proposed for persistent Brain use enter Pending Brain Update. Strategic roadmap revisions require owner review. Both decisions may share a review screen but remain separate, and neither authorizes publication/spend. Preserve diagnosis, decisions, expected versus actual observations, and caveats for future work. Task status can update from actual events without approval for every internal transition.

### 5.10 Deliverables and history

**FR-41. Persistent outputs.** Research, strategies, plans, copy/briefs, calendars, and review diagnoses must be saved with originating task/goal, version, creation time, relevant context version, and sources/assumptions. The owner can find, inspect, reuse, export, delete, and reopen the related next action. Use text/Markdown for narrative outputs, CSV for structured records, and applicable original-media downloads; a designed PDF/Word export suite is not required. Task/approval/history relationships must survive ordinary navigation and retrieval.

**FR-42. Context reuse.** The Director must retrieve relevant approved Brain information, active imported/owner-provided context with their status labels, and prior work without treating historical deliverables as current authority. Deleted records and rejected updates must not re-enter active context through retrieval. Expose time-sensitive dates, limitations, supersession, and dependencies under FR-48. A proposed preference does not become an adopted preference merely because it appears in an old conversation.

### 5.11 Application language and accessibility of information

**FR-43. Full localization and responsive access.** Phase 2 must localize Today/navigation, onboarding, Brain graph/details, Director Chat, Canvas/events, roadmap/tasks, campaigns/calendar, approvals, import/manual outcome/review flows, data controls, settings, and empty/loading/error states in Arabic and English. Support Arabic RTL, English LTR, mixed scripts, keyboard operation, readable labels/errors, and responsive mobile critical flows. The owner must be able to review content, choose destination/time, approve, correct context, and record results without a desktop-only dependency. Accessible lists supplement the graph and Canvas.

**FR-44. Independent language choices.** Application interface language, conversation language, and marketing-content language must be separately selectable or inferable from the owner's request and context. A user may use an English interface, converse in Egyptian Arabic, and create Gulf-oriented campaign content.

Country, currency, and IANA timezone must be stored explicitly rather than inferred from language. Dates, numbers, currencies, names, and links must remain understandable in mixed-script views. Scheduling must use timezone rules, including seasonal clock changes, rather than a permanent offset for Cairo or another market.

### 5.12 Outcome, task, review, and context contracts

**FR-45. Outcome and capacity contract.** The system must retain one owner-selected primary goal and review horizon, a relevant conversion path, and available owner capacity. Baselines and targets must be separate, with observed, owner-reported, estimated, unknown, and not-applicable states as appropriate. A target is a desired outcome, not a forecast. Metric definitions retain source, period, and unit. Active work must fit time, skills/assets, budget, and fulfillment constraints or explicitly expose the conflict. Business budget, AI resource allowance, and authority to spend externally are distinct; none implies another.

**FR-46. Executable roadmap tasks.** Every active task must state its rationale/evidence, responsible party, execution mode, inputs, deliverable, dependencies, effort, completion condition, and review need. It must retain applicable resource limits, task authorization, artifact review state, and a separate external-approval reference. Metric/baseline status, review date, observations, caveats, and proposed decisions are attached when relevant; not-applicable fields need not burden a single-item request. Use progressive disclosure instead of a long mandatory form.

Maintain independent work, external-action, and learning states. A completed draft is not completed owner implementation; an owner attestation is not independent verification; a published post is not measured commercial success. An experiment adds hypothesis, baseline status, observation window, comparison basis where available, primary outcome, guardrail, and decision rule before interpretation. It does not require a separate experimentation platform or authorize automatic scaling.

**FR-47. Outcome capture and recurring review.** Alongside CSV/Excel import, owners must be able to enter relevant aggregate inquiries, qualified inquiries, bookings/customers, spend, and qualitative observations with reporting owner, source, period, and currency where applicable. Unknown fields are optional and never default to zero; customer-level contact storage is not required. The Director must produce a concise review and proposed next decision, including inconclusive results, and surface reviews due in the Today view. Cadence follows the business's sales cycle. Strategic roadmap and persistent Brain changes retain separate review under FR-40.

**FR-48. Context impact and freshness.** Version decision-critical context and retain dependencies for active tasks/artifacts. Changes to offer, price, audience, service area, or claim support must flag affected work for review; hold conflicting scheduled posts pending owner resolution. Do not silently rewrite approved content, regenerate everything, spend new research resources, trigger external actions, or overwrite historical evidence. If approved publishing details change, FR-34 requires fresh approval; even if they do not change, record the owner's resolution before clearing a context hold. Unrelated work must remain usable. Conflicting/superseded records stay intelligible subject to data controls.

**FR-49. Honest capability and handoff contract.** Recommendations and tasks must distinguish supported analysis/drafting, supported connected execution, owner execution, blocked work, and later-phase capability. Unsupported execution must produce useful instructions/artifacts and a completion checklist when feasible, with an explicit responsible party and unblock conditions. Owner-reported results remain labeled. Never claim external work was completed by BUSMORA merely because a draft, recommendation, or integration screen exists.

### 5.13 Curated task recipes

Use a small set of reusable configurations of FR-46, initially about ten to twelve recipes: goal/baseline setup; audience/alternative research; claim-proof review; offer/message clarification; destination/CTA review; inquiry qualification; inquiry-response drafting; one-channel campaign preparation; approved Facebook publishing; outcome review; and an appropriate existing-customer follow-up/referral draft. A recipe is selected only when relevant and supported; it is not a mandatory checklist or separate module. Verify any claimed external measurement or use an owner QA checklist with honest limits.

## 6. Phase 3 and Phase 4+ requirements

Later phases retain their own acceptance standards and receive no assumed delivery capacity before the binding Phase 2 release is secure. Order additional integrations by observed owner friction; a read-only measurement source may be more valuable than another publisher. Do not enable a feature merely because its interface is present.

### 6.1 Phase 3 — Social Profile Import

**P3-01.** The owner can provide a supported social page or profile URL to establish or enrich the Brain using accessible public or appropriately authorized business content. One supported link is enough to start. BUSMORA should extract relevant offerings, messages, historical communication patterns, and brand voice where available and ask concise follow-up questions if the source is sparse. A URL alone must not be treated as permission to access private content or analytics. The **Coming Soon** control becomes enabled only when this workflow functions end to end.

### 6.2 Phase 3 — Additional publishing and connected analytics

**P3-02.** Instagram Business and X can each be connected, approved, scheduled, published, verified, monitored, and disconnected independently. Each must meet the same approval, version binding, authorization, idempotency, failure visibility, and release-quality principles as Facebook Pages before being labelled available.

**P3-03.** Where the owner authorizes it and the platform makes metrics available, BUSMORA can retrieve connected-platform performance data and use it in the same goal-based analysis and controlled-learning process as manual imports. Direct analytics access is distinct from publishing authorization.

### 6.3 Phase 4+ — Creation and commercial expansion

**P4-01.** Native AI image generation may create visual assets from a campaign brief and approved brand context. Its eventual agent or Skill design, provider, evaluation, rights, and cost handling require separate design before commitment. Phase 2 continues to support visual briefs and owner media.

**P4-02.** A later commercial phase may add subscriptions, payment collection, plan-based usage quotas, and billing administration. No payment provider, pricing structure, or mandatory paid pilot is selected by this PRD. Phase 2 still requires internal usage controls and cost monitoring.

## 7. Approval and knowledge-control matrix

| Activity | Phase 2 rule |
|---|---|
| Internal steps necessary for an explicit owner request | Proceed within task authorization and resource limits; clarify only essential gaps |
| Optional new research or a substantial workflow outside the request | Present the choice and wait for owner authorization |
| Initial website import | Store and use immediately as imported, source-labelled context; owner can inspect and correct |
| Owner edits to business information | Apply as owner-provided information; version material changes and flag dependencies, without treating the edit as verification or publication approval |
| AI-proposed persistent fact, interpretation, or preference | Hold as Pending Brain Update until owner approves or edits and approves |
| Saving a historical deliverable or performance observation | Save with source and date; do not automatically promote its interpretations to trusted Brain knowledge |
| Actual task/execution status | Update from recorded events or explicitly labeled owner reports; do not require approval for every internal transition or equate completion with growth |
| Proposed strategy/roadmap revision | Show the change, rationale, dependencies, and impact on capacity; apply after owner review |
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

**NFR-01. Truthful state.** Brain graph, Canvas, tasks, calendar, and review information must derive from stored knowledge and actual events. Planned, running, awaiting input/review, completed, failed, held, cancelled, and uncertain states require corresponding evidence. Drafted, owner-implemented, published, and commercially effective are distinct. Do not simulate agents, tool calls, sources, progress percentages, metrics, or customer results.

**NFR-02. Source integrity.** Retain enough source identity, date, and supporting context to inspect consequential claims. Source validity and actual claim support must be evaluated separately. Inaccessibility, staleness, contradiction, uncertain independence, and limited geographic coverage must be visible. Cache reuse must preserve freshness labels. Generated claims and owner approvals must never be substituted for independent evidence.

**NFR-03. Workspace security.** Authentication and authorization must isolate each owner's context, retrieval, files, tasks, credentials, approvals, exports, and execution. Store secrets/tokens securely, restrict tool access to the current workspace, and enforce current authorization in backend services independently of the model or UI. Test tenant isolation, credential handling, access revocation, and approval boundaries. Implementation choices belong in technical design.

**NFR-04. Untrusted input handling.** Pages, research, uploads, and tool results are data, not instructions. They must not authorize tools, knowledge overwrites, budget increases, or disclosure. Validate fetch URLs and guard against internal-network requests and malicious redirects. Do not copy private business data into public searches because a source requests it. Parse spreadsheets without executing macros/formulas; protect exported spreadsheets from formula injection. Validate structured model outputs before persistence or action.

**NFR-05. Durable execution and recovery.** Research, imports, tasks, and scheduled publishing must remain traceable through ordinary navigation and worker interruptions. Preserve approvals and attempt/outcome history; test retries, concurrent workers, cancellation races, revocation, uncertain provider responses, and overdue work. Do not blindly replay external operations after recovery or backup restore. Recovery must revalidate authority and reconcile uncertain outcomes. Backup/restore and deployment/rollback procedures must be exercised before release.

**NFR-06. Resource accountability.** Monitor actual model/tool/search/integration usage per task and workspace, including failed calls, retries, and child work. Enforce limits and concurrency reservations independently of agents; record budget approvals. Declare task-specific latency, timeout, usage, and concurrency ceilings at a stated release load, then measure them (§10). Internal resource authorization is distinct from customer charging. Cost per useful cycle must include repeated work and relevant service costs, not only successful model calls.

**NFR-07. Accessible bilingual operation.** Critical mobile and keyboard journeys, RTL/LTR/mixed-script layouts, forms/errors, graph/list interactions, Canvas/events, dates, currencies, and approvals must be understandable in the selected language. Verify country/timezone handling, seasonal clock changes, independent language settings, and all four content registers. The owner must be able to identify precisely what will happen, where, and when. English or Egyptian Arabic success does not establish Gulf-oriented quality.

**NFR-08. Observability.** Operators must be able to investigate failures, source gaps, stuck jobs, approval transitions, publishing attempts, revocation, and cost spikes using real records. Provide actionable owner errors and recovery runbooks, with a small internal view/logging path sufficient for the release. Logs and user-facing activity must redact secrets and avoid unnecessary private data or private model reasoning. Operational telemetry must remain separate from claimed customer business performance.

**NFR-09. Data control.** Export, deletion, correction, and disconnection must cover product data, integrations, derived retrieval records, and affected artifacts. Define active-data deletion, backup expiry, sensitive-data handling, token lifecycle, and the minimal retained audit metadata before release. Explain retention limits accurately; do not promise immediate deletion from every backup if unavailable. Restore procedures must respect prior deletion/disconnection decisions. Prefer aggregate outcome data over storing customer contacts.

## 9. Phase 2 acceptance scenarios

The following are mandatory product-level release checks. Detailed fixtures and test cases belong in the evaluation/engineering plans and use the gates in §10. A claimed capability must be exercised against its real implementation, not a visual stand-in. Preserve evidence against each applicable FR/NFR using the crosswalk in §14.

| ID | Scenario | Observable pass condition |
|---|---|---|
| AC-01 | Website onboarding | An owner imports an accessible site; usable business and brand context appears in the Brain with sources and status; the owner can enter the Director without mandatory field review |
| AC-02 | No-website onboarding | Minimal guided context, including unknown audience/baseline, leads to a concise next action and useful artifact without forcing complete economics or a website |
| AC-03 | Brain governance | The owner explores and edits the graph, distinguishes imported from confirmed or inferred items, and approves, edits, rejects, exports, or deletes applicable information |
| AC-04 | Diagnosis and roadmap | BUSMORA explains its evidence-linked constraint hypothesis, material alternatives, key missing evidence, why-now priority, capacity/dependencies, and next action; tasks have completion/review criteria; revisions require review |
| AC-05 | Direct work | The owner launches a supported task from chat without completing preceding roadmap tasks; the task reuses relevant Brain context and history |
| AC-06 | Dynamic orchestration | Simple input avoids fabricated specialist work; complex tasks vary planning/delegation with context inside shared tool/iteration/time/concurrency limits; ordinary UI actions do not require AI routing |
| AC-07 | Agent Canvas | The owner sees genuine task events, sources, handoffs, failures, and input requests and can inspect useful details; displayed progress matches recorded execution |
| AC-08 | Multi-source research | Research dynamically examines distinct relevant supported source types, verifies claim support, distinguishes duplicated sources, exposes gaps/contradictions, and links implications to an actionable next task; access/stopping limits produce honest partial results |
| AC-09 | Research budget | Parent, child, concurrent and retried work stay within the authorized allowance; attempted increases pause for owner approval; refusal preserves a bounded result without tool/task budget evasion |
| AC-10 | Campaign creation | The owner can request either a single item or an editable text-first campaign, with brand-aware copy, channel details, visual briefs, and owner media when needed |
| AC-11 | Language experience | The complete application works in Arabic RTL and English LTR; separate interface, conversation, and content-language choices work across the agreed registers |
| AC-12 | Facebook publishing | An authorized Page accepts each committed format after exact individual/batch approval; a scheduled job publishes for real, retains platform evidence, and handles retries without duplicate publication; unsupported/missing media is rejected before approval |
| AC-13 | Approval invalidation | Editing approved content, media, destination, time/timezone, or delivery allowance prevents dispatch under the old approval until the new details are approved |
| AC-14 | Performance learning | Both a valid CSV and a valid Excel file pass mapping/validation and deterministic calculation against a goal; observations and interpretations remain distinct; unusable layouts produce a useful template/manual fallback; roadmap and Brain changes require separate review |
| AC-15 | History and control | Deliverables and task history are retrievable and reusable; the owner can export or delete them and disconnect a linked account |
| AC-16 | Honest availability | Social Profile Import is visibly disabled and labelled Coming Soon; Instagram Business, X, connected analytics, native image generation, and billing are not represented as operational until implemented |
| AC-17 | Operational readiness | Security, isolation, integration, failure and recovery, observability, and end-to-end checks pass against the implemented workflows |
| AC-18 | New service business with unknown audience/baseline | Unknown/not-applicable remains explicit; propose useful evidence or offer work without invented audience proportions, CAC, or growth forecasts |
| AC-19 | Many inquiries, few qualified conversations | Investigate qualification, offer, response process, and alternative explanations before automatically recommending more posting or spend |
| AC-20 | Two owner hours per week and no test budget | Active work fits the capacity or exposes the conflict; no unsupported multi-channel plan or paid execution; acquisition is not pushed when fulfillment is already full |
| AC-21 | No website but working messaging/marketplace route | The actual route informs the plan; no unnecessary site requirement or simulated social import |
| AC-22 | Direct request for one Arabic post | Produce a scoped, brand-aware draft without a mandatory campaign or unapproved research; unsupported claims are omitted, qualified, or queried |
| AC-23 | Offer/price changes after approval | Affected work becomes stale and conflicting scheduled posts are held; owner resolution is recorded; edits/rescheduling require fresh approval; unrelated work remains usable |
| AC-24 | Best task needs an unsupported connector | Supply usable owner instructions/artifacts and a completion checklist; distinguish attested implementation from system-verified execution |
| AC-25 | Unknown outcomes, zero denominator, duplicated or incompatible data | Unknown never becomes zero; invalid ratios are withheld; repeated/overlapping totals, currencies and metric definitions are flagged before combining; provenance/period remain inspectable |
| AC-26 | Two work/review cycles with an inconclusive result | Today view surfaces next work/review; observations, hypotheses and decisions remain distinct; cadence fits the sales cycle; owner-reviewed revisions or further evidence follow without a false causal winner |
| AC-27 | Contradictory or malicious public source | Conflict remains visible; source instructions cannot authorize tools, override knowledge, raise budgets, leak private data, or redirect retrieval to internal resources |
| AC-28 | Timeout, revocation, cancellation race, or overdue publish | Reconcile or hold uncertain outcomes; no blind duplicate, unauthorized late dispatch, or silent send after reconnection; cancellation state and platform acceptance remain honest |
| AC-29 | Mobile mixed-script review and approval | Text/media, destination, date/time/timezone, status and controls are legible and usable in Arabic RTL and English LTR; keyboard/detail alternatives and independent language settings work |
| AC-30 | Intended ordinary owner connects and publishes | The production configuration supports the owner's actual access path and genuine publication; developer-role account success alone cannot satisfy the gate |

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
| Required behavior | All AC-01–30 pass under declared conditions, with detailed tests covering applicable FRs/NFRs | Native publishing, graph, Canvas, research, imports, and data controls use their real implementations |
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

Define activation as the owner accepting a useful next task and its initial artifact after seeing the direction and limitations. Track setup → first direction → accepted task → accepted artifact → recorded completion/attestation → observations or explicit no-data state → owner-reviewed next decision. Keep this usage funnel separate from customer business outcomes.

A **completed review cycle** contains an accepted task, recorded completion state, observations or explicit absence of data, and an owner-reviewed next decision. Report subsequent-cycle completion by activated-owner cohort, denominator, and review window; exclude team/test activity. Do not label this retention measure marketing ROI.

Measure major edits, rejected recommendations/reasons, context-entry effort, clarification burden, failed imports, uncertain publications, and actual cost per accepted artifact/completed cycle. Compare equivalent tasks with the owner's current process, such as general AI chat, spreadsheets, and native publishing, using comparable business facts/evidence. Repeat sessions to assess context reuse and correction. These studies measure usefulness and effort; small samples do not establish regional market demand or causal revenue lift.

## 11. Dependencies, risks, and technical investigations

### 11.1 Early decisions and risk owners

Assign actual team members to these workstream roles at kickoff. Deadlines are project-relative and do not imply that an investigation or approval has already occurred.

| Area / accountable role | Decision or evidence due | Required response / boundary |
|---|---|---|
| Launch cohort / product and evaluation lead | Weeks 1–2: reachable service niches, conversion paths, owner capacity, Facebook relevance, and separate GCC evaluation needs | Narrow the first-use promise without claiming validated demand or reducing language commitments |
| Facebook / integration lead | Start week 1; by week 2 document selected app configuration, required access, verification/review path, supported formats, and blockers; publish a real test as early as access permits | Submit for production access at the earliest eligible point; no promised approval duration; developer-only evidence cannot pass AC-30 |
| Research / AI lead with evaluation partner | By week 2: evaluated baseline for source access, regional relevance, grounding, stopping, provider cost/latency, and budget control | Preserve dynamic multi-source research inside supported access; do not promise private-source workarounds; finalize bounded tool/provider choices from evidence |
| Shared data and state / platform lead | Weeks 1–2: entity/evidence/task/metric/approval/version contracts and transition rules; integrate through weeks 3–4 | Graph, Canvas, chat, publishing, and review share records; no competing sources of truth or silent knowledge writes |
| Execution / platform and integration leads | Weeks 1–4: durable jobs/events, budget reservations, action preflight, retry/reconciliation, cancellation and context-hold design | Model decisions cannot override controls; ambiguous outcomes remain held rather than blindly repeated |
| Evaluation / evaluation lead | By week 2: rubrics, held-out coverage, severity definitions, sampling, load and numeric runtime limits; extend evidence with each slice | Do not invent success rates or lower gates to mask failures; arrange relevant native reviewers early |
| Data governance / platform lead | Initial design by week 4: retention, deletion/derived records, backup expiry/restore, export, logs, and token lifecycle | Test owner controls before release; retain only justified audit data and explain actual retention |
| Localization / UX lead | Weeks 1–2: shared bilingual components and review plan; continuous checks thereafter | Egyptian/MSA success does not establish GCC quality; missing review or mobile approval failures remain explicit gaps |
| Capacity / whole team | Week 1 availability check; week 2 and milestone re-estimates | Use actual productive hours and throughput; stop expansion and simplify within the contract before accepting a release shortfall |

Risk signals requiring corrective work include generic content-first plans across contrasting cases, repeated low-value clarifications/approvals, growing pending-update queues, weak Arabic source coverage, costly research loops, and graph/Canvas embellishment delaying an integrated owner journey. Address the underlying workflow, evaluation, or implementation limit before adding more generators or integrations.

### 11.2 Architecture and implementation boundaries

The default approach is one modular application with a shared system of record, file storage, and durable background execution, using established managed components where suitable. A relational store with explicit relationships can support the graph; a dedicated graph database, microservice fleet, custom agent framework, or streaming platform is not required. Provider/framework/database selections remain technical decisions.

Keep clear logical boundaries for context/evidence, tasks/artifacts/versions, proposals/decisions, approvals/integrations/attempts, calculations/reviews, and execution events/usage. Graph and Canvas render those records. A small set of research/analysis/drafting specialists may be selected dynamically; deterministic services own permissions, calculations, scheduling, imports, and side effects. Start with a small evaluated model/tool path and add complexity only for demonstrated quality/cost/latency benefit.

Supporting engineering designs must record concrete schemas, state transitions, source/format limits, deletion behavior, and failure tests. This section does not add public APIs, user-programmable workflows, or a separate research product.

## 12. Scope exclusions and unresolved commercial hypotheses

### 12.1 Explicit Phase 2 non-goals

| Excluded work | Phase 2 alternative / disposition |
|---|---|
| Operational Social Profile Import; Instagram Business/X publishing; connected analytics | Remain Phase 3; use guided context, owner examples, export, and manual/file observations |
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

Evaluate whether owners need direction more than additional content volume; whether the chosen service cohort shares a useful workflow; where qualified demand actually originates; whether manual observations are tolerable; whether Arabic flows improve comprehension; whether corrected context improves later work; and whether repeated value earns willingness to pay. Use real owner sessions when available, initially assisted and later without explanation, and compare with current workflows.

These are commercial hypotheses, not asserted facts or paid-pilot graduation prerequisites. Small samples can expose usability problems but cannot establish regional demand, retention, or causal growth. Choose packaging only after observing useful repeat behavior and service costs, including failed/repeated calls, infrastructure, and support. Weak repeat use should trigger examination of recommendations, feasibility, and measurement friction before adding channels or notifications.

## 13. Delivery and scope control

### 13.1 Capacity assumptions

Five people over 24 weeks provide 120 person-weeks of calendar allocation, not necessarily full-time engineering. The initial scenario is **20 productive hours per person per week**, or 2,400 hours; actual availability must be checked. At 15 hours the total is 1,800; at 25 it is 3,000. These are planning calculations, not verified estimates of implementation effort.

Reserve the final six weeks, approximately 600 hours in the 20-hour scenario, for integration failures, language/usability repair, security/recovery testing, and stabilization. The first 18 weeks have a 1,800-hour allocation including ongoing tests and supporting design work. No Phase 3/4 work is funded by this envelope.

| Shared workstream | Initial allocation | Accountable role |
|---|---:|---|
| Platform, evidence/Brain, approvals, data controls | 320 hours | Platform lead |
| Director, research, recipes, grounding, resource controls | 400 hours | AI lead with evaluation partner |
| Bilingual responsive UX, tasks, graph, Canvas | 380 hours | UX lead |
| Facebook access, scheduling, publishing and recovery | 260 hours | Integration lead |
| Imports, metrics, manual results, review | 220 hours | Evaluation/data lead |
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
| **5–8** | First Phase 2 slice: diagnosis/Now–Next–Later, direct chat, claim review, bounded graph/Canvas, exact approval, publishing worker | Owner corrects context, chooses a task, inspects work, reviews a draft and executes an approved supported test post; production-access submission starts as soon as eligible |
| **9–12** | Research quality/stopping, dynamic specialists, campaign/calendar review, owner handoffs, initial recipes, cost/error visibility | Research informs an actionable decision; budget/source failures return honest partial results; shared context works across views; early usability findings addressed |
| **13–16** | CSV/Excel mapping, deterministic metrics, manual outcomes/reviews, context staleness, deletion/export, publishing edge cases | Two cycles with actual entered/imported observations; controlled revisions; no fabricated result or automatic stale-content publication |
| **17–18** | Complete committed language/control paths; full acceptance suite; intended-owner access validation; feature freeze | Requirement ledger records implemented behavior and every unresolved failure; no feature inferred complete from UI alone |
| **19–22** | Reliability/fault injection, isolation/security, native review, mobile accessibility, cost/load checks, backup restore | Critical/high defects resolved; acceptance evidence and operator recovery paths complete |
| **23–24** | Release rehearsal, deployment/rollback, final relevant regressions, user/operator documentation and graduation evidence | Release only after binding gates pass; unmet production access or native capability remains a blocker |

External approvals have no guaranteed duration. Investigate from week 1 and submit at the earliest eligible point; do not schedule an approval outcome as predictable engineering work. Integrate weekly around one schema and end-to-end owner tasks. Five isolated subsystems do not constitute a completed release.

### 13.3 Release ledger and change control

Maintain a ledger for every FR/NFR and acceptance scenario with implementation reference, accountable person, verification evidence, and current status. Delivery priority is order of work, not permission to omit a commitment. Any new feature must identify its capacity source.

If the estimate exceeds capacity: stop later-phase/cosmetic work; simplify within the supported contract (focused graph views, fewer recipes, simpler calendar, bounded import layouts); reuse established components; re-estimate and expose the gap. Do not cut truthful state, authorization, data controls, recovery, required languages, or real native workflows. Changes to committed formats or behavior require an explicit documented release-contract decision. If a required capability remains unavailable, mark Phase 2 incomplete rather than labeling it done, beta, or replaced by manual handoff.

## 14. Requirement-to-acceptance crosswalk

This is the product-level starting map for the release ledger, not a report of tests already run. Detailed tests must cover each individual requirement and its failure paths; an AC association alone does not prove coverage.

| Requirements | Principal acceptance scenarios |
|---|---|
| FR-01–02 | AC-15, AC-17, AC-26, AC-29 |
| FR-03–07 | AC-01–02, AC-16, AC-18, AC-21, AC-27 |
| FR-08–13 | AC-03, AC-15, AC-23, AC-27 |
| FR-14–16 | AC-04, AC-18–21, AC-24, AC-26 |
| FR-17–18 | AC-05, AC-22 |
| FR-19–23 | AC-06–07, AC-09, AC-17 |
| FR-24–28 | AC-08–09, AC-18, AC-27 |
| FR-29–32 | AC-10–11, AC-22, AC-24 |
| FR-33–36 | AC-12–13, AC-23, AC-28–30 |
| FR-37 | AC-16, AC-20, AC-24, AC-27 |
| FR-38–40 | AC-14, AC-25–26 |
| FR-41–42 | AC-05, AC-15, AC-23, AC-26 |
| FR-43–44 | AC-11, AC-29 |
| FR-45 | AC-04, AC-18–21, AC-25 |
| FR-46 | AC-04, AC-20, AC-24, AC-26 |
| FR-47 | AC-25–26 |
| FR-48 | AC-03, AC-15, AC-23 |
| FR-49 | AC-16, AC-24 |
| NFR-01–02 | AC-03, AC-07–08, AC-16, AC-23, AC-26–27 |
| NFR-03–06 | AC-09, AC-12–13, AC-17, AC-25, AC-27–28 |
| NFR-07 | AC-11, AC-29 |
| NFR-08–09 | AC-15, AC-17, AC-23, AC-28 |
| P3-01–03, P4-01–02 | AC-16 checks honest Phase 2 availability only; later-phase operation requires its own real end-to-end evidence under §6 |



