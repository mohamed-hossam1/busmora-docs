# Decision Registry

> A chronological record of the product and architecture decisions made during discovery and engineering baselining, with the rationale behind each. This is the authoritative reference for the current design.

---

## 1. Vision & Scope

| # | Decision | Rationale |
|---|---|---|
| D-01 | Build a focused **AI Marketing Director**, not a generic multi-employee platform | A single vertical can reach real depth, high strategic value, and demonstrable quality within the timeline. |
| D-02 | Foundational concepts stay **domain-neutral** (Employee/Director, Capability, Workflow, Integration, Task Record) | Keeps future expansion addable without redesign while shipping only the Marketing Director now. |
| D-03 | Prove extensibility by a **paper design exercise at architecture freeze** | Validates the seam without building admin/marketplace machinery or a second employee. |
| D-04 | **Phased Release Contract:** 24-week release commitment is **Phase 2 (First Production-Ready Release)** | Binds delivery to core capabilities (guided/website intake, Brain graph, diagnosis, Now/Next/Later roadmap, research, text-first campaigns, Facebook publishing, CSV/.xlsx learning). Phase 3 (Instagram/X, social link import, connected analytics) and Phase 4+ (AI image gen, billing) remain in vision without consuming Phase 2 capacity. |
| D-05 | Team of **5 students over 24 weeks (~2,400 productive hours)** | 18 weeks feature development (1,800 h) + 6 weeks stabilization (600 h) across 5 designated workstreams (Platform, AI/Research, UX, Integration, Evaluation). |

## 2. Market & Positioning

| # | Decision | Rationale |
|---|---|---|
| D-06 | **Arabic-first** SMBs and owner-operated services selling via inquiries/bookings (5–50 employees) | The measured wedge is Arabic quality, where global tools are weakest. |
| D-07 | Full **RTL** Arabic and English UI; native content generation across 4 registers (Egyptian, Gulf, MSA, English) | RTL and dialect authenticity are core differentiators. |
| D-08 | **Continuous empirical evaluation over 24 authored cases & AC-01–30; paid pilots are commercial learning, not graduation prerequisites** | Academic graduation and production readiness require objective evaluation and functional acceptance gates; commercial revenue validation is tracked separately. |
| D-09 | Customer discovery **paired with continuous evaluation** | Real owner sessions generate usability evidence while held-out test suites ensure robustness. |
| D-10 | Escape the "Manual Work Multiplier" and "Generic Context" traps by positioning as an **Autonomous Execution Layer** with **Arabic dialect authenticity + provenance-backed execution** | Ideation tools cause churn due to the "editing trap" and tool-hopping; owners require end-to-end execution without manual handoffs. |
| D-10b | Design human review for **fast operational confidence** (Review & Approval UI) | Eliminates the "babysitting burden" that plagues autonomous agents while guaranteeing safety. |

## 3. Product Surface

| # | Decision | Rationale |
|---|---|---|
| D-11 | Five responsibility areas: Research, Strategy/Planning, Content, Performance, External Execution | Complete marketing-director surface. |
| D-12 | Five core artifact types: Research Report, Now/Next/Later Roadmap, Content Package, Performance Diagnosis, Review & Approval Card | Concrete deliverables the owner can inspect, reuse, and act on. |
| D-13 | **Phase 2 Publishing Channel: Facebook Pages only (text/link and single owner-supplied image)** | Bounded social scope ensures rock-solid delivery, idempotent dispatch, and read-back verification. Instagram Business and X publishing are deferred to Phase 3 (P3-02). |
| D-14 | **Text-First Campaigns, Visual Briefs & Owner Media in Phase 2; Native AI Image Generation in Phase 4+** | Eliminates image-generation cost uncertainty and provider churn from the 24-week timeline while delivering immediate copy value. |
| D-15 | Structured **forms + free text** for progressive clarification | Efficient gap filling without blocking useful work when details are unknown. |
| D-16 | **Simplified approval state machine** + idempotency + read-back verification | Safe automation with verifiable outcomes. |
| D-17 | Full **authentic React Flow Agent Canvas** plus detail list | Renders recorded execution events and tools rather than simulated progress, building owner trust. |
| D-18 | **Immediately usable imported website context; Social Profile Import disabled as Coming Soon** | Website crawls populate active, source-labelled imported/unverified context without blocking field reviews. Social Profile Link intake is deferred to Phase 3 (P3-01). |
| D-19 | Delegated **audit log** with provenance, conflict, freshness, and retirement handling | Governance and trust in the knowledge lifecycle. |

