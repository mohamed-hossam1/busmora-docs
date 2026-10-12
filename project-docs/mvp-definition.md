# MVP Definition

## AI Marketing Employee — 6-Month Release

> Defines what is in scope, how it is delivered, and what success looks like.

---

## 1. Release Strategy

BusMora ships as **one complete Marketing Employee product** over a single **6-month (24-week) release**, built by a team of **four students** working in parallel with end-to-end vertical-slice ownership.

Everything the product promises — research, analysis, content, campaign planning, performance interpretation, and approved social publishing — is delivered in this release. The architecture is designed from day one so that a future controlled Employee (e.g. Sales or Operations) can be added without redesigning the foundational domain model.

```
BusMora Marketing Employee — One Release, Full Scope
   → Onboarding + Business Brain
   → Research + Competitor Analysis + Content + Performance
   → Campaign Planning + Batch Approval + Multi-Channel Publishing
```

---

## 2. What the User Can Do

1. Sign up and enter their website URL.
2. The system crawls the website in the background while the user explores the platform.
3. The user reviews extracted knowledge candidates (categorized sections) and approves, edits, or rejects them.
4. The user opens the Marketing Employee chat.
5. The Employee proactively suggests missing information.
6. The user requests marketing tasks:
   - "Analyze my competitors" → deep competitor analysis from real sources.
   - "Do market research for product X" → market research from Reddit, YouTube, and the Web.
   - "Write 5 Instagram posts about the new product" → content written in the brand voice, with a generated brand-aligned image.
   - "Create a content calendar for next month" → a structured, editable content calendar.
   - "Here is my monthly performance CSV — what went wrong?" → a structured performance diagnosis.
7. The user watches agents working in real time on the canvas.
8. Results are delivered as structured artifacts in the Deliverables folder.
9. The user reviews planned posts in the Batch Review UI, edits or approves them, and the system publishes to Facebook, Instagram, and/or X.
10. Published posts are read-back verified, and results are reported per post.
11. Important knowledge from tasks is extracted to the Brain as candidates for review.
12. The Brain grows with each task, making future results better.

---

## 3. Components

| Component | Description |
|---|---|
| **SaaS Foundation** | Auth (registration/login), Workspace creation, navigation, multi-tenant isolation |
| **Website Crawler** | Background job: crawl → extract text → LLM analysis → knowledge candidates (bounded to public pages) |
| **Business Brain** | Relational Concept Graph (`brain_nodes` + `brain_edges` in Postgres with Qdrant vectors), Vector Seed + 1–2 Hop Expansion retrieval, candidate lifecycle, interactive React Flow graph canvas, provenance, conflict and freshness handling, audit log |
| **Chat Interface** | Hybrid chat with structured forms, free-text clarification, "Search yourself" option, message history |
| **Workflow Visualization** | Full canvas with agent nodes, animated connections, real-time status via SSE |
| **Router** | Classifies user input (simple question vs. complex task) and selects the processing path |
| **Orchestrator** | Brain lookup → missing-info detection → clarification → sub-agent delegation → result collection → knowledge extraction |
| **Market Research Agent** | Crawlers (Reddit, YouTube, Web) → data collection → LLM analysis → structured research report |
| **Competitor Analysis Agent** | Crawl competitor websites + Facebook Ads Library → comparison analysis → structured report |
| **Content Writer Agent** | Generate posts/articles using Brain context (brand voice, audience, products) → drafts with variations |
| **Image Generation** | Brand-template visuals (generated background/imagery + deterministic brand overlay) for image-required channels |
| **Campaign Planner Agent** | Generate content calendars based on Brain + research results |
| **Performance Interpretation** | Validate structured CSV input, compute deterministic KPIs, and produce a diagnosis with recommendations |
| **Social Media Poster Agent** | Post to Facebook, Instagram, and X via the MCP Server; report success and failure per post |
| **OAuth Integration** | Meta (FB + IG) and X — token exchange, encrypted storage, refresh, and preflight checks |
| **MCP Server** | `publish_facebook()`, `publish_instagram()`, `publish_x()` tool implementations with idempotency and read-back verification |
| **Batch Approval UI** | Review grid with edit/approve/reject per item, bulk actions, and scheduled publishing |
| **Deliverables System** | Central storage for all AI-generated outputs, browsable by user |
| **Knowledge Extraction** | Post-task analysis → extract reusable knowledge → create candidates → add to Brain |
| **Bilingual Foundation** | Arabic + English support in UI (full RTL) and native content generation |
| **Evaluation & Observability** | Human-reviewed scenario evaluation; LLM tracing, cost, and latency tracking |

---

## 4. Scope Boundaries

| Feature | Reason |
|---|---|
| Other AI Employees (Sales, Support, etc.) | Vertical focus — a future controlled release; extensibility proven by paper design |
| LinkedIn posting | Organization-page API approval is selective and often denied |
| TikTok posting | Strict UI/UX audit requirements |
| Paid ad management (Meta Ads, Google Ads) | Spends real money — out of scope for this release |
| Automatic performance analytics APIs | Requires extensive, permission-heavy integrations; CSV input covers the use case |
| Multi-user workspaces | Target user is a single person; adds auth complexity |
| Document upload & extraction | Brain populated via crawling + conversations + tasks instead |
| Advanced Brain governance automation | Conflict auto-resolution deferred; conflicts are surfaced to the human |
| Native mobile app | Web responsive is sufficient |

