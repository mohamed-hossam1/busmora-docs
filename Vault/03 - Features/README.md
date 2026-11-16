# Business Features

Each `03 - Features/<feature-name>/Overview.md` answers: what does this stable, user-facing business capability do now?

Use [[Templates/Feature Overview Template]] for purpose, current behavior, business rules, boundaries, and links to contributing tasks. Reuse the same overview as the capability evolves. Examples include Business Brain, Director Chat, Deliverables, and Post Scheduling; create folders only when there is implemented capability to document.

Update the overview alongside the behavior change. On working branches, identify proposed behavior; after merge it describes current business truth. Across repositories, link code and docs PRs and coordinate their merge dependency as described in [[01 - Rules/Task Lifecycle Protocol]].

CI, Docker, infrastructure, and internal refactoring belong in [[02 - Tasks/README|Tasks]] and create no artificial business features. Keep execution logs and benchmark details in the contributing task records.
