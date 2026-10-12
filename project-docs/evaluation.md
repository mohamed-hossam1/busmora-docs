# Evaluation Plan

> How BusMora measures whether its core claims hold. Every claim is verified with evidence gathered from human reviewers and instrumented product data — not from assertions.

---

## 1. Purpose

BusMora makes three central product claims. This document defines how each is tested:

1. **Arabic quality** — output reads as natively written, not translated.
2. **Brain value** — grounding in the Business Brain produces better results than raw document retrieval.
3. **Execution reliability** — approved content is published correctly and verifiably.

---

## 2. Methodology Overview

Evaluation combines:

- **Human review sessions** — reviewers score outputs against published rubrics.
- **Paired comparisons** — grounded Brain results are compared against a document-only baseline on the same tasks.
- **Instrumented product data** — approval, edit, correction, and publishing records collected by the platform.
- **LLM tracing** — per-task cost, latency, and model behavior captured in Langfuse.

The evaluation is conducted by humans on a defined scenario set. It is designed to be repeatable: the same scenarios, rubrics, and scoring sheets are used for each round so results are comparable over time.

---

## 3. Scenario Set

A fixed library of realistic marketing scenarios drawn from the target segment (Arabic-first SMBs in e-commerce and local services). Each scenario specifies:

- A seeded workspace with representative Business Brain content.
- A user request and any clarification turns.
- The expected deliverable type.
- The rubric dimensions that apply.

Scenario categories:

| Category | Example |
|---|---|
| Brand voice definition | "Define our brand voice for a Gulf audience" |
| Market research | "Research the skincare market in Egypt" |
| Competitor analysis | "Analyze what competitor X is advertising" |
| Content creation | "Write 5 Instagram posts for the new face serum" |
| Campaign planning | "Build a content calendar for next month" |
| Performance interpretation | "Here is our monthly CSV — what went wrong?" |
| Onboarding crawl | "Extract our company knowledge from our website" |
| Multilingual mixing | "Write this post in Arabic but keep product names in English" |

---

## 4. Arabic Quality Rubric

Reviewers score each Arabic deliverable on a 1–5 scale across five dimensions:

| Dimension | What reviewers assess |
|---|---|
| **Fluency** | Reads as natural, idiomatic Arabic — no translated feel. |
| **Dialect fit** | Matches the requested register (Modern Standard / Egyptian / Gulf / Levantine). |
| **Brand fit** | Consistent with the approved brand voice and tone. |
| **Cultural fit** | Appropriate idioms, references, and phrasing for the local audience. |
| **Factual grounding** | Claims are consistent with the Business Brain and cited sources. |

A deliverable is considered **acceptable** when every dimension scores ≥ 4 and no dimension scores below 3.

### Reviewer panel

- **Two independent human reviewers** score every deliverable in an evaluation round.
- **Agreement:** when reviewers differ by more than one point on any dimension, a **third reviewer breaks the tie**.
- Reviewers are Arabic-native speakers; at least one reviewer per round works in the target domain.

---

## 5. Brain Value Measurement

To test whether the Business Brain adds value, the same task is run in two configurations:

| Configuration | Retrieval |
|---|---|
| **Treatment** | Business Brain graph retrieval (vector seed + 1–2 hop relational expansion). |
| **Control** | Document-only retrieval — raw source chunks without graph extraction. |

Both configurations use the same model, prompts, and scenario. Reviewers see outputs without knowing which configuration produced them (blind scoring).

Measures captured per configuration:

- **Clarification turns** — how many information gaps the agent had to ask about.
- **Factual corrections** — how often the user had to correct a factual claim.
- **Brand corrections** — how often the user had to correct tone or voice.
- **Reviewer quality score** — the Arabic rubric average.
- **Acceptance** — whether output was usable without major edits.

**Hypothesis:** the grounded configuration produces **≥ 50% fewer clarification turns and factual/brand corrections** than the document-only control on comparable tasks while matching or exceeding reviewer quality scores.

---

## 6. Execution Reliability & Babysitting Burden

Publishing correctness and user friction are measured from platform records:

- **Publish success rate** — approved posts that publish without error (target ≥ 95%).
- **Read-back verification** — posts confirmed to exist on the platform after dispatch.
- **Idempotency** — no duplicate posts across retries or reconnects.
- **Zero-Editing Acceptance** — percentage of content deliverables approved with zero or minor edits (target ≥ 75%), validating that users escape the "Editing Trap."
- **Batch Review Efficiency** — total human time spent reviewing a full weekly campaign batch in the Batch Review UI (target ≤ 15 minutes), proving the absence of a "babysitting burden."

---

## 7. Onboarding & Knowledge Quality

- **Crawl approval rate** — share of extracted candidates approved rather than rejected (target ≥ 80%).
- **Catalog coverage** — whether reviewers judge the candidate set as complete for the company.
- **Provenance correctness** — whether each approved item points to a valid source.

---

## 8. Instrumentation

| Signal | Source |
|---|---|
| Token usage, latency, model and prompt version | Langfuse traces |
| Per-task cost | LiteLLM cost tracking + Langfuse |
| Clarification and correction events | Product database (conversation + task records) |
| Approval, edit, and publish records | Approval actions + execution attempts |
| Review time per batch | Batch review session timestamps |
| Reviewer scores | Evaluation scoring sheets |

---

## 9. Reporting

Each evaluation round produces:

- **Period and scenario set** covered.
- **Per-dimension scores** with disagreement and tie-break notes.
- **Treatment vs. control comparison** for Brain value.
- **Execution reliability & review efficiency figures.**
- **Notable failures** with reproduction details.
- **Actions** owned by a named team member.

Reports are retained so that results across rounds can be compared.

---

## 10. Success Criteria Summary

| Claim | Measure | Target |
|---|---|---|
| Arabic quality | Rubric dimensions | All ≥ 4, none < 3 |
| Brain value | Clarification turns + corrections vs. control | ≥ 50% reduction |
| Brain value | Reviewer quality vs. control | Match or exceed |
| Zero-Editing Trap | Acceptance without major rewrites | ≥ 75% |
| Review Efficiency | Weekly batch approval session time | ≤ 15 min |
| Execution reliability | Publish success + read-back | ≥ 95% |
| Corpus independence | No duplicate posts | 100% |
| Onboarding quality | Candidate approval rate | ≥ 80% |
