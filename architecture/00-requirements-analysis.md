# 00 — Requirements Analysis

> **Purpose:** a complete understanding of what the docs require. **No architecture proposals.**
> **Rule:** every item cites its source. Items marked *inferred* follow from the cited text; they are not new requirements.
> **Range analysed:** `37a3aa1` (2026-10-09) → `HEAD` `99305cc` (2026-10-13). Written 2026-10-16.

### Source key

| Key | File | Lines |
|---|---|---|
| **DISC** | `project-docs/product-discovery-summary.md` | 210 |
| **DR** | `project-docs/decision-registry.md` | 99 |
| **TA** | `project-docs/technical-architecture.md` | 571 |
| **PRD** | `project-docs/prd.md` | 1033 |
| **MVP** | `project-docs/mvp-definition.md` | 313 |
| **EVAL** | `project-docs/evaluation.md` | 158 |
| **BM** | `project-docs/business-model-value-proposition.md` | 202 |
| **DECK** | `presentation/1-BusMora.html` (13 slides; citations give the file line) | 844 |
| **DRAW** | `project-draw.excalidraw` (citations give the panel title or label) | 23,802 |

Precedence as the docs state it: DR says it is "the authoritative reference for the current design" (DR line 3). PRD is "Baselined — reflects the decisions in decision-registry.md" (PRD line 6). DECK and DRAW make no claim about precedence.

---

## 0. What changed in the recent update

**Baseline assumption:** "before the update" is `37a3aa1`. The three latest commits (Oct 12–13) form one coherent update. They add `project-docs/` and the presentation, delete the three root-level spec files, and rework the drawing. The previous commit is three days earlier.

| File | Change | Commit(s) | Summary |
|---|---|---|---|
| `BUSMORA-MASTER-SPECIFICATION.md` | **Deleted** (608 lines) | dbaf3bf | Old "Master System Specification" for an **AI Employee *Platform***. It had a Platform Admin portal (`busmora.com/admin/*`), a Neo4j knowledge graph, AWS KMS, ModelGateway, LangSmith, Datadog, LinkedIn, `eu-central-1`/Frankfurt, a 10–49-employee ICP, an "84 assessed runs" evaluation corpus, and six 4-week milestones. |
| `PRD.md` | **Deleted** (311 lines) | dbaf3bf | Old platform PRD with three personas, including "Karim — The Platform Administrator", plus "Platform Administration & Employee Assembly", LinkedIn and `eu-central-1`. |
| `SPEC.md` | **Deleted** (220 lines) | dbaf3bf | Old technical spec: Neo4j, ModelGateway, LangSmith, Platform Admin, LinkedIn, Frankfurt, "Temporal-First, No Message Broker". |
| `project-docs/product-discovery-summary.md` | **New** (210) | d015ccc | Problem, segment, vision, competition, key decisions, assumptions. |
| `project-docs/technical-architecture.md` | **New** (571) | d015ccc | v2.0. Four services, PostgreSQL concept graph + Qdrant, LangGraph-on-Temporal, crawling, SSE, security, sprint plan, hardening pillars. |
| `project-docs/prd.md` | **New** (1033) | dbaf3bf | v2.0. Replaces the deleted `PRD.md`. **Marketing Employee only**, no admin persona, FR/NFR/AC. |
| `project-docs/mvp-definition.md` | **New** (313) | dbaf3bf | One 24-week, full-scope release; components; seven journeys; month plan. |
| `project-docs/decision-registry.md` | **New** (99) | dbaf3bf | D-01…D-46. |
| `project-docs/evaluation.md` | **New** (158) | dbaf3bf | Arabic rubric, Brain treatment-vs-control, execution reliability. |
| `project-docs/business-model-value-proposition.md` | **New** (202) | dbaf3bf | Value proposition, moat, Stripe tiers, cost drivers, GTM. |
| `project-draw.excalidraw` | **Heavily modified** (diff ±13,614 lines) | dbaf3bf | Live elements went from 309 to 450 and embedded images from 20 to 37. **Removed labels:** `Neo4j (Brain)`, `Agent Configuration`, `Marketplace`, `Subscriptions`, `Agents Config`, `MAIN AGENT (Employee)`, `Frontend renders token-by-token`. **Added:** the 4-service topology, the execution/visualization pipeline, "MAIN AGENT — AI Marketing Employee", the dual-memory Brain panels, four "pillars", the vector-DB decision (Qdrant), the SQL-DB decision (PostgreSQL), a PDF-parser benchmark, a project-management process (Jira + Obsidian vault), and reference screenshots. |
| `presentation/1-BusMora.html` | **New** (844) | 99305cc | 13-slide pitch deck. **Still carries the pre-update design:** Neo4j, Admin Employee Builder, LinkedIn, `eu-central-1`, ModelGateway, LangSmith, Datadog, 10–49 employees. Every one of these terms appears in the deleted files above and nowhere in `project-docs/`. |
| `skills-lock.json`, `.agents/`, `.claude/`, `.commandcode/` | Pruned (−3,594 lines, 147 files) | 99305cc | Agent-tooling cleanup. **Not product content; excluded from this analysis.** |

**Consequence:** most contradictions in §8 come from DECK and parts of DRAW reflecting the pre-update design, while `project-docs/` reflects the new one.

---

## 1. Product summary

BusMora is a multi-tenant SaaS that gives Arabic-first SMBs (5–50 employees, e-commerce and local services, Egypt and the GCC) one **AI Marketing Employee** (DISC §2–3, PRD §1).
A user signs up and enters their website URL. A bounded background crawl seeds a **Business Brain**: a PostgreSQL concept graph plus Qdrant vectors, plus a procedural "style" memory. The user reviews every candidate fact individually (PRD §5.2–5.3, DR D-20–D-24, D-43).
In a chat with a live agent canvas, a Router/Orchestrator diagnoses each request and delegates to five sub-agents: market research, competitor analysis, content writer, campaign planner, social poster (PRD §9).
The sub-agents crawl Reddit, YouTube, the Facebook Ads Library and the web. They write dialect-aware Arabic/English content with brand-template images, plan content calendars, and diagnose uploaded performance CSVs (PRD §5.7–5.12).
Outputs go to a Deliverables store. Reusable facts flow back to the Brain as candidates (PRD §5.6, §10).
Publishing to Facebook Pages, Instagram Business and X happens only after human batch approval. It goes through an MCP server, idempotently, with read-back verification (PRD §11).
It is a 4-student, 24-week graduation project meant to launch as a real product with a 1–3-business paid pilot on Stripe subscriptions (DR D-05, D-08, D-35).

---

## 2. Actors and main user journeys

### 2.1 Actors

| Actor | Role | Source |
|---|---|---|
| **Business Owner / Founder** | Onboards, manages the Brain, batch-approves, connects platforms (UC-01, 05, 11, 12) | PRD §3.1, §4 |
| **Marketing Person** (solo marketer/coordinator) | Requests research, competitor analysis, content, calendars, posting, performance diagnosis | PRD §3.1, §4 |
| *Owner and Marketing Person are the same human* (*inferred*) | Each workspace is "Single-user access (no team invitations in this release)" | PRD FR-003; DISC §6 "Workspace: Single user" |
| **AI Marketing Employee** (system) | Router, Orchestrator, five sub-agents, Knowledge Extractor | PRD §9; TA §6.2 |
| **System — Knowledge Discovery** | Extracts candidates from completed tasks (UC-06) | PRD §4 |
| **Human evaluation reviewers** (internal) | Two Arabic-native reviewers per round, a third breaks ties | EVAL §4; DR D-33 |
| **BusMora team / operators** (internal) | Pilot onboarding, evaluation rounds, observability | MVP §8 Month 6; EVAL §9 |
| **Platform Admin** (*contested*) | "Assemble the employee": instructions, sub-agents, tools, MCP, immutable revisions | DECK slide 10 (lines 663–727) only; see C-2 |
| **Anonymous prospect** (*GTM only*) | Enters a URL on the landing page for a free "Instant Brain Audit" | BM §5.1 only; see C-15 |
| **External platforms** | Meta (FB Pages, IG Business), X, crawled sources, LLM providers, Stripe | PRD §12; DR D-35 |

### 2.2 Journeys

| # | Journey | Flow (condensed) | Source |
|---|---|---|---|
| J1 | **Onboarding** (first 10 min) | Sign up (email + password) → enter URL → background crawl while exploring → notification "N items ready" → Brain page with categorized sections → approve, edit or reject each item → enter chat | MVP §6 J1; PRD §7.1 |
| J2 | **First conversation** | Employee greets proactively and flags missing Brain info (e.g. brand voice, competitors) → user picks "Define Brand Voice" → structured form (tone, style, dialect, sample text) → Brain updated | MVP §6 J2; PRD §8.2 |
| J3 | **Market research** | Request → Router: complex → Orchestrator retrieves Brain → delegates to Market Research → crawls Reddit, YouTube, web → canvas shows activity → report → Deliverables → candidates → notification | MVP §6 J3; PRD §7.2 |
| J4 | **Content with a generated visual** | "Write 5 Instagram posts…" → Brain context → Content Writer + Image Generation → caption, hashtags, visual brief, brand-template visual per post → user reviews in chat → Deliverables | MVP §6 J4 |
| J5 | **Competitor analysis** | Crawl competitor site + FB Ads Library + social presence → comparison report → candidates | MVP §6 J5; PRD FR-070–071 |
| J6 | **Campaign → approval → publishing** | Calendar request → Campaign Planner → calendar in Batch Review UI → edit, approve or reject per post → OAuth check (connect if needed, task resumes) → MCP publish → read-back → per-post result | MVP §6 J6; PRD §7.3, §11.3 |
| J7 | **Performance interpretation** | Upload CSV → validate/normalize → deterministic KPIs → diagnosis (well / under / 3 adjustments) → Deliverables + candidates | MVP §6 J7; PRD FR-095 |
| J8 | **Brain management** | View, edit, add, approve or reject nodes and edges on the graph canvas; provenance; filters | PRD UC-05, FR-021 |
| J9 | **Deliverables access** | Browse, filter by type/date, view, download (stretch goal) | PRD UC-08, FR-051 |
| J10 | **Workflow monitoring** | Live canvas during any task; reconnect replays missed events | PRD UC-07, FR-045 |

