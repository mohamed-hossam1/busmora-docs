# Decision Registry

> A chronological record of the product and architecture decisions made during discovery and engineering baselining, with the rationale behind each.
>
> **Authority Precedence:** Current functional requirements and technical contracts are governed by **[core/prd.md](../core/prd.md)** (PRD v2.0) and the canonical specifications in `core/`. Every entry below is aligned with PRD v2.0; superseded earlier decisions are listed in §12 so their history stays visible.

---

## 1. Vision & Scope

| # | Decision | Rationale |
|---|---|---|
| D-01 | Build a focused **AI Marketing Director**, not a generic multi-employee platform | A single vertical can reach real depth, high strategic value, and demonstrable quality within the timeline. |
| D-02 | Foundational concepts stay **domain-neutral** (Employee/Director, Capability, Workflow, Integration, Task Record) | Keeps future expansion addable without redesign while shipping only the Marketing Director now. |
| D-03 | Prove extensibility by a **paper design exercise at architecture freeze** | Validates the seam without building admin/marketplace machinery or a second employee. |
| D-04 | **Phased Release Contract:** the 24-week commitment is **Phase 2 (First Production-Ready Release)** | Binds delivery to the PRD §3 capabilities: email-link and Google auth, confirmed website/manual intake, Brain Graph/List, independent Director Chat with conversation-local Canvas, adaptive Roadmap Discovery with complete actionable steps, research, document Deliverables, first-class Posts in Scheduling, real Facebook Page publishing, notifications, Settings and requested CSV/.xlsx analysis. Phase 3 (social link import, Instagram/X, connected analytics), Phase 4+ (AI image generation, billing) and Phase 5 (Meta Ads Management) consume no Phase 2 capacity. |
| D-05 | Team of **5 students over 24 weeks (~2,400 productive hours)** | 18 weeks feature development (1,800 h) + 6 weeks stabilization (600 h) across 5 designated workstreams (Platform, AI/Research, UX, Integration, Evaluation/data) (PRD §13.1). |

## 2. Market & Positioning

| # | Decision | Rationale |
|---|---|---|
| D-06 | Primary launch cohort: **Arabic-first owner-operated service businesses** selling through inquiries, consultations or bookings; experienced marketers are secondary users | A coherent inquiry-to-customer workflow; a product hypothesis, not a validated market-size conclusion (PRD §2.1). |
| D-07 | Full **RTL** Arabic and English UI; native content generation across 4 registers (Egyptian, Gulf-oriented, MSA, English) | RTL and dialect authenticity are core differentiators; each register needs suitable native review. |
| D-08 | **Continuous empirical evaluation over ~24 authored cases and all active acceptance scenarios (AC-01–25, AC-27–58); paid pilots are commercial learning, not graduation prerequisites** | Production readiness requires objective evaluation and functional acceptance gates; commercial validation is tracked separately (PRD §§10, 12.2). |
| D-09 | Customer discovery **paired with continuous evaluation** | Real owner sessions generate usability evidence while held-out test suites ensure robustness. |
| D-10 | Position as an Arabic-first **Marketing Director that diagnoses before it executes**, with **provenance-backed work and honest handoffs** | Owners need a defensible next action, not more content volume. Supported work is executed; unsupported external work gets owner-ready instructions and is never represented as done by BusMora (FR-14, FR-49). |
| D-10b | Design human review for **fast operational confidence** (Post Batch Review, Brain proposals, Roadmap revision approval) | Removes the "babysitting burden" while keeping every consequential action explicitly approved. |

## 3. Product Surface

