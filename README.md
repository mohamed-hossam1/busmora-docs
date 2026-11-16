# BusMora — Project Documentation & Architecture

> **BusMora** is an autonomous **AI Marketing Director** platform designed for Arabic-first small and medium businesses (5–50 employees, service businesses and SMBs selling via inquiries/bookings) in Egypt and the GCC. After a short owner-confirmed setup, it combines a persistent **Business Brain** (relational concept graph + procedural rules) with independent **Director Chat** and a guided **Roadmap** of executable steps. The Director diagnoses the likely constraint, researches within per-task budgets, writes authentic content across 4 registers, keeps documents in Deliverables and Posts in Scheduling, and publishes to Facebook Pages only with exact owner approval.

---

## Repository Map

```
busmora-docs/
├── core/                         # The 5 Canonical Specifications
│   ├── prd.md                    # Product Requirements Document (PRD v2.0)
│   ├── DESIGN-SYSTEM.md          # Design Tokens, Components & RTL Layouts
│   ├── SYSTEM-ARCHITECTURE.md    # Canonical System Architecture v2.0
│   ├── BACKEND-TECHNICAL-DESIGN.md# Backend Monolith & Worker Design
│   ├── FRONTEND-TECHNICAL-DESIGN.md# Frontend CSR & Event Shell Design
│   └── README.md
├── domain/                       # Domain-Driven Design (DDD) Specifications
│   ├── GLOSSARY.md               # Ubiquitous Language & Terminology
│   ├── contexts.md               # Bounded Contexts & Subdomain Boundaries
│   ├── model.md                  # Tactical Domain Aggregates & Invariants
│   ├── events.md                 # Domain Events Catalog & Outbox Schemas
│   └── README.md
├── strategy/                     # Product, Business & Governance Strategy
│   ├── business-model-value-proposition.md # Value Proposition & Commercial Hypotheses
│   ├── mvp-definition.md         # Phase 2 Release Contract (24 Weeks, 5 Students)
│   ├── evaluation.md             # ~24-Case Scenario Library & PRD §10.2 Release Gates
│   ├── decision-registry.md      # Decision Registry (D-01 to D-70)
│   └── README.md
├── diagrams/                     # Architecture & Workflow Diagrams (Excalidraw)
│   ├── project-draw.excalidraw
│   └── README.md
├── presentation/                 # Stakeholder Pitch Decks
│   └── 1-BusMora.html
└── Vault/                        # Obsidian Knowledge Base & Execution Engine
    ├── 00 - Home.md
    ├── 01 - Rules/
    ├── 02 - Tasks/                # Execution history by Jira key
    ├── 03 - Features/             # Current business capabilities
    └── Templates/                # Task, research, benchmark, feature overview
```

---

## 1. Core Specifications (`core/`)

The 5 canonical documents defining the functional and architectural contract are in [**`core/`**](./core/README.md):

* [**Product Requirements Document (`prd.md`)**](./core/prd.md) — PRD v2.0: Authoritative release contract covering FR-01–58, NFR-01–09, and AC-01–58 (with FR-47 and AC-26 retired) for the 24-week Phase 2 release.
* [**Design System (`DESIGN-SYSTEM.md`)**](./core/DESIGN-SYSTEM.md) — Design tokens, component behaviors, typography scales, and strict RTL-first bidirectional layout rules.
* [**System Architecture v2.0 (`SYSTEM-ARCHITECTURE.md`)**](./core/SYSTEM-ARCHITECTURE.md) — 4 deployables, 8 process roles, Platform-Centred backend style, Amendment A1 (internal HTTP for quick replies $\le 3\text{s}$ and Brain search), multi-tenant isolation, and KMS envelope encryption.
* [**Backend Technical Design (`BACKEND-TECHNICAL-DESIGN.md`)**](./core/BACKEND-TECHNICAL-DESIGN.md) — NestJS modular monolith across 13 business modules, Drizzle ORM (sole writer), ambient Unit of Work, contract system, Temporal workflows, and read-only views with RLS for Python compute.
* [**Frontend Technical Design (`FRONTEND-TECHNICAL-DESIGN.md`)**](./core/FRONTEND-TECHNICAL-DESIGN.md) — Next.js 14+ client-rendered application (CSR) prepared for Workspace Event Shell (FE-3), feature-by-context boundaries, React Query caching, central security gateways, and RTL-first styling.

---

## 2. Domain-Driven Design (`domain/`)

The domain taxonomy and aggregate models are in [**`domain/`**](./domain/README.md):

