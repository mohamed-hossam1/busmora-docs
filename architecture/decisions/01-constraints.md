# 01 — Constraints & Evaluation Criteria

> **Purpose:** record every decision from the grilling session (2026-10-16 → 2026-10-17) on the open questions in [00-requirements-analysis.md](./00-requirements-analysis.md). Also defines the gates and weighted criteria used later to benchmark architecture options.
> **Not in this document:** architecture proposals.
> **PRD policy:** `project-docs/prd.md` is **not edited** unless a change is critical. Where a decision refines or overrides the PRD, it is recorded here with the PRD section it affects (see Appendix A).
> **Status labels:** **Settled** = answered by the user. **Doc-derived** = follows from the docs via the precedence rule and was confirmed in Q19. **Provisional** = answered, but the user reserved further study. **Deferred** = explicitly postponed by the user. **Open (design)** = left to the architecture step.

---

## 0. Ground rules

| # | Rule | Status | Source |
|---|---|---|---|
| R-1 | `project-docs/` is the single source of truth. Every deck-only item is **superseded**: admin Employee Builder, Neo4j, LinkedIn, ModelGateway/LangSmith/Datadog, the 4-sub-agent roster, REST between workers and AI. | Settled | Q1 |
| R-2 | Precedence when docs conflict: **decision-registry > PRD > technical-architecture > other `project-docs/`**. | Settled (user: "do like the docs said") | Q11, Q14, Q19 |
| R-3 | Where the docs already specify something, follow them as written. New decisions are made only where the docs are silent or contradict each other beyond what R-2 resolves. | Settled | Q11, Q14 |
| R-4 | The PRD is not edited unless a change is critical; overrides live in this document. | Settled | user instruction during Q7 |
| R-5 | **Two standalone repositories, settled and final:** `busmora-backend` (busmora-api, busmora-workers, busmora-ai) and `busmora-frontend` (busmora-web). This supersedes the single monorepo in TA §12.3. | Settled (2026-10-18) | user instruction after 02 |

---

## 1. Decision log

| Q | Topic | Decision (short) | Status | Resolves (00 doc refs) |
|---|---|---|---|---|
| Q1 | Canonical design | `project-docs/` canonical, DR is the tie-breaker, deck superseded | Settled | 00 §9 Q1–2; C-1–C-8 |
| Q2 | Billing scope | No in-product billing this release; per-workspace usage/cost metering from day one | Settled | 00 §9 Q5; C-14 |
| Q3 | Publishing path | Deterministic, non-LLM dispatch after approval | Settled | 00 §9 Q3; C-5 |
| Q4 | Approval lifecycle | Six rules; adds `cancelled` and `expired` states | Settled | 00 §9 Q6; C-16 |
| Q5 | One-off content | Any social-post output can enter Batch Review | Settled | C-18 |
| Q6 | Region | Single region, Frankfurt | **Provisional** (user studying) | 00 §9 Q4; C-4 |
| Q7 | Platform apps | One BusMora Meta app + one X app; tester fallback for the whole pilot; Instagram limit 50/24h | Settled | 00 §9 Q8 |
| Q8 | Scale | Size for the pilot; prove NFR-004 by load test; per-workspace limits | Settled | 00 §9 Q10 |
| Q9 | Budget | — | **Deferred** (all financial topics) | — |
| Q10 | Durability & latency | Durable clarification waits (7-day expiry); fast path for simple chat; p95 latency targets | Settled | 00 §9 Q7 |
| Q11 | Brain governance | Follow the docs via R-2 | Doc-derived | 00 §9 Q9; C-9–C-11, C-17 |
| Q12 | Brand assets | Crawl-only; no upload; no document/PDF ingestion | Settled | 00 §9 Q11; C-12 |
| Q13 | Brand fallback | Name-as-text, neutral palette, default fonts; editable as Brain facts | Settled | — (branch of Q12) |
| Q14 | Web discovery | Custom crawling only; no search API | Settled ("as docs said") | 00 §9 Q12 |
| Q15 | Recovery | RPO 0 for PostgreSQL and object storage; Qdrant/Redis rebuildable; RTO ≤ 1 h | Settled | 00 §9 gaps (RPO/RTO) |
| Q16 | Canvas & chat | Live-only canvas; whole messages, no token streaming | Settled (answer "ش" read as "a") | 00 §9 Q17 |
| Q17 | Time zones | One time zone per workspace; posts stored as UTC instants plus the zone | Settled | 00 §9 Q18 |
| Q18 | Deletion & retention | Operator-run full workspace deletion; retention rules | Settled | 00 §9 Q16 |
| Q19 | Doc-derived resolutions | Items 2–14 confirmed; item 1 (cloud provider) **not recorded** | Doc-derived / Deferred | 00 §9 Q13–15, Q19–23; C-13, C-20, C-21, C-22, C-24–C-26 |
| Q20 | Evaluation | Gates G1, G2, G3, G5 + nine weighted criteria; **G4 (region) withheld** | Settled / Provisional | — |
| — | Repositories | Two standalone repositories (R-5) | Settled (2026-10-18) | 02 G-R1 |
| — | Architecture adopted | B1 + amendment A1 (internal HTTP for quick replies and Brain retrieval); FE-1 now, FE-3 later | Settled (2026-10-18) | [04-decision.md](./04-decision.md) |
| — | Scoring and TA deviations | Score every option first. Deviations from the technical-architecture doc are reported as G5 exposure, not used to exclude options (§4.3 step 6). | Settled (2026-10-18) | 02 §0.1 |

