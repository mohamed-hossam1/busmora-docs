# G. Requirements Coverage Matrix

> Part of the [BUSMORA 16-week implementation plan](./README.md). Requirement text: [PRD v2.0](../core/prd.md) §§5, 8, 9. The former overflow items (OV-nn) and the tasks that replaced them: [H](./09-risks-overflow-decisions.md).
> This maps planned work, not executed tests. Nothing here has been built or passed.

## G.1 Status definitions

| Status | Meaning |
|---|---|
| **Full** | Every part of the requirement is covered by scheduled tasks (the 64 primary tasks and the additional tasks T65–T99). It can pass by Week 16 if those tasks meet their acceptance criteria. |
| **Partial** | Some parts are scheduled and named parts are not. **No row has this status since 2026-11-14.** |
| **Unplanned** | No scheduled task. **No row has this status since 2026-11-14.** |
| **Full — externally gated** | All engineering is scheduled, but passing needs an approval the team does not control. |

## G.2 Summary

| Set | Active | Full | Partial | Unplanned | Externally gated |
|---|---:|---:|---:|---:|---:|
| Functional requirements (FR-01–58, FR-47 retired) | 57 | 56 | 0 | 0 | 1 |
| Non-functional requirements (NFR-01–09) | 9 | 9 | 0 | 0 | 0 |
| Acceptance scenarios (AC-01–25, AC-27–58; AC-26 retired) | 57 | 56 | 0 | 0 | 1 |

PRD §10.2 requires **all** active acceptance scenarios to pass. **All 57 are now fully planned**: 56 by scheduled tasks alone, and AC-30 by scheduled tasks plus Meta's approval, which the team does not control. The plan therefore targets a release-ready Phase 2 at the end of Week 16, with Meta's review as the one external dependency. "Fully planned" means every part has a task with acceptance criteria; it is not a record that anything has been built or has passed.

**Change on 2026-11-14.** By the plan owner's instruction, every item of the former overflow register was moved into the schedule as additional tasks T65–T99, with hours treated as unlimited. The last column of each table below shows which former overflow item a row depended on. The ratings rest on 1,950 scheduled hours (about 30.5 per person per week on average, up to 45 in Week 11).

## G.3 Functional requirements

