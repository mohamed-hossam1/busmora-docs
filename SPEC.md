# Technical Specification: BusMora AI Employee Platform & Marketing Employee (MVP)

## Problem Statement

Growing businesses and startups (specifically digital-first B2B professional services firms with 10–49 employees) require continuous, recurring marketing and operational capacity to stay competitive—such as conducting competitor intelligence, crafting marketing strategies, producing brand-aligned content across channels, analyzing performance data, and distributing campaigns. However, these businesses cannot afford to hire dedicated, multi-person marketing teams or specialized full-time staff.

Currently, when business operators turn to AI tools, they encounter two extremes, both of which fail them:
1. **Unconstrained prompt wrappers and fragile visual workflow builders**: These require business operators to become prompt engineers and workflow debuggers. They suffer from frequent breakages, lack structured roles, and place the burden of system assembly onto non-technical business owners.
2. **Context-blind and ungrounded generation**: Generic AI assistants lack deep, provenance-backed corporate memory. They hallucinate company capabilities, violate brand rules, drift off tone, and produce generic outputs that require extensive manual rework.

Furthermore, autonomous AI agents pose significant reputational and financial risks. Business owners cannot trust an autonomous agent to publish directly to public social channels, modify live marketing campaigns, or spend company funds without strict human oversight. At the same time, existing tools force users to manually transfer research, drafts, CSV data, and copy across disconnected applications with no durable audit trail or execution safety.

## Solution

BusMora is an AI Employee Platform that delivers pre-configured, production-ready AI Employees to handle recurring business operations under non-negotiable human governance.

The platform debuts with the **Marketing Employee** as its beachhead MVP, managing five bounded responsibility areas:
1. **Research & Intelligence**: Conducting competitor, market, and audience research grounded in verified company context and bounded public web search to produce Decision-Ready Research Briefs.
2. **Strategy & Planning**: Formulating multi-channel campaign strategies, quarterly objectives, and tactical marketing plans.
3. **Content Production & Adaptation**: Producing brand-aligned copy, headlines, and channel-adapted post variations that adhere strictly to company brand voice guidelines.
4. **Performance Interpretation**: Ingesting structured performance CSV data, validating schemas, computing deterministic KPIs, and synthesizing actionable diagnostic reports and strategic recommendations.
5. **Approval-Governed External Execution**: Preparing and executing social media publications to authorized organization-owned channels (e.g., LinkedIn Organization Page or Facebook Page) via a provider-agnostic Social Publishing MCP connector.

### Core Solution Pillars:
- **Human-in-the-Loop Governance**: The system can never autonomously publish content, alter live campaigns, spend money, or alter canonical knowledge. All external side effects require explicit human approval via an immutable Approval Action with a full preview inspector and a 24-hour expiration timer.
- **The Business Brain**: A company-scoped, provenance-backed repository of Canonical Company Knowledge structured around 9 predefined entity types. It is populated through guided onboarding web crawls (up to 10 pages), document uploads, and employee insights, all gated through a human Candidate Review Inbox (`/brain/candidates`).
- **Two-Stage GraphRAG Engine**: Combines vector semantic retrieval in Qdrant with 1–2 hop subgraph expansion in Neo4j to retrieve deeply contextualized, hallucination-free knowledge with complete provenance.
- **Dual-Pane Task Workspace**: An operational collaboration interface pairing a real-time conversational stream (left pane) with a rich Decision Canvas (right pane) that renders interactive Decision-Ready Artifacts and approval inspectors.
- **Durable Orchestration (Temporal-First)**: Reliable, fault-tolerant execution handling long-running research, deferred schedules (e.g., "publish tomorrow at 4 PM"), and recurring cron workflows without external message brokers.
- **Security & Privacy Boundary**: Complete Workspace tenant isolation, AWS KMS envelope encryption for third-party OAuth credentials, and a strict guarantee that Platform Admins have zero standing access to tenant data planes or secrets.

---

## User Stories

