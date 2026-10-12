# Decision Registry

> A chronological record of the product and architecture decisions made during discovery, with the rationale behind each. This is the authoritative reference for the current design.

---

## 1. Vision & Scope

| # | Decision | Rationale |
|---|---|---|
| D-01 | Build a focused **Marketing Employee**, not a generic multi-employee platform | A single vertical can reach real depth and demonstrable quality within the timeline. |
| D-02 | Foundational concepts stay **domain-neutral** (Employee, Capability, Workflow, Integration, Task Record) | Keeps a future Employee addable without redesign while shipping only the Marketing Employee now. |
| D-03 | Prove extensibility by a **paper design exercise at architecture freeze** | Validates the seam without building admin/marketplace machinery or a second Employee. |
| D-04 | Ship **one release with full scope** rather than deferring features | The user rejected priority stacking; the architecture supports the full product from day one. |
| D-05 | Team of **4 students over 6 months (24 weeks)** | Fixed constraint; addressed with vertical end-to-end ownership. |

## 2. Market & Positioning

| # | Decision | Rationale |
|---|---|---|
| D-06 | **Arabic-first** SMBs (e-commerce and local services, 5–50 employees) | The measured wedge is Arabic quality, where global tools are weakest. |
| D-07 | Full **RTL** Arabic and English UI; Arabic native content generation | RTL is table stakes for an Arabic-first product. |
| D-08 | **Paid pilot** with 1–3 businesses | Validates willingness to pay before wider rollout. |
| D-09 | Customer discovery **doubles with the paid pilot**; validated via continuous social sentiment mining | Real usage generates the evidence while social mining keeps us grounded in founder pain points. |
| D-10 | Escape the "Manual Work Multiplier" and "Generic Context" traps by positioning as an **Autonomous Execution Layer** with **Arabic dialect authenticity + provenance-backed execution** | Ideation tools cause churn due to the "editing trap" and tool-hopping; founders require end-to-end execution without manual handoffs. |
| D-10b | Design human review for **15-minute batch operational wins** (Batch Review UI) | Eliminates the "babysitting burden" that plagues autonomous agents while guaranteeing enterprise-grade safety. |

## 3. Product Surface

| # | Decision | Rationale |
|---|---|---|
| D-11 | Five responsibility areas: Research, Strategy/Planning, Content, Performance, External Execution | Complete marketing-employee surface. |
| D-12 | Five artifacts: Research Brief, Campaign/Content Plan, Content Package, Performance Diagnosis, Approval Card | Concrete deliverables the user can consume and act on. |
| D-13 | Channels: Facebook Pages, Instagram Business, X, with **Temporal scheduling** | Meets users where they already market; avoids restricted channels (LinkedIn, TikTok). |
| D-14 | Instagram images are **brand-template composites** (generated imagery + deterministic brand overlay) | Reliable on-brand visuals without unbounded generation cost. |
| D-15 | Structured **forms + free text** for clarification | Efficient gap filling with the flexibility to elaborate. |
| D-16 | **Simplified approval state machine** + idempotency + read-back verification | Safe automation with verifiable outcomes. |
| D-17 | Full **animated React Flow agent canvas** plus dual-pane layout | Transparency into autonomous work builds trust. |
| D-18 | **10-page onboarding crawl** bound | Bootstraps the Brain quickly without unbounded crawling. |
| D-19 | Delegated **audit log** with provenance, conflict, freshness, and retirement handling | Governance and trust in the knowledge lifecycle. |

## 4. Business Brain

