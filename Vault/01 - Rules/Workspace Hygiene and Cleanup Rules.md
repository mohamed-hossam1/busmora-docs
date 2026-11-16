# Workspace Hygiene & Post-Task Cleanup Rules

Every agent operating within any BusMora repository must ensure the codebase remains clean, disciplined, and free of abandoned or obsolete artifacts upon completing any task.

---

## 1. Core Principles

- **Zero Dead Code:** If code, components, or styles are replaced or no longer imported, they must be deleted, not left in the repository.
- **No Orphaned Assets:** Images, SVGs, or mocks that are unreferenced in production code must be deleted immediately.
- **No Casing or Shadow Duplicates:** In case-insensitive or multi-agent environments, creating lowercase and uppercase duplicates (e.g., `Hero.tsx` and `hero.tsx`) is strictly forbidden. Delete superseded or duplicate casing variants immediately.
- **Self-Cleaning Scratch Code:** Any one-off inspection scripts, temporary data files, or exploratory benchmarks must either be placed in `.gitignore`'d `scratch/` directories or deleted before finishing the turn.

---

## 2. Post-Task Checklist

Before marking any task as complete or reporting success to the engineer:

1. [ ] **Audit `git status`:** Inspect all untracked and modified files. Verify that every changed file directly serves the task goal.
2. [ ] **Prune Dead/Unused Files:** Recursively remove abandoned components, superseded drafts, and dead folders.
3. [ ] **Verify Imports & Build:** Run `bun run build` (or repository-specific test/build runner) to confirm no broken imports or compilation errors remain after pruning.
4. [ ] **Report Cleanup Actions:** Explicitly inform the engineer of any obsolete or duplicate files purged during the task.
