# J. Approved Execution Contract

> Approved planning basis: project owner's directives of 2026-11-15. Applies to every file in `planning/` and `report/`.

## Authority and scope

All 99 tasks T01–T99, seven Epics, two phases, four people and the 16-week delivery commitment remain in force. All Phase 1 and Phase 2 features are committed. There is no scope reduction, deferred Phase 2 backlog or reduced release variant.

This contract supersedes earlier planning assumptions about manual engineering hours, Meta API access, late validation and internal operations. It also explicitly overrides conflicting delivery and publishing premises in [PRD](../core/prd.md), [System Architecture](../core/SYSTEM-ARCHITECTURE.md), [Backend Design](../core/BACKEND-TECHNICAL-DESIGN.md), [Frontend Design](../core/FRONTEND-TECHNICAL-DESIGN.md) and [concepts brief](../concepts_brief.md) for this plan. The publishing overrides remain explicit here; the repository ownership sections in the source designs are updated to the four-repository decision below. Their other product, security, data-control and acceptance requirements continue to apply. Requirement identifiers remain stable; the AC-30 transport interpretation below governs implementation.

## Repository ownership

The project owner's repository decision of 2026-11-15 establishes **four repositories in total: three code repositories and one documentation repository**.

| Repository | Owned code or documentation | Process roles / output |
|---|---|---|
| `busmora-web` | Next.js application, UI and generated public API client | web image / web role |
| `busmora-api` | NestJS public API, domain modules, database schemas/migrations and platform activities | API image / api-http and api-activities |
| `busmora-ai` | FastAPI, agents, retrieval, research crawling, Python Temporal workflows and publishing worker | Separate AI and workers images / ai-http, ai-agent, ai-crawl, workers-workflows and workers-publish |
| `busmora-docs` | Requirements, architecture, decisions, planning, evaluation specifications and execution evidence | Existing documentation repository; no runtime image |

Repository count is separate from deployment count: the existing four deployables and eight process roles remain. Python worker code lives in `busmora-ai`, with publishing secrets and browser execution still isolated from AI/crawl processes. NestJS remains the sole domain writer.

`busmora-api` publishes the versioned public OpenAPI artifact consumed by `busmora-web`. `busmora-ai` owns canonical Pydantic payload models and publishes versioned Temporal JSON Schema and internal FastAPI OpenAPI artifacts; `busmora-api` pins those versions and generates its types/clients. Code repositories have independent CI, dependency locks and compatibility checks; no consumer imports another repository's source tree. There is no additional contracts or workers repository.

T01 creates and bootstraps both `busmora-api` and `busmora-ai`, including local backend composition and independent checks. T02 creates `busmora-web`. T05 documents local startup across the three code repositories and the public contract; T06 verifies the API/AI activity and HTTP contract boundaries. These obligations stay within the existing task IDs and scheduled weeks. Documentation and evidence remain in the existing `busmora-docs` repository.

This replaces the earlier combined-backend/frontend repository split. Source designs, ADR-002 and repository references throughout the architecture research use this same four-repository layout.

## AI-assisted capacity

The 1,950 hours in task tables are a traditional manual-effort baseline, not required human developer hours. The project owner confirms that the team's 3×–5× acceleration has been benchmarked and proven; this audit accepts that confirmation rather than rerunning the benchmark. It yields 390–650 human developer hours for the baseline. The full 99-task, 16-week schedule is validated and committed.

Use 3× for staffing and 5× as the faster case. Preserve each task's baseline estimate and range for traceability; record actual human time, agent usage, review time and rework separately. Agent generation, tests and tooling accelerate execution while people remain responsible for design, review and acceptance.

To remove the older Definition of Done accounting ambiguity, reserve up to three additional manual-equivalent hours for each of the 99 tasks (297 total, 59.4–99 human hours) and a separate two uncompressed human hours per person per week (128 total) for dogfooding, coordination and human judgment. The resulting planning envelope is 577.4–877 human hours across 64 person-weeks. Against a 20-human-hour-per-person weekly planning allocation (1,280 hours), this leaves 403–702.6 human hours for technical repair and integration inside the same 16 weeks. These are effort allowances, not extra tasks or buffer weeks.

