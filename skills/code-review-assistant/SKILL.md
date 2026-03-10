---
name: code-review-assistant
description: >
  TRIGGER: Use this skill when the user asks to review code, check code quality, audit
  code, find bugs, "review this code", "check for issues", "code review", "find problems",
  "audit my code", "check for security issues", "is this code okay", "what's wrong with
  this code", "improve this code", or when the user shares code and wants feedback on
  quality, security, performance, or best practices. Produces a structured review with
  severity levels, specific issues, and suggested fixes.
---

# Code Review Assistant

Review code for security vulnerabilities, performance issues, accessibility gaps, style inconsistencies, and logical errors. Produce a structured, actionable review that helps developers ship better code.

## Step-by-Step Process

### 1. Understand the Context

Before reviewing, determine:

- **What language/framework** is being used
- **What the code does** — read surrounding code if needed for context
- **Is this a PR review or a full-file review** — scope the review accordingly
- **Are there project conventions** — check for linting configs, .editorconfig, existing patterns

### 2. Review Categories

Analyze the code across these dimensions:

#### Security
Look for:
- SQL injection (string concatenation in queries)
- XSS (unescaped user input in HTML output)
- Command injection (user input in shell commands)
- Path traversal (user input in file paths)
- Insecure deserialization
- Hardcoded secrets, API keys, passwords
- Missing input validation at trust boundaries
- Insecure cryptographic practices (MD5, SHA1 for passwords)
- Missing authentication/authorization checks
- CSRF vulnerabilities
- Open redirects
- Prototype pollution (JavaScript)

#### Performance
Look for:
- N+1 database queries
- Missing database indexes (queries on unindexed columns)
- Unbounded queries (no LIMIT clause)
- Memory leaks (event listeners not removed, unclosed resources)
- Unnecessary re-renders (React: missing memo, unstable references)
- Blocking the event loop (sync I/O in async contexts)
- Redundant computations inside loops
- Missing caching for expensive operations
- Large bundle imports when a smaller import is available

#### Correctness
Look for:
- Off-by-one errors
- Race conditions
- Unhandled promise rejections / exceptions
- Incorrect null/undefined handling
- Type mismatches
- Edge cases not covered (empty arrays, zero values, negative numbers)
- Incorrect comparison operators (`==` vs `===` in JS)
- Missing return statements
- Mutation of shared state

#### Maintainability
Look for:
- Functions longer than 50 lines
- Deeply nested conditionals (3+ levels)
- Magic numbers without named constants
- Dead code or unused variables
- Duplicated logic that should be abstracted
- Unclear variable/function names
- Missing error handling
- Inconsistent patterns within the same codebase

#### Accessibility (for UI code)
Look for:
- Missing alt text on images
- Click handlers on non-interactive elements (div, span)
- Missing ARIA labels on icon buttons
- Insufficient color contrast
- Missing form labels
- Missing keyboard navigation support
- Auto-playing media without controls

### 3. Severity Levels

Assign a severity to each issue:

| Severity | Meaning | Icon |
|----------|---------|------|
| **Critical** | Security vulnerability, data loss risk, or crash. Must fix. | `[CRITICAL]` |
| **Warning** | Bug, performance issue, or bad practice. Should fix. | `[WARNING]` |
| **Info** | Style, readability, or minor improvement. Nice to fix. | `[INFO]` |

### 4. Output Format

Structure the review like this:

```markdown
## Code Review Summary

**Files reviewed:** list of files
**Overall assessment:** Brief 1-2 sentence summary

### Critical Issues

#### [CRITICAL] SQL Injection in user search — `src/api/users.js:42`

The search query concatenates user input directly into the SQL string,
allowing SQL injection attacks.

**Current code:**
```js
const results = await db.query(`SELECT * FROM users WHERE name = '${req.query.name}'`);
```

**Suggested fix:**
```js
const results = await db.query('SELECT * FROM users WHERE name = $1', [req.query.name]);
```

**Why this matters:** An attacker can extract, modify, or delete any data
in the database by crafting a malicious search query.

---

### Warnings

#### [WARNING] N+1 query in post listing — `src/api/posts.js:28`

Each post triggers a separate query to fetch its author. With 100 posts,
this sends 101 database queries instead of 2.

**Suggested fix:** Use a JOIN or batch the author lookups.

---

### Suggestions

#### [INFO] Consider extracting magic number — `src/utils/retry.js:15`

The number `3` appears without context. A named constant like
`MAX_RETRY_ATTEMPTS` makes the intent clear.
```

### 5. Review Principles

- **Be specific**: Point to exact lines. Show the problematic code and the fix.
- **Explain why**: Don't just say "this is bad." Explain the consequence of the issue.
- **Suggest fixes**: Every issue should have a concrete suggested fix, not just a complaint.
- **Prioritize**: Critical → Warning → Info. A review with 30 style nits and a buried security issue is a bad review.
- **Be constructive**: Acknowledge good patterns you see. "Good use of parameterized queries here" reinforces good behavior.
- **Don't nitpick style**: If the project has a linter, leave formatting to the linter. Focus on substance.
- **Stay in scope**: Review the code that was written/changed, not the entire surrounding codebase.

## Edge Cases

- **Generated code**: If reviewing generated code (protobuf, GraphQL codegen, Prisma client), note that it's generated and focus only on how it's used.
- **Test code**: Be less strict about DRY and abstraction in tests. Test readability matters more than test elegance.
- **Prototype / PoC code**: Note if the code appears to be a prototype. Still flag security issues but lighten up on architecture concerns.
- **Large PRs**: If the PR is 500+ lines, focus on architecture and security first. Suggest the author break future PRs into smaller pieces.
- **No issues found**: Say so explicitly. "No significant issues found. The code is clean, well-structured, and follows project conventions."
