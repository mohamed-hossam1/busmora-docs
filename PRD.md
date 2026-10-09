# Product Requirements Document (PRD): BusMora AI Employee Platform (MVP)

> **Document Status**: Authoritative Product Requirements Document (PRD)  
> **Release Target**: BusMora Beachhead MVP — The Marketing Employee  
> **Primary ICP**: Digital-First B2B Professional Services Firms (10–49 Employees)  
> **Companion Document**: [Master System Specification & Technical Architecture](BUSMORA-MASTER-SPECIFICATION.md) | [Technical Specification](SPEC.md)

---

## 1. Executive Summary & Vision

### 1.1 Product Vision
**BusMora** is an AI Employee platform built to deliver **autonomous operational capacity without autonomous business risk**. 

Growing businesses struggle with recurring operational demands—such as competitor research, strategic planning, channel-adapted content creation, and data interpretation. Yet, hiring specialized full-time staff is cost-prohibitive, and existing AI solutions force operators to choose between two bad alternatives: generic, hallucinating chatbots that lack brand memory, or brittle visual workflow builders that require non-technical operators to become prompt engineers and pipeline debuggers.

BusMora introduces the **AI Employee** paradigm: pre-configured, role-specialized digital team members that join a company's private workspace, deeply absorb its brand and business truth into a provenance-backed **Business Brain**, and execute multi-step workflows under **strict, non-negotiable human governance**.

### 1.2 Beachhead Product: The Marketing Employee
For its MVP release, BusMora focuses exclusively on a single high-impact role: **The Marketing Employee**. 
The Marketing Employee assumes five bounded responsibility areas:
1. **Competitor & Market Intelligence**: Producing Decision-Ready Research Briefs grounded in verified business context.
2. **Strategy & Campaign Planning**: Formulating quarterly and tactical multi-channel marketing roadmaps.
3. **Brand-Aligned Content Production**: Crafting channel-adapted copy and messaging variants strictly compliant with corporate brand voice.
4. **Performance Data Diagnosis**: Ingesting raw performance data (CSV), calculating deterministic KPIs, and synthesizing diagnostic reports.
5. **Approval-Governed External Execution**: Preparing publication drafts and dispatching verified posts to authorized company social channels (e.g., LinkedIn/Facebook Company Pages) only after explicit human sign-off.

---

## 2. Problem Statement & Customer Validation

### 2.1 The Core Market Problem
Digital-first B2B professional services firms (consultancies, agencies, IT services, and advisory firms with 10–49 employees) face intense pressure to maintain active marketing and thought leadership. However:
- **Budget & Hiring Constraints**: They cannot justify hiring a dedicated 3-to-5-person internal marketing team, and retaining external agencies is expensive and often disconnects from daily operational realities.
- **The AI Prompt & Workflow Failure**:
  - *Generic Chatbots*: Lack corporate memory, hallucinate services and capabilities, drift off-brand, and generate superficial content that requires heavy rework.
  - *Visual Workflow Builders & Automation Tools*: Require non-technical founders to assemble nodes, troubleshoot API failures, and craft prompts, shifting the engineering burden onto the customer.
- **The Autonomy Trust Gap**: Business operators cannot risk letting an autonomous AI agent publish directly to public brand channels, alter campaigns, or modify corporate truth without oversight. At the same time, manual copy-pasting across disconnected apps destroys productivity.

### 2.2 Ideal Customer Profile (ICP)
- **Company Size**: 10–49 employees (primary sweet spot: 20–49).
- **Business Model**: Digital-first B2B services, technology consulting, specialized agencies.
- **Current Marketing State**: Led by a founder/operator or a solo generalist marketer; active on at least one corporate channel (e.g., LinkedIn Company Page); owns historical performance data (analytics/CRM CSV exports).
- **Core Buying Motivation**: Scaling consistent marketing execution without hiring headcount or managing complex software pipelines.

---

## 3. User Personas & Jobs-to-be-Done (JTBD)

### 3.1 Persona 1: Tariq — The Overwhelmed B2B Founder / Operator
- **Role**: Managing Director / Co-founder at a 25-person tech consultancy.
- **Pain Points**:
  - Drowning in client delivery; marketing is sporadic and inconsistent.
  - Wants company thought leadership and positioning, but lacks time to write or research.
  - Fearful of AI "going rogue" or sounding generic and unprofessional.