### Hybrid Onboarding & Company Setup
1. As an Authorized Company Human, I want to submit my company name and public website URL during onboarding, so that the platform can automatically discover and extract my company's foundational context.
2. As an Authorized Company Human, I want the onboarding crawl to run asynchronously in the background and allow me to skip directly to the dashboard, so that I am not blocked while web pages are being crawled and analyzed.
3. As an Authorized Company Human, I want to receive a persistent dashboard notification when candidate extraction completes, so that I know exactly when my company's initial context is ready for review.
4. As an Authorized Company Human, I want the onboarding crawler to respect robots.txt and limit crawling to a maximum of 10 public pages, so that the platform extracts relevant information responsibly without scraping unwanted or private areas.

### Business Brain & Canonical Knowledge Management
5. As an Authorized Company Human, I want to explore my company's Canonical Knowledge organized strictly across 9 predefined entity types, so that I have a clear, structured view of our brand facts, offerings, audiences, and strategic rules.
6. As an Authorized Company Human, I want to manually create new Knowledge Items under any of the 9 entity types, so that I can seed proprietary business facts directly into the Business Brain.
7. As an Authorized Company Human, I want to edit existing active Knowledge Items, so that our canonical corporate knowledge accurately reflects current company positioning.
8. As an Authorized Company Human, I want to retire outdated Knowledge Items without permanent UI deletion, so that old facts are excluded from GraphRAG retrieval while maintaining complete audit and historical provenance.
9. As an Authorized Company Human, I want each Knowledge Item to display its source document provenance and confidence score, so that I can verify where each fact originated.

### Candidate Review Inbox
10. As an Authorized Company Human, I want to view a dedicated Candidate Review Inbox (`/brain/candidates`) displaying extracted or proposed knowledge, so that unverified information is never automatically treated as canonical truth.
11. As an Authorized Company Human, I want to see a side-by-side comparison between candidate statements and existing matching canonical items, so that I can easily spot conflicting statements or updates.
12. As an Authorized Company Human, I want to "Approve as New" a candidate item, so that it becomes an active Knowledge Item in the Business Brain.
13. As an Authorized Company Human, I want to "Edit & Approve" a candidate item, so that I can refine the wording before promoting it to canonical status.
14. As an Authorized Company Human, I want to "Edit & Approve as Update" an existing item, so that the current canonical value is updated while retaining the previous version in the audit history.
15. As an Authorized Company Human, I want to "Reject" candidate items that are inaccurate or irrelevant, so that they are discarded from the review queue.
16. As an Authorized Company Human, I want candidate items to require individual review without bulk approval, so that our company memory maintains high factual integrity.

### Dual-Pane Task Workspace & Collaboration
17. As an Authorized Company Human, I want to initiate marketing tasks using natural language prompts within a dedicated task workspace, so that I can delegate complex marketing objectives to the Marketing Employee.
18. As an Authorized Company Human, I want to see token-level streaming and real-time step progress indicators in the conversational pane, so that I understand exactly what research or analysis the employee is performing.
19. As an Authorized Company Human, I want the task stream to automatically reconnect via Server-Sent Events (SSE) using the Last-Event-ID header after a network disruption, so that I do not lose in-flight task progress or streaming history.
20. As an Authorized Company Human, I want to inspect Decision-Ready Artifacts (reports, briefs, strategy tables) on a dedicated full-fidelity Decision Canvas (right pane), so that I can review structured outputs without cluttering the chat history.
21. As an Authorized Company Human, I want each task to clearly display the immutable Employee Configuration revision it was pinned to, so that I have full transparency regarding the exact instructions and models used for that run.
22. As an Authorized Company Human, I want to provide 1-to-5 star ratings and feedback comments on completed tasks, so that the team can monitor and improve employee performance over time.

### Marketing Employee Capabilities & Sub-Agents
23. As an Authorized Company Human, I want the Marketing Employee to perform competitor and audience research and output a Decision-Ready Research Brief, so that I can make informed positioning decisions without conducting manual research.
24. As an Authorized Company Human, I want the Marketing Employee to formulate goal-driven campaign strategies grounded in our active Strategic Goals and Brand Rules, so that our marketing initiatives directly support business objectives.
25. As an Authorized Company Human, I want the Marketing Employee to produce brand-aligned copy and channel-adapted post variations, so that our messaging remains consistent across diverse distribution channels.
26. As an Authorized Company Human, I want to upload marketing performance CSV files directly into a task, so that the Marketing Employee can validate schemas and compute key performance indicators.
27. As an Authorized Company Human, I want the Marketing Employee to diagnose underperformance from uploaded CSV data and produce actionable optimization recommendations, so that I can optimize marketing ROI based on empirical data.
28. As an Authorized Company Human, I want to run composite multi-step tasks (e.g., Research → Strategy → Content or Performance CSV → Revision), so that complex end-to-end marketing workflows are completed cohesively in a single task session.