* [**Ubiquitous Language & Glossary (`GLOSSARY.md`)**](./domain/GLOSSARY.md) — Canonical terminology, forbidden synonyms, and lifecycle statuses (AI Marketing Director, Onboarding Draft, Roadmap Discovery, Diagnostic Lenses, Context Staleness Holds, Honest Handoffs, ScheduledTime).
* [**Bounded Contexts (`contexts.md`)**](./domain/contexts.md) — Clear boundaries and ownership across Core (Business Brain, Marketing Employee, Governed Publishing), Supporting, and Generic contexts.
* [**Domain Model (`model.md`)**](./domain/model.md) — Aggregates, entity lifecycles, and domain invariants including post `held` states and per-task research budgets.
* [**Domain Events (`events.md`)**](./domain/events.md) — Immutable domain event catalog, payload schemas, and transactional outbox conventions.

---

## 3. Product & Business Strategy (`strategy/`)

Business models, release boundaries, and decision records are in [**`strategy/`**](./strategy/README.md):

* [**Business Model & Value Proposition**](./strategy/business-model-value-proposition.md) — Value proposition canvas, competitive positioning, defensibility and commercial hypotheses (no price or payment provider selected).
* [**MVP Definition**](./strategy/mvp-definition.md) — Phase 2 release contract (24 weeks by 5 students, ~2,400 hours), personas, and 7 core user journeys.
* [**Evaluation Plan**](./strategy/evaluation.md) — ~24-case scenario library with contrast pairs, PRD §10.2 release gates (AC-01–25, AC-27–58), Arabic quality rubrics across 4 registers, and blind Brain benchmark.
* [**Decision Registry**](./strategy/decision-registry.md) — Product and architecture decisions (D-01 to D-70) with superseded history.

---

## 4. Engineering Workflow and Vault

The engineer drives decisions; the AI challenges assumptions, checks system constraints before research, compares independently gathered candidates with the engineer, and benchmarks measurable claims in an isolated sandbox. Implementation uses acceptance-driven Red → Green → Refactor.

Start at [Vault Home](./Vault/00%20-%20Home.md) or the [Task Lifecycle Protocol](./Vault/01%20-%20Rules/Task%20Lifecycle%20Protocol.md).

- [Rules](./Vault/01%20-%20Rules/README.md): shared conventions and lifecycle milestones.
- [Tasks](./Vault/02%20-%20Tasks/README.md): one `Task.md` per Jira ticket, with optional `research.md`, `benchmark.md`, and `evidence/`.
- [Features](./Vault/03%20-%20Features/README.md): one living `Overview.md` per stable business capability. Technical tasks create no feature folder.
- [Templates](./Vault/Templates/README.md): four compact starting points.

Jira stays **To Do** during discovery, becomes **In Progress** immediately before implementation code/tests, and becomes **In Review** when the PR opens. **Done requires verified merge into main** of all required delivery PRs.

Experiment in git-ignored `scratch/benchmarks/<JIRA-KEY>/` with isolated dependencies; preserve useful reproducibility material in the task's optional evidence folder. Confirm ignoring in the repository where the experiment runs.

Workflow skills live once in `.agents/skills/`; `.claude/skills/` links to those sources. Implementation, conditional migration, delivery, and PR-review skills share the Vault protocol instead of maintaining competing lifecycle rules.

### Historical research

The retired top-level `research/` documents remain available in Git history at commit `7408b51703a8699bb9296439ffcb98e214902cc8`. For example:

```bash
rtk git show 7408b51703a8699bb9296439ffcb98e214902cc8:research/technical/01-constraints.md
```

The same commit contains the former `research/product/` and `research/technical/00`–`04` records. Historical source aliases **01** (constraints) and **04** (architecture decision) in core specifications refer to those records; they are historical rationale, not current product scope. New task research belongs with its Jira task. Current contracts remain in `core/`, `domain/`, and `strategy/`.

---

## Key Architecture Highlights

1. **Four Repositories:** `busmora-web` (Next.js), `busmora-api` (NestJS), `busmora-ai` (FastAPI, AI activities and Temporal Python workers), and `busmora-docs` (requirements, architecture, planning and evidence). Code repositories exchange versioned HTTP/activity contracts; the workers remain a separate deployable built from `busmora-ai`.
2. **Platform-Centred with Sole Domain Writer:** `busmora-api` (NestJS) is the single owner and only writer of all PostgreSQL domain tables. Python services are stateless compute behind Temporal and never write domain tables.
3. **Dual-Memory Business Brain:** Semantic memory (PostgreSQL concept graph + Qdrant vectors) grounds factual knowledge; procedural memory grounds formatting and dialect style rules learned from human edits.
4. **Governed Safe Publishing:** The Social Poster agent acts only before approval; publishing is deterministic, byte-for-byte, idempotent dispatch through MCP tools in the worker tier, verified by reading the post back from the platform.
