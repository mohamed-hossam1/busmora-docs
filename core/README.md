# Core Specifications (`core/`)

This directory houses the **canonical, non-negotiable core specifications** for the **BusMora** platform. Together, these foundational documents define the end-to-end contract across product requirements, design system, overall system architecture, backend implementation, and frontend design.

---

## The Core Specifications

```mermaid
flowchart TD
    PRD["1. Product Requirements (prd.md)<br/><i>v1.1: AI Marketing Director, FR-01–49, AC-01–30</i>"]
    DS["2. Design System (DESIGN-SYSTEM.md)<br/><i>Tokens, Components, RTL-First Layouts</i>"]
    SYS["3. System Architecture (SYSTEM-ARCHITECTURE.md)<br/><i>Topology: 4 Deployables, 8 Process Roles, Security & A1</i>"]
    BE["4. Backend Design (BACKEND-TECHNICAL-DESIGN.md)<br/><i>Server: NestJS Modular Monolith, Sole Writer, RLS & Temporal</i>"]
    FE["5. Frontend Design (FRONTEND-TECHNICAL-DESIGN.md)<br/><i>Client: Next.js CSR, Workspace Event Shell, RTL-First</i>"]

    PRD --> DS
    PRD --> SYS
    DS --> FE
    SYS --> BE
    SYS --> FE
    BE <-->|REST / SSE / Contract| FE
```

### 1. [Product Requirements Document (`prd.md`)](./prd.md)
* **Scope:** Authoritative functional requirements (FR-01 through FR-49), non-functional requirements (NFR-01 through NFR-09), and release acceptance scenarios (AC-01 through AC-30) for the autonomous **AI Marketing Director** across its 24-week Phase 2 release contract.
* **Key Sections:** Marketing Director persona, 5 STAR diagnostic lenses, Now/Next/Later adaptive roadmap, dynamic multi-source research with per-task budget governance (FR-27), text-first content in 4 registers (Egyptian, Gulf, MSA, English), governed Facebook Page publishing with exact-version binding and delivery lateness allowances (FR-33–36), dual-format performance ingestion (CSV & Excel `.xlsx`) and manual aggregate outcome capture (FR-38, FR-47), context staleness holds (FR-48), and honest unsupported execution handoffs (FR-49).

### 2. [Design System (`DESIGN-SYSTEM.md`)](./DESIGN-SYSTEM.md)
* **Scope:** Design tokens, UI component specifications, typography scales, color palettes, and strict RTL/LTR bidirectional layout rules for `busmora-frontend`.

### 3. [System Architecture v1.0 (`SYSTEM-ARCHITECTURE.md`)](./SYSTEM-ARCHITECTURE.md)
* **Scope:** Canonical system-level topology, deployable units, inter-process communication, and security posture for the 24-week Phase 2 release built by 5 students (~2,400 hours).
* **Key Decisions:**
  * **4 Deployables / 8 Process Roles:** `busmora-web`, `busmora-api` (HTTP + Scheduler), `busmora-workers` (Temporal + Scheduled Publisher + Ingestion Crawler), `busmora-ai` (Temporal Workers + Internal HTTP).
  * **Platform-Centred Backend (Option B1):** `busmora-api` is the single owner and sole writer of all PostgreSQL domain tables.
  * **Amendment A1 (Internal HTTP):** Direct synchronous HTTP on private loopback for fast interactive chat replies ($\le 3\text{s}$) and Brain semantic search.
  * **Dual-Memory Brain:** Semantic memory (PostgreSQL concept graph + Qdrant vectors) + procedural rules.
  * **Security & Multi-Tenancy:** Row-Level Security (RLS) tenant isolation and envelope encryption (AWS KMS / Tink) for OAuth credentials.

### 4. [Backend Technical Design (`BACKEND-TECHNICAL-DESIGN.md`)](./BACKEND-TECHNICAL-DESIGN.md)
* **Scope:** Internal software design and code organization of `busmora-backend`.
* **Key Mechanisms:**
  * **NestJS Modular Monolith:** 13 business modules strictly matching the bounded contexts.
  * **Encapsulated Contracts:** Modules communicate exclusively through public `contract/` interfaces; direct cross-module table access is strictly prohibited.
  * **Ambient Unit of Work:** Atomic transactional persistence via Drizzle ORM.
  * **Read-Only Python Views:** Python workers execute complex vector/graph queries using dedicated read-only credentials enforced by RLS.
  * **Durable Orchestration:** Temporal workflows for background campaigns, market intelligence crawling, and Facebook publishing.

### 5. [Frontend Technical Design (`FRONTEND-TECHNICAL-DESIGN.md`)](./FRONTEND-TECHNICAL-DESIGN.md)
* **Scope:** Internal client architecture and state management of `busmora-frontend`.
* **Key Mechanisms:**
  * **Client-Side Rendered (CSR):** Next.js 14+ SPA optimized for interactive workspace state, Today view, and continuous SSE streaming.
  * **Prepared for Workspace Event Shell (FE-3):** Modular context-based directory structure (`features/brain`, `features/publishing`, `features/chat`, `features/roadmap`) ready for event-driven multi-panel canvas.
  * **State & Sync:** Server cache managed via TanStack React Query; interactive UI states managed via Zustand.
  * **Security & Tokens:** Short-lived access tokens (15 min) in memory only; secure refresh token in HttpOnly `SameSite=Strict` cookie with reuse detection.
  * **MENA First:** Built-in RTL layout direction, Arabic typography, and dialect formatting.

---

## Product Experience Companion

### [Final Product UI Journey (`FINAL-PRODUCT-UI-JOURNEY.md`)](./FINAL-PRODUCT-UI-JOURNEY.md)
* **Scope:** A detailed Arabic, screen-by-screen walkthrough of the customer experience from the marketing landing page and onboarding through planning, task execution, governed publishing, recurring review, and long-term retention. It includes textual wireframes, lifecycle diagrams, responsive behavior, system states, and explicit Phase 2 versus future-scope labels.
* **Authority:** This is an explanatory experience blueprint derived from the five canonical specifications. If it ever conflicts with a canonical requirement, the PRD and its supporting canonical designs remain authoritative.

### [UI Demo Master Prompt (`UI-DEMO-MASTER-PROMPT.md`)](./UI-DEMO-MASTER-PROMPT.md)
* **Scope:** A copy-ready implementation prompt for AI coding tools to build a complete interactive BusMora UI prototype with coherent Arabic demo data, responsive routes, deterministic state transitions, and PRD-aligned acceptance checks.
* **Authority:** This is an implementation aid derived from the canonical specifications and the Final Product UI Journey. It does not add or change product requirements.

---

## Interconnection with Related Directories

* [**Domain-Driven Design (`../domain/`)**](../domain/): Tactical aggregates, bounded contexts, domain events, and the canonical [Glossary](../domain/GLOSSARY.md).
* [**Product & Business Strategy (`../strategy/`)**](../strategy/): Business model, defensibility moat, MVP 24-week boundaries, and historical decision records.
* [**Research & Benchmarks (`../research/`)**](../research/): Customer discovery findings and technical benchmark evidence behind architectural decisions.