DECK slide 7 (lines 562–585) gives the same owner flow as routes: `/signup` → `/onboarding` → `/brain/candidates` → `/tasks` → SSE stream → Approval → Read-back. Setup is one-time and the rest is everyday use.

---

## 3. Functional requirements by domain

### 3.1 Account, authentication, workspace

| ID | Requirement | Source |
|---|---|---|
| ACC-1 | Register with email + password. Email verification required. A personal workspace is created automatically on registration. | PRD FR-001 |
| ACC-2 | Email/password login with Argon2id hashes. JWT access tokens + Redis-backed rotating refresh tokens. Password reset flow. | PRD FR-002; DR D-29; TA §9.1 |
| ACC-3 | One workspace per user, isolated from other users, single-user access (no invitations this release). | PRD FR-003; DISC §6 |
| ACC-4 | Navigation: Marketing Employee (chat), Brain, Deliverables, Settings (account, connected platforms). | PRD FR-004 |
| ACC-5 | Access JWT carries `user_id` and the active `workspace_id`. Every request maps to a workspace. Cross-workspace access is forbidden across relational, vector and object storage. | TA §9.1, §1.2 |

### 3.2 Onboarding and website crawl

| ID | Requirement | Source |
|---|---|---|
| ONB-1 | User enters the company URL during onboarding. The system validates that it is accessible. | PRD FR-010 |
| ONB-2 | The crawl is a background job and never blocks the user. Progress is visible (pages crawled, estimated completion). | PRD FR-010, FR-014; DECK slide 7 notes |
| ONB-3 | Crawls public pages, respects robots.txt (configurable), extracts HTML text, follows internal links to a configurable depth, and handles header/footer/nav/main structures. | PRD FR-011 |
| ONB-4 | Bounds: at most 5 minutes per website; 10 key pages by default. | PRD FR-011; DR D-18 |
| ONB-5 | An LLM extracts company description/industry, products/services (with prices if found), audience indicators, brand messaging/tone, contacts/locations, FAQ, and "other". Each item becomes a Knowledge Candidate. | PRD FR-012 |
| ONB-6 | Completion notification. The Brain page shows a badge with the pending-candidate count. | PRD FR-013 |
| ONB-7 | Failure handling: unreachable → retry 2× → notify. No useful content → inform the user and suggest manual population. Timeout at 5 min → partial results + notification. Empty items create no candidates. | PRD §13.1, §13.4 |
| ONB-8 | HTML crawl snapshots are kept for provenance. | TA §5.3 |
| ONB-9 | Candidates are visible within 5 minutes of registering. | PRD AC-01 |

### 3.3 Business Brain — model and lifecycle

| ID | Requirement | Source |
|---|---|---|
| BRN-1 | Concept nodes `brain_nodes` in **nine** categories: `company_profile`, `offering`, `audience`, `brand_rule`, `positioning`, `strategic_goal`, `channel`, `competitor`, `company_fact`. Fields: `id`, `category`, `title`, `summary`, `properties` (JSONB), `status` (`candidate`\|`approved`\|`rejected`\|`retired`), `source`, `source_detail`, provenance/freshness metadata, timestamps. | PRD FR-020; DR D-21; TA §5.1 |
| BRN-2 | Typed, directed edges `brain_edges` with a strict enum: `TARGETS`, `COMPETES_WITH`, `HAS_TONE`, `SOLVES_PAIN_POINT`, `RELATED_TO` (bidirectional). Fields: `id`, `source_node_id`, `target_node_id`, `relation_type`, `weight`, `created_at`. | PRD FR-020; TA §5.1 |
| BRN-3 | **Procedural memory** `brain_procedural_rules` holds guidelines derived from human edits, approved post styles, dialect-register calibrations, and post-task reviews. | TA §5.1; PRD §10.1; DR D-43 |
| BRN-4 | Lifecycle: candidate→approved, candidate→rejected, approved→edited, approved→retired (with an audit record). Crawl, conversation and task-result knowledge starts as `candidate`. **Manual knowledge is immediately `approved`.** | PRD FR-023, §10.2 |
| BRN-5 | Candidates are reviewed **individually**; there is no bulk approval. | PRD FR-013; DR D-24; DRAW "1. Ingestion & Extraction" |
| BRN-6 | Conflicts are surfaced to the user (notification, "ask which is correct") and never auto-resolved. | PRD FR-023, §13.4; MVP §4 |
| BRN-7 | Audit-preserving retirement; no hard delete. | DISC §6 "Lifecycle" (but see C-10) |
| BRN-8 | Classification: task-specific and temporary info is not stored. Reusable info becomes a candidate. Sensitive info (passwords, financial data, personal info) is not stored and the user is warned. | PRD §10.3 (first) |
| BRN-9 | Delegated audit log with provenance, conflict, freshness and retirement handling. | DR D-19; DRAW "Provenance-backed Audit Trail: Source URL, freshness timestamp, version retirement" |
| BRN-10 | Source-document records link canonical knowledge to raw sources in object storage. | TA §5.1 |

### 3.4 Business Brain — UI and retrieval

| ID | Requirement | Source |
|---|---|---|
| BRN-11 | React Flow graph of approved and candidate nodes, draggable, with labeled edges. | PRD FR-021; DISC §6 |
| BRN-12 | Node side panel: view, edit JSON properties, **delete** node. | PRD FR-021 |
| BRN-13 | Candidate review in categorized sections: Company Info, Products & Services, Brand & Messaging, Audience & Market, Contact & Locations, Policies & FAQ, Other. Actions: Approve, Reject, Edit→Approve, Add New. | PRD FR-013 |
| BRN-14 | Manual node creation and drawing edges between nodes. | PRD FR-021 |
| BRN-15 | Filter by category or relationship type; full-text search. | PRD FR-021 |
| BRN-16 | Provenance view: the source and raw snapshot behind each node. | PRD FR-021 |
| BRN-17 | Retrieval, stage 1: Qdrant top-k over node embeddings, filtered by `workspace_id` and `status='approved'`. Stage 2: PostgreSQL traversal of 1–2 hops over `brain_edges`. Then a structured JSON/XML context block under a token ceiling. | PRD FR-022, §10.4; TA §5.4; DR D-22 |
| BRN-18 | Procedural rules are injected into prompts alongside the semantic context. | PRD §9.1 "(Semantic + Procedural Memory)"; DRAW "3. Two-Stage GraphRAG Retrieval" |
| BRN-19 | A document-only **control route** (chunked raw sources in Qdrant, no graph) for measurement. | PRD FR-022; TA §5.4; DR D-23 |
| BRN-20 | If the context window is exceeded, truncate the oldest Brain context and log a warning. | PRD §13.2 |
| BRN-21 | Approved knowledge is used by the Employee. | PRD AC-03 |

### 3.5 Chat and clarification

| ID | Requirement | Source |
|---|---|---|
| CHT-1 | Split layout with chat on the left and canvas on the right. On mobile the canvas is hidden or toggled. | PRD FR-030 |
| CHT-2 | Message types: text, text response, structured form, task status, deliverable card (view/download), knowledge notification, error. | PRD FR-031 |
| CHT-3 | Forms support text, radio, checkbox and dropdown fields. Each field can offer "Search yourself" (the agent researches instead). Submissions arrive as structured data. | PRD FR-032; AC-11 |
| CHT-4 | Free-text clarification alongside forms. | DISC §6; DR D-15 |
| CHT-5 | Conversations persist; the user can scroll back and open earlier conversations from a list. | PRD FR-033 |
| CHT-6 | Persona: professional, proactive, transparent. Replies in the user's language and can switch mid-conversation. Uses Brain terminology. | PRD §8.1 |
| CHT-7 | Proactive behaviour: greet and flag Brain gaps in the first conversation, ask via form when info is missing, suggest follow-ups after tasks, notify about candidates. *Idle-period suggestions are marked "(Future)".* | PRD §8.2; MVP J2; AC-04 |
| CHT-8 | Politely decline non-marketing requests. Requests in other languages get an English reply noting AR/EN support. | PRD §13.4 |
| CHT-9 | Brand-voice definition form: tone, language style, dialect, sample text. | MVP J2 |

### 3.6 Orchestration and autonomy

| ID | Requirement | Source |
|---|---|---|
| ORC-1 | Router: simple → direct reply (lightweight LLM); complex → Orchestrator; ambiguous → ask before routing. | PRD §9.2; TA §6.2 |
| ORC-2 | Orchestrator: load context, **diagnose the primary commercial constraint** (discovery / positioning / conversion), find gaps, clarify, plan subtasks with dependencies, delegate, monitor and emit events, recover from errors, review quality, assemble, extract knowledge. | PRD §9.1, §9.3; TA §6.2; DR D-44 |
| ORC-3 | Anti-Goodhart guardrails: anchor execution to verified business goals, not vanity proxies. | TA §6.2; DR D-46 |
| ORC-4 | Sub-agents: Market Research, Competitor Analysis, Content Writer, Campaign Planner, Social (Media) Poster. A selection table maps task types to agents. | PRD §9.4; DISC §6; TA §6.2 |
| ORC-5 | Sub-agents never talk to each other; everything goes through the Orchestrator. Each sub-agent gets only its own tools. | PRD §9.5; TA §6.4, §13 #3 |
| ORC-6 | Autonomy. **Autonomous:** Brain queries, public crawling, agent choice and order, retries (up to 3), adding candidates, generating content. **Needs the user:** posting, changing approved knowledge, connecting platforms. **Not supported:** spending money (ads). | PRD §8.3 |
| ORC-7 | Shared workflow state is checkpointed at every transition, so tasks can pause/resume for clarification and failure recovery. | TA §6.1, §13 #1 |
| ORC-8 | Sub-agent failure → up to 3 retries → alternative source/agent → transparent notification. Time limit → partial result + notice. Unsourced claims are flagged. "Silent retry + fallback". | PRD §13.2; DISC §6 |
| ORC-9 | Tasks continue after the browser closes and results are there on return. Tasks over 10 minutes show progress and do not block the UI. | PRD §13.4; DECK slide 9 notes |
| ORC-10 | Different tasks take different paths; there is no fixed workflow. | PRD FR-046; DECK slide 10b |
| ORC-11 | Recursion depth capped; cumulative task cost tracked. | TA §13 #9; DR D-31 |
| ORC-12 | LLM output parsed against strict schemas: self-correct on error, fall back to clean text. | TA §13 #2 |

