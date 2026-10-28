# Evaluation Plan & Acceptance Framework

> **Version:** 2.0 (PRD v1.1 Alignment) · **Date:** 28 October 2026  
> **Status:** Canonical Quality & Verification Contract — Aligned with [PRD v1.1 (`core/prd.md`)](../core/prd.md) §9–§10.

---

## 1. Purpose & Guiding Principles

This document defines how BusMora evaluates whether its core product and architecture claims hold. Every claim is verified with empirical evidence gathered from human reviewers, held-out business cases, and instrumented system data—not from assertions or unverified simulations.

### Core Principles
1. **Continuous Evaluation:** Evaluation runs throughout the 24-week development lifecycle, combining functional, integration, security, and reliability checks with native human review.
2. **Graduation & Release Prerequisite:** Acceptance is determined by passing all 30 release scenarios (AC-01 through AC-30) and meeting strict quality/safety gates. Paid pilots and a fixed number of external testers are commercial learning opportunities, **not graduation prerequisites**.
3. **Truthfulness Over Optimism:** Mocked operations, simulated agent activity, and interface-only demonstrations cannot establish capability completion. Any observed critical violation blocks release until repaired.

---

## 2. Methodology & Instrumentation

Evaluation combines four rigorous methodologies:
* **Human Reviewer Panel:** Native Arabic reviewers score outputs against published rubrics across all supported dialects.
* **Paired Treatment-vs-Control Runs:** Grounded Business Brain runs are evaluated against a document-only baseline on identical tasks.
* **24 Authored Business Cases:** A comprehensive test suite spanning sparse/conflicting data, direct requests, owner overrides, capacity limits, different conversion bottlenecks, and all 4 content registers.
* **Platform Instrumentation & Tracing:** Langfuse captures per-task token usage, model latency, and prompt versions; product databases record approval transitions, staleness holds, and publish attempts.

---

## 3. The 24-Case Scenario Library

The evaluation suite includes approximately **24 authored business cases** drawn from Arabic-first SMBs and owner-operated services selling via consultations, bookings, or inquiries in Egypt and the GCC.

### Key Dimensions Covered
* **Contrast Pairs:** Scenarios designed in pairs where a single material variable changes (e.g. unknown vs measured baseline; zero customers vs repeat customer bottleneck; high inquiry volume vs poor qualification).
* **Fulfillment & Capacity Constraints:** Tests where owner capacity is strictly bounded (e.g. 2 owner hours/week, no ad budget), verifying that the Director prioritizes feasible actions rather than generic volume.
* **Dialect & Register Variations:** Distinct cases covering Egyptian Conversational, Gulf-oriented (Saudi/UAE lifestyle), Modern Standard Arabic (MSA), and English.
* **Uncertainty & Source Gaps:** Scenarios with incomplete websites, unverified claims, or conflicting public sources, testing honest partial results and refusal to hallucinate facts.
* **Held-Out Test Set:** A dedicated partition of cases remains strictly unused during prompt tuning to prevent overfitting.

### Scenario Categories
| Category | Core Challenge Tested | Example |
|---|---|---|
| **Intake & First Direction** | Unverified site crawl; unknown audience/baseline | "Extract company knowledge from URL and recommend initial focus" |
| **Constraint Diagnosis** | 5 STAR lenses; identifying non-acquisition bottlenecks | "Lots of inquiries on WhatsApp, but almost zero conversions" |
| **Multi-Source Research** | Grounded retrieval, citations, per-task budget enforcer | "Analyze competitor pricing and customer complaints on Reddit/Web" |
| **Brand Voice & Copywriting** | Culturally authentic text in 4 registers | "Write 3 Facebook posts targeting Egyptian clinic patients" |
| **Capacity-Bounded Planning** | Now/Next/Later roadmap fitting 2 hours/week | "Build next week's plan with zero ad spend and limited time" |
| **Facebook Publishing** | Exact-version approval, timezone scheduling, read-back | "Approve and schedule post for tomorrow 10:00 AM Cairo time" |
| **Performance Learning** | CSV and Excel (`.xlsx`) validation, deterministic KPIs | "Upload monthly lead sheet and diagnose CAC/conversion drop" |
| **Context Staleness** | Core offer change triggers post hold | "Change service price from \$50 to \$70; verify scheduled posts hold" |
| **Unsupported External Handoff** | Offline / live-site work with honest checklists | "Draft WhatsApp response scripts and landing page copy briefs" |

---

## 4. Arabic Quality & Dialect Rubric

Reviewers score each deliverable on a **1–5 scale** across five dimensions:

| Dimension | What Reviewers Assess | Acceptance Standard |
|---|---|---|
| **Fluency** | Reads as natural, idiomatic Arabic with no translated feel | ≥ 4, none < 3 |
| **Dialect Fit** | Accurately matches requested register (Egyptian, Gulf, MSA, English) | ≥ 4, none < 3 |
| **Brand Fit** | Consistently reflects approved brand voice, tone, and positioning | ≥ 4, none < 3 |
| **Cultural Fit** | Appropriately uses regional idioms, references, and phrasing | ≥ 4, none < 3 |
| **Factual Grounding** | Claims match Business Brain facts and authentic cited sources | 100% verifiable |

### Reviewer Panel Rules
* **Two independent native Arabic reviewers** score every deliverable in an evaluation round.
* If reviewers differ by > 1 point on any dimension, a **third reviewer breaks the tie**.
* Gulf-oriented content must be evaluated by reviewers familiar with GCC vernacular; Egyptian or MSA success cannot be extrapolated to Gulf quality.

---

## 5. Brain Value Measurement (Treatment vs. Control)

To measure the empirical contribution of the Business Brain, tasks are executed in two blind configurations:

| Configuration | Retrieval & Context Mechanism |
|---|---|
| **Treatment (Brain)** | Relational concept graph retrieval (vector seed + 1–2 hop expansion) + procedural rules. |
| **Control (Document-Only)** | Raw source document chunk retrieval without relational graph extraction or rules. |

### Evaluation Metrics
* **Clarification Turns:** Number of informational gaps the system asks the owner to fill.
* **Factual Corrections:** Number of factual inaccuracies the owner must correct.
* **Brand Corrections:** Frequency of tone, voice, or dialect corrections required.
* **Core Hypothesis:** The grounded Business Brain configuration produces **≥ 50% fewer clarification turns and factual/brand corrections** than the document-only control.

---

## 6. Binding Phase 2 Release Gates (AC-01 through AC-30)

Release acceptance requires passing the complete suite of **30 Acceptance Criteria (AC-01 to AC-30)** under declared release load:

| Area | Release Gate / Acceptance Scenario | Observable Pass Condition |
|---|---|---|
| **Onboarding & First Value** | **AC-01, AC-02, AC-21** | Website URL crawl seeds immediately usable imported context; guided setup handles unknown fields; messaging route supported without forcing a site. |
| **First Value Usability** | **PRD §10 Target** | Median active owner interaction time from signup to first usable artifact is **≤ 10 minutes** for supported cases. |
| **Brain Governance** | **AC-03, AC-15, AC-23** | Graph exploration, epistemic separation (imported vs confirmed vs hypothesis), deletion/export, and context staleness holds on material changes. |
| **Diagnosis & Roadmap** | **AC-04, AC-18, AC-19, AC-20** | STAR lenses diagnose primary constraint; Now/Next/Later roadmap reflects real capacity; alternatives explained; direct requests scoped (AC-05). |
| **Dynamic Orchestration** | **AC-06, AC-07, AC-17** | Fast Router avoids fabricated agent work; Dynamic Orchestrator adapts plans; authentic Canvas displays recorded events; durable task recovery. |
| **Multi-Source Research** | **AC-08, AC-09, AC-27** | Genuine retrieval across distinct public sources; citations verified; per-task budget strictly enforced; extensions require explicit approval. |
| **Campaigns & Dialects** | **AC-10, AC-11, AC-22** | Text-first assets and visual briefs across 4 registers; full Arabic RTL / English LTR responsive UI with independent language settings. |
| **Facebook Publishing** | **AC-12, AC-13, AC-28, AC-30** | Individual and batch approvals bind to exact versions; scheduled dispatch with lateness allowance; idempotent execution; read-back verification; AC-30 ordinary-owner production configuration verified. |
| **Performance Learning** | **AC-14, AC-25, AC-26** | CSV and Excel (`.xlsx`) validation previews; deterministic calculations; aggregate manual outcome capture; recurring review with honest inconclusive states. |
| **Truthfulness & Boundaries** | **AC-16, AC-24, AC-27** | Zero critical violations: no hallucinated evidence, no fake progress, no unauthorized actions; Social Profile Import visibly disabled as Coming Soon. |

---

## 7. Product Measures & Commercial Learning

The usage and learning funnel is tracked systematically across cycles:
$$\text{Setup} \longrightarrow \text{First Direction} \longrightarrow \text{Accepted Task} \longrightarrow \text{Accepted Artifact} \longrightarrow \text{Recorded Outcome} \longrightarrow \text{Completed Review}$$

* **Completed Review Cycle:** An accepted task, recorded completion/attestation, aggregate observations (or explicit no-data state), and an owner-reviewed next decision.
* **Commercial Hypotheses:** Track whether owners return for subsequent cycles, self-reported time saved (target 5–10 hours/week), and willingness to pay. These observations inform post-graduation commercialization but do not alter the technical release contract.