- **Job-to-be-Done**:  
  *"When our market demands consistent presence and thought leadership, I want an intelligent team member who already knows our offerings and brand voice to prepare complete briefs and draft posts, so that I only spend 5 minutes reviewing and approving them without worrying about brand damage."*

### 3.2 Persona 2: Layla — The Solo Marketing Lead
- **Role**: Head of Marketing (Team of 1) at a 35-person professional services firm.
- **Pain Points**:
  - Responsible for everything: competitor research, writing copy, analyzing metrics, and posting.
  - Context switching between spreadsheets, docs, and social networks kills strategic focus.
  - Spends hours formatting CSV metrics and drafting multiple variations of the same message.
- **Job-to-be-Done**:  
  *"When I need to launch a new campaign or analyze monthly performance, I want an AI colleague to crunch the numbers, extract competitor insights, and prepare multi-channel copy variations, so that I can focus on strategic decisions rather than repetitive execution."*

### 3.3 Persona 3: Karim — The Platform Administrator (Internal)
- **Role**: BusMora Internal Operations & Product Assembly Lead.
- **Pain Points**:
  - Needs to deliver standardized, high-performing AI Employees without writing bespoke code for each tenant.
  - Must ensure prompt revisions and tool configurations do not break in-flight customer tasks.
  - Must uphold zero-access privacy guarantees regarding customer proprietary data.
- **Job-to-be-Done**:  
  *"When new tools or prompt improvements are developed, I want to assemble and publish versioned Employee configurations safely, so that all tenants receive consistent, tested capabilities without exposing customer data."*

---

## 4. Product Value Proposition & Core Solution Pillars

```
+-----------------------------------------------------------------------------------+
|                               BUSMORA PLATFORM                                    |
+-----------------------------------------------------------------------------------+
|  1. Pre-Configured AI Employee        |  2. The Business Brain                    |
|     - Role-specialized digital staff  |     - Provenance-backed corporate memory  |
|     - Marketing Employee beachhead    |     - 9 canonical business entity types   |
|     - Multi-step bounded autonomy     |     - Human candidate review inbox        |
+---------------------------------------+-------------------------------------------+
|  3. Non-Negotiable Human Governance   |  4. Dual-Pane Collaboration Workspace     |
|     - Zero unapproved executions      |     - Conversational stream (left pane)   |
|     - Immutable approval cards        |     - Decision Canvas (right pane)        |
|     - 24-hour expiration timers       |     - Structured Decision-Ready Artifacts |
+---------------------------------------+-------------------------------------------+
|  5. Safe Execution & Scheduling       |  6. Absolute Tenant Isolation             |
|     - JIT social channel connection   |     - Zero cross-tenant data leakage      |
|     - Read-back verification          |     - Zero platform admin standing access |
|     - Durable scheduled dispatch      |     - Immutable audit trail               |
+-----------------------------------------------------------------------------------+
```

1. **Pre-Configured AI Employee (Not a Blank Slate)**: Users hire an employee with defined job responsibilities, pre-assembled capabilities, and strict behavioral boundaries—not an empty prompt box or complicated node canvas.
2. **The Business Brain (Company Truth with Provenance)**: A dedicated corporate memory structured around 9 business entities. All knowledge is traceable to source documents or website crawls, and new facts are promoted to canonical status only via human review.
3. **Non-Negotiable Human-in-the-Loop Governance**: AI proposes; humans authorize. The system cannot publish content, alter live campaigns, spend money, or alter corporate facts without an explicit human signature.
4. **Dual-Pane Task Workspace**: Combines a real-time conversational stream (for task delegation, step progress, and iterative feedback) with a dedicated **Decision Canvas** (for reviewing rich reports, strategy tables, and approval previews side-by-side).
5. **Verifiable External Action**: Outbound actions to authorized organization channels feature pre-dispatch idempotency, scheduled release timers, and immediate read-back verification to guarantee publication accuracy without duplicates.
6. **Enterprise-Grade Privacy & Auditability**: Workspaces are completely isolated. Platform administrators have zero standing access to customer knowledge or credentials, and all actions are recorded in an immutable audit trail.

