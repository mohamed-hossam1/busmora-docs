# 03 — Benchmark

> **What this is:** the backend options B1–B3 and frontend options FE-1–FE-3 from [02-options.md](./02-options.md), scored against the gates and weights in [01-constraints.md §4](./01-constraints.md).
> **What this is not:** an adoption decision. That comes next, from the results and trade-offs laid out here.
> **Inputs:** 01 (including R-5 two repositories and §4.3 step 6 G5 handling, both settled 2026-10-18) and 02. `project-docs/` and the domain files are unchanged since the full read on 2026-10-17 (git clean; timestamps match).
> **Settled before scoring:** the two standalone repositories (01 R-5) apply to every option, so they are not counted against any of them.

---

## 0. Method

### 0.1 Steps
Follows 01 §4.3:
1. **Gates.** G1, G2 and G3 are applied strictly. **G5** is reported as *exposure* and does not exclude an option (step 6, your instruction). **G4** is withheld.
2. **Indicators first.** Measurable facts are collected for each option from 02 before any score is given (§2.1, §3.2).
3. **Scores.** Each option gets a 1–5 score per criterion: **1** = fails or carries a major unmitigated risk; **3** = meets with notable effort or risk; **5** = meets cleanly with low risk. 2 and 4 sit between. Each score has a one-line justification citing indicators.
4. **Weighted score** = Σ (weight × score) / 5, on a scale of 20–100. Ties go to the higher C1, then C2.
5. **Sensitivity.** Rankings are re-checked under (a) ±1 changes to the cells most open to judgement and (b) four alternative weight sets.

### 0.2 Objectivity caveat
The same author designed the options and scored them. To limit bias, every score cites indicators that anyone can check against 02, and the sensitivity analysis shows exactly which cells decide each ranking. The appendix has a blank sheet for each team member to score independently, mirroring the two-reviewer approach in EVAL §4. Comparing those sheets is the strongest available check.

### 0.3 Applying C1–C9 to the frontend
The weights in 01 apply to the frontend unchanged. This is how each criterion is read for a frontend:

| Criterion | What it means for the frontend |
|---|---|
| C1 Delivery feasibility | Concepts the team must learn beyond client-side React; frontend infrastructure to build; backend work the option requires |
| C2 Reliability | Live state stays correct after disconnects and restarts; behaviour when one tier fails |
| C3 Security | Where tokens live; exposure to cross-site scripting and request forgery; how SSE is authenticated |
| C4 Operational simplicity | Server tiers the frontend adds; servers holding connections; hops to debug |
| C5 Latency & real-time UX | Network legs on the Canvas path (≤ 500 ms); how fast non-Canvas state appears |
| C6 AI quality enablers | Support for evaluation instrumentation (batch-review timing, edit and correction events) and Structured Forms |
| C7 Observability & testability | Ease of mocking the API; testability of the data flow; passing a trace id through |
| C8 Extensibility | Effort to add a future Employee's screens |
| C9 Scalability headroom | Server load the frontend architecture creates as workspaces grow |

---

## 1. Gate check

