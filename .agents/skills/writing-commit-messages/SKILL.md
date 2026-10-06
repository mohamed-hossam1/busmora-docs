---
name: writing-commit-messages
description: Write clear, conventional commit messages with proper type prefixes and optional scopes (single line only, no body or footer).
user-invocable: true
---

# Writing Commit Messages

Write commit messages that are useful for humans and machines. Keep them strictly single-line without any body or footer.

## Format

```
<type>(<optional scope>): <subject>
```

Always use a single-line message. Do **not** add a `<body>` or `<footer>`.

### Subject Line Rules

- **50 characters or less** for the subject
- Use imperative mood: "add feature" not "added feature" or "adding feature"
- Don't capitalize the first letter after the type prefix
- No period at the end
- Do not include any body or footer

### Types

| Type | When to use |
|------|-------------|
| `feat` | New user-facing feature |
| `fix` | Bug fix |
| `refactor` | Code restructuring without behavior change |
| `docs` | Documentation changes |
| `test` | Adding or updating tests |
| `chore` | Build, CI, tooling, deps |
| `perf` | Performance improvement |
| `style` | Formatting, whitespace (not CSS) |
| `ci` | CI/CD pipeline changes |
| `revert` | Reverting a previous commit |

### Scope (Optional)

The area of the codebase affected:
- `feat(auth): add OAuth2 login flow`
- `fix(api): handle null response from payments endpoint`
- `refactor(db): extract query builder into module`

## Examples

Good:
```
feat(dashboard): add real-time notification bell
fix: resolve race condition in WebSocket reconnect
refactor(api): consolidate error handling middleware
test: add integration tests for payment webhook
chore: upgrade TypeScript to 5.4
```

Bad:
```
fixed stuff
WIP
update
changes
asdf
```

## When to Commit

- Each commit should represent one logical change
- Don't mix refactoring with feature work in the same commit
- Don't commit half-working code (use `git stash` instead)
- Commit early and often on feature branches, squash before merge if needed

## Breaking Changes

If the commit introduces a breaking change, append `!` after the type or scope:
- `feat(api)!: change auth token format`