## 4. Business Brain

| # | Decision | Rationale |
|---|---|---|
| D-20 | **PostgreSQL relational concept graph** (`brain_nodes` / `brain_edges`) + **Qdrant** vectors | One source of truth, no separate graph database to operate. |
| D-21 | **Nine canonical entity types**: Company Profile, Offering, Audience, Brand Rule, Positioning, Strategic Goal, Channel, Competitor, Company Fact | Bounded, meaningful categories for grounding. |
| D-22 | Retrieval = **vector seed + 1–2 hop relational expansion** | Balances relevance with structured relationships. |
| D-23 | A **document-only control route** for measurement | Enables the Brain value comparison in `evaluation.md`. |
| D-24 | **Epistemic separation in Brain updates** | Website import is active with an imported/unverified designation; AI-proposed persistent facts enter Pending Brain Update; owner edits take immediate effect; Post approvals bind to exact versions. |
| D-25 | Brain hypothesis: **≥ 50% fewer clarification turns and factual/brand corrections** vs. control | Falsifiable measure of the core value claim. |

## 5. Architecture

| # | Decision | Rationale |
|---|---|---|
| D-26 | **Four deployables** (`busmora-web`, `busmora-api`, `busmora-workers`, `busmora-ai`) running 8 process roles across **two standalone repositories** (`busmora-backend`, `busmora-frontend`) | Clean separation of user-facing API, worker scheduling, and AI workloads with clear repository ownership. |
| D-27 | **Temporal Cloud** for durable workflows | Managed reliability for long-running, human-in-the-loop tasks. |
| D-28 | **Single region, Frankfurt (provisional)** | Managed hosting (Temporal Cloud, Langfuse Cloud) lacks MENA regions; pilot customers do not require in-country residency (ADR-016). |
| D-29 | **Custom Argon2id auth** with JWT access tokens and Redis refresh rotation | Full control over tenancy and session security. |
| D-30 | **Cloud KMS envelope encryption** for OAuth credentials | Protects credentials at rest and in use. |
| D-31 | **LiteLLM** gateway with an **Arabic bake-off** and cost ceilings | Provider flexibility with quality and cost control. |
| D-32 | **Custom Scrapling crawlers** with proxy and headless-browser pools | Tailored extraction for the required sources. |
| D-33 | **Langfuse** plus **two human reviewers** (tie-break by a third) | Observability and independent quality scoring. |
| D-34 | **SSE with Last-Event-ID replay** (via Redis Streams) for the canvas | Simple, reconnectable streaming without client state in the AI service. |

## 6. Monetization

| # | Decision | Rationale |
|---|---|---|
| D-35 | **No in-product billing this release; per-workspace usage & cost metering from day one** | Paid pilot is charged externally; Stripe does not serve Egypt/KSA merchants directly; real usage data feeds post-pilot pricing tiers (ADR-017, SC-2, P4-02). |
| D-36 | Tiers **re-derived from real API and compute costs** | Prevents high-volume operations from running a tier underwater. |

## 7. Evaluation

| # | Decision | Rationale |
|---|---|---|
| D-37 | Evaluation is **human-conducted** over a 24-case scenario set with published rubrics | Repeatable, evidence-based quality assessment across held-out contrast pairs. |
| D-38 | Brain value measured by **blind treatment-vs-control** comparison | Isolates the contribution of grounding. |

## 8. Defensibility & Advanced Agent Architecture

