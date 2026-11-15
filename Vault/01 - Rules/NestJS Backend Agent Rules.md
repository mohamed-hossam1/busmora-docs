# NestJS Backend Agent Rules

These rules govern backend decisions that are **not obvious from reading the repository**. They are not a NestJS tutorial or a replacement for product/API specifications. Apply them before every backend task.

## Architecture and scope

1. **Preserve feature ownership.** Extend the module that owns a capability. Do not scatter one feature's rules across unrelated modules or introduce a new module/layer merely to make the structure look more sophisticated.
2. **Keep responsibilities clear.** Controllers handle transport concerns; application/domain services own behavior; persistence and external-service details stay behind the project's established boundaries. Do not bypass an existing service to take a shortcut from a controller.
3. **Avoid artificial abstractions and dependency cycles.** Create shared providers/utilities only when ownership or reuse justifies them. Treat circular dependencies as a design problem; do not add `forwardRef()` merely to hide one without a documented reason.
4. **Do not change cross-cutting behavior for a local task.** Global guards, pipes, filters, interceptors, middleware, configuration, and shared providers affect other features. Change them only when the requested behavior truly requires that scope.

## Contracts and security

5. **Protect existing API contracts.** Do not silently change endpoints, DTO meaning, response shapes, status codes, pagination, or error behavior. Surface breaking changes and coordinate their impact before implementation.
6. **Keep trust boundaries explicit.** Validation, authentication, authorization, and business invariants are different checks. Do not treat a valid DTO or authenticated user as proof they can read or change a particular resource. Enforce ownership/permissions on the server.
7. **Fail truthfully and safely.** Do not swallow exceptions, turn failures into successful responses, expose internal details to clients, or log credentials, secrets, tokens, or unnecessary private data. Follow existing error contracts and logging conventions.

## Data and external effects

8. **Protect consistency on writes.** When one operation changes multiple related records, use the project's established transactional/concurrency pattern. Do not introduce race-prone read-then-write logic or assume a database rollback can undo an external action.
9. **Treat external effects differently from ordinary reads.** Publishing, payment, messaging, and other consequential calls require the appropriate authorization, timeout, retry, and duplicate-prevention strategy. Never blindly replay an operation whose external outcome is uncertain.
10. **Do not make dangerous persistence changes casually.** Schema changes need a reviewed migration and compatibility plan. Never use destructive sync/reset behavior, backfill production data, or drop fields as an incidental part of another task.

## Quality and handoff

11. **Test the risky path, not only the happy path.** Cover changed authorization boundaries, validation failures, missing resources, conflicts, concurrent requests, and external-service failures as relevant. Use the repository's existing test tools; do not claim coverage that was not run.
12. **Make the smallest complete change.** Do not add libraries, background infrastructure, new architectural patterns, unrelated refactors, or mocked production behavior without a clear requirement. Report what changed, what was actually verified, and what remains blocked or unverified.