### 3.7 Market research and the crawling engine

| ID | Requirement | Source |
|---|---|---|
| RES-1 | Market Research takes topic, scope and focus. It crawls Reddit (subreddits, posts, comments), YouTube (titles, descriptions, top comments) and the web, then produces a seven-section report. | PRD FR-060, FR-061 |
| RES-2 | Every claim cites a source (URL, platform, date accessed); the user can click through to verify. | PRD FR-062 |
| RES-3 | Intent qualification: Monitor → Qualifier (scores 0–100; high intent = competitor complaints, recommendations, price questions; low intent is filtered out) → Enrichment that feeds buyer vernacular to the Content Writer. | TA §7.4; DR D-45 |
| RES-4 | Pipeline: Scrapling stealth fetch → sanitize (strip HTML/scripts/styles) → intent qualifier → clean, scored Markdown to the agent. | TA §7.1 |
| RES-5 | Data per source. Reddit: posts, comments, upvotes, subreddit info. YouTube: titles, descriptions, top comments, view counts. Ads Library: active ads, copy, creatives, advertiser. Competitor sites: content, products, pricing, messaging. General web: pages relevant to queries. | PRD §12.1 |
| RES-6 | Domain-level rate limiting and cooldown, caching, rotating proxies, a bounded headless-browser pool. When rate-limited: backoff → skip the source. On CAPTCHA: skip, use alternatives, notify. | TA §7.3; PRD §12.1, §13.1 |
| RES-7 | Cache-first: the same competitor is not re-crawled within 7 days unless requested. | BM §4.3 |
| RES-8 | Research reports use at least two real sources. | PRD AC-05 |

### 3.8 Competitor analysis

| ID | Requirement | Source |
|---|---|---|
| CMP-1 | With competitor names: crawl their websites (products, pricing, messaging, positioning) and the FB Ads Library. Without names: the Orchestrator asks or runs Market Research first. | PRD FR-070 |
| CMP-2 | Eight-section report, including a side-by-side comparison using Brain data. | PRD FR-071 |
| CMP-3 | Also covers the competitor's social media presence. | MVP J5 (not in PRD §12.1's source list) |
| CMP-4 | Key insights become Brain candidates. | MVP J5 |

### 3.9 Content and visuals

| ID | Requirement | Source |
|---|---|---|
| CNT-1 | Inputs: content type (social post / article / ad copy), platform (IG / FB / X / general), brief, Brain context, research context. | PRD FR-080 |
| CNT-2 | Output: caption, hashtags, visual description or brand-template visual, platform formatting (X character limits, emoji). | PRD FR-081 |
| CNT-3 | **Must** use the Brain's brand voice. If none is defined, clarify through the Orchestrator. | PRD FR-082; AC-07 |
| CNT-4 | Arabic, English or both, generated natively (not translated), in the dialect set in the Brain. Mixed language is supported ("keep product names in English"). | PRD FR-083; EVAL §3 |
| CNT-5 | Drafts with variations. | MVP §3 |
| CNT-6 | Brand-template composite = generated imagery + deterministic overlay (logo, colors, typography). Cached by template and dimension. Every visual is human-approved before publishing. | PRD FR-085; DR D-14 |
| CNT-7 | High-intent buyer vernacular from research flows into copy. | TA §7.4 |

### 3.10 Campaign planning

| ID | Requirement | Source |
|---|---|---|
| CAL-1 | Content Calendar built from Brain knowledge, research results and user parameters. | PRD FR-090 |
| CAL-2 | Per week, per date and platform: post, hashtags, visual. | PRD FR-091 |
| CAL-3 | Parameters: period (1 week / 2 weeks / 1 month), platforms, frequency per platform, themes/products, special dates. | PRD FR-092 |
| CAL-4 | The calendar is structured and editable, and opens in the Batch Review UI. | MVP §2, J6; AC-14 |
| CAL-5 | The Content Writer may be used for each post. | PRD §9.4 |

### 3.11 Performance interpretation

| ID | Requirement | Source |
|---|---|---|
| PRF-1 | Upload a CSV. Validate and normalize its schema; give a clear error on unrecognized headers. | PRD FR-095 |
| PRF-2 | Compute deterministic KPIs (CTR, conversion rate, CPC changes). | PRD FR-095; MVP J7 |
| PRF-3 | The **Orchestrator** produces the diagnosis: what worked, what underperformed, three prioritized adjustments. | PRD FR-095; MVP J7 |
| PRF-4 | Saved as a deliverable; reusable facts become candidates. | PRD FR-095; AC-20 |
| PRF-5 | Uploaded CSVs are stored in object storage. | TA §5.3 |

### 3.12 Approval, scheduling and publishing

| ID | Requirement | Source |
|---|---|---|
| PUB-1 | Approval is required **only** to publish a post (Batch Review) and to connect a platform (OAuth consent). Research, content and calendar generation need none. | PRD §11.1 |
| PUB-2 | State machine: `draft → pending_approval → approved \| rejected → scheduled → publishing → published \| failed`. Every transition is audited; every attempt carries an idempotency key. | PRD §11.2; DR D-16 |
| PUB-3 | Batch Review UI: grid/list with platform icon, date/time, preview, status. Per post: approve, reject, edit→approve. Bulk: approve all remaining, reject all remaining. | PRD §11.3; UC-11; AC-16 |
| PUB-4 | Approved posts are queued. Durable workflows release each one at its scheduled time. Publishing goes through MCP tools idempotently, with read-back verification and a per-post result. | PRD §11.3, FR-100; AC-17 |
| PUB-5 | MCP tools: `publish_facebook(page_id, message, image_url?) → {post_id,url}`, `publish_instagram(account_id, caption, image_url) → {media_id,url}`, `publish_x(text, media_ids?) → {tweet_id,url}`. | PRD FR-102 |
| PUB-6 | Preconditions: the platform is connected with valid tokens and the post is `approved`. Expired token → silent refresh → otherwise notify to reconnect. | PRD FR-103 |
| PUB-7 | Errors: API error → retry 2× → specific notice. Platform rejection → show the reason and suggest a fix. Partial batch → per-post status. Rate limit → queue and retry after cooldown. Duplicate request → blocked by idempotency key. | PRD §13.3 |
| PUB-8 | Failed posts can be retried without creating duplicates. | PRD AC-18 |
| PUB-9 | Immutable snapshots of proposed actions, plus idempotent dispatch records. | TA §5.1 |
| PUB-10 | Pre-dispatch execution locks. Timers for deferred scheduling and **approval expiration**. Signals `approve`, `revise`, `cancel`, `oauth_connected`. | TA §4.2 |
| PUB-11 | Generated images are served via signed public URLs for platform publishing. | TA §5.3 |
| PUB-12 | The approval card shows the target, schedule, expiry ("in 24h") and the "EXACT TEXT THAT WILL BE POSTED". | DECK slide 8 (lines 619–623) only |

### 3.13 Platform connections (OAuth)

| ID | Requirement | Source |
|---|---|---|
| OAU-1 | Connect Facebook, Instagram and X via OAuth **lazily**, only when posting is requested. The OAuth link appears in chat, then the agent confirms. | PRD UC-12, §7.3; AC-15, AC-19 |
| OAU-2 | Tokens are KMS envelope-encrypted with context `{workspace_id, integration_id, credential_version}`. Plaintext exists only in memory during calls and is never logged or traced. | PRD §12.3; TA §9.3; DR D-30 |
| OAU-3 | Automatic refresh; on failure, notify to reconnect. Token status shows in Settings. The user can disconnect at any time. | PRD §12.3 |
| OAU-4 | Token exchange, encrypted storage, refresh, preflight checks. | MVP §3 |
| OAU-5 | OAuth changes are recorded in the audit log. | TA §5.1 |

### 3.14 Workflow visualization canvas

| ID | Requirement | Source |
|---|---|---|
| CNV-1 | A node graph per task: an Orchestrator hub plus Market Research, Competitor Analysis, Content Writer, Campaign Planner and Social Media Poster nodes. | PRD FR-040, FR-041 |
| CNV-2 | Node states: idle (gray), active (animated), completed (green check), error (red, only when user-visible). | PRD FR-042 |
| CNV-3 | Animated directional edges ("delegated task" / "returned results") appear as the workflow progresses. | PRD FR-043 |
| CNV-4 | Tool use shows an abstracted label under the active node (e.g. "Searching Reddit"), not the internal tool name. | PRD FR-044 |
| CNV-5 | SSE with `Last-Event-ID` replay from a Redis Stream buffer, without interrupting the run. Events: `agent_started{agent_id,agent_name,task_summary}`, `agent_tool_used{agent_id,tool_label}`, `agent_completed{agent_id,result_summary}`, `agent_error{agent_id,error_summary}`, `flow_connection{from_agent,to_agent,direction}`. | PRD FR-045; TA §8; DR D-34; AC-08 |
| CNV-6 | Dual-pane chat + decision canvas, animated. | DISC §6; DR D-17 |

### 3.15 Deliverables