---

## 5. Goals & Success Metrics (KPIs)

### 5.1 Business & Adoption Metrics
| Metric | Definition | Target (MVP) |
|---|---|---|
| **Time to First Value (TTFV)** | Time from completing onboarding to inspecting the first Decision-Ready Artifact | < 15 minutes |
| **Weekly Active Workspaces (WAW)** | Percentage of active workspaces delegating $\ge 2$ tasks per week | $\ge 60\%$ |
| **Task Completion Rate** | Tasks reaching successful artifact generation without abandonment | $\ge 85\%$ |
| **Onboarding Conversion Rate** | Users completing website crawl and approving initial context | $\ge 75\%$ |

### 5.2 Quality & Trust Metrics
| Metric | Definition | Target (MVP) |
|---|---|---|
| **Artifact Acceptance Rate** | Deliverables approved by the user with minor/no edits ($< 20\%$ diff) | $\ge 75\%$ |
| **Autonomous Action Invariant** | Rate of external actions published without human approval | **0.00% (Strict Zero)** |
| **Brand Voice Fidelity** | Human rating on brand alignment and tone consistency | $\ge 4.2 / 5.0$ |
| **Knowledge Hallucination Rate** | Factually contradictory claims about company services | $< 2\%$ of generated claims |

### 5.3 Efficiency Metrics
| Metric | Definition | Target (MVP) |
|---|---|---|
| **Time Saved per Marketing Asset** | Reduction in hours spent researching and drafting campaigns vs. manual work | $\ge 70\%$ time reduction |
| **Review Turnaround Time** | Time human spends reviewing and signing off an Approval Card | $< 2$ minutes per action |

---

## 6. Detailed User Journeys

### Journey 1: Guided Onboarding & Instant Brain Seeding
1. **Initiation**: The user signs up and enters their company name, domain, and public website URL.
2. **Background Context Discovery**: The system initiates an automated shallow crawl (up to 10 public pages, respecting `robots.txt`).
3. **Unblocked Progression**: The user is not blocked; they can explore the dashboard immediately while discovery runs asynchronously.
4. **Completion Notification**: When extraction completes, a persistent notification alerts the user: *"Your company's context is ready for review."*
5. **Candidate Review**: The user visits `/brain/candidates` to approve or adjust extracted knowledge facts (offerings, target audiences, brand rules).

### Journey 2: Delegating a Task & Working in the Dual-Pane Workspace
1. **Task Delegation**: In `/tasks`, the user prompts the Marketing Employee: *"Analyze our competitor X and propose a 3-post LinkedIn thought-leadership campaign highlighting our custom integration capability."*
2. **Transparent Reasoning**: In the left pane, the user watches real-time streaming progress indicators (e.g., retrieving brand facts, crawling competitor public page, synthesizing strategy).
3. **Canvas Inspection**: The right pane updates in real time to render a rich, formatted **Decision-Ready Research & Campaign Brief**.
4. **Iterative Refinement**: The user replies in the chat: *"Tone down post 2 to sound more technical and less promotional."* The employee updates the canvas artifact in place.

### Journey 3: Approving & Scheduling an External Social Post
1. **Action Generation**: When the user requests publication, the employee produces an **Approval Action Card** on the Decision Canvas.
2. **Inspection**: The card displays exact target channel (e.g., "Acme LinkedIn Page"), live rendered preview, proposed time, and action buttons.
3. **Just-In-Time (JIT) Connection**: If the channel is not yet linked, a JIT connect prompt appears directly in the workflow. The user links the account and the task automatically resumes.
4. **Governance Decision**:
   - **Approve Now**: Immediately publishes with verified read-back.
   - **Schedule for Later**: Sets a future timestamp (e.g., tomorrow at 10 AM).
   - **Request Revision**: Requests changes, creating an updated draft and superseding the previous one.
   - **Expire**: Unreviewed actions automatically expire after 24 hours to prevent accidental stale posting.