### Approval-Governed External Execution & Scheduling
29. As an Authorized Company Human, I want any external social post prepared by the employee to be presented as an immutable Approval Action card, so that no external side effect can occur without my explicit consent.
30. As an Authorized Company Human, I want to inspect the exact post preview, target organization channel, scheduled time, and payload on the Decision Canvas before approving, so that I have complete confidence in what will be published.
31. As an Authorized Company Human, I want to click "Approve Now" on an action card to trigger immediate execution, so that verified content is published without unnecessary delay.
32. As an Authorized Company Human, I want to schedule an approved post for a future publication date and time, so that the system automatically dispatches the post at the optimal moment.
33. As an Authorized Company Human, I want scheduled posts to be held by durable orchestration timers that survive server restarts or network outages, so that publication timing remains dependable.
34. As an Authorized Company Human, I want to cancel or reschedule a scheduled post prior to dispatch, so that I can retract or adjust planned publications if business priorities change.
35. As an Authorized Company Human, I want approval actions to automatically expire after 24 hours if no action is taken, so that outdated or stale drafts are never inadvertently published.
36. As an Authorized Company Human, I want to request changes or edits on a proposed approval action, so that the employee generates a revised proposal that supersedes the prior draft.
37. As an Authorized Company Human, I want the system to execute a preflight reauthorization check when a scheduled post wakes up, so that expired or revoked OAuth tokens alert me immediately rather than failing silently.

### Social Publishing & OAuth Integrations
38. As an Authorized Company Human, I want an interactive Just-In-Time (JIT) OAuth Connect Card to appear directly in the chat stream when an employee prepares an action requiring an unconnected integration, so that I can connect the channel without leaving my workflow.
39. As an Authorized Company Human, I want the in-flight task to automatically resume once JIT OAuth connection is completed, so that I do not have to restart or re-prompt the task.
40. As an Authorized Company Human, I want to manage and view connection health across organization targets in `/settings/integrations`, so that I can monitor token status, expiration, and connected account names.
41. As an Authorized Company Human, I want social publishing to strictly target authorized organization-owned Pages (e.g., LinkedIn Organization Pages, Facebook Pages) and prohibit personal profiles, so that personal privacy and corporate brand governance are safeguarded.
42. As an Authorized Company Human, I want post publication to transition to success only after read-back verification confirms the post is live and publicly visible, so that I am guaranteed the action succeeded.
43. As an Authorized Company Human, I want ambiguous network timeouts during dispatch to be reconciled via feed inspection rather than blind re-posting, so that accidental duplicate posts are completely avoided.

### Platform Administration & Assembly
44. As a Platform Admin, I want to manage an Employee Catalog at `/admin/employees`, so that I can define and maintain standardized AI Employees for the platform.
45. As a Platform Admin, I want to configure an Employee's name, description, system prompt instructions, approval policies, and assigned sub-agents, tools, and MCP bindings, so that I can assemble specialized AI Employees from registered codebase components.
46. As a Platform Admin, I want admin edits to create drafts and publish immutable configuration revisions (e.g., rev-1, rev-2), so that existing in-flight tasks remain pinned to their initial revision without disruption.
47. As a Platform Admin, I want the admin portal to be strictly isolated with zero access to tenant Brain items, chat transcripts, task artifacts, or credentials, so that tenant confidentiality and data privacy are cryptographically and architecturally guaranteed.