| ID | Requirement | Source |
|---|---|---|
| DLV-1 | Every output persists with `id`, `title`, `type` (`research_report`\|`competitor_analysis`\|`content_posts`\|`content_calendar`\|`performance_diagnosis`\|`general`), `content` (Markdown or structured), `task_id`, `created_at`. | PRD FR-050; AC-09 |
| DLV-2 | Browse; filter by type and date; view in full; download as PDF/Markdown (**stretch goal**). | PRD FR-051 |
| DLV-3 | Five named artifacts: Research Brief, Campaign/Content Plan, Content Package, Performance Diagnosis, Approval Card. | DR D-12; DRAW "5 Responsibilities & Deliverables" |

### 3.16 Knowledge extraction (post-task)

| ID | Requirement | Source |
|---|---|---|
| KEX-1 | After each task, find info that is factual, about the company/market/competitors, and reusable. Turn it into candidates with a category, the source task ID and a confidence indicator, then notify. | PRD §10.3 (second); UC-06; AC-10 |
| KEX-2 | Sources: conversations (semantic or procedural), task results and **user edit diffs** (procedural). | PRD §10.2 |
| KEX-3 | The Knowledge Extractor node runs after Assembly. | TA §6.3 |
| KEX-4 | Four ways in: onboarding crawl, manual entry, the agent itself (missing general info, valuable task output), and generic user feedback ("make the tone calmer" → a BrandRule candidate). | DECK slide 5b (lines 503–540) |
| KEX-5 | "Validated feedback becomes task history, Agent memory, or Business Context depending on its scope and repetition." | DRAW "How BusMora Works" example |

### 3.17 Notifications (cross-cutting, as stated)

Crawl complete (FR-013). Pending-candidate badge (FR-013, FR-021). "N new knowledge items" chat message (FR-031). Error message after retries fail (FR-031, §13.2). Token refresh failure (FR-103, §12.3). Brain conflict (§13.4). Per-post publish results (§11.3). Partial crawl or partial task result (§13.1, §13.2). **No delivery channel is named; every surface described is in-app (chat messages, badges).**

### 3.18 Audit and governance

| ID | Requirement | Source |
|---|---|---|
| AUD-1 | An append-only audit log covering knowledge promotions, edits, approvals, retirements, publish dispatches and OAuth changes. | PRD FR-023, §11.2; AC-21; TA §5.1 |

### 3.19 Billing and plans

| ID | Requirement | Source |
|---|---|---|
| BIL-1 | Tiered monthly/annual Stripe subscriptions per workspace, plus a flat-priced paid pilot for 1–3 businesses. | DR D-35; BM §4.1 |
| BIL-2 | Tiers: **Starter** (Brain crawling + Content Writer; 5 research tasks and 30 posts per month). **Growth** (full suite, 3 channels; 25 research tasks and 120 scheduled posts per month; continuous Brain updates). **Scale** (high concurrency, expedited crawling, priority LLM routing, unlimited deliverables, **daily competitor monitoring**). | BM §4.2 |
| BIL-3 | X publishing and heavy image generation are metered inside each tier. | BM §4.2 |
| BIL-4 | "Billing" belongs to the Platform Service. | DRAW "System Architecture (BusMora 4-Service Topology)" |
| — | **No FR or AC in PRD/MVP covers billing, quotas or plan gating** (see C-14). | PRD §5, §14; MVP §3 |

### 3.20 Evaluation and instrumentation (product-facing)

| ID | Requirement | Source |
|---|---|---|
| EVL-1 | Record clarification and correction events (conversation and task records). | EVAL §8 |
| EVL-2 | Record approvals, edits and publishes, and batch-review session timestamps (review time per batch). | EVAL §6, §8 |
| EVL-3 | Langfuse traces: tokens, latency, model and **prompt version**; per-task cost via LiteLLM. | EVAL §8; PRD NFR-005 |
| EVL-4 | Run the same scenario in treatment (graph) and control (document-only) configurations with the same model and prompts; scoring is blind. | EVAL §5; DR D-38 |
| EVL-5 | A fixed scenario set with **seeded workspaces**. | EVAL §3 |
| EVL-6 | Pilot instrumentation: usage, acceptance, time saved. | BM §5.4; DISC §7; PRD §15.1 |

### 3.21 Items stated only in DECK/DRAW/BM (contested or unique)

| Item | Source | Status |
|---|---|---|
| Admin "Employee Builder": identity and system prompt, guardrails, pick sub-agents/tools/MCP servers, "Publish Revision" → "rev-2 · Deployed & Immutable"; the admin has no customer-data access | DECK slide 10 | Conflicts with DR D-01/D-03 (C-2) |
| LinkedIn post drafting and publishing | DECK slides 8, 10 | Conflicts with PRD §16 (C-3) |
| "Instant Brain Audit" lead magnet: anonymous URL → in **90 s** a brand summary, top 3 competitor blind spots and 3 post ideas | BM §5.1 | Not in PRD/MVP (C-15) |
| Daily competitor monitoring (Scale tier) | BM §4.2 | No FR (C-14) |
| "Marketing CSVs & Brand Asset Ingestion"; "The business provides its information, preferences, resources and documents" | DRAW | Conflicts with "Document upload: Post-MVP" (C-12) |
| "After approval, the Agent may publish, **send**, or store the result according to configured permissions" | DRAW "How BusMora Works" example | "Send" and "configured permissions" appear nowhere else |

---

## 4. Non-functional requirements

| Area | Requirement | Stated / Inferred | Source / basis |
|---|---|---|---|
| **Latency** | Website crawl ≤ 5 min; simple chat reply ≤ 3 s; task-start acknowledgement ≤ 2 s; research task ≤ 5 min; content ≤ 30 s per piece; canvas update ≤ 500 ms after the backend event | Stated | PRD NFR-001 |
| Latency | Candidates visible within 5 min of signup; first useful output within 15 min of signup | Stated | PRD AC-01, §15.2; MVP §7 |
| Latency | Human batch review of a weekly campaign ≤ 15 min | Stated | EVAL §6, §10; DR D-10b |
| Latency | Instant Brain Audit teaser in 90 s | Stated (GTM only) | BM §5.1 |
| Latency | Research runs take "tens of seconds to minutes"; tasks > 10 min must show progress | Stated | TA §8; PRD §13.4 |
| **Scale** | ≥ 100 concurrent workspaces; crawl jobs queued and rate-limited; agent tasks queued and run asynchronously via durable workflows | Stated | PRD NFR-004 |
| Scale | Launch load is 1–3 paid pilot businesses | Stated | DR D-08; PRD §2.1 |
| Scale | Per-tier volumes: up to 25 research tasks and 120 scheduled posts/month (Growth); "high concurrency" and daily competitor monitoring (Scale) | Stated (BM only) | BM §4.2 |
| Scale | Instagram cap of 100 posts per 24 h per account | Stated | PRD §12.2 |
| Scale | Durable waits can last up to ~1 month (calendar periods up to 1 month, released at scheduled times) | *Inferred* | PRD FR-092 + §11.3 step 4; TA §11.2 "wait durably… without consuming compute" |
| **Real-time** | SSE + `Last-Event-ID` replay via Redis Streams; incrementing event IDs; AI service holds no client state | Stated | DR D-34; TA §8.2; PRD FR-045 |
| **Availability / reliability** | Uptime ≥ 99% excluding planned maintenance | Stated | PRD NFR-002 |
| Reliability | Task completion ≥ 85%; publish success with read-back ≥ 95% | Stated | PRD NFR-002, §15.2; EVAL §10 |
| Reliability | **Zero data loss** for Brain data and deliverables | Stated | PRD NFR-002 |
| Reliability | Tasks survive service restarts (resume from the last successful activity) and browser closure | Stated | TA §11.2; PRD §13.4 |
| Reliability | Retry policy: tool calls/sub-agents up to 3; site unreachable 2×; platform API 2×; exponential backoff; deterministic timeouts | Stated | PRD §8.3, §13; TA §13 #4 |
| Reliability | 100% duplicate-free publishing (idempotency keys + unique execution constraints) | Stated | EVAL §10; TA §13 #5; PRD FR-103 |
| Reliability | In-flight tasks are unaffected by later config edits (frozen config payload); prompts are versioned with backward compatibility for in-flight runs | Stated | TA §11.2, §13 #10 |
| **Security** | Argon2id; short-lived JWT; random refresh tokens in Redis, rotated on use | Stated | TA §9.1; DR D-29 |
| Security | Tenant isolation on every query and vector search (`workspace_id`), and across object storage | Stated | TA §1.2, §5.2, §9.1; PRD NFR-003; AC-13 |
| Security | KMS envelope encryption for OAuth tokens; secrets in a vault, never in code; plaintext never logged | Stated | PRD NFR-003; TA §9.3; DR D-30 |
| Security | Prompt-injection defense: sanitize, quarantine inside data boundaries, guardrail instructions | Stated | TA §9.2; PRD NFR-003 |
| Security | SSRF protection (target domains checked against private IP ranges) | Stated | TA §9.3, §13 #11 |
| Security | Sanitize user input; rate-limit public endpoints; scope tools per sub-agent | Stated | PRD NFR-003; TA §13 #3, #8 |
| Security | Admin role has no access to customer data | Stated (DECK only) | DECK slide 10 notes (line 664) |
| **Privacy** | Passwords, financial data and personal info are never stored in the Brain; the user is warned | Stated | PRD §10.3 |
| Privacy | Hosting region matched to the pilot (MENA) "to support data-residency expectations" | Stated | DR D-28; TA §10.2 |
| Privacy | No retention, export or deletion policy (user, workspace, crawl snapshots, scraped third-party content) is stated; "no hard delete" + "append-only" makes this a design constraint | *Inferred gap* | DISC §6; PRD FR-023 |
| **Payments** | Stripe subscriptions per workspace; flat pilot price; tiers derived from measured costs; X/image metering | Stated | DR D-35, D-36; BM §4 |
| Payments | The product never spends the customer's money (no ad management) | Stated | PRD §8.3, §16 |
| **Cost** | ≤ $0.15–$0.40 per task; ≤ $0.05 per search; image cost tracked per dimension; cost ceilings and token budgets; per-workspace cost tracking | Stated | BM §4.3; DR D-31; TA §13 #9; PRD NFR-005 |
| Cost | X: $0.015 per plain post, $0.20 per post with a URL | Stated | PRD §12.2 |
| **Offline** | Offline mode: "Not planned — SaaS requires connectivity" | Stated | PRD §16 |
| **Mobile** | Responsive web; canvas hidden or toggled on mobile; native app post-MVP | Stated | PRD FR-030, §16; MVP §4 |
| **i18n / RTL** | Arabic (RTL) and English (LTR) UI; native generation in both; date/time follow user locale | Stated | PRD NFR-006; DR D-07; AC-12 |
| i18n | Dialects: Egyptian conversational, Gulf lifestyle, MSA (+ Levantine in some files) | Stated (inconsistent, C-13) | DISC §3; MVP J2; EVAL §4 |
| i18n | Time-zone handling for scheduled posts across Egypt and the GCC is not specified | *Inferred gap* | PRD FR-091–092, §11.3 |
| **Observability** | Log all agent actions, tool calls and errors; monitor task durations, success and error rates; Langfuse LLM tracing; Temporal UI | Stated | PRD NFR-005; TA §10.1; DR D-33 |
| **Auditability** | Append-only audit trail; provenance on every node | Stated | PRD FR-023; AC-21 |
| **Quality** | Arabic rubric: all dimensions ≥ 4, none < 3; ≥ 75% (or 70%, C-20) accepted with zero or minor edits; Brain ≥ 50% fewer clarifications and corrections | Stated | EVAL §10; PRD §15.2 |
| **Crawl politeness** | robots.txt respected "(configurable)"; domain cooldowns; bounded browser pool; 10-page onboarding cap | Stated | PRD FR-011; TA §7.3; DR D-18 |
| **Delivery constraints** | 4 students, 24 weeks, vertical slices; API contracts first (OpenAPI + TS interfaces); Docker Compose locally | Stated | DR D-05; TA §12.1 |