---

## 2. Constraints by area

### 2.1 Scope and product shape

- **SC-1** One product: the **Marketing Employee**. No admin portal, Employee Builder, marketplace or second Employee in this release. Domain-neutral seams only (D-01–D-03). *(Q1)*
- **SC-2** **No in-product billing.** Pilots are charged outside the app. There are no tiers, quotas, plan gating or "daily competitor monitoring". *(Q2)*
- **SC-3** **Usage and cost metering per workspace ships from day one**: LLM, crawling and proxy use, image generation, X posts. It feeds the cost-derived tiers decided in D-36. *(Q2)*
- **SC-4** The **Instant Brain Audit** lead magnet (anonymous landing-page crawl) is out of scope. *(Q19.3)*
- **SC-5** Supported dialects: **Egyptian, Gulf and MSA**. Levantine is dropped. *(Q19.4)*
- **SC-6** Channels: Facebook Pages, Instagram Business and X only (D-13). LinkedIn and TikTok remain post-MVP. *(Q1)*

### 2.2 Approval, scheduling and publishing

- **PB-1** **No LLM in the path after approval.** Before approval, the Social Poster agent formats posts per platform and produces the **immutable approval snapshot**. After approval, a deterministic scheduled workflow publishes **exactly that snapshot** through the MCP tools, with idempotency locking, read-back verification and status updates. *(Q3)*
- **PB-2** The MCP publishing server runs **inside the worker/AI tier**, not as a fifth deployable. The deterministic publish workflow calls it. *(Q19.2; TA §1.1; D-26)*
- **PB-3** **State set:** `draft → pending_approval → approved | rejected → scheduled → publishing → published | failed`, plus **`cancelled`** and **`expired`**. Every transition and snapshot version is audited. *(Q4)*
- **PB-4** **Editing:** editing a pending post creates a new snapshot version, and the old one is kept. Editing an approved or scheduled post returns it to `pending_approval`. *(Q4.1)*
- **PB-5** **Regenerate:** the user can ask for a rewrite with feedback, producing a new draft version that returns to `pending_approval`. The agent never modifies an approved post. *(Q4.2)*
- **PB-6** **Cancel:** allowed from `approved` or `scheduled`, any time before `publishing`. *(Q4.3)*
- **PB-7** **Expiry:** there is no fixed approval timer. A post still pending when its scheduled time passes becomes `expired`. Rescheduling returns it to `pending_approval`. *(Q4.4)*
- **PB-8** **Failure:** automatic retries follow PRD §13.3 (2× on API error; cooldown on rate limit). Then the post goes to `failed` with the platform's reason. Manual retry is allowed and must not create a duplicate. *(Q4.5)*
- **PB-9** **Credentials broken at dispatch:** attempt a silent refresh. If that fails, mark `failed` with "reconnect needed" and notify the user. *(Q4.6)*
- **PB-10** **Any social-post output can be sent to Batch Review**, whether from a calendar or a one-off request (e.g. MVP journey J4), with a chosen time or "post now". Nothing reaches a platform any other way. Articles and ad copy stay copy/download only. *(Q5)*
- **PB-11** **Lazy OAuth:** the connection prompt appears when the user requests posting, i.e. sends items to Batch Review for publishing, before approval (PRD §7.3, AC-19). *(Q19.5)*
- **PB-12** **Time zones:**
  - Each workspace has one business time zone, set at onboarding (defaults to the browser's zone) and editable in Settings.
  - Calendars are planned and displayed in that zone.
  - Each scheduled post stores a **UTC instant plus the zone it was planned in**, so daylight-saving changes are handled.
  - "Post now" ignores the schedule.
  - Changing the zone **never** moves approved or scheduled posts.

  *(Q17)*

### 2.3 External platforms

- **EP-1** BusMora operates **one Meta app** (Facebook Login for Business) and **one X developer app** (OAuth 2.0 PKCE). Customers never need their own developer accounts. *(Q7.1)*
- **EP-2** BusMora's X developer account pays for posts. X costs are recorded per workspace via SC-3. *(Q7.2)*
- **EP-3** **There is no registered legal entity**, so Meta Business Verification and Advanced Access are impossible for now. **For the whole pilot, pilot customers are added as testers on the app** so Standard Access works. *(Q7.3 + user answer)*
- **EP-4** **Instagram publishing limit = 50 posts per 24 hours per account** (Meta reference doc). This overrides "100 posts/24h" in PRD §12.2. *(Q7.4)*
- **EP-5** **Mandatory Sprint-1 spike** (risk from facts F-9–F-11): publish from a tester-owned Facebook Page and Instagram professional account through the app, and verify the post is **publicly visible**. Meta's docs state that data created in Development mode is visible only to people with a role on the app; it's unconfirmed whether that applies to Business-type apps. Also verify the app can be linked to an unverified business portfolio. *(derived from Q7 facts)*
- **EP-6** Email is used only for verification and password reset (FR-001/002), which requires an email-sending integration. *(Q19.8)*

### 2.4 Hosting and region

- **HR-1** **Provisional:** all components run in a **single region in Frankfurt**. This rests on the premise that no pilot customer has a legal or contractual in-country requirement. D-28 ("MENA") would become "the nearest region where all managed services are available". **The user is studying this before it becomes a gate (G4).** *(Q6, Q20)*
- **HR-2** **Cloud provider: Deferred.** It is not recorded until decided. *(Q19.1)*
- **HR-3** Known fact regardless of HR-1: **customer data leaves MENA** through Temporal Cloud (no Middle East region) and the LLM providers. *(F-1–F-3)*

### 2.5 Scale and capacity

- **SC-7** **Provisioned capacity** is sized for the pilot: ≤ 3 workspaces and a handful of concurrent tasks system-wide. *(Q8.1)*
- **SC-8** **PRD NFR-004 (≥ 100 concurrent workspaces)** is met by a **load test with stubbed LLM and crawler calls** before pilot onboarding. That capacity is not kept running. *(Q8.2)*
- **SC-9** **Per-workspace limits:** at most **2 concurrent tasks** and **1 crawl** at a time. Further requests queue. *(Q8.3)*
- **SC-10** **Research volume** is bounded by the documented budgets, not item counts: **≤ 5 min wall-clock** and **≤ $0.40 per task** (≤ $0.05 per search). When either limit hits, the collected data is used and the user is told the result is partial. *(Q8.4; the cost figures are from BM §4.3 and fall under the financial deferral if revisited)*

### 2.6 Durability, latency and recovery

- **DL-1** **Clarification waits are durable.** A paused task survives deploys, restarts and browser closure, and resumes when answered. It **expires after 7 days** unanswered: cancelled, with a notice to the user. *(Q10.1)*
- **DL-2** **Simple chat is not durable.** Router "simple" replies are persisted as messages, but their workflow state need not survive a crash; the user re-sends. *(Q10.2)*
- **DL-3** **Hard p95 latency targets:**
  - ≤ 3 s for a simple chat reply
  - **≤ 2 s from submitting a complex task to visible acknowledgement** (task record plus first canvas event)
  - ≤ 500 ms from a backend event to the canvas update
  - Long work is asynchronous and exempt from the 2–3 s targets (research ≤ 5 min, content ≤ 30 s per piece).

  *(Q10.3; PRD NFR-001)*
- **DL-4** **RPO = 0** for everything committed to PostgreSQL: Brain nodes, edges, procedural rules and candidates; deliverables; conversations; approvals and snapshots; publish attempts; the audit log; credential records. Point-in-time restore is available. *(Q15.1)*
- **DL-5** **Object storage** (snapshots, CSVs, images) has no-loss durability. *(Q15.2)*
- **DL-6** **Qdrant and Redis are derived stores.** They may lose data but must be **fully rebuildable from PostgreSQL and object storage**, including re-embedding nodes and control-route chunks. Losing Redis events can only affect live-canvas replay. *(Q15.3)*
- **DL-7** **RTO ≤ 1 hour** for a full outage, within the ≥ 99% uptime budget (about 7.3 h/month). *(Q15.4)*
- **DL-8** In-flight workflows resume after recovery from durable workflow history (TA §11.2). *(Q15.5)*

### 2.7 Business Brain governance (doc-derived)

- **BG-1** Manual entries on the Brain page are **approved immediately** (PRD FR-023, §10.2).
- **BG-2** Knowledge sourced from conversations, **including structured-form answers**, starts as **`candidate`** (PRD FR-023). PRD wins over MVP journey J2's "Brain updated".
- **BG-3** **"Delete" is implemented as retirement with an audit trail.** There is no hard delete of individual items (D-19, FR-023 append-only audit, DISC §6). The only hard deletion is workspace deletion (DT-1).
- **BG-4** Candidates are reviewed **individually**; there is no bulk approval (D-24).
- **BG-5** **Procedural rules** derived from edits and feedback are candidates that need approval before they affect generation (PRD §10.2).
- **BG-6** Conflicts are flagged to the user and never auto-resolved (FR-023).
- **BG-7** The nine canonical types (D-21), the seven review sections (FR-013) and the edge definitions (FR-020) stay as written. **How they map onto each other is Open (design).**

### 2.8 Brand assets and ingestion

- **BA-1** **Logo and colors come only from the onboarding website crawl.** There is no brand-asset upload. *(Q12)*
- **BA-2** **No document or PDF ingestion this release.** The "PDF/CSV parsing" in TA §1.1 is limited to CSV. The only upload is the performance CSV. *(Q12)*
- **BA-3** **Fallbacks:** if no logo is found, the overlay uses the business name as text and the Brain page shows "logo not detected" (fix the site, then re-crawl). If no colors are found, a neutral default palette is used. *(Q13.1–2)*
- **BA-4** **Typography:** the crawled font is used only if it is freely licensed and loadable by the renderer. Otherwise BusMora's default Arabic/Latin font pair is used. *(Q13.3)*
- **BA-5** Detected colors and the font choice are stored as **Brain facts**, editable on the Brain page. No image upload. *(Q13.4)*

### 2.9 Research and crawling

- **RC-1** **Custom Scrapling crawling only**, including "General Web" (PRD §12.1, D-32). **No third-party search API.** How the crawler discovers General Web pages is **Open (design)**. *(Q14)*
- **RC-2** The onboarding website crawl **respects robots.txt (configurable)** (FR-011). Platform and competitor crawling uses stealth navigation, proxies, rate limits and cooldowns (TA §7.3). *(Q19.9)*
- **RC-3** Raw research crawl data not referenced by a Brain node is kept only for the **7-day cache window** (DT-3). *(Q18.3)*

### 2.10 Real-time and chat UX

- **RT-1** The **canvas is live-only**. Replay covers reconnects during a run plus a short grace window after it ends; the window length is a design detail. Finished tasks show their final state with no re-animation. *(Q16)*
- **RT-2** **No token streaming.** Agent replies appear as complete messages. Progress is shown through task-status messages and canvas events. *(Q16)*
- **RT-3** **Router:** ambiguous requests ask the user before routing, and simple requests get a direct reply (PRD §9.2). This overrides TA §6.3's simple → Assembly → Knowledge Extractor path. *(Q19.6)*
- **RT-4** **Notifications are in-app only** (chat messages and badges). *(Q19.8)*

### 2.11 Data lifecycle

- **DT-1** **Workspace deletion is operator-run** this release, not self-serve. On request, it permanently removes all of that workspace's data from PostgreSQL (**including its audit log**), Qdrant, object storage, Redis and LLM traces. Its OAuth credentials are made unrecoverable. Only a minimal deletion record remains (workspace ID, timestamp, operator). *(Q18.1)*
- **DT-2** **Provenance snapshots** (crawl HTML, CSVs) are kept as long as any node or deliverable references them, including retired nodes. *(Q18.2)*
- **DT-3** **Unreferenced raw research data** is deleted after **7 days**. Deliverables keep source URL, platform and access date (FR-062), not the raw scraped content. *(Q18.3)*
- **DT-4** **Every stored row and object is attributable to a workspace**, so DT-1 is complete and verifiable. *(Q18.4; extends TA §1.2, §5.3)*

### 2.12 Other doc-derived resolutions (Q19)

- **DD-1** **Deliverables:** the PRD FR-050 `type` list is the data model. D-12's artifacts map onto it:
  - Research Brief → `research_report` / `competitor_analysis`
  - Plan → `content_calendar`
  - Content Package → `content_posts`
  - Performance Diagnosis → `performance_diagnosis`
  - The **Approval Card is an approval record, not a deliverable**.
- **DD-2** **Targets:**
  - Content acceptance **≥ 75%** (PRD §2.2 + EVAL; overrides 70% in PRD §15.2)
  - Onboarding completion = **reviewed at least one candidate** (PRD §15.2)
  - Brain value = **≥ 50%** fewer clarifications and corrections (D-25)
- **DD-3** **Team and timeline:** 4 students, 24 weeks (D-05). The TA-vs-MVP sprint mapping is a **planning** matter, not an architecture constraint. Both docs agree the walking skeleton comes first.
- **DD-4** **ORM:** Drizzle (TA §3.1).

### 2.13 Deferred by the user

| Item | Note |
|---|---|
| **All financial topics:** budget ceilings, credits, pricing, payment provider | Q9: "ignore any financial thing for now". Facts gathered are kept in §3 for later. Stripe (D-35) is unavailable to Egypt- and Saudi-based merchants (F-12). |
| **Cloud provider** | Q19.1: "don't add it for now". |
| **Region as a gate (G4)** | Q20: "will not be added until I study it". HR-1 remains provisional. |

### 2.14 Open for the architecture step (design decisions, not constraints)

1. Which service owns the PostgreSQL schema and its migrations. Both NestJS (Drizzle) and Python (SQLAlchemy/asyncpg) access it, and graph expansion runs in the AI service.
2. Performance CSV formats and the normalization layer (FR-095: validate, normalize, clear error).
3. How General Web pages are discovered within custom crawling (RC-1).
4. Mapping between review sections, node types and edge-target kinds (BG-7).
5. Where a paused task's durable wait lives, Temporal or LangGraph checkpoints, within DL-1/DL-2.
6. The length of the canvas replay grace window (RT-1).

### 2.15 Not raised in the session (still unaddressed in any doc)

Accessibility. Supported browsers. Upload size limits (CSV). Per-user API rate limits. Log and trace retention periods. Support/SLA for pilots. Constraints on sending customer data to LLM providers.

---

## 3. External facts relied upon

Gathered by research on 2026-10-16. Most vendor pages show no date. **UNCONFIRMED** marks facts the research could not verify.

| # | Fact | Source |
|---|---|---|
| F-1 | Temporal Cloud has **no Middle East region**. Its Frankfurt options are AWS `eu-central-1` and GCP `europe-west3`. GCP namespace provisioning is temporarily restricted from Oct 2026. | docs.temporal.io/cloud/regions |
| F-2 | Langfuse Cloud regions are US, EU (`eu-west-1`), Japan and HIPAA, with no Middle East region. It is **self-hostable** (MIT, except the `ee/` directories). | langfuse.com/security/data-regions; langfuse.com/self-hosting |
| F-3 | Qdrant Cloud runs on AWS, GCP and Azure with no published region list; a Middle East region is **UNCONFIRMED**. AWS `eu-central-1` appears on its status page. A Hybrid Cloud option (bring your own Kubernetes) exists. | qdrant.tech/documentation/hybrid-cloud; status.qdrant.io |
| F-4 | AWS Middle East: `me-south-1` (Bahrain) and `me-central-1` (UAE) have RDS/Aurora PostgreSQL, ElastiCache, KMS and S3. Saudi Arabia is not live yet (planned Dec 2026). | docs.aws.amazon.com regional service pages |
| F-5 | GCP Middle East: `me-central1` (Doha) and `me-central2` (Dammam, restricted to KSA customers via CNTXT) have Cloud SQL, Memorystore, Cloud KMS and GCS. | docs.cloud.google.com location pages |
| F-6 | Azure: UAE North, UAE Central (restricted) and Qatar Central have PostgreSQL Flexible Server, Key Vault and Blob Storage. Azure Cache for Redis is retiring. | learn.microsoft.com |
| F-7 | Meta: one app can serve many businesses via Facebook Login for Business. Posting to businesses that have no role on the app needs **Advanced Access**, which requires **Business Verification** and App Review for each permission. | developers.facebook.com/docs/graph-api/overview/access-levels |
| F-8 | Meta permissions: `pages_manage_posts`, `pages_read_engagement`, `pages_show_list` (Pages). `instagram_basic` + `instagram_content_publish` via Facebook Login, or the `instagram_business_*` scopes via Instagram Login. Instagram media must be on a **publicly accessible URL**, and JPEG is the only image format. | developers.facebook.com/docs/pages-api/posts; …/instagram-platform/content-publishing |
| F-9 | Standard Access applies only to people with a role on the app; Meta's docs require Business Verification only for Advanced Access. Up to **50 testers** for apps not linked to a verified business. | …/access-levels; …/build-and-test/app-roles |
| F-10 | "Data generated while an app is in Development mode … can only be seen by role users." Business-type apps "do not have app modes"; whether the visibility rule applies to them is **UNCONFIRMED**. | …/build-and-test/app-modes; …/app-types |
| F-11 | An app accessing data you don't own must be connected to a **business portfolio**. An individual can register as a Meta developer. | …/development/create-an-app; …/development/register |
| F-12 | Stripe merchants: **UAE supported; Egypt and Saudi Arabia not**. | stripe.com/global |
| F-13 | Instagram publishing limit: the reference doc says **50 per 24 h**. The guide says 100 in one place and 50 in another. A carousel counts as one post. | …/ig-user/content_publishing_limit |
| F-14 | X: pay-per-use, **$0.015 per post, $0.20 per post with a URL**, charged to the developer's credits. Individuals can sign up. OAuth 2.0 PKCE lets the app act for any authorizing user (`tweet.write`, `offline.access`; 2 h access tokens). Limits: 100 posts per 15 min per user and 10,000 per 24 h per app. | docs.x.com pricing, OAuth 2.0 and rate-limit pages; Developer Agreement (updated 2026-04-27) |

---

## 4. Evaluation framework (for benchmarking architecture options)

### 4.1 Hard gates (pass/fail; an option failing any gate is excluded)

| Gate | Requirement | Basis |
|---|---|---|
| **G1** | No external write without approval. The approved snapshot is published **byte-for-byte**, with **no duplicates** across retries and reconnects. | PB-1, PB-3, PB-8; PRD FR-103 |
| **G2** | **Tenant isolation is enforced in every store** (relational, vector, object, cache/streams, traces), and every record is workspace-attributable. | PRD NFR-003, AC-13; TA §9.1; DT-4 |
| **G3** | **No loss** for PostgreSQL and object storage (RPO 0, PITR). Qdrant and Redis are fully **rebuildable** from them. | DL-4–DL-6 |
| ~~G4~~ | ~~Single region (Frankfurt)~~. **Withheld pending the user's study** (HR-1). | Q20 |
| **G5** | Consistent with the decision registry and every **Settled** or **Doc-derived** item in this document, unless the user explicitly reopens one. | R-1–R-3 |

### 4.2 Weighted criteria (weights sum to 100; cost excluded per Q9)

| # | Criterion | What it measures | Evidence to look at | Weight |
|---|---|---|---|---|
| **C1** | **Delivery feasibility** | Buildable by 4 students in 24 weeks | Count of new technologies vs team familiarity; how many vertical slices are independently buildable; how early the walking skeleton (prompt → API → workflow → AI stub → SSE) runs | **20** |
| **C2** | **Reliability & durability** | Durable clarification waits, idempotent publishing, crash resume, failure isolation | Behaviour under the kill tests (worker or API killed mid-task, mid-publish); duplicate-publish impossibility argument; blast radius of one failing component | **15** |
| **C3** | **Security & isolation** | Few tenant-enforcement points; minimal credential exposure; injection and SSRF defense | Number of places where `workspace_id` must be enforced; where OAuth plaintext exists and for how long; whether untrusted crawled text can reach a tool-calling context | **15** |
| **C4** | **Operational simplicity** | Fewer moving parts, easier to run and debug | Number of deployables, stores and external services; Docker Compose parity with production; steps to trace one task end to end | **12** |
| **C5** | **Latency & real-time UX** | Meets the DL-3 p95 targets | Number of network hops/queues on the simple-chat and task-acknowledgement paths; event path from agent to canvas | **10** |
| **C6** | **AI quality enablers** | Grounding/retrieval quality, Arabic generation, evaluability | Support for the treatment-vs-control switch with the same model and prompts; procedural-rule injection; prompt/model versioning per run | **10** |
| **C7** | **Observability & testability** | Tracing, evaluation harness, deterministic tests | Per-task trace correlation across services; ability to stub the LLM, crawler and platform APIs at clear boundaries; seeded-workspace scenarios | **8** |
| **C8** | **Extensibility** | Domain-neutral seams for a future Employee; swappable providers | Changes needed to add a second Employee (paper exercise); adapter seams for LLM, embeddings, image, proxy and publishing | **6** |
| **C9** | **Scalability headroom** | Passes the 100-concurrent-workspace load test (SC-8) without redesign | Which components scale horizontally vs need redesign; queueing behind the per-workspace limits (SC-9) | **4** |
| | | | **Total** | **100** |

### 4.3 Scoring method

1. Apply the gates. Drop any option that fails G1, G2, G3 or G5.
2. Score each remaining option **1–5** on each criterion:
   - **1** = fails the criterion or carries a major unmitigated risk
   - **3** = meets it with notable effort or risk
   - **5** = meets it cleanly with low risk

   Write one line of justification per score, citing the evidence column.
3. **Weighted score** = Σ (weight × score) / 5, which gives a range of 20–100.
4. **Tie-break:** higher C1, then higher C2.
5. If G4 is later adopted, re-run step 1 only. If cost is later added, re-weight and record the new weights here.
6. **G5 and TA deviations** (user instruction, 2026-10-18): a deviation from the technical-architecture doc does **not** exclude an option at step 1. Each option's deviations are listed as its **G5 exposure**, and you accept or reject them when an architecture is adopted. Deviations from the decision registry or from Settled/Doc-derived items here still fail G5.

---

## 5. Risks recorded during the session

| # | Risk | Source | Mitigation recorded |
|---|---|---|---|
| K-1 | Pilot posts may not be publicly visible under Standard Access / Development mode | F-10 | Mandatory Sprint-1 spike (EP-5) |
| K-2 | No legal entity, so Advanced Access is impossible and the pilot is capped by the tester model | EP-3, F-9 | Tester fallback for the whole pilot; 50-tester ceiling is far above 1–3 pilots |
| K-3 | Customer data leaves MENA (Temporal Cloud, LLMs) whatever the region | HR-3 | HR-1 provisional; user studying |
| K-4 | Instagram limit is 50/24h, not 100 | EP-4, F-13 | Recorded as the constraint |
| K-5 | Brand overlay quality depends on what the crawl can detect | BA-1 | Fallbacks BA-3–BA-5 |
| K-6 | Budget unknown; managed-service choices may later conflict with it | Q9 | Deferred; cost can be added as a criterion (4.3 step 5) |

---

## Appendix A — Places where this document refines or overrides `project-docs/` (PRD not edited, per R-4)

| Doc / section | Doc says | This document says | Ref |
|---|---|---|---|
| PRD §12.2 | Instagram "100 posts/24h limit" | 50 posts/24h | EP-4 |
| PRD §11.2 | State machine without cancel/expiry | Adds `cancelled`, `expired`; edit/regenerate return to `pending_approval` | PB-3–PB-7 |
| PRD §11.1 | Content generation: "user copies manually" | Any social-post output can enter Batch Review | PB-10 |
| PRD §15.2 | Content relevance ≥ 70% | ≥ 75% (PRD §2.2 / EVAL) | DD-2 |
| PRD §6 NFR-004 | ≥ 100 concurrent workspaces | Met by stubbed load test; capacity sized for pilot | SC-7, SC-8 |
| PRD FR-085 | Brand overlay (logo, colors, typography); source unspecified | Crawl-only, with fallbacks | BA-1–BA-5 |
| PRD FR-021 | "delete the node" | Delete = retire with audit | BG-3 |
| DR D-28 | Region matched to pilot (MENA) | Single Frankfurt region (**provisional**) | HR-1 |
| DR D-35 | Stripe tiered subscriptions + paid pilot | No in-product billing this release; financials deferred | SC-2, §2.13 |
| TA §1.1 | "PDF/CSV parsing" | CSV only | BA-2 |
| TA §6.3 | Router simple → Assembly → Knowledge Extractor | Simple → direct reply; ambiguous → ask first | RT-3 |
| TA §4.2 | "approval expiration" timer | No fixed timer; expiry at the scheduled time | PB-7 |
| MVP J2 | Form answer → "Brain updated" | Becomes a candidate | BG-2 |
| MVP J2 / EVAL §4 | Levantine dialect | Dropped | SC-5 |
| BM §4.2 / §5.1 | Tiers, quotas, daily monitoring, Instant Brain Audit | Out of scope this release | SC-2, SC-4 |
| TA §12.3 | Monorepo `busmora/` (frontend, platform, workers, ai-service) | Two standalone repositories: `busmora-backend`, `busmora-frontend` | R-5 |
| TA §3.1 | Temporal TypeScript SDK "(Workflow Client)" | Also runs an activity worker in `busmora-api` | 04 X-1 |
| TA §1.2 | "Communication via Temporal activities" | Plus internal HTTP api → ai for quick replies and Brain retrieval only | 04 X-2 |