---

## 5. User Persona

### "Sarah" — The Solo Marketer

**Profile:**
- 28 years old, Marketing Coordinator or Founder at a 15-person e-commerce company in Egypt or Saudi Arabia.
- She IS the marketing department. No team, no agency retainer.
- Uses Canva for visual tweaks, ChatGPT for drafts, and posts manually across social channels.
- Disillusioned by generic AI copy generators that act as "Manual Work Multipliers"—she spends more time fixing robotic "AI slop" than writing from scratch.
- Bilingual (Arabic/English), Arabic-first, targets regional consumers who demand culturally authentic Egyptian conversational or Gulf lifestyle copy.

**Pain Points:**
- **The "Editing Trap" & Babysitting Burden:** AI drafts sound hollow and repetitive; fixing them eats up the time she hoped to save.
- **The "Amnesia of Tools":** Wastes 30 minutes every morning re-pasting product specs, brand voice guidelines, and target audience data into fresh chat windows.
- **Tool-Hopping Fragmentation:** Exhausted by juggling separate disconnected tools for research, copywriting, design templates, and social scheduling.
- **The "Pan-Arabic Trap":** Western tools output stiff Modern Standard Arabic (MSA) that feels like an imported manual rather than an engaging social commerce post.
- **Manual Execution Grind:** Great strategic ideas stall because executing calendars, formatting captions, and scheduling posts consumes 10+ hours a week.

**What She Wants:**
- "I don't need another brainstorming chatbot—I need an **Autonomous Execution Layer** that turns my decisions into completed, ready-to-publish campaigns."
- A dedicated marketing department head with permanent memory (**Business Brain**) that never forgets her brand or products.
- Deep research pulled from **real customer conversations** (Reddit, YouTube, competitor ad libraries) rather than synthetic AI hallucinations.
- Culturally authentic Arabic copy that sounds like a native human copywriter, tailored to her specific target dialect.
- A **15-minute weekly batch review** where she can inspect, approve, and verify published posts with absolute peace of mind.
- A system that gets better the more she uses it.

**Success for Sarah:**
- Saves 5+ hours/week on marketing tasks.
- Gets competitor insights she couldn't find manually.
- Produces consistent, on-brand content she can publish with confidence.
- Feels like she has a smart marketing partner, not another tool.

---

## 6. Core User Journeys

### Journey 1: Onboarding (First 10 Minutes)

```
Sarah signs up
    → Creates account (email + password)
    → Enters company website URL
    → System starts background crawling
    → Sarah sees "Crawling in progress..." indicator
    → Sarah can explore the platform while waiting
    → Notification appears: "12 knowledge items ready for review"
    → Sarah opens Brain page
    → Sees categorized sections:
        📌 Company Info (3 items)
        📌 Products (4 items)
        📌 Brand & Messaging (2 items)
        📌 Contact & Locations (3 items)
    → Reviews each item: ✅ Approve / ❌ Reject / ✏️ Edit
    → Approves 10, edits 1, rejects 1
    → Brain is populated with initial knowledge
    → Sarah enters Marketing Employee chat
```

### Journey 2: First Conversation

```
Sarah opens Marketing Employee
    → Employee (proactive):
        "Hello Sarah! I reviewed your company info.
         I noticed that Brand Voice and key competitors
         are not defined yet. Would you like me to help define them
         first, or do you have a specific task in mind?"
    → Sarah: "Define Brand Voice"
    → Employee shows structured form:
        📋 Tone: [🔘 Formal 🔘 Casual 🔘 Professional 🔘 Friendly]
        📋 Language style: [🔘 Technical 🔘 Simple 🔘 Storytelling]
        📋 Dialect: [🔘 Modern Standard Arabic 🔘 Egyptian 🔘 Gulf 🔘 Levantine]
        📋 Sample text that represents your brand: [____________]
    → Sarah fills in → Brain updated
    → Employee: "Great! Now I can write content that matches your brand voice.
                 What's the next task?"
```

### Journey 3: Market Research Task

```
Sarah: "Do market research on the skincare products market in Egypt"
    → Router: classifies as complex task → routes to Orchestrator
    → Orchestrator: retrieves relevant Brain context
    → Orchestrator: no existing market research data
    → Canvas shows: [Orchestrator] → planning
    → Orchestrator delegates to Market Research Agent
    → Canvas shows: [Orchestrator] → [Market Research] (active)
    → Market Research Agent:
        - Crawls Reddit (r/SkincareAddiction, r/Egypt)
        - Crawls YouTube (top skincare channels targeting Egypt)
        - Crawls relevant web sources
    → Canvas shows tools being used (abstracted)
    → Results collected → Orchestrator reviews
    → Orchestrator presents structured report:
        📊 Market Research Report
        ├── Market Size & Trends
        ├── Consumer Preferences (from Reddit/YouTube)
        ├── Key Players
        ├── Opportunities
        └── Recommendations
    → Report saved to Deliverables
    → Knowledge extraction: key market facts → Brain candidates
    → Notification: "3 new knowledge items for review"
```

