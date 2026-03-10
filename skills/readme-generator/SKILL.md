---
name: readme-generator
description: >
  TRIGGER: Use this skill when the user asks to create a README, generate a README,
  write a README, make a README.md, "help me with my README", "create project documentation",
  "document my project", "I need a README", "write docs for my repo", or when the user
  has a project without a README and needs one created. This skill detects the tech stack
  and generates a complete, professional README.md with badges, install instructions,
  usage examples, and proper structure.
---

# README Generator

Create professional, comprehensive README.md files for any project. Detect the tech stack automatically and generate documentation that helps users understand, install, and use the project quickly.

## Step-by-Step Process

### 1. Analyze the Project

Scan the codebase to determine:

**Language & Framework Detection:**
- Check for `package.json` → Node.js (check `dependencies` for React, Vue, Express, etc.)
- Check for `requirements.txt`, `pyproject.toml`, `setup.py` → Python (check for Django, Flask, FastAPI)
- Check for `go.mod` → Go
- Check for `Cargo.toml` → Rust
- Check for `Gemfile` → Ruby
- Check for `pom.xml`, `build.gradle` → Java
- Check for `*.sln`, `*.csproj` → C# / .NET

**Project Type Detection:**
- CLI tool: has `bin` field in package.json, or uses argparse/click/commander
- Web app: has frontend framework dependencies
- API: has route definitions, Express/FastAPI/Flask
- Library: has `main`/`module` in package.json, or is pip-installable
- Monorepo: has `workspaces` or `lerna.json` or `packages/` directory

**Additional signals:**
- Check for Docker files → add Docker instructions
- Check for CI config → mention CI status
- Check for test framework → add testing section
- Check `.env.example` → document env vars
- Check for a LICENSE file → reference it

### 2. Generate the README Structure

Use this structure, omitting sections that don't apply:

```markdown
# Project Name

Brief, compelling one-line description.

[Badges row]

## Table of Contents (if README > 100 lines)

## Overview
2-3 sentences explaining what this project does, who it's for, and why it exists.

## Features
- Key feature 1
- Key feature 2

## Prerequisites
What needs to be installed before setup.

## Installation
Step-by-step install instructions.

## Usage
Code examples showing common use cases.

## API Reference (if applicable)
Key endpoints or public API surface.

## Configuration (if applicable)
Environment variables, config files.

## Development
How to set up for local development, run tests.

## Contributing
Link to CONTRIBUTING.md or brief guidelines.

## License
License type with link to LICENSE file.
```

### 3. Write Compelling Content

**Title**: Use the actual project name. Add a tagline underneath if the name isn't self-explanatory.

**Badges** — select relevant ones from:
```markdown
![License](https://img.shields.io/badge/license-MIT-blue.svg)
![Node Version](https://img.shields.io/badge/node-%3E%3D18-brightgreen)
![Python Version](https://img.shields.io/badge/python-3.10%2B-blue)
![Build Status](https://img.shields.io/github/actions/workflow/status/USER/REPO/ci.yml)
![npm version](https://img.shields.io/npm/v/PACKAGE)
![PyPI version](https://img.shields.io/pypi/v/PACKAGE)
```

Replace USER, REPO, and PACKAGE with actual values. Only include badges that would actually resolve — don't add a PyPI badge for a Node project.

**Installation** — provide copy-pasteable commands:
```bash
# Clone and install — always include both steps
git clone https://github.com/user/repo.git
cd repo
npm install  # or pip install, cargo build, etc.
```

**Usage** — show real code examples:
- Start with the simplest possible example
- Then show 1-2 more advanced use cases
- Use the actual API/CLI interface from the source code
- Include expected output where helpful

### 4. Format and Polish

- Use consistent heading levels (h2 for main sections, h3 for subsections)
- Add blank lines between sections for readability
- Keep line length reasonable (wrap prose at ~100 chars)
- Use code blocks with language identifiers for syntax highlighting
- Link to other docs in the repo where relevant
- Don't add a Table of Contents for short READMEs (under 100 lines)

## Output

Write the complete README.md content. If replacing an existing README, show the user the new version and ask before overwriting.

## Edge Cases

- **Monorepo**: Create a root README that links to sub-package READMEs. Briefly describe each package.
- **No source code yet**: Generate a template README with placeholder sections the user can fill in. Mark placeholders clearly with `<!-- TODO: ... -->` comments.
- **Private/internal project**: Skip badges that require public registries. Focus on development setup and internal contribution guidelines.
- **Existing README**: Read it first. Preserve any custom content, project-specific context, or sections the auto-generator wouldn't know about. Enhance rather than replace where possible.
- **Multiple languages**: Document the primary language prominently, mention others in a "Tech Stack" section.
