# A. Planning Assumptions and Feasibility Assessment

> Part of the [BUSMORA implementation plan](./README.md). Authority: [approved execution contract](./11-execution-contract.md), 2026-11-15.

## A.1 Verdict

**PASS — full Phase 1 and Phase 2 delivery is feasible and committed within 16 weeks under the approved AI-assisted model.** All 99 tasks remain scheduled; unscheduled overflow is zero. Publishing uses client-authorized browser sessions, and weekly staging, dogfooding and regression cover the full delivery period.

The 1,950-hour baseline measures traditional manual effort. The project owner confirms that the team's 3×–5× AI acceleration is benchmarked and proven. This audit accepts that confirmation; it does not independently reproduce the benchmark. The 16-week full-scope schedule is validated.

## A.2 Capacity calculation

| Work | Tasks | Manual-equivalent hours | Human hours at 5× | Human hours at 3× |
|---|---:|---:|---:|---:|
| Primary tasks | 64 | 1,280 | 256 | 426.7 |
| Additional tasks T65–T99 | 35 | 670 | 134 | 223.3 |
| Total task baseline | 99 | 1,950 | 390 | 650 |
| Additional DoD allowance, up to 3 hours per task | Included in existing tasks | 297 | 59.4 | 99 |
| Weekly dogfooding, review and coordination allowance | No new tasks | 128 human hours, uncompressed | 128 | 128 |
| **Total human planning envelope** | **99** | — | **577.4** | **877** |

Four people × 16 weeks × 20 human hours gives a 1,280-hour staffing allocation. The envelope leaves 403–702.6 human hours for integration, repair and rework within the same weeks. The two-hour weekly human allowance per person is separate from compressed engineering effort; machine runtime and platform response time are tracked separately.

The baseline alone averages 6.1–10.2 human hours per person per week. Week 11's 180 manual-equivalent hours become 36–60 team human hours. Assignee A's 68-hour baseline allocation becomes 13.6–22.7 human hours before allowances; the integration lead redistributes work and reviews within that week. A 2,600-manual-hour sensitivity case becomes 520–866.7 human hours; including the same allowances yields 707.4–1,093.7 hours. These calculations support capacity planning without interpreting manual estimates as required overtime.

## A.3 Estimation method and ownership

| Delivery block | Tasks | Manual-equivalent hours |
|---|---:|---:|
| Phase 1, Weeks 1–6 | 24 | 480 |
| Phase 2 build, Weeks 7–13 | 55 | 1,040 |
| Expanded verification and stabilization, Weeks 14–16 | 20 | 430 |
| **Total** | **99** | **1,950** |

Task metadata preserves baseline estimates, ranges and technical confidence. Apply the approved multiplier to derive human engineering effort, add the explicit allowances, then assign agent work, a human owner, a peer reviewer and a staging slot. Record actual human time, agent usage, repair time and evidence each week.

The earlier task estimates did not uniformly include the later-added cross-cutting DoD. The separate 297-hour manual-equivalent allowance resolves that accounting gap without rewriting or hiding the 1,950-hour baseline. It is allocated inside the same 99 tasks.

## A.4 Delivery architecture and full scope

Retain the approved repositories, four deployables, eight process roles, managed stores, generated contracts and bilingual component base. Replace social API dispatch with the isolated browser publishing worker defined in [file 11](./11-execution-contract.md#publishing-through-authenticated-browser-sessions).

Incremental sequencing does not reduce delivery scope: the Brain List precedes its graph; List/Unscheduled precede calendar views; the first recipes precede the remainder. T65–T99 complete every formerly listed overflow item. All are committed within the same 16 weeks.

## A.4.1 Timing and integration

Bootstrap staging exists in Week 1 through T01/T02; T06 extends it in Week 2 and T12 hardens it in Week 3. Quality and value checks start with these increments and continue as features arrive. T56/T93/T95 expand and consolidate earlier evidence.

Embedding selection remains in T24, object-storage selection in T12, and the complete component kit in T10. Earlier model/context experiments use fixed fixtures and do not claim these later components already exist.

## A.5 Approved operating assumptions

| ID | Assumption | Delivery mechanism |
|---|---|---|
| EA-1 | Benchmarked AI agents and tooling provide 3×–5× compression of manual effort | Use 3× for allocation; record execution actuals without reopening validated feasibility |
| EA-2 | Four people deliver through settled domain/module boundaries | Automated PR linting, named schema ownership and peer review are established controls, not collaboration risks |
| EA-3 | Approved process topology remains | Reuse established components; isolate browser execution in workers-publish |
| EA-4 | Accounts, keys and infrastructure are provisioned progressively | Internal ops supplies each milestone's inputs during its week; no front-loaded setup gate |
| EA-5 | Publishing uses Scrapling and Playwright with client-owned encrypted sessions | T03, T19, T35, T38, T39, T43, T47, T55 and T63 verify adaptive matching, deduplication, dashboard recovery and read-back |
| EA-6 | Register quality improves weekly | Team dogfooding plus rubric-based checks; optional external feedback does not delay core engineering |
| EA-7 | Regression and repair happen throughout delivery | Per-change checks, weekly suites, explicit final-build certification |
| EA-8 | Full Phase 1/2 scope remains fixed | No feature removed, swapped or deferred; all 99 IDs retained |
| EA-9 | Integration and completion handoffs are explicit | Assigned merge/review ownership and final T61/T62/T99 → T98 certification |

## A.6 Estimate confidence

Confidence labels measure technical uncertainty, not feasibility of manual typing. Session recovery, uncertain-publication reconciliation, orchestration and model behavior retain wider ranges and receive early exercises and frequent regression. Added-task estimates remain traceable to their original ranges.

Weekly actuals account for implementation, review and use of the repair allowance. The proven multiplier and settled collaboration model are not unresolved feasibility risks. Technical defect repair remains inside the committed scope and schedule; incomplete work is never marked complete.

## A.7 Technical uncertainties and controls

- Browser UI changes: Scrapling adaptive matching, Playwright semantic/action checks, versioned fixtures and safe holds on uncertain matches.
- Session expiry/challenges: owner reauthentication, encrypted storage and safe holds.
- Unknown publication result: reconciliation by durable identity/read-back; no blind resend.
- Runtime boundary defects: typed HTTP/activity contracts, durable command reconciliation, isolation checks and cumulative regression.
- Model grounding/register variance: weekly fixtures, comparative runs and tuning, with held-out results preserved.
- Runtime and resource limits: T07, T16 and T21 record values before dependent acceptance; weekly measurements precede T58 confirmation.

Company registration, internal budget administration and optional outside dialect reviewers are parallel internal activities, not project-blocking prerequisites. Detailed responses and owners appear in [file 09](./09-risks-overflow-decisions.md).
