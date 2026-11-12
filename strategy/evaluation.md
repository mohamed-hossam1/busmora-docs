# Evaluation Plan & Acceptance Framework

> **Version:** 3.0 (PRD v2.0 alignment) · **Date:** 30 October 2026
> **Status:** Supporting evaluation framework.
> **Authority:** Acceptance scenarios and release gates are governed by **[PRD v2.0 (`core/prd.md`)](../core/prd.md)** §§9–10 (AC-01–25 and AC-27–58 active; AC-26 retired). [SYSTEM-ARCHITECTURE.md](../core/SYSTEM-ARCHITECTURE.md) Appendix A maps every requirement and scenario to its design surface.

---

## 1. Purpose & Guiding Principles

This document defines how BusMora checks whether its product and architecture claims hold. Every claim is verified with evidence from human reviewers, held-out business cases and instrumented system data, not from assertions or simulations.

### Core Principles
1. **Continuous evaluation:** evaluation runs throughout the 24 weeks, combining functional, integration, security and reliability checks with suitable native human review.
2. **Release prerequisite:** acceptance means passing every active acceptance scenario (AC-01–25, AC-27–58) and the PRD §10.2 gates. Paid pilots and a fixed number of external testers are commercial learning, **not graduation prerequisites**.
3. **Truthfulness over optimism:** mocked operations, simulated agent activity and interface-only demos cannot establish completion. Any observed critical violation blocks release until repaired and retested.
4. **Frozen standards:** rubrics, sampling, critical/high defect definitions and task-specific runtime/resource limits are frozen by the end of week 2. Changes need documented rationale and cannot lower a gate to hide a failure.

---

## 2. Methodology & Instrumentation

* **Human reviewer panel:** suitable reviewers score outputs against frozen rubrics in every content register; Gulf-oriented content needs regional review.
* **Paired treatment-vs-control runs:** Business Brain runs are compared with a document-only baseline on identical tasks.
* **~24 authored business cases:** sparse/conflicting evidence, direct requests, owner overrides, capacity and fulfillment limits, different conversion problems, source failures and all four registers.
* **Fixed and live sources:** fixed source snapshots for reproducible grounding tests, plus a small live set for freshness and access.
* **Instrumentation:** Langfuse records per-task usage, latency and prompt versions; product records hold approval transitions, holds, publish attempts and resource reservations.

LLM judging may screen outputs; source inspection and native human review decide material factual, cultural and strategic questions.

---

## 3. The Scenario Library

About **24 authored business cases** drawn from Arabic-first owner-operated service businesses selling through consultations, bookings or inquiries in Egypt and named GCC countries.

### Key Dimensions Covered
* **Contrast pairs:** one material fact changes (unknown vs. measured baseline; no customers vs. repeat customers; many inquiries vs. poor qualification).
* **Practical constraints:** limited assets, permissions, business budget or fulfillment capacity, checking that the Director prioritizes feasible work. There is no standard weekly-owner-hours question (AC-20).
* **Registers:** Egyptian, Gulf-oriented (named country/audience), MSA and English.
* **Uncertainty and source gaps:** sparse websites, unsupported claims and contradictory or malicious sources.
* **Held-out partition:** never used for prompt tuning; coverage reported. Synthetic fixtures are labelled and never counted as customer evidence.
* **Required extra cases:** healthy acquisition but weak repeat demand, and low-volume/long-cycle businesses (PRD §9).

### Scenario Categories
| Category | Core Challenge Tested | Example |
|---|---|---|
| **Intake & first choice** | Crawl-prefilled draft, exact-version confirmation, no-website route | "Import my site, fix one offering, confirm, then choose Chat or Roadmap" |
| **Constraint diagnosis** | Five diagnostic lenses; non-acquisition constraints | "Lots of inquiries on WhatsApp, but almost no bookings" |
| **Roadmap Discovery** | Adaptive questions, Unknown/Skip, exact-question research return | "Build a plan to get my first ten customers; research the price question for me" |
| **Step execution** | Dependencies, Skip/Revisit, owner-reported Done, revision diff | "Skip the landing-page step and show what that blocks" |
| **Multi-source research** | Grounded retrieval, citations, per-task budget | "Analyze competitor pricing and customer complaints" |
| **Brand voice & copywriting** | Authentic text in 4 registers, claim integrity | "Write 3 Facebook posts for Egyptian clinic patients" |
| **Scheduling & Facebook publishing** | Exact-version approval, rescheduling, uncertain outcome | "Approve tomorrow 10:00 Cairo time; then drag it to Thursday" |
| **Requested analysis** | CSV / `.xlsx` mapping, deterministic metrics, unknown ≠ zero | "Here is last month's ad export — what changed?" |
| **Context staleness** | Price change holds only affected Posts | "Change the service price from EGP 5,000 to EGP 8,000" |
| **Unsupported handoff** | Honest instructions and checklist | "Draft WhatsApp follow-up scripts and landing-page copy" |

