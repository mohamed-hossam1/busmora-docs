# Vault Conventions

These conventions govern how all team members and AI-agent skills create, find, and maintain Vault content. They are global rules, not feature-specific requirements.

## 1. Canonical locations

- `00 - Home.md` is the Vault entry point and navigation overview.
- `01 - Initiatives/<Initiative Name>/` contains initiative-level context: `PRD.md`, an optional `Benchmark.md`, and `Slices/`.
- `01 - Initiatives/<Initiative Name>/Slices/` contains lightweight Slice overviews and links only. Do not copy full feature specifications here.
- `02 - Features/<Feature Name>/` is the single canonical home for a Feature's `Overview.md`, `Requirements.md`, `Technical Design.md`, and `Testing.md`.
- `02 - Features/<Feature Name>/Tasks/JIRA-123.md` holds the execution record for a Jira task associated with that Feature. Create the `Tasks/` directory and record when needed.
- `03 - Skills/` contains reusable AI-agent skills and workflow instructions.
- `04 - Rules/` contains global, team-wide rules. Do not put feature-specific implementation details here.
- `Templates/` contains reusable starting points for documents.

## 2. Initiative, Slice, and Feature boundaries

- An Initiative describes a larger objective and its rationale. Its PRD records the initiative-level problem, goals, and scope.
- A Slice connects an Initiative to one canonical Feature. Each Slice must link to exactly one canonical Feature.
- A Feature documents one coherent unit of functionality. Its feature folder is the source of truth for feature-level requirements and design.
- Do not create competing canonical copies of the same Feature or duplicate full feature documentation in an Initiative's Slice.
- Create `Benchmark.md` only when a meaningful comparison is needed, especially for algorithm, library, or tool selection. Not every technical decision needs a benchmark.

## 3. Task records versus canonical feature documents

- The Jira task record captures task-specific workflow state: decisions made during clarification, plan approval, progress through workflow skills, verification evidence, skipped activities and reasons, deviations, and handoff notes.
- Canonical Feature documents capture durable knowledge about the feature: its overview, requirements, intended technical design, and testing approach.
- When implementation reveals a durable change to feature behavior or design, update the relevant canonical document as well as the task record where appropriate.
- Do not use a task record as a substitute for updating canonical documentation, and do not copy the entire feature specification into every task record.

## 4. Documentation lifecycle and accuracy

- For a new or materially changed feature, document and align on the intended solution before implementation. Update existing records when they already cover the feature; create missing records when needed.
- After implementation, record the actual implementation, relevant decisions, deviations from the intended design, and verification results through the appropriate workflow skill.
- Do not claim that tests, checks, or manual verification passed unless there is evidence. Record failures, skipped checks, and the reason for skipping them honestly.
- Separate confirmed facts, agreed decisions, assumptions, and unresolved questions. Never turn an assumption into a decision without confirmation.
- If Jira, Vault documentation, and current code disagree, investigate and surface the conflict. Do not silently choose one source or overwrite a meaningful decision.
- Code is authoritative for what currently runs; the Vault is the canonical record of intended behavior, rationale, and agreed design. When they diverge, document and resolve the divergence rather than pretending one describes the other.

## 5. Navigation and links

- Start at `00 - Home.md`, then follow links to the relevant Initiative, Slice, and Feature.
- Use Obsidian wikilinks (`[[Note Name]]` or path-qualified wikilinks when needed) for internal navigation.
- Use relative Markdown links when portability outside Obsidian is important.
- Prefer links to canonical documents over copying their content into multiple places.
- Keep links and references accurate when moving or renaming notes.
- Use clear, stable names for Initiatives, Slices, Features, and documents. Follow the existing folder and filename conventions rather than inventing a new structure for each task.

## 6. AI-agent and skill boundaries

- Agents must map relevant Vault context before inspecting the codebase, then inspect only code areas relevant to the task.
- Read only the feature documents and additional rules needed for the current task; do not load unrelated parts of the Vault without a reason.
- Skills define workflow execution. Global conventions belong here in Rules, not copied inconsistently into individual skills.
- Follow the established skill order and prerequisites. Record completion status and handoff information so a later skill can verify whether prerequisites were completed.
- The responsible developer owns the task end-to-end. Do not assume a separate team review is mandatory unless an explicit team rule says so.

## 7. Safety and maintenance

- Never store secrets, credentials, access tokens, or sensitive personal data in the Vault.
- Agree on significant structural changes with the team before applying them broadly.
- Prefer updating an existing canonical note over creating near-duplicate notes.
- Keep documents focused and useful; remove stale or duplicated material when its canonical replacement is established.
