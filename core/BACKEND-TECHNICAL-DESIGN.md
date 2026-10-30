# BusMora — Backend Technical Design

> **Version:** 2.0 · **Status:** Aligned design; implementation dependencies remain open · **Date:** 2026-10-30
> **Scope:** the internal software architecture of the `busmora-backend` repository: `busmora-api` (NestJS), `busmora-workers` (Python), `busmora-ai` (Python).
> **Builds on:** [SYSTEM-ARCHITECTURE.md](./SYSTEM-ARCHITECTURE.md) (system-level decisions aligned to PRD v2.0), [domain/](../domain/) (language, contexts, model, events), [strategy/](../strategy/), and [prd.md](./prd.md).
> **Design only:** no code. Folder structures are trees.
> **Rule kept:** no technology beyond those named in the source documents. Where this design needs something the docs don't name, it's listed as a gap (Appendix A) and not chosen.
> **Reference knowledge:** module, interface, seam, adapter, depth and "one adapter = hypothetical seam" come from the repository's `codebase-design` skill. The domain vocabulary and ADR format come from `domain-modeling`. "architecture-patterns" and "api-design-principles" are not installed, so general pattern knowledge stands in for them.

---

> **Authority:** [PRD v2.0](./prd.md) is the product/release contract. SYS §9.5 records older registry/domain/research conflicts; glossary names remain useful, but obsolete lifecycle or phase rules do not override this design. SYS Appendix A provides the complete requirement alignment matrix. This is design coverage, not evidence that workflows are implemented.

