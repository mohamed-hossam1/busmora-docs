# BusMora: consolidated project brief for the next Wayfinding phase

## Purpose of this file

This is the single working context for the next Wayfinding phase. It consolidates the research and decisions reached so far, identifies what remains unresolved, and corrects an earlier mistake: the prior map resolved the product and domain direction, but did **not** produce a build-ready architecture or technology decision.

Treat the settled decisions below as constraints unless new evidence makes revisiting one necessary. The next phase must turn them into a concrete, technically feasible MVP plan for a four-student team over eight months. It should make justified technology choices rather than deferring the technology stack again.

## Project in one paragraph

BusMora is an AI-assisted business-work system for companies that need recurring marketing capacity but cannot justify a full-time marketing hire. Its MVP is one broad but bounded, company-specific **Marketing Employee**. The Employee uses reviewed company context and reusable capabilities to research, plan, create content, interpret performance, and prepare or perform supported external actions under human approval. It is not a single fixed workflow, a promise to do all marketing, or a generic agent-builder platform.

## Constraints and operating assumptions

- Team: four students.
- Time available: eight months.
- First product: a Marketing Employee, not a general multi-agent platform.
- Build and validate a credible MVP before expanding to Sales, Operations, or customer-created Employees.
- Meaningful external side effects require human approval.
- The product must be able to add a later controlled Employee without a foundational domain-model redesign, but no second Employee needs to be implemented now.
- Product value and customer evidence matter more than architectural novelty.

## Customer-discovery starting point

### Priority cohort

Begin external discovery with digital-first B2B professional-services firms with 10–49 employees, favouring 20–49. A qualified company has:

- a public website and differentiated offer;
- a named owner/operator or lean marketer responsible for marketing;
- an active content or outbound channel;
- at least two relevant sources among CRM/analytics, email, social, paid media, or ecommerce catalogue;
- read-only, non-sensitive performance data it can share; and
- no dedicated multi-person marketing department.

### Comparator cohort

Recruit a smaller comparable group of digital-commerce brands. It is a falsifying comparator, not an assumed target segment. Commerce provides strong attribution but has stronger native AI substitutes, integration needs, and execution expectations.

### Discovery before pilots

Run three to five discovery conversations in each cohort. Score recurring work volume across research/content/campaign/analysis, current substitute spend, available data sources, decision-maker access, willingness to approve work, and measurable leading/outcome metrics. Choose pilots only after that screen. Do not start with companies whose main need is autonomous ad spend or unapproved publishing.

### Competitive reality

The relevant substitutes are employees, agencies, freelancers, marketing automation, platform-native AI, and general AI plus company documents. Shared company context is therefore not a differentiator by itself. BusMora must prove a better work outcome: traceable, approval-governed, context-aware coordination that reduces human clarification and correction on a meaningful recurring job.

## Product contract: the Marketing Employee

### What it is

The Marketing Employee is a user-facing, company-specific worker with a bounded portfolio of recurring responsibilities. It selects and composes enabled capabilities according to a request. It is not a workflow-specific automation, an autonomous publisher, or a claim to perform every marketing activity.

### Contracted responsibility areas

1. **Research & Intelligence** — competitor, market, and audience research.
2. **Strategy & Planning** — marketing, campaign, and content plans and recommendations.
3. **Content Production & Adaptation** — brand-grounded copy, content, and supported channel adaptations.
4. **Performance Interpretation** — reporting, diagnosis, and optimisation recommendations.
5. **External Execution** — preparation, approval, execution, and verification of supported external actions through integrations.

### Explicit first-release exclusions

- Sales or CRM operations.
- Financial commitments.
- Unapproved high-impact communications.
- Arbitrary integrations.
- Self-modification or runtime agent-code generation.
- A generic visual agent-builder or customer-created Employee types.

### Competence contract

Research, planning, and performance work must produce a **Decision-Ready Artifact**: grounded in available company context and relevant sources; clear about facts versus inference and material uncertainty; sufficient for a human to make the next business decision without redoing the work.

Content and external-execution work must produce an **Approval-Ready Action**: explicit about proposed output/action, target, and relevant context; requiring approval before meaningful side effects; limited to the approved supported action; and verified where technically possible.

Every responsibility area must reuse confirmed applicable context, ask only materially necessary questions, avoid inventing or contradicting confirmed facts, preserve relevant context across multi-area work, and disclose missing information, unsupported capability, and tool failure.

### Representative evaluation suite