---

## 5. Decisions already made

### 5.1 Decision registry (DR) — all entries

| ID | Decision | ID | Decision |
|---|---|---|---|
| D-01 | Focused Marketing Employee, not a multi-employee platform | D-24 | Individual-only candidate review |
| D-02 | Domain-neutral foundations (Employee, Capability, Workflow, Integration, Task Record) | D-25 | Brain hypothesis: ≥ 50% fewer clarifications and corrections |
| D-03 | Extensibility proven by a **paper exercise**; no admin or marketplace machinery | D-26 | **Four services**: `busmora-web` (Next.js), `busmora-api` (NestJS), `busmora-workers` (Temporal), `busmora-ai` (FastAPI) |
| D-04 | One release, full scope | D-27 | Temporal Cloud |
| D-05 | 4 students, 24 weeks | D-28 | Managed/hybrid hosting; MENA region |
| D-06 | Arabic-first SMBs, 5–50 employees | D-29 | Custom Argon2id auth, JWT + Redis refresh rotation |
| D-07 | Full RTL AR/EN UI; native Arabic generation | D-30 | Cloud KMS envelope encryption for OAuth |
| D-08 | Paid pilot with 1–3 businesses | D-31 | LiteLLM gateway, Arabic bake-off, cost ceilings |
| D-09 | Customer discovery runs alongside the pilot, plus social sentiment mining | D-32 | Custom Scrapling crawlers + proxy and headless-browser pools |
| D-10 | Positioning: Autonomous Execution Layer | D-33 | Langfuse + two human reviewers (third breaks ties) |
| D-10b | 15-minute batch review | D-34 | SSE + `Last-Event-ID` via Redis Streams |
| D-11 | Five responsibility areas | D-35 | Stripe tiered subscriptions + flat pilot |
| D-12 | Five artifacts | D-36 | Tiers re-derived from real costs |
| D-13 | FB Pages, IG Business, X with Temporal scheduling; no LinkedIn or TikTok | D-37 | Human-conducted evaluation, fixed scenarios |
| D-14 | IG images = brand-template composites | D-38 | Blind treatment-vs-control |
| D-15 | Forms + free text | D-43 | Dual memory: semantic vs procedural |
| D-16 | Simplified approval state machine + idempotency + read-back | D-44 | "Gen-3": diagnose before allocating |
| D-17 | Animated React Flow canvas, dual pane | D-45 | Intent qualification pipeline for social mining |
| D-18 | 10-page onboarding crawl | D-46 | Anti-Goodhart governance via batch approval |
| D-19 | Delegated audit log (provenance, conflict, freshness, retirement) | | |
| D-20 | PostgreSQL concept graph + Qdrant; **no separate graph DB** | | |
| D-21 | Nine canonical entity types | | |
| D-22 | Vector seed + 1–2 hop expansion | | |
| D-23 | Document-only control route | | |

**Deferred, with defaults:** D-39 embedding model (OpenAI `text-embedding-3-small`-class, Sprint 1). D-40 image model (hosted text-to-image, Sprint 1). D-41 object storage (S3-compatible R2/MinIO, Sprint 1). D-42 rotating residential proxy vendor (Sprint 3).

### 5.2 Decisions in TA that are not in DR

| Area | Decision | Source |
|---|---|---|
| Frontend | Next.js 14+ App Router, TypeScript, React Flow (canvas + Brain graph), Tailwind/shadcn, React Query/SWR, SSE client with Last-Event-ID | TA §3.1 |
| Platform API | NestJS 10+, Passport + JWT, **Drizzle ORM**, Redis Streams subscriber for SSE, Temporal TypeScript SDK (workflow client), ioredis, class-validator + Swagger | TA §3.1 |
| Worker/AI | Python 3.11+, FastAPI/Uvicorn, LangGraph, LiteLLM, Scrapling, Temporal Python SDK (activity worker), Qdrant client, SQLAlchemy/asyncpg, Pydantic v2, MCP SDK | TA §3.1 |
| Data/infra | PostgreSQL 16+, Qdrant, Redis 7+, Temporal Cloud, Cloud KMS, S3-compatible storage, Docker Compose for local dev | TA §3.1 |
| LLM tiers | "Light" for router, extraction, summarization and candidates; "heavy" for planning, writing, analysis and synthesis; primary heavy model picked by Arabic bake-off, with a fallback provider | TA §3.2 |
| Boundary | Temporal owns the durable business workflow and approval signals; LangGraph owns AI reasoning; `busmora-ai` runs as a Temporal **activity worker** | TA §1.2, §11.2 |
| Workflows | `TaskExecutionWorkflow`, `OnboardingWorkflow`; signals `approve`, `revise`, `cancel`, `oauth_connected`; timers for deferred scheduling and approval expiration; pre-dispatch locks | TA §4.2 |
| MCP server | FastAPI (Python) component for social publishing tools | TA §2 |
| Storage layout | `s3://busmora-sources/{workspace_id}/{source_type}/{timestamp}_{filename}` | TA §5.3 |
| Tenancy | Qdrant payload filtering + an index on `workspace_id` and `status`; PostgreSQL foreign keys | TA §1.2, §5.2 |
| Agent graph | START → Router → (simple) Assembly, or (complex) Orchestrator ⇄ Clarification / five sub-agents → Assembly → Knowledge Extractor → END | TA §6.3 |
| Repo | Monorepo `busmora/` with `frontend/`, `platform/`, `workers/`, `ai-service/`, `docs/`, `scripts/`, `docker-compose.yml` | TA §12.3 |
| Process | Weekly sprint planning, API contracts first, feature branches + PR review | TA §12.1 |
| Hardening | Twelve engineering pillars (state, schema validation, tenant/tool RBAC, retries, idempotency, tracing, evals, rate limits, cost ceilings, versioning, anti-injection, pooling) | TA §13 |

### 5.3 Decisions recorded only in DRAW or DECK

| Decision | Source |
|---|---|
| **Qdrant** over Pinecone ("Not Open Source", "Provider Hosted", "EXCLUDED (high cost)") and pgvector ("struggle in heavy meta data filtering (real blocker)"). Reasons: vector search is core, the AI service is separate, heavy metadata filtering. Margin note: "---we may consider chroma". | DRAW "Vector db decisioning" |
| **PostgreSQL** over MySQL/MariaDB: relational model, JSONB "for flexible and queryable page content", extensibility, and "Fits our NestJS + **Prisma** stack" | DRAW "SQL DATABASE OPTIONS" (5 embedded images) |
| No Kafka/RabbitMQ: "Temporal covers distribution and timers" | DECK slide 11 notes (line 753) |
| Team process: Jira for tasks; an Obsidian vault in a separate synced repo; a "migrate-outdated-code" skill as a second review; PR flow; bugs fixed in-PR or ticketed | DRAW "Project management" / "Task life cycle" (process, not product) |
| PDF parser benchmark (LlamaParse, PyMuPDF4LLM, LiteParse, Docling, OpenDataLoader, PDFMux) on Arabic, text, tables, images, layout and speed. **No choice recorded.** | DRAW "Parse Phase Benchmark" |

---

## 6. MVP vs later phases

The plan has **no internal phases**: "One release, full scope" (DR D-04; MVP §1; PRD §1.5). Everything below "In this release" ships within 24 weeks.

