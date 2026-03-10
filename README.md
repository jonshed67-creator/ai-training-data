# Claude Code Skills Toolkit

**Drop-in skill files that supercharge your Claude Code workflow.**

A curated collection of pre-made `SKILL.md` files for [Claude Code](https://docs.anthropic.com/en/docs/claude-code). Each skill is a self-contained document that teaches Claude how to perform a specific task — from crafting git commits to generating color palettes to formatting meeting notes.

No configuration. No dependencies. Just copy a `SKILL.md` into your project and go.

---

## Available Skills

| Skill | Description |
|-------|-------------|
| [git-commit-crafter](skills/git-commit-crafter/) | Generate conventional commit messages by analyzing staged changes |
| [readme-generator](skills/readme-generator/) | Create professional README.md files with auto-detected tech stack |
| [api-endpoint-documenter](skills/api-endpoint-documenter/) | Generate API documentation from Express, FastAPI, Flask route files |
| [color-palette-generator](skills/color-palette-generator/) | Build accessible color palettes with CSS vars, Tailwind config, and HTML preview |
| [changelog-writer](skills/changelog-writer/) | Produce Keep a Changelog formatted changelogs from git history |
| [env-scaffolder](skills/env-scaffolder/) | Scan codebases for env vars and generate documented .env.example files |
| [code-review-assistant](skills/code-review-assistant/) | Structured code reviews with severity levels, security checks, and fix suggestions |
| [project-scaffolder](skills/project-scaffolder/) | Bootstrap new projects — React + Vite, Python CLI, Node.js API, static HTML |
| [regex-builder](skills/regex-builder/) | Build regex from plain English with explanations, test cases, and multi-flavor output |
| [meeting-notes-formatter](skills/meeting-notes-formatter/) | Turn messy meeting notes into structured minutes with action items |
| [apple-design-system](skills/apple-design-system/) | Apple HIG reference for building iOS/macOS apps with correct design patterns |

---

## Quick Start

### 1. Pick a skill

Browse the [`skills/`](skills/) directory and find one you need.

### 2. Copy it into your project

```bash
# Copy a single skill
cp -r skills/git-commit-crafter/ your-project/.claude/skills/

# Or copy everything
cp -r skills/ your-project/.claude/skills/
```

### 3. Use it

Claude Code automatically detects skill files in your project's `.claude/skills/` directory. Just use Claude Code as you normally would — the skills activate based on context.

For example, after installing `git-commit-crafter`, just stage some changes and ask Claude to write a commit message. The skill handles the rest.

---

## How Skills Work

A Claude Code skill is a markdown file (`SKILL.md`) with YAML frontmatter that tells Claude **when** and **how** to perform a specific task.

```yaml
---
name: my-skill
description: >
  TRIGGER: Use this skill when the user asks to...
  (detailed description of when this skill should activate)
---

# My Skill

Instructions for Claude on how to perform this task.
Step-by-step process, examples, edge cases, etc.
```

**Key parts:**

- **`name`**: Identifier for the skill
- **`description`**: Controls when the skill triggers — be specific and include keywords, phrases, and contexts that should activate it
- **Body**: The actual instructions Claude follows — written in imperative form with clear steps

Skills live in your project's `.claude/skills/` directory (or subdirectories). Claude Code loads them automatically.

---

## Project Structure

```
skills/
├── git-commit-crafter/
│   └── SKILL.md
├── readme-generator/
│   └── SKILL.md
├── api-endpoint-documenter/
│   └── SKILL.md
├── color-palette-generator/
│   └── SKILL.md
├── changelog-writer/
│   └── SKILL.md
├── env-scaffolder/
│   └── SKILL.md
├── code-review-assistant/
│   └── SKILL.md
├── project-scaffolder/
│   └── SKILL.md
├── regex-builder/
│   └── SKILL.md
├── meeting-notes-formatter/
│   └── SKILL.md
└── apple-design-system/
    ├── SKILL.md
    └── references/
        └── apple-design-system-reference.md
```

---

## Contributing

We welcome contributions! Whether it's a new skill, an improvement to an existing one, or a bug fix.

### Adding a new skill

1. Fork this repo
2. Create a new directory under `skills/` with your skill name (use kebab-case)
3. Add a `SKILL.md` with proper YAML frontmatter (`name` and `description` fields)
4. Write clear, detailed instructions in the body
5. Open a pull request

### Skill quality guidelines

- **Be genuinely useful** — solve a real problem developers face
- **Write detailed trigger descriptions** — include specific phrases and contexts
- **Include examples** — show input/output where relevant
- **Handle edge cases** — document what happens with unusual inputs
- **Keep it under 500 lines** — if a skill is too long, it may need to be split
- **Test the YAML frontmatter** — make sure it parses correctly

See [CONTRIBUTING.md](CONTRIBUTING.md) for full guidelines.

---

## License

[MIT](LICENSE) — use these skills however you want.