The suite samples the bounded contract; it is not an exhaustive definition of marketing.

1. Competitor-and-audience intelligence brief.
2. Marketing/campaign plan from a stated goal.
3. Brand-grounded content package with channel adaptations.
4. Performance report with diagnosis and recommendations.
5. Prepare, approve, and perform a supported external action.
6. Research → plan → content.
7. Plan → content → approval-gated execution.
8. Performance interpretation → revised plan/content.

Where more than one relevant integration is supported, external-execution evaluation should cover more than one. Selected scenarios have matched repeated-use versions.

## Domain and orchestration decisions

### Core concepts

- **Employee**: a user-facing, company-specific worker abstraction. The MVP supports only the Marketing Employee.
- **Employee Configuration**: the particular Employee's instructions and contracted scope; enabled compatible capabilities; permitted integrations/actions and limits; approval requirements; and applicable knowledge access. It excludes company knowledge, credentials, templates, and delegation topology.
- **Capability**: a small, reusable, separately evaluable business ability with a clear input/output and meaningful safety or permission boundary. It is neither a Responsibility Area nor an implementation-level helper.
- **Workflow**: an explicit bounded sequence used only where repeatable state, control, auditability, retries, or an approval boundary justify it.
- **Tool**: a bounded callable operation a capability may use. It may be internal or supplied through an integration.
- **Integration**: the governed connection to an external system, including external identity, authorization, supported actions, and available verification.
- **Task Record**: the retained account of a task, including artifacts, approvals, execution result, feedback, and references to material effective revisions.

### MVP capability candidates

- Grounded research and synthesis.
- Plan/recommendation formation.
- Brand-grounded content adaptation.
- Performance diagnosis.
- External-action preparation.
- Approved execution and verification.

These are a provisional catalog. The next phase must decide the concrete first implementation subset and their interfaces/evaluations.

### One explicit workflow category

The MVP has one workflow category: **approval-governed external action**.

`prepare → approve → execute → verify`

Research, planning, content, performance, and non-executing multi-area work remain flexible capability composition even when they have several sequential steps. The workflow begins at the meaningful external side-effect boundary. Platform-specific behaviour belongs at the Integration/action boundary, not in a separate workflow per platform.

### Deliberate non-concepts for the MVP

- Specialist and Sub-agent are not first-class domain concepts. Internal delegation may be an implementation technique, but creates no user-facing/configurable entity, permission model, or separate evaluation contract.
- Employee Template is deferred.
- Employee Role is not a first-class MVP entity. The product supports one controlled Marketing Employee; a later Employee can introduce a controlled contract without forcing a generic Role layer now.
- MCP is an implementation protocol/adapter option, not a product-domain concept or required architecture.

## Business Brain contract

### Knowledge boundary

The **Business Brain** is company-scoped, provenance-bearing **Canonical Company Knowledge** that authorized Employees may reuse. It is not a transcript store, generic document index, or unreviewed automatic memory.

- **Canonical Company Knowledge**: reviewed durable information, such as confirmed facts, brand rules, audiences, offerings, approved strategic decisions, standing preferences, and reviewed corrections.
- **Evidence and Provenance**: attached metadata—the source, author, freshness, confidence, applicability, and, where relevant, scope/effective timeframe.
- **Task-Local Context**: a task request, temporary assumptions, working notes, retrieved material, drafts, and intermediate reasoning; private to the task by default.
- **Task Record**: completed artifact, approvals, execution outcome, observations, and feedback; not shared knowledge by default.
- **Candidate Knowledge**: a task-derived fact, observation, correction, decision, or outcome proposed for review before it becomes canonical.

The only promotion path is:

`Task/Observation → Candidate Knowledge → evaluation/confidence → authorized confirmation/promotion → Canonical Company Knowledge`

Drafts, model inferences, imported integration data, observations, and unreviewed outcomes never write directly to canonical knowledge.

### Governance, conflicts, and freshness

An **Authorized Company Human** alone may promote, edit, supersede, retire, or change applicability of canonical knowledge. Employees create candidates with evidence. Canonical changes retain actor, time, evidence, reason, and prior state where applicable.

Candidates never silently overwrite canonical knowledge. A human may reject the candidate, supersede/revise the existing item, retain both under different scopes, or leave the conflict unresolved. Material unresolved conflicts must be shown as uncertainty or trigger clarification; the MVP does not automate conflict resolution.