---

## 4. Arabic Quality & Dialect Rubric

Reviewers score each output on a **1–5 scale**:

| Dimension | What Reviewers Assess | Acceptance Standard |
|---|---|---|
| **Fluency** | Natural, idiomatic language with no translated feel | ≥ 4, none < 3 |
| **Dialect fit** | Matches the requested register and named audience | ≥ 4, none < 3 |
| **Brand fit** | Reflects the confirmed brand voice and positioning | ≥ 4, none < 3 |
| **Cultural fit** | Appropriate regional references and phrasing | ≥ 4, none < 3 |
| **Factual grounding** | Claims match current Brain facts and real cited sources | All audited claims supported or labelled |

### Reviewer Panel Rules
* **Two independent reviewers** score every output; if they differ by more than 1 point on any dimension, **a third breaks the tie**.
* Gulf-oriented content is reviewed by reviewers familiar with the named GCC audience; Egyptian or MSA success never stands in for Gulf quality.

---

## 5. Brain Value Measurement (Treatment vs. Control)

| Configuration | Retrieval & Context |
|---|---|
| **Treatment (Brain)** | Concept-graph retrieval (vector seed + 1–2 hop expansion) + approved procedural rules |
| **Control (Document-Only)** | Raw source chunks without graph extraction or rules |

### Metrics
* **Clarification turns:** informational gaps the system asks the owner to fill.
* **Factual corrections:** inaccuracies the owner must correct.
* **Brand corrections:** tone, voice or dialect corrections required.
* **Hypothesis:** the Brain configuration needs fewer clarifications and corrections than the control. The numeric target is set with the frozen rubric in weeks 1–2 (PRD §10.2), not assumed in advance.

---

## 6. Release Gates (PRD §10.2)

| Area | Gate | Evidence |
|---|---|---|
| **Required behavior** | All active AC-01–25 and AC-27–58 pass under declared conditions | Native publishing, graph, Canvas, research, imports and data controls use real implementations |
| **Truthfulness / control** | Zero critical violations in the release suite | Invented evidence, unauthorized action, budget bypass, cross-workspace disclosure or fake execution blocks release |
| **Usefulness** | ≥ 80% of held-out cases pass without major correction, overall **and within each register** | Counts, denominators, coverage, reviewer basis and small-sample uncertainty reported |
| **Grounding** | All audited consequential claims supported or labelled | Source existence and actual support checked |
| **Publication** | Retry, concurrency, cancellation, revocation, version/context change, late-job and uncertain-result scenarios pass | Real platform and production-owner evidence (AC-30) |
| **Calculations / imports** | Deterministic fixtures match; unknown, zero, duplicate, period, unit/currency and parsing cases pass | Models explain, code calculates |
| **Recovery / security** | No unresolved critical/high defect | Access, credential, deletion, backup/restore, durable-job and rollback checks |
| **Usability** | No blocking defect in Arabic/English critical journeys; ten-minute first-value target measured | Median/distribution of active interaction time, background latency separately |
| **Runtime / resources** | Documented task-specific latency, timeout, concurrency and cost ceilings met | Values set from early slices and verified at a declared release load |

### Acceptance scenario groups
| Group | Scenarios |
|---|---|
| Onboarding, auth and routing | AC-01, AC-02, AC-21, AC-31–34 |
| Brain governance and context | AC-03, AC-15, AC-23, AC-38 |
| Diagnosis, Discovery and Roadmap | AC-04, AC-18–20, AC-24, AC-35–40, AC-45–46, AC-57 |
| Chat, orchestration and Canvas | AC-05–07, AC-17, AC-41–44 |
| Research | AC-08, AC-09, AC-27 |
| Content and language | AC-10, AC-11, AC-22, AC-29 |
| Documents, Scheduling and publishing | AC-12, AC-13, AC-28, AC-30, AC-47–54 |
| Requested analysis | AC-14, AC-25 |
| Honest availability and removed surfaces | AC-16, AC-55, AC-56, AC-58 |

---

## 7. Product Measures & Commercial Learning

$$\text{Confirmed setup} \longrightarrow \text{Chat or Roadmap} \longrightarrow \text{Requested Task} \longrightarrow \text{Accepted output} \longrightarrow \text{Recorded execution or attestation}$$

* Also measure return use, correction effort, clarification burden, failed intake, uncertain publication and actual cost per accepted output or Task. There is no review-cycle funnel or periodic performance dashboard.
* Compare equivalent work with owners' existing tools using the same facts, and report denominators, assistance and active time separately from background latency.
* **Commercial hypotheses** (PRD §12.2): whether owners need direction more than content volume, whether the service cohort shares a useful workflow, and whether repeated value earns willingness to pay. Small samples cannot establish regional demand, retention or causal growth, and none of this changes the technical release contract.
