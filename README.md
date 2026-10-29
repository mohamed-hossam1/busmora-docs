# BusMora — Project Documentation & Architecture

> **BusMora** is an autonomous **AI Marketing Director** platform designed for Arabic-first small and medium businesses (5–50 employees, service businesses and SMBs selling via inquiries/bookings) in Egypt and the GCC. It combines a reviewed **Business Brain** (relational concept graph + procedural rules) with an **autonomous execution layer** that diagnoses commercial constraints using the **5 STAR Lenses**, prioritizes an adaptive **Now/Next/Later Roadmap**, writes culturally authentic Arabic content across 4 registers, conducts dynamic research with per-task budgets, and safely publishes to social media under human approval.

---

## Repository Map

```
busmora-docs/
├── core/                         # The 5 Canonical Specifications
│   ├── prd.md                    # Product Requirements Document (PRD v1.1)
│   ├── DESIGN-SYSTEM.md          # Design Tokens, Components & RTL Layouts
│   ├── SYSTEM-ARCHITECTURE.md    # Canonical System Architecture v1.0
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
│   ├── business-model-value-proposition.md # 4-Pillar Moat & Phased Commercialization
│   ├── mvp-definition.md         # Phase 2 Release Contract (24 Weeks, 5 Students)
│   ├── evaluation.md             # 24-Case Scenario Library & AC-01–30 Release Gates
│   ├── decision-registry.md      # Authoritative Decision Registry (D-01 to D-60)
│   └── README.md
├── research/                     # Discovery, Benchmarks & Deep Analysis
│   ├── product/                  # Customer & Market Research
│   │   └── product-discovery-summary.md # Pain points, interviews, competition
│   ├── technical/                # Architectural Exploration & Trade-off Benchmarks
│   │   ├── 00-requirements-analysis.md # Historical audit & gap analysis
│   │   ├── 01-constraints.md     # Binding constraints & hard gates (G1–G5)
│   │   ├── 02-options.md         # Deep options analysis (B1–B3, FE-1–FE-3)
│   │   ├── 03-benchmark.md       # Objective benchmark & scoring matrix
│   │   └── 04-decision.md        # Architectural decision record (B1+A1, FE-1)
│   └── README.md
├── diagrams/                     # Architecture & Workflow Diagrams (Excalidraw)
│   ├── project-draw.excalidraw
│   └── README.md
├── presentation/                 # Stakeholder Pitch Decks
│   └── 1-BusMora.html
└── Vault/                        # Obsidian Knowledge Base & Execution Engine
    ├── 00 - Home.md
    ├── 01 - Initiatives/
    ├── 02 - Features/
    ├── 03 - Skills/
    ├── 04 - Rules/
    └── Templates/
```

---

## 1. Core Specifications (`core/`)

The 5 canonical documents defining the functional and architectural contract are in [**`core/`**](./core/README.md):

* [**Product Requirements Document (`prd.md`)**](./core/prd.md) — PRD v1.1: Authoritative release contract covering FR-01–49, NFR-01–09, and AC-01–30 for the 24-week Phase 2 release.
* [**Design System (`DESIGN-SYSTEM.md`)**](./core/DESIGN-SYSTEM.md) — Design tokens, component behaviors, typography scales, and strict RTL-first bidirectional layout rules.
* [**System Architecture v1.0 (`SYSTEM-ARCHITECTURE.md`)**](./core/SYSTEM-ARCHITECTURE.md) — 4 deployables, 8 process roles, Platform-Centred backend style, Amendment A1 (internal HTTP for quick replies $\le 3\text{s}$ and Brain search), multi-tenant isolation, and KMS envelope encryption.
* [**Backend Technical Design (`BACKEND-TECHNICAL-DESIGN.md`)**](./core/BACKEND-TECHNICAL-DESIGN.md) — NestJS modular monolith across 13 business modules, Drizzle ORM (sole writer), ambient Unit of Work, contract system, Temporal workflows, and read-only views with RLS for Python compute.
* [**Frontend Technical Design (`FRONTEND-TECHNICAL-DESIGN.md`)**](./core/FRONTEND-TECHNICAL-DESIGN.md) — Next.js 14+ client-rendered application (CSR) prepared for Workspace Event Shell (FE-3), feature-by-context boundaries, React Query caching, central security gateways, and RTL-first styling.

---

## 2. Domain-Driven Design (`domain/`)

