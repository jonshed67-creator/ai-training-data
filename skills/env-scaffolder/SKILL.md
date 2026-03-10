---
name: env-scaffolder
description: >
  TRIGGER: Use this skill when the user asks to create a .env.example, scaffold environment
  variables, generate env template, "create .env.example", "find all env vars", "what env
  vars does this project need", "document environment variables", "scan for env vars",
  "make an env template", or when the user needs to document required environment variables
  for a project. Scans the codebase for environment variable references and generates a
  complete .env.example with descriptions and sensible defaults.
---

# Env Scaffolder

Scan a codebase for environment variable references and generate a complete `.env.example` file with descriptions, categories, and sensible default values.

## Step-by-Step Process

### 1. Scan for Environment Variable References

Search the codebase using these patterns:

**JavaScript / TypeScript:**
```bash
grep -rn "process\.env\." --include="*.js" --include="*.ts" --include="*.jsx" --include="*.tsx"
```

**Python:**
```bash
grep -rn "os\.environ\|os\.getenv\|environ\.get\|config(" --include="*.py"
```

**Go:**
```bash
grep -rn "os\.Getenv\|os\.LookupEnv\|viper\." --include="*.go"
```

**Ruby:**
```bash
grep -rn "ENV\[" --include="*.rb"
```

**Also check:**
- `.env` files (if they exist and aren't gitignored — warn if `.env` is committed!)
- Docker Compose files: `environment:` sections
- CI/CD configs: GitHub Actions `env:`, Jenkins `environment {}`
- Config files: YAML/JSON config that references env vars
- Validation schemas: Joi, Zod, or Pydantic models for config

### 2. Extract Variable Names

For each reference found, extract:
- **Variable name**: e.g., `DATABASE_URL`
- **Where it's used**: file and line number
- **Default value**: if one is provided (e.g., `process.env.PORT || 3000`)
- **Required or optional**: is there a fallback/default?

### 3. Categorize Variables

Group variables into logical categories:

| Category | Examples |
|----------|---------|
| **App** | `PORT`, `NODE_ENV`, `APP_URL`, `LOG_LEVEL` |
| **Database** | `DATABASE_URL`, `DB_HOST`, `DB_PORT`, `DB_NAME` |
| **Authentication** | `JWT_SECRET`, `SESSION_SECRET`, `OAUTH_CLIENT_ID` |
| **External APIs** | `STRIPE_KEY`, `AWS_ACCESS_KEY_ID`, `SENDGRID_API_KEY` |
| **Storage** | `S3_BUCKET`, `UPLOAD_DIR`, `CDN_URL` |
| **Email** | `SMTP_HOST`, `SMTP_PORT`, `FROM_EMAIL` |
| **Redis/Cache** | `REDIS_URL`, `CACHE_TTL` |
| **Feature Flags** | `ENABLE_BETA`, `FEATURE_NEW_UI` |

### 4. Generate .env.example

Write the file with this format:

```bash
# =============================================================================
# App Configuration
# =============================================================================

# The port the server listens on
PORT=3000

# Application environment (development, staging, production)
NODE_ENV=development

# Base URL of the application (used for generating links)
APP_URL=http://localhost:3000

# =============================================================================
# Database
# =============================================================================

# PostgreSQL connection string
# Format: postgresql://user:password@host:port/database
DATABASE_URL=postgresql://localhost:5432/myapp_dev

# =============================================================================
# Authentication
# =============================================================================

# Secret key for signing JWTs — generate with: openssl rand -hex 32
JWT_SECRET=

# Session secret — generate with: openssl rand -hex 32
SESSION_SECRET=

# =============================================================================
# External Services
# =============================================================================

# Stripe API keys — get from https://dashboard.stripe.com/apikeys
STRIPE_SECRET_KEY=
STRIPE_PUBLISHABLE_KEY=
STRIPE_WEBHOOK_SECRET=
```

### 5. Writing Rules

- **Add a comment above every variable** explaining what it does
- **Provide sensible defaults** for non-sensitive values (`PORT=3000`, `NODE_ENV=development`)
- **Leave sensitive values empty** (API keys, secrets, passwords)
- **Include generation hints** for secrets: `# Generate with: openssl rand -hex 32`
- **Add format examples** for complex values: `# Format: postgresql://user:pass@host:port/db`
- **Link to docs** for third-party services: `# Get from https://dashboard.stripe.com/apikeys`
- **Sort by importance**: required vars first, optional vars after
- **Mark optional vars**: add `(optional)` in the comment

### 6. Safety Checks

After generating the file:

1. **Check .gitignore**: Verify `.env` is in `.gitignore`. If not, warn the user and offer to add it.
2. **Check for committed secrets**: Look for `.env` files tracked by git. Warn if found.
3. **Check for hardcoded secrets**: Flag any API keys, passwords, or secrets hardcoded in source files.

## Output

- Write the `.env.example` file
- Report how many variables were found and categorized
- Flag any security concerns (committed .env, hardcoded secrets, missing .gitignore entry)

## Edge Cases

- **Multiple .env files**: Some projects use `.env.development`, `.env.production`, `.env.test`. Generate a base `.env.example` and note the environment-specific overrides.
- **Docker Compose env**: Cross-reference docker-compose.yml environment variables with the app's usage.
- **Monorepo**: Generate separate `.env.example` files per package, or a single root one with clear sections.
- **No env vars found**: The project might use a config file instead (JSON, YAML, TOML). Note this and offer to document the config file format instead.
- **Dynamic env var names**: Some code constructs env var names dynamically (`process.env[\`PREFIX_${name}\`]`). Flag these and document the pattern.