| # | Decision | Rationale |
|---|---|---|
| D-11 | Responsibility areas: Diagnosis/Roadmap, Research, Content/Campaigns, Governed Publishing, Requested Analysis | Complete marketing-director surface without a recurring performance-review workflow (FR-58). |
| D-12 | Output types: versioned **document Deliverables** (research, strategy, analysis, briefs), the **Roadmap** (complete step plan), and Publishing-owned **Posts** | Documents and Posts have separate owners and homes (Deliverables vs. Scheduling); a Post never needs a document origin (FR-41, FR-57). |
| D-13 | **Phase 2 Publishing Channel: Facebook Pages only (text/link and single owner-supplied image)** | Bounded social scope ensures dependable delivery, reconciliation and read-back verification. Instagram Business and X are deferred to Phase 3 (P3-02). |
| D-14 | **Text-First Campaigns, Visual Briefs & Owner Media in Phase 2; Native AI Image Generation in Phase 4+** | Removes image-generation cost uncertainty and provider churn from the 24-week timeline while delivering immediate copy value. |
| D-15 | **Choice-first** structured forms (3–4 meaningful options + Other / Custom, with Unknown / Skip / Not Applicable) | Efficient gap filling without blocking useful work when details are unknown (FR-18). |
| D-16 | **Explicit Post state machine** with exact-version approval, holds, uncertain-outcome reconciliation and read-back verification | Safe automation with verifiable outcomes; no blind retry (FR-34–36). |
| D-17 | **Authentic React Flow Agent Canvas** plus accessible event list, scoped to the selected conversation's Task | Renders recorded execution events rather than simulated progress (FR-22). |
| D-18 | **Website or manual intake into one editable draft, activated only after exact-version owner confirmation; Social Profile Import disabled as Coming Soon** | Crawl observations and extracted proposals are never active Brain knowledge until the owner confirms the displayed draft; no duplicate initial candidate queue. Social Profile Link intake is Phase 3 (FR-03–07, P3-01). |
| D-19 | Append-only **audit log** with provenance, conflict, freshness, retirement and deletion handling | Governance and trust in the knowledge lifecycle, under the documented retention policy (FR-13, NFR-09). |

## 4. Business Brain

| # | Decision | Rationale |
|---|---|---|
| D-20 | **PostgreSQL relational concept graph** (`brain_nodes` / `brain_edges`) + **Qdrant** vectors | One source of truth, no separate graph database to operate. |
| D-21 | **Nine canonical entity types**: Company Profile, Offering, Audience, Brand Rule, Positioning, Strategic Goal, Channel, Competitor, Company Fact | Bounded, meaningful categories for grounding. |
| D-22 | Retrieval = **vector seed + 1–2 hop relational expansion**, always revalidated against PostgreSQL eligibility | Balances relevance with structured relationships; deleted/pending/rejected/superseded data never re-enters context. |
| D-23 | A **document-only control route** for measurement | Enables the Brain value comparison in `evaluation.md`. |
| D-24 | **Epistemic separation in Brain updates** | Confirmed intake and explicit owner edits are current owner-provided context; AI-proposed durable facts, interpretations and preferences enter Pending Brain Updates for individual approval; confirmation/approval is not independent verification (FR-09, FR-10). |
| D-25 | Brain hypothesis: **fewer clarification turns and factual/brand corrections** vs. the document-only control | Falsifiable measure of the core value claim; the target value is frozen with the evaluation rubric (PRD §10.2). |

## 5. Architecture

| # | Decision | Rationale |
|---|---|---|
| D-26 | **Four deployables** (`busmora-web`, `busmora-api`, `busmora-workers`, `busmora-ai`) running several process roles across **two standalone repositories** (`busmora-backend`, `busmora-frontend`) | Clean separation of user-facing API, worker scheduling, and AI workloads with clear repository ownership. |
| D-27 | **Temporal Cloud** for durable workflows | Managed reliability for long-running, human-in-the-loop tasks and month-long scheduled Posts. |
| D-28 | **Single region, Frankfurt (provisional)** | Managed hosting (Temporal Cloud, Langfuse Cloud) lacks MENA regions; initial owners do not require in-country residency (ADR-016). |
| D-29 | **Custom Argon2id auth** with JWT access tokens and Redis refresh rotation; **Google OIDC** added on the same session architecture | Full control over tenancy and session security (FR-50). |
| D-30 | **Cloud KMS envelope encryption** for OAuth credentials | Protects credentials at rest and in use. |
| D-31 | **LiteLLM** gateway with an **Arabic bake-off** and cost ceilings | Provider flexibility with quality and cost control. |
| D-32 | **Scrapling retrieval adapter** retained; public search/discovery provider chosen by an early evidence-backed spike | Tailored extraction for accessible sources without promising comprehensive or private access (FR-24, OD-22). |
| D-33 | **Langfuse** plus **suitable human reviewers** (two, tie-break by a third; regional review for Gulf content) | Observability and independent quality scoring per register. |
| D-34 | **SSE with Last-Event-ID replay** (via Redis Streams) for the canvas | Simple, reconnectable streaming without client state in the AI service. |