### Journey 4: Uploading Performance CSV & Getting Strategic Insights
1. **Data Upload**: The user drags and drops a raw performance export (e.g., monthly campaign impressions, clicks, conversions).
2. **Deterministic Processing**: The employee validates headers, computes standardized KPIs (CTR, conversion rate, CPC changes), and flags anomalies.
3. **Strategic Recommendations**: The canvas presents an executive diagnosis: what performed well, what underperformed, and three prioritized adjustments for next month's content strategy.

---

## 7. Functional Requirements

### 7.1 Hybrid Onboarding & Company Context Discovery
- **FR-1.1**: The system shall allow users to input company name, website URL, and primary industry during initial setup.
- **FR-1.2**: Context discovery shall run asynchronously, allowing users to proceed to the workspace immediately without blocking.
- **FR-1.3**: Automated crawler shall strictly respect `robots.txt` and cap public web extraction at a maximum of 10 pages.
- **FR-1.4**: Context extraction shall categorize discovered facts into initial Candidate Knowledge items and notify the user upon completion.

### 7.2 The Business Brain & Canonical Knowledge Management
- **FR-2.1**: The Business Brain shall organize canonical knowledge strictly across 9 structured entity types:
  1. `CompanyProfile` (mission, founding, core description)
  2. `Offering` (products, services, solutions, packages)
  3. `Audience` (buyer personas, ICP, industry verticals)
  4. `BrandRule` (tone of voice, forbidden words, stylistic constraints)
  5. `Positioning` (value propositions, differentiators, key messaging)
  6. `StrategicGoal` (quarterly targets, active marketing objectives)
  7. `Channel` (active distribution platforms, posting frequency)
  8. `Competitor` (direct/indirect competitors, strengths, weaknesses)
  9. `CompanyFact` (statistics, case study proof points, verifiable certifications)
- **FR-2.2**: Users shall be able to manually create, view, edit, and retire Knowledge Items under any entity type.
- **FR-2.3**: Knowledge Items cannot be permanently deleted from the UI; items are transitioned to `retired` status to preserve full audit provenance.
- **FR-2.4**: Every Knowledge Item shall display its source provenance (original URL, document name, extraction timestamp) and verification state.

### 7.3 Candidate Review Inbox (`/brain/candidates`)
- **FR-3.1**: Extracted or proposed facts shall be isolated in a dedicated Candidate Inbox and never treated as canonical truth until approved.
- **FR-3.2**: The inbox shall present side-by-side diff comparisons between proposed statements and existing active canonical items.
- **FR-3.3**: Users shall have four distinct review actions for each candidate:
  - *Approve as New*: Creates a new active canonical item.
  - *Edit & Approve*: Allows wording adjustments prior to canonical promotion.
  - *Edit & Approve as Update*: Replaces an existing canonical item while preserving audit history.
  - *Reject*: Discards inaccurate or unwanted candidate facts.
- **FR-3.4**: Bulk approval shall be explicitly prohibited to ensure intentional human review and maintain high factual integrity.

### 7.4 Dual-Pane Task Workspace & Collaboration
- **FR-4.1**: The workspace UI shall provide a dual-pane layout: a real-time conversation stream on the left, and a full-fidelity Decision Canvas on the right.
- **FR-4.2**: The conversation pane shall render token-level streaming and visible step progress indicators (e.g., "Searching Brain...", "Synthesizing Brief...").
- **FR-4.3**: The task stream shall automatically recover and resume state without progress loss upon network disconnection.
- **FR-4.4**: The Decision Canvas shall render formatted Decision-Ready Artifacts (markdown, structured tables, visual cards) distinct from the chat stream.
- **FR-4.5**: Each task shall visibly link to the exact, immutable Employee Configuration revision under which it was executed.
- **FR-4.6**: Completed tasks shall allow user satisfaction ratings (1 to 5 stars) and qualitative feedback.

### 7.5 Marketing Employee Capabilities
- **FR-5.1 (Research & Intelligence)**: Shall autonomously conduct competitor and audience research and output a Decision-Ready Research Brief.
- **FR-5.2 (Strategy & Planning)**: Shall formulate structured campaign roadmaps aligned with active company Strategic Goals and Brand Rules.
- **FR-5.3 (Content Production)**: Shall generate channel-adapted copy variants (headlines, body copy, hashtags) strictly honoring defined brand voice rules.
- **FR-5.4 (Performance Interpretation)**: Shall ingest structured marketing CSV files, validate column headers, compute deterministic metrics, and generate diagnostic reports with actionable recommendations.
- **FR-5.5 (Multi-Step Tasks)**: Shall support composite workflows in a single task session (e.g., Research $\rightarrow$ Strategy $\rightarrow$ Content Variations).