| Gate | B1 | B2 | B3 | FE-1 | FE-2 | FE-3 |
|---|---|---|---|---|---|---|
| **G1** No unapproved, altered or duplicate publish | Pass: atomic claim of the approved Post Version + idempotency key = Version id | Pass: one workflow per Post + idempotency key | Pass: same as B1 | Pass: approvals carry the Post Version id (02 C.4 #4) | Pass | Pass |
| **G2** Tenant isolation in every store | Pass | Pass | Pass (more enforcement points, scored in C3) | Pass: holds no other tenant's data | Pass | Pass |
| **G3** No loss in PostgreSQL / object storage; Qdrant and Redis rebuildable | Pass: single writer, transactional | Pass: persist then acknowledge | Pass: outbox replay | n/a (stores nothing durable) | n/a | n/a |
| **G5** exposure: deviations from the technical-architecture doc, reported not excluded | **1**: Temporal TS SDK as activity worker | **2**: domain-state allocation moves to workers; all commands via Temporal | **4**: internal HTTP; Brain in ai; TS SDK worker; Redis Streams for domain events | **0** | **1** (interpretation): proxy tier between browser and api for REST and SSE | **1**: SSE carries non-Canvas UI events (TA §2 and §8 scope streams to the Canvas) |
| **G5** decision-registry / 01 violations | None | None | None | None | None | None |

**One residual risk sits outside every option.** Meta and X don't accept idempotency keys. If the publish activity crashes after the platform accepted the post but before the attempt was recorded, a retry could post twice. Every option relies on the same common publish path (02 §0.3 #5). It closes this gap the same way: record the attempt before calling the platform, then do a read-back check before any retry. This doesn't separate the options.

**Result:** all six options pass G1–G3. None is excluded. G5 exposure is carried into the decision inputs (§5).

---

## 2. Backend scoring

### 2.1 Indicators (facts from 02)

| # | Indicator | B1 | B2 | B3 |
|---|---|---|---|---|
| E1 | Integration styles between services | 1 (Temporal) | 1 (Temporal) | 3 (Temporal, internal HTTP, events) |
| E2 | Process roles, including web | 7 | 7 | 10 |
| E3 | Infrastructure building blocks beyond product features | **5:** TS activity worker; payloads mirrored between Pydantic and TS; retrieval views; `IndexingWorkflow`; `QuickReplyWorkflow` | **6:** Update handlers and validators; continue-as-new (`BrainWorkflow`); a `persist` activity per transition; versioning for 4 long-lived workflow types; `QuickReplyWorkflow`; retrieval views | **8:** transactional outbox (both sides); relay (both sides); inbox/dedup consumers (both sides); projections; API composition; internal HTTP + service auth; TS activity worker; Python migration toolchain (**not in the stack**) |
| E4 | Long-lived workflow types to version (> 1 day) | 2: Task ≤ 7 d, PostRelease ≤ 1 month | 4: Task, Post, Brain (open-ended), Connected Account (open-ended) | 2: Task, PostRelease |
| E5 | PostgreSQL writers / migration toolchains | 1 / 1 | 2 / 1 | 2 / 2 (one not in the stack) |
| E6 | Language domain rules are written in | TypeScript (team's documented NestJS + PostgreSQL experience) | Python workflows (no documented team experience with Temporal) | TS for 7 contexts, Python for 4 |
| E7 | Places that must enforce the workspace | 4: api queries; ai read-only views; Qdrant filter; workflow inputs | 6: api queries; `persist` activities; Update validators; workflow ids; ai views; Qdrant filter | 7: api queries; ai HTTP endpoints; api consumers; ai consumers; outboxes; ai schemas; Qdrant filter |
| E8 | Service-to-service auth mechanism defined by the docs | Temporal + database roles (method for Temporal is gap G-D5) | Same | Same, **plus internal HTTP auth that is undefined** (G-D7) |
| E9 | Processes that ever hold OAuth plaintext | 1 (`workers.publish`) | 1 | 1 |
| E10 | Network legs on the quick-reply path, excluding the LLM call | 8 † | 8 † | 2 (api → ai → api) |
| E11 | Network legs on the Canvas path | 3 (ai → Redis → api → browser) | 3 | 3 |
| E12 | What stops if Temporal Cloud is unavailable | Tasks, quick replies, publishing, indexing, onboarding | Tasks, quick replies, publishing, onboarding, **Brain Review, Batch Review actions**, account refresh | Tasks, publishing timers, onboarding |
| E13 | How lifecycle races are prevented (cancel vs release, edit vs publish) | Atomic claim in PostgreSQL | Serialized inside one workflow per Post | Atomic claim in PostgreSQL |
| E14 | Lifecycle tests: time-skipping, in one codebase | No: rules in TS, sequencing in Python | Yes: workflow tests with time skipping | No: split across services and events |
| E15 | Retrieval code and Brain data owned by the same service | No (ai reads views of another owner's tables) | No | Yes |
| E16 | Services touched to add a future Employee (paper exercise) | 3 (api modules, workflows, ai) | 2–3 (workflows, ai; thin api endpoints) | 1–2 (new ai context + event subscriptions) |

† api → Temporal (start) → workflow worker → Temporal (schedule activity) → ai → Temporal (complete) → workflow worker → Temporal → api (result).

### 2.2 Scores and justifications

| Criterion (weight) | B1 | B2 | B3 |
|---|---|---|---|
| **C1** Delivery feasibility (20) | **4**: one integration style, 5 extra blocks, rules in the team's familiar NestJS (E1, E3, E6). The cost is polyglot activity payloads. | **3**: one style, but 6 blocks including advanced Temporal features (Updates, continue-as-new, versioning 4 long-lived types), with rules in Python workflows (E3, E4, E6) | **2**: three integration styles and 8 blocks, one needing a tool outside the stack. Mitigable by using Drizzle for all schemas, which weakens ownership (E1, E3, E5). |
| **C2** Reliability & durability (15) | **4**: durable waits; races prevented by an atomic claim (correct, but depends on every write path using it); Temporal outage pauses AI and publishing only (E12, E13) | **4**: race-free by construction and durable everywhere. Offsets: the widest blast radius (E12) and version risk on open-ended workflows (E4). | **3**: same claim as B1, plus eventual-consistency failure modes (lost or duplicate events handled by outbox and inbox). Narrowest blast radius (E12). |
| **C3** Security & isolation (15) | **5**: 4 enforcement points, single writer, plaintext in one process, crawled text never near publish tools (E7, E9) | **4**: 6 enforcement points across two writers; otherwise the same as B1 (E7) | **3**: 7 enforcement points and an undefined internal-HTTP auth mechanism (E7, E8) |
| **C4** Operational simplicity (12) | **4**: 7 roles, one style, 2 long-lived workflow types; Temporal UI shows each process (E1, E2, E4) | **3**: 7 roles and one style, but 4 long-lived types (2 open-ended) make every deploy a versioning exercise (E4) | **2**: 10 roles, 3 styles, relays and consumer groups to operate; no single place to trace a request without the correlation work in G-D12 (E1, E2) |
| **C5** Latency & real-time UX (10) | **3**: quick reply and task acknowledgement cross Temporal 8 times before the router's LLM call; the 3 s / 2 s targets are reachable but not comfortable (E10) | **3**: same paths as B1 (E10). Batch Review and Brain commands also pay Update round trips; no NFR covers those. | **5**: 2 legs on the quick path; same Canvas path (E10, E11) |
| **C6** AI quality enablers (10) | **4**: Treatment/Control flag frozen per run; procedural rules and prompt versions recorded. Tuning retrieval needs coordinated view changes in api-owned schema (E15). | **4**: same as B1 (E15) | **5**: retrieval and Brain data share an owner, so graph queries and indexes evolve in one place (E15) |
| **C7** Observability & testability (8) | **4**: all externals stubbable at activity boundaries; workflow id = Task id correlates everything; lifecycle tests split across two languages (E14) | **5**: Temporal history per Post, Task and Brain; lifecycle and timer behaviour testable with time skipping in one codebase (E14) | **3**: a request spans HTTP, events and workflows; event contract tests needed; eventual consistency complicates assertions (E1, E14) |
| **C8** Extensibility (6) | **3**: a future Employee touches all 3 services, and the api monolith accumulates every context (E16) | **4**: new workflow types plus ai graph; api stays thin (E16) | **5**: a new context subscribes to existing events (E16) |
| **C9** Scalability headroom (4) | **4**: all roles stateless and horizontal; single writer is ample at 100 workspaces | **4**: horizontal `persist` and ai roles; per-workspace Brain serialization is fine at this scale | **5**: every role scales independently |

### 2.3 Weighted results

| Criterion | Weight | B1 (score → points) | B2 | B3 |
|---|---|---|---|---|
| C1 | 20 | 4 → 80 | 3 → 60 | 2 → 40 |
| C2 | 15 | 4 → 60 | 4 → 60 | 3 → 45 |
| C3 | 15 | 5 → 75 | 4 → 60 | 3 → 45 |
| C4 | 12 | 4 → 48 | 3 → 36 | 2 → 24 |
| C5 | 10 | 3 → 30 | 3 → 30 | 5 → 50 |
| C6 | 10 | 4 → 40 | 4 → 40 | 5 → 50 |
| C7 | 8 | 4 → 32 | 5 → 40 | 3 → 24 |
| C8 | 6 | 3 → 18 | 4 → 24 | 5 → 30 |
| C9 | 4 | 4 → 16 | 4 → 16 | 5 → 20 |
| **Weighted score** (Σ / 5) | **100** | **79.8** | **73.2** | **65.6** |
| Rank | | 1 | 2 | 3 |

### 2.4 Sensitivity

**(a) Weight scenarios** (each sums to 100):

| Scenario | Weights C1 / C2 / C3 / C4 / C5 / C6 / C7 / C8 / C9 | B1 | B2 | B3 | Order |
|---|---|---|---|---|---|
| Base (01 §4.2) | 20 / 15 / 15 / 12 / 10 / 10 / 8 / 6 / 4 | 79.8 | 73.2 | 65.6 | B1 > B2 > B3 |
| Reliability-first | 15 / 25 / 15 / 7 / 10 / 10 / 8 / 6 / 4 | 79.8 | 75.2 | 67.6 | B1 > B2 > B3 |
| UX & AI-quality-first | 15 / 15 / 15 / 7 / 20 / 15 / 7 / 4 / 2 | 78.2 | 73.0 | 72.0 | B1 > B2 > B3 |
| Long-term product | 15 / 15 / 15 / 8 / 8 / 8 / 6 / 15 / 10 | 78.4 | 75.0 | 71.8 | B1 > B2 > B3 |
| Equal weights | 11.1 each | 77.8 | 75.6 | 73.3 | B1 > B2 > B3 |
| Extreme: latency / AI / extensibility / scale | 5 / 10 / 10 / 5 / 25 / 20 / 0 / 15 / 10 | 74.0 | 73.0 | **86.0** | **B3** > B1 > B2 |

The order holds under every plausible re-weighting. B3 comes first only when delivery feasibility and operational simplicity together drop to about 10% while latency, AI enablers, extensibility and scale reach about 70%.

**(b) Score uncertainty.** These are the cells most open to judgement, and the effect of moving each by one point:

| Cell | Alternative | Effect |
|---|---|---|
| B1 · C1 (4) | 3 (if the polyglot activities prove harder than expected) | B1 → 75.8 |
| B1 · C3 (5) | 4 | B1 → 76.8 |
| B2 · C1 (3) | 2 (if the Temporal learning curve bites) | B2 → 69.2 |
| B2 · C2 (4) | 5 (if race-freedom is valued above blast radius) | B2 → 76.2 |
| B3 · C2 (3) | 4 | B3 → 68.6 |
| B3 · C5 (5) | 4 | B3 → 63.6 |

- **B1 vs B2 is close.** Two one-point changes flip it: B1·C1 down to 3 and B2·C2 up to 5 give B2 76.2 against B1 75.8. The cells that decide it are **C1 for B1 and C2 for B2**.
- **B3 in third place is robust.** It stays last after any two single-cell changes in its favour.

### 2.5 Trade-offs: what each option buys, what it costs, and what would have to be true

| | Buys | Costs | Right choice if… |
|---|---|---|---|
| **B1** | Fewest moving parts that still meet every gate. Domain rules in the stack the team knows. Isolation enforced in the fewest places. | Every persistence step crosses languages through Temporal. Rules (NestJS) and sequencing (Python) live in different places. Quick replies queue through Temporal. A large api. | You accept 1 G5 exposure (TS SDK as activity worker). Quick-reply p95 through Temporal Cloud measures inside 3 s. Mirrored Pydantic/TS payloads stay manageable. |
| **B2** | Lifecycle correctness by construction; native timers; the best per-aggregate history and time-skipping tests. | Temporal on almost every user write. Advanced Temporal concepts to learn. Open-ended workflows to version on every deploy. | The team commits early to Updates, continue-as-new and workflow versioning. Race-freedom is valued above blast radius. You accept 2 G5 exposures. |
| **B3** | Shortest chat path; retrieval and Brain data owned together; the strongest seams for a future Employee; independent failure and scaling. | Three integration styles, ten process roles, a migration tool outside the stack, the most enforcement points, eventual consistency. | Latency, AI-iteration speed and extensibility outweigh delivery and operations risk (for example, a larger or more experienced team). You accept 4 G5 exposures. |

---

## 3. Frontend scoring

### 3.1 Gates
See §1: all three pass G1 and G2, and G3 does not apply.

### 3.2 Indicators (facts from 02)

| # | Indicator | FE-1 | FE-2 | FE-3 |
|---|---|---|---|---|
| F1 | New concepts beyond client-side React | 0 | 4 (Server Components, Server Actions, streaming Route Handlers, App Router caching) | 2 (typed event router, event-patched cache) |
| F2 | Frontend runtime tiers | 1 (static export possible) | 2 (Node server required, in front of the api) | 1 (static export possible) |
| F3 | Access token readable by browser JavaScript | Yes | No | Yes |
| F4 | Network legs from api to the Canvas | 1 | 2 (api → Next.js → browser) | 1 |
| F5 | How non-Canvas live state arrives (badges, Post statuses, crawl progress) | Polling | Revalidation + polling | Pushed on the workspace stream |
| F6 | SSE connections per open workspace | 1 per running Task (≤ 2, 01 SC-9) | Same, all held by the Next.js server | 1 |
| F7 | Backend work the option requires | None | None | Workspace stream + UI events |
| F8 | Replay model | Per-Task `Last-Event-ID` (native, D-34) | Per-Task, through a proxy | Merged stream + reset-and-refetch |
| F9 | Extra frontend surface per new API endpoint | None | A BFF route or action | An event handler if the endpoint pushes |
| F10 | Unresolved gaps on the security path | Refresh-token transport, SSE auth (G-D6, G-T3) | Session storage and encryption, request-forgery protection (G-T19, G-D6) | Same as FE-1 |

### 3.3 Scores and justifications

| Criterion (weight) | FE-1 | FE-2 | FE-3 |
|---|---|---|---|
| **C1** (20) | **5**: no new concepts, no backend work, single tier (F1, F2, F7) | **2**: four new framework concepts; a BFF layer duplicating api surface; an SSE proxy to build (F1, F9) | **3**: two new concepts plus backend work for a workspace stream (F1, F7) |
| **C2** (15) | **4**: native per-Task replay; polling guarantees eventual freshness; no frontend server to fail (F5, F8) | **3**: Next.js restarts drop every proxied stream; server rendering itself is robust (F2, F8) | **3**: one stream carries all live state, so a fault degrades every screen; merged replay needs a reset-and-refetch fallback (F8) |
| **C3** (15) | **3**: token in browser JavaScript; SSE auth needs a cookie or a URL token (F3, F10) | **4**: no tokens in the browser; same-origin cookie. Session handling and request-forgery protection still to be designed (F3, F10). | **3**: same as FE-1 (F3, F10) |
| **C4** (12) | **5**: one tier, can be static (F2) | **2**: a second server tier holding long-lived connections; one more hop to debug (F2, F6) | **4**: one tier, but a richer event plumbing to diagnose (F2, F8) |
| **C5** (10) | **4**: Canvas 1 leg, meets ≤ 500 ms; non-Canvas freshness bounded by the polling interval (F4, F5) | **3**: 2 legs on the Canvas path through a proxy (F4) | **5**: Canvas 1 leg and everything else pushed (F4, F5) |
| **C6** (10) | **4**: all three capture batch-review timing, edit diffs and corrections through the same API; Structured Forms are equal | **4**: same | **4**: same |
| **C7** (8) | **4**: plain request/response; easy to mock the API | **3**: server components and actions need server-side test setups; two tiers | **3**: event-handling paths and cache patches need dedicated tests |
| **C8** (6) | **4**: a new feature folder and routes | **3**: also needs BFF routes and actions per endpoint (F9) | **4**: a new feature folder plus its event handlers |
| **C9** (4) | **4**: static delivery; polling load on the api grows with open tabs | **3**: every request and stream passes through Next.js servers that must be scaled (F6) | **5**: one connection per workspace, no polling (F6) |

### 3.4 Weighted results

| Criterion | Weight | FE-1 | FE-2 | FE-3 |
|---|---|---|---|---|
| C1 | 20 | 5 → 100 | 2 → 40 | 3 → 60 |
| C2 | 15 | 4 → 60 | 3 → 45 | 3 → 45 |
| C3 | 15 | 3 → 45 | 4 → 60 | 3 → 45 |
| C4 | 12 | 5 → 60 | 2 → 24 | 4 → 48 |
| C5 | 10 | 4 → 40 | 3 → 30 | 5 → 50 |
| C6 | 10 | 4 → 40 | 4 → 40 | 4 → 40 |
| C7 | 8 | 4 → 32 | 3 → 24 | 3 → 24 |
| C8 | 6 | 4 → 24 | 3 → 18 | 4 → 24 |
| C9 | 4 | 4 → 16 | 3 → 12 | 5 → 20 |
| **Weighted score** | **100** | **83.4** | **58.6** | **71.2** |
| Rank | | 1 | 3 | 2 |

### 3.5 Sensitivity

| Scenario | Weights C1 / C2 / C3 / C4 / C5 / C6 / C7 / C8 / C9 | FE-1 | FE-2 | FE-3 | Order |
|---|---|---|---|---|---|
| Base | 20 / 15 / 15 / 12 / 10 / 10 / 8 / 6 / 4 | 83.4 | 58.6 | 71.2 | FE-1 > FE-3 > FE-2 |
| Security-first | 10 / 15 / 25 / 12 / 10 / 10 / 8 / 6 / 4 | 79.4 | 62.6 | 71.2 | FE-1 > FE-3 > FE-2 |
| Real-time-first | 10 / 15 / 15 / 7 / 25 / 10 / 8 / 6 / 4 | 80.4 | 61.6 | 76.2 | FE-1 > FE-3 > FE-2 |
| Equal weights | 11.1 each | 82.2 | 60.0 | 75.6 | FE-1 > FE-3 > FE-2 |
| Extreme real-time and scale | 5 / 15 / 15 / 7 / 30 / 5 / 5 / 3 / 15 | 79.4 | 61.6 | **81.0** | **FE-3** > FE-1 > FE-2 |

**Score uncertainty:**
- **FE-1 vs FE-3** flips only after **four** one-point changes. For example: FE-1·C1 to 4, FE-1·C3 to 2 and FE-3·C1 to 4 give FE-1 76.4 against FE-3 75.2, still FE-1. A fourth change (FE-3·C2 to 4) brings FE-3 to 78.2 and puts it ahead. No combination of three changes flips it.
- **FE-2's last place is robust.** Even with C3 weighted at 25, it stays third.

### 3.6 Trade-offs

| | Buys | Costs | Right choice if… |
|---|---|---|---|
| **FE-1** | Fastest to build; one tier; native per-Task replay; no backend work | Token in browser JavaScript (needs the G-D6 decisions); polling for non-Canvas live state | Speed and simplicity matter most, and a short-lived in-memory access token is acceptable at pilot scale |
| **FE-2** | Tokens never in the browser; same-origin; server-composed pages | Steepest learning curve; a second server tier holding streams; a proxy hop on the Canvas | Keeping tokens out of browser JavaScript is a hard requirement, and the team is fluent in Server Components |
| **FE-3** | Every screen live with one connection; matches the domain events | Richest frontend–backend event contract; backend workspace stream; cache-patching logic | Live badges, statuses and progress everywhere matter enough to fund the extra contract and backend work |

---

## 4. Pairings (backend × frontend)

No combined weight exists in 01, so the pairings are not reduced to one number. The table notes where a pairing changes a criterion:

| | FE-1 | FE-2 | FE-3 |
|---|---|---|---|
| **B1** | No extra work on either side; the shape closest to the technical-architecture doc | Proxy hop (FE-2) **and** Temporal on the quick path (B1) both sit on the latency path: the weakest C5 pairing | api writes UI events as it persists each change. Single writer makes this one place. |
| **B2** | No extra work; Update round trips visible on Batch Review and Brain actions | Same as B1 + FE-2, plus Update latency | `persist` activities write UI events, the only writer of process state |
| **B3** | Eventual consistency shows up as polling delay | Proxy hop on top of eventual consistency | The events already exist: the most natural fit, and the richest contract |

---

## 5. Decision inputs for adoption (results, not a decision)

1. **Rankings under the base weights:**
   - **Backend:** B1 79.8 · B2 73.2 · B3 65.6.
   - **Frontend:** FE-1 83.4 · FE-3 71.2 · FE-2 58.6.
2. **Robustness:**
   - **B1 vs B2 is the fragile comparison.** Two one-point judgement changes (B1·C1, B2·C2) flip it.
   - FE-1 vs FE-3 needs four one-point changes to flip.
   - B3's and FE-2's last places survive every realistic weight scenario.
3. **G5 exposure you would be accepting:**
   - B1: 1 deviation (TS SDK as activity worker).
   - B2: 2 (allocation; commands via Temporal).
   - B3: 4.
   - FE-1: 0. FE-2: 1 (interpretation). FE-3: 1.
4. **Evidence that would firm up the deciding cells**, before or right after adoption:
   - **B1·C1 and C5:** a walking-skeleton spike that runs one Python workflow calling a TypeScript activity, and measures quick-reply p95 through Temporal Cloud.
   - **B2·C1:** whether the team can absorb Updates, continue-as-new and workflow versioning in Sprints 1–2.
   - **FE-1·C3:** the G-D6 decisions on refresh-token transport and access-token lifetime.
5. **Independent re-scoring** with the appendix sheet, especially C1 and C2 for B1 and B2. This is the cheapest way to remove the author-bias caveat in §0.2.

---

## Appendix — Blank sheet for independent re-scoring

Each team member scores alone, 1–5 per cell, with one line of justification citing an indicator from §2.1 or §3.2. Compare the sheets and discuss any cell where two scorers differ by more than one point (as in EVAL §4).

**Backend**

| Criterion (weight) | B1 | B2 | B3 |
|---|---|---|---|
| C1 (20) | | | |
| C2 (15) | | | |
| C3 (15) | | | |
| C4 (12) | | | |
| C5 (10) | | | |
| C6 (10) | | | |
| C7 (8) | | | |
| C8 (6) | | | |
| C9 (4) | | | |
| Weighted (Σ w·s / 5) | | | |

**Frontend**

| Criterion (weight) | FE-1 | FE-2 | FE-3 |
|---|---|---|---|
| C1 (20) | | | |
| C2 (15) | | | |
| C3 (15) | | | |
| C4 (12) | | | |
| C5 (10) | | | |
| C6 (10) | | | |
| C7 (8) | | | |
| C8 (6) | | | |
| C9 (4) | | | |
| Weighted (Σ w·s / 5) | | | |