## 6. Monetization

| # | Decision | Rationale |
|---|---|---|
| D-35 | **No in-product billing in Phase 2; per-workspace usage & cost metering from day one** | Billing is Phase 4+ with no provider or price selected; Stripe does not serve Egypt/KSA merchants directly; real usage data informs later packaging (ADR-017, P4-02). |
| D-36 | Tiers **re-derived from real API and compute costs** | Prevents high-volume operations from running a tier underwater. |

## 7. Evaluation

| # | Decision | Rationale |
|---|---|---|
| D-37 | Evaluation is **human-conducted** over a ~24-case scenario set with frozen rubrics | Repeatable, evidence-based quality assessment across held-out contrast pairs. |
| D-38 | Brain value measured by **blind treatment-vs-control** comparison | Isolates the contribution of grounding. |

## 8. Defensibility & Advanced Agent Architecture

| # | Decision | Rationale |
|---|---|---|
| D-43 | **Dual-Memory Partitioning (Semantic vs. Procedural Memory)** in Business Brain | Separates business facts (Semantic) from owner-approved working rules such as dialect and style (Procedural). |
| D-44 | **Marketing Director Paradigm (Diagnosis Over Agent Sprawl)** | One Director diagnoses the business constraint before selecting a small set of dynamic specialists; agent count is a technical choice (FR-20–21). |
| D-45 | **Customer-language extraction from accessible public sources** | Preserves original wording, source and date for buyer vernacular; public comments are sampled observations, not survey results (FR-25). |
| D-46 | **Anti-Goodhart Governance via explicit approvals** | Prevents optimizing misleading proxy metrics; completion, publication and commercial impact stay distinct. |
| D-47 | **Platform-Centred Backend (NestJS as Sole Writer)** | `busmora-api` owns all domain state and migrations; Python services are stateless compute behind Temporal (ADR-003). |
| D-48 | **Internal HTTP for Quick Replies and Brain Retrieval (Amendment A1)** | Two synchronous internal endpoints (quick reply ≤ 3s and Brain search) bypass Temporal latency while keeping data ownership in api (ADR-004). |
| D-49 | **Deterministic Publishing Without LLM After Approval** | Agents only draft; publishing is deterministic execution via MCP tools in `workers-publish` (ADR-005). |
| D-50 | **Approval Lifecycle Bound to Post Versions & Execution Details** | Approval binds to an immutable `PostVersion`, destination, `ScheduledTime` and authorization generation; any critical change creates a new version and needs fresh approval (ADR-006, FR-34). |
| D-51 | **Two Standalone Repositories (`busmora-backend` & `busmora-frontend`)** | Clarifies ownership, separates frontend dependencies, enforces API contract boundaries via OpenAPI and `oasdiff` (ADR-002). |
| D-52 | **Single Migration Toolchain (Drizzle ORM)** | `busmora-api` manages all PostgreSQL tables; Python reads eligible context via read-only views with RLS (ADR-008). |
| D-53 | **Client-Rendered Application (FE-1) Prepared for Workspace Event Shell (FE-3)** | Next.js App Router for routing/layout only; data via React Query and feature modules with public `index.ts` interfaces (ADR-009). |
| D-54 | **Meta Tester Mode for development & AC-30 Production Gate** | Development uses Meta Tester Mode; the Phase 2 release requires an ordinary owner to connect and publish through the production configuration (ADR-018, AC-30). |

## 9. Core Contracts (introduced in v1.1, retained in v2.0)