| Req | Title | Status | Scheduled tasks | Verification evidence | Former overflow |
|---|---|---|---|---|---|
| FR-01 | Accounts and workspace | Full | T09, T13, T15 | AC-31 demonstration; cross-workspace check (T15, T53) | — |
| FR-02 | Navigation | Full | T02, T26, T51, T54 | Six routes; sign-in routing by onboarding state; no last-route restore (T26); deep links (T51) | — |
| FR-03 | Website import | Full | T28, T32 | Three real sites: normal, sparse, unreachable | — |
| FR-04 | Confirmed intake | Full | T26, T32 | Nothing retrievable before exact-version confirmation | — |
| FR-05 | Completion choice | Full | T26 | Chat and Create My Roadmap offered after confirmation | — |
| FR-06 | Unified essential business form | Full | T26, T32 | Same form for both paths; four pricing models | — |
| FR-07 | Social import visibility | Full | T26, T59 | Disabled, labelled Coming Soon, cannot start | — |
| FR-08 | Structured memory | Full | T17, T24, T50, T90, T77 | Nine categories, relationships, one record set | — (was OV-06, OV-02) |
| FR-09 | Epistemic and lifecycle state | Full | T17, T46, T50 | Status, provenance, conflicts and unknowns shown | — |
| FR-10 | Update rules | Full | T26, T46, T50, T66 | Activation only after confirmation; individual proposal decisions; an edit marks dependent work | — |
| FR-11 | Historical work | Full | T18, T20, T24, T25, T86 | Conversations and documents retrievable; Brain context reused | — (was OV-12) |
| FR-12 | Interactive graph | Full | T17, T50, T77 | Accessible List with full correction | — (was OV-02) |
| FR-13 | Owner data controls | Full | T19, T50, T52, T65, T67, T69 | Inspect, edit, remove, disconnect; search of derived stores after removal; export opened outside the product; restore drill | — |
| FR-14 | Business-aware diagnosis | Full | T48, T37, T56, T91 | Four cases including a contrast pair; reviewer scores | — (was OV-08) |
| FR-15 | Marketing scope | Full | T25, T48 | Out-of-scope request handled within marketing scope | — |
| FR-16 | Complete actionable Roadmap | Full | T37, T41, T45, T88, T89 | Complete plan; one active; real dependencies | — (was OV-17) |
| FR-17 | Direct Chat | Full | T18, T22, T25, T29, T79, T83 | Work launched with no Roadmap | — (was OV-16) |
| FR-18 | Choice-first clarification | Full | T10, T40 | AC-35 demonstration | — |
| FR-19 | Fast Router | Full | T18 | Simple message answered directly; latency measured | — |
| FR-20 | Dynamic Orchestrator | Full | T22, T25, T42 | Varying plans; ceilings enforced outside the model | — |
| FR-21 | Specialist capabilities | Full | T25, T36, T48, T35, T72, T75 | Research, analysis, planning/copy; controlled publishing | — (was OV-19) |
| FR-22 | Authentic conversation-local Canvas | Full | T33, T40, T80 | Recorded events only; list alternative; exact form link | — (was OV-07) |
| FR-23 | Durable work and retry presentation | Full | T22, T33, T57 | Survives interruption; retries shown as Processing; truthful terminal states | — |
| FR-24 | Dynamic multi-source research | Full | T36, T44 | Distinct source types; stated stopping reason | — |
| FR-25 | Research output | Full | T36, T44 | Report links question, evidence, limits, next task | — |
| FR-26 | Evidence quality | Full | T44, T56 | Planted unsupported claim and contradiction; grounding audit | — |
| FR-27 | Per-task research budget | Full | T42 | AC-09 with concurrent child work | — |
| FR-28 | Modular research capability | Full | T04, T36 | Provider baseline; separable planning, retrieval, synthesis | — |
| FR-29 | Flexible output size | Full | T29, T74, T76 | Single post without forced campaign; documents | — (was OV-10) |
| FR-30 | Brand understanding and claim integrity | Full | T32, T29, T74, T76 | Brand cues from website; candidates for interpretations | — (was OV-10) |
| FR-31 | Text-first assets | Full | T21, T29, T47 | Visual briefs as documents; invalid media blocks readiness | — |
| FR-32 | Language and audience fit | Full | T08, T29, T56, T95 | Per-request register; one review round | — (was OV-11) |
| FR-33 | Connected Facebook Page | Full — externally gated | T03, T19, T35, T38, T47, T63 | Tester-mode publication (T35, T47); ordinary-owner evidence (T63) | Needs Meta's approval |
| FR-34 | Review and approval | Full | T31, T47, T82 | Exact-version individual approval; invalidation | — (was OV-14) |
| FR-35 | Scheduled execution | Full | T35, T43, T68, T55 | Recheck of version, destination, authorization, cancellation, window and context-review hold before dispatch; late posts held | — |
| FR-36 | Verification and recovery | Full | T39, T43, T55 | Induced faults on a real Page | — |
| FR-37 | Other external actions | Full | T59 | Inventory check: no outreach, spend or site-change capability exists | — |
| FR-38 | Requested file analysis | Full | T72, T75 | Demonstrated by the named tasks; evidence run in T96 and T98 | — (was OV-19) |
| FR-39 | Goal-based interpretation | Full | T72, T75 | Demonstrated by the named tasks; evidence run in T96 and T98 | — (was OV-19) |
| FR-40 | Evidence governance (analysis) | Full | T72, T75 | Demonstrated by the named tasks; evidence run in T96 and T98 | — (was OV-19) |
| FR-41 | Document Deliverables | Full | T20, T29, T52, T86 | Versions, search, filters, text/Markdown export, removal | — (was OV-12) |
| FR-42 | Context reuse | Full | T24, T25, T46, T50, T65, T66 | Rejected, removed and replaced items never used; expired facts shown as expired | — |
| FR-43 | Full localization and responsive access | Full | T02, T10, every owner-facing task, T54, T92 | Built journeys in both languages on a phone | — (was OV-21) |
| FR-44 | Independent language choices | Full | T02, T18, T29, T31, T73 | Interface, conversation and content language differ in one session; explicit time zone | — (was OV-20) |
| FR-45 | Goal and practical constraints | Full | T30, T34 | Objective, route and constraints recorded; no weekly-hours question | — |
| FR-46 | Executable steps | Full | T37, T41, T45, T49 | Every step field present; work, platform and owner states independent | — |
| FR-47 | Retired | — | — | Not a release obligation | — |
| FR-48 | Context impact and freshness | Full | T66, T68, T55 | Price change marks the work that used it and holds an approved Post | — |
| FR-49 | Honest capability and handoff contract | Full | T37, T45, T49, T59, T64, T85 | Owner-execution steps with checklist; owner-reported label | — (was OV-09) |
| FR-50 | Authentication | Full | T09, T13, T70 | Email link, reset, session rotation | — (was OV-01) |
| FR-51 | Roadmap Discovery | Full | T30, T34, T79, T83, T84 | AC-36 demonstration | — (was OV-16, OV-18) |
| FR-52 | Exact-question research | Full | T79, T83 | Demonstrated by the named tasks; evidence run in T96 and T98 | — (was OV-16) |
| FR-53 | Step control | Full | T22, T45, T49 | AC-39, AC-40 demonstrations | — |
| FR-54 | Plan lifecycle and revision | Full | T37, T41, T88, T89 | Drafts, proposal, one active plan, archive | — (was OV-17) |
| FR-55 | Scheduling | Full | T27, T31, T35, T78, T81, T82, T87 | List and Unscheduled; versioned editor; form reschedule | — (was OV-13, OV-14, OV-15) |
| FR-56 | Settings and notifications | Full | T19, T43, T51, T73, T70, T88, T89 | Connection states; Notification Bell | — (was OV-20, OV-01, OV-17) |
| FR-57 | Output ownership | Full | T27, T29, T74, T76, T79, T83 | Posts only in Scheduling; documents only in Deliverables | — (was OV-10, OV-16) |
| FR-58 | Removed surface exclusion | Full | T02, T59 | Inventory check of routes, interfaces, schemas, jobs | — |