Strict domain separation, isolated module boundaries, automated PR linting and named schema owners are confirmed delivery controls, not unresolved collaboration risks. Assign peer reviews and staging slots at the start of each week; the integration owner coordinates schema merges and publishing-state changes. Track actuals for execution accounting and use the reserved effort for corrective work while keeping all 99 tasks committed; this does not reopen the benchmark or schedule.

## Publishing through authenticated browser sessions

The Python `workers-publish` role uses **Scrapling adaptive parsing with Playwright browser actions**, in an isolated context authenticated by the client's own session or credentials. Normal publishing runs headlessly; initial login and reauthentication provide an owner-controlled interactive step launched from the dashboard. No Meta developer application, Graph API publishing token, business verification or App Review approval is a delivery prerequisite. This is separate from `ai-crawl`: research crawlers and models never receive publishing sessions.

1. T03 defines the authorized browser-session flow, supported Page actions, evidence and session recovery cases.
2. T19 connects the owner-controlled account, selects a Page and stores encrypted session state scoped to its workspace.
3. T35 executes an exact-version approved text/link Post through that session at its scheduled time.
4. T38 hardens session restoration, browser deployment, health checks and owner reauthentication for production.
5. T39 reconciles uncertain outcomes through visible Page history and durable post identity or permalink.
6. T43 handles expiry, revocation, holds, cancellation and safe reconnection; T47 adds single-image publishing.
7. T55 verifies faults and isolation; T63 verifies both formats with a real client's authorized production session.

Credentials, cookies and session storage are secrets. Only the publishing worker can decrypt them. They never enter model prompts, ordinary logs, screenshots or exported diagnostics. Browser contexts are isolated by workspace and account; actions verify the selected Page. Disconnect advances authorization generation, destroys usable session material and prevents queued actions under the old generation.

Before a publish action, recheck version, destination, authorization generation, cancellation, context hold and lateness allowance. Record the attempt before clicking Publish. A click or successful browser navigation is not proof of publication: capture post identity/permalink and a read-back that matches the approved content and destination. An ambiguous result is held for reconciliation, never blindly resent. Use a durable claim per approved version to prevent two workers sending concurrently.

Expired sessions, login challenges, unmatched or ambiguous DOM elements and account restrictions are technical operating cases. Pause affected publication and automatically expose the appropriate recovery action in the dashboard; authentication problems request owner reauthentication, while DOM mismatches request adapter repair. Do not bypass challenges or report unverified success. Exercise these cases weekly and keep adapter repair within the publishing tasks.

**AC-30 interpretation:** an intended client authorizes their own browser session, selects a Page they control, approves and publishes text/link and single-image Posts under the production configuration, and sees verified outcomes. App-role distinctions and API-access approvals are inapplicable. FR-33 and AC-30 are fully scheduled technical obligations.

### Runtime boundaries and durable handoffs

The public path remains **Next.js → NestJS `api-http` → Temporal Python workflows → activities**. FastAPI `ai-http` is an internal read-only AI/retrieval service, not a replacement public API or a publishing endpoint. NestJS `api-activities` alone applies publishing/connection domain writes. `workers-publish` performs browser actions and returns typed observations; it has no direct domain-database access. Authentication state is encrypted in the publishing role before its opaque envelope is persisted through platform activities.

T05/T06 define and check the generated HTTP contracts and versioned Pydantic/JSON Schema/Ajv activity contracts. Publishing payloads carry workspace, connection, Page, Post/version, schedule revision, authorization generation, credential version, attempt ID, content/media references and deadline. They contain no plaintext credentials or plaintext browser storage; session material travels only as scoped references or opaque encrypted envelopes. Distinguish verified success, definite pre-action failure, reauthentication required, adapter mismatch and uncertain external outcome; generic timeouts never become success or an instruction to resend.

Preserve the existing after-commit/reconciliation design: deterministic workflow IDs and a reconciliation scan recover a committed command whose workflow start or signal was lost. T06 exercises this with a stub; T35 applies it to publishing. Browser work belongs in activities, never deterministic workflow code or the FastAPI request loop. T38 pins compatible Scrapling, Playwright and browser-image versions and bounded activity, heartbeat and browser-action timeouts; T55 tests termination and rollout behavior.

### Adaptive DOM safety