| # | Decision | Rationale |
|---|---|---|
| D-55 | **Per-Task Research Budget & Mandatory Approval for Extensions (FR-27)** | Every research task receives an authorized time/tool/compute ceiling; child work inherits the budget; any extension requires explicit owner approval. |
| D-56 | **Requested one-off CSV + Excel `.xlsx` analysis (FR-38–40)** | Only on an explicit Chat request; template mapping preview, deterministic calculation in application code, one-off analytical document; no recurring capture, dashboard or automatic plan revision. |
| D-57 | **Context Staleness & Freshness Holds (FR-48)** | Material changes to core business facts version the context, flag dependent work, and hold only conflicting scheduled Posts until owner resolution. |
| D-58 | **Honest Capability & Unsupported Handoff Contract (FR-49)** | Unsupported external actions generate ready-to-use artifacts, instructions, and completion checklists; owner-reported completion stays labelled. |
| D-59 | **Five diagnostic lenses as guidance (FR-14)** | Offer/audience, destination, conversion, acquisition and existing-customer opportunities, constrained by capacity/fulfillment; useful guidance, not a numeric score or mandatory worksheet. |
| D-60 | **~24-Case Evaluation Suite & PRD §10.2 release gates** | Held-out contrast pairs; ≥80% pass without major correction overall and per register; zero critical truthfulness/control violations; all active AC-01–25 and AC-27–58; ten-minute first-value usability target. |

## 10. PRD v2.0 Redesign Decisions

| # | Decision | Rationale |
|---|---|---|
| D-61 | **Email-link verification + Google OIDC** (code flow, state, PKCE S256, nonce); identity is issuer + subject; never merge on matching email | Secure low-friction signup without account takeover (FR-50, ADR-012). |
| D-62 | **Minimal essential intake** (name, description, offerings, country/service area, conversion route state) with optional pricing models | Onboarding gathers no strategy; strategy moves to Roadmap Discovery (FR-06, ADR-021). |
| D-63 | **Independent Chat and Roadmap** after setup; normal login opens `/employee`, no last-route restoration | Chat never requires a Roadmap; owners choose their path (FR-02, FR-05, FR-17). |
| D-64 | **Saved adaptive Discovery with exact-question research return** | Durable, backend-validated linkage; query parameters are never authority (FR-51–52). |
| D-65 | **One active complete step Roadmap** with immutable versions, drafts, archived history and approved revision diffs | Real dependencies block execution; Skip/Revisit and owner-reported completion are distinct from verified completion (FR-16, FR-53–54, ADR-022). |
| D-66 | **Two running Tasks per Workspace** with atomic, FIFO admission | Bounded cost/concurrency across conversations, with truthful waiting state (FR-53, AC-41). |
| D-67 | **Posts are Publishing-owned and live in Scheduling** (Month/Week/List/Unscheduled, manual New Post, undated drafts) | One authoritative copy; no document-to-Post auto-creation (FR-55, FR-57, ADR-023). |
| D-68 | **Conversation-local Canvas; recoverable retries stay Processing** | Truthful comprehension without retry noise; terminal failure surfaced honestly (FR-22–23, ADR-024). |
| D-69 | **Retire Today Dashboard, Outcomes & Reviews and Now/Next/Later** with no replacement | Removes recurring review work from the release; FR-47 and AC-26 reserved as retired (FR-58). |
| D-70 | **Phase 5 Meta Ads Management as future vision only** | No current Ads connector, tool or spend permission (P5-01, AC-58). |

## 11. Deferred Provider Selections

| # | Item | Default | Decision Point |
|---|---|---|---|
| D-39 | Embedding model | OpenAI `text-embedding-3-small`-class | Week 1–2 bake-off |
| D-40 | Image generation model (Phase 4+) | Hosted text-to-image | Phase 4 planning |
| D-41 | Object storage provider | S3-compatible (R2 / MinIO) | Week 1–2 |
| D-42 | Rotating proxy vendor | Managed residential proxy provider | After the research provider spike (OD-22) |

## 12. Superseded Decisions (history)

| Earlier decision | Superseded by |
|---|---|
| One release with full scope; 4 students | D-04, D-05 (phased contract, 5 students) |
| Paid pilot with 1–3 businesses as a goal | D-08 (paid pilots are commercial learning only) |
| Facebook + Instagram + X publishing; brand-template image composites | D-13, D-14 |
| 10-page onboarding crawl creating individually reviewed candidates | D-18, D-62 (confirmed draft intake) |
| Performance CSV diagnosis; v1.1 manual aggregate outcome capture (FR-47) | D-56, D-69 |
| v1.1 Now/Next/Later roadmap and Today Dashboard | D-65, D-69 |

---