| In this release (MVP §3 components) | Later / not planned | Source for "later" |
|---|---|---|
| SaaS foundation (auth, workspace, navigation, tenancy) | Other AI Employees (Sales, Support, Operations): a "future controlled release"; this release only does the paper design exercise | MVP §4, §9; PRD §16; DR D-03 |
| Website crawler → candidates | LinkedIn posting | MVP §4; PRD §16 |
| Business Brain (graph, retrieval, lifecycle, React Flow, provenance, conflict and freshness handling, audit) | TikTok posting | MVP §4; PRD §16 |
| Chat with forms, free text, "Search yourself", history | Paid ad management (Meta Ads, Google Ads) | MVP §4; PRD §16 |
| Workflow canvas via SSE | Automatic performance-analytics APIs (CSV covers it) | MVP §4; PRD §16 |
| Router, Orchestrator | Multi-user workspaces | MVP §4; PRD §16 |
| Market Research, Competitor Analysis, Content Writer, Campaign Planner, Social Poster agents | Document upload and extraction | MVP §4; PRD §16 |
| Image generation (brand template) | Advanced Brain governance automation (auto-resolving conflicts) | MVP §4; PRD §16 |
| Performance CSV interpretation | Native mobile app | MVP §4; PRD §16 |
| OAuth (Meta + X), MCP server, Batch Approval UI, scheduled publishing | Offline mode, white-labeling (**not planned**) | PRD §16 |
| Deliverables system | Idle-period proactive suggestions ("Future") | PRD §8.2 |
| Knowledge extraction | Deliverable PDF/MD download is a "stretch goal" | PRD FR-051 |
| Bilingual foundation (full RTL) | Wider tier rollout; final prices confirmed after the pilot | BM §4.1–4.2 |
| Evaluation & observability | | |

**Month plan (MVP §8):** M1 foundations + walking skeleton + discovery interviews. M2 Brain, GraphRAG, crawler, candidate UI, extensibility paper exercise. M3 LangGraph, Router, Orchestrator, forms, Market Research, Content Writer, image gen, deliverables. M4 Competitor, Campaign Planner, CSV, full canvas. M5 approvals, OAuth, MCP, scheduling, read-back, knowledge extraction, evaluation. M6 RTL polish, Arabic evaluation, pilot, hardening. TA §12.2 has a **different** sprint mapping (C-19).

---

## 7. External integrations

| Integration | Purpose | Mechanism / constraint as stated | Source | Status |
|---|---|---|---|---|
| **Meta Graph API v21+** (Facebook Pages) | Publish posts | OAuth 2.0 Page Access Token; "Requires Business Verification + App Review for production"; no API charge | PRD FR-101, §12.2; BM §4.2 | Decided |
| **Instagram Content Publishing API** | Publish IG posts (image required) | OAuth via Meta; 100 posts/24 h; two-step container model; no built-in scheduling | PRD FR-101–102, §12.2 | Decided |
| **X API v2** | Publish posts | OAuth 2.0; pay-per-use $0.015 plain / $0.20 with URL | PRD FR-101, §12.2 | Decided |
| **Reddit** | Sentiment, pain points | Custom Scrapling crawler (not an API) | PRD §12.1; TA §7.2 | Decided |
| **YouTube** | Audience discussions | Custom crawler | PRD §12.1 | Decided |
| **Facebook Ads Library** | Competitor ads | Custom crawler | PRD §12.1 | Decided |
| **Competitor sites / general web** | Messaging, pricing, research | Custom crawler; "Page content relevant to search queries" — **how URLs are discovered (search provider?) is not stated** | PRD §12.1 | Gap (Q12) |
| **Rotating residential proxy** | Platform scraping | Managed vendor | DR D-42 | Deferred (Sprint 3) |
| **LLM providers** via LiteLLM | Light and heavy models | Arabic bake-off; fallback provider; cost tracking | DR D-31; TA §3.2 | Provider deferred |
| **Embedding model** | Brain + control-route vectors | Default `text-embedding-3-small`-class | DR D-39 | Deferred (Sprint 1) |
| **Image generation** | Brand-template backgrounds | Hosted text-to-image | DR D-40 | Deferred (Sprint 1) |
| **Object storage** | Snapshots, CSVs, images (signed public URLs) | S3-compatible (R2/MinIO default) | DR D-41; TA §5.3 | Deferred (Sprint 1) |
| **Temporal Cloud** | Durable workflows | Managed | DR D-27 | Decided |
| **Cloud KMS** | Credential encryption | Envelope encryption | DR D-30 | Decided (provider unnamed; DECK says AWS KMS) |
| **Langfuse** | LLM tracing and cost | — | DR D-33 | Decided (DECK says LangSmith, C-6) |
| **Stripe** | Subscriptions | Tiered per workspace + flat pilot | DR D-35 | Decided |
| Managed PostgreSQL / Qdrant / Redis | Data plane | Managed services | TA §10.2 | Decided |
| **Email delivery** | Verification and password reset | *Inferred* from FR-001/FR-002; no provider named | PRD FR-001–002 | Gap |
| **MCP** (Model Context Protocol) | Publishing tool interface | MCP server → official platform APIs | DISC §6; PRD FR-102 | Decided |

---

## 8. Contradictions between files

Ordered by architectural impact. Quotes are verbatim; Arabic speaker notes are followed by a translation.

### High impact

**C-1 — Graph store: PostgreSQL vs Neo4j**
- DR D-20: "**PostgreSQL relational concept graph** (`brain_nodes` / `brain_edges`) + **Qdrant** vectors | One source of truth, no separate graph database to operate."
- DECK slide 5 (line 474): `'STAGE 2', 'Neo4j · expand 1-2 hops'`. Slide 9 data plane (line 653): `['Neo4j', 'Knowledge graph']`. Slide 11 (line 760): `['Neo4j', 'N']`. Slide 5 notes: "التانية في Neo4j بنوسّع حواليهم خطوة أو اتنين" (*"the second stage, in Neo4j, expands one or two hops around them"*).
- TA §5.4, PRD FR-022 and DRAW all use the PostgreSQL traversal.

**C-2 — Product shape: one Marketing Employee vs an AI-employee platform with an admin builder**
- DR D-01: "Build a focused **Marketing Employee**, not a generic multi-employee platform". D-03: "Validates the seam without building admin/marketplace machinery or a second Employee."
- DECK slide 1 (lines 379–382): "AI Employee Platform", "AI employees for startups and growing businesses". Slide 10 (lines 666–721): "Admin dashboard", "Assemble the employee: instructions, sub-agents, tools & MCP", `busmora.com/admin/employees/builder`, "Publish Revision", "rev-2 · Deployed & Immutable". DECK TEAM (line 296): "Admin, Approvals & KMS".
- DRAW "How BusMora Works": a Specialized Agent has "its defined role, skills, tools, MCPS, workflows, Permissions and guardrails".

**C-3 — LinkedIn**
- PRD §16: "LinkedIn posting | Organization-page API approval is selective and often denied | Post-MVP". DR D-13: "avoids restricted channels (LinkedIn, TikTok)".
- DECK slide 8 (lines 597, 610, 621): "Draft a LinkedIn post about our new service", "LinkedIn post for Acme Corp's page", "Acme Corp · LinkedIn Page". Slide 10 (line 716): "social_publishing:v1 (LinkedIn OAuth)".

**C-4 — Hosting region**
- DR D-28: "**Managed/hybrid hosting**, region matched to the pilot (MENA)". TA §10.2: "Region: matched to the pilot geography (MENA) to keep latency low and support data-residency expectations".
- DECK slide 9 (line 652): "eu-central-1 · Frankfurt". Slide 11 (line 761): `['eu-central-1', 'E']`.

**C-5 — Who publishes: an LLM sub-agent or a durable worker?**
- TA §6.3: `Orchestrator -->|"posting needed"| SocialPoster["Social Poster"]` (a LangGraph node inside a task run). PRD §9.1: "[Social Media Poster Agent] ← MCP Server tools". PRD FR-100: the poster "Receives approved posts from the Batch Approval workflow."
- TA §4.2 (Worker Service): "Social Publishing MCP Adapter: dispatch and read-back verification". PRD §11.3: "Durable workflows release each post at its scheduled time." TA §2 lists "MCP Server | FastAPI (Python)" as a component, but D-26 counts **four** services.

**C-6 — LLM gateway and observability vendors**
- DR D-31: "**LiteLLM** gateway". D-33: "**Langfuse**". TA §3.1 matches.
- DECK slide 11 (lines 759, 761): `['ModelGateway', 'G']`, `['LangSmith', 'S']`, `['Datadog APM', 'D']`.

**C-7 — Sub-agent roster**
- DISC §6 / TA §6.2 / PRD §9.4: "Market Research, Competitor Analysis, Content Writer, Campaign Planner, Social Poster". DRAW: "5 Sub-Agents (Research, Competitor, Copy, Campaign, Poster)".
- DECK slide 10 (lines 697–702): "Sub-agents (4 assigned)": `ResearchSubAgent`, `StrategySubAgent`, `ContentSubAgent`, `PerformanceSubAgent`. Its tools are "Web Search", "GraphRAG Query", "CSV Analytics Engine".
- PRD FR-095 says the **Orchestrator**, not a sub-agent, produces the performance diagnosis.

**C-8 — Service-to-service communication**
- TA §1.2: "NestJS owns the user-facing API; FastAPI owns AI workloads. Communication via Temporal activities." TA §11.2: "`busmora-ai` acts as a Temporal activity worker". DRAW: "busmora-ai (Temporal / Activity)".
- DECK slide 9 (line 642): arrow labels `['REST + SSE', 'Temporal', 'REST']`. The workers→ai hop is labeled **REST**.

### Medium impact

**C-9 — Manual entry and the review gate**
- PRD FR-023: "Manual knowledge is immediately `approved`." PRD §10.2: "Manual Entry | … | `approved` | No — user's own input is trusted".
- DECK slide 5b (lines 509, 538): "Manual — The user adds or edits Knowledge Items directly", captioned "All four sources pass through the same gate". Notes: "كل المصادر دي بتعدّي على نفس البوابة: Candidate ثم مراجعة بشرية ثم Active" (*"all these sources pass the same gate: Candidate, then human review, then Active"*).

**C-10 — Delete vs retire**
- DISC §6: "Lifecycle | Candidate → Approved → Rejected (audit-preserving retirement, no hard delete)". DR D-19: "retirement handling".
- PRD FR-021: "allowing the user to view, edit JSON properties, or **delete** the node."
- Status names differ too. PRD FR-020 uses `approved`/`retired`; DECK slide 4 (line 449) uses "Active · Knowledge Item" and slide 5 (line 476) uses `status = active`.

