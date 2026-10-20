# BusMora — Project Documentation & Architecture

> **BusMora** is an autonomous AI Marketing Employee platform designed for Arabic-first small and medium businesses (5–50 employees) in Egypt and the GCC. It combines a reviewed **Business Brain** (knowledge graph + style memory) with an **autonomous execution layer** that researches markets, writes culturally authentic Arabic content, plans campaigns, and safely publishes to social media under human batch approval.

---

## Repository Map

```
busmora-docs/
├── project-docs/                 # Product discovery, PRD, business model & evaluation
│   ├── product-discovery-summary.md
│   ├── business-model-value-proposition.md
│   ├── prd.md
│   ├── mvp-definition.md
│   ├── evaluation.md
│   ├── decision-registry.md
│   └── README.md
├── architecture/                 # System architecture, technical designs & domain model
│   ├── SYSTEM-ARCHITECTURE.md
│   ├── BACKEND-TECHNICAL-DESIGN.md
│   ├── FRONTEND-TECHNICAL-DESIGN.md
│   ├── domain/                   # Domain-Driven Design (Glossary, Contexts, Models, Events)
│   ├── decisions/                # Requirements analysis, constraints, options & benchmarks
│   └── README.md
├── diagrams/                     # Design canvases & architectural diagrams
│   ├── project-draw.excalidraw
│   └── README.md
├── presentation/                 # Product pitch deck & presentation slides
│   └── 1-BusMora.html
└── Vault/                        # Obsidian team workspace (Initiatives, Features, Skills, Rules)
    ├── 00 - Home.md
    ├── 01 - Initiatives/
    ├── 02 - Features/
    ├── 03 - Skills/
    └── 04 - Rules/
```

---

## 1. Product & Strategy Specifications (`project-docs/`)

The product strategy and functional specifications are maintained in [**`project-docs/`**](./project-docs/README.md):

* [**Product Discovery Summary**](./project-docs/product-discovery-summary.md) — The pain points ("Manual Work Multipliers", "Editing Trap", "Tool Amnesia"), beachhead customer profile, and product vision.
* [**Business Model & Value Proposition**](./project-docs/business-model-value-proposition.md) — 4-pillar defensibility moat, competitive positioning, and cost-derived pricing tiers.
* [**Product Requirements Document (PRD v2.0)**](./project-docs/prd.md) — Core functional (FR) and non-functional (NFR) requirements, sub-agent capabilities, and acceptance criteria.
* [**MVP Definition**](./project-docs/mvp-definition.md) — 24-week full-scope release boundaries, Sarah persona, and core user journeys.
* [**Evaluation Plan**](./project-docs/evaluation.md) — Rubrics for Arabic fluency, dialect fit (Egyptian, Gulf, MSA), blind Brain treatment-vs-control testing, and execution reliability.
* [**Decision Registry**](./project-docs/decision-registry.md) — Authoritative historical record of product and architecture decisions (D-01 to D-54).

---

## 2. Technical Architecture & Design (`architecture/`)

The system architecture and internal engineering designs are maintained in [**`architecture/`**](./architecture/README.md):

* [**System Architecture v1.0**](./architecture/SYSTEM-ARCHITECTURE.md) — The canonical system-level architecture: 4 deployables, 8 process roles, Platform-Centred backend style, Amendment A1 (internal HTTP for quick replies ≤ 3s and Brain search), multi-tenant isolation, and KMS envelope encryption.
* [**Backend Technical Design**](./architecture/BACKEND-TECHNICAL-DESIGN.md) — Internal design of `busmora-backend`: NestJS modular monolith across 13 business modules, Drizzle ORM (sole writer), ambient Unit of Work, contract system, Temporal workflows, and read-only views with RLS for Python compute.
* [**Frontend Technical Design**](./architecture/FRONTEND-TECHNICAL-DESIGN.md) — Internal design of `busmora-frontend`: Next.js 14+ client-rendered application (CSR) prepared for Workspace Event Shell (FE-3), feature-by-context boundaries, React Query caching, central security gateways, and RTL-first styling.
* [**Domain-Driven Design (`domain/`)**](./architecture/domain/) — Canonical [Glossary](./architecture/domain/GLOSSARY.md), 11 [Bounded Contexts](./architecture/domain/contexts.md), tactical [Domain Model](./architecture/domain/model.md), and [Domain Events](./architecture/domain/events.md).
* [**Architecture Decisions & Benchmarks (`decisions/`)**](./architecture/decisions/) — Evidence-based records: requirements analysis, constraints, options benchmarking (B1–B3, FE-1–FE-3), and final adoption decisions.

---

## 3. Team Knowledge Base (`Vault/`)

The team uses the **Obsidian Vault** as an interactive knowledge and execution tracking layer:
* [**00 - Home.md**](./Vault/00%20-%20Home.md) — Entry point and navigation map.
* [**01 - Initiatives/**](./Vault/01%20-%20Initiatives/README.md) — High-level initiatives and PRD tracking.
* [**02 - Features/**](./Vault/02%20-%20Features/README.md) — Canonical feature overviews, requirements, and per-task Jira records.
* [**03 - Skills/**](./Vault/03%20-%20Skills/README.md) — AI-agent implementation and PR review workflow skills.
* [**04 - Rules/**](./Vault/04%20-%20Rules/README.md) — Global documentation lifecycle and team conventions.

---

## Key Architecture Highlights

1. **Two Standalone Repositories:** `busmora-backend` (NestJS, Temporal Python workers, FastAPI AI service) and `busmora-frontend` (Next.js web application), communicating strictly through versioned OpenAPI contracts.
2. **Platform-Centred with Sole Domain Writer:** `busmora-api` (NestJS) is the single owner and only writer of all PostgreSQL domain tables. Python services are stateless compute behind Temporal and never write domain tables.
3. **Dual-Memory Business Brain:** Semantic memory (PostgreSQL concept graph + Qdrant vectors) grounds factual knowledge; procedural memory grounds formatting and dialect style rules learned from human edits.
4. **Governed Safe Publishing:** The Social Poster agent acts only before approval; publishing is deterministic, byte-for-byte, idempotent dispatch through MCP tools in the worker tier, verified by reading the post back from the platform.