| # | Decision | Rationale |
|---|---|---|
| D-43 | **Dual-Memory Partitioning (Semantic vs. Procedural Memory)** in Business Brain | Solves brand drift by separating factual entities (Semantic) from persistent formatting and dialect rules (Procedural). |
| D-44 | **Gen-3 Marketing Director Paradigm (Diagnosis Over Agent Sprawl)** | Replaces chaotic 16-agent swarms with one unified Director that diagnoses business constraints before allocating execution. |
| D-45 | **Intent Qualification Pipeline for Social Mining** | Filters noisy social scrapes into high-intent signals (competitor complaints, price questions) to feed genuine buyer vernacular into copy. |
| D-46 | **Anti-Goodhart Governance via Review & Approval** | Prevents autonomous agents from optimizing for misleading proxy metrics by preserving human strategic oversight. |
| D-47 | **Platform-Centred Backend (NestJS as Sole Writer)** | `busmora-api` owns all domain state and migrations; Python services are stateless compute behind Temporal (ADR-003). |
| D-48 | **Internal HTTP for Quick Replies and Brain Retrieval (Amendment A1)** | Two synchronous internal endpoints (quick reply ≤ 3s and Brain search) bypass Temporal latency while keeping data ownership in api (ADR-004). |
| D-49 | **Deterministic Publishing Without LLM After Approval** | Social Poster agent acts only before approval; publishing is byte-for-byte idempotent execution via MCP tools in `workers-publish` (ADR-005). |
| D-50 | **Approval Lifecycle Bound to Post Versions & Execution Details** | Approval binds to an immutable `PostVersion`, scheduled time/timezone, and lateness allowance; adding `cancelled` and `expired` states; optimistic concurrency on versions (ADR-006, FR-34). |
| D-51 | **Two Standalone Repositories (`busmora-backend` & `busmora-frontend`)** | Clarifies ownership, separates frontend dependencies, enforces API contract boundaries via OpenAPI and `oasdiff` (ADR-002, R-5). |
| D-52 | **Single Migration Toolchain (Drizzle ORM)** | `busmora-api` manages all PostgreSQL tables; Python reads approved knowledge via read-only views with RLS (ADR-008). |
| D-53 | **Client-Rendered Application (FE-1) Prepared for Workspace Event Shell (FE-3)** | Next.js App Router for routing/layout only; data via React Query and modular features with public `index.ts` interfaces (ADR-009). |
| D-54 | **Meta Tester Mode for Pilot & AC-30 Production Gate** | Initial development uses Meta Tester Mode; Phase 2 release gate AC-30 requires verifying production configuration for ordinary owners without developer roles (ADR-018, AC-30). |

## 9. PRD v1.1 Refinements & Core Contracts

| # | Decision | Rationale |
|---|---|---|
| D-55 | **Per-Task Research Budget & Mandatory Approval for Extensions (FR-27)** | Every research task receives an authorized time/tool/compute ceiling; child work inherits the budget; any extension requires explicit owner approval. |
| D-56 | **Dual-Format Performance Ingestion (CSV + Excel `.xlsx`) & Manual Aggregate Entry (FR-38, FR-47)** | Ingests CSV and Excel `.xlsx` files with a schema validation preview; deterministic calculation in application code; manual aggregate outcome capture for businesses without ad spreadsheets. |
| D-57 | **Context Staleness & Freshness Holds (FR-48)** | Material changes to core business facts (pricing, offers, service scope) version the context, flag dependent active tasks, and place scheduled posts on hold until owner resolution. |
| D-58 | **Honest Capability & Unsupported Handoff Contract (FR-49)** | Unsupported external actions generate ready-to-use artifacts, instructions, and completion checklists; system never claims external execution occurred without proof. |
| D-59 | **5 STAR Diagnostic Lenses & Adaptive Now/Next/Later Roadmap (FR-14, FR-16, FR-45, FR-46)** | Diagnoses bottlenecks across Situation/Offer, Traffic/Acquisition, Activation/Conversion, Retention/Referral, and Operations/Fulfillment; manages prioritized Now/Next/Later roadmap. |
| D-60 | **24-Case Evaluation Suite & 30 Release Acceptance Gates (AC-01–30)** | Evaluated across 24 authored cases with contrast pairs; ≥80% pass rate without major edits across 4 registers; 0 critical violations in safety/truthfulness; 10-minute first-value usability target. |

## 10. Deferred Provider Selections

| # | Item | Default | Decision Point |
|---|---|---|---|
| D-39 | Embedding model | OpenAI `text-embedding-3-small`-class | Sprint 1 bake-off |
| D-40 | Image generation model (Phase 4+) | Hosted text-to-image behind brand template | Phase 4 planning |
| D-41 | Object storage provider | S3-compatible (R2 / MinIO) | Sprint 1 |
| D-42 | Rotating proxy vendor | Managed residential proxy provider | Sprint 3 |

---
