---
name: project-scaffolder
description: >
  TRIGGER: Use this skill when the user asks to scaffold a project, create a new project,
  set up a project, bootstrap a project, "start a new project", "create a new app",
  "set up a React project", "scaffold a Python CLI", "new Node API", "project boilerplate",
  "initialize a project", "create project structure", or when the user wants a fresh
  project with proper configuration, folder structure, and tooling. Supports React + Vite,
  Python CLI, Node.js API, and static HTML site stacks.
---

# Project Scaffolder

Set up complete, production-ready project boilerplates with proper folder structure, linting, formatting, testing, and configuration. Support multiple stacks and follow current best practices.

## Step-by-Step Process

### 1. Determine the Stack

Ask the user which stack they want, or detect from context:

| Stack | Best For |
|-------|----------|
| **React + Vite** | Single-page apps, dashboards, web apps |
| **Python CLI** | Command-line tools, scripts, automation |
| **Node.js API** | REST APIs, backend services, microservices |
| **Static HTML** | Landing pages, portfolios, simple sites |

### 2. Scaffold by Stack

---

#### React + Vite

```
project-name/
├── public/
│   └── favicon.svg
├── src/
│   ├── assets/
│   ├── components/
│   │   └── App.tsx
│   ├── hooks/
│   ├── pages/
│   ├── styles/
│   │   └── global.css
│   ├── utils/
│   ├── main.tsx
│   └── vite-env.d.ts
├── .eslintrc.cjs
├── .gitignore
├── .prettierrc
├── index.html
├── package.json
├── tsconfig.json
├── tsconfig.node.json
└── vite.config.ts
```

**package.json essentials:**
```json
{
  "scripts": {
    "dev": "vite",
    "build": "tsc && vite build",
    "preview": "vite preview",
    "lint": "eslint src --ext .ts,.tsx",
    "format": "prettier --write src"
  },
  "dependencies": {
    "react": "^19.0.0",
    "react-dom": "^19.0.0"
  },
  "devDependencies": {
    "@types/react": "^19.0.0",
    "@types/react-dom": "^19.0.0",
    "@vitejs/plugin-react": "^4.0.0",
    "eslint": "^9.0.0",
    "prettier": "^3.0.0",
    "typescript": "^5.0.0",
    "vite": "^6.0.0"
  }
}
```

Include a minimal working `App.tsx` that renders a welcome page — not a blank div, but something the user can see in the browser immediately.

---

#### Python CLI

```
project-name/
├── src/
│   └── project_name/
│       ├── __init__.py
│       ├── __main__.py
│       ├── cli.py
│       └── core.py
├── tests/
│   ├── __init__.py
│   └── test_core.py
├── .gitignore
├── .python-version
├── pyproject.toml
└── README.md
```

**pyproject.toml essentials:**
```toml
[project]
name = "project-name"
version = "0.1.0"
description = ""
requires-python = ">=3.10"
dependencies = ["click>=8.0"]

[project.scripts]
project-name = "project_name.cli:main"

[tool.pytest.ini_options]
testpaths = ["tests"]

[tool.ruff]
line-length = 88
target-version = "py310"

[tool.ruff.lint]
select = ["E", "F", "I", "N", "UP", "B"]
```

Include a working CLI with `click` that has a `--help` flag and at least one example command.

---

#### Node.js API

```
project-name/
├── src/
│   ├── routes/
│   │   ├── index.ts
│   │   └── health.ts
│   ├── middleware/
│   │   └── errorHandler.ts
│   ├── utils/
│   │   └── logger.ts
│   ├── app.ts
│   └── server.ts
├── tests/
│   └── health.test.ts
├── .env.example
├── .eslintrc.cjs
├── .gitignore
├── .prettierrc
├── package.json
├── tsconfig.json
└── nodemon.json
```

**package.json essentials:**
```json
{
  "scripts": {
    "dev": "nodemon",
    "build": "tsc",
    "start": "node dist/server.js",
    "lint": "eslint src --ext .ts",
    "test": "jest"
  },
  "dependencies": {
    "express": "^5.0.0",
    "cors": "^2.0.0",
    "helmet": "^8.0.0"
  },
  "devDependencies": {
    "@types/express": "^5.0.0",
    "@types/node": "^22.0.0",
    "typescript": "^5.0.0",
    "nodemon": "^3.0.0",
    "jest": "^30.0.0",
    "@types/jest": "^30.0.0",
    "ts-jest": "^30.0.0"
  }
}
```

Include a working health check endpoint, error handling middleware, and a structured logger.

---

#### Static HTML Site

```
project-name/
├── css/
│   └── style.css
├── js/
│   └── main.js
├── images/
│   └── .gitkeep
├── .gitignore
├── index.html
└── README.md
```

Include a clean, responsive `index.html` with:
- Proper meta tags (viewport, description, charset)
- Linked CSS and JS
- A minimal, good-looking default page
- CSS reset/normalize included

---

### 3. Universal Files

Every project gets these regardless of stack:

**.gitignore** — comprehensive for the stack:
- Node: `node_modules/`, `dist/`, `.env`, `.env.local`
- Python: `__pycache__/`, `*.pyc`, `.venv/`, `dist/`, `.env`
- General: `.DS_Store`, `Thumbs.db`, `*.log`, `.idea/`, `.vscode/` (except settings)

**README.md** — minimal but useful:
```markdown
# Project Name

Brief description.

## Getting Started

1. Install dependencies: `npm install` / `pip install -e .`
2. Start development: `npm run dev` / `python -m project_name`

## Scripts

- `npm run dev` — start dev server
- `npm run build` — build for production
- `npm test` — run tests
```

### 4. Post-Scaffold Steps

After creating the files:
1. Initialize git: `git init`
2. Tell the user what to do next (install dependencies, start dev server)
3. Don't auto-install dependencies — let the user decide when

## Edge Cases

- **Project name with special chars**: Sanitize for package names (lowercase, hyphens only)
- **Directory already exists**: Warn the user and ask before overwriting anything
- **Monorepo**: If the user wants multiple packages, set up a workspace structure with npm/pnpm workspaces or Python namespace packages
- **Custom stack**: If the user wants a stack not listed, adapt the closest template and customize. Ask about specific tooling preferences.
- **Existing project**: If scaffolding inside an existing project, only add missing config files. Don't overwrite existing source code.
