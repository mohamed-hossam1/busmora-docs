# BusMora Glossary

The shared ubiquitous language of BusMora, a platform that gives an Arabic-first small business an **AI Marketing Director**. The Director learns the business, diagnoses commercial bottlenecks, prioritizes feasible marketing work on an adaptive roadmap, prepares culturally authentic content, and safely publishes to social platforms under human approval. Terms are grouped by bounded context (see [contexts.md](./contexts.md)).

---

## Identity & Workspace

**Business**:
The real-world small or medium company (service or commercial SMB) that BusMora works for.  
_Avoid_: Client, company, customer, organization

**Workspace**:
A Business's private, isolated space in BusMora, holding everything known and produced for it under single-owner isolation.  
_Avoid_: Account, tenant, organization, project

**Owner** (or **User**):
The primary operator who owns a Workspace, with sole authority over its Brain, integrations, approvals, and data.  
_Avoid_: Client, subscriber, admin, Sarah

**Business Time Zone**:
The single IANA time zone a Workspace plans, schedules, and publishes in.  
_Avoid_: Locale, user time zone, browser time

---

## Business Brain & Memory

**Business Brain**:
The Workspace's permanent, reviewed memory of its Business: what it is, what it sells, who it serves, how it speaks, and its strategic decisions.  
_Avoid_: Knowledge base, memory, context, company profile

**Knowledge Item**:
One structured fact about the Business, belonging to exactly one of the nine canonical Knowledge Categories.  
_Avoid_: Node, fact, entity, concept, knowledge candidate (when approved)

**Knowledge Category**:
One of the nine fixed kinds of Knowledge Item: Company Profile, Offering, Audience, Brand Rule, Positioning, Strategic Goal, Channel, Competitor, Company Fact.  
_Avoid_: Entity type, node type, section

**Relationship**:
A named, directed link between two Knowledge Items, such as an Offering that targets an Audience.  
_Avoid_: Edge, link, connection

**Epistemic Status**:
The explicit classification of any knowledge piece: Source Observation, Owner-Provided Claim, AI Hypothesis/Interpretation, or Confirmed Decision.  
_Avoid_: Certainty level, raw fact

**Imported Context**:
Context extracted from a website URL crawl, active immediately as imported/unverified with provenance, without demanding blocking manual review.  
_Avoid_: Raw crawl, scraped text

**Procedural Rule**:
A reviewed instruction about *how* the Director should work for this Business, learned from owner edits and feedback.  
_Avoid_: Style memory, calibration, guideline

**Pending Brain Update**:
An AI-proposed persistent fact, interpretation, or preference awaiting explicit owner approval before becoming persistent knowledge.  
_Avoid_: Suggestion, draft knowledge, pending item

**Review**:
The owner's decision to approve, edit-and-approve, or reject a Candidate or Pending Brain Update.  
_Avoid_: Approval (reserved for Posts), validation, moderation

**Context Staleness Hold**:
A safety hold placed on dependent active tasks and scheduled posts when core business facts (pricing, offers, claims) are modified (FR-48).  
_Avoid_: Broken post, frozen queue

**Retirement**:
Withdrawing an approved Knowledge Item from active retrieval while keeping its historical provenance.  
_Avoid_: Deletion, archiving, removal

**Conflict**:
Two Knowledge Items that contradict each other, surfaced to the owner rather than resolved silently.  
_Avoid_: Duplicate, inconsistency

**Provenance**:
The inspectable record of where a Knowledge Item came from: website import, task, conversation, or direct owner entry.  
_Avoid_: Source info, origin, lineage

**Brand Voice**:
The Brand Rules that together define how the Business sounds: tone, style, and Dialect.  
_Avoid_: Tone of voice, brand tone, writing style

**Dialect**:
The Arabic register marketing content is written in: Egyptian Conversational, Gulf-oriented, Modern Standard Arabic (MSA), or English.  
_Avoid_: Language style, accent, locale

**Brand Kit**:
The logo, colors, and typography detected for the Business from its website.  
_Avoid_: Brand assets, brand template, style guide

---

## Marketing Director & Orchestration

**Marketing Director**:
The AI partner in a Workspace that understands business context, diagnoses commercial constraints, maintains the roadmap, and orchestrates marketing tasks.  
_Avoid_: Agent, bot, copilot, employee (when emphasizing strategic direction)

**STAR Diagnostic Lenses**:
The 5 commercial perspectives (Situation/Offer, Traffic/Acquisition, Activation/Conversion, Retention/Referrals, Operations/Fulfillment) used to identify primary bottlenecks.  
_Avoid_: Audit framework, marketing scorecard

**Constraint Hypothesis**:
The Director's calibrated assessment of which specific commercial bottleneck limits the owner's goal right now.  
_Avoid_: Bottleneck analysis, strategy guess

**Now/Next/Later Roadmap**:
The prioritized, capacity-aware plan of active (Now), queued (Next), and future (Later) marketing tasks (FR-16).  
_Avoid_: Backlog, task list, project plan

**Curated Task Recipe**:
A pre-configured, structured template for a common marketing task (e.g. claim review, offer clarification, one-channel campaign, recurring review).  
_Avoid_: Workflow template, agent prompt

**Orchestrator**:
The dynamic multi-agent coordinator that loads context, selects specialists/tools, plans steps, and recovers from failures.  
_Avoid_: Supervisor, main agent, planner

**Fast Router**:
The fast, cost-conscious AI routing step that handles simple conversation directly and routes substantive work to the Orchestrator.  
_Avoid_: Intent classifier, triage bot

**Conversation**:
An ongoing exchange of messages between the owner and the Marketing Director.  
_Avoid_: Chat, session, thread