### 7.6 Approval-Governed External Execution & Scheduling
- **FR-6.1**: Every proposed external side effect shall be rendered as an immutable **Approval Action Card** requiring explicit human action.
- **FR-6.2**: The Approval Card shall present the exact target organization channel, live rendered preview, scheduled time, and payload.
- **FR-6.3**: Users shall have the option to *Approve Immediately*, *Schedule for Future Date/Time*, *Request Changes*, or *Reject*.
- **FR-6.4**: Scheduled actions shall survive system interruptions and dispatch automatically at the designated timestamp.
- **FR-6.5**: Approval cards shall automatically expire after 24 hours of inactivity to prevent accidental publication of stale drafts.
- **FR-6.6**: Users shall be able to cancel or reschedule any pending scheduled action prior to dispatch.
- **FR-6.7**: Published actions shall confirm success only after performing a read-back verification confirming the post is live on the target network.

### 7.7 Social Integrations & Channel Governance
- **FR-7.1**: The platform shall provide Just-In-Time (JIT) connection cards inside the task flow when an action targets an unlinked channel.
- **FR-7.2**: Social publishing shall be strictly restricted to verified **organization-owned Pages/Channels** (e.g., LinkedIn Company Page, Facebook Page); personal profile publishing is strictly prohibited.
- **FR-7.3**: An integrations hub at `/settings/integrations` shall allow monitoring connection health, expiration status, and authorized target names.
- **FR-7.4**: Network timeouts during dispatch shall trigger automated feed inspection to prevent duplicate posts.

### 7.8 Platform Administration & Employee Assembly
- **FR-8.1**: Platform Admins shall manage a centralized Employee Catalog at `/admin/employees`.
- **FR-8.2**: Admins shall assemble AI Employees from registered capabilities, configuring names, descriptions, system prompts, approval policies, and assigned tools.
- **FR-8.3**: Modifications to Employee definitions shall be published as immutable versioned revisions (`rev-1`, `rev-2`), ensuring running customer tasks are never broken by upstream prompt edits.
- **FR-8.4**: The Admin Portal shall maintain strict architectural isolation, with zero standing access to customer workspace data, Brain facts, or credentials.

### 7.9 Security, Multi-Tenancy & Audit Trail
- **FR-9.1**: Workspaces shall be strictly isolated by Tenant ID across all data stores; cross-tenant access is structurally impossible.
- **FR-9.2**: Third-party credentials and tokens shall be protected using workspace-bound envelope encryption.
- **FR-9.3**: The platform shall provide an immutable, append-only security audit log at `/audit`, tracking all knowledge promotions, approvals, dispatches, and credential modifications.

---

## 8. Non-Functional Requirements (NFRs)

### 8.1 Security & Compliance
- **NFR-1.1 (Cryptographic Isolation)**: Customer credentials and OAuth tokens must be encrypted with keys bound to the specific workspace tenant.
- **NFR-1.2 (Zero Admin Visibility)**: Platform administrators must have zero visibility into customer confidential Brain facts, task transcripts, or stored credentials.
- **NFR-1.3 (Data Sovereignty)**: All data processing and storage must reside within the specified deployment region (`eu-central-1`).

### 8.2 Reliability & Fault Tolerance
- **NFR-2.1 (Durable Execution)**: Long-running research tasks, approval holds, and scheduled release timers must survive process crashes and server reboots without state loss.
- **NFR-2.2 (Idempotent Dispatch)**: All outbound social publishing actions must be locked and idempotent; no network glitch may result in duplicate posts.
- **NFR-2.3 (Availability)**: The client workspace portal must maintain $\ge 99.5\%$ operational availability during business hours.