| # | Decision | Rationale |
|---|---|---|
| D-20 | **PostgreSQL relational concept graph** (`brain_nodes` / `brain_edges`) + **Qdrant** vectors | One source of truth, no separate graph database to operate. |
| D-21 | **Nine canonical entity types**: Company Profile, Offering, Audience, Brand Rule, Positioning, Strategic Goal, Channel, Competitor, Company Fact | Bounded, meaningful categories for grounding. |
| D-22 | Retrieval = **vector seed + 1–2 hop relational expansion** | Balances relevance with structured relationships. |
| D-23 | A **document-only control route** for measurement | Enables the Brain value comparison in `evaluation.md`. |
| D-24 | **Individual-only** candidate review (no bulk approval) | Forces deliberate knowledge quality. |
| D-25 | Brain hypothesis: **≥ 50% fewer clarification turns and factual/brand corrections** vs. control | Falsifiable measure of the core value claim. |

## 5. Architecture

| # | Decision | Rationale |
|---|---|---|
| D-26 | **Four services**: `busmora-web` (Next.js), `busmora-api` (NestJS), `busmora-workers` (Temporal), `busmora-ai` (FastAPI) | Clean separation of user-facing API and AI workloads within a manageable count. |
| D-27 | **Temporal Cloud** for durable workflows | Managed reliability for long-running, human-in-the-loop tasks. |
| D-28 | **Managed/hybrid hosting**, region matched to the pilot (MENA) | Balances operational load with latency and data-residency expectations. |
| D-29 | **Custom Argon2id auth** with JWT access tokens and Redis refresh rotation | Full control over tenancy and session security. |
| D-30 | **Cloud KMS envelope encryption** for OAuth credentials | Protects credentials at rest and in use. |
| D-31 | **LiteLLM** gateway with an **Arabic bake-off** and cost ceilings | Provider flexibility with quality and cost control. |
| D-32 | **Custom Scrapling crawlers** with proxy and headless-browser pools | Tailored extraction for the required sources. |
| D-33 | **Langfuse** plus **two human reviewers** (tie-break by a third) | Observability and independent quality scoring. |
| D-34 | **SSE with Last-Event-ID replay** (via Redis Streams) for the canvas | Simple, reconnectable streaming without client state in the AI service. |

## 6. Monetization

| # | Decision | Rationale |
|---|---|---|
| D-35 | **Stripe** tiered subscriptions per Workspace, plus a flat paid pilot | Predictable SMB pricing aligned to "fractional hire" framing. |
| D-36 | Tiers **re-derived from real API and compute costs** | Prevents high-volume channels from running a tier underwater. |

## 7. Evaluation

| # | Decision | Rationale |
|---|---|---|
| D-37 | Evaluation is **human-conducted** over a fixed scenario set with published rubrics | Repeatable, evidence-based quality assessment. |
| D-38 | Brain value measured by **blind treatment-vs-control** comparison | Isolates the contribution of grounding. |

## 8. Defensibility & Advanced Agent Architecture

| # | Decision | Rationale |
|---|---|---|
| D-43 | **Dual-Memory Partitioning (Semantic vs. Procedural Memory)** in Business Brain | Solves brand drift by separating factual entities (Semantic) from persistent formatting and dialect rules (Procedural). |
| D-44 | **Gen-3 Marketing Employee Paradigm (Diagnosis Over Agent Sprawl)** | Replaces chaotic 16-agent swarms with one unified employee that diagnoses business constraints before allocating execution. |
| D-45 | **Intent Qualification Pipeline for Social Mining** | Filters noisy social scrapes into high-intent signals (competitor complaints, price questions) to feed genuine buyer vernacular into copy. |
| D-46 | **Anti-Goodhart Governance via Batch Approval** | Prevents autonomous agents from optimizing for misleading proxy metrics (cheap clicks) by preserving human strategic oversight. |

## 9. Deferred Provider Selections

| # | Item | Default | Decision Point |
|---|---|---|---|
| D-39 | Embedding model | OpenAI `text-embedding-3-small`-class | Sprint 1 bake-off |
| D-40 | Image generation model | Hosted text-to-image behind the brand template | Sprint 1 bake-off |
| D-41 | Object storage provider | S3-compatible (R2 / MinIO) | Sprint 1 |
| D-42 | Rotating proxy vendor | Managed residential proxy provider | Sprint 3 |

---
