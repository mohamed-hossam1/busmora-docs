# Business Model & Value Proposition

## BusMora: AI Marketing Director for Arabic-First Business Owners

> **Version:** 3.0 — October 2026 (PRD v2.0 alignment)
> **Status:** Supporting strategy document. Product scope and release gates are governed by [PRD v2.0](../core/prd.md). Everything commercial below is a **hypothesis to test** (PRD §12.2), not a selected price, payment provider or validated market.

---

## 1. Executive Summary

**BusMora** gives Arabic-first owner-operated service businesses an **AI Marketing Director**: it learns the essentials of the business once, helps the owner decide what to do next with limited time and budget, prepares usable marketing work in authentic Arabic or English, and publishes to Facebook only with exact owner approval.

The differentiation hypothesis is **better decisions, less owner effort and continuity across tasks in a usable Arabic-first experience** (PRD §2). Arabic generation, persistent context or multiple agents alone are not a competitive advantage, and BusMora makes no claim of exclusive capability, superior marketing results or regional product-market fit without evidence.

---

## 2. Customer Profile & Value Proposition Canvas

### 2.1 Customer Profile

**Primary:** the owner of an Arabic-first service business (clinic, consultancy, studio, training provider…) in Egypt or a named GCC country, selling through inquiries, consultations or bookings. **Secondary:** an owner or marketer with marketing experience who wants to save execution time.

#### Customer Jobs
* **Functional:**
  * Decide what marketing work deserves time and budget now.
  * Get that work done: research, offer and message drafts, campaigns, Facebook posts, response scripts.
  * Understand evidence they already have (an ad export, a lead sheet) when they ask.
  * Write copy natively in Egyptian Arabic, Gulf-oriented Arabic, MSA or English.
* **Social:** look credible and active next to better-funded competitors.
* **Emotional:** confidence that decisions rest on evidence; less marketing grind.

#### Customer Pains
* **No clear direction:** tools produce more content when the real problem may be the offer, the inquiry response or conversion.
* **The editing trap:** generic AI drafts need heavy rewriting, especially in Arabic.
* **Tool amnesia:** re-explaining the business to every new chat.
* **Tool-hopping:** research, drafting, scheduling and publishing in separate tools.
* **Translated-sounding Arabic:** stiff MSA that does not fit local audiences.
* **Agency cost:** retainers and hires most small businesses cannot sustain.

#### Customer Gains
* A defensible next action with its evidence and uncertainty.
* Usable artifacts, or honest owner instructions when BusMora cannot execute something.
* Persistent business context that the owner confirmed and can correct.
* Fast, exact approval for Facebook publishing.
* Arabic that reads as locally written.

### 2.2 Value Proposition

| Pain Relievers | Gain Creators | Product Capabilities (Phase 2) |
| :--- | :--- | :--- |
| **Business Brain:** confirmed essentials once, then owner edits and approved updates with provenance | **Diagnosis before channel selection:** a primary constraint hypothesis with evidence and alternatives | Director Chat, Roadmap Discovery and complete step plans |
| **Native register generation:** Egyptian, Gulf-oriented, MSA and English, full RTL | **Executable steps:** run with the Director, or get an owner-ready handoff | Research within authorized budgets; text-first campaigns and visual briefs |
| **One place to work:** documents in Deliverables, Posts in Scheduling | **Fast exact approval:** individual or bulk Batch Review of the displayed versions | Governed Facebook Page publishing with verification and recovery |
| **Safe control:** nothing external happens without explicit approval | **Inspectable work:** authentic Canvas and Brain graph | Requested one-off CSV / `.xlsx` analysis |

---

## 3. Competitive Positioning

### 3.1 Market Matrix

```
                          High Context / Business-Aware
                                      ▲
                                      │        ★ BusMora
                                      │   (Diagnosis + Brain + Arabic-native
                                      │    + governed execution)
              Jasper AI (Brand Voice) │
       (Ideation / Template Only)     │
                                      │
   Point Solutions /                  │                  Autonomous AI
   Generative Assistants              │                  Employees
   ───────────────────────────────────┼───────────────────────────────────►
   (ChatGPT, Copy.ai,                 │          (Artisan [Sales only],
    Buffer, Hootsuite)                │           11x [Sales/Voice only])
                                      │
                                      ▼
                           Low Context / Generic
```

### 3.2 Comparison

