---
name: changelog-writer
description: >
  TRIGGER: Use this skill when the user asks to generate a changelog, write a CHANGELOG,
  create release notes, "what changed since last release", "write a CHANGELOG.md",
  "generate release notes", "summarize recent changes", "create a changelog from commits",
  "update the changelog", or when the user needs to document changes between versions,
  tags, or dates. Reads git log history and produces a clean, categorized CHANGELOG.md
  following the Keep a Changelog format.
---

# Changelog Writer

Generate clean, categorized changelogs from git history. Follow the Keep a Changelog format (https://keepachangelog.com/) to produce human-readable release notes that help users understand what changed between versions.

## Step-by-Step Process

### 1. Determine the Range

Ask the user or detect automatically:

```bash
# List recent tags to find the range
git tag --sort=-version:refname | head -20

# If the user specifies two tags
git log v1.2.0..v1.3.0 --oneline

# If the user specifies dates
git log --after="2024-01-01" --before="2024-02-01" --oneline

# If no range specified, use last tag to HEAD
LAST_TAG=$(git describe --tags --abbrev=0 2>/dev/null)
git log ${LAST_TAG}..HEAD --oneline
```

If there are no tags, use a date range or generate a changelog for the entire history.

### 2. Collect and Analyze Commits

```bash
# Get detailed commit info
git log v1.2.0..v1.3.0 --format="%H|%s|%b|%an|%ae|%ai" --no-merges
```

For each commit, determine its category based on:

1. **Conventional commit prefix**: `feat:` → Added, `fix:` → Fixed, etc.
2. **Commit message keywords**: "add", "new" → Added; "fix", "resolve", "patch" → Fixed
3. **Files changed**: README changes → docs; test files → internal; config → Changed
4. **Fallback**: If uncategorizable, put in "Changed"

### 3. Categorize Changes

Group into Keep a Changelog categories:

| Category | What Goes Here |
|----------|---------------|
| **Added** | New features, new files, new capabilities |
| **Changed** | Changes to existing functionality, refactors |
| **Deprecated** | Features marked for future removal |
| **Removed** | Removed features, deleted code |
| **Fixed** | Bug fixes |
| **Security** | Security-related fixes or improvements |

### 4. Write the Changelog

Use this format:

```markdown
# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/),
and this project adheres to [Semantic Versioning](https://semver.org/).

## [1.3.0] - 2024-02-15

### Added
- Add user avatar upload with image cropping support (#142)
- Add dark mode toggle to settings page (#138)
- Add CSV export for analytics dashboard (#135)

### Changed
- Upgrade React from 18.2 to 18.3 (#140)
- Improve search performance with debounced input (#137)

### Fixed
- Fix login redirect loop when session expires (#141)
- Fix incorrect date formatting in UTC-negative timezones (#136)
- Fix memory leak in WebSocket connection handler (#133)

### Security
- Update jsonwebtoken to patch CVE-2024-XXXXX (#139)

## [1.2.0] - 2024-01-10

### Added
- ...

[1.3.0]: https://github.com/user/repo/compare/v1.2.0...v1.3.0
[1.2.0]: https://github.com/user/repo/compare/v1.1.0...v1.2.0
```

### 5. Writing Rules

Follow these rules to make the changelog genuinely useful:

- **Write for humans, not machines.** "Fix crash when uploading large images" is better than "fix: handle edge case in upload handler"
- **Start each entry with a verb**: Add, Fix, Update, Remove, Improve, Upgrade
- **Include PR/issue numbers** when available: `(#142)`
- **Group related changes**: If 3 commits all fix the search feature, combine them into one entry
- **Skip noise**: Don't include typo fixes, merge commits, version bumps, or CI-only changes unless they're significant
- **Be specific**: "Fix login redirect loop when session expires" not "Fix login bug"
- **Note breaking changes prominently**: Add a `### BREAKING` section or prefix entries with `**BREAKING:**`

### 6. Detect Version Number

If the user hasn't specified a version number:

1. Look at the latest tag for the current version scheme
2. Analyze the changes to suggest a semver bump:
   - Breaking changes → major bump
   - New features → minor bump
   - Bug fixes only → patch bump
3. Suggest the version number but let the user decide

## Output

Write the changelog content and either:
- Create a new `CHANGELOG.md` file
- Prepend to an existing `CHANGELOG.md` (preserving older entries)

Always ask before overwriting an existing CHANGELOG.md.

## Edge Cases

- **No conventional commits**: Categorize by analyzing the diff and commit messages contextually. Most repos don't use conventional commits — the skill should still work well.
- **Squash merges**: Each squash merge is one entry. Read the squash commit body for individual changes.
- **Very active repos**: If 100+ commits, focus on user-facing changes and summarize internal refactors. Group related commits aggressively.
- **Monorepo**: Group changes by package/workspace. Use subheadings for each package.
- **No tags**: Use dates as version headers: `## [Unreleased] - 2024-02-15`
- **Pre-1.0**: Note that the API is unstable and breaking changes may happen in minor versions.