### Security, Multi-Tenancy & Audit Logging
48. As an Authorized Company Human, I want all third-party OAuth access tokens and secrets to be encrypted using AWS KMS envelope encryption bound to my Workspace ID, so that my credentials can never be accessed or decrypted by other tenants or platform operators.
49. As an Authorized Company Human, I want an immutable, append-only security audit log at `/audit`, so that every canonical knowledge promotion, approval decision, external execution attempt, and OAuth state change is recorded for governance.
50. As an Authorized Company Human, I want my workspace data strictly partitioned by Workspace ID across relational, graph, vector, and object storage, so that zero cross-tenant data leakage can ever occur.

---

## Implementation Decisions

### 1. Multi-Service Architecture & Boundaries
The platform is partitioned into four distinct application modules:
- **`busmora-web` (Client & Admin Portals)**: A single Next.js 15 application hosting both the Platform Admin portal (`busmora.com/admin/*`) and the Client Workspace portal (`busmora.com/w/{slug}/*`). The portals maintain strictly isolated route trees, sessions, and navigation shells.
- **`busmora-api` (Core Business API)**: A NestJS application handling custom authentication (Argon2id password hashing, short-lived JWT access tokens, Redis-backed rotating refresh tokens), tenant workspace boundaries, Drizzle ORM persistence to PostgreSQL, Temporal workflow dispatch with frozen configuration snapshots, and an SSE gateway that forwards real-time execution events from Redis streams.
- **`busmora-workers` (Durable Orchestration Workers)**: Python Temporal workers executing durable workflows (`TaskExecutionWorkflow`, `OnboardingWorkflow`), handling workflow signals (`approve`, `revise`, `cancel`, `oauth_connected`), managing 24-hour expiration timers and deferred schedule sleeps, acquiring pre-dispatch idempotency locks, and interfacing with external provider adapters.
- **`busmora-ai` (Agent Intelligence & Knowledge Runtime)**: A FastAPI service hosting the `ModelGateway`, LangGraph sub-agent graph orchestration (Research, Strategy, Content, CSV Performance intake), shallow crawler (max 10 pages), candidate extractor, and the Two-Stage GraphRAG retrieval engine.

### 2. Durable Orchestration Invariant (Temporal-First, No Message Broker)
- All distributed task execution, durable activity retries, long-running waits, approval holds, 24-hour expiration timers, and deferred schedule timers are managed natively through **Temporal Cloud** (with a documented runbook for self-hosted Temporal migration).
- **No separate message broker** (Kafka, RabbitMQ, Celery, or SQS) is deployed anywhere in the platform architecture.
- When a task begins, `busmora-api` serializes the active Employee Configuration into an immutable JSON payload and passes it into the Temporal workflow input. In-flight tasks remain pinned to this frozen configuration snapshot regardless of future admin edits.

### 3. Multi-Model Data Plane
Specialized data planes handle distinct storage workloads:
- **PostgreSQL**: Relational transactional domain state, users, workspaces, memberships, employees, configuration revisions, tasks, task records, approval actions, execution attempts, source document metadata, and immutable audit events.
- **Neo4j**: Business Brain knowledge graph strictly enforcing the 9 predefined entity types (`CompanyProfile`, `Offering`, `Audience`, `BrandRule`, `Positioning`, `StrategicGoal`, `Channel`, `Competitor`, `CompanyFact`). Every Cypher query enforces tenant isolation (`WHERE n.workspace_id = $workspace_id AND n.status = 'active'`).
- **Qdrant**: Vector embeddings (1536 dimensions, Cosine distance) for hybrid semantic retrieval, indexed on `workspace_id` and `status`.
- **Amazon S3**: Object storage for raw evidentiary documents, crawl HTML snapshots, and uploaded CSV files partitioned deterministically: `s3://busmora-sources/{workspace_id}/{source_type}/{timestamp}_{filename}`.
- **Redis**: Caching, rate limiting, and durable SSE replay stream buffers keyed by `task_id:{id}`.

### 4. Two-Stage Hybrid GraphRAG Engine
Context retrieval for AI Employee prompts executes as a two-stage pipeline:
- **Stage 1 (Vector Retrieval)**: Semantic similarity search in Qdrant filtered by `workspace_id` and `status == 'active'` returning Top-K seed Knowledge Item IDs.
- **Stage 2 (Subgraph Expansion)**: Cypher query in Neo4j expanding 1–2 hops around seed items, traversing connected Audiences, Offerings, Brand Rules, and Competitor nodes, collecting full provenance and freshness metadata.
- **Experimental Control Baseline**: In parallel, a document-only control route chunks raw S3 documents into Qdrant vectors without graph extraction to benchmark GraphRAG performance in the Quality Gate.

