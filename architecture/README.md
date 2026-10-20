# BusMora Architecture & Technical Design

Welcome to the technical architecture directory for **BusMora** (AI Marketing Employee Platform). This directory contains the complete architectural specifications, internal technical designs, domain models, and the decision records that formed the system.

---

## Directory Structure

```
architecture/
├── SYSTEM-ARCHITECTURE.md        # Canonical system architecture v1.0
├── BACKEND-TECHNICAL-DESIGN.md   # Internal design for busmora-backend (NestJS + Python)
├── FRONTEND-TECHNICAL-DESIGN.md  # Internal design for busmora-frontend (Next.js)
├── domain/                       # Domain-Driven Design (DDD) model & language
│   ├── GLOSSARY.md               # Canonical ubiquitous language
│   ├── contexts.md               # Bounded contexts & boundaries
│   ├── model.md                  # Aggregates, entities & value objects
│   └── events.md                 # Domain events catalog
└── decisions/                    # The benchmark & architecture decision process
    ├── 00-requirements-analysis.md
    ├── 01-constraints.md
    ├── 02-options.md
    ├── 03-benchmark.md
    └── 04-decision.md
```

---

## 1. Core Technical Specifications

These documents form the **accepted build baseline** for developers:

| Specification | Target Repository / Scope | Key Highlights |
|---|---|---|
| [**SYSTEM-ARCHITECTURE.md**](./SYSTEM-ARCHITECTURE.md) | Whole System (`busmora-web`, `api`, `workers`, `ai`) | 4 deployables, 8 process roles, Platform-Centred style (`busmora-api` as sole domain writer), Amendment A1 (internal HTTP for quick replies ≤ 3s and Brain search), multi-tenant isolation, KMS encryption. |
| [**BACKEND-TECHNICAL-DESIGN.md**](./BACKEND-TECHNICAL-DESIGN.md) | `busmora-backend` | Modular monolith across 13 business modules in NestJS, pure domain layer, ambient Unit of Work (`AsyncLocalStorage`), module contract isolation (`contract/`), Temporal workflows in Python, read-only `brain_read` views with RLS. |
| [**FRONTEND-TECHNICAL-DESIGN.md**](./FRONTEND-TECHNICAL-DESIGN.md) | `busmora-frontend` | Client-Rendered Application (CSR) with Next.js 14+ App Router, feature modules with public `index.ts` interfaces, React Query cache, central gateways for session (`shared/session`), streams (`shared/streams`), and API (`shared/api`), RTL-first design. |

---

## 2. Domain-Driven Design (`domain/`)

The foundational business rules and domain modeling artifacts:

* [**GLOSSARY.md**](./domain/GLOSSARY.md) — The single source of truth for project terminology (e.g. *Business Brain*, *Knowledge Item*, *Post*, *Post Version*, *Review* vs *Approval*).
* [**contexts.md**](./domain/contexts.md) — The 11 Bounded Contexts (Core: Business Brain, Marketing Employee, Governed Publishing; Supporting; Generic).
* [**model.md**](./domain/model.md) — Tactical domain model: Aggregates, Entities, Value Objects, and Domain Invariants.
* [**events.md**](./domain/events.md) — Complete catalogue of domain events across context boundaries.

---

## 3. Architecture Decision Process (`decisions/`)

The chronological, evidence-based exploration and benchmarking process that led to the adopted design:

1. [**00 — Requirements Analysis**](./decisions/00-requirements-analysis.md): Rigorous extraction and gap analysis of all product requirements and historical documentation.
2. [**01 — Constraints & Evaluation Criteria**](./decisions/01-constraints.md): The binding constraints (K-01 to K-11), hard gates (G1–G5), and 9 weighted evaluation criteria.
3. [**02 — Architecture Options**](./decisions/02-options.md): Deep architectural options for backend (B1 Platform-Centred, B2 Workflow-Centred, B3 Context-Owned) and frontend (FE-1 CSR, FE-2 BFF, FE-3 Workspace Event Shell).
4. [**03 — Options Benchmark**](./decisions/03-benchmark.md): Objective scoring against gates and weighted criteria.
5. [**04 — Architecture Decision**](./decisions/04-decision.md): Final adoption record: **B1 + Amendment A1** for backend and **FE-1 (prepared for FE-3)** for frontend.