| Dimension | Generic AI (ChatGPT / Claude) | Content SaaS (Jasper AI) | Social Management (Buffer / Hootsuite) | BusMora (AI Marketing Director) |
| :--- | :--- | :--- | :--- | :--- |
| **What it does** | Answers prompts | Drafting templates | Scheduling & publishing | Diagnosis, planning, research, drafting and governed publishing |
| **Arabic quality** | Often translated-sounding | English-first | N/A | Native registers, RTL-first, reviewed per register |
| **Context** | Per-chat | Static brand guidelines | None | Owner-confirmed Business Brain with provenance |
| **Market evidence** | Generic web search | Basic search add-ons | Basic social analytics | Dynamic multi-source research with citations and stated gaps |
| **Governance** | None | None | Team permissions | Exact-version approval, audit log, honest handoffs |
| **Observability** | Text streaming | Form wizard | Calendar | Authentic task Canvas |

### 3.3 Defensibility Hypotheses

These are reasons BusMora may be hard to copy, to be tested rather than assumed:

1. **Context continuity:** confirmed facts, owner corrections and approved working rules (Semantic and Procedural memory) accumulate per business; their value is measured against a document-only control (EVAL §5).
2. **Last-mile execution infrastructure:** durable workflows, exact-version approval, reconciliation of uncertain outcomes and encrypted credentials are harder to rebuild than a prompt wrapper.
3. **Regional language layer:** register-specific generation and review for Egyptian, Gulf-oriented and MSA audiences, grounded in customer language from accessible sources.
4. **Governed autonomy:** the Director does the heavy work, while the owner keeps every consequential decision; completion, publication and commercial impact are never conflated.

### 3.4 Diagnosis Over Agent Sprawl

* **Point tools:** the owner operates many separate dashboards.
* **Agent sprawl:** many uncoordinated bots that the owner must manage.
* **Marketing Director (BusMora):** one Director diagnoses the likely constraint first (offer/audience, destination, conversion, acquisition, existing customers) and then selects a small set of specialists for the work. The right recommendation may be no new acquisition campaign at all (FR-16).

---

## 4. Monetization (Phase 4+ hypothesis)

* **Phase 2 has no in-product billing** and no selected payment provider or price (PRD §12.1, P4-02). Paid pilots are optional commercial learning, not a release or graduation requirement.
* **Usage and cost metering from day one:** every model call, search, crawl and platform call is recorded per workspace and per Task, including failed and retried work (NFR-06). This is internal resource accountability, not customer charging.
* **Packaging later:** choose packaging only after observing useful repeat behavior and real service costs (models, retries, infrastructure, support). Avoid lifetime-unlimited promises.
* **Payment constraint to remember:** Stripe does not directly serve Egyptian or Saudi merchant accounts, so the provider choice is open.

### 4.1 Cost Drivers to Measure

| Driver | Notes |
|---|---|
| **LLM inference** | Heavier models for planning/synthesis, lighter for routing and extraction; early provisional sizing was ≤ $0.40 per task |
| **Search, crawling and proxies** | Cache-first with freshness labels; early provisional sizing was ≤ $0.05 per search |
| **Platform calls** | Facebook Graph API has no per-post charge but has rate limits; Phase 3 platforms (e.g. X) may charge per post |
| **Infrastructure and support** | Temporal, databases, storage, observability, operator time |

The provisional figures are sizing inputs only (SYSTEM-ARCHITECTURE §2.2), not measured results.

---

## 5. Go-To-Market Hypotheses

1. **First-direction moment:** an owner confirms their essentials and gets a useful first recommendation or artifact quickly (ten-minute active-interaction target, measured).
2. **Arabic before/after examples:** concrete comparisons of register-specific output against generic tools.
3. **Fractional-help framing:** position against the cost of agency help rather than a cheap text generator, once value and costs are measured.
4. **Owner sessions as evidence:** real owner sessions (assisted first, then unassisted) show whether direction is valued over content volume and whether repeated use earns willingness to pay.

---

## 6. Value Proof

* **Arabic quality:** scored against frozen rubrics by suitable reviewers in each register.
* **Brain value:** Brain retrieval vs. document-only control on identical cases.
* **Decision usefulness:** ≥ 80% of held-out cases pass without major correction (PRD §10.2).
* **Execution reliability:** real Facebook publication evidence, including uncertain-outcome and recovery scenarios.
* **Owner effort:** active interaction time, correction effort and cost per accepted output, reported with denominators. Small samples cannot establish revenue causality, regional demand or willingness to pay.
