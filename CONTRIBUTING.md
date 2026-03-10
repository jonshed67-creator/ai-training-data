# Contributing

Thanks for your interest in contributing to the Claude Code Skills Toolkit! This guide will help you create and submit high-quality skills.

## How to Contribute

### Adding a New Skill

1. **Fork** this repository
2. **Create a branch**: `git checkout -b add-skill-name`
3. **Create the skill directory**: `skills/your-skill-name/`
4. **Write the SKILL.md** following the format below
5. **Test it** by copying it into a project's `.claude/skills/` directory and verifying it triggers correctly
6. **Open a pull request** with a clear description of what the skill does

### Improving an Existing Skill

1. Fork and branch as above
2. Make your changes to the existing `SKILL.md`
3. Explain in the PR what you changed and why

### Reporting Issues

Open an issue if:
- A skill doesn't trigger when it should
- A skill produces incorrect or unhelpful output
- You have an idea for a new skill but don't want to build it yourself

## SKILL.md Format

Every skill must follow this structure:

```yaml
---
name: skill-name-in-kebab-case
description: >
  TRIGGER: Use this skill when the user asks to [specific actions].
  Include keywords, phrases, and contexts that should activate the skill.
  Be detailed — a vague description means the skill won't trigger reliably.
---

# Skill Title

Clear instructions for Claude on how to perform this task.
```

### Required Fields

| Field | Description |
|-------|-------------|
| `name` | Kebab-case identifier (e.g., `git-commit-crafter`) |
| `description` | Detailed trigger description — when should this skill activate? |

### Writing Good Instructions

- **Use imperative form**: "Scan the codebase" not "You should scan the codebase"
- **Be specific**: Include exact commands, formats, and examples
- **Explain why**: Don't just say what to do — explain why it matters
- **Include edge cases**: What happens with empty input? Huge input? Weird formats?
- **Show examples**: Input/output examples make instructions unambiguous
- **Keep it under 500 lines**: If it's longer, consider splitting into multiple skills

### Writing Good Trigger Descriptions

The `description` field determines when your skill activates. Make it reliable:

**Good:**
```yaml
description: >
  TRIGGER: Use this skill when the user asks to format meeting notes,
  clean up meeting notes, organize meeting notes, "format these notes",
  "structure my notes", or when the user pastes messy text that appears
  to be from a meeting.
```

**Bad:**
```yaml
description: Formats notes.
```

Include:
- Action verbs the user might use
- Quoted phrases for exact matches
- Context descriptions for implicit triggers
- Related tasks that should also activate the skill

## Directory Structure

```
skills/your-skill-name/
├── SKILL.md          # Required — the skill definition
├── scripts/          # Optional — helper scripts for deterministic tasks
├── references/       # Optional — reference documents the skill reads
└── assets/           # Optional — templates, images, or other static files
```

## Quality Checklist

Before submitting, verify:

- [ ] YAML frontmatter parses correctly (test with a YAML validator)
- [ ] `name` field matches the directory name
- [ ] `description` is detailed enough to trigger reliably
- [ ] Instructions are clear and actionable
- [ ] Examples demonstrate real use cases
- [ ] Edge cases are documented
- [ ] The skill is under 500 lines
- [ ] No placeholder or "coming soon" content
- [ ] The skill solves a real problem

## Code of Conduct

Be kind, be helpful, be constructive. We're all here to make better tools.