### Journey 4: Content Creation with Generated Visual

```
Sarah: "Write 5 Instagram posts to promote the new face serum product"
    → Orchestrator: retrieves product info, brand voice, and audience from Brain
    → All available ✅
    → Orchestrator delegates to Content Writer Agent and Image Generation
    → Canvas shows flow
    → Content Writer produces 5 posts:
        Each with: caption (Arabic), hashtags, and a suggested visual brief
    → Image Generation produces a brand-template visual per post
    → Sarah reviews in chat, edits or approves
    → Saved to Deliverables
```

### Journey 5: Competitor Analysis

```
Sarah: "Analyze competitor X — see what they're posting on social media and what their ads are"
    → Orchestrator delegates to Competitor Analysis Agent
    → Agent crawls:
        - Competitor website (products, pricing, messaging)
        - Facebook Ads Library (active ads)
        - Social media presence
    → Canvas shows agent activity
    → Delivers structured comparison report
    → Key competitor insights → Brain candidates
```

### Journey 6: Campaign Planning, Approval, and Publishing

```
Sarah: "Create a content calendar for next month across Facebook, Instagram, and X"
    → Orchestrator delegates to Campaign Planner Agent
    → Planner uses Brain + prior research to build a structured calendar
    → Calendar shown in Batch Review UI
    → Sarah edits individual posts, approves some, rejects others
    → On publishing, system checks OAuth:
        - Not connected → shows connect card → Sarah connects → task resumes
        - Connected → proceeds
    → Approved posts are published via the MCP Server
    → Each post is read-back verified
    → Results (success/failure) shown per post
```

### Journey 7: Performance Interpretation

```
Sarah uploads a monthly performance CSV
    → System validates headers and normalizes the schema
    → Deterministic KPIs are computed (CTR, conversion rate, CPC changes)
    → Orchestrator produces a diagnosis:
        📈 What performed well
        📉 What underperformed
        ✅ Three prioritized adjustments for next month
    → Diagnosis saved to Deliverables; relevant facts become Brain candidates
```

---

## 7. Success Criteria

| Metric | Target |
|---|---|
| **Onboarding completion rate** | ≥ 70% of signups complete Brain review |
| **Website crawl quality** | ≥ 80% of extracted candidates approved (not rejected) |
| **Task completion rate** | ≥ 85% of user requests produce a usable result |
| **Time to value** | First useful output within 15 minutes of signup |
| **Research quality** | Reports contain real, verifiable, sourced data (not hallucinated) |
| **Content relevance** | ≥ 70% of generated content usable without major edits |
| **Arabic quality** | Meets the Arabic rubric in `evaluation.md` |
| **Brain value** | Measurably fewer clarification turns and corrections than a document-retrieval baseline |
| **Publishing success** | ≥ 95% of approved posts publish successfully and pass read-back verification |
| **Calendar adoption** | ≥ 50% of active users generate at least one calendar |
| **Platform connection** | ≥ 60% of active users connect at least one platform |
| **Brain growth** | ≥ 20 approved items after 5 tasks |

---

## 8. Delivery Timeline (6 Months)

> Four students working in parallel with vertical end-to-end ownership. Each milestone ends with an end-to-end demonstration.

```
Month 1:    Foundations + Walking Skeleton
            Docker Compose, PostgreSQL schema, auth, Next.js skeleton,
            end-to-end prompt → API → Temporal → worker → AI stub → SSE → chat.
            Discovery interviews with target SMBs.

Month 2:    Business Brain & GraphRAG
            Brain schema (nodes + edges), Qdrant collection, website crawler,
            candidate review UI, two-stage retrieval + document control route.
            Extensibility paper exercise at architecture freeze.

Month 3:    Marketing Employee Runtime
            LangGraph engine, Router, Orchestrator, clarification forms,
            Market Research Agent, Content Writer Agent, image generation,
            Deliverables storage.

Month 4:    Analysis, Planning & Visualization
            Competitor Analysis Agent, Campaign Planner, Performance CSV intake,
            full React Flow canvas + SSE.

Month 5:    Approvals, Publishing & Evaluation
            Approval lifecycle + Batch Review, OAuth (Meta + X), MCP Server,
            scheduling, read-back verification, Knowledge Extraction,
            evaluation harness and human review sessions.

Month 6:    Bilingual Polish, Pilot & Hardening
            Full RTL polish, Arabic evaluation, paid pilot onboarding (1–3 businesses),
            observability, stabilization, documentation and graduation preparation.
```

---

## 9. Extensibility

The platform ships one controlled Marketing Employee, but its foundational concepts — Employee Configuration, Capability, Workflow, Integration, Task Record, approval/execution semantics, and the Business Brain — are domain-neutral. A paper design exercise at architecture freeze tests that a future Sales or Operations Employee can be added without redesigning these concepts.