Canonical knowledge does not auto-expire. Time-sensitive items may have a review-by date or effective period. Superseded items remain auditable but are not current outside their valid scope/effective period. Stale or narrowly scoped knowledge must never silently be presented as current/applicable.

### Sharing behaviour

An authorized Marketing Employee uses relevant canonical knowledge permitted by its configuration together with provenance, applicability, freshness, and uncertainty. It must not receive another worker's task-local context, private drafts, hidden reasoning, credentials, or unrestricted task records. “Shared” means relevant authorized knowledge is available when needed—not that the entire Brain is injected into every task.

Applicability and authorization are distinct. The MVP remains company-scoped and defers per-item permissions, department/role scope administration, direct task-record sharing, and a dedicated Knowledge Access Scope entity.

### Business Brain hypothesis

The narrow near-term claim is:

> Reviewed canonical context can reduce clarification, factual/brand correction, and rework on comparable repeated tasks for the same company, without lowering contract quality or creating material onboarding/review burden.

Compare ordinary document retrieval (control) with reviewed, provenance-linked canonical context (treatment), holding model, Employee contract, capabilities, tools, sources, and task shape constant. Track clarification quantity/quality, material correction effort, human contract score, grounding/brand contradictions, rework, and context onboarding/maintenance effort.

The diagnostic target is at least 50% fewer clarification turns and factual/brand corrections with no material quality regression. This is not proof of customer value. Cross-Employee transfer, sustained customer benefit, ROI, and superiority in real customer environments remain unproven and require dedicated later evidence.

## Internal quality gate

The internal gate establishes contract compliance and controlled-pilot readiness only. It cannot establish product-market fit, willingness to pay, ROI, real-world Brain superiority, or cross-Employee transfer.

### Corpus

Use three curated **Evaluation Companies**, each with a hidden **Truth Packet** of confirmed facts, sources, brand rules, task data, and realistic ambiguity/incompleteness.

- 48 breadth runs: eight representative scenarios × three companies × two independent runs.
- 36 repeated-use runs: three multi-area scenarios × three companies × baseline/accumulated-context conditions × two runs per condition.
- Total: 84 assessed runs.

Three companies expose obvious overfitting but are not generalization evidence. Preserve task/source conditions across repeated-use pairs, changing reviewed context as the intended variable.

### Gate evidence and review

Every **Assessed Run** retains input, output, full trace, metrics, and review evidence. Primary dimensions are hard-rule compliance, human contract score, material grounding/brand/context error and correction effort, and task outcome. Clarification quality, latency, calls, tool failures, retries, cost, and repeated-use delta are diagnostic evidence.

Hard rules are deterministic. Two independent human reviewers are the release authority; LLM-as-judge may assist diagnosis but is never the sole release authority. Mandatory rubric dimensions are scenario-specific and each reviewer scores `0 = material contract failure`, `1 = material correction needed or insufficient evidence`, or `2 = meets contract`. Treatment/control review is blinded where practical.

If material disagreement or insufficient evidence prevents a defensible pass, a third reviewer examines the retained evidence. Original reviews remain recorded. If evidence remains insufficient, the run is a non-pass.

### Pilot-readiness thresholds

All of these must hold:

- Zero hard-rule violations across 84 runs.
- At least 76 of 84 passing runs (90.48%).
- At least 80% passing within each applicable Responsibility Area and each composite task shape; denominators derive from the frozen corpus mapping.
- Both initial reviewers assign `2` to every applicable mandatory dimension for a human-quality pass.
- No accepted run has a material grounding, brand, or context contradiction.
- Each repeated-use pair has no hard-rule violation, no lower median human contract score, and no higher median material-correction effort in the accumulated-context condition than baseline.

Systematic clustering by scenario, capability, integration, or behaviour triggers engineering review even if aggregate thresholds pass.

### Failure attribution

Record every failure with observed failure; primary suspected source; optional contributors; trace evidence; attribution confidence; and whether alternatives can be distinguished. Allowed source categories are Employee policy/configuration, capability/workflow, tool/integration, company context, orchestration, model variability/provider, evaluation fixture/data, and Unknown. Attribution is an evidence-backed hypothesis, not false-precision root-cause certainty.

## Platform boundary and extension rule

The MVP is a controlled Marketing Employee product, not a generic builder. Product/Engineering controls the Employee contract and responsibility boundary, capability implementations/revisions, workflow/safety rules, integration/action contracts and hard limits, mandatory approvals, and evaluation/release criteria.