### 8.3 Performance & User Experience
- **NFR-3.1 (Streaming Responsiveness)**: Time to first token in the conversational task pane must not exceed 2.5 seconds.
- **NFR-3.2 (Seamless Reconnection)**: If network connectivity drops during an active task, the UI must re-establish the stream and restore missing events within 3 seconds of reconnection.
- **NFR-3.3 (Canvas Rendering)**: Decision-Ready Artifacts must render smoothly without UI stutter, supporting full copy-to-clipboard and export.

---

## 9. Scope Management: MVP vs. Post-MVP

### 9.1 In-Scope for MVP
- Single AI Employee: **The Marketing Employee**.
- Onboarding crawl (up to 10 pages) and Candidate Review Inbox.
- Business Brain with 9 structured entity types and source provenance.
- Dual-Pane Workspace (Conversation + Decision Canvas).
- 4 primary deliverables: Research Briefs, Campaign Strategies, Brand-Aligned Copy, and CSV Performance Diagnostics.
- Social Publishing to Organization Pages (LinkedIn / Facebook Pages) with immutable Approval Cards.
- Scheduled publishing with 24-hour expiration timers.
- JIT OAuth connection flow and `/settings/integrations` hub.
- Platform Admin Employee Catalog with versioned configuration revisions.
- Immutable `/audit` governance log.

### 9.2 Explicitly Out of Scope for MVP
| Excluded Capability | Rationale | Deferred To |
|---|---|---|
| **Autonomous External Posting** | Violates core safety invariant; 100% human-in-the-loop required. | Never (Core Platform Principle) |
| **Paid Ad Spend & Budget Execution** | High financial liability; requires mature attribution modeling. | Post-MVP / V2 |
| **Personal Social Profile Publishing** | Brand risk and personal account API compliance issues. | Out of Scope |
| **Visual Node-Based Workflow Canvas** | Adds complexity; users want pre-configured employees, not DIY builders. | Excluded by Design |
| **Granular Multi-Role RBAC** | All active workspace members share uniform permissions for MVP simplicity. | V1.2 |
| **Deep / Dynamic Crawling (>10 Pages)** | Unbounded crawling risks scraping irrelevant noise and heavy SPA rendering costs. | V1.2 |
| **Hard UI Deletion of Brain Items** | Deletion destroys provenance; retiring items preserves audit integrity. | Excluded by Design |
| **Additional Employee Roles (Sales, Support)** | Focused beachhead required to prove marketing depth and quality. | V2 |

---

## 10. Product Risks & Mitigation Strategies

| Risk | Impact | Probability | Mitigation Strategy |
|---|---|---|---|
| **User Review Fatigue** | Users find reviewing candidate facts or approvals burdensome and abandon the workflow. | Medium | Provide concise side-by-side diffs, highlight key changes, and enforce clean Decision-Ready previews that require $< 60$ seconds to assess. |
| **Hallucinated Company Capabilities** | The employee suggests services or facts the company does not provide. | High | Strict GraphRAG retrieval grounded solely in active Canonical Knowledge Items; provenance citations displayed on all claims. |
| **Token Expiry During Scheduled Dispatch** | A scheduled post fails at execution time because the customer's social OAuth token expired. | Medium | Automatic preflight reauthorization check when the schedule timer wakes up; immediate urgent notification sent if token is invalid. |
| **CSV Formatting Variations** | Users upload non-standard or malformed performance CSV files. | High | Strict header validation and schema normalization with clear, user-friendly error guidance indicating missing required columns. |
| **Accidental Duplicate Posts** | Network timeout causes retry of an already-posted social update. | Medium | Pre-dispatch idempotency locking and indeterminate state feed inspection before any retry attempt. |

---

## 11. Release Criteria & Quality Gates

The Marketing Employee MVP shall be approved for general release only when the following release gates are met:
1. **Deterministic Safety Gate**: 100% pass rate on boundary tests asserting zero unauthorized outbound API calls and zero unapproved knowledge promotions.
2. **Quality Gate (Blinded Human Evaluation)**: 84 representative task evaluations across 3 distinct test company profiles achieving $\ge 85\%$ acceptance by independent human reviewers.
3. **Durable Recovery Gate**: 100% recovery of in-flight tasks and scheduled dispatches across simulated worker restarts and network disconnects.
4. **Security Audit Gate**: Zero cross-tenant data leakage and zero standing admin access verified by automated boundary regression tests.