The domain taxonomy and aggregate models are in [**`domain/`**](./domain/README.md):

* [**Ubiquitous Language & Glossary (`GLOSSARY.md`)**](./domain/GLOSSARY.md) — Canonical terminology, forbidden synonyms, and lifecycle statuses (AI Marketing Director, Now/Next/Later, STAR Lenses, Context Staleness Holds, Honest Handoffs).
* [**Bounded Contexts (`contexts.md`)**](./domain/contexts.md) — Clear boundaries and ownership across Core (Business Brain, Marketing Employee, Governed Publishing), Supporting, and Generic contexts.
* [**Domain Model (`model.md`)**](./domain/model.md) — Aggregates, entity lifecycles, and domain invariants including post `held` states and per-task research budgets.
* [**Domain Events (`events.md`)**](./domain/events.md) — Immutable domain event catalog, payload schemas, and transactional outbox conventions.

---

## 3. Product & Business Strategy (`strategy/`)

Business models, release boundaries, and decision records are in [**`strategy/`**](./strategy/README.md):

* [**Business Model & Value Proposition**](./strategy/business-model-value-proposition.md) — 4-pillar defensibility moat, competitive positioning, and phased commercialization.
* [**MVP Definition**](./strategy/mvp-definition.md) — Phase 2 release contract (24 weeks by 5 students across 5 workstreams, ~2,400 hours), 5 STAR diagnostic lenses, and 7 core user journeys.
* [**Evaluation Plan**](./strategy/evaluation.md) — 24-case scenario library with contrast pairs, 30 release acceptance gates (AC-01 through AC-30), Arabic quality rubrics across 4 registers, and blind Brain benchmark.
* [**Decision Registry**](./strategy/decision-registry.md) — Authoritative historical record of product and architecture decisions (D-01 to D-60).

---

## 4. Research & Benchmarks (`research/`)

Discovery findings and architecture trade-off exploration records are in [**`research/`**](./research/README.md):

* [**Product Discovery**](./research/product/product-discovery-summary.md) — The pain points ("Manual Work Multipliers", "Editing Trap", "Tool Amnesia"), customer profiles, and vision.
* [**Technical Research & Decisions**](./research/technical/) — 5-stage benchmark process: [00 Requirements Analysis](./research/technical/00-requirements-analysis.md), [01 Constraints & Criteria](./research/technical/01-constraints.md), [02 Architecture Options](./research/technical/02-options.md), [03 Options Benchmark](./research/technical/03-benchmark.md), and [04 Final Decision Record](./research/technical/04-decision.md).

---

## 5. Team Knowledge Base (`Vault/`)

The team uses the **Obsidian Vault** as an interactive knowledge and execution tracking layer:
* [**00 - Home.md**](./Vault/00%20-%20Home.md) — Entry point and navigation map.
* [**01 - Initiatives/**](./Vault/01%20-%20Initiatives/README.md) — High-level initiatives and PRD tracking.
* [**02 - Features/**](./Vault/02%20-%20Features/README.md) — Canonical feature overviews, requirements, and per-task Jira records.
* [**03 - Skills/**](./Vault/03%20-%20Skills/README.md) — Unified AI-agent skills catalog (engineering design, lifecycle, and review skills).
* [**04 - Rules/**](./Vault/04%20-%20Rules/README.md) — Global documentation lifecycle, team conventions, and the [End-to-End Task Execution Guide](./Vault/04%20-%20Rules/End-to-End%20Task%20Execution%20Guide.md).

---

## Key Architecture Highlights

1. **Two Standalone Repositories:** `busmora-backend` (NestJS, Temporal Python workers, FastAPI AI service) and `busmora-frontend` (Next.js web application), communicating strictly through versioned OpenAPI contracts.
2. **Platform-Centred with Sole Domain Writer:** `busmora-api` (NestJS) is the single owner and only writer of all PostgreSQL domain tables. Python services are stateless compute behind Temporal and never write domain tables.
3. **Dual-Memory Business Brain:** Semantic memory (PostgreSQL concept graph + Qdrant vectors) grounds factual knowledge; procedural memory grounds formatting and dialect style rules learned from human edits.
4. **Governed Safe Publishing:** The Social Poster agent acts only before approval; publishing is deterministic, byte-for-byte, idempotent dispatch through MCP tools in the worker tier, verified by reading the post back from the platform.