Company administrators configure the supported Marketing Employee: bounded instructions/scope, compatible capabilities, supported integrations, permitted actions within hard limits, stricter approvals, and Candidate Knowledge review/promotion. They cannot create arbitrary agent types, upload agent code, design unrestricted delegation graphs, weaken hard safety rules, or bypass approval/evaluation controls.

Task Records retain identifiable references to the material revisions that determined their outcomes: Employee Configuration, enabled capabilities, workflow/control boundary when used, relevant integration/action contract, and canonical knowledge revisions/identifiers where practical. A configuration change creates a new revision; historical records retain their original references. Customer-selectable pinning, arbitrary downgrade, and sophisticated rollout management are deferred.

The extension criterion is a controlled product release for a future Sales or Operations Employee that adds its contract, capabilities, integrations/actions, justified workflows, evaluations, and Business Brain content **without redesigning** Employee Configuration, Capability, Workflow, Integration, Task Record, approval/execution semantics, or Business Brain—and without a Marketing-specific exception in these foundational concepts. Test this initially with a paper-design exercise, not an early second-Employee implementation.

## What the next Wayfinding phase must decide

The earlier decision map was incomplete for a build-ready MVP. The next phase must resolve at least the following, in dependency order.

1. **MVP user journeys and onboarding** — company setup, source ingestion, initial canonical-knowledge review, Employee configuration, task submission, review/approval, feedback/correction, and task history.
2. **Concrete first capability and integration subset** — choose the smallest set that can credibly exercise the contract and representative suite; define what is intentionally unsupported.
3. **Approval-governed external-action lifecycle** — action states, approvals/rejections/edits, idempotency, retries, partial success, verification, audit trail, and human recovery.
4. **Authorization, tenant isolation, and secret handling** — company/user boundaries, administrator/approver roles, integration credentials, access enforcement, and direct Task Record access policy.
5. **Business Brain implementation and review flow** — ingestion, Candidate Knowledge generation/review, provenance/conflict/freshness representation, retrieval behaviour, and how ordinary retrieval control is kept comparable for experiments.
6. **Technical architecture and stack** — frontend, backend, data/storage, authentication, queue/background execution, model/provider strategy, retrieval/search, integration adapters, observability, deployment, and local/developer environment. Select technologies against team skill, eight-month feasibility, evaluation needs, cost, security, and vendor lock-in.
7. **Evaluation and observability implementation** — traces, revision references, evaluator workflow, corpus/Truth Packet storage, deterministic assertions, human-review capture, cost/latency measurement, and regression execution.
8. **MVP delivery plan** — thin vertical slices, dependencies, milestones, ownership across four students, risks, test strategy, pilot-readiness gate, and customer-validation plan.
9. **Future-Employee paper test** — use a controlled hypothetical Sales or Operations Employee to test the platform boundary after the MVP architecture is proposed; revise foundational concepts only if the paper test exposes a real incompatibility.

## Non-goals for the next phase

- Do not turn BusMora into a generic agent-code generator, generic visual agent builder, or unrestricted multi-agent system.
- Do not assume the Business Brain is a differentiated feature before the paired repeated-work experiment supports it.
- Do not claim product-market fit, ROI, cross-Employee transfer, or broad generalization from the internal quality gate.
- Do not commit to full autonomous publishing, financial actions, or arbitrary integrations.
- Do not build Sales or Operations before Marketing is externally validated.


## Copyable message for Wayfinder

> I want you to act as the Wayfinder for the next, technical planning phase of BusMora. Treat `BUSMORA-WAYFINDING-BRIEF.md` as the single working context and source of settled product/domain constraints. Do not reopen a settled decision unless new evidence creates a concrete conflict.
>
> The previous Wayfinding work resolved product direction but stopped short of a build-ready MVP plan. Your objective is to produce a validated, explicit technical and delivery decision record for a four-student team with eight months: concrete MVP user journeys; the smallest capability/integration subset; approval/execution semantics; security and tenancy; Business Brain implementation/review; evaluation/observability; a justified technology stack and architecture; and a phased delivery/pilot-validation plan.
>
> Keep BusMora a controlled Marketing Employee product, not a generic agent builder. Make technology choices using evidence and practical trade-offs. Preserve the existing approval, provenance, evaluation, and extensibility constraints. Work in a dependency-aware decision map, research facts rather than asking me for them, and batch all currently answerable questions together. End with a build-ready plan and explicitly list any remaining assumptions or risks.