## G.4 Non-functional requirements

| Req | Title | Status | Scheduled tasks | Verification evidence | Former overflow |
|---|---|---|---|---|---|
| NFR-01 | Truthful state | Full | T22, T33, T35, T39, T49, T59, T77, T78, T81 | States derive from recorded events in every built view | — (was OV-02, OV-13) |
| NFR-02 | Source integrity | Full | T32, T36, T44, T56 | Citation support check; grounding audit | — |
| NFR-03 | Workspace security | Full | T09, T13, T15, T19, T53 | Cross-workspace check in every store and module | — |
| NFR-04 | Untrusted input handling | Full | T20, T25, T28, T32, T44, T53, T72, T75 | Hostile sources; internal-fetch refusal; safe document display; formula-safe export (T67) | — (was OV-19) |
| NFR-05 | Durable execution and recovery | Full | T06, T16, T22, T39, T43, T55, T57, T60, T69 | Interruption, publishing faults, restore and rollback rehearsed; restore respects removals | — |
| NFR-06 | Resource accountability | Full | T14, T18, T42, T58 | Usage per task and workspace; ceilings measured at declared load | — |
| NFR-07 | Accessible bilingual operation | Full | T02, T10, T31, T54, T56, T92, T95 | Built journeys; one review round across registers | — (was OV-21, OV-11) |
| NFR-08 | Observability | Full | T11, T14, T64 | Request traced; runbooks tested by a second person | — |
| NFR-09 | Data control | Full | T16, T50, T52, T60, T64, T65, T67, T69 | Policy; removal with derived clean-up; export; restore suppression; disconnection | — |

## G.5 Acceptance scenarios

