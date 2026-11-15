# Vault Conventions

## Canonical locations

- `00 - Home.md`: entry point and navigation.
- `01 - Rules/`: agreed team conventions and the [[Task Lifecycle Protocol]].
- `02 - Tasks/<JIRA-KEY>/Task.md`: chronological execution record; the only required task document.
- `02 - Tasks/<JIRA-KEY>/research.md`, `benchmark.md`, and `evidence/`: optional findings, measurements, and sanitized reproducibility material.
- `03 - Features/<feature-name>/Overview.md`: current living business capability, linked to its contributing tasks.
- `Templates/`: four minimal starting templates; remove unused sections.
- `core/` and `domain/` outside the Vault: durable architecture, requirements, and domain definitions.

Technical tasks such as CI, Docker, infrastructure, and internal refactoring do not create business feature folders. Reuse an existing capability overview when behavior changes. Task decisions and test logs stay in Tasks; do not copy them into the living overview.

## Context and truth

Read relevant rules and system constraints before research, then follow task and feature links as needed. Inspect code to establish runtime behavior. Do not load the whole Vault or require a Jira link before consulting relevant architecture.

Separate confirmed facts, agreed decisions, assumptions, and unresolved questions. Explain and resolve conflicts between Jira, specifications, and code with the engineer. Features describe merged business behavior, while `core/` may include explicitly identified design intent; neither is evidence that code has shipped.

Follow the [[Documentation Lifecycle]] for updates and the [[Task Lifecycle Protocol]] for Jira milestones. Record actual outcomes, including failures and skipped checks. Keep secrets and sensitive personal data out of research, logs, screenshots, and datasets.

## Navigation and ownership

Use path-qualified Obsidian wikilinks from the Vault root when note names could be ambiguous. Use relative Markdown links for documents outside the Vault. Update incoming links when moving files; prefer links to canonical facts over copies.

`.agents/skills/` is the canonical skill source. `.claude/skills/` contains relative symlinks to those same skill directories. Edit the source once. Shared lifecycle rules belong here; skills supply focused execution guidance and link back to them.

The engineer owns decisions end-to-end. Honor existing instructions and authorization; do not introduce repeated confirmation gates or mandatory unrelated cleanup. Significant new scope or unresolved architectural decisions require discussion.
