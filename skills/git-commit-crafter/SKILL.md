---
name: git-commit-crafter
description: >
  TRIGGER: Use this skill when the user asks to write a commit message, craft a commit,
  generate a commit, create a conventional commit, format staged changes as a commit,
  or says things like "commit this", "what should I commit", "write me a commit message",
  "conventional commit", "format my commit". Also trigger when the user has staged changes
  and asks for help describing them. This skill analyzes staged git changes and produces
  well-structured conventional commit messages with proper type, scope, and description.
---

# Git Commit Crafter

Generate conventional commit messages by analyzing staged git changes. Follow the Conventional Commits specification (https://www.conventionalcommits.org/) to produce consistent, meaningful commit messages that make git history useful.

## Step-by-Step Process

### 1. Inspect Staged Changes

Run these commands to understand what's been changed:

```bash
git diff --cached --stat
git diff --cached
git diff --cached --name-only
```

If nothing is staged, check `git status` and inform the user they need to stage changes first. Suggest specific files to stage based on what's modified.

### 2. Determine the Commit Type

Analyze the changes and select the most appropriate type:

| Type | When to Use |
|------|-------------|
| `feat` | A new feature or capability is added |
| `fix` | A bug is corrected |
| `docs` | Documentation-only changes (README, comments, JSDoc) |
| `style` | Formatting, semicolons, whitespace — no logic changes |
| `refactor` | Code restructuring without changing behavior |
| `perf` | Performance improvements |
| `test` | Adding or updating tests |
| `build` | Build system or dependency changes (webpack, npm, pip) |
| `ci` | CI/CD configuration changes (GitHub Actions, Jenkins) |
| `chore` | Maintenance tasks (updating .gitignore, tooling config) |
| `revert` | Reverting a previous commit |

If changes span multiple types, use the most significant one. A feature that also fixes a bug is a `feat`. A refactor that also updates tests is a `refactor`.

### 3. Detect the Scope

Scope narrows down what part of the codebase is affected. Detect it from:

- **Directory structure**: `src/auth/login.ts` → scope is `auth`
- **Module name**: changes in `api/users.py` → scope is `users`
- **Config type**: `.eslintrc` changes → scope is `lint`
- **Component name**: `Button.tsx` → scope is `button`

Rules for scope:
- Use lowercase, single-word scopes when possible
- Omit scope if changes span 3+ unrelated areas
- Use hyphenated names for multi-word scopes: `user-auth`
- Common scopes: `api`, `ui`, `db`, `auth`, `config`, `deps`, `cli`

### 4. Write the Subject Line

Format: `type(scope): description`

Rules:
- Use imperative mood: "add" not "added" or "adds"
- Don't capitalize the first letter of the description
- No period at the end
- Keep it under 72 characters total
- Be specific: "add email validation to signup form" not "update form"

### 5. Write the Body (When Needed)

Add a body when:
- The "why" isn't obvious from the subject
- Multiple files are changed
- There's important context reviewers need

Format:
- Separate from subject with a blank line
- Wrap at 72 characters
- Explain what changed and why, not how (the diff shows how)

### 6. Add Footer (When Needed)

Include footers for:
- **Breaking changes**: `BREAKING CHANGE: description of what breaks`
- **Issue references**: `Closes #123` or `Fixes #456`
- **Co-authors**: `Co-authored-by: Name <email>`

Breaking changes also get a `!` after the type/scope: `feat(api)!: change response format`

## Output Format

Present the commit message in a code block the user can copy:

```
type(scope): concise description of the change

Optional body explaining the motivation and context.
Describe what changed and why, not the implementation details.

Optional footer(s)
```

Then provide the ready-to-run git command:

```bash
git commit -m "type(scope): concise description" -m "Body text here"
```

## Examples

### Simple feature addition
```
feat(auth): add password strength indicator to signup form
```

### Bug fix with context
```
fix(api): prevent duplicate user creation on concurrent requests

The POST /users endpoint had no idempotency check, allowing race
conditions to create duplicate accounts. Add a unique constraint
on email and handle the conflict with a 409 response.

Closes #287
```

### Breaking change
```
feat(api)!: return paginated response from GET /users

BREAKING CHANGE: GET /users now returns { data: [], meta: { page, total } }
instead of a flat array. Clients must update to read from the data field.
```

### Multi-file refactor
```
refactor(db): extract query builders into dedicated modules

Split the monolithic database.js into focused modules:
- queries/users.js — user CRUD operations
- queries/posts.js — post and comment queries
- queries/analytics.js — reporting aggregations

No behavior changes. All existing tests pass without modification.
```

## Edge Cases

- **Merge commits**: Don't rewrite merge commit messages. Inform the user.
- **Empty diff**: If `git diff --cached` is empty, tell the user to stage changes.
- **Massive changes**: If 20+ files changed, suggest splitting into multiple commits and recommend a logical grouping.
- **Generated files**: Note if changes include generated files (lock files, build output) and suggest whether they should be in a separate commit.
- **Mixed concerns**: If staged changes clearly contain unrelated work (fixing a bug AND adding a feature), recommend splitting into separate commits with `git add -p`.
