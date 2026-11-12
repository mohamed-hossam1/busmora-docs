# Domain-Driven Design (`domain/`)

This directory documents the **Domain-Driven Design (DDD)** model for the BusMora platform. It establishes the ubiquitous language, defines strict bounded contexts, models tactical domain aggregates, and catalogues domain events.

---

## Specification Documents

```mermaid
flowchart LR
    G["1. Glossary (GLOSSARY.md)<br/><i>Ubiquitous Language</i>"] --> C["2. Bounded Contexts (contexts.md)<br/><i>Core, Supporting, Generic</i>"]
    C --> M["3. Domain Model (model.md)<br/><i>Aggregates, Entities, Invariants</i>"]
    M --> E["4. Domain Events (events.md)<br/><i>Transactional Outbox & Messaging</i>"]
```

### 1. [Glossary & Ubiquitous Language (`GLOSSARY.md`)](./GLOSSARY.md)
* The single source of truth for terminology across code, database schemas, UI text, and documentation.
* Enforces exact terminology (e.g. `Marketing Director`, `KnowledgeItem` instead of "Node", `Post` instead of "Publication", `Review` vs `Approval`, `Onboarding Draft`, `Roadmap Discovery`, `Owner-reported Done`, `Context Staleness Hold`, `Honest Handoff`).
* Defines lifecycle states, time zone semantics, and banned terminology.

### 2. [Bounded Contexts (`contexts.md`)](./contexts.md)
* Classifies the system contexts:
  * **Core Contexts:** *Business Brain* (the platform's durable moat), *Marketing Employee* (AI Marketing Director diagnosis, planning, execution), *Governed Publishing* (exact-version approval, verified Facebook publishing).
  * **Supporting Contexts:** *Market Intelligence* (multi-source research with per-task budgets), *Deliverables* (versioned non-Post documents and requested one-off CSV/.xlsx analysis), *Channel Connections* (Facebook in Phase 2; Instagram/X in Phase 3), *Evaluation* (internal 24-case benchmark suite).
  * **Generic Contexts:** *Identity & Workspace*, *Usage Metering*, *Audit Trail*, *Notifications*.
* Defines clear boundaries, upstream/downstream relationships, and ownership rules.

### 3. [Tactical Domain Model (`model.md`)](./model.md)
* Aggregates, entity lifecycles, and value objects per bounded context:
  * **Post Aggregate:** `Draft` $\rightarrow$ `PendingApproval` $\rightarrow$ `Approved` $\rightarrow$ `Scheduled` $\rightarrow$ `Publishing` $\rightarrow$ `Submitted` / `Uncertain` $\rightarrow$ `Published` / `Failed`, plus `Held`, `Cancelled`, `Expired`; undated drafts stay in Unscheduled.
  * **Identity & Knowledge Aggregates:** Auth identities, the confirmed Onboarding Draft, Knowledge Items with epistemic labels, relationships, procedural rules, Pending Brain Updates and real deletion.
  * **Task & Roadmap Aggregates:** Tasks with budgets and two-slot admission, saved Discovery, one active Roadmap with immutable versions and complete steps, Step Executions and Strategic Revisions.

### 4. [Domain Events Catalog (`events.md`)](./events.md)
* Authoritative catalog of immutable domain facts:
  * Naming conventions: Past tense, PascalCase (e.g. `OnboardingConfirmed`, `PostApproved`, `PostHeldOnStaleness`, `ContextVersionChanged`, `TaskBudgetExtensionRequested`, `StepSkipped`).
  * Event envelope schemas and transactional outbox persistence rules.
  * In-process dispatching for same-transaction consistency and Redis Streams transport for cross-process choreography.