**C-11 — Brain taxonomy is internally inconsistent**
- PRD FR-020 / DR D-21: exactly **nine** canonical node types. Yet FR-020 defines edges to types outside the nine: "`HAS_TONE`: `[Brand Rule Node] ──► [Tone Node]`" and "`SOLVES_PAIN_POINT`: `[Offering Node] ──► [Pain Point Node]`".
- PRD FR-013 review sections — "Company Info, Products & Services, Brand & Messaging, Audience & Market, Contact & Locations, Policies & FAQ, Other" — do not map onto the nine types. Nothing says where "Contact & Locations" or "Policies & FAQ" land.

**C-12 — Document and asset ingestion**
- PRD §16: "Document upload | Brain populated via crawling, conversations, and tasks | Post-MVP". MVP §4: "Document upload & extraction" out of scope.
- TA §1.1 (worker/AI box): "Scrapling crawlers · **PDF**/CSV parsing". DRAW: "Marketing CSVs & **Brand Asset Ingestion**" and "The business provides its information, preferences, resources and **documents**". DRAW also has a PDF parser benchmark.
- PRD FR-085's brand overlay needs "logo, colors, typography", but no requirement says how they enter the system.

**C-13 — Supported dialects**
- DISC §3: "dialect-aware generation (Egyptian conversational, Gulf lifestyle, MSA)". BM §2.2 and PRD §1.4 give the same three.
- MVP J2: "Dialect: [🔘 Modern Standard Arabic 🔘 Egyptian 🔘 Gulf 🔘 **Levantine**]". EVAL §4: "(Modern Standard / Egyptian / Gulf / **Levantine**)". DRAW: "Levantine & Modern Standard".

**C-14 — Billing and quotas promised but not specified**
- DR D-35: "**Stripe** tiered subscriptions per Workspace". BM §4.2 gives quotas and a "Daily competitor monitoring" tier feature. DRAW Platform Service ends with "· Billing".
- PRD §5 and §14 have no billing, quota or plan-gating FR or AC. PRD FR-004 Settings = "(account, connected platforms)". MVP §3 lists no billing component. No FR defines scheduled or recurring competitor monitoring; PRD §8.2 marks scheduled proactive work "(Future)".

**C-15 — "Instant Brain Audit" vs onboarding bounds**
- BM §5.1: "Allow prospective users to enter their website URL on the landing page for free. In **90 seconds**, show a teaser… brand summary, your top 3 competitor blind spots, and 3 high-impact post ideas."
- PRD FR-010: the crawl starts "During onboarding" (after registration). FR-011: "Timeout: max 5 minutes per website". NFR-001: "Website crawl completion | ≤ 5 minutes". No FR for anonymous use.

**C-16 — Approval model gaps between TA, PRD, DECK and DRAW**
- PRD §11.2 states: "`draft → pending_approval → approved | rejected → scheduled → publishing → published | failed`". It has no expired, revised or cancelled states.
- TA §4.2: "Signals: approve, revise, cancel, oauth_connected" and "Timers: deferred scheduling, approval expiration". DECK slide 8 (line 621): "Expires … in 24h". DRAW: the owner "requests regeneration of the deliverables".
- PRD §11.3's batch actions are approve, reject and edit; there is no "reject" signal and no "cancel" action.

**C-17 — Conversation input: candidate or direct write?**
- PRD FR-023: "Knowledge from conversations starts as `candidate`." DECK slide 5b: "the answer becomes a candidate".
- MVP J2: brand-voice form → "Sarah fills in → **Brain updated**".

**C-18 — Content approval and copy-paste**
- PRD §11.1: "Content generation | No | Content shown in chat; **user copies manually**".
- DISC §5: "Publish-Ready Campaigns that require **zero manual copy-pasting** across disparate tools". MVP J4: "Sarah reviews in chat, edits or approves". PRD FR-085: "Every generated visual is human-approved before publishing" (implying non-calendar content can be published).

**C-19 — Delivery schedule**
- MVP §8: Month 4 has "Competitor Analysis Agent, Campaign Planner, Performance CSV intake, full React Flow canvas"; Month 5 has "Approval lifecycle + Batch Review, OAuth (Meta + X), MCP Server, scheduling… Knowledge Extraction"; Month 6 has the pilot.
- TA §12.2: "11–12 | Campaign Planner, Approval lifecycle + Batch Review, OAuth (Meta + X), MCP Server, scheduling"; "13–14 | Knowledge Extraction node, evaluation harness…"; "15+ | Paid pilot onboarding". Sprint length is never stated; TA §12.1 says "Weekly sprint planning". With two sprints per month (sprints 1–2 = Month 1 in both docs), sprints 13–14 fall in Month 7, past 24 weeks.

**C-20 — Content-acceptance target**
- PRD §2.2: "≥ 75% of generated content accepted with zero or minor edits". EVAL §6 and §10: "≥ 75%".
- PRD §15.2: "Content relevance | ≥ 70% | % of content usable without major edits". MVP §7: "≥ 70%".

**C-21 — Lazy-OAuth ordering**
- PRD §7.3: "User requests posting → Platform connected? → … → H[Agent confirms connection] → C[Proceed to posting] → I[Generate posts] → J[Batch approval UI]" (connect first, then generate).
- MVP J6: "Calendar shown in Batch Review UI → Sarah edits… approves… → **On publishing, system checks OAuth**: Not connected → shows connect card → Sarah connects → task resumes" (generate first, connect at publish time).

### Low impact

**C-22 — ORM:** TA §3.1 and DECK slide 11 say "Drizzle ORM". DRAW's SQL decision images say "Fits our NestJS + **Prisma** stack (familiar and proven)" and "Prisma — ORM / Database client".

**C-23 — DB-choice rationale describes a different data model:** DRAW "Why SQL and not NoSQL?" shows entities "User, **Organization**, **Document**, **Roles & Permissions**, Chunks, Metadata" and "our application stores **page content**… as JSONB". PRD FR-003 says "Single-user access" and §16 puts document upload Post-MVP.

**C-24 — Router simple path:** TA §6.3: `Router -->|"chat (simple)"| Assembly`, and Assembly then goes through the Knowledge Extractor. PRD §7.2 says "Simple question → Direct LLM response" and §9.2 says "Ambiguous | Clarification | Ask user before routing"; the TA graph has no ambiguous branch.

**C-25 — Deliverable taxonomy:** DR D-12 lists five artifacts including "Approval Card". PRD FR-050 lists six `type` values (`research_report`, `competitor_analysis`, `content_posts`, `content_calendar`, `performance_diagnosis`, `general`) with no approval card.

**C-26 — Target segment and team size:** DISC §2 and PRD say "5–50 employees"; DECK slide 2 notes say "من 10 لـ 49 موظف" (*"10 to 49 employees"*). DR D-05 says "**4 students**"; DECK `MEMBERS` (lines 285–291) lists **five** names, while `TEAM` (lines 292–297) lists four roles.

**C-27 — Execution scope in the deck:** PRD and MVP specify calendar batch publishing to FB, IG and X. DECK slide 6 notes say "وتنفيذ بنشر بوست واحد على صفحة الشركة" (*"execution = publishing one post to the company page"*); slide 8 shows "Schedule: Now".

**C-28 — Smaller inconsistencies:**
- Agency cost: DISC §1 says "$1,500–$3,000+/mo"; BM §2.1 says "$1,000–$3,000+/month".
- Brain-value target: MVP §7 "Measurably fewer…" vs DR D-25 "≥ 50% fewer".
- Onboarding completion: PRD §15.2 "% of signups who review at least one Brain candidate" vs MVP §7 "≥ 70% of signups complete Brain review".
- Provider timing: TA App. B says items are "decided during the Sprint 1 bake-off", but its own table puts the proxy vendor at "Sprint 3".
- Scope: DR D-04 "Ship **one release with full scope** rather than deferring features" vs PRD FR-051 "Download as PDF or Markdown (stretch goal)".
- Object storage and KMS: DR D-41 default "S3-compatible (R2 / MinIO)" and D-30 "Cloud KMS" vs DECK "Amazon S3" and "AWS KMS".

---

## 9. Open questions that would change the architecture (ranked by impact)

