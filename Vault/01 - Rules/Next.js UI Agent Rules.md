# Next.js UI Agent Rules

These rules govern UI decisions that may not be obvious from reading the code. Do not repeat framework documentation or existing project configuration. Product requirements live in the project's own docs.

For frontend localization, public-page rendering, navigation and metadata, also follow [[Next.js Multilingual and SEO Rules]]. Arabic/English support and crawlable public content are mandatory for affected public routes; preserve the protected application's existing data-fetching boundaries.

## Design decisions

1. **Preserve the existing visual language.** When a task does not specify a new design, follow the closest existing page/component for typography, spacing, color, radius, icon style, and interaction patterns. Do not introduce a new visual style.
2. **Do not invent missing design decisions.** If two plausible designs would materially change the UX and there is no precedent or reference, ask for direction. Make only low-impact, reversible choices independently.
3. **Match references deliberately.** If the task supplies a screenshot or design, reproduce its structure, hierarchy, spacing, and responsive intent.

## Components and styling

4. **Reuse before creating.** Prefer an existing component or variant. Create a new shared abstraction only for a real repeated pattern; do not over-engineer a one-off UI.
5. **Do not change global styling for a local problem.** Avoid broad CSS overrides, new design tokens, or modifications to shared primitives unless the task explicitly requires that impact.
6. **Keep Client Component boundaries narrow in App Router.** Use `'use client'` for the actual interactive subtree, not a whole page/layout merely to support one interactive child. Follow the repository's routing mode if it is not App Router.
7. **Do not introduce another UI system.** Avoid new component libraries, icon packs, CSS frameworks, or animation dependencies unless explicitly approved.
8. **Install shadcn components via CLI only — Never copy-paste.** Whenever a shadcn component is needed, the agent is strictly prohibited from manually writing, copying, or hallucinating component code into `shared/ui`. The agent MUST execute the official CLI command (e.g. `bunx --bun shadcn@latest add <component>` or `npx shadcn@latest add <component>`) to ensure it is installed deterministically according to `components.json`, with the correct Tailwind configuration, Radix primitives, and dependencies.

## Architecture: Feature-Based Architecture

8. **Strict Feature-Based Architecture.** Organize frontend code by domain feature modules under `features/<feature-name>/`:
   - `features/<feature-name>/ui/`: Feature-specific UI components and screen composition.
   - `features/<feature-name>/api/`: Feature-specific queries, mutations, and API consumption hooks.
   - `features/<feature-name>/model/`: Feature view models, state reducers, validation, and domain rules.
   - `features/<feature-name>/index.ts`: The explicit public interface. All cross-feature imports and route imports must come strictly through `index.ts` — no deep imports into feature internals.
   - `app/` (routes and layouts): Composition only. Routes wire URL parameters and features together; no business logic, data fetching, or heavy styling in `app/`.
   - `shared/`: Shared primitives, UI kit (`shared/ui`), API client gateway (`shared/api`), session management, and cross-cutting utilities.

## UI completeness

9. **Finish the experience, not just the screenshot.** New interactions must have coherent pending, disabled, empty, error, and success states wherever relevant. No fake handlers, mock success, or placeholder actions in production UI.
10. **Protect essential usability.** New UI must preserve mobile access, keyboard operation, and readable content. Where the application supports RTL or localization, the change must work in those modes too.
11. **Prevent visual regressions.** Verify the affected UI at narrow and wide viewports, with realistic long/empty content and interaction states. If visual inspection was not possible, say so rather than claiming it was checked.

## Scope and handoff

12. **Avoid unrelated UI changes.** Do not redesign adjacent screens, reformat unrelated files, or reorganize a feature while completing a focused task.
13. **Report what is real.** End with the visible changes, checks actually performed, and any unverified visual or interaction behavior. Do not claim a feature is complete because the page merely renders.

## Navigation & Routing: Centralized Route Registry

14. **No hardcoded URL string literals.** Hardcoded URL string literals in components, hooks, links, or navigation handlers (e.g. `<Link href="/posts">` or `router.push("/employee/123")`) are strictly prohibited across the entire codebase.
    - **File location:** All URLs must reside in `shared/routes/routes.ts` (exported via `shared/routes/index.ts`).
    - **Single Registry Object:** Define and export a single constant object `ROUTES` (with `as const`).
    - **Static vs Dynamic paths:**
      - **Static routes** are defined as plain strings:
        ```typescript
        LANDING: '/',
        LOGIN: '/login',
        SETTINGS: '/settings',
        ```
      - **Dynamic routes** are defined as pure functions taking typed parameters and returning strings:
        ```typescript
        CHAT: (conversationId: string) => `/employee/${conversationId}`,
        POST_DETAILS: (postId: string) => `/scheduling/posts/${postId}`,
        ```
    - **Strict consumption:** All `<Link href={...}>`, `router.push(...)`, `redirect(...)`, and navigation helpers must import and use `ROUTES` from `@/shared/routes`.