**Quick Reply**:
An immediate answer to a simple greeting or question that does not start a substantive Task ($\le 3\text{s}$).  
_Avoid_: Simple task, chat response

**Task**:
One unit of marketing work with explicit rationale, effort, dependencies, completion conditions, and review criteria (FR-46).  
_Avoid_: Job, request, workflow, run

**Per-Task Research Budget**:
The authorized computational, time, and tool quota for a research task; any increase requires explicit owner approval (FR-27).  
_Avoid_: Tool limit, token pool

**Honest Handoff**:
Clear instructions, copy, and completion checklists provided for unsupported external work (e.g. WhatsApp follow-up or live website edits), never claimed as executed by BusMora (FR-49).  
_Avoid_: Unsupported error, manual fallback

**Clarification**:
A structured pause where the Director requests essential missing details from the owner.  
_Avoid_: Question, prompt, interruption

**Canvas**:
The live, authentic picture of a running Task rendering recorded execution events, active specialists, tools, and handoffs.  
_Avoid_: Decision canvas, workflow visualization, agent graph

**Partial Result**:
An honest, bounded deliverable produced when retrieval or authorized budget is exhausted, clearly disclosing limitations.  
_Avoid_: Degraded result, failure

---

## Market Intelligence

**Onboarding Crawl**:
The bounded first read of the Business's own website (≤ 10 pages, ≤ 5 min) that extracts business context and brand cues into imported context.  
_Avoid_: Site scraper, full crawl

**Research Crawl**:
Dynamic multi-source search and retrieval across public web pages, customer reviews, and community sentiment within an authorized budget.  
_Avoid_: Scraping job, data collection

**Snapshot**:
The preserved copy of an external page as it was when crawled, retaining access timestamp.  
_Avoid_: Cache, raw page, dump

**Research Signal**:
A piece of crawled material judged to show authentic buyer intent or market sentiment.  
_Avoid_: Lead, mention, insight

**Buyer Vernacular**:
The authentic words and phrases real buyers use about a problem or category, captured in Research Reports.  
_Avoid_: Keywords, customer language

**Source Citation**:
The inspectable reference a Deliverable gives for a claim: origin URL, platform, and access date.  
_Avoid_: Reference, link, footnote

---

## Deliverables & Performance

**Deliverable**:
A finished, persistent piece of marketing work handed to the owner and saved in the Workspace.  
_Avoid_: Artifact, output, report

**Research Report**:
A Deliverable synthesizing competitive findings, customer pain points, and strategic recommendations with inspectable citations.  
_Avoid_: Research brief, market analysis

**Content Package**:
A Deliverable containing channel-specific copy, visual briefs, CTAs, and hashtags written in the Brand Voice.  
_Avoid_: Copy set, post batch

**Performance Report**:
A campaign performance file in CSV or Excel (`.xlsx`) format uploaded by the owner, or entered manually as aggregate observations.  
_Avoid_: Analytics export, spreadsheet

**Performance Diagnosis**:
A Deliverable analyzing uploaded or entered performance against goals using deterministic metrics and proposing prioritized adjustments.  
_Avoid_: Analytics dashboard, BI report

---

## Governed Publishing

**Post**:
One piece of social content intended for one Connected Account at one specific time.  
_Avoid_: Publication, update, approval card

**Post Version**:
An immutable snapshot of a Post's text, media, destination, and execution parameters; approval always binds to a specific Post Version.  
_Avoid_: Draft, revision

**Review & Approval**:
The interface where the owner inspects, edits, approves, or rejects planned Posts.  
_Avoid_: Approval queue, review inbox

**Approval**:
The owner's explicit consent that an exact Post Version, destination, scheduled time/timezone, and delivery lateness allowance may be published.  
_Avoid_: Review (reserved for Candidates), sign-off

**Scheduled Time**:
The moment a Post is planned to publish, fixed in UTC and displayed in the Business Time Zone.  
_Avoid_: Publish slot, post date

**Lateness Allowance**:
The maximum disclosed delivery window after the Scheduled Time during which publishing is still permitted before requiring re-approval.  
_Avoid_: Grace period, timeout

**Publish Attempt**:
One idempotent try at putting an approved Post Version live on its platform.  
_Avoid_: Dispatch, execution, job

**Read-Back Verification**:
Confirming, after dispatch, that the post actually exists on the external platform before marking it published.  
_Avoid_: Confirmation, check

---

## Channel Connections

**Platform**:
An external social network BusMora publishes to: **Facebook Pages** in Phase 2; **Instagram Business** and **X** in Phase 3.  
_Avoid_: Channel (reserved for Brain category), network, provider

**Connected Account**:
A Business's authorized account on a Platform that the owner has connected to BusMora.  
_Avoid_: Social account, integration

**Reconnect Needed**:
The state of a Connected Account whose OAuth authorization has expired or been revoked and requires owner reconnection.  
_Avoid_: Expired token, disconnected

---

## Usage, Audit & Evaluation

**Usage Record**:
A measured amount of costly resources (model tokens, search calls, compute) attributed to a Workspace and Task.  
_Avoid_: Billing event, charge

**Audit Event**:
An append-only record that a governed action occurred: Approval, Review, publish, Retirement, or account connection change.  
_Avoid_: Log, history entry

**Scenario**:
One of the 24 authored realistic business cases used to evaluate system quality under fixed rubrics.  
_Avoid_: Test case, benchmark task

**Evaluation Round**:
A structured scoring pass of the scenario library by native human reviewers across the 4 registers.  
_Avoid_: Test run, eval

**Treatment and Control**:
The two blind configurations used to measure Business Brain value: with the Brain (Treatment), or with raw documents only (Control).  
_Avoid_: A/B test, baseline