| Rank | Question | Why it changes the architecture | Related |
|---|---|---|---|
| **1** | **Which design is canonical: `project-docs/` (PostgreSQL graph, LiteLLM, Langfuse, MENA, Marketing-only) or the deck (Neo4j, ModelGateway, LangSmith, Datadog, `eu-central-1`, Admin Builder, LinkedIn)?** DR claims authority, but the deck is the newest commit. | Decides the data plane (number of stores), vendors, region and product surface. Every downstream decision depends on it. | C-1–C-8 |
| **2** | **Is there a platform-admin "Employee Builder" with versioned, immutable employee revisions in this release, or is the Marketing Employee fixed in code behind domain-neutral seams (D-02/D-03)?** | Adds or removes an admin portal, an internal-staff identity model, an employee-configuration schema with revisions, and the "admin cannot see customer data" boundary. Also changes what TA §11.2's "frozen configuration" snapshots. | C-2, C-7 |
| **3** | **What component dispatches publishes, and is the MCP server its own deployable?** Is it the LangGraph Social Poster inside the task run, or a deterministic Temporal activity firing at schedule time (possibly weeks later)? | Sets the idempotency and locking boundary, where OAuth tokens are decrypted, the service count (4 vs 5), and what the canvas shows for a scheduled publish. | C-5, PUB-4, PUB-10 |
| **4** | **Exact hosting region and data-residency obligations for pilot customers** (which MENA country or cloud, and whether any customer requires in-country storage). | Constrains cloud provider, availability of managed PostgreSQL/Qdrant/Redis/KMS, Temporal Cloud region, and latency targets (≤ 500 ms canvas, ≤ 3 s chat). | C-4; DR D-28 |
| **5** | **Are billing, quotas and plan gating built in the 24 weeks?** That covers Stripe subscription lifecycle, per-tier metering (research tasks, scheduled posts, X posts, image gen), feature gating (Starter = Brain + Content Writer only), and "daily competitor monitoring". | Adds a usage ledger and metering to the data model, admission control before workflows start, Stripe webhooks, and scheduled recurring workflows. | C-14; BIL-1–3 |
| **6** | **What is the complete approval lifecycle?** Covers expiry (e.g. 24 h?), `revise`/regenerate, `cancel`, editing an approved or scheduled post, behaviour when tokens are revoked before a scheduled time, and whether non-calendar content (J4) can be published or is copy-only. | Determines the state machine, Temporal signals and timers, the immutable-snapshot model, and the Batch Review UI contract. | C-16, C-18, C-21 |
| **7** | **Where does a paused task wait for clarification input** — a Temporal signal, a LangGraph checkpoint, or both — and does every chat message, including simple ones, go through Temporal? | TA defines two durability layers (TA §1.2, §6.1, §13 #1) but no "clarification answered" signal (TA §4.2). Affects whether the ≤ 2–3 s latency targets are achievable and how resume works. | ORC-1, ORC-7; NFR-001 |
| **8** | **Whose platform credentials and developer accounts are used?** One BusMora Meta app (needs Business Verification + App Review) and one BusMora X developer account that BusMora pays per post, or bring-your-own per workspace? | Changes the credential model, cost attribution (X $0.015–$0.20 per post), metering, and the go-live dependency on Meta App Review. | PRD §12.2; BM §4.2 |
| **9** | **What are the Brain governance rules?** Manual entry: reviewed or not. Form answers: direct write or candidate. Delete or retire. How the seven review sections map to the nine types. Whether "Tone" and "Pain Point" are node types. Whether procedural rules go through review. | Determines the Brain schema, the status enum, audit semantics, and the retrieval filter (`approved` vs `active`). | C-9, C-10, C-11, C-17 |
| **10** | **What do the expected-load numbers actually say?** 1–3 pilot businesses vs ≥ 100 concurrent workspaces vs Scale-tier "high concurrency". How many concurrent crawls/tasks per workspace? How many items per research run? DRAW's reference example collected about 2,000 items in about 15 minutes; PRD caps research at 5 minutes. | Sizes worker, browser and proxy pools, Qdrant/PostgreSQL capacity, and per-task cost ceilings. | NFR-001, NFR-004; BM §4.2 |
| **11** | **Is any file/asset upload in scope** (logo, brand kit, fonts, PDFs) beyond the performance CSV? | Brand overlays need logo, colors and typography (FR-085). TA mentions PDF parsing and DRAW benchmarks PDF parsers. Decides whether a parsing pipeline and asset store exist. | C-12 |
| **12** | **How does "General Web" research find pages?** Through a search API (which one), or only by crawling seed URLs? | Adds an external integration, cost line and rate-limit domain. | RES-1, RES-5; DECK slide 10 "Web Search" |
| **13** | **What is the policy when robots.txt or a platform's terms disallow crawling?** "Respects robots.txt (configurable)" coexists with "stealth anti-bot navigation". Is "configurable" per source, per workspace, or global? | Shapes crawler design, per-source adapters, and whether some sources must move to official APIs. | PRD FR-011; TA §7.3 |
| **14** | **Is the Instant Brain Audit (anonymous, 90 s) part of this release?** | Would require an unauthenticated entry point, abuse and rate controls, and a fast path separate from the 5-minute onboarding crawl. | C-15 |
| **15** | **Which service owns the PostgreSQL schema?** NestJS (Drizzle) and Python (SQLAlchemy/asyncpg) both list PostgreSQL access (TA §3.1), and graph expansion runs in the AI service. DRAW's rationale says "our AI service is intentionally separated from the main PostgreSQL-backed application". | Decides migration ownership, shared-DB vs API boundary, and tenant-filter enforcement points. | TA §3.1, §5.4; DRAW |
| **16** | **Data retention and deletion:** account/workspace deletion, crawl-snapshot retention, scraped third-party personal data (usernames, comments), CSV contents, and how deletion squares with "append-only" audit and "no hard delete". | Determines storage lifecycle policies and whether audit records need redaction. | NFR Privacy; BRN-7, AUD-1 |
| **17** | **Must past task runs be replayable on the canvas,** or is replay only for reconnecting during a live run? Must chat replies stream token by token? (The removed DRAW label "Frontend renders token-by-token" was the only mention.) | Decides whether events are persisted beyond the Redis Stream buffer and the stream retention size. | CNV-5 |
| **18** | **Scheduling semantics:** which time zone (per workspace or per post), how far ahead (up to 1 month), handling of the IG 100/24 h cap, and rescheduling. | Shapes timer design and the calendar data model. | CAL-3; PRD §12.2 |
| **19** | **Notification channels:** in-app only, or email too? An email provider is needed anyway for verification and password reset. | Adds a notification service and integration. | §3.17; ACC-1–2 |
| **20** | **Which CSV formats must performance intake recognize** (which ad platforms' exports, which columns)? | Defines the normalization layer and its validation rules. | PRF-1 |
| 21 | Which delivery plan governs (TA sprints vs MVP months), and how long is a sprint? | Sequencing of the walking skeleton and when each subsystem must be architecture-ready. | C-19 |
| 22 | Is Levantine a supported dialect? | Prompt/procedural-memory variants and evaluation scope; no topology impact. | C-13 |
| 23 | Drizzle or Prisma? | Tooling only. | C-22 |

### Not addressed anywhere in the docs

Accessibility. Backup/RPO/RTO (NFR-002 says "zero data loss" without recovery objectives). Supported browsers. Upload size limits. Per-user API rate limits (only "public endpoints" is mentioned). Log and trace retention. Support/SLA for pilot customers. Account/workspace deletion. LLM-provider data-processing constraints for customer data.

---

## Appendix A — Coverage proof

Every file was read start to end. Long files were read in chunks: PRD in 1–350, 350–700, 700–1033; DECK in 1–300, 300–580, 580–844. The Excalidraw file was parsed programmatically: every live element's text, its container and its arrow bindings were dumped, and all 37 embedded PNG images were opened and viewed.

| File | Lines | How covered |
|---|---|---|
| `project-docs/product-discovery-summary.md` | 210 | Full read |
| `project-docs/decision-registry.md` | 99 | Full read |
| `project-docs/technical-architecture.md` | 571 | Full read |
| `project-docs/prd.md` | 1033 | Full read (3 chunks) |
| `project-docs/mvp-definition.md` | 313 | Full read |
| `project-docs/evaluation.md` | 158 | Full read |
| `project-docs/business-model-value-proposition.md` | 202 | Full read |
| `presentation/1-BusMora.html` | 844 (843 newline-terminated) | Full read (3 chunks), including all 13 slides' markup, embedded strings and Arabic speaker notes |
| `project-draw.excalidraw` | 23,802 | Parsed: 544 elements (450 live / 94 deleted). Live = 227 text, 77 arrows, 62 rectangles, 37 images, 31 ellipses, 13 lines, 3 diamonds. 32 bound labels, 0 frames, 0 arrow labels. All 37 images viewed. |
| *Deleted at HEAD (for the change summary only):* `BUSMORA-MASTER-SPECIFICATION.md`, `PRD.md`, `SPEC.md` | 608 / 311 / 220 (at `37a3aa1`) | Headings and targeted term search via `git show` |

### DRAW content map (for reference)

- **Reference material (not BusMora requirements):**
  - "High Level Tools", "Low Level Tools", "Tool-Calling Architecture" and "Pipeline Architecture" panels: 18 LangChain/LangGraph tutorial code screenshots, plus a LangGraph concept diagram.
  - Generic agent-pattern sketches: Single Agent (LLM ⇄ Tools), Network, Supervisor, Tool-Calling, Hierarchical, Pipeline.
  - Ten external course slides: startup stages, nine business functions, an "R.C.C.F.O." prompting framework, AI-assisted marketing levels 1–5, a tool reference guide.
  - A Temporal client/server/worker reference diagram.
  - An external research write-up: Reddit, Facebook Ads, X and YouTube mining for a different product, about 1,982 items in about 15 minutes.
- **"How BusMora Works"** (generic flow, arrows bound): Business → Business Brain → Specialized Agent → Goal → Missing Information → Create Plan → Execute Workflow → Request Approval → Perform Work → Deliver Results → Learn and Adapt → Business Brain.
- **"Example: Marketing Content Agent"** (arrows bound): Business goal → Context retrieval → Missing information → Planning → Execution → Human review → Controlled action → (unbound arrow toward) Learning.
- **"System Architecture (BusMora 4-Service Topology)"**: stacked layers Frontend / Platform Service (incl. "Billing") / Worker & AI Services / data stores. No arrows.
- **"Execution & Visualization Pipeline"**: a vertical sequence by layout, with no bound arrows. User sends task → busmora-api (NestJS POST) → Temporal Cloud → busmora-ai (Temporal Activity) → LangGraph Gen-3 Orchestrator → Redis Streams → SSE Gateway → React Flow canvas → "15-Min Batch Review UI → Signal / Verified MCP Publish (FB, IG, X)".
- **"MAIN AGENT — AI Marketing Employee"** (arrows bound): Diagnose Constraint → 5 Sub-Agents Delegation → Synthesis & Dual-Memory.
- **Brain panels:** 1. Ingestion & Extraction; 2. Dual-Memory Storage Plane; 3. Two-Stage GraphRAG Retrieval.
- **Four pillars:** Arabic Quality, Brain Moat, Execution Layer, Anti-Goodhart.
- **Decision panels:** "Vector db decisioning" (→ Qdrant), "SQL DATABASE OPTIONS" (→ PostgreSQL), "Parse Phase Benchmark" (no decision).
- **"Project management" / "Task life cycle"**: team process.