| AC | Scenario | Status | Scheduled tasks | Verification evidence | Former overflow |
|---|---|---|---|---|---|
| AC-01 | Website onboarding | Full | T26, T28, T32 | Real site import to confirmed draft | — |
| AC-02 | Manual onboarding | Full | T26 | Essential fields only; not-ready route | — |
| AC-03 | Brain governance | Full | T17, T46, T50, T65, T67, T69, T77 | List exploration; approve, edit, reject; full removal; export | — (was OV-02) |
| AC-04 | Diagnosis and Roadmap | Full | T30, T34, T37, T41, T48, T88, T89 | Complete plan; activation by approval | — (was OV-17) |
| AC-05 | Direct work | Full | T25, T29 | Task from Chat with no Roadmap, using Brain context | — |
| AC-06 | Dynamic orchestration | Full | T18, T25, T42 | Contrasting requests; ceilings; no AI call for ordinary controls | — |
| AC-07 | Authentic Canvas | Full | T33, T40 | Recorded events only; exact form link | — |
| AC-08 | Multi-source research | Full | T36, T44 | Fixed-source run with planted faults | — |
| AC-09 | Research budget | Full | T42 | Concurrent child work; declined extension | — |
| AC-10 | Campaign creation | Full | T29, T74, T76 | Single item with register and visual brief | — (was OV-10) |
| AC-11 | Language experience | Full | T02, T29, T54, T56, T92, T73 | Built journeys in both languages; per-request register | — (was OV-21, OV-20) |
| AC-12 | Facebook publishing | Full | T31, T35, T39, T47, T55, T82 | Both formats after individual approval; no duplicate | — (was OV-14) |
| AC-13 | Approval invalidation | Full | T31, T55 | Each approval-critical field | — |
| AC-14 | Requested file analysis | Full | T72, T75 | Demonstrated by the named tasks; evidence run in T96 and T98 | — (was OV-19) |
| AC-15 | History and control | Full | T19, T52, T65, T67, T86 | Retrieve, remove, export, disconnect | — (was OV-12) |
| AC-16 | Honest availability | Full | T26, T59 | Disabled Social Import; inventory of later-phase features | — |
| AC-17 | Operational readiness | Full | T53, T55, T57, T59, T60, T94, T96, T97, T98, T99 | Checks pass for built workflows | — (was OV-23) |
| AC-18 | New service business, unknown audience | Full | T37, T48, T56 | Case run; no invented figures | — |
| AC-19 | Many inquiries, few qualified | Full | T48, T56 | Case run; qualification examined first | — |
| AC-20 | Material practical constraints | Full | T30, T34, T37 | Constraints respected; no weekly-hours question | — |
| AC-21 | No website, working messaging route | Full | T26, T37 | Route informs the plan; no site required | — |
| AC-22 | One scoped Arabic Post | Full | T29 | One Post in Scheduling, no duplicate document | — |
| AC-23 | Offer/price changes after approval | Full | T66, T68, T55 | Price change after approval holds the Post on a tester-mode Page | — |
| AC-24 | Best task needs an unsupported connector | Full | T37, T45, T49, T85 | Owner-execution step with instructions and checklist | — (was OV-09) |
| AC-25 | Unknown outcomes, zero denominator | Full | T72, T75 | Demonstrated by the named tasks; evidence run in T96 and T98 | — (was OV-19) |
| AC-26 | Retired | — | — | Not a release test | — |
| AC-27 | Contradictory or malicious source | Full | T28, T32, T44, T53 | Hostile source set; internal-fetch refusal | — |
| AC-28 | Timeout, revocation, cancel race, overdue | Full | T39, T43, T55 | Induced faults on a real Page | — |
| AC-29 | Mobile mixed-script review and approval | Full | T31, T54, T73 | Approval preview on a phone in both languages | — (was OV-20) |
| AC-30 | Intended ordinary owner connects and publishes | Full — externally gated | T03, T38, T60, T63 | Ordinary-owner publication on production, or blocked record | Needs Meta's approval and a real owner |
| AC-31 | Email verification link | Full | T09 | Replayed and expired link | — |
| AC-32 | Google identity | Full | T70 | Demonstrated by the named tasks; evidence run in T96 and T98 | — (was OV-01) |
| AC-33 | Optional offering pricing | Full | T26 | Four pricing models; missing is not zero | — |
| AC-34 | Completion and return routing | Full | T26, T51 | Routing by onboarding state; deep links; no last-route restore | — |
| AC-35 | Choice-first forms | Full | T10, T40 | Options from Brain facts; Unknown/Skip/N-A | — |
| AC-36 | Wizard persistence | Full | T30, T34 | Close and reopen with adaptive branching; two-tab conflict | — |
| AC-37 | Question research and return | Full | T79, T83 | Demonstrated by the named tasks; evidence run in T96 and T98 | — (was OV-16) |
| AC-38 | Knowledge reuse | Full | T34, T84 | Confirmed Brain facts reused in Discovery | — (was OV-18) |
| AC-39 | Execute step | Full | T45 | Double click creates one Task | — |
| AC-40 | Skip and Revisit | Full | T49 | Skip on a running step; dependents explained | — |
| AC-41 | Task capacity across conversations | Full | T22, T40, T45, T57 | Third Task waits; atomic admission under load | — |
| AC-42 | Conversation Canvas switching | Full | T33, T80 | Canvas follows the selected conversation | — (was OV-07) |
| AC-43 | Recoverable retry UX | Full | T22, T33, T51, T57 | Retry stays Processing with no failure flash, counter or notification | — |
| AC-44 | Terminal failure | Full | T22, T33, T57 | Truthful failed or partial state with action | — |
| AC-45 | Strategic revision | Full | T88, T89 | Demonstrated by the named tasks; evidence run in T96 and T98 | — (was OV-17) |
| AC-46 | One active Roadmap | Full | T41 | Simultaneous approvals | — |
| AC-47 | Document lineage | Full | T20, T29, T52, T66 | New immutable version on edit, with sources; possibly-outdated mark | — |
| AC-48 | Manual New Post | Full | T27, T31 | Draft without a Page; approval needs connection | — |
| AC-49 | Unscheduled drafts | Full | T27 | No date means Unscheduled only | — |
| AC-50 | Calendar views | Full | T27, T78, T81 | List view | — (was OV-13) |
| AC-51 | Bulk approval | Full | T82 | Demonstrated by the named tasks; evidence run in T96 and T98 | — (was OV-14) |
| AC-52 | Rescheduling | Full | T31, T78, T81 | Form reschedule creates a version and clears approval; zone change does not move the instant | — (was OV-13) |
| AC-53 | Historical publication | Full | T35, T87 | Published Post shown at its actual time | — (was OV-15) |
| AC-54 | Connection recovery | Full | T19, T43, T55 | Disconnect blocks; reconnect releases nothing | — |
| AC-55 | Contextual notifications | Full | T51, T88, T89 | Every built notification kind; unavailable subject | — (was OV-17) |
| AC-56 | Removed route absence | Full | T02, T59 | Inventory check | — |
| AC-57 | Step model absence checks | Full | T37, T41, T59 | Inventory check | — |
| AC-58 | Deferred Meta Ads | Full | T59 | No Ads connector, tool or spend permission | — |