### 5. Approval Action State Machine & External Execution
Approval actions follow an immutable lifecycle with deterministic state transitions:

```
[draft] ──(check OAuth unconnected)──► [awaiting_authorization] ──(JIT OAuth)──► [ready_for_approval]
[draft] ──(check OAuth connected)────► [ready_for_approval]
[ready_for_approval] ──(User approves)──────────────► [approved] ──► [executing]
[ready_for_approval] ──(User schedules future time)──► [scheduled] ──► [executing (at timestamp)]
[ready_for_approval] ──(User requests changes)──────► [superseded]
[ready_for_approval] ──(User rejects)───────────────► [rejected]
[ready_for_approval] ──(24h timer expires)──────────► [expired]
[scheduled] ──────────(User cancels)───────────────► [cancelled]
[executing] ──────────(Provider accepted + read-back verified)──► [succeeded]
[executing] ──────────(Provider rejection)─────────────────────► [failed]
[executing] ──────────(Network timeout / 5xx)──────────────────► [indeterminate] ──(Reconciliation)──► [succeeded / failed]
```

- **Pre-Dispatch Lock**: `busmora-workers` creates an `execution_attempts` record with a unique `idempotency_key` before calling any external provider API.
- **Read-Back Verification**: Succeeded state requires both an accepted provider response (HTTP 200/201) and an immediate follow-up read-back GET confirming public visibility.
- **Indeterminate Reconciliation**: Ambiguous timeouts transition to `indeterminate` and trigger a feed inspection reconciliation query before any retry is considered.

### 6. Provider-Agnostic Social Publishing MCP Connector (`mcp:social_publishing:v1`)
- Core platform workflows interact with a generic Social Publishing MCP interface exposing: `get_connection_status`, `list_authorized_targets`, `preflight_action`, `execute_approved_action`, `get_execution_status`, and `remediate`.
- Isolated provider adapters in `busmora-workers` translate calls to target networks (e.g., LinkedIn Organization Pages or Meta/Facebook Pages).
- Strictly restricted to organization-owned targets; personal profile publishing is explicitly disallowed across all adapters.

### 7. AWS KMS Envelope Encryption Vault
- Integration OAuth access tokens and credentials are encrypted using AWS KMS envelope encryption.
- Each integration generates a unique Data Encryption Key (DEK) bound to an explicit Workspace Encryption Context: `{ workspace_id, integration_id, credential_version }`.
- Decryption requires an exact matching `workspace_id`. Platform Admins have zero access to DEKs or plaintext credentials.

### 8. Reconnectable SSE Event Streaming
- `busmora-ai` publishes execution events to a Redis Stream.
- `busmora-api` streams events to `busmora-web` via Server-Sent Events with sequential message IDs.
- Clients reconnecting after drops send `Last-Event-ID: {n}`, allowing `busmora-api` to replay missed tokens and cards from the Redis buffer without interrupting running Temporal workflows.

---

## Testing Decisions

### What Makes a Good Test
Tests must verify **external observable behavior** across defined system seams, not internal implementation mechanics or transient variables. A test passes if the system produces the correct HTTP response, triggers the expected durable state transition, emits the correct SSE events, and respects security and data integrity invariants.

### The System Testing Seams
1. **Primary End-to-End System Seam: Client HTTP REST / SSE & Temporal Workflow Seam**
   - The primary seam for integration and end-to-end testing sits at the authenticated HTTP interface of `busmora-api` and the SSE event stream.
   - Tests issue authenticated requests (creating tasks, querying brain items, submitting approvals), subscribe to SSE streams, and verify that the system moves through valid domain states.
   - **External Test Adapters**: External dependencies (LLM APIs in `ModelGateway`, social network APIs in the Social Publishing MCP adapter, and target web pages in the crawler) are placed behind external adapters and mocked/stubbed during automated regression runs.
