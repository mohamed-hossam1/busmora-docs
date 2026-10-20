# BusMora Glossary

The shared language of BusMora, a product that gives an Arabic-first small business one AI Marketing Employee. The Employee learns the business, researches its market, produces marketing work, and publishes to social platforms only with human approval. Terms are grouped by the bounded context that owns them (see [contexts.md](./contexts.md)).

## Identity & Workspace

**Business**:
The real-world small or medium company that BusMora works for.
_Avoid_: Client, company, customer, organization

**Workspace**:
A Business's private, isolated space in BusMora, holding everything known and produced for it.
_Avoid_: Account, tenant, organization, project

**User**:
The one person who operates a Workspace, whether founder or marketing coordinator.
_Avoid_: Owner, marketing person, member, customer, Sarah

**Business Time Zone**:
The single time zone a Workspace plans and reads its schedules in.
_Avoid_: Locale, user time zone

## Business Brain

**Business Brain**:
The Workspace's permanent, reviewed memory of its Business: what it is, what it sells, who it serves and how it speaks.
_Avoid_: Knowledge base, memory, context, company profile

**Knowledge Item**:
One reviewed or reviewable fact about the Business, belonging to exactly one Knowledge Category.
_Avoid_: Node, fact, entity, concept, knowledge candidate (when approved)

**Knowledge Category**:
One of the nine fixed kinds of Knowledge Item: Company Profile, Offering, Audience, Brand Rule, Positioning, Strategic Goal, Channel, Competitor, Company Fact.
_Avoid_: Entity type, node type, section

**Relationship**:
A named, directed link between two Knowledge Items, such as an Offering that targets an Audience.
_Avoid_: Edge, link, connection

**Procedural Rule**:
A reviewed instruction about *how* the Employee should work for this Business, learned from the User's edits and feedback.
_Avoid_: Style memory, calibration, guideline, procedural memory record

**Candidate**:
A Knowledge Item or Procedural Rule that has been proposed but not yet reviewed by the User.
_Avoid_: Suggestion, draft knowledge, pending item

**Review**:
The User's individual decision to approve, edit-and-approve or reject one Candidate.
_Avoid_: Approval (reserved for Posts), validation, moderation

**Retirement**:
Withdrawing an approved Knowledge Item from use while keeping its history.
_Avoid_: Deletion, archiving, removal

**Conflict**:
Two Knowledge Items that cannot both be true, surfaced to the User rather than settled automatically.
_Avoid_: Duplicate, inconsistency

**Provenance**:
The record of where a Knowledge Item came from: a crawled page, a Task, a Conversation or the User directly.
_Avoid_: Source info, origin, lineage

**Brand Voice**:
The Brand Rules that together define how the Business sounds: tone, style and Dialect.
_Avoid_: Tone of voice, brand tone, writing style

**Dialect**:
The Arabic register content is written in: Egyptian, Gulf or Modern Standard Arabic.
_Avoid_: Language style, accent, locale

**Brand Kit**:
The logo, colors and typography detected for the Business from its website.
_Avoid_: Brand assets, brand template, style guide

## Marketing Employee

**Marketing Employee**:
The single AI worker a Workspace has, which receives requests, plans work and delivers results.
_Avoid_: Agent, assistant, bot, AI employee, copilot

**Sub-agent**:
A specialist part of the Marketing Employee with one job: Market Research, Competitor Analysis, Content Writing, Campaign Planning or Social Posting.
_Avoid_: Agent, worker, tool, specialist bot

**Orchestrator**:
The part of the Marketing Employee that diagnoses a request, plans it and delegates to Sub-agents.
_Avoid_: Supervisor, main agent, planner

**Conversation**:
An ongoing exchange of messages between the User and the Marketing Employee.
_Avoid_: Chat, session, thread

**Quick Reply**:
An immediate answer to a simple message that does not start a Task.
_Avoid_: Simple task, chat response

**Task**:
One unit of marketing work the User asks the Marketing Employee to carry out, ending in Deliverables or an explanation of why it could not.
_Avoid_: Job, request, workflow, run, mission

**Constraint Diagnosis**:
The Orchestrator's judgement of which commercial bottleneck a Task should address: discovery, positioning or conversion.
_Avoid_: Bottleneck analysis, strategy step

**Clarification**:
A pause in a Task where the Employee asks the User for missing information through a structured form.
_Avoid_: Question, prompt, interruption

**Search Yourself**:
The User's choice, on a Clarification field, to let the Employee research the answer instead.
_Avoid_: Skip, auto-fill

**Canvas**:
The live picture of a running Task showing which Sub-agents are working and what they are doing.
_Avoid_: Decision canvas, workflow visualization, agent graph

**Partial Result**:
A Task outcome built from whatever was gathered before a time or budget limit was reached, labelled as incomplete.
_Avoid_: Fallback result, degraded result