## G.6 Other commitments

| Commitment | Source | Status | Scheduled tasks | Former overflow |
|---|---|---|---|---|
| About 24 authored cases with held-out set | PRD §10.1 | Full | T07 (4), T23 (10), T71, T93 | — (was OV-22) |
| Rubric, severity, limits frozen by Week 2 | PRD §10.2 | Full | T07 | — |
| ≥80% held-out pass, overall and per register | PRD §10.2 | Full | T56 (one round, 14 cases), T95, T71, T93 | — (was OV-11, OV-22) |
| Zero critical truthfulness/control violations | PRD §10.2 | Full | T53, T55, T57, T61, T62, T94, T96, T97, T98, T99 | — (was OV-23) |
| Brain versus document-only comparison | D-23, D-38 | Full | T71, T93 | — (was OV-22) |
| Ten to twelve curated recipes | PRD §5.13 | Full | T48 (about six, simplification authorized by §13.3), T91 | — (was OV-08) |
| Runtime and cost ceilings at declared load | PRD §10.2 | Full | T07, T58 | — |
| Backup/restore and rollback exercised | NFR-05 | Full | T16, T60 | — |
| Release ledger for every FR/NFR/AC | PRD §13.3 | Full | T59 | — |
| Data governance design by Week 4 | PRD §11.1 | Full | T16 | — |
| Facebook path recorded by Week 2 | PRD §11.1 | Full | T03 | — |
| Research provider baseline by Week 2 | PRD §11.1 | Full | T04 | — |

**Reading the task columns.** "Scheduled tasks" lists the tasks whose acceptance carries the requirement. A task's own "Requirement references" may also cite requirements it only supports; the two lists are not expected to be identical.

## G.7 What "Full" does and does not mean

"Full" means the work is planned at an honest size. It does not mean the requirement will pass. Fourteen baseline tasks are low confidence ([A.6](./01-assumptions-and-feasibility.md)); a requirement that depends on one of them is exposed to its overrun. Requirements whose quality depends on model behaviour (FR-14, FR-24–26, FR-32) can be fully planned and still fail human review in T56.