T03 establishes versioned, sanitized fixtures and trusted element fingerprints. Scrapling relocates candidate elements; Playwright re-resolves the current live element and verifies Page/account identity, expected composer, action label, uniqueness and approved content before an irreversible action. Missing, multiple or semantically wrong matches stop safely; a similarity score alone never authorizes a click. Test Arabic/English layouts, changed wrappers, stale DOM snapshots and look-alike Publish buttons. T38 repeats these tests across the pinned runtime.

Fingerprint storage must be explicitly isolated by workspace/account, adapter version and UI locale, not only by website domain. Encrypt sensitive persisted fingerprints; exclude credentials and unnecessary private content. Only verified fixtures update the trusted baseline. Browser profiles, temporary media, crash dumps and diagnostic artifacts follow the same secret-isolation/cleanup controls; do not share profile directories between contexts or expose browser debugging endpoints.

Adaptive matching improves resilience but is not proof that arbitrary UI changes can never fail: the [official Scrapling documentation](https://scrapling.readthedocs.io/en/latest/parsing/adaptive.html#troubleshooting) includes missing and wrong-element matches. The required behavior is correct adaptation **or safe hold**, never a best-guess publication.

### Duplicate prevention and cancellation boundary

The safety objective is no duplicate dispatch for an approved Post version. Implement it with the following contract, rather than treating a local idempotency key as a platform-side exactly-once guarantee:

1. NestJS owns a durable uniqueness key scoped to workspace, Post and approved version. Bind the approved content/media digest, Page, schedule revision and authorization generation to the attempt. Retries, workflow restarts and reconnects reuse that logical identity; they cannot create a fresh key to escape an unresolved attempt. Any unresolved attempt holds the entire Post across later edits/reschedules; a new version cannot bypass it. Deliberate duplication of a confirmed published Post remains the separate owner action in T87.
2. Atomically record the claim and attempt before dispatch, under the existing Post lock. Recheck approval, destination, generation, holds, cancellation and deadline. Serialize browser mutation per connected account/profile as well as per Post. Assign one execution owner; do not transfer an in-flight attempt merely because a heartbeat or lease expired.
3. Run the irreversible dispatch activity without automatic retries. Read-only reconciliation and idempotent platform writes may retry. Once dispatch may have begun, a lost response, worker crash or timeout leaves the Post uncertain/held. Neither a workflow reset nor a replacement worker repeats that browser action. A safe retry requires durable proof of failure before submission and termination of the previous browser execution; otherwise retain the hold.
4. Persist a durable post identity/permalink and read back the approved content, media and destination before marking published. Absence from Page history is not proof of non-publication. Identical content posted manually or by another approved Post cannot be attributed to this attempt using text/time alone; ambiguous attribution stays held. T39 adds automated reconciliation to T35's initial safe hold.
5. Cancellation/disconnect committed before dispatch authorization prevents dispatch. Once dispatch is authorized, cancellation is a stop request, not proof that Facebook rejected an in-flight action. Close the browser context, reconcile and show cancelled only when no submission is established; otherwise show verified publication or uncertainty. Never claim to retract an accepted Post. Reconnection cannot revive an old attempt or approval invalidated by a generation change.
6. Preserve unresolved-attempt identities and publication evidence alongside the protected revocation/hold record across restore. T43/T60/T69 ensure an older database or workflow history cannot send again; a restore without the required control records remains publication-disabled until reconciled.

This deliberately favors a safe hold over a duplicate when the external outcome is unknown. [Temporal's activity documentation](https://docs.temporal.io/activity-definition) requires application-level idempotency; its [cancellation lifecycle](https://docs.temporal.io/activity-execution#cancellation) is not an atomic rollback of an external browser action. These controls define the implementation obligation; only execution tests can verify it.

### Dashboard reauthentication and session persistence

T19 implements the minimal dashboard flow; it does not wait for T51's Notification Bell. On expiry/challenge, an idempotent platform activity records `reconnect_needed`, an actionable reason and a deduplicated notification. The dashboard reads authoritative connection state and offers Re-authenticate. Browser/session failures do not restart a publishing action automatically.

The authenticated owner requests a short-lived, single-use reauthentication operation bound to user, workspace, connection and expected authorization generation. An access-controlled interactive browser channel belongs only to that operation; the owner enters credentials directly into the platform login, with no credential logging, recording or public browser-control endpoint. Restrict navigation to configured platform login/Page destinations and prevent access to internal networks. Check request origin/CSRF protection and operation expiry. Reject wrong-workspace, replayed and late completions. The worker verifies the account and selected Page, encrypts the resulting state, and the API commits it only if the operation and expected generation are still current. Disconnect invalidates pending operations, drains old contexts and prevents a late completion from reconnecting the account.

Version the encrypted session envelope and bind its encryption context to workspace, connection and credential version. Capture the state actually required by the tested login (cookies, local storage and, if needed, IndexedDB); do not assume browser `sessionStorage` is automatically persisted. T03 records the inventory; T19/T38 test restart/rotation and safe failure on incomplete state. [Playwright's authentication guide](https://playwright.dev/python/docs/auth) distinguishes these storage mechanisms. Persist ciphertext only; plaintext working state stays in memory or a restricted ephemeral profile that is cleaned on exit.

Connection state has a monotonic revision. Replayed/out-of-order events cannot replace newer state; a dashboard reconnect or expired stream cursor refetches an authorized snapshot. T06 establishes cursor/deduplication/reset behavior and T19 applies it to connection refresh. T43 tests two concurrent reauth windows, disconnect during login and late worker results. A stale completion can contribute reconciliation evidence for its own attempt but cannot reopen a disconnected connection or clear a newer hold.

### Approved image integrity

T47 binds the approved image to an immutable object version and digest, not a mutable URL. Resolve a fresh short-lived download reference at execution, verify workspace ownership, digest and current media eligibility, then upload those exact bytes. No arbitrary external URL fetch is allowed. A replaced/deleted image, wrong digest, expired access that cannot be renewed, or failed upload stops before final submission; temporary files are cleaned. Read-back must establish the image and caption, allowing documented platform transformations rather than requiring platform-generated bytes to match the original hash. Changing media requires new approval; refreshing access to the same approved object does not.

### Week 1 completion contract

T01–T04 start independently from the approved documents. Provision only the inputs each task needs during its week; production accounts, all LLM keys and the final managed infrastructure are not kickoff gates.

| Task / owner | Week-1 completion evidence |
|---|---|
| T01 / A | `busmora-api` and `busmora-ai` skeletons: seven backend process roles and local stores start from locked dependencies; CI lint/type/test/build checks reject a failing PR; access-controlled bootstrap staging runs real health/store probes (worker readiness may use process/queue probes, not invented HTTP servers); a second developer reproduces setup |
| T02 / B | Independently deployed `busmora-web` shell; six honest empty destinations, Arabic RTL/English LTR, mobile/keyboard checks and merge-blocking CI; no backend integration or product account system required yet |
| T03 / C | Authorized standalone Scrapling/Playwright text/link experiment, compatible version manifest, Page/permalink/read-back evidence, adaptive mismatch and recovery contract, encrypted-state inventory and redacted fixtures; not represented as a finished dashboard/publisher |
| T04 / D | Fixed research cases, raw provider results, measured latency/cost/coverage, selected path and fallback; credentials provisioned for these experiments only |

Each record identifies source revision or experiment artifact, environment/configuration, reviewer, actual results, defects and reruns. A/B own their bootstrap deployments and access restrictions; internal ops supplies the isolated runtime and required experiment accounts as normal task inputs. T03/T04 artifacts join the weekly evidence pack, not fake application endpoints. Before T07, keep these records in the docs repository; T07 imports them. Mark non-applicable domain audit/usage/notification checks with a reason, while recording real experiment usage and access controls. T12 hardens staging later without postponing Week-1 deployment.

## Weekly staging and value validation

At the end of every week, deploy the current increment to staging and dogfood it. Weeks 1–2 use an access-controlled bootstrap staging environment delivered by T01/T02 and extended in T06. T12 hardens it into the managed, automated environment; staging does not begin in Week 3.

| Weeks | Weekly validation focus | Owning work |
|---|---|---|
| 1–2 | Bilingual shell, deployability, authorized browser experiment, research and model samples, rubric and context-versus-document experiments | T01–T08 |
| 3–4 | Accounts, forms, tracing, isolation, records, lifecycle and recovery | T09–T16 |
| 5–6 | Knowledge, real replies, connection, documents, files, durable Tasks and retrieval; repeat Brain comparisons on the implemented path | T17–T24 |
| 7–9 | Intake, Director, outputs, approval, publishing, Canvas, research and added features | T25–T36, T65, T70–T75 |
| 10–13 | Session hardening, recovery, Roadmap, data controls and all remaining feature breadth | T37–T52, T66–T69, T76–T91 |
| 14–15 | Broader security, language, recovery, load and held-out evidence on the integrated product | T53–T60, T92–T97 |
| 16 | Final stabilization, production owner validation, polish and final-candidate regression | T61–T64, T98–T99 |

Early experiments validate only the functionality and hypotheses they exercise; they do not claim that later features already exist. The weekly record includes build identifier, exercised paths, team or client participants, sources/cases, observed value, defects, repairs and reruns. Team dogfooding and client observations are labelled separately. T07 formalizes the record and imports Weeks 1–2 notes.

Brain-versus-document comparisons and register tuning recur as the implementation grows. T93 consolidates the full held-out comparison; it is not the start of value validation. Negative measurements drive implementation, retrieval and prompt refinement within the committed scope. Optional external dialect feedback supplements weekly team evaluation and does not delay the core engine. Preserve provenance and mark missing independent scores as unmeasured.

## Continuous tests and final artifact

Each change receives automated checks, peer review and affected-scenario regression. Each weekly staging increment runs the cumulative suite for implemented workflows; security, session isolation, publishing controls and recovery checks grow with those workflows. T56/T95 are expanded quality checkpoints, and T58 is the full-load confirmation of earlier measurements.

T61, T62 and T99 accept defects discovered throughout Week 16, including those raised by T63 and T98. T98 starts from the available candidate and repeats the full set of 57 scenarios after the last repair merge. T63 supplies production browser-publication evidence for the same final build; a relevant later change invalidates and reruns it. T64 incorporates the final behavior into runbooks.

The release evidence identifies the exact build, configuration, fixtures and environment. Any subsequent code or relevant configuration change invalidates the affected results and the final-candidate certification. T98's full suite and T63's production results must be green on the final candidate before promotion. Open critical or high technical defects require repair and retest; the commitment does not authorize declaring failing evidence passed.

## Gates and coordination

The seven-Epic order and scheduled weeks remain. Gates are assessed continuously through the week, with repair and reruns inside the allocated weekly effort. Teams may prepare independent work, but may not merge or activate a dependent capability against a failed prerequisite. A gate defect receives a named owner and an immediate within-week repair/assignment plan.

Task startup prerequisites normally come from earlier weeks. Completion evidence has explicit same-week handoffs: weekly staging integration, final repair merges → T98 certification, T63 production evidence → release certification, and repair notes → T64. These handoffs supersede earlier blanket claims that all task completions are independent. They add neither tasks nor weeks.

## Internal operations and measurable controls

Company registration, payment administration, infrastructure purchasing and optional reviewer recruitment run in parallel under internal ownership. Accounts, LLM keys and servers are provisioned progressively in the week of the consuming milestone: bootstrap/experiment access in T01–T04, model access in T08, managed staging in T12, protected client sessions in T19 and production configuration in T60. They are not front-loaded planning gates. Keep service availability checks with the consuming task; an actual outage is a technical incident, not a legal-registration dependency.

T07 freezes measurable workload, latency, concurrency, provider-call and cost envelopes using T04/T08 measurements and documented provisional targets. T16 sets retention and recovery controls; T21 sets upload limits before dependent acceptance. Record chosen values and fixture IDs rather than inventing test results. Infrastructure budgets are managed internally; resource usage remains measured and bounded in product behavior.

## Architecture and resolution status

Retain the approved four deployables and eight process roles, with browser automation inside the publishing worker. Reuse the prescribed component kit, generated API client, workflow engine and managed stores. Each task records adopt/configure/build decisions inside that architecture.

| Alternative | Decision and trade-off |
|---|---|
| Manual-only engineering estimates | Replaced by the approved AI-assisted model; requires human review and measured actuals |
| API-gated social publishing | Replaced by owner-authorized browser automation; requires session isolation, UI maintenance and uncertain-outcome reconciliation |
| A reduced Phase 2 release | Rejected by the project owner's full-scope instruction; all 99 tasks remain |
| Replacing the approved topology with a new modular deployment | Not selected for this delivery plan; retaining existing contracts avoids redesign and preserves process isolation, with integration and deployment overhead covered by automation |

Review status **PASS — planning alignment / RESOLVED** means the documents implement these approved directives and assign verification. It does not mean software has shipped, test runs have occurred, or a new independent Goldfish review has been performed.