## Market Intelligence

**Onboarding Crawl**:
The bounded first read of the Business's own website that seeds Candidates and the Brand Kit.
_Avoid_: Website scan, initial import, site crawl

**Research Crawl**:
Gathering of public material (community discussions, videos, competitor sites and ads) for a Task.
_Avoid_: Scraping job, data collection

**Snapshot**:
The preserved copy of a page as it was when crawled.
_Avoid_: Cache, raw page, dump

**Research Signal**:
A piece of crawled material judged to show real buyer intent, such as a complaint about a competitor or a price question.
_Avoid_: Lead, insight, mention

**Buyer Vernacular**:
The actual words and phrases real buyers use about a product or problem, taken from Research Signals.
_Avoid_: Keywords, customer language

**Source Citation**:
The reference a Deliverable gives for a claim: where it came from, on which platform, and when it was accessed.
_Avoid_: Reference, link, footnote

## Deliverables

**Deliverable**:
A finished piece of work the Employee hands to the User and keeps in the Workspace.
_Avoid_: Artifact, output, report, result

**Research Report**:
A Deliverable describing a market, its audience and its opportunities, with Source Citations.
_Avoid_: Research brief, market analysis

**Competitor Analysis**:
A Deliverable comparing one competitor with the Business, with Source Citations.
_Avoid_: Competitor report, competitor brief

**Content Package**:
A Deliverable of ready-to-use content pieces (posts, articles, ad copy) written in the Brand Voice.
_Avoid_: Content posts, copy set

**Content Calendar**:
A Deliverable that lays out Posts across dates and Platforms for a chosen period.
_Avoid_: Campaign plan, content plan, schedule

**Performance Diagnosis**:
A Deliverable explaining what worked and what didn't in an uploaded Performance Report, with prioritized adjustments.
_Avoid_: Analytics report, performance analysis

**Performance Report**:
The marketing results file the User uploads for diagnosis.
_Avoid_: CSV, metrics export

**Brand Visual**:
An image for a Post made of generated imagery with the Brand Kit laid over it.
_Avoid_: Brand-template composite, creative, generated image

## Governed Publishing

**Post**:
One piece of social content intended for one Connected Account at one time.
_Avoid_: Publication, tweet, update, approval action, approval card

**Post Version**:
One unchangeable state of a Post's content; approval always applies to a specific Post Version.
_Avoid_: Snapshot (reserved for crawled pages), draft, revision

**Batch Review**:
The place where the User approves, edits, regenerates, rejects or cancels many Posts in one sitting.
_Avoid_: Approval queue, review inbox, approval UI

**Approval**:
The User's explicit consent that a specific Post Version may be published as-is.
_Avoid_: Review (reserved for Candidates), sign-off, confirmation

**Scheduled Time**:
The moment a Post is planned to go live, fixed in absolute time and read in the Business Time Zone.
_Avoid_: Publish date, slot

**Publish Attempt**:
One try at putting an approved Post Version live on its Platform.
_Avoid_: Dispatch, execution, job

**Read-back Verification**:
Confirming, after a Publish Attempt, that the Post actually exists on the Platform.
_Avoid_: Confirmation, check, validation

**Expired Post**:
A Post that was still awaiting Approval when its Scheduled Time passed.
_Avoid_: Stale post, missed post

## Channel Connections

**Platform**:
An external social network BusMora publishes to: Facebook Pages, Instagram Business or X.
_Avoid_: Channel (reserved for the Brain category), network, provider, integration

**Connected Account**:
A Business's own account on a Platform that the User has authorized BusMora to publish to.
_Avoid_: Integration, connection, linked account, social account

**Reconnect Needed**:
The state of a Connected Account whose authorization can no longer be renewed without the User.
_Avoid_: Expired token, disconnected

## Usage, Audit & Notifications

**Usage Record**:
A measured amount of a costly resource (model use, crawling, images, X posts) attributed to a Workspace and Task.
_Avoid_: Billing event, charge, metering record

**Audit Event**:
A permanent, unalterable record that a governed action happened: a Review, Approval, publish, Retirement or Connected Account change.
_Avoid_: Log, history entry, activity

**Notification**:
An in-app message telling the User something needs attention or has finished.
_Avoid_: Alert, email, toast

## Evaluation

**Scenario**:
A fixed, realistic marketing request used to judge the Employee's quality the same way every round.
_Avoid_: Test case, benchmark task

**Evaluation Round**:
One scoring pass of the full Scenario set by human Reviewers.
_Avoid_: Test run, eval

**Reviewer**:
An Arabic-native person who scores Deliverables against the rubric in an Evaluation Round.
_Avoid_: Annotator, rater, judge

**Treatment and Control**:
The two ways a Scenario is run to measure the Business Brain's value: with the Brain, or with the Business's raw documents only.
_Avoid_: A/B test, baseline
