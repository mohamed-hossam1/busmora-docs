# AGENTS.md — BusMora Documentation & Rules Hub

This repository is the canonical source of truth for architectural standards, team rules, and domain definitions across the entire BusMora platform.

Any agent working on frontend (`busmora-web`), backend (`busmora-api`), or AI services (`busmora-ai`) must inspect and follow the specifications documented here before making design or implementation decisions.

---

## 1. Global & Engineering Rules (`Vault/01 - Rules/`)

Every agent must strictly adhere to the team rules located in [`Vault/01 - Rules/`](./Vault/01%20-%20Rules/README.md):

- **Next.js & Frontend UI Rules:**
  - [`Vault/01 - Rules/Next.js UI Agent Rules.md`](./Vault/01%20-%20Rules/Next.js%20UI%20Agent%20Rules.md): Next.js UI conventions, Feature-Based Architecture, centralized `ROUTES` registry, and strict RTL/interaction completeness.
  - [`Vault/01 - Rules/Next.js Multilingual and SEO Rules.md`](./Vault/01%20-%20Rules/Next.js%20Multilingual%20and%20SEO%20Rules.md): Mandatory Arabic/English locale URLs, server-rendered public content, version-aware `next-intl`, RTL/LTR, navigation, SEO and verification. Public rendering follows this scoped rule; the protected application's client data model remains unchanged.
- **NestJS & Backend Rules:**
  - [`Vault/01 - Rules/NestJS Backend Agent Rules.md`](./Vault/01%20-%20Rules/NestJS%20Backend%20Agent%20Rules.md): Module ownership, explicit trust boundaries, transactional consistency on writes, and testing risky paths.
- **Engineering Workflow & Lifecycle:**
  - [`Vault/01 - Rules/Task Lifecycle Protocol.md`](./Vault/01%20-%20Rules/Task%20Lifecycle%20Protocol.md): Jira milestones, discovery guidelines, and acceptance-driven TDD from Red to Green.
  - [`Vault/01 - Rules/Documentation Lifecycle.md`](./Vault/01%20-%20Rules/Documentation%20Lifecycle.md): Lifecycle of task execution records, research evidence, and living feature capability overviews.
  - [`Vault/01 - Rules/Vault Conventions.md`](./Vault/01%20-%20Rules/Vault%20Conventions.md): Canonical vault navigation, link conventions, and strict separation between facts, decisions, and assumptions.
  - [`Vault/01 - Rules/Workspace Hygiene and Cleanup Rules.md`](./Vault/01%20-%20Rules/Workspace%20Hygiene%20and%20Cleanup%20Rules.md): Mandatory post-task cleanup, dead-code pruning, eliminating file duplicates, and zero-debris working tree.

---

## 2. Domain-Driven Design Specifications (`domain/`)

Before naming models, defining database entities, or creating UI components, consult the ubiquitous language and domain model in [`domain/`](./domain/README.md):

- [`domain/GLOSSARY.md`](./domain/GLOSSARY.md): Canonical terminology for all actors, marketing entities, and platform concepts (never invent alternate names).
- [`domain/contexts.md`](./domain/contexts.md): Bounded contexts, ownership, and subdomain boundaries mapping directly to system modules.
- [`domain/model.md`](./domain/model.md): Tactical aggregate definitions, invariants, and lifecycle state transitions.
- [`domain/events.md`](./domain/events.md): Domain events catalog and transactional outbox schemas.

---

## 3. Core Architecture & Design Specifications (`core/`)

The 5 canonical documents governing system design and product requirements in [`core/`](./core/README.md):

- [`core/prd.md`](./core/prd.md): Authoritative product requirements document (PRD v2.0), user journeys, and release gates.
- [`core/SYSTEM-ARCHITECTURE.md`](./core/SYSTEM-ARCHITECTURE.md): High-level system architecture, 4-repo layout, 8 process roles, auth cookies, and multi-tenant KMS envelope encryption.
- [`core/BACKEND-TECHNICAL-DESIGN.md`](./core/BACKEND-TECHNICAL-DESIGN.md): Modular monolith architecture across 13 business modules, Drizzle ORM, Unit of Work, and Temporal workflows.
- [`core/FRONTEND-TECHNICAL-DESIGN.md`](./core/FRONTEND-TECHNICAL-DESIGN.md): Next.js 14+ client-rendered architecture, feature boundaries, React Query caching, and SSE streams gateway.
- [`core/DESIGN-SYSTEM.md`](./core/DESIGN-SYSTEM.md): Visual tokens, typography scale, component standards, and strict RTL-first bidirectional layout rules.