## Table of contents
1. [Recap of system-level decisions](#1-recap-of-system-level-decisions)
2. [Internal architecture pattern](#2-internal-architecture-pattern)
3. [Module boundaries](#3-module-boundaries)
4. [Folder structure](#4-folder-structure)
5. [Design patterns](#5-design-patterns)
6. [Domain layer per module](#6-domain-layer-per-module)
7. [Data access](#7-data-access)
8. [API layer conventions](#8-api-layer-conventions)
9. [Cross-cutting concerns](#9-cross-cutting-concerns-only-what-the-docs-require)
10. [Testing strategy](#10-testing-strategy)
11. [Naming conventions and coding rules](#11-naming-conventions-and-coding-rules)
12. [Walkthrough: adding a feature end to end](#12-walkthrough-adding-a-feature-end-to-end)
13. [Internal decisions (ADRs)](#13-internal-decisions-adrs)
- [Appendix A — Gaps surfaced by this design](#appendix-a--gaps-surfaced-by-this-design)

---

## 1. Recap of system-level decisions

These are fixed by [SYSTEM-ARCHITECTURE.md](./SYSTEM-ARCHITECTURE.md). This document only designs *inside* them.

| # | System decision | Consequence for the internal design |
|---|---|---|
| S-1 | **Platform-Centred backend** (SYS ADR-003). `busmora-api` is the **single owner and only writer** of all domain data, the Brain included. | All business rules and all state transitions live in NestJS modules. Python never writes domain tables. |
| S-2 | **Python is stateless compute behind Temporal.** `busmora-workers` holds workflows (sequencing only) and the publish role. `busmora-ai` holds agents, crawling, retrieval, indexing, text/visual briefs and deterministic metrics. | Python code is organised around use cases and adapters, not around persistence. |
| S-3 | **Amendment A1** (SYS ADR-004): internal HTTP from api to `ai-http` for **quick replies** and **read-only Brain retrieval** only. | Exactly one outbound HTTP adapter in api, with two operations. |
| S-4 | **Persistence crosses languages through Temporal.** `api-activities` is a TypeScript activity worker hosting the "platform" activities. | NestJS modules expose activity adapters next to their HTTP adapters. |
| S-5 | **Deterministic publishing** (SYS ADR-005): no LLM after Approval; MCP tools in `workers-publish`; claim → attempt → publish → read-back. | The publishing module owns the claim and attempt records. The worker owns platform calls. |
| S-6 | **Process roles:** `api-http`, `api-activities`, `workers-workflows`, `workers-publish`, `ai-http`, `ai-agent`, `ai-crawl` (+ `web`). | Each app has one entrypoint per role, over shared code. |
| S-7 | **Data:** PostgreSQL as source of truth, one schema per context. **Drizzle is the single migration toolchain.** Python reads eligibility/provenance views through read-only roles with row-level security. Qdrant and Redis are derived. | Migrations live only in api. Views are the contract with Python. |
| S-8 | **No message bus.** Audit, usage and notification records are written **in the same transaction** as the change. | In-process domain events with same-transaction handlers. |
| S-9 | **Auth:** Argon2id; 15-minute JWT; rotating refresh token in an HttpOnly cookie; stream cookie for SSE; 60-second asymmetric service JWT for api → ai; one Temporal API key per role. | Guards and signers live in a shared `platform/auth` area, not in business modules. |
| S-10 | **Contracts:** code-first OpenAPI from `@nestjs/swagger`, with `oasdiff` in CI. Pydantic is canonical for Temporal payloads: JSON Schema → TypeScript types + Ajv. FastAPI's OpenAPI generates the api's `ai-http` client. | A `contracts/` area in the repo; generated code is never hand-edited. |
| S-11 | **Two standalone repositories**: this design covers `busmora-backend`. | Frontend concerns stop at the published contract. |

**Stack used in this design:**
- **TypeScript:** NestJS 10+, Passport.js + JWT, Argon2id, Drizzle ORM, Temporal TypeScript SDK, ioredis, class-validator, `@nestjs/swagger`, Ajv, `openapi-typescript`/`openapi-fetch` for the generated `ai-http` client.
- **Python:** 3.11+, FastAPI + Uvicorn, LangGraph, LiteLLM, Scrapling, Temporal Python SDK, Qdrant client, SQLAlchemy/asyncpg, Pydantic v2, MCP SDK.
- **Infrastructure:** PostgreSQL 16+, Qdrant, Redis 7+, Temporal Cloud, Cloud KMS, S3-compatible object storage, Langfuse, Docker Compose.
- **Built-ins only:** the Node.js runtime (e.g. `AsyncLocalStorage`, `crypto.randomUUID`), PostgreSQL features (schemas, views, row-level security, advisory locks), Temporal features (Schedules, signals, versioning/patching, test environments) and the Python standard library.

---

### 1.1 Product and release boundaries

The current product has Marketing Director, Roadmap, Business Brain, Deliverables, Scheduling and Settings, with contextual in-app notifications. Confirmed essential onboarding precedes independent Chat or adaptive Roadmap Discovery. Phase 2 supports Arabic/English UI and Egyptian, Gulf-oriented, MSA and English content registers; Facebook Page organic publishing requires real platform authorization. Social Profile Import, Instagram Business/X and connected analytics remain Phase 3; native image generation and billing remain Phase 4. **Phase 5 Meta Ads Management is future-only**: potential in-product business/ad-account connection, campaign/ad-set/ad creation, explicit budgets/spend authorization, monitoring and owner-approved optimization, subject to validated API access, permissions, formats and metrics; Pixel/Conversions API may be required. There are no current Ads execution endpoints or implicit spending powers. Today Dashboard, Outcomes & Reviews and fixed Now/Next/Later planning have no active modules, routes, schedules or domain models; §7.6 preserves necessary historical evidence.

## 2. Internal architecture pattern

### 2.1 Forces
1. **Single writer** (S-1): one NestJS codebase holds every business rule for 13 business areas. Without strong internal boundaries it becomes one tangled service.
2. **Cross-context transactions are required.** For example, completing a Task writes document Deliverables, Posts, Candidates, usage, audit and notifications **in one transaction** (SYS §4.6).
3. **Five students, vertical-slice ownership** (K-01, PRD §13.1). A feature must be findable in one place, and members must not tread on each other across their designated workstreams.
4. **Testability without the network.** LLMs, crawlers, Meta, X, KMS and Temporal must be replaceable in tests.
5. **A future Employee** (AG-8) should be addable as new configuration, capabilities and graph nodes, not a rewrite.
6. **Long-lived workflows** (up to a month) need contracts and schemas that only grow (S-10).

### 2.2 Alternatives considered (for `busmora-api`, where the domain lives)

| Alternative | What it is | Fit | Verdict |
|---|---|---|---|
| **A. Technical layering** | Global folders: `controllers/`, `services/`, `repositories/`, `entities/` | Simple to start, but every feature touches 4 global folders. Module ownership is invisible, and cross-context coupling grows silently. Fails forces 1 and 3. | Rejected |
| **B. Modular monolith by bounded context, pragmatic layers inside each module** | One module per context. Inside each: `domain` (pure), `application` (use cases), `infrastructure` (persistence, adapters), `interface` (HTTP and activity adapters). Modules talk only through a small public interface. Ports exist only at real seams. | Matches the 11 contexts and the single writer. Makes cross-context transactions possible with one unit of work. Vertical slices land inside one module. Testable. | **Chosen** |
| **C. Full Clean / Hexagonal everywhere** | Same modules as B, plus a port (interface) in front of **every** dependency: every repository, every module call, mappers at each ring | Maximal isolation, but most ports would have one adapter: hypothetical seams that cost a 5-person team time. The real DB is substitutable locally (Docker Compose), so repository ports buy little. | Rejected (B keeps its benefits where seams are real) |
| **D. Vertical-slice per endpoint (feature folders, no shared domain)** | Each use case owns its handler, SQL and DTOs; little shared domain | Fast for CRUD, but the core rules (Post lifecycle, Knowledge lifecycle, Task capacity) would be duplicated across slices. Weak for invariant-heavy aggregates. | Rejected (but B's `application/` *is* organised per use case) |
| **E. CQRS with separate read models / event sourcing** | Write model emits events; projections serve reads | Over-engineered for pilot scale. Conflicts with "no message bus". Adds projection drift. | Rejected |

### 2.3 Chosen pattern: Modular monolith with pragmatic layers and selective ports

```mermaid
flowchart TB
  subgraph Module["One business module (e.g. publishing)"]
    IF["interface<br/>http controllers · activity adapters"]
    APP["application<br/>use cases · contract implementation · event handlers · ports"]
    DOM["domain<br/>aggregates · value objects · domain events · policies"]
    INF["infrastructure<br/>Drizzle tables · repositories · mappers"]
  end
  PLAT["platform/*<br/>db · events · auth · temporal · ai-gateway · crypto · storage · streams · errors · config · logging · clock"]
  CON["contract (this module's public face)<br/>interface + token · contract types · public events"]
  OTHER["another module's contract/<br/>(the ONLY thing other modules may import)"]
  IF --> APP
  APP --> DOM
  APP --> INF
  INF --> DOM
  APP -->|"implements"| CON
  APP -->|"ports"| PLAT
  INF --> PLAT
  APP -->|"calls interface by token / subscribes to public events"| OTHER
```

**The dependency rule (enforced in CI by an import-boundary check, §10):**
1. `domain` depends on **nothing** outside itself: no NestJS, no Drizzle, no other modules, no I/O. Time arrives as a parameter or through the `Clock` port.
2. `application` may depend on its own `domain`, its own `infrastructure` (repositories), `platform` ports, its own `contract/` (which it implements), and **other modules' `contract/` only** (re-exported by their `index.ts`). It depends on the contract's **interface and token**, never on the class implementing it.
3. `infrastructure` may depend on its own `domain` and on `platform/db`. It never depends on `application` or `interface`.
4. `interface` (HTTP, activities) may depend only on its own `application`. Controllers and activities hold no business rules.
5. **No module imports another module's `domain/`, `application/`, `infrastructure/` or `interface/`.** It imports only that module's `index.ts`, which exports **nothing but the `contract/` folder**. Domain classes, internal domain events, repositories and Drizzle tables are never exported. §3.5 explains why this keeps internal changes from reaching other modules.
6. `platform/*` never imports from `modules/*`.
7. Cycles between modules are forbidden. The allowed directions are fixed in §3.3.

**Why it fits:**
- It turns the single-writer api into 13 deep modules with small interfaces. That gives locality for each owner and leverage for callers.
- It keeps one unit of work across modules, as the same-transaction rule requires.
- It puts ports exactly where two adapters genuinely exist: Temporal, `ai-http`, KMS, object storage, mail, clock, rate limiting. Production and test adapters both exist for each.

### 2.4 Internal patterns of the Python apps (same principles, smaller)

| App | Pattern | Layers |
|---|---|---|
| `busmora-workers` | **Thin deterministic workflows + activity adapters** | `workflows/` (sequencing only; no business rules, no I/O) → activity references by task queue. `publish/` is a small ports-and-adapters module: activities → MCP tools → platform adapters (anti-corruption for Meta and X). |
| `busmora-ai` | **Ports and adapters around a pure domain** | `entrypoints/` (FastAPI routes, Temporal activities) → `application/` (use cases) → `domain/` (LangGraph graph definition, retrieval algorithm, intent qualification, formatting rules, KPIs) ← `adapters/` (LiteLLM, Qdrant, read-only views, checkpoints, Scrapling, object storage, Redis Streams, Langfuse) |

The same dependency rule applies:
- `domain` imports no adapters.
- `entrypoints` hold no logic.
- Adapters implement ports declared beside the use cases that need them.
- Python never writes domain tables (S-1).

---

## 3. Module boundaries

### 3.1 `busmora-api` business modules

| Module | Context (domain/) | Owns (state and rules) | Public contract (`contract/`, re-exported by `index.ts`; see §3.5) |
|---|---|---|---|
| `identity` | Identity & Workspace | Users, provider identities, local credentials, Workspaces/settings, OnboardingDraft and confirmation, verification/reset, lifecycle and sessions | `IdentityApi`: workspace reads, assert active, draft save/confirm; `OnboardingConfirmed.v1` activates Brain in the same transaction; auth events audited |
| `employee` | Marketing Employee (configuration seam, D-02) | Employee configuration, Capabilities, Sub-agent roster, model tiers, prompt versions; freezing configuration for runs | `EmployeeApi` (via `EMPLOYEE_API`): `freezeConfiguration(workspaceId, mode)` → `FrozenConfiguration` (`mode` = normal, treatment or control) |
| `conversation` | Marketing Employee | Conversations, Messages (eight baseline kinds plus post_reference), Quick Replies, structured-form rendering data | `ConversationApi` (via `CONVERSATION_API`): `appendEmployeeMessage` (used only by its own event handlers; exported for evaluation fixtures). Consumes Task, Deliverable and Knowledge events to append messages. |
| `tasks` | Marketing Employee | Tasks, authorizations/resources, Clarifications, goals/constraints, RoadmapDraft/questions/answers/research links, Roadmaps/versions/steps/dependencies/revisions and owner attestations | `TasksApi`: request/start/complete/cancel, conversation-scoped queries, generate/activate/revise plan, launch/skip/revisit/attest step, resource reservation/settlement; public `Task*`, `Roadmap*`, `ResourceConsumed` events |
| `brain` | Business Brain | Knowledge Items, Relationships, Procedural Rules, Candidates, Review, Retirement, Deletion, Conflicts, Provenance, Brand Kit view, retrieval views for Python, index-version bookkeeping | `BrainApi` (via `BRAIN_API`): `proposeCandidates(workspaceId, proposals, provenance)`, `activateConfirmedIntake`, `applyOwnerContext`, `getEligibleContextRefs`, `getBrandKit`, `requestDataDeletion`. Events: `Knowledge*`, `Relationship*`, `ProceduralRuleApproved` |
| `crawls` | Market Intelligence | Onboarding Crawl and Research Crawl records, Snapshot metadata and reference counting (retention), crawl-cache bookkeeping | `CrawlsApi` (via `CRAWLS_API`): `getSnapshotMeta`, `markSnapshotReferenced`. Events: `OnboardingCrawl*`, `BrandKitDetected`, `ResearchCrawlCompleted`, `SourceSkipped` |
| `deliverables` | Deliverables | Versioned editable Deliverables (each revision immutable except governed deletion/redaction), Source Citations, scoped Analysis Uploads, validated owner media, exports | `DeliverablesApi` (via `DELIVERABLES_API`): `createDeliverables(taskId, outputs)`, `getDeliverable`, `reviseDeliverable`, `exportData`, `requestDataDeletion`. Events: `DeliverableCreated`, `AnalysisUploadRejected` |
| `publishing` | Governed Publishing | Posts, Post Versions, Campaign grouping, Approvals (single and bulk), Scheduled Times, Publish Attempts, the Post state machine, the claim, Batch Review Sessions (review-time instrumentation, EVAL §8) | `PublishingApi`: dispatch preflight and context-impact handling inside publishing; consumes Brain context/deletion events. Events: `Post*`, `ReviewSessionEnded` |
| `connections` | Channel Connections | Connected Accounts, KMS-encrypted credential envelopes, Reconnect Needed | `ConnectionsApi` (via `CONNECTIONS_API`): `getPublishingAccount(workspaceId, platform)` → status + authorization generation + opaque credential reference/envelope; `hasConnectedAccount`. Events: `Account*` |
| `evaluation` | Evaluation | Scenarios, Evaluation Rounds, Scenario Runs (Treatment/Control), reviewer scores, review-time measurements | Operator commands only (CLI); no contract operations. Consumes the public event `ReviewSessionEnded.v1`. |
| `usage` | Usage Metering | Usage Records (append-only, retention-limited) | No admission authority; consumes `ResourceConsumed` for actual/estimated model, search, retrieval, integration and compute costs. `tasks` owns enforceable reservations (§6.4.1) |
| `audit` | Audit Trail | Audit Events (append-only) | No contract operations; consumes public events every governed-action event |
| `notifications` | Notifications | In-app Notifications, read/unread | HTTP queries only; consumes attention-needed events |

### 3.2 Shared technical area: `platform/` (no business rules)

| Area | Provides | Seam (port) | Adapters |
|---|---|---|---|
| `platform/db` | Drizzle client, **TransactionManager** (ambient transaction through Node's `AsyncLocalStorage`), schema registry, the single migration history | — (real PostgreSQL in tests) | — |
| `platform/events` | In-process **domain-event dispatcher**: same-transaction handlers + after-commit hooks | — | — |
| `platform/auth` | Passport JWT strategy, access-token guard, stream-cookie guard, refresh-token rotation service, Argon2id hasher, service-JWT signer, `@CurrentTenant` | — | — |
| `platform/tenancy` | `TenantContext` (workspace id, user id, trace id) and its propagation | — | — |
| `platform/temporal` | Workflow starting and signalling; TS activity-worker bootstrap; Ajv validation of activity payloads | `WorkflowGateway` | Temporal client / in-memory recorder |
| `platform/ai-gateway` | Quick reply and Brain retrieval over `ai-http` | `AiGateway` (2 operations) | Generated `ai-http` client + service JWT / fake |
| `platform/crypto` | KMS envelope encryption (encrypt only, in api) | `CredentialCipher` | Cloud KMS / fake |
| `platform/storage` | Object storage for uploads; signed URL issuance where api needs it | `BlobStore` | S3-compatible / in-memory |
| `platform/mail` | Verification and reset emails | `Mailer` | Provider (open, OD-10) / in-memory outbox |
| `platform/rate-limit` | Guard for public endpoints | `RateLimiter` | Mechanism open (OD-14) / allow-all in tests |
| `platform/streams` | SSE gateway: reads per-Task Redis Streams; replay from `Last-Event-ID` or the `after` query parameter; heartbeats; reset event. Also reads the per-Task **Canvas state** hash for snapshots (§8.1.3). | `CanvasStateReader` | ioredis / in-memory fake |
| `platform/idempotency` | `Idempotency-Key` handling for commands (stored in PostgreSQL) | — | — |
| `platform/errors` | Domain-error base types, HTTP exception filter, error envelope | — | — |
| `platform/http` | Global validation pipe, URI versioning, pagination helpers, OpenAPI document setup | — | — |
| `platform/config` | Typed configuration validated at startup (class-validator) | — | — |
| `platform/logging` | Structured logger with trace, workspace and task ids; redaction | `Logger` (sink open, OD-13) | NestJS logger / test capture |
| `platform/clock` | Current time | `Clock` | system / fixed-time fake |

### 3.3 How modules communicate

**Allowed:**

| Mechanism | When | Rule |
|---|---|---|
| **Contract call** (synchronous, in-process) | A use case needs another context to *do* something inside the current transaction, e.g. `tasks.completeTask` → `deliverables.createDeliverables` + `brain.proposeCandidates` | Only through the target's **contract interface**, injected by its token (e.g. `BRAIN_API`). Interfaces are few and deep. Arguments and returns are **contract types** defined in `contract/`, never domain objects or ORM rows. |
| **Public event subscription** (same transaction) | A context must *react* to a fact owned by another context: audit, usage, notifications; conversation messages on Task events; conversation appending typed Post references on `PostDrafted` | The handler lives in the **consuming** module and subscribes to the producer's **public event contract** (e.g. `DeliverableCreated.v1`), never its internal domain event. It runs before commit; if it throws, the whole command rolls back. |
| **After-commit hook** | A side effect outside PostgreSQL: starting or signalling a workflow, re-indexing | Registered by the use case and run only after commit. It must be idempotent: deterministic workflow ids, safe repeated signals. |
| **Shared kernel** | Workspace id, user id, `BusinessTimeZone`, `Platform`, `Dialect`, money/cost units | Defined once in `modules/identity/index.ts` (workspace types) and `platform/tenancy`. Read-only types. |

**Fixed dependency directions** (arrows = "may call the contract interface of / may subscribe to the public events of"):
```
conversation ─► tasks ─► employee ─► identity
                 │  ├─► deliverables ─► identity
                 │  └─► brain ─► crawls ─► identity
publishing ─► connections ─► identity
tasks ─► publishing (create Posts from validated Task results)
publishing ─► deliverables (validate immutable owner-media references only)
conversation ─► deliverables, publishing (validate typed revision targets only)
brain ⇢ (subscribes) identity events (confirmed initial intake)
conversation ⇢ (subscribes) publishing events (typed Post references)
conversation ⇢ (subscribes) tasks, deliverables, brain events
evaluation ─► tasks, employee, deliverables
evaluation ⇢ (subscribes) publishing events (ReviewSessionEnded.v1)
publishing ⇢ (subscribes) brain events (context impact/deletion)
tasks ⇢ (subscribes) brain events (context impact/deletion)
usage, audit, notifications ⇢ (subscribe) events of all modules; nobody depends on them
```

**Forbidden:**
1. Importing anything from another module except its `index.ts`.
2. Reading or writing another module's tables, including cross-module SQL joins. Cross-module references are **ID values**. Foreign keys are allowed only within a module and to `identity.workspaces`.
3. Calling another module's contract from `domain/`.
4. Cycles in the graph above. A new edge needs a design note.
5. Business rules in controllers, activities, event handlers of `audit`/`usage`/`notifications`, or Python workflows.
6. Any Python process writing domain tables. Python returns results through Temporal, and the owning module persists them.
7. Any synchronous api → ai call other than the two `AiGateway` operations (S-3).
8. Publishing tools or credential plaintext anywhere outside `workers-publish`.

### 3.4 Python-side boundaries

| App | Area | Owns | Exposed as |
|---|---|---|---|
| workers | `workflows/` | Sequencing of TaskExecution, Onboarding, PostRelease, Indexing, DataDeletion, DataExport, WorkspaceDeletion and the maintenance schedules | Temporal workflow types (task queue `workflows`) |
| workers | `publish/` | MCP tools, platform adapters, KMS decrypt, read-back | Activities on task queue `publish` |
| ai | `employee` domain | LangGraph graph (Router, Orchestrator, Sub-agents, Assembly, Knowledge Extractor), tool registry per Sub-agent | Activity `ai.run_agent_step`; HTTP `route-and-reply` |
| ai | `retrieval` domain | Two-stage retrieval, context assembly, token ceiling, control route | HTTP `brain-retrieval`; in-process to the graph |
| ai | `intelligence` domain | Sanitization, quarantine, intent qualification | In-process; crawl activities on task queue `ai-crawl` |
| ai | `content` domain | Text/platform formatting, dialect handling, visual-brief composition and claim support | In-process drafting; no native image execution in Phase 2 |
| ai | `performance` domain | CSV/.xlsx normalization for explicit requested analysis and deterministic KPIs | Activity `ai.compute_kpis` |

**Python rules:**
- Workflows never import adapters.
- Sub-agents never call each other; all hand-offs go through Orchestrator state (engineering orchestration boundary supporting FR-20).
- Each Sub-agent's tool list is fixed in the tool registry, and crawling tools are never given to writers.
- `ai-http` routes depend only on the routing and retrieval use cases.
- The Canvas adapter writes each signal to the per-Task Redis Stream **and** updates the per-Task Canvas state hash in the same Redis transaction, so a snapshot and its stream position always agree (§8.1.3). The key layout is part of `packages/contracts-py`.
- Python areas talk to each other only through the use-case interfaces and ports declared in `application/`. Python apps talk to each other and to api only through `packages/contracts-py` (§3.5 applies the same way).

### 3.5 Interface-only communication: a change inside one module never breaks another

**Goal:** any change inside a module compiles and runs without touching any other module, as long as its **contract** is unchanged. That covers its domain model, use cases, database tables, queries, internal events and adapters.

**1. Every module has a `contract/` folder. It is the only thing other modules can see.**

| File (in `modules/<m>/contract/`) | Contains | Example (brain) |
|---|---|---|
| `<m>.api.ts` | A TypeScript **interface** describing the module's operations, plus a NestJS **injection token** | `interface BrainApi { proposeCandidates(…); getBrandKit(…) }` and `BRAIN_API` |
| `<m>.types.ts` | **Contract types**: plain, serialisable input and output shapes owned by the contract. They are **not** domain classes. | `CandidateProposal`, `BrandKitView`, `ProvenanceRef` |
| `<m>.events.ts` | **Public event contracts**, each with an explicit version | `KnowledgeItemApproved.v1 { workspaceId, itemId, category, occurredAt }` |
| `<m>.errors.ts` | The contract's error codes, so callers can react without importing domain errors | `BRAIN_ITEM_NOT_FOUND` |

`index.ts` re-exports **only** `contract/`.

**2. Consumers depend on the interface, never the implementation.**
- A consuming module injects `BRAIN_API` and types it as `BrainApi`.
- The class that implements it (`BrainApiImpl` in `application/`) is bound to the token in `brain.module.ts`. No other module can import it.
- The implementation can be renamed, split or rewritten freely.

**3. Contract types are not domain types.**
- The provider maps between its domain objects and its contract types inside its own `application/` layer.
- If the Brain's `KnowledgeItem` gains fields, changes structure or gets a new invariant, only that mapping changes. Callers still receive the same `BrandKitView`.

**4. Public events are not internal events.**
- Each module raises **internal domain events** inside its domain.
- Before dispatch, the module maps them to **public event contracts** (`…v1`).
- Subscribers in other modules only ever see public events. A module can add, rename or restructure internal events without affecting anyone.

**5. Which changes can break whom:**

| Change inside module M | Other modules affected? |
|---|---|
| Domain model, aggregates, value objects, policies | **No** (hidden behind contract types) |
| Use cases, implementation class, internal refactors | **No** (consumers hold the interface) |
| Tables, columns, migrations, queries, indexes (own schema) | **No** (no other module reads M's tables, §3.3) |
| Internal domain events | **No** (only public `…vN` events cross) |
| Adapters, ports, platform usage | **No** |
| **Additive contract change**: new optional field, new operation, new event type or version | **No** (existing consumers keep compiling and working) |
| **Breaking contract change**: removing or retyping a field, changing an operation's meaning | **Not allowed in place.** Publish `…v2` / a new operation **next to** `v1`. Migrate consumers one at a time. Remove `v1` only when it has no consumers left. |

**6. How it's enforced:**
- **Compile time:** consumers can only import `contract/` (through `index.ts`). The architecture check fails the build on any deeper import (§10).
- **Contract tests:** each module has a test suite that exercises its `contract/` interface and public events, through the real implementation and real PostgreSQL. It runs on every change to the module, so an internal edit that silently changes contract behaviour fails in the provider's own build, before any consumer notices.
- **Contract change review:** any change under `contract/` is flagged in the pull request and must be additive, or add a new version.
- **Same rule across languages:** Python ↔ TypeScript payloads (`packages/contracts-py`), the `ai-http` API and the `brain_read` views follow the same additive rules (§7.4, R-5).

**7. Wiring example** (described, not code):
- `tasks` needs to propose Candidates.
- `tasks.module.ts` imports `BrainModule`, which exports only the `BRAIN_API` provider.
- The `CompleteTask` use case declares a dependency on `BrainApi` via `BRAIN_API` and passes `CandidateProposal[]`, a contract type.
- Nothing in `tasks` names a Brain class, table or internal event.

---

## 4. Folder structure

```
busmora-backend/
├── apps/                                     # one folder per deployable (D-26); each builds one image
│   ├── api/                                  # busmora-api — NestJS, TypeScript; sole domain writer
│   │   ├── src/
│   │   │   ├── main.http.ts                  # entrypoint for role api-http (REST + SSE)
│   │   │   ├── main.activities.ts            # entrypoint for role api-activities (Temporal TS activity worker)
│   │   │   ├── main.cli.ts                   # entrypoint for operator commands (workspace deletion, evaluation seeding)
│   │   │   ├── http.module.ts                # root wiring for api-http: modules' http adapters + platform
│   │   │   ├── activities.module.ts          # root wiring for api-activities: modules' activity adapters + platform
│   │   │   ├── cli.module.ts                 # root wiring for operator commands
│   │   │   ├── modules/                      # business modules, one per bounded context
│   │   │   │   ├── identity/                 # Users, Workspaces, sessions, time zone
│   │   │   │   │   ├── domain/               # aggregates, value objects, domain events, policies (pure TS)
│   │   │   │   │   ├── application/          # use cases (commands/queries), contract implementation, event handlers
│   │   │   │   │   │   ├── commands/         # one file per command use case (e.g. register-user)
│   │   │   │   │   │   ├── queries/          # one file per query use case (read models for HTTP)
│   │   │   │   │   │   └── handlers/         # subscriptions to other modules' public events (if any)
│   │   │   │   │   ├── infrastructure/       # persistence for this module only
│   │   │   │   │   │   ├── schema/           # Drizzle table definitions for this module's PostgreSQL schema
│   │   │   │   │   │   ├── repositories/     # one repository per aggregate root
│   │   │   │   │   │   └── mappers/          # row ↔ domain mapping
│   │   │   │   │   ├── interface/            # adapters that call application use cases
│   │   │   │   │   │   ├── http/             # controllers + request/response DTOs (class-validator, @nestjs/swagger)
│   │   │   │   │   │   └── activities/       # Temporal activity implementations owned by this module (if any)
│   │   │   │   │   ├── contract/             # the ONLY public face of the module (§3.5)
│   │   │   │   │   │   ├── identity.api.ts   # interface + injection token (e.g. IDENTITY_API)
│   │   │   │   │   │   ├── identity.types.ts # contract types (plain, serialisable; not domain classes)
│   │   │   │   │   │   ├── identity.events.ts# public, versioned event contracts (…v1)
│   │   │   │   │   │   └── identity.errors.ts# contract error codes
│   │   │   │   │   ├── identity.module.ts    # Nest module: binds the implementation to the token; exports only the token
│   │   │   │   │   └── index.ts              # re-exports contract/ and nothing else
│   │   │   │   ├── employee/                 # Employee configuration, Capabilities, frozen configuration (same layout)
│   │   │   │   ├── conversation/             # Conversations, Messages, Quick Replies (same layout)
│   │   │   │   ├── tasks/                    # Tasks, Discovery drafts/questions, Roadmaps/steps/revisions, capacity, budgets, Canvas snapshot
│   │   │   │   ├── brain/                    # Knowledge Items, Relationships, Procedural Rules, Review (same layout)
│   │   │   │   │   └── infrastructure/
│   │   │   │   │       └── read-views/       # definitions of the read-only views Python consumes (contract)
│   │   │   │   ├── crawls/                   # crawl records, Snapshot metadata, retention (same layout)
│   │   │   │   ├── deliverables/             # Deliverables, Citations, Analysis Uploads (same layout)
│   │   │   │   ├── publishing/               # Posts, Versions, Approvals (single + bulk), Attempts, claim, Review Sessions (same layout)
│   │   │   │   ├── connections/              # Connected Accounts, credential envelopes (same layout)
│   │   │   │   ├── evaluation/               # Scenarios, rounds, runs, scores (same layout; CLI interface)
│   │   │   │   ├── usage/                    # Usage Records (handlers + queries)
│   │   │   │   ├── audit/                    # Audit Events (handlers + queries)
│   │   │   │   └── notifications/            # Notifications (handlers + queries)
│   │   │   ├── platform/                     # technical capabilities shared by modules; no business rules
│   │   │   │   ├── auth/                     # JWT strategy, guards (access, stream cookie), refresh rotation, Argon2id, service-JWT signer
│   │   │   │   ├── tenancy/                  # TenantContext and its propagation (HTTP and activities)
│   │   │   │   ├── db/                       # Drizzle client, TransactionManager (ambient transaction)
│   │   │   │   │   ├── migrations/           # the ONE migration history (generated SQL, reviewed in PR)
│   │   │   │   │   └── roles/                # role, grant and row-level-security definitions for Python read roles
│   │   │   │   ├── events/                   # in-process domain-event dispatcher; after-commit hooks
│   │   │   │   ├── temporal/                 # WorkflowGateway port + adapter; worker bootstrap; Ajv payload validation
│   │   │   │   ├── ai-gateway/               # AiGateway port + adapter over the generated ai-http client
│   │   │   │   ├── crypto/                   # CredentialCipher port + KMS adapter
│   │   │   │   ├── storage/                  # BlobStore port + object-storage adapter
│   │   │   │   ├── mail/                     # Mailer port (provider open)
│   │   │   │   ├── rate-limit/               # RateLimiter port + guard (mechanism open)
│   │   │   │   ├── streams/                  # SSE gateway: Redis Streams reader, replay, heartbeats, reset
│   │   │   │   ├── idempotency/              # Idempotency-Key store + interceptor
│   │   │   │   ├── errors/                   # domain-error base, error codes catalogue, exception filter, envelope
│   │   │   │   ├── http/                     # validation pipe, /v1 versioning, pagination, OpenAPI setup
│   │   │   │   ├── config/                   # typed config schema, validated at startup
│   │   │   │   ├── logging/                  # structured logger, redaction, trace propagation
│   │   │   │   └── clock/                    # Clock port + system adapter
│   │   │   └── generated/                    # machine-generated; never edited by hand
│   │   │       ├── temporal-contracts/       # TS types generated from contracts/temporal JSON Schema
│   │   │       └── ai-http-client/           # TS client generated from contracts/ai-http OpenAPI
│   │   └── test/
│   │       ├── fakes/                        # second adapters for every port (Temporal, AI, KMS, storage, mail, clock, rate limit)
│   │       ├── builders/                     # aggregate and request builders; seeded-workspace helpers
│   │       ├── domain/                       # pure unit tests per module (mirrors modules/*/domain)
│   │       ├── integration/                  # use-case tests against real PostgreSQL (Docker Compose)
│   │       ├── http/                         # end-to-end HTTP tests per module (app + test database)
│   │       ├── activities/                   # activity tests in the Temporal TS test environment
│   │       └── architecture/                 # import-boundary and dependency-direction checks
│   ├── workers/                              # busmora-workers — Python; sequencing + publish
│   │   ├── src/busmora_workers/
│   │   │   ├── main_workflows.py             # entrypoint for role workers-workflows
│   │   │   ├── main_publish.py               # entrypoint for role workers-publish
│   │   │   ├── workflows/                    # deterministic workflow definitions (no I/O, no business rules)
│   │   │   │   ├── task_execution.py         # TaskExecutionWorkflow (Clarification waits, capacity wait)
│   │   │   │   ├── onboarding.py             # OnboardingWorkflow
│   │   │   │   ├── post_release.py           # PostReleaseWorkflow (one per Post)
│   │   │   │   ├── indexing.py               # IndexingWorkflow
│   │   │   │   ├── data_deletion.py          # selected-data deletion with suppression and per-store evidence
│   │   │   │   ├── data_export.py            # module-owned projections, package and expiry
│   │   │   │   ├── workspace_deletion.py     # WorkspaceDeletionWorkflow
│   │   │   │   └── maintenance.py            # scheduled workflows: retention sweep, reconciliation
│   │   │   ├── activity_refs/                # typed references to activities on other task queues (platform.*, ai.*, crawl.*)
│   │   │   ├── publish/                      # role workers-publish (ports and adapters)
│   │   │   │   ├── activities/               # publish_post_version, read_back, refresh_credential
│   │   │   │   ├── mcp_server/               # MCP tools: publish_facebook (Phase 2); publish_instagram, publish_x (Phase 3)
│   │   │   │   ├── platforms/                # anti-corruption adapters for Meta Graph (FB Pages in Phase 2; IG/X in Phase 3)
│   │   │   │   └── secrets/                  # KMS decryption, in memory only
│   │   │   ├── config/                       # settings validated with Pydantic models
│   │   │   └── observability/                # logging setup, trace-id propagation
│   │   └── tests/
│   │       ├── workflows/                    # time-skipping workflow tests with stubbed activities
│   │       ├── publish/                      # publish activities against fake platform adapters
│   │       └── fakes/                        # fake platforms, fake KMS
│   └── ai/                                   # busmora-ai — Python; agents, crawling, retrieval
│       ├── src/busmora_ai/
│       │   ├── main_http.py                  # entrypoint for role ai-http (FastAPI, internal only)
│       │   ├── main_agent.py                 # entrypoint for role ai-agent (Temporal activity worker)
│       │   ├── main_crawl.py                 # entrypoint for role ai-crawl (Temporal activity worker)
│       │   ├── entrypoints/                  # adapters that call use cases; no logic
│       │   │   ├── http/                     # routes (route-and-reply, brain-retrieval), service-JWT verification, schemas
│       │   │   └── activities/               # run_agent_step, extract_candidates, index_items, compute_kpis, crawl_*
│       │   ├── application/                  # use cases + the ports they need
│       │   ├── domain/                       # pure logic; no adapters
│       │   │   ├── employee/                 # LangGraph graph, state schema, Sub-agents, tool registry, budgets
│       │   │   ├── retrieval/                # seed + 1–2 hop expansion, assembly, token ceiling, control route
│       │   │   ├── intelligence/             # sanitization, quarantine boundaries, intent qualification
│       │   │   ├── content/                  # platform formatting, dialect rules, text/visual-brief composition rules
│       │   │   └── performance/              # CSV/.xlsx normalization, deterministic KPIs
│       │   ├── adapters/                     # implementations of ports
│       │   │   ├── llm/                      # LiteLLM (light/heavy tiers, fallback, cost capture)
│       │   │   ├── vector/                   # Qdrant (workspace + status filters always applied)
│       │   │   ├── brain_views/              # SQLAlchemy Core reads of eligible Brain + module-owned historical views
│       │   │   ├── checkpoints/              # LangGraph checkpointer in the runtime schema
│       │   │   ├── crawler/                  # Scrapling, browser pool, proxies, SSRF guard, cache
│       │   │   ├── blob/                     # object storage (Snapshots and applicable artifacts)
│       │   │   ├── canvas/                   # Canvas signal emitter (Redis Streams, per Task)
│       │   │   ├── tracing/                  # Langfuse
│       │   ├── prompts/                      # version-controlled prompt templates (version recorded per run)
│       │   └── config/                       # settings validated with Pydantic models
│       └── tests/
│           ├── domain/                       # pure tests: retrieval assembly, qualification, KPIs, formatting
│           ├── application/                  # use cases with fake adapters
│           ├── entrypoints/                  # FastAPI routes and activities with fakes
│           └── fakes/                        # fake LLM, fake vector store, fake crawler, fake views
├── packages/
│   └── contracts-py/                         # CANONICAL Pydantic models: Temporal payloads, Canvas signal envelope
├── contracts/                                # generated artefacts, committed, verified in CI
│   ├── temporal/                             # JSON Schema exported from contracts-py
│   ├── ai-http/                              # OpenAPI exported from ai-http
│   └── api/                                  # openapi.json generated from busmora-api; published for the frontend
├── evaluation/                               # Scenario definitions, seeded-workspace data, rubrics (EVAL §3–§5)
├── deploy/
│   └── compose/                              # Docker Compose: all backend roles + PostgreSQL, Redis, Qdrant, object storage
├── scripts/                                  # contract generation, drift checks, seeding, local setup
├── ci/                                       # pipeline definitions (CI tooling open, OD-16)
└── docs/
    └── adr/                                  # internal ADRs (seeded from §13)
```

---

## 5. Design patterns

### 5.1 Patterns used

| Pattern | Where | Problem it solves |
|---|---|---|
| **Modular monolith** | `apps/api/src/modules/*` | One writer holding 13 contexts without becoming a ball of mud; vertical-slice ownership |
| **Published interface (module contract)** | each module's `contract/`: interface + token, contract types, versioned public events | Modules depend only on interfaces. Internal changes never ripple to other modules (§3.5). Callers learn a small responsibility-focused interface, extended only for committed journeys. |
| **Dependency inversion via injection tokens** | `contract/<m>.api.ts` token bound in `<m>.module.ts` | Consumers never reference the implementing class, so it can be rewritten freely |
| **Integration (public) events separate from domain events** | internal events mapped to `…vN` public events before dispatch | Internal event changes stay private; public events evolve additively by version |
| **Aggregate + state machine** | `Post`, `Task`, `KnowledgeItem`, `ProceduralRule`, `ConnectedAccount`, `Workspace` | Invariants held in one place. Examples: approval binds to one Post Version; edit returns to `pending_approval`; ≤ 2 running Tasks; owner input is active with owner provenance. |
| **Value object** | `ScheduledTime`, `BusinessTimeZone`, `Provenance`, `CredentialEnvelope`, `IdempotencyKey`, `PostContent`, `KnowledgeCategory`, `Cost` | Validity by construction; comparison by value; no primitive obsession around time zones and money |
| **Policy (strategy for rules)** | `TaskCapacityPolicy`, `PostTransitionPolicy`, `CandidateOriginPolicy`, `RetentionPolicy` | Keeps rules readable and unit-testable apart from aggregates |
| **Repository** (concrete, per aggregate root) | `modules/*/infrastructure/repositories` | Keeps Drizzle out of the domain. One place per aggregate for loading, saving and the mandatory workspace scope. |
| **Unit of Work (ambient transaction)** | `platform/db` TransactionManager, propagated by `AsyncLocalStorage` | Cross-module same-transaction writes (task completion, audit, usage, notifications) without passing transactions by hand |
| **Domain events, in-process, same transaction** | `platform/events` + `application/handlers` | Decouples audit, usage, notifications, conversation messages and typed result messages from producers, without a message bus (S-8) |
| **After-commit hook + deterministic workflow ids** | use cases that start or signal workflows | Workflows start only for committed state. Retries can't create duplicates, because the workflow id equals the Task, Post or index-version id. |
| **Ports and adapters (selective)** | `WorkflowGateway`, `AiGateway`, `CredentialCipher`, `BlobStore`, `Mailer`, `RateLimiter`, `Clock`, `Logger`; in ai: `Llm`, `VectorIndex`, `BrainViews`, `Crawler`, `Blob`, `CanvasSink`, `Tracer`; in workers: `PlatformPublisher` | Replace the network in tests. Every port has **two real adapters** (production + test). |
| **Anti-corruption layer** | `workers/publish/platforms/*`; `platform/ai-gateway` | Meta and X API shapes and the `ai-http` client types never leak into domain types |
| **Process manager** | Python workflows | Durable sequencing with timers and signals, while rules stay in api |
| **Claim check** | Temporal payloads carry ids and object-storage keys | Avoids Temporal payload-size limits; content travels by reference |
| **Idempotency key / idempotent consumer** | `platform/idempotency` (HTTP); activity idempotency by (workflow id, step); publish key = Post Version id | Local duplicate writes are suppressed; external publishing uses reconciliation and uncertainty holds (FR-36), not a local-key exactly-once guarantee |
| **Optimistic concurrency** | `version` columns on Post, Knowledge Item, intake/discovery drafts and Roadmaps; commands carry the version they act on | Stale edits and approvals are rejected (Approval binds to a version) |
| **Pessimistic per-workspace lock** | PostgreSQL advisory transaction lock in `TaskCapacityPolicy` and the Post claim | Exact enforcement of "≤ 2 running Tasks" and of a single claim, under concurrency |
| **Snapshot (frozen configuration)** | `employee.freezeConfiguration` → workflow input | In-flight runs are unaffected by later edits (SYS §7.7); the seam for a future Employee |
| **Envelope encryption** | `platform/crypto` (encrypt) / `workers/publish/secrets` (decrypt) | Credential plaintext exists only in memory in one process |
| **Read-only view contract** | `brain/infrastructure/read-views` + `platform/db/roles` | Python reads eligible labelled context without owning or coupling to write tables |
| **Mapper / DTO** | `interface/http` DTOs, `infrastructure/mappers` | API shapes and storage shapes evolve independently of the domain |
| **Registry (tool scoping)** | `ai/domain/employee` tool registry per Sub-agent | Each Sub-agent sees only its own tools (§3.4, §9.8) |
| **Template / builder for prompts** | `ai/prompts` | Versioned, reviewable prompts; the version is recorded per run |

### 5.2 Patterns deliberately avoided

| Avoided | Why |
|---|---|
| Event sourcing | Pilot scale; PostgreSQL rows with an audit trail meet the requirements; projections would add drift |
| CQRS with separate read stores | One database is enough; read queries live in each module's `application/queries` |
| Message bus or transactional outbox | Excluded at system level (S-8). After-commit hooks + deterministic ids + reconciliation cover external side effects. |
| Generic repository / base CRUD service | Hides aggregate invariants; makes it easy to skip the workspace scope |
| Repository interfaces (ports) for every repository | One adapter = a hypothetical seam. Tests use real PostgreSQL. |
| Active Record / ORM entities as domain objects | Couples rules to Drizzle; makes the domain impure |
| Service locator / global singletons | Hidden dependencies, untestable; NestJS dependency injection is used instead |
| Shared `utils`/`common` business module | Turns into a dumping ground. Shared *technical* code goes to `platform/`; shared *business* code doesn't exist. |
| Sharing domain classes or internal events across modules | Would make any internal change a cross-module break. Only contract types and public events cross (§3.5). |
| Distributed transactions or sagas across services | Python never writes domain data, so no cross-service transaction exists |
| Aspect-oriented business logic (decorators that change behaviour) | Rules must be visible in use cases. Decorators are limited to wiring, validation and auth. |
| An LLM call in any publish path | System rule (S-5) |

---

## 6. Domain layer per module

The vocabulary follows [domain/GLOSSARY.md](../domain/GLOSSARY.md). Storage names that differ from it are noted in §7.2.

Everything in this section is **internal** to its module. Aggregates, value objects and policies never leave it. The events listed are **internal domain events**; each module maps the ones others need into **public, versioned event contracts** in its `contract/<m>.events.ts` (§3.5). Cross-module inputs and outputs are **contract types**, never these domain classes.

### 6.1 `identity`

`User` has id/email/verified/UI language; `LocalCredential` is optional (Argon2id hash), and `AuthIdentity` has provider, issuer, subject, userId and providerEmail metadata. Unique `(issuer, subject)` prevents identity duplication. Email equality alone never links accounts. Google-only Users have no local hash. Registration creates an isolated Workspace shell owned by one User; business name/services are collected later.

`Workspace` holds explicit country/currency/IANA business zone, independent UI/conversation/content defaults and active/deletion_requested/deleted state. `LoginSession` refresh families remain in Redis: 15-minute in-memory access JWT, rotation/reuse revocation, 10-second grace, 7-day idle/30-day absolute expiry; scoped stream cookie and revocation behavior are unchanged.

`OnboardingDraft` belongs to identity as initial workspace setup, with `id, workspaceId, version, status: editing|extracting|ready_for_confirmation|confirmed, source: website|manual, crawlId?, fields, fieldProvenance, extractionRevision?, confirmedAt?, confirmationId?`. Extraction may fail/partially complete without losing editable fields; crawl outcome remains separate. `fields: BusinessInformation` requires name, description, one or more offerings, country/serviceArea and conversionRoute `{state: ready|not_ready, routes: [{kind, value}]}`. Ready requires at least one valid route; not_ready is an explicit valid answer. Offering: `{id, name, description, category?, details?, pricing: {model: fixed|starting_from|quote_required|unknown, amount?: decimal-string, currency?: ISO-code}}`. Amount is optional even for fixed/starting_from until known, nonnegative when explicit, and requires currency; quote_required/unknown carry no numeric amount. No zero defaults. Optional website/social/contact/logo/colors/tone/languages/zone are progressive.

Raw crawl observations belong to crawls; structured extraction is untrusted proposed data referenced by the draft. OnboardingWorkflow calls identity-owned platform activities to merge extraction into the draft with expected version and per-field origin; late extraction cannot overwrite owner-touched fields. Conflicts require inspection. Only explicit confirm of the displayed version emits `OnboardingConfirmed.v1` with immutable approved values/provenance and confirmationId. Brain consumes it in the **same transaction**, activates initial records once, and schedules indexing after commit. Failure rolls back confirmation and activation together. No initial Candidate queue or eligible draft embeddings. Identity never imports Brain internals or calls back into Brain; this event preserves the dependency direction `brain → identity`.

Events: `UserRegistered`, `EmailVerified`, `AuthIdentityLinked`, `SessionRevoked`, `WorkspaceCreated`, `OnboardingDraftUpdated`, `OnboardingConfirmed`, `BusinessTimeZoneChanged`, `WorkspaceDeletionRequested/Deleted`. Export/delete covers provider links, drafts and source dependencies; credential secrets never appear in export.

### 6.2 `employee`

| Kind | Items |
|---|---|
| **Aggregates** | `EmployeeConfiguration`: the Marketing Employee's Capabilities, Sub-agent roster, model tier per role (light/heavy), prompt-template versions, budget defaults (provisional task-specific time/cost/iteration/concurrency ceilings; bounded by owner authorization) |
| **Value objects** | `Capability`, `SubAgentKind`, `ModelTier`, `PromptVersion`, `TaskBudget`, `RetrievalMode` (treatment or control), `FrozenConfiguration` (an immutable, serialisable copy) |
| **Invariants** | Freezing always produces a complete configuration. One active configuration per Employee kind. |
| **Events** | none needed (configuration changes are operator actions) |

### 6.3 `conversation`

Conversation owns durable message history and its `origin: direct|wizard_question|roadmap_step`. Eight baseline `MessageKind` values remain: `text`, `text_response`, `structured_form`, `task_status`, `deliverable_card`, `knowledge_notification`, `budget_extension_request`, `error`. Add `post_reference` with `{postId, postVersionId?, taskId?, campaignId?}`: it renders a Scheduling link, not duplicated document content. Register all nine discriminated payloads in OpenAPI and frontend exhaustive renderers.

`StructuredForm` contains stable field IDs, text/number/link/radio/checkbox/dropdown types, choice labels/values, required flags, custom-answer support and legitimate unknown/skip/not_applicable options. Default to 3–4 meaningful Brain-derived options plus Other. Necessary real values remain free input. Task clarification differs from Discovery question state; both share a rendering schema, not ownership.

Every durable Task has one non-null `conversationId` scoped to its Workspace. Direct message creation validates conversation ownership before requesting Tasks. A step/question launch may allocate a new conversationId in the command; `TaskRequested.v1` is consumed by conversation to create that conversation and its validated origin **in the same transaction**. A preexisting conversation must have been validated by conversation's command path. This avoids a reverse tasks → conversation call or cross-module SQL. User-supplied origin IDs alone cannot allocate authority.

Question origin includes the tasks-owned researchLinkId/draftId/questionId; step origin includes roadmapId/roadmapVersion/stepId. Messages are append-only in ordinary use, subject to governed redaction/deletion. `TaskRequested`/result events append exactly once using eventId, and typed result references remain stable after navigation. Direct Chat has no Use in Roadmap action. Conversation creation/list/detail/messages remain conversation-owned; execution queries stay tasks-owned.

### 6.4 `tasks`

| Kind | Items |
|---|---|
| **Aggregates** | `Task` (conversationId, validated origin, type, request, status, frozen configuration ref, constraint diagnosis, budget: `TaskBudget`, cost so far) containing `Clarification` (form, answers, asked at, expires at = +7 days), `BudgetExtensionRequest` (reason, additionalCostUsd, additionalTimeMinutes, expectedBenefit, status: pending/approved/rejected), and `SubAgentWork` (sub-agent, input/result summaries, user-visible tool labels) |
| **Value objects** | `TaskStatus` (requested, running, awaiting_clarification, awaiting_budget_approval, completed, partially_completed, failed, cancelled), `ConstraintDiagnosis` (offer_audience, destination, conversion, acquisition, existing_customer; capacity/fulfillment constrain all lenses), `TaskBudget` (timeAllowanceMinutes, tokenAllowance, toolCallAllowance, paidSearchComputeAllowance, costCeilingUsd), `Cost`, `StepResult` |
| **Policies** | `TaskCapacityPolicy` (≤ 2 running per workspace), `ClarificationExpiryPolicy` (7 days), `PartialResultPolicy` (time or cost ceiling reached), `BudgetGovernancePolicy` (FR-27: per-task authorized budget; child work and retries share parent budget; hitting budget ceiling halts execution and requests owner approval; refusal produces strongest supported partial result; agents cannot switch tools or split tasks to evade budget) |
| **Read models** | `CanvasSnapshot` (Task status; per actual node: agent/tool/service/workflow identity, planned / active / awaiting_input / held / completed / error / cancelled state and safe activity/evidence labels; active connections; `asOfEventId`). Built by a query, never stored by `tasks` (§8.1.3). |
| **Invariants** | Transitions only as in the Task state diagram. At most one open Clarification. Accounted consumption never decreases; unused reservations can be released explicitly. Any research budget increase requires explicit owner approval before further consumption. |
| **Events** | `TaskRequested`, `TaskStarted`, `ConstraintDiagnosed`, `ClarificationRequested`, `ClarificationAnswered`, `ClarificationExpired`, `TaskBudgetExtensionRequested`, `TaskBudgetExtensionApproved`, `TaskBudgetExtensionRejected`, `TaskCompleted`, `TaskPartiallyCompleted`, `TaskFailed`, `TaskCancelled`, `ResourceConsumed` |

### 6.4.1 Discovery, Roadmap and step contracts

All these domain records belong to `tasks`; there is no new service. Roadmap execution and generation reuse TaskExecutionWorkflow.

| Record | Required fields / invariant |
|---|---|
| `RoadmapDraft` | id, workspaceId, version, status draft/generating/proposed/archived, goal, relevant constraints, currentQuestionId, questionSequenceVersion, generatedTaskId?, proposedRoadmapId?, contextRefs; server-persisted resume state |
| `DiscoveryQuestion` | Stable id within draft, questionVersion, semanticKey, prompt/form schema, applicable/required/reason, sequence/order, basis refs. Adaptive changes retain old questions/answers as history; unrelated answers survive |
| `DiscoveryAnswer` | id, draftId, questionId/version, version, status unanswered/answered/skipped/unknown/not_applicable/researching/confirmed, value?, selectedResultRef?, source/owner/time/context refs. Special states carry no fabricated positive value |
| `QuestionResearchLink` | id, workspaceId, draftId, questionId/questionVersion, conversationId, taskId, status active/completed/unavailable, resultRefs, selectedResultRef?, return target derived as `/roadmap/drafts/{draftId}/questions/{questionId}`; every resource validated server-side |
| `GoalAndConstraints` | Owner-selected objective, actual route, optional material assets/skills/permission/fulfillment/business-budget constraints, success definition and relevant baseline/target labels. No mandatory weekly-owner-hours field. Business budget, computational allowance and external spend authority are separate |
| `Roadmap` | id, workspaceId, status proposed/active/archived/rejected, activeVersion?, sourceDraftId, goalId, version. Partial unique index on workspace for status active; workspace lock for activation/replacement |
| `RoadmapVersion` | Immutable plan snapshot with id, roadmapId, version, goal/context/evidence/uncertainty, ordered step IDs, createdAt, approval decision, baseVersion?; old versions archived on replacement |
| `RoadmapStep` | Stable step id, roadmapId/version, title, objective, rationale/evidence/uncertainty, party, executionMode analysis_draft/connected_execution/owner_execution/blocked/later_phase, requiredInputs, expectedOutput, completionCondition, estimatedEffort, dependencies, externalApprovalRequirements |
| `StepExecution` | roadmapId/version/stepId, projectionVersion, taskRefs, documentRefs, postRefs; workState planned/ready/running/awaiting_input/awaiting_approval/completed/blocked/failed/cancelled/skipped/not_applicable/superseded; actionState not_required/awaiting_owner/submitted/verified/uncertain/failed/held/cancelled; ownerAttestation separate |
| `OwnerAttestation` | status none/reported_done, reporting owner/time, step and artifact/version refs, completion checklist, optional eligible evidence refs. UI says **Owner-reported Done**, never platform-verified |
| `StepDecision` | Skip/Revisit/Not Applicable intent, actor/time, reason?, affected dependencies and pre/post projection versions; skip is not a Task cancellation |
| `StrategicRevision` | id, roadmapId, baseRoadmapVersion, version, proposed plan, diff added/modified/removed-or-superseded/dependency changes, rationale/evidence/constraints, status proposed/approved/rejected/superseded and owner decision |

Generation uses current eligible context and saved answers; the first significant question selects goal and subsequent questions adapt. Known confirmed context is reused with provenance. Indispensable missing values require an explanation; otherwise preliminary plans disclose uncertainty. Confirmed answers adopt explicit owner decisions/facts through Brain's public contract; AI interpretations beyond selected values remain separate Candidates. Skip/unknown/not_applicable never synthesize positive facts. Later drafts reuse only currently eligible accepted knowledge, not a stale historical answer as current authority.

**Research return:** save draft/answer version before launch; create one linked Task/conversation idempotently. `UseResearchResult` validates the exact linkage, question version, result ownership/type and draft editability; it stores selected/edited answer as answered, returns the canonical question destination, and does not activate knowledge yet. `ConfirmDiscoveryAnswer` is the separate owner confirmation. A changed/deleted question or plan returns stale/unavailable, never redirects to an arbitrary user-supplied URL. Ordinary Chat and roadmap execution cannot invoke this command.

**Execution and dependency rules:** recommended order is presentation only. Validate dependency DAG (no cross-workspace refs or cycles), prerequisite completion semantics and required inputs on launch. An owner-reported prerequisite satisfies only an explicitly owner-attestable condition, never a verified-platform requirement. A skipped prerequisite remains unmet unless an approved alternate path/revision changes it. Every applicable step can be skipped; if running, retain its Task and actual state separately and offer a distinct cancel. Revisit restores planned/ready/blocked from current evidence without creating a Task. In-flight completion remains recorded but cannot silently undo skip intent. Reopen never relaunches; explicit rerun requires a new authorized intent and attempt reference.

**Draft editing:** draft/proposed drafts accept versioned edits; generating drafts are locked until completion or cancellation and archived drafts are read-only. Editing a proposed draft invalidates its unactivated proposal (rejected with reason source_changed) and returns the draft to draft; saved prior proposals remain history. Activation verifies the exact proposal and source-draft versions and archives that source draft. Initial proposal rejection leaves the active plan untouched and returns its source draft to editable draft.

**Activation/revision:** draft → generation Task → proposed plan → owner approval → active. Approval atomically checks base/proposal versions and one-active uniqueness, archives previous plan/version and records the decision. Rejection preserves active plan. Revisions can follow owner request, goal/context changes or accepted research/skip impact; no periodic trigger. Removed steps are superseded in the new plan; running Tasks/Post approvals retain original immutable origin refs and are not cancelled, reapproved or relaunched by revision.

### 6.4.2 Runtime resources and state

`TaskStatus` remains requested/running/awaiting_clarification/awaiting_budget_approval/completed/partially_completed/failed/cancelled. Requested includes capacity waiting with `waitingReason=capacity`; hard maximum **two running Tasks per Workspace**, independent of conversations, enforced by advisory transaction lock. Paused Tasks release their execution slot; approved continuation or valid clarification returns to requested and must reacquire a slot before running; no over-admission when answers arrive concurrently. Clarification expiry remains seven days. Cancellation stops further internal work and preserves truthful partial results.

`ResourceReservation` records parent allowance/authorization, reserved/settled/released usage per call, including children, concurrency and retries. Reserve bounded worst-case before calls; pending budget extension stops new consumption. Explicit approval adds recorded resources; refusal yields the strongest supported partial result. Router/quick replies also get a durable usage-request identity and bounded permit through tasks before read-only ai-http; timeout cannot release possibly spent usage without reconciliation. No tool/task splitting can evade limits.

Step work state, Task runtime and platform action are separate. Awaiting input/approval exposes the exact pending decision, while recoverable internal attempt failures remain `running` and public Canvas `active` labelled Processing. Policy exhaustion produces terminal failed/partial states. Private diagnostics retain attempt causes/counters; there is no universal three-attempt product promise.

### 6.5 `brain`

| Kind | Items |
|---|---|
| Aggregates | `KnowledgeItem` (category, title/summary/properties, epistemic kind, lifecycle, approval, verification, source/extraction status, version, provenance/freshness); `Relationship` (typed endpoints and their versions, lifecycle/provenance); `ProceduralRule` (explicit owner or approved inferred working preference, separate from business observations); `Conflict` (versions, evidence, unresolved/resolved decision) |
| Value objects | `KnowledgeCategory`: company_profile, offering, audience, brand_rule, positioning, strategic_goal, channel, competitor, company_fact. Seven proposal-review UI groups map by category/typed subtype as defined in DESIGN-SYSTEM §9.5; these are presentation groups, not new knowledge categories. `LifecycleState`: active/candidate/rejected/retired/superseded/deletion_pending/deleted. `ApprovalState`: not_required/pending/approved/rejected. `EpistemicState`: source_observation/owner_claim/ai_hypothesis/owner_decision. `VerificationStatus`: unverified/owner_confirmed/independently_supported, with basis and date. `Provenance`: origin, URL/page or owner/task ref, captured/imported time, extraction status and uncertainty rationale. No arbitrary confidence percentages. |
| Origin policy | `CandidateOriginPolicy`: initial confirmed intake → active + approved + source_observation or owner_claim/owner_decision, with confirmationId and source provenance; unconfirmed intake is ineligible; explicit owner input/edit → active + not_required + owner_claim (or explicit owner decision/preference). AI-proposed persistence → candidate + pending, whether derived from chat, research, requested analysis or edit inference. Approval sets active + approved without falsely changing verification. Re-import adds observations/conflicts, never silently overwrites established versions. |
| Other policies | `ReviewPolicy`: individual proposal decisions as a retained implementation choice. `SensitiveInfoPolicy`: allow prices, marketing budgets, aggregate spend and relevant outcomes; reject credentials, payment-account secrets and unnecessary customer-level/personal financial records. `RetirementPolicy`: legitimate historical withdrawal. `DataDeletionPolicy`: deny retrieval immediately and durably purge/redact applicable stores (§9.9). `ContextStalenessPolicy`: version material offer/price/audience/service-area/claim-support changes and notify dependent tasks/posts. |
| Invariants | Relationships are workspace-local and retrieval requires eligible endpoints and edge. Current owner-confirmed intake/owner-provided/approved context is usable with labels; pending/rejected/deleted/retired/superseded data cannot be current authority. Approving persistence is not independent verification. Every material edit creates a version. Deletion overrides historical retention and late indexing. |
| Events | `KnowledgeImported`, `OnboardingKnowledgeActivated`, `KnowledgeOwnerProvided`, `KnowledgeCandidateProposed`, `KnowledgeItemApproved`, `KnowledgeCandidateRejected`, `KnowledgeItemEdited`, `KnowledgeItemRetired`, `RelationshipAdded/Retired/Deleted`, `KnowledgeConflictDetected`, `ProceduralRuleApproved`, `ContextVersionChanged`, `KnowledgeDeletionRequested/Completed` |

`ContextVersionChanged.v1` contains changed item/version identifiers and impact classes, not unredacted content. `tasks`, `deliverables` and `publishing` own their dependency maps and react through public events in the same transaction: flag only dependent work, apply `PostHeldOnStaleness.v1` to conflicting scheduled posts, and preserve unrelated work. Record owner resolution before clearing a hold; changed approved details require fresh approval. No automatic regeneration, research extension, or send follows a context change (FR-48).

### 6.6 `crawls`

| Kind | Items |
|---|---|
| **Aggregates** | `OnboardingCrawl` (URL, pages ≤ 10, progress, outcome, Brand Kit detection) · `ResearchCrawl` (Task ref, sources tried/skipped, items, time and cost spent) |
| **Entities** | `SnapshotRecord` (object key, URL, platform, captured at, reference count) |
| **Value objects** | `CrawlOutcome` (complete, partial, failed), `SourceSkip` (source, reason), `SourceRef` |
| **Policies** | `OnboardingBounds` (≤ 10 pages, ≤ 5 min, robots.txt respected, configurable), `OneCrawlPerWorkspace`, `RetentionPolicy` (keep while referenced; unreferenced raw research data 7 days) |
| **Events** | `OnboardingCrawlStarted`, `OnboardingCrawlCompleted`, `OnboardingCrawlFailed`, `BrandKitDetected`, `ResearchCrawlCompleted`, `SourceSkipped` |

### 6.7 `deliverables`

| Kind | Items |
|---|---|
| **Aggregates** | `Deliverable` (type, title, content, Task ref, partial flag) · `AnalysisUpload` (conversationId, authorized analysisTaskId, fileFormat: csv/xlsx, object key, schemaMappingPreview, mappingVersion, validationOutcome, deterministicMetrics, source/period/units) |
| **Value objects** | `DeliverableType` (research_report, competitor_analysis, audience_research, marketing_strategy, positioning_report, campaign_plan, marketing_brief, visual_brief, content_strategy, analytical_report, general), `SourceCitation` (URL, platform, accessed at), `DeliverableContent` (Markdown or structured), `SchemaMappingPreview` (metric definitions, date range, granularity, units/currency, identifiers, FR-38) |
| **Policies** | `DeterministicMetricsPolicy` (FR-38, FR-39, AC-25: metrics computed strictly by application code; zero-denominator protection; incompatible currencies flagged; duplicate rows and overlapping totals quarantined; spreadsheet macros/formulas never executed) |
| **Invariants** | Each version is immutable during ordinary editing; new revisions retain task/goal, context versions, source/assumption refs and creation time. Owner deletion/redaction is a separate governed operation. Consequential factual claims have inspected supporting context/citations or explicit owner-claim/hypothesis labels; unsupported material claims are omitted, qualified or queried (FR-26, FR-30). Spreadsheets parsed without formula execution. |
| **Events** | `DeliverableCreated`, `AnalysisUploadRejected` |

### 6.8 `publishing`

| Kind | Items |
|---|---|
| **Aggregates** | `Post` (nullable draft destination ConnectedAccount ID, target platform: facebook_page, postFormat: text_link / single_image, nullable proposed scheduled time, latenessAllowanceMinutes, status, version counter, origin: direct_chat/roadmap_step/campaign/manual, conversationId?/taskId?/roadmapId?/roadmapVersion?/stepId?/campaignId?) containing `PostVersion` entities (immutable text/link, optional validated ownerMediaId + mediaVersion/hash, exact destination account, scheduled instant/zone/allowance, author, created at), an `Approval` value (postVersionId binding all execution details, approving owner/time, channel: single or bulk), and `PublishAttempt` entities (idempotency key, started at, outcome: started / submitted / confirmed / failed / uncertain, provider request/post IDs and URL, timestamps, read-back evidence and authorization generation) · `ReviewSession` (user, optional campaignId and explicit postIds, started at, last activity at, ended at, end reason: finished / left / abandoned, counts of approvals, rejections, edits and regenerations) |
| **Value objects** | `PostStatus` (draft, pending_approval, approved, rejected, scheduled, held, publishing, submitted, uncertain, published, failed, cancelled, expired), `ScheduledTime` (UTC instant + planning IANA zone), `Platform` (facebook_page in Phase 2; instagram_business and x prepared for Phase 3), `PostFormat` (text_link, single_image), `PostContent`, `IdempotencyKey`, `FailureReason` |
| **Policies** | `BulkDecisionPolicy`: a bulk approve/reject acts only on the Posts and versions the User listed, each Post decided on its own (FR-34). `ReviewSessionPolicy`: one open session per user per workspace; starting a new one closes the previous as abandoned. `PostTransitionPolicy` (the full state machine, SYS §3.4, including `held` on context staleness). `ClaimPolicy`: no dispatch for undated/unconnected draft; exact approved text/media/account/time/zone/allowance, connected authorization generation, no cancellation/context hold, valid delivery window; atomic claim/attempt registration. Revalidate immediately before dispatch; pending past schedule → expired; approved past window → held. `LatenessAllowancePolicy` (FR-35: post dispatches only within scheduledTime + latenessAllowanceMinutes; if window passes, post enters held for a new time and approval). `RetryPolicy`: provisional two retries only for confirmed safe failures after fresh preflight within the allowance; ambiguous acceptance → uncertain → reconcile or hold. Manual retry is also gated and cannot erase an uncertain attempt. `PlatformLimits`: verify Facebook format/access constraints early; Instagram/X quotas and adapters are Phase 3 investigations. |
| **Invariants** | Approval binds to exactly one Post Version, exact account, scheduled time/zone, and lateness allowance (FR-34). A new version clears Approval. At most one successful attempt per Post. Cancellation requests may arrive during a claim race: prevent dispatch if the cancellation wins; if the provider has accepted, report the actual outcome and owner-removal limitation rather than claiming reversal. Changing the business zone never moves an approved or scheduled Post. Material context changes place conflicting scheduled posts into `held` state (FR-48). Timeouts transition attempts to `uncertain` (FR-36). |
| **Events** | `PostDrafted`, `PostSubmittedForApproval`, `PostVersionCreated`, `PostApproved`, `PostRejected`, `PostScheduled`, `PostRescheduled`, `PostHeldOnStaleness`, `PostHoldResolved`, `PostCancelled`, `PostExpired`, `PostSubmitted`, `PostOutcomeUncertain`, `PostPublished`, `PostPublishFailed`, `PublishAttemptRecorded`, `CampaignCreated`, `ReviewSessionStarted`, `ReviewSessionEnded` |

#### 6.8.1 First-class Posts and calendar

`PublishingApi.createPosts(workspaceId, validatedOrigin, outputs)` is called directly by tasks during result assembly; a manual `CreatePost` sets origin=manual and rejects client-forged Task/step provenance. No DeliverableCreated subscriber creates Posts. One mixed result transaction may create a campaign planning document and six Posts through separate contracts and link returned IDs. `Campaign` in publishing groups Posts with optional document/task refs; it is organic grouping, not paid advertising authority. Deliverables retains applicable media/blob validation, exposed through a versioned public media projection; no duplicated Post text store.

Drafts may lack destination and schedule; `scheduledTime=null` means Unscheduled. Save/edit never approves. Version snapshot freezes text/link, immutable media version/hash, destination, UTC instant, planning zone and lateness allowance when set. Submit-for-approval requires all execution parameters; “post now” resolves to an explicit disclosed instant/window before approval. Every content/destination/time/zone/allowance edit creates a new immutable version, clears approval, and returns previously governed editable content to pending_approval only if complete, otherwise draft. Published/uncertain in-flight records cannot be edited into a resend; duplicate a published Post creates a fresh draft identity.

Calendar query uses inclusive `from` / exclusive `to` UTC boundaries derived from the selected IANA display zone. Projection `calendarAt` uses actual `publishedAt` for published records, proposed scheduled instant otherwise, with `dateBasis: actual|proposed`. Null dates appear in Unscheduled; a published record with unknown actual time exposes evidence-unavailable and is not placed at a guessed historical date. Stable cursor `(calendarAt,id)` is bound to range/filter; list and month/week consume the same query. Status/platform/campaign filters and `limit,cursor` apply. Show unapproved scheduled proposals distinctly; date presence is not send authority. Retain both scheduled and actual times in details. Changing workspace timezone changes formatting/range boundaries only, never approved version parameters.

Batch Review `ReviewSession` remains publishing-owned and measures only Post decisions; it has no recurring marketing-performance semantics. Its source is campaignId?/postIds, not a mandatory document. Notifications use exact Post subjects.

### 6.9 `connections`

| Kind | Items |
|---|---|
| **Aggregates** | `ConnectedAccount` (platform, platform account id, status, credential envelope, credential version, connected at) |
| **Value objects** | `CredentialEnvelope` (opaque ciphertext + encryption context `{workspace_id, integration_id, credential_version}`), `AccountStatus` (connected, reconnect_needed, disconnected) |
| **Invariants** | OAuth callback tokens are transiently received and KMS-encrypted in api; only workers-publish decrypts persisted envelopes for authorized dispatch. Connect from settings or a lazy publishing prompt; connection is not action/analytics approval. Disconnect always blocks future sends and increments authorization generation; reconnect never resumes overdue/uncertain work implicitly. |
| **Events** | `AccountConnected`, `AccountReconnectNeeded`, `AccountDisconnected` |

### 6.10 `evaluation`, `usage`, `audit`, `notifications`

| Module | Model |
|---|---|
| `evaluation` | Aggregates `Scenario` (category, seeded workspace, request, expected Deliverable type, rubric dimensions) and `EvaluationRound` with `ScenarioRun` (mode: treatment or control; Deliverable ref; clarification and correction counts) and `ReviewerScore`. Events `ScenarioRun`, `DeliverableScored`. Rule: same model and prompts in both modes; two reviewers, third on a disagreement > 1 point. |
| `usage` | `UsageRecord` (append-only): resource kind (model tokens, search, retrieval/crawl/proxy, integration, compute), quantity, actual/estimated cost, parent Task/call ref and failure/retry status |
| `audit` | `AuditEvent` (append-only): action, actor (user, employee, system), subject, at |
| `notifications` | `Notification`: id/workspaceId/ownerId/version, kind, typed subject and version, occurredAt, readAt?, deduplication eventId. Subject types conversation_form/task/deliverable/post/roadmap_revision/brain_proposal/connection; server constructs allowlisted deep links and checks availability. Read is not subject resolution. |

---

## 7. Data access

### 7.1 Repositories
- **One repository per aggregate root**, in `modules/<m>/infrastructure/repositories`. It loads and saves the **whole aggregate**, including child entities such as Post Versions and Clarifications.
- **Every method takes `workspaceId` as its first argument.** There is no unscoped read or write.
- Query use cases (`application/queries`) may read their **own** module's tables directly through Drizzle for list and detail views, still workspace-scoped. They never read other modules' tables.
- Repositories return **domain objects** (via mappers), never Drizzle rows.
- Writes use **optimistic concurrency** (`version` column) on Post and Knowledge Item, and append-only inserts on audit and usage.

### 7.2 Data ownership and schemas

| PostgreSQL schema | Owner module | Main tables (storage names) |
|---|---|---|
| `identity` | identity | `users`, `local_credentials`, `auth_identities`, `workspaces`, `onboarding_drafts`, `onboarding_confirmations`, `email_verifications`, `password_resets`, `data_deletion_requests`, `restore_suppressions` |
| `employee` | employee | `employee_configurations`, `capabilities` |
| `conversation` | conversation | `conversations`, `messages` |
| `tasks` | tasks | `tasks` (conversation_id required), `clarifications`, `subagent_work`, `roadmap_drafts`, `discovery_questions`, `discovery_answers`, `question_research_links`, `roadmaps`, `roadmap_versions`, `roadmap_steps`, `step_executions`, `step_decisions`, `step_dependencies`, `strategic_roadmap_revisions`, `goals`, `owner_attestations`, `resource_reservations`, `budget_decisions` |
| `brain` | brain | `brain_nodes` (= Knowledge Items), `brain_node_versions`, `brain_edges` (= Relationships), `brain_procedural_rules`, `brain_conflicts`, `brain_index_state`, `intake_activations` (unique confirmation_id) |
| `brain_read` | brain (views only) | read-only versioned eligibility views for confirmed-intake/owner-provided/approved items, eligible relationships, adopted procedural rules and provenance; workspace isolation enforced by roles/RLS |
| `conversation_read`, `tasks_read`, `deliverables_read` | respective source module (read-only views) | safe historical projections with dates, source/context dependencies, rejected/deleted suppression and labelled historical authority; no cross-module base-table joins |
| `crawls` | crawls | `onboarding_crawls`, `research_crawls`, `snapshots` |
| `deliverables` | deliverables | `deliverables`, `deliverable_versions`, `source_citations`, `analysis_uploads`, `publishing_media`, `export_jobs` |
| `publishing` | publishing | `posts`, `post_versions`, `post_approvals`, `campaigns`, `post_context_dependencies`, `post_holds`, `publish_attempts`, `review_sessions` |
| `connections` | connections | `connected_accounts` |
| `evaluation` | evaluation | `scenarios`, `evaluation_rounds`, `scenario_runs`, `reviewer_scores` |
| `usage` | usage | `usage_records` |
| `audit` | audit | `audit_events` (append-only; ordinary writes append-only; privileged governed redaction/deletion and retention jobs remove payloads when required) |
| `notifications` | notifications | `notifications` |
| `platform` | platform/idempotency | `idempotency_keys` (key, workspace, request hash, response snapshot, expiry) |
| `ai_runtime` | **busmora-ai** (technical, not domain) | LangGraph checkpoints. The schema and grants are created by a Drizzle migration; the tables inside are managed by the LangGraph checkpointer (BTD-ADR-09). |

The storage names `brain_nodes` and `brain_edges` are kept from the product docs for traceability. Code uses the glossary terms `KnowledgeItem` and `Relationship`, and the mappers are the only place both names meet.

**Cross-module references** are ID columns without foreign keys. The exception is `workspace_id`, which every table carries, with a foreign key to `identity.workspaces`. **Workspace deletion** is orchestrated module by module by `WorkspaceDeletionWorkflow` through each module's deletion activity, so no cascading foreign key crosses modules.

### 7.3 Transactions
- **One command = one transaction**, opened by the use case through `TransactionManager.run()`. Repositories, contract implementations and same-transaction event handlers all join it automatically through the ambient context.
- **Cross-module writes** happen only by calling other modules' contract interfaces *inside* the same transaction. Example: `CompleteTask` → `DeliverablesApi.createDeliverables` / `PublishingApi.createPosts` / `BrainApi.proposeCandidates`; handlers then append usage, audit, notifications and conversation messages.
- **After-commit hooks** start or signal workflows, and are idempotent:

  | Workflow | Workflow id |
  |---|---|
  | `TaskExecutionWorkflow` | Task id |
  | `PostReleaseWorkflow` | Post id + schedule revision, one active release per Post |
  | `DataDeletionWorkflow` / `DataExportWorkflow` | Workspace + request id |
  | `IndexingWorkflow` | item id + version |

  If a hook fails after commit, a scheduled **reconciliation** workflow finds rows whose workflow should exist (e.g. a Task `requested` with no running workflow, or an item whose index version lags) and starts it.
- **Isolation:** the default is read committed. **Advisory transaction locks** keyed by workspace serialise capacity checks and Post claims.
- **Activities are idempotent.** Each "platform" activity carries the workflow id plus a step sequence number, and the owning module records processed step keys. A Temporal retry therefore re-applies nothing.

### 7.4 Migrations
- **Drizzle is the single toolchain** (SYS ADR-008).
  - Table definitions live in each module's `infrastructure/schema/`.
  - There is **one ordered migration history** in `platform/db/migrations/`.
  - Generated SQL is reviewed in the pull request.
  - Roles, grants, views and row-level-security policies are migrations too (`platform/db/roles/`).
- **Expand → migrate → contract.** Columns are added before they are used and removed only after no code, view or in-flight workflow depends on them. Month-long workflows and Python's views make breaking changes unsafe.
- **Views are a contract** with Python. `brain_read` and the module-owned historical read views change only additively; version the schemas without leaving an obsolete consumer able to bypass eligibility/deletion checks. A breaking change means a new view name, with both kept until Python is migrated.
- Migrations run as a step before api roles start, never from Python.

### 7.5 Python data access
- `ai-agent` and `ai-http` use **SQLAlchemy Core** (no ORM models) against `brain_read` views, through read-only roles with row-level security keyed to the verified workspace; module-owned `conversation_read`, `tasks_read` and `deliverables_read` views provide eligible historical projections. `ai-agent` also uses the `ai_runtime` schema for checkpoints.
- Every Qdrant query applies workspace + `retrieval_eligible=true` + current lifecycle/version filters inside `VectorIndex`, then revalidates each hit against `brain_read` before assembly. Eligibility means current owner-confirmed intake/owner-provided context or owner-approved AI persistence. Labels remain in results. Every graph hop checks the edge and both endpoints; no retired/superseded/deleted/rejected/pending item can leak through expansion.
- `brain_read.eligible_items_v1`, `eligible_relationships_v1` and `adopted_rules_v1` expose version, lifecycle, epistemic kind, approval, verification, provenance/freshness and deletion generation. Source metadata belongs to `crawls`; copy only the needed public projection via its contract/events, not cross-module base-table joins. Domain editors use separate authorized queries to inspect pending/history states.
- Historical work retrieval reads module-owned `conversation_read`, `tasks_read` and `deliverables_read` views (only each owner’s own tables, with identical tenant/eligibility grants), marked historical with dates/limitations and source dependencies. Exclude/redact rejected proposals and deleted content, including inferred preferences in old chats. Superseded observations cannot be current authority. If a source cannot be safely redacted at claim/span level, exclude that whole historical record/chunk until rebuilt safely. The document-only evaluation route must apply identical suppressions; raw snapshots/chunks cannot bypass governance.
- Indexing runs after import, owner input, approval, edits, retirement, supersession and deletion. Jobs carry version + deletion generation, check current eligibility before and after writing, and remove stale points. Retrieval revalidation protects the asynchronous gap. Checkpoints revalidate context on resume; deleted/stale context cannot be replayed into new artifacts or action approval.
- `busmora-workers` has **no database access**. Everything it needs arrives in activity results.

---

### 7.6 Redesign schema and compatibility treatment

These documents specify a prospective target; they do not establish production data exists. For a fresh implementation create only the current schema. If deployed v1.1 data/consumers exist, first inventory schema, consumer versions, pending approvals, running Temporal histories and retention duties. Expand with auth identities/optional local credentials, intake drafts, discovery/step/version/link tables and nullable Post schedule/origin/campaign fields. Backfill only evidenced relationships; unmappable origins remain explicit historical metadata, not invented ownership.

Retire exclusive daily-aggregation and recurring-performance-review endpoints/jobs/tables and fixed planning-column fields. Historical review decisions/observations may be retained as access-controlled audit/history under deletion policy, not served as a current feature or recurring schedule. Cancel only retired periodic triggers after recording their disposition. Preserve Post Batch Review `review_sessions`, publishing attempts, approvals, provenance and safe in-flight workflows. Migrate legacy document-backed social content to one authoritative Post lineage with original approval hashes/evidence; never duplicate/send it or reinterpret old approval as approval of changed details. Legacy imported Brain values with no owner confirmation become ineligible intake proposals until confirmed; suppress derived indexes before exposing the new retrieval policy.

Publish explicit versioned compatibility contracts if old clients are deployed, migrate consumers, replay-test patched workflows, then contract obsolete storage only after no live consumer/history needs it. Compatibility adapters may expose safe tombstone/unavailable responses; no active obsolete workflow remains in the target release. Preserve restore suppressions, audit integrity and backup retention. No unreviewed destructive drop or automatic external-effect replay is authorized by this documentation revision.

## 8. API layer conventions

All routes are `/v1`, generated from NestJS DTOs/controllers with Swagger and `oasdiff`; frontend pins the version and generates types. This is a prospective revised contract, not evidence of deployed endpoints. Existing deployed consumers, if any, require explicit compatibility migration (§7.6); no silent breaking change to a published version.

Tenant auth derives Workspace/owner from the verified access token, never body IDs. Cross-workspace resource IDs return 404. Queries require the same scope. Origin/auth callbacks are the limited public exceptions described in §8.2. Every application command requires `Idempotency-Key`; provider redirect callbacks instead use atomic single-use state/code validation because browsers cannot attach this header. Auth commands additionally enforce one-time-token replay safety, with workspace/actor/operation/key/request hash and original result persisted; changed body under a key fails 409 `IDEMPOTENCY_CONFLICT`. Auth keys are scoped to anonymous flow/session where no Workspace exists. Snapshot resource versions are separate from immutable Post Version IDs: edits/decisions send `version` and, for Posts, `postVersionId`. Aggregate mismatch is 409 `STALE_VERSION`; related-target mismatch 404; invalid transition 409; validation 400; domain precondition 422; rate limit 429; unauthenticated 401; genuine forbidden action 403; dependency unavailable 503. Capacity waiting returns accepted 202 with requested Task and waitingReason, not simulated execution.

Error envelope: `{error:{code,messageKey,details,correlationId}}`. Codes also include `VALIDATION_FAILED`, `RATE_LIMITED`, `BUDGET_CEILING_REACHED`, `DEPENDENCY_BLOCKED`, `ORIGIN_UNAVAILABLE`, `RESEARCH_LINK_INVALID`, `RECONNECT_NEEDED`, `CONTEXT_STALENESS_HOLD`, `UNRESOLVED_PUBLISH_OUTCOME`, `ANALYSIS_FILE_UNRECOGNIZED`, `INVALID_ANALYSIS_METRIC`. Validation rejects unknown properties; server state machines decide allowed actions. DTOs expose `allowedActions` and blocker reasons for presentation only.

Cursor lists return `{items,nextCursor}` with stable sort, configured limits and filter-bound cursors. UTC ISO instants plus explicit IANA zones; ScheduledTime includes `{instant,zone,latenessAllowanceMinutes}`. Asynchronous commands return 202 plus resource IDs/status and canonical links; completion is polled or real Task SSE. CORS allows only configured app origin with credentials; refresh retains Origin/custom-header checks.

SSE `GET /v1/streams/tasks/{taskId}` uses the scoped HttpOnly stream cookie and tasks workspace ownership, current revocation checks, heartbeats/reset and bounded replay. `Last-Event-ID` wins over optional initial `after`. Only safe public signals are delivered. Snapshot endpoint below supplies stream position; no historical replay after buffer expiry.

### 8.1 Batch Review and Canvas endpoints (frontend gap FG-10)

The Batch Review and Canvas endpoints close the specified portions of FG-10 in [FRONTEND-TECHNICAL-DESIGN.md](./FRONTEND-TECHNICAL-DESIGN.md). Each one follows the conventions in the table above. These contracts are additive to `/v1`; further required owner workflows follow below.

#### 8.1.1 Bulk approve / reject the remaining Posts
**Source:** FR-34 (selected/batch exact-version approval; bulk reject is a retained UI convenience), SYS §5.3 (Batch Review), the Approval invariant in §6.8.

| Item | Specification |
|---|---|
| **Endpoints** | `POST /v1/posts/bulk-approve` · `POST /v1/posts/bulk-reject` (owned by `publishing`) |
| **Headers** | `Idempotency-Key` required |
| **Request** | `{ items: [ { postId, version, postVersionId } ] }`: 1 to N items (N = configured maximum, G-16). Duplicate `postId`s are a validation error. |
| **What "remaining" means** | The client sends exactly the Posts that are still undecided **on the User's screen**, each with the version the User saw. The server never expands "all remaining" by itself, because an Approval must bind to a version the User actually saw. A Post that changed after the screen loaded comes back as `stale_version` instead of being approved unseen. |
| **Processing** | Each item runs the **same use case as the single command** (`ApprovePost` / `RejectPost`), in **its own transaction**, in request order. One item's failure never rolls back another (the Post is the consistency boundary). Each successful item registers its own after-commit hook (signal to its `PostReleaseWorkflow`) and emits its usual events. Approvals record `channel = bulk`. Each item gets its own audit event, all carrying the request's correlation id. |
| **Per-item outcomes** | `approved` · `rejected` · `already_decided` (the same version was already approved or rejected: a safe no-op, so retries stay correct) · `stale_version` · `invalid_transition` · `not_found` (also for another workspace's ids) |
| **Response** | `200` whenever the request itself is valid: `{ results: [ { postId, outcome, post? } ], counts: { succeeded, failed } }`. `post` is the updated Post summary when it changed. Whole-request failures use the normal envelope: `400` (empty, too many, duplicates), `401`, `429`. |
| **Idempotency** | The whole result is stored under the key; a retry with the same key returns it. If the process stops before the result is stored, a retry re-runs the items, and those already done return `already_decided`. |
| **Not affected** | AI Brain proposals stay individual-only as this design's chosen UI (FR-10); initial intake has its own exact-version confirmation: there is no bulk endpoint under `/brain`. |

#### 8.1.2 Batch Review Session tracking
**Source:** EVAL §6 (Batch Review Efficiency ≤ 15 minutes) and EVAL §8 ("Review time per batch — Batch review session timestamps"), SYS §7.4 (product instrumentation in PostgreSQL).

| Item | Specification |
|---|---|
| **Endpoints** | `POST /v1/batch-review/sessions` (start) · `POST /v1/batch-review/sessions/{id}/end` (end). Both owned by `publishing`. |
| **Start** | `Idempotency-Key` required. Body `{ campaignId?, postIds? }` (explicit Posts being reviewed, if scoped) → `201 { id, startedAt }`. If the User already has an open session in this workspace, it is closed first as `abandoned` at its last activity. |
| **End** | Body `{ reason: "finished" \| "left" }` → `200 { id, startedAt, endedAt, durationSeconds, endReason, counts }`. Ending an already-ended session returns it unchanged, so the call is naturally idempotent. Another user's or workspace's session → `404`. |
| **Activity** | Measured **on the server**. `publishing`'s own handlers for the User's Post actions (approve, reject, edit → new version, accepted AI regeneration → new version, bulk approve/reject) update the open session's `lastActivityAt` and counts. The client sends no heartbeats, and Post commands don't change. |
| **Abandoned sessions** | A tab closed or a network lost leaves a session open. The maintenance schedule (§9.6) closes sessions idle longer than the idle timeout (value open, G-16) with `endedAt = lastActivityAt` (or `startedAt` if nothing happened) and reason `abandoned`. Duration therefore runs to the last real decision, not to the sweep. |
| **Clock** | All times come from the server's `Clock`; client timestamps are never trusted. |
| **Output** | Ending a session (any reason) emits the public event `ReviewSessionEnded.v1` `{ sessionId, workspaceId, userId, campaignId?, postIds?, startedAt, endedAt, durationSeconds, endReason, counts }`. `evaluation` stores the review-time measurement from it; `audit` records it. |
| **Data kept** | Timestamps and counts only; no Post content. Subject to the documented minimal audit/retention policy and workspace deletion. |

#### 8.1.3 Canvas snapshot for resets
**Source:** SYS §6.4 (reset → "the client refetches through queries"), SYS ADR-013 / ADR-020 (bounded live replay; five legacy agent signals plus execution_event), §9.7.

| Item | Specification |
|---|---|
| **Endpoint** | `GET /v1/tasks/{id}/canvas` (owned by `tasks`; normal access-token auth; workspace-scoped like every query) |
| **Response** | `200 { taskId, conversationId, taskStatus, nodes: [ { nodeId, kind, agent?, state, toolLabel?, evidenceRefs? } ], connections: [ { from, to, active } ], asOfEventId, live }`. `nodes` come only from the recorded plan/execution; each carries stable `nodeId`, `kind` (agent/tool/service/workflow), optional agent identity and planned/active/awaiting_input/held/completed/error/cancelled state. Recoverable attempt failures preserve active/Processing; only exhausted logical failures become error. Private attempt events never populate public nodes or snapshots. No fixed five-agent roster. `toolLabel` is the **user-visible** label only. `asOfEventId` is the stream position the snapshot reflects, or `null`. |
| **Live source** | Together with each signal, `busmora-ai` updates a **per-Task Canvas state hash** in Redis, in the same Redis transaction as the stream write (§3.4): node states, tool labels, active connections and the last event id. `platform/streams` reads it through `CanvasStateReader`. One small read; the stream is never replayed to build a snapshot. |
| **Durable fallback** | If the hash has expired (same retention as the stream, G-16) or the Task has ended, the snapshot is built from PostgreSQL: Task status, persisted plan/work records and durable safe action summaries give only known states, evidence links and gaps, with no invented tool activity or active connections; `asOfEventId = null`, `live = false`. For an ended Task, the durable states win over the hash. |
| **Client sequence** | On page load, or on a `reset` event: `GET …/canvas` → draw it → open the stream with `?after=<asOfEventId>` (§8 SSE row). If `asOfEventId` is `null`, open without `after` (live only). If `after` is already outside the replay window, the stream sends `reset` again; the frontend limits these retries. |
| **Ownership** | `tasks` builds the snapshot from its records and the reader, validating Workspace and returning conversationId. Conversation-local list queries validate that relationship; the client discards a mismatched snapshot/event. The Redis key layout and hash fields are defined in `packages/contracts-py`, and change additively like the other Python contracts. |

#### 8.1.4 Domain mutation contracts

The tables in §8.2 are the canonical redesigned command inventory. Generic command rules in §8 apply to **every** row; no endpoint under another context duplicates the operation. Onboarding confirmation, Roadmap activation and output result writes are atomic application commands with audit/events in the same transaction. Workflow start/signal is after commit with deterministic IDs and reconciliation.

### 8.2 API and DTO catalogue

#### Authentication (`identity`)

| Endpoint | Input → output; transition, failure and effects |
|---|---|
| `POST /v1/auth/register` | `{email,password}` only → generic 202 `{verificationRequired:true}`; create unverified User/Workspace shell, Argon2id credential and hashed single-use verification token; send link after commit; audit UserRegistered; generic duplicate response avoids enumeration |
| `POST /v1/auth/verify-email` | `{token}` from email link → verified account and onboarding destination with the normal rotated session cookies/access-token response; atomic consume/expiry; invalid/expired returns safe 400, replay cannot authenticate; EmailVerified |
| `POST /v1/auth/resend-verification` | `{email}` → generic 202; throttled, invalidate superseded token; no account enumeration |
| `POST /v1/auth/login`, `/refresh`, `/logout` | Credentials or existing rotating session → access JWT/refresh+stream cookies, or revoked session; refresh/logout require allowed Origin/custom header and invalidate stream access on logout; invalid credentials 401 |
| `POST /v1/auth/password-reset/request`, `/confirm` | `{email}` → generic 202; `{token,newPassword}` → 200 after single-use expiry check; rotate/revoke sessions; never silently adds a local credential to a Google-only account |
| `POST /v1/auth/google/start` | `{intent: signup\|login, returnPath?}` → `{authorizationUrl,flowId}`; one-time server flow stores state, PKCE verifier, nonce and allowlisted return path; short-lived browser binding cookie |
| `GET /v1/auth/google/callback` | provider code/state/error → validate flow and OIDC then establish normal session cookies; redirect to `/auth/google/return` without credentials in URL; cancelled/invalid/expired flow gives safe failure; no replay |
| `POST /v1/auth/google/link` | authenticated recent-reauth owner, `{version}` → separate link-intent OAuth flow; callback verifies same owner/flow before unique provider binding; matching email alone never links; 409 identity already linked, audit AuthIdentityLinked |
| `GET /v1/auth/me`, `PATCH /v1/auth/account` | current provider/verification metadata; versioned supported profile/password change with recent reauthentication → updated account; Google-only password unavailable; audit account/session effects |

Google uses authorization-code OIDC with PKCE S256 and state/nonce. Validate discovery-configured issuer, audience/authorized party, signature/JWKS, expiry/issued-at, nonce, subject and verified-email claim. Identity key is `(issuer,subject)`. A new provider identity whose email collides with an existing User requires existing-account sign-in and explicit link, with no detailed account disclosure. Use a narrow, short-lived **SameSite=Lax OAuth flow cookie** for the top-level provider callback; existing refresh/stream cookies remain Strict and are not relied on for callback correlation. Linking remembers the recently authenticated owner in the protected flow, not a callback query parameter. Consume state once, clear correlation data and never put code/provider/access tokens into logs or frontend storage. Provider tokens are used transiently for identity validation, not persisted as publishing credentials. Library/JWKS cache, consent configuration, exact token lifetimes and browser compatibility must be verified before implementation release.

#### Onboarding and settings (`identity`)

| Endpoint | Input DTO → output DTO; transitions / effects |
|---|---|
| `POST /v1/onboarding/drafts` | `{source,websiteUrl?}` → 201 `OnboardingDraft`; validate URL; website schedules bounded OnboardingWorkflow after commit, manual starts editing |
| `GET /v1/onboarding/drafts/current`, `GET /v1/onboarding/drafts/{id}` | → draft fields, provenance, actual crawl outcome/progress and version; never active Brain projection |
| `PATCH /v1/onboarding/drafts/{id}` | `{version,fields: BusinessInformation patch}` → updated draft/version; editing/extracting/ready_for_confirmation allowed, confirmed rejected; OnboardingDraftUpdated; merge detects owner-field/extraction conflict |
| `POST /v1/onboarding/drafts/{id}/confirm` | `{version}` → `{draft,confirmationId,workspace,onboardingComplete:true}`; validate five essential groups, atomic confirm + Brain activation; 422 incomplete; stale 409; duplicate key returns original; OnboardingConfirmed/OnboardingKnowledgeActivated; indexing after commit |
| `GET/PATCH /v1/workspace` | read or `{version,country?,currency?,businessTimeZone?,uiLanguage?,conversationLanguage?,contentRegister?,reduceMotion?}` → settings; explicit IANA validation, no moving approved Posts; WorkspaceSettingsChanged/BusinessTimeZoneChanged |

#### Conversations, Tasks and Roadmap (`conversation` / `tasks`)

| Endpoint / owner | Input → output; transitions / effects |
|---|---|
| `GET/POST /v1/conversations` / conversation | list or `{title?}` → direct Conversation; user cannot forge privileged origin |
| `GET /v1/conversations/{id}`, `GET/POST /v1/conversations/{id}/messages` / conversation | cursor history or `{message,attachmentRefs?,contentRegister?,revisionTarget?}` → persisted message + quick reply or authorized Task reference; input/output message DTO discriminator synchronized; TaskRequested only for substantive work |
| `GET /v1/conversations/{id}/tasks` / conversation adapter + tasks contract | conversation validates its parent and calls `TasksApi.listForConversation`; validated workspace/conversation relationship → cursor TaskSummary list with origin and status; no unrelated Tasks |
| `GET /v1/tasks/{id}`, `/canvas` / tasks | status/authorization/usage/result refs and safe snapshot; completed summary survives Redis expiry |
| `POST /v1/tasks/{id}/clarifications/{clarificationId}/answer` / tasks | `{version,formVersion,answers}` → Task; only open unexpired form, resume returns to requested with capacity waiting reason until a slot is reacquired; ClarificationAnswered; signal after commit |
| `POST /v1/tasks/{id}/budget-extension/{decision}` / tasks | decision approve/reject; `{version,requestId,requestVersion}` → Task/allowance; only pending request; exact increment authorization or partial stop; TaskBudgetExtensionApproved/Rejected |
| `POST /v1/tasks/{id}/cancel` / tasks | `{version}` → truthful Task/partial refs; requested/running/paused allowed; terminal safe no-op or conflict; TaskCancelled; no reverse external effect |
| `POST /v1/roadmap/drafts` / tasks | `{goal?}` → 201 RoadmapDraft with reusable context; no active-plan replacement |
| `GET /v1/roadmap/drafts`, `GET /v1/roadmap/drafts/{id}` / tasks | saved drafts + adaptive questions/answers/currentQuestionId/version |
| `PATCH /v1/roadmap/drafts/{id}` / tasks | `{version,goal?,constraints?,currentQuestionId?}` → updated draft + questionSequenceVersion; re-evaluate affected questions without losing unrelated answers; RoadmapDraftUpdated |
| `PUT /v1/roadmap/drafts/{id}/questions/{questionId}/answer` / tasks | `{version,questionVersion,answerVersion,status: answered\|skipped\|unknown\|not_applicable,value?}` → answer/draft versions; domain validates required reason/options; DiscoveryAnswerSaved |
| `POST /v1/roadmap/drafts/{id}/questions/{questionId}/confirm` / tasks | `{version,questionVersion,answerVersion}` → confirmed answer/context refs; only selected valid answer; owner facts/decisions persisted through Brain contract, interpretations separately pending; DiscoveryAnswerConfirmed |
| `POST /v1/roadmap/drafts/{id}/questions/{questionId}/research` / tasks | `{version,questionVersion,answerVersion,scope,resourceAuthorization}` → 202 `{researchLinkId,taskId,conversationId,returnPath}`; save first, queue capacity honestly; QuestionResearchStarted + TaskRequested |
| `POST /v1/roadmap/research-links/{id}/use-result` / tasks | `{version,questionVersion,answerVersion,resultRef,selectedValue,ownerEdit?}` → answered question + canonical returnPath; validate exact link/result/draft state; ResearchResultSelected; no Brain activation until confirm |
| `POST /v1/roadmap/drafts/{id}/generate` / tasks | `{version,resourceAuthorization}` → 202 generation Task/conversation; draft→generating; completed result→proposed, failure→draft with result/error preserved; RoadmapProposed |
| `GET /v1/roadmap`, `GET /v1/roadmaps`, `GET /v1/roadmaps/{id}/versions/{version}` / tasks | active or empty, saved plan/history list, immutable version respectively; no fixed column properties |
| `POST /v1/roadmaps/{id}/activate` / tasks | `{version,proposalVersion,sourceDraftVersion,expectedActiveRoadmapId?,expectedActiveVersion?}` → active Roadmap; workspace lock/unique index, stale base 409; atomic archive/cutover; RoadmapActivated |
| `POST /v1/roadmaps/{id}/reject` / tasks | `{version,proposalVersion}` → rejected initial proposal and editable source draft; proposed only, active plan untouched; RoadmapRejected |
| `GET /v1/roadmaps/{id}/steps/{stepId}` / tasks | plan version query → step/execution/output refs, blockers and allowedActions |
| `POST /v1/roadmaps/{id}/steps/{stepId}/run` / tasks | `{version,stepVersion,resourceAuthorization}` → 202 Task/conversation/execution refs; validate active plan/eligible step; duplicate intent returns same Task, existing run returns reference; queued Task leaves step ready with Waiting for capacity and recorded run ref until admission; StepLaunched |
| `POST /v1/roadmaps/{id}/steps/{stepId}/{decision}` / tasks | decision skip/revisit/not-applicable; `{version,stepVersion,reason?}` → step + dependencyImpact; owner intent, current applicability and status checks; StepSkipped/StepRevisited/StepNotApplicable; no Task cancellation |
| `POST /v1/roadmaps/{id}/steps/{stepId}/attestations` / tasks | `{version,stepVersion,checklist,evidenceRefs?}` → OwnerAttestation; validate owner-executable condition/evidence scope; OwnerImplementationAttested, no platform verification |
| `POST /v1/roadmaps/{id}/revision-requests` / tasks | `{version,request,resourceAuthorization}` → 202 revision Task/conversation; owner-directed work, eventual proposed diff; StrategicRoadmapRevisionProposed |
| `GET /v1/roadmap/revisions/{id}`, `POST /v1/roadmap/revisions/{id}/{decision}` / tasks | read diff; approve/reject `{version,revisionVersion,baseRoadmapVersion}` → revision and current Roadmap; proposal/base locks, stale 409; approved atomic version cutover or rejection unchanged; StrategicRoadmapRevisionApproved/Rejected |

`revisionTarget` is a discriminated reference: `{kind: document,id,version}` or `{kind: post,id,version,postVersionId}`. Conversation validates access through the owning module contract before requesting a revision Task. Document revision preserves its identity and creates a new immutable version; Post regeneration uses the Publishing version contract, clears approval and cannot mutate an unresolved dispatch. No LLM runs in the deterministic publish path. A version conflict returns 409 for owner review, never silently overwrites a newer artifact.

`version` for draft commands is draft concurrency; for step commands it is active Roadmap version and `stepVersion` is StepExecution projection version. Returned DTOs carry new versions. Read-only historical plans cannot execute. Approved steps execute only with supported authority; later_phase/blocked modes offer instructions or real blockers.

#### Documents and requested analysis (`deliverables`)

| Endpoint | Input → output; state / effects |
|---|---|
| `GET /v1/deliverables`, `GET /v1/deliverables/{id}` | search/type/date/cursor → library; detail with immutable version/source/context/origin refs; no Post types |
| `PATCH /v1/deliverables/{id}` | `{version,content,title?}` → new immutable version, same identity; stale 409; DeliverableVersionCreated. Chat revision uses same contract, not copy creation |
| `POST /v1/analysis-uploads` | `{conversationId,analysisTaskId}` + multipart CSV/.xlsx → 202 upload/mapping status; task authorization, bounded parsing/size/sheet/row/cell validation; AnalysisUploadCreated/Rejected; no recurring workflow |
| `GET /v1/analysis-uploads/{id}`, `POST /v1/analysis-uploads/{id}/confirm-mapping` | preview or `{version,mapping,period,units,currency?,identifiers}` → validated mapping/status; deterministic math via existing Task activities; duplicate/incompatible data 422; AnalysisMappingConfirmed |
| `POST /v1/media` | multipart owner image → validation record `{id,version,hash,status,limits}`; type/size/dimensions/malware checks; immutable approved media reference, 422 invalid |
| `POST /v1/data-exports`, `GET /v1/data-exports/{id}` | `{selection}` → 202 job/status/expiring URL; module-owned export projections and current deletion checks; requested/processing/completed/retry_needed; DataExportRequested/Completed |
| `DELETE /v1/deliverables/{id}` | `{version}` → 202 deletion request; immediate deny marker, asynchronous purge/redact under §9.9; DeliverableDeletionRequested |

#### Posts and calendar (`publishing`)

| Endpoint | Input → output; state / effects |
|---|---|
| `POST /v1/posts` | `{platform,postFormat,text,link?,mediaRef?,destinationId?,scheduledTime?,campaignId?}` → 201 Post/currentVersion, manual origin; nullable date/destination valid draft; PostDrafted |
| `GET /v1/posts`, `GET /v1/posts/{id}` | unscheduled=true or from/to/displayZone/platform/campaignId/status/cursor → CalendarPost list; detail includes history/current version/attempt evidence and allowedActions |
| `POST /v1/posts/{id}/versions` | `{version,postVersionId,content,destinationId?,scheduledTime?,mediaRef?}` → Post + immutable version, approval cleared; editable draft/pending/rejected/approved/scheduled/eligible held or failed only after no unresolved dispatch; PostVersionCreated |
| `POST /v1/posts/{id}/reschedule` | `{version,postVersionId,scheduledTime}` → new version/pending approval; nullable schedule only through draft edit; uncertainty blocks change; PostVersionCreated/PostRescheduled |
| `POST /v1/posts/{id}/submit-for-approval` | `{version,postVersionId}` → pending_approval; complete format/media/destination/time required; PostSubmittedForApproval; deterministic release timer may start, never dispatch unapproved |
| `POST /v1/posts/{id}/approve`, `/reject` | `{version,postVersionId}` → Post; pending and displayed exact version only; current authorization/preconditions checked; PostApproved/Rejected; approval snapshots actor/time and authorization generation |
| `POST /v1/posts/bulk-approve`, `/bulk-reject` | `{items:[{postId,version,postVersionId}]}` → per-item outcomes (§8.1.1); no server expansion, partial success, per-item audit |
| `POST /v1/posts/{id}/cancel` | `{version,postVersionId}` → actual state; atomic cancellation-versus-claim race; accepted effect cannot be reversed; PostCancelled or actual evidence |
| `POST /v1/posts/{id}/retry`, `/reconcile` | `{version,postVersionId}` → 202 attempt/status; retry only established safe failure + fresh preflight/window; reconcile uncertain/submitted without resend; PublishAttemptRecorded/PostOutcomeUncertain/PostPublished/PostPublishFailed |
| `POST /v1/posts/{id}/resolve-hold` | `{version,postVersionId,holdId,holdVersion,resolution}` → Post; record owner context resolution; unchanged approved details resume only with current authority/window; changes/new time require version/approval; unresolved uncertainty cannot be cleared into resend; PostHoldResolved |
| `POST /v1/posts/{id}/duplicate` | `{version,postVersionId}` → new Draft identity, no schedule/approval/attempts inherited; PostDrafted |
| `DELETE /v1/posts/{id}` | `{version,postVersionId}` → 202 governed local deletion; freeze undispatched work, preserve minimal attempt evidence and truthful accepted-effect limitations; never calls platform removal implicitly |
| `GET/POST /v1/campaigns` | list or `{title,description?}` → grouping with version; no paid-ad semantics; CampaignCreated |
| `POST /v1/batch-review/sessions`, `/v1/batch-review/sessions/{id}/end` | start `{campaignId?,postIds?}` or end `{reason}` → ReviewSession timings/counts; server clock, one open per owner, idempotent end and abandon sweep (§8.1.2) |

#### Brain, connections and notifications

| Endpoint / owner | Input → output; state / effects |
|---|---|
| `GET/POST /v1/brain/items`, `GET/PATCH /v1/brain/items/{id}` / brain | list/search/detail or `{category,properties,provenance}` / `{version,changes}` → current item/version; owner edit supersedes prior value, targeted impact; KnowledgeOwnerProvided/ContextVersionChanged |
| `GET/POST /v1/brain/relationships`, `GET /v1/brain/search`, `GET /v1/brain/conflicts` / brain | typed workspace-local endpoints/version refs or search → eligible graph/list/results; no draft/deleted leakage; RelationshipAdded |
| `GET /v1/brain/candidates`, `POST /v1/brain/candidates/{id}/approve`, `/reject` / brain | `{version,editedValue?}` → active approved or rejected; individual decisions, conflict checks; KnowledgeItemApproved/KnowledgeCandidateRejected |
| `POST /v1/brain/conflicts/{id}/resolve` / brain | `{version,selectedVersionId?,editedValue?,reason}` → resolved conflict/current value; stale 409, material changes emit ContextVersionChanged |
| `POST /v1/brain/items/{id}/retire`, `DELETE /v1/brain/items/{id}`, `DELETE /v1/brain/relationships/{id}` / brain | `{version}` → retired history or 202 deletion request with immediate suppression; retirement is not deletion |
| `GET /v1/connections`, `POST /v1/connections/{platform}/connect`, `/reconnect`, `GET /v1/connections/{platform}/callback`, `POST /v1/connections/{id}/disconnect` / connections | OAuth start `{returnPath?}`, callback validated state/code, disconnect `{version}` → connection/status/consequences; only facebook_page active; connected/reconnect_needed/disconnected; generation increments and dependent sends held; AccountConnected/Disconnected/ReconnectNeeded; no auto overdue release |
| `GET /v1/notifications`, `GET /v1/notifications/unread-count`, `POST /v1/notifications/{id}/read`, `/unread`, `POST /v1/notifications/read-all` / notifications | cursor list/count; `{version}` or read-all `{throughOccurredAt}` → read state/count; only notification transition, audit metadata, no subject approval; validated typed subject/deepLink or unavailable |
| `DELETE /v1/workspace`, `GET /v1/data-deletions/{id}` / identity | `{version}` → 202 deletion request/status requested/processing/completed/retry_needed; immediate workspace deny, durable cleanup and restore suppression; WorkspaceDeletionRequested/Deleted |

Public events use `.v1` envelopes `{eventId,workspaceId,actor,subjectId,subjectVersion,occurredAt,correlationId,payload}` with minimal safe IDs/changes. Audit, usage, notifications and conversation result records are same-transaction consumers. Notification subscriptions include terminal Task/completion/clarification, document ready, Post approval/published/held/uncertain, connection reconnect, Brain proposal and proposed strategic revision. Internal retry diagnostics produce no notification. No notification is a command authorization.

## 9. Cross-cutting concerns (only what the docs require)

### 9.1 Authentication and authorization
- **Users (api-http):**
  - The Passport JWT strategy verifies the 15-minute access token. A guard builds the `TenantContext` (workspace, user, trace id). Every controller handler receives it.
  - Refresh rotation, reuse detection, the grace window and the session limits live in `platform/auth` + `identity` (S-9).
  - The refresh endpoint also checks the `Origin` header and requires the custom header.
- **Authorization model:** one role (the User owns their workspace). Authorization means **every resource belongs to the caller's workspace**. Repositories enforce it by requiring `workspaceId`; a cross-workspace id returns 404.
- **Streams:** a separate guard reads the path-scoped stream cookie.
- **Operators:** only through `main.cli.ts` commands run with infrastructure access (workspace deletion, evaluation). There are no operator HTTP endpoints; how operators themselves are authenticated is part of OD-16.
- **api-activities:** the workspace comes from the workflow input and is checked against the target aggregate's workspace.
- **ai-http:** verifies the service JWT (`aud=busmora-ai`, 60 s) with the public key, and takes the workspace only from the claim.
- **workers-publish:** has no inbound surface. It decrypts credentials in memory and never logs them.

- **Local/Google identity:** §8.2 defines verification links, OIDC state/PKCE/nonce, provider-subject uniqueness, collision-safe explicit linking and the narrow callback correlation cookie. Existing Strict session cookies, in-memory access tokens, Origin/custom-header protection, tenant checks and revocation stay intact. Verification/reset tokens are high-entropy, hashed at rest, expiring and single-use; neither callback URLs nor telemetry retain them.

### 9.2 Configuration
- Each app has one typed configuration object, validated at startup: class-validator in api, Pydantic models in Python. Invalid configuration stops the process.
- Secrets (Temporal API key per role, service-JWT private key in api / public key in ai, database credentials, provider keys) arrive from the secrets store, which is still open (OD-5). They are never committed.
- Per-role settings select the entrypoint's behaviour: task queue names, concurrency limits, the browser-pool size.

### 9.3 Logging and tracing
- **Structured logs** with `trace_id`, `workspace_id`, `task_id`/`post_id` where relevant, plus event name and outcome. The sink is open (OD-13).
- **Redaction** of tokens, credential envelopes, passwords and cookie values happens in the logger, not at call sites.
- The **trace id** starts at api-http, travels in the service JWT (HTTP path) and in workflow inputs (Temporal path; OD-9 covers full propagation), and is tagged on Langfuse traces.
- Every agent action, tool call and error is logged in `busmora-ai` and traced in Langfuse with workspace, Task, model, prompt version and cost (NFR-08).

### 9.4 Error handling

| Layer | Behaviour |
|---|---|
| Domain | Throws typed domain errors (`InvalidTransition`, `StaleVersion`, `CapacityExceeded`, `PolicyViolation`). Never HTTP or Temporal types. |
| Application | Lets domain errors pass; wraps adapter failures as `DependencyUnavailable` with context |
| HTTP interface | The exception filter maps errors to the envelope and status (§8). Unknown errors become 500 with a correlation id and no internals. |
| Activities (TS and Python) | Domain errors → **non-retryable** Temporal failures carrying the error code. Infrastructure failures → **retryable**, with the documented retry policies (agent tool/sub-agent retries up to 3 then authorized alternatives; site unreachable up to 2 retries; publishing only for reconciled retry-safe failures within authorization/window; uncertain acceptance is never automatically replayed). |
| Workflows | Translate final failures into domain outcomes through platform activities: `partially_completed`, `failed` with a user-facing explanation, Post `failed` with reason. Never silent. |
| LLM output | Parsed against Pydantic schemas; bounded self-correction within allowance; malformed commands/proposals fail closed. A clearly labelled narrative partial result may be saved but never converted to executable authority |

### 9.5 Caching (only where the docs require it)

| Cache | Where | Rule |
|---|---|---|
| Crawl cache | `ai-crawl` + Redis (keys with 7-day TTL); bookkeeping in `crawls` | Seven-day provisional TTL; reuse retains source/access dates and freshness/limitations. A justified authorized refresh is possible; deletion purges cached content and copied summaries |
| Canvas replay | Redis Streams per Task | Replay during the run plus a short grace window (length open) |
| Sessions | Redis refresh-token families | Derived; loss means re-login |

No HTTP response caching or query caching in api: not required, and it would complicate the single-writer consistency.

### 9.6 Background jobs
**Temporal is the only background-job mechanism.** There are no in-process cron jobs or queues.

| Job | Mechanism |
|---|---|
| Tasks, onboarding, publishing, indexing, data export/deletion and workspace deletion | Workflows (S-2) |
| Retention sweep (unreferenced raw research data after 7 days; expired idempotency keys) | Temporal Schedule → maintenance workflow → platform activities |
| Closing abandoned Batch Review Sessions (§8.1.2) | Same maintenance schedule → `publishing` activity |
| Reconciliation (workflows that should exist after a failed after-commit hook; lagging index versions) | Temporal Schedule → maintenance workflow |
| Capacity queue | When a running Task ends, `tasks` signals the oldest queued Task's workflow. Each waiting workflow also re-checks on a durable timer as a fallback. |

### 9.7 Real-time
- `busmora-ai` writes Canvas signals (`agent_started`, `agent_tool_used`, `agent_completed`, `agent_error`, `flow_connection`, plus `execution_event` for actual tool/service/workflow/input/hold records) to a **per-Task Redis Stream**, using the envelope from `contracts-py`.
- Platform activities persist safe action summaries and publish `execution_event` after commit for actual workflow/service/input/hold transitions, using the same stream/state-hash transaction contract; workflow code itself performs no I/O. Missing live emissions recover from durable status, not invented animation.
- `platform/streams` in api-http reads them, replays from `Last-Event-ID` (or `after`), sends heartbeats, and sends **reset** when replay is impossible.
- After a reset, the client redraws from `GET /v1/tasks/{id}/canvas`, which reads the per-Task Canvas state hash written atomically with the stream, then resumes the stream from the snapshot's position (§8.1.3).
- Task-status chat messages are persisted by the `conversation` module (event handlers) and fetched by the frontend through queries.
- **Ready for the Workspace Event Shell later:** UI events would be emitted by api event handlers into a per-workspace stream. The handler slot already exists (same-transaction handlers + after-commit hook).

### 9.8 Tenancy and security rules repeated in code
- `workspaceId` is mandatory in every repository method, every activity input, every Qdrant filter, every object key prefix and every Redis key.
- Crawled text is sanitized and quarantined in `ai/domain/intelligence` **before** it reaches any prompt.
- The SSRF guard lives in the crawler adapter.
- Sub-agent tool scoping lives in the tool registry.
- KMS decryption code exists only in `workers/publish/secrets`.

---

### 9.9 Data lifecycle, deletion and recovery

Follow SYS §4.5 for active deletion, minimal audit, backup limitations and restore suppression (FR-13, FR-42; NFR-09). `DataDeletionWorkflow` states are requested → processing → completed or retry_needed, with per-store evidence. In the initiating transaction increment deletion generation, deny reads/retrieval and record affected dependency IDs; disable relevant pending sends. Module activities purge/redact primary content, derived summaries/versions/citations, Qdrant/control chunks, Redis caches, object versions/exports/media, checkpoints and provider traces as applicable. Audit keeps only justified subject/action/actor/time/outcome metadata, never a deleted payload copy.

Late jobs/index writes and export downloads revalidate deletion generation and current eligibility. Retention sweeps cannot resurrect deleted content because a reference remains. Backups are subject to documented expiry, not instant-erasure claims. A restore first applies protected deletion/disconnection suppressions, revokes stale jobs/tokens, reconciles external attempts and rebuilds eligible projections in isolation. Numeric retention, processor handling and backup coverage remain blockers G-18/SYS OD-21 until documented and tested.

### 9.10 Research, content and input boundaries

Research planning, discovery/retrieval, extraction, claim-source association, synthesis and budget accounting are separate use-case interfaces with BUSMORA interpretation layered on top (FR-24–28). Scrapling remains a retrieval adapter; the search/discovery baseline and regional source access must be evaluated (SYS OD-22). Vary steps/source types with the question, retain original customer wording and separately labelled translations, distinguish sampled comments from representative evidence and duplicated material from corroboration. Preserve publication/access dates, support excerpts and material contradictions; stop at sufficient evidence, repetition, exhausted access or allowance and save honest partial work.

The Router answers simple conversation and delegates substantive tasks; deterministic navigation/approvals bypass AI. The run context stores objective, authorized tools, task/workspace, context versions, inputs, evidence/output/review contracts and bounded iteration/time/usage/concurrency. Specialists may be selected/skipped/repeated within the evaluated registry; no recursive spawning or arbitrary executable workflows. Canvas nodes/events reflect actual agent/tool/service execution, terminal failures and owner-input points, without secrets or private reasoning.

Source text, uploads and tool output cannot grant authority. Validate structured outputs and URL schemes/DNS/redirects, block internal-network fetches and unsolicited private-data search queries, and treat sanitization as one layer rather than an injection guarantee. Spreadsheet parsing never executes macros/formulas; CSV exports escape formula-leading cells. Content preserves observed voice versus inferred traits, and task-level country/audience/register overrides stored defaults. Unsupported quantified claims, testimonials, discounts/deadlines and superiority claims require evidence, omission, qualification or a targeted question; accepting copy is not evidence or identity-use consent.

## 10. Testing strategy

Test frameworks are still open (OD-12). This strategy is framework-neutral. Following `codebase-design`: **the interface is the test surface**, and tests replace (not layer on) lower-level tests once a deeper interface covers them.

| Layer / target | What is tested | How | Dependencies |
|---|---|---|---|
| **api · domain** | Aggregates, value objects, policies: Post state machine, claim rules, Knowledge lifecycle, capacity, expiry, rotation policy | Pure in-process unit tests, table-driven over every allowed and forbidden transition | none (in-process) |
| **api · application** | Use cases and contract implementations through their public interfaces: transactions, cross-module writes, same-transaction handlers, after-commit hooks, idempotency, workspace scoping | Integration tests against **real PostgreSQL** (Docker Compose), with **fake adapters** for ports: Temporal recorder, fake AI gateway, fake KMS, in-memory blob, fixed clock | local-substitutable (PostgreSQL); fakes for remote ones |
| **api · HTTP interface** | Routing, validation, error envelope, status codes, pagination, idempotency header, version conflicts, auth guards, cookie rules, SSE replay and reset (including `after`). Bulk approve/reject with mixed outcomes (one stale, one invalid, one already decided) and a retry under the same key. Review sessions: start closes an open one, end is idempotent, the sweep closes idle ones at last activity. Canvas snapshot from the live hash and from the durable fallback. | End-to-end tests booting the api-http app on a test database, with fake ports | as above + Redis |
| **api · activities** | Each platform activity's contract (input validated by Ajv), idempotent re-execution, mapping of domain errors to non-retryable failures | Temporal TS SDK test environment invoking activities | as above |
| **api · architecture** | Dependency rule (§2.3), module import boundaries (only `contract/` is importable across modules), no `domain` → framework imports | Static import-graph check in CI (tooling open; Appendix A) | none |
| **api · module contracts** | Each module's `contract/` interface and public events behave as documented: inputs, outputs, error codes, event payloads per version. Changes under `contract/` are additive, or add a new version. | A contract suite per module running against the real implementation and PostgreSQL, triggered on any change in that module; a CI diff check on `contract/` folders | PostgreSQL |
| **Contracts** | Pydantic → JSON Schema → TS types up to date; schema evolution additive; `ai-http` client regenerated; `openapi.json` unchanged or additive (`oasdiff`) | CI checks on `contracts/` | none |
| **Migrations** | Every migration applies on a copy of the previous schema; views and row-level security behave (a read-only role can't see another workspace, pending AI proposals or ineligible/deleted versions, while confirmed-intake and owner context remains usable) | Migration test job against PostgreSQL | PostgreSQL |
| **workers · workflows** | Sequencing, Clarification wait and 7-day expiry, capacity wait, Post release at the Scheduled Time, expiry, cancellation, retries, reconciliation | Temporal Python **time-skipping** test environment with stubbed activities | none (in-process) |
| **workers · workflow versioning** | Replaying recorded histories of in-flight workflows against new code | Replay tests on stored histories | none |
| **workers · publish** | MCP tools and platform adapters: idempotency key use, attempt-before-call, read-back before retry, rate-limit cooldown, refresh → reconnect needed, Facebook text/link and single owner-image validation, uncertain-result hold; IG/X tests are Phase 3 | Fake platform adapters (the second adapter of `PlatformPublisher`) | true external → fakes |
| **ai · domain** | Retrieval assembly (seed + hops, token ceiling, control route), intent qualification, sanitization/quarantine, formatting limits, KPI math | Pure tests | none |
| **ai · application** | Agent step (graph runs to interrupt/finish with fake LLM responses), tool scoping per Sub-agent, extraction, indexing filters, crawl bounds | Fake LLM, fake vector index, fake crawler, fake views | fakes |
| **ai · entrypoints** | `ai-http` routes: service-JWT verification (wrong audience, expired, bad signature), workspace taken only from claims, timeout budget; activity payload validation | FastAPI's test client + fakes; activity invocation | fakes |
| **Security** | Tenant isolation across api, views, Qdrant and object keys; prompt-injection quarantine; SSRF guard; no credential in logs | Dedicated suites using two seeded workspaces | mixed |
| **System** | Walking-skeleton spikes (SYS §9.4); kill tests (stop a worker or api mid-task and mid-publish → resume, no duplicate); 100-workspace load test with stubbed LLM and crawler | Docker Compose environment (load tool open) | all |
| **Evaluation** | Scenario set run in Treatment and Control with frozen configuration; outputs exported for blind human scoring | `evaluation/` + operator CLI | real models |

---

### 10.1 PRD release evidence

All active FR-01–46, FR-48–58, NFR-01–09 and AC-01–25, AC-27–58 require evidence; FR-47/AC-26 are retired. System Appendix A maps every requirement and the 40 approved redesign checks. Test secure email links and Google linking/replay/cancellation, intake confirmation races and late extraction, draft/question persistence and exact research return, dependency DAG/Skip/Revisit, one-active-plan concurrency, two-Task capacity across conversations, public retry projection versus terminal failure, first-class Post/document result transactions, nullable dates, actual historical calendar dates, versioned bulk/reschedule/context holds, disconnect and uncertain-result reconciliation.

Preserve all unrelated gates: retrieval eligibility across current/history/control routes and deletion races; source injection/SSRF; budget reservations/children/retries/refusal; CSV **and** .xlsx mapping/unknown/zero/duplicates/period/currency/formula safety; real Facebook text/link and single-image evidence with ordinary-owner production access (AC-30); backup restore/rollback, expiry/cancellation and tenant isolation. Stubbed fault injection does not prove native capability. Negative inventories assert absence of retired routes, jobs, columns and triggers, while Post ReviewSession tests remain.

Evaluation follows PRD §10: held-out contrast cases and four registers, ≥80% without major correction overall and per register, zero critical truth/control violations in the suite, no unresolved critical/high security/recovery defect, grounded consequential claims and measured runtime/resource limits at declared load. First useful artifact active interaction time is separate from background latency. No documentation test is implementation evidence.

## 11. Naming conventions and coding rules

### 11.1 Naming

| Item | Convention | Example |
|---|---|---|
| Domain terms in code | **Glossary terms only** (domain/GLOSSARY.md). Never the "Avoid" synonyms. | `KnowledgeItem` (not Node), `Post` (not Publication), `Review` vs `Approval` |
| Storage names | snake_case, plural tables, schema per module; product-doc names kept where they exist | `brain.brain_nodes`, `publishing.post_versions` |
| TS files | kebab-case, suffixed by role | `approve-post.command.ts`, `post.aggregate.ts`, `scheduled-time.value.ts`, `post.repository.ts`, `posts.controller.ts`, `approve-post.request.dto.ts`, `claim-post.activity.ts`, `post-published.event.ts` |
| TS types | PascalCase; use cases named `<Verb><Noun>` | `ApprovePost`, `ProposeCandidates`, `CompleteTask` |
| Domain events | Past tense, PascalCase, from domain/events.md | `PostApproved`, `KnowledgeItemRetired` |
| Error codes | UPPER_SNAKE, stable | `STALE_VERSION` |
| Python modules | snake_case; packages by layer then area | `busmora_ai/domain/retrieval/assembly.py` |
| Temporal workflows | PascalCase + `Workflow` | `PostReleaseWorkflow` |
| Temporal activities | `<owner>.<verb_noun>` snake_case, owner = task queue family | `platform.claim_post`, `ai.run_agent_step`, `crawl.crawl_site`, `publish.publish_post_version` |
| Task queues | role-aligned | `platform`, `ai-agent`, `ai-crawl`, `publish`, `workflows` |
| Workflow ids | deterministic, prefixed by kind | `task-{taskId}`, `post-{postId}-{scheduleRevision}`, `index-{itemId}-{version}`, `deletion-{workspaceId}` |
| HTTP | plural nouns, kebab-case paths, verbs only for action sub-resources | `/v1/analysis-uploads`, `/v1/posts/{id}/approve` |
| Redis keys | `{workspaceId}:{area}:{id}` | `ws-…:canvas:task-…` |
| Object keys | `{workspace_id}/{source_type}/{timestamp}_{filename}` (SYS §4.5) | — |

### 11.2 Coding rules
1. **The dependency rule (§2.3) and module boundaries (§3.3) are enforced in CI.** A violation fails the build.
2. **Domain code is pure.** No framework imports, no I/O, no current-time calls (use `Clock`), no randomness except via injected id generation.
3. **Every repository method, activity input and external query is workspace-scoped.**
4. **One command = one transaction = one use case file.** No business logic in controllers, activities, handlers of generic modules, or Python workflows.
5. **External side effects run after commit** with deduplication, fresh authority checks and provider reconciliation; local ids/keys cannot promise exactly-once remote execution.
6. **No new port without two real adapters** (production + test).
7. **Generated code is never edited.** Change the source (Pydantic model, DTO, FastAPI route) and regenerate.
8. **Contracts evolve additively:** API, Temporal payloads and `brain_read` views.
9. **Workflow code changes keep in-flight runs replayable** (Temporal versioning/patching); a replay test is required for changed workflows.
10. **Prompts change by new version files**, never by editing a released version. The version used is recorded per run.
11. **No plaintext secrets/tokens, unnecessary personal data or private reasoning in logs, traces, exceptions or workflow histories.** Credential envelopes travel only through the authorized api → publish boundary as ciphertext; prefer short-lived references. Metadata retention and external payload exposure are governed by §9.9 and SYS OD-1.
12. **No LLM call in any publish path.** No crawler tool in non-research Sub-agents.
13. **Strict typing:** no implicit `any` in TS; fully typed Pydantic models in Python. Value objects for time, money, ids and statuses; no raw strings for enums.
14. **Errors are typed domain errors** with catalogued codes; no string-matching on messages.
15. **Tests sit at the module interface:** contracts, use cases, routes, activities. Internal helpers are tested through them.
16. **Modules speak only through interfaces.**
    - Inject other modules by contract token; use contract types; subscribe to public `…vN` events.
    - Never export domain classes, internal events, repositories or tables.
    - A breaking contract change is never made in place: add `v2` next to `v1`, migrate consumers, then remove `v1`.

---

## 12. Walkthrough: adding a feature end to end

**Example feature: "Reschedule an expired or overdue-held Post"** (FR-34–36: a new schedule needs fresh approval). The same steps apply to any feature; steps that don't apply are skipped.

| # | Step | Where | What to do |
|---|---|---|---|
| 1 | **Name it in the domain language** | domain/GLOSSARY.md | Confirm the terms (Post, Scheduled Time, Expired Post). If a new term is needed, update the glossary first. |
| 2 | **Locate the owning module** | §3.1 | Publishing owns Posts → `modules/publishing`. If two modules seem to own it, stop and resolve ownership before coding. |
| 3 | **Model the rule in the domain** | `publishing/domain` | Add `Post.reschedule(newTime, now)`: allowed from `expired` or an overdue-held state after any uncertain outcome is reconciled, `newTime` must be in the future. Effect: status → `pending_approval`, emits `PostRescheduled` (add it to domain/events.md). Extend `PostTransitionPolicy`. |
| 4 | **Unit-test the domain** | `test/domain/publishing` | Table-driven: allowed from expired/eligible held states; reject unresolved uncertainty and forbidden states; past time rejected; event emitted. |
| 5 | **Write the use case** | `publishing/application/commands/reschedule-post.command.ts` | Load the Post (workspace-scoped) → call `reschedule` → save with optimistic version → register an after-commit hook to start a new `PostReleaseWorkflow` (deterministic id per Post + schedule revision). |
| 6 | **Expose and consume through the contract** | `publishing/contract/publishing.events.ts`; consuming modules' `application/handlers` | Map the internal `PostRescheduled` event to a new public `PostRescheduled.v1` contract. Adding an event is additive, so no existing consumer changes. `audit` subscribes to `PostRescheduled.v1` and appends an Audit Event; `notifications` needs nothing. No other module imports anything new from `publishing` except this public event. |
| 7 | **Persistence** | `publishing/infrastructure` | If new columns are needed (e.g. `schedule_revision`), add them to the module's Drizzle schema, generate the migration into `platform/db/migrations`, and review the SQL. Expand-only. |
| 8 | **Contract first (API)** | `publishing/interface/http` | Add the `RescheduleRequest` DTO (`{version,postVersionId,scheduledTime:{instant,zone,latenessAllowanceMinutes}}`, validated) and the response DTO. Add `POST /v1/posts/{id}/reschedule` with the `Idempotency-Key` requirement and error codes (`INVALID_TRANSITION`, `STALE_VERSION`). **Merge this signature PR first.** CI regenerates `openapi.json` and `oasdiff` confirms it is additive. |
| 9 | **Temporal contract (if a workflow or activity changes)** | `packages/contracts-py` → `contracts/temporal` → `apps/api/src/generated` | Change the Pydantic model (additive), export JSON Schema, regenerate TS types. CI checks for drift and additivity. |
| 10 | **Workflow change (if any)** | `workers/workflows/post_release.py` | Reuse the existing workflow with the new Scheduled Time. If its logic changes, guard the change with Temporal versioning and add a replay test with a recorded history. |
| 11 | **Activity change (if any)** | `publishing/interface/activities` | `platform.claim_post` already checks state and version, so nothing changes. Any new activity must be idempotent per (workflow id, step). |
| 12 | **AI involvement (if the feature needs it)** | `ai/domain` + `ai/application` + `ai/entrypoints/activities` | Add the use case behind a port, register the activity on the right task queue, emit Canvas signals only through `CanvasSink`, version any prompt. (Not needed for this example.) |
| 13 | **Tests at each interface** | `test/integration`, `test/http`, `workers/tests/workflows` | Use case against PostgreSQL with a fake `WorkflowGateway` (assert the start was requested after commit). HTTP test for 200/409/422 + idempotent replay. Workflow time-skipping test for release at the new time. |
| 14 | **Frontend handoff** | published `openapi.json` | Tag the contract version. The frontend regenerates types and adds the Batch Review action. |
| 15 | **Docs** | `docs/adr` only if an internal decision changed | Most features need no ADR. Update domain/events.md and the glossary when the language changes. |

---

## 13. Internal decisions (ADRs)

Format: the `domain-modeling` skill's ADR format (title; context, decision, why; considered options and consequences where useful). All are **Accepted** for build.

### BTD-ADR-01 — Modular monolith by bounded context with pragmatic layers
- **Context:** the api is the single writer of 13 business areas. The work must be divisible among 5 people, and cross-context transactions are required.
- **Decision:** one NestJS module per context with `domain / application / infrastructure / interface` layers. Modules communicate only through their `contract/` (interface + token, contract types, versioned public events), re-exported by `index.ts` (§3.5).
- **Considered:**
  - technical layering (feature scattering);
  - full Clean/Hexagonal everywhere (hypothetical seams);
  - vertical slice per endpoint (duplicated invariants);
  - CQRS / event sourcing (over-engineering; conflicts with no message bus).
- **Consequences:** clear ownership and locality. The rules must be enforced by CI import checks.

### BTD-ADR-02 — Ports only at real seams; concrete repositories
- **Context:** `codebase-design`: one adapter means a hypothetical seam. PostgreSQL is locally substitutable through Docker Compose.
- **Decision:** ports for Temporal, `ai-http`, KMS, object storage, mail, rate limiting, clock and logging, each with a production and a test adapter. Repositories are concrete classes, and tests run against real PostgreSQL.
- **Considered:** a repository interface for every aggregate, with in-memory fakes. Rejected: double maintenance, and the fakes would diverge from real SQL behaviour (locks, constraints, row-level security).
- **Consequences:** application tests need a database; they are faster to trust and slower to run.

### BTD-ADR-03 — Ambient transaction (unit of work) across modules
- **Context:** completing a Task must write document Deliverables, Posts, Candidates, usage, audit, notifications and messages atomically.
- **Decision:** `TransactionManager.run()` opens one transaction per command and propagates it through Node's `AsyncLocalStorage`. Repositories, contract implementations and same-transaction handlers join it automatically.
- **Considered:**
  - passing a transaction object through every call: noisy and error-prone;
  - per-module transactions with compensation: needless sagas inside one database.
- **Consequences:** contract implementations must never open their own transactions. Long work (LLM, HTTP) must never run inside a transaction.

### BTD-ADR-04 — In-process domain events with same-transaction handlers
- **Context:** there is no message bus (SYS §3.5). Audit, usage and notifications must be written with the change.
- **Decision:** aggregates collect events; the unit of work dispatches them before commit to handlers in consuming modules. External effects use after-commit hooks.
- **Considered:**
  - transactional outbox + relay: excluded at system level;
  - a third-party event-emitter package: not in the stack, and it would hide transaction semantics.
- **Consequences:** handler failures roll back the command, which is the desired atomicity. Handlers must be fast and must not call external systems.

### BTD-ADR-05 — Commit-then-start with deterministic workflow ids + reconciliation
- **Context:** starting a workflow can't join a PostgreSQL transaction.
- **Decision:**
  - Start or signal workflows in after-commit hooks, using deterministic ids (`task-{id}`, `post-{id}-{scheduleRevision}`, `index-{id}-{version}`), so a duplicate start is a no-op.
  - A scheduled reconciliation workflow repairs rows whose workflow is missing.
- **Considered:**
  - start-then-commit: an orphan workflow if the commit fails;
  - an outbox: excluded.
- **Consequences:** a short window where a committed Task has no workflow. Reconciliation closes it, and the user sees "starting".

### BTD-ADR-06 — Workspace scope as an explicit repository parameter (api); row-level security for Python roles
- **Context:** tenant isolation must hold in every store (G2).
- **Decision:**
  - api repositories require `workspaceId` on every method, and cross-workspace ids return 404.
  - Row-level security is applied to the `brain_read` views used by Python's read-only roles, keyed to the verified workspace.
- **Considered:** row-level security on every api table. Rejected for this release: it adds session-variable plumbing on every connection and makes migrations and tests harder. The api is the single, tested writer.
- **Consequences:** api isolation rests on code discipline plus the CI import and security tests. Python isolation also has a database-level guard.

### BTD-ADR-07 — One Drizzle schema per module, one migration history, expand → contract
- **Context:** single migration toolchain (SYS ADR-008). Long-lived workflows and Python views depend on the schema.
- **Decision:** each module defines its tables in `infrastructure/schema`. There is one ordered migration history in `platform/db/migrations`. All changes are expand-then-contract, and views are versioned additively.
- **Considered:** a migration folder per module, rejected because ordering across modules would be ambiguous.
- **Consequences:** schema reviews go through one folder, and removing columns takes two releases.

### BTD-ADR-08 — Read-only views as the Python contract
- **Context:** Python must read eligible imported/owner-provided/approved context without owning tables (SYS ADR-004, ADR-008).
- **Decision:**
  - The `brain_read` eligibility views expose current owner-confirmed intake/owner-provided/approved context, eligible relationships and adopted rules with provenance under tenant-scoped read-only roles; indexing and historical/control routes obey §7.5.
  - Python uses SQLAlchemy Core only against explicit read views, including the module-owned historical projections in §7.5; no reciprocal domain-module dependency is introduced.
- **Considered:**
  - Python reading base tables: couples it to write-model internals and exposes candidates;
  - an api read endpoint: adds a network hop to every retrieval.
- **Consequences:** view changes are coordinated, additive contract changes.

### BTD-ADR-09 — LangGraph checkpoints in a Python-managed runtime schema
- **Context:** LangGraph needs durable checkpoints to pause and resume (SYS §4.6), which is technical state, not domain state.
- **Decision:**
  - A Drizzle migration creates the `ai_runtime` schema and its grants.
  - Tables inside it are managed by the LangGraph checkpointer under the `ai-agent` role.
  - Checkpoints are not authoritative domain records, but may contain copies of private context; minimize payloads and govern them with deletion generations, retention and resume revalidation.
- **Considered:**
  - Drizzle defining the checkpointer's tables: couples us to the library's internal schema;
  - checkpoints in Temporal payloads: size limits, and it bloats history.
- **Consequences:** a documented exception to "Drizzle defines every table", limited to technical state. Deletion removes/redacts affected checkpoints, including item-level dependencies, and restore cannot resume with excluded context (§9.9).

### BTD-ADR-10 — Thin deterministic workflows; rules stay in api
- **Context:** Python workflows sequence long work, but S-1 puts every rule in api.
- **Decision:**
  - Workflows only sequence activities, wait on signals and timers, and translate outcomes.
  - Every decision (capacity, claim, transition validity, expiry) is a platform activity in the owning module.
- **Considered:** rules inside workflows (the rejected B2 style). Rejected because it splits rules across languages.
- **Consequences:** more activity round trips, accepted per SYS ADR-003. Workflows stay small and replay-safe.

### BTD-ADR-11 — Idempotency stored in PostgreSQL
- **Context:** duplicate submissions must not create duplicate Tasks or approvals, even across restarts (FR-34; durable local command records, NFR-05).
- **Decision:**
  - `Idempotency-Key` records (workspace, key, request hash, response snapshot, expiry) live in `platform.idempotency_keys`, written in the command's transaction.
  - Activity idempotency uses processed step keys inside the owning module.
- **Considered:** Redis. Rejected because Redis is derived and may lose data.
- **Consequences:** a retention sweep removes expired keys.

### BTD-ADR-12 — Concurrency control: optimistic versions + advisory locks
- **Context:** stale approvals must fail, "≤ 2 running Tasks" must be exact, and a Post must be claimed once.
- **Decision:**
  - A `version` column, with commands carrying `postVersionId` / `version`.
  - PostgreSQL advisory transaction locks per workspace (capacity) and per Post (claim).
- **Considered:** serializable isolation everywhere. Rejected: retries on every conflict and harder reasoning.
- **Consequences:** locks are short and held only inside one transaction.

### BTD-ADR-13 — Ports and adapters inside `busmora-ai`; tool scoping by registry
- **Context:** agent code depends on LLMs, Qdrant, crawlers, storage and tracing, all of which must be faked in tests. Sub-agents must see only their own tools.
- **Decision:**
  - `entrypoints → application → domain`, with adapters behind ports declared by use cases.
  - The LangGraph graph and the tool registry live in `domain/employee`.
  - The `VectorIndex` adapter always applies workspace and status filters.
- **Considered:** calling SDKs directly from graph nodes. Rejected as untestable, and filters could be forgotten.
- **Consequences:** a port per external dependency (two adapters each), and a fake LLM suite for agent tests.

### BTD-ADR-14 — MCP server embedded in `workers-publish`
- **Context:** MCP is the retained engineering tool boundary (SYS ADR-005), supporting FR-33–36; the PRD does not mandate a protocol. SYS ADR-002 forbids a fifth deployable.
- **Decision:**
  - The MCP server module runs inside the `workers-publish` process.
  - The publish activity invokes its tools; the transport between them is an internal detail.
  - Platform specifics sit behind anti-corruption adapters.
- **Considered:**
  - an MCP server as its own process role: an extra network hop and plaintext crossing a boundary;
  - calling platform APIs directly without MCP: deviates from the documented tool interface.
- **Consequences:** the MCP tool interface stays the seam for adding platforms later (Instagram Business/X in Phase 3; other platforms need a separate product decision).

### BTD-ADR-15 — Canonical Pydantic contracts in a shared Python package
- **Context:** workers, ai and api exchange Temporal payloads. Pydantic is canonical (SYS ADR-014).
- **Decision:**
  - `packages/contracts-py` holds every cross-language payload and the Canvas envelope.
  - Workers and ai import it. JSON Schema is exported into `contracts/temporal`, and TS types are generated into api.
- **Considered:** models duplicated in each Python app. Rejected: drift between workers and ai.
- **Consequences:** a payload change touches one package, regenerates artefacts, and must pass the additivity check.

### BTD-ADR-16 — Operators act through CLI commands, not HTTP
- **Context:** operator recovery and evaluation need no admin UI; owners still initiate data export/deletion through authenticated product APIs (§8.2). There is no admin UI (K-06).
- **Decision:** `main.cli.ts` runs application use cases (and workflow starts) in a standalone NestJS context with infrastructure access.
- **Considered:** operator HTTP endpoints. Rejected: they need a second auth model and enlarge the attack surface.
- **Consequences:** operator authentication is infrastructure access (OD-16). Every operator action is audited.

### BTD-ADR-17 — Modules communicate only through published contracts
- **Context:** with 13 modules in one codebase and 5 people working in parallel, an edit inside one module must not break or force edits in others. Exporting domain classes or internal events, even through `index.ts`, would let internal changes ripple outward.
- **Decision:**
  - Each module publishes a `contract/` folder:
    - an interface + injection token;
    - plain contract types;
    - versioned public event contracts;
    - error codes.
  - `index.ts` exports only that.
  - Consumers inject the interface by token and subscribe only to public events. Providers map domain ↔ contract internally.
  - Contracts change additively; breaking changes add a new version next to the old one.
  - Enforced by the CI import check, per-module contract suites and contract-diff review (§3.5, §10).
- **Considered:**
  - exporting a concrete facade class with domain types: internal type changes would break callers;
  - sharing domain events across modules: any internal event refactor would break subscribers;
  - separate deployable services per module: real isolation, but contradicts the single-writer, four-deployable system design.
- **Consequences:**
  - A small mapping layer per module, between domain and contract.
  - Extra `…v2` contracts during migrations.
  - In return, any change that leaves a module's contract alone needs no edits anywhere else.

### BTD-ADR-18 — Bulk Post decisions: explicit versions, one transaction per Post
- **Context:** FR-34 requires selected/batch approval of the exact displayed set; bulk reject remains a compatible convenience. Approval must bind to the version the User saw, and a review batch can hold many Posts.
- **Decision:**
  - `POST /v1/posts/bulk-approve` and `/bulk-reject` take an explicit list of `{ postId, version, postVersionId }`.
  - Each item reuses the single-Post use case in its own transaction, and the response reports an outcome per item.
  - The whole result is idempotent under one key.
- **Considered:**
  - A server-side "everything still pending": it could approve versions the User never saw.
  - One all-or-nothing transaction: one stale Post would block the whole batch, and the locks would be long.
  - Looping single calls from the client: N requests, N keys, and no single result.
- **Consequences:** partial success is normal, and the client must show per-item outcomes. The Post state machine and the claim are unchanged.

### BTD-ADR-19 — Batch Review Sessions measured on the server, owned by `publishing`
- **Context:** EVAL §6/§8 needs review time per batch (target ≤ 15 minutes) from session timestamps.
- **Decision:**
  - The client only starts and ends a session.
  - Activity and counts come from `publishing`'s own handlers for the User's Post actions.
  - Idle sessions are closed by the maintenance schedule at their last activity.
  - `ReviewSessionEnded.v1` feeds `evaluation`.
- **Considered:**
  - Client-reported timestamps or heartbeats: untrusted, and lost when a tab closes.
  - Deriving sessions afterwards from audit events alone: no reliable start or end.
  - Placing sessions in `evaluation`: `evaluation` would then need Post internals.
- **Consequences:** one new table and one new public event. The idle timeout is a tunable value (G-16).

### BTD-ADR-20 — Canvas snapshot from an atomically written state hash
- **Context:** SSE resets require the client to redraw the live Canvas, and the Canvas is live-only (SYS ADR-020).
- **Decision:**
  - `busmora-ai` updates a per-Task Canvas state hash in the same Redis transaction as each stream write.
  - `GET /v1/tasks/{id}/canvas` returns it together with its stream position, falling back to durable Task records.
  - The stream accepts `?after=` so the client resumes exactly from the snapshot.
- **Considered:**
  - Folding the whole stream on every reset: costly, and incomplete once the stream is trimmed.
  - Persisting Canvas state in PostgreSQL: makes a live-only view durable and adds writes on every signal.
  - Showing an empty Canvas after a reset: loses in-progress state.
- **Consequences:** a small, additive change to the Python Canvas adapter and `contracts-py`. The outer SSE envelope remains stable; node kind/state fields and execution_event are additive payload contracts (§9.7), coordinated with frontend consumers.

---

### BTD-ADR-21 — Draft confirmation and explicit output ownership

The approved redesign makes initial intake non-authoritative until owner confirmation and makes Posts independent of documents. Identity owns the draft/confirmation; Brain activates through a same-transaction confirmation event. Tasks assembles typed document/Post results through their owning public contracts, adding the narrow tasks → publishing edge. This preserves the single writer and module boundaries without a second approval queue, new service or document-to-Post event coupling.

### BTD-ADR-22 — Persisted Discovery and immutable plan authority

Tasks owns saved Discovery questions/answers/research links and one active Roadmap with immutable versions. Explicit owner approval and a workspace lock prevent competing plans; stable execution origins preserve work across revisions. Browser parameters alone were rejected because they cannot provide durable question identity or authorization after reconnect.

## Appendix A — Gaps surfaced by this design

These are not decided here. They are needed to build, and the docs don't name a tool. They add to SYS §9.2 (OD-1 … OD-20).

| # | Gap | Needed for | Related |
|---|---|---|---|
| G-1 | Test frameworks for TS and Python; end-to-end and load tools | §10 | OD-12 |
| G-2 | Import-boundary / static-analysis tooling to enforce §2.3 and §3.3 in CI | §10 architecture tests | new |
| G-3 | TypeScript package/workspace tooling and Python packaging for a polyglot repo with `packages/contracts-py` | §4 | new |
| G-4 | Object-storage client libraries (TS and Python) | `platform/storage`, `ai/adapters/blob` | OD-2, OD-4 |
| G-5 | KMS client libraries (TS encrypt, Python decrypt) | `platform/crypto`, `workers/publish/secrets` | OD-2 |
| G-6 | Python Redis client (Canvas signals, crawl cache) | `ai/adapters/canvas`, `ai/adapters/crawler` | new |
| G-7 | Python JWT verification library for the service JWT | `ai/entrypoints/http` | new |
| G-8 | Python HTTP client for Meta and X APIs (if not provided through the MCP SDK's dependencies) | `workers/publish/platforms` | new |
| G-9 | JSON Schema → TypeScript type generator | `contracts/temporal` → `generated/` | OD-8 |
| G-10 | Logging sink, metrics and error tracking | §9.3 | OD-13 |
| G-11 | Rate-limiting mechanism | `platform/rate-limit` | OD-14 |
| G-12 | Email provider | `platform/mail` | OD-10 |
| G-13 | Phase 4+ only: image generation/composition library, shaping and rights | Future native image design, not Phase 2 build/test scope | OD-11 |
| G-14 | Local Temporal for Docker Compose | `deploy/compose`, workflow tests | OD-15 |
| G-15 | CI/CD runner and operator access model | `ci/`, BTD-ADR-16 | OD-16 |
| G-16 | Values: pagination defaults and maxima, upload size limit, Canvas grace window, Redis Streams and Canvas-state retention, idempotency-key expiry, maximum items per bulk decision, Batch Review Session idle timeout | §8, §9 | OD-18, §9.3 of SYS |

| G-17 | CSV/.xlsx parser and published templates, supported sheets/fields/limits; owner-media type/size/dimension limits and Facebook validation | FR-31, FR-38; resolve before release | SYS OD-18 |
| G-18 | Active cleanup deadlines, minimum audit retention, backup/trace/provider expiry, suppression-ledger and restore verification | FR-13, FR-42; NFR-09 | SYS OD-21 |
| G-20 | Google OIDC library/configuration, callback/session compatibility tests, token TTLs and delivery policy; no account-linking product rule remains open | §8.2 | identity implementation |
| G-21 | Calendar range limits/cursor consistency, adaptive-question policy evaluation and migration inventory if legacy deployment exists | §§6.4.1, 6.8.1, 7.6 | implementation |
| G-19 | Search/discovery provider baseline and safe reservation path for synchronous/child/concurrent calls | FR-24–28; NFR-06 | SYS OD-22 |