2. **Evaluation Harness Seam: LangSmith 84-Run Quality Gate Seam**
   - Release readiness is governed by a dedicated evaluation runner exercising the Marketing Employee over 84 runs across 3 hidden Truth Packets in LangSmith.
   - Tests assert deterministic boundary invariants (valid JSON schemas, zero unapproved external calls, correct mathematical KPI calculations) and submit runs to two-reviewer blinded human grading.

### Modules Tested
- **`busmora-api` Core Gateway & Auth**: Authentication flows (Argon2id, JWTs, Redis refresh tokens), tenant workspace isolation guards, Drizzle relational schema migrations, task creation, and SSE replay logic.
- **`busmora-workers` Durable Orchestration**: Temporal `TaskExecutionWorkflow` and `OnboardingWorkflow`, signal handling (`signal_approve_action`, `signal_cancel_action`), 24-hour expiration timers, deferred schedule sleeps, and pre-dispatch idempotency locking.
- **`busmora-ai` Agent Runtime & GraphRAG**: LangGraph sub-agent graph execution, Pydantic structured output validation, ModelGateway fallback and retry handling, shallow crawler page boundary enforcement (max 10 pages), and Neo4j Cypher tenant filtering.
- **Social Publishing MCP Adapter**: Organization channel preflight validation, simulated post dispatch, read-back verification assertions, and indeterminate feed reconciliation.
- **KMS Vault & Security**: Envelope encryption and decryption assertions ensuring cross-workspace decryption attempts fail cryptographically.

### Prior Art & Test Patterns
- Prior art in the repository's engineering skills emphasizes testing through deep module interfaces using external seams (Michael Feathers' seam concepts), avoiding shallow pass-through mocks, and verifying state transitions and contract guarantees.

---

## Out of Scope

The following capabilities are explicitly out of scope for this MVP release:
1. **Autonomous External Execution**: Autonomous posting, publishing, or live updating without explicit human approval is strictly prohibited.
2. **Financial Transactions & Paid Ad Spend**: Managing live ad spend, credit cards, bidding engines, or modifying paid campaign budgets.
3. **Personal Social Profile Publishing**: Publishing to personal LinkedIn or Facebook profiles (only organization-owned Pages/Channels are supported).
4. **Visual Workflow Canvas / Node-Based Builders**: Drag-and-drop workflow builders or user-authored execution graphs (employees are configured via structured admin forms from engineering-registered building blocks).
5. **Granular Multi-Role Workspace RBAC**: Custom roles or granular permission tiers within a tenant (all active Workspace members share uniform permissions in the MVP).
6. **Broad Unbounded Web Crawling**: Crawling beyond 10 public pages or crawling dynamic, authenticated, or JavaScript-heavy single-page applications without explicit user setup.
7. **Direct UI Hard-Deletion of Canonical Knowledge**: Hard deletion of Knowledge Items is prohibited; items can only be retired to preserve audit provenance.
8. **Automated LLM-as-a-Judge Release Authority**: Automated LLM evaluation systems acting as gatekeepers; human review in LangSmith is the sole authority for release readiness.
9. **Separate Message Brokers**: Deployment of Kafka, RabbitMQ, Celery, or SQS (Temporal handles all queues, timers, and workflows).
10. **Additional AI Employee Roles in Initial Release**: Roles such as Sales Representative or Support Agent are deferred to post-MVP releases (although the architecture has been verified for extensibility).

---

## Further Notes

- **24-Week Delivery Roadmap**: Structured across six 4-week milestones (M1: Foundations & Walking Skeleton, M2: Business Brain & GraphRAG, M3: Marketing Employee Sub-Agents, M4: Approvals & Social MCP, M5: Quality Gate Evaluation, M6: Controlled Pilot & Hardening).
- **Vertical Slice Ownership**: The 4-student engineering team operates on an end-to-end vertical slice ownership model, with each student delivering features across the full stack (`busmora-web` → `busmora-api` → `busmora-workers` → `busmora-ai`).
- **Core Planning Principle**: "Simplify implementation, not the agreed product contract." If schedule constraints arise, simplify implementation complexity (e.g. rely on managed cloud services or simpler forms) rather than eliminating agreed functional capabilities.
- **Geographic Deployment**: Single-region cloud deployment in Frankfurt (`eu-central-1`).

