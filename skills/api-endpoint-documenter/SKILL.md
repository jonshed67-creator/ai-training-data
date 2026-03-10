---
name: api-endpoint-documenter
description: >
  TRIGGER: Use this skill when the user asks to document API endpoints, generate API docs,
  create API documentation, document routes, "document my API", "generate endpoint docs",
  "write API reference", "create route documentation", or when the user points to route
  files and wants documentation generated. Works with Express, FastAPI, Flask, Django REST,
  Hono, Koa, NestJS, and other web frameworks. Produces clean markdown API documentation
  with request/response examples, status codes, and authentication requirements.
---

# API Endpoint Documenter

Generate clean, developer-friendly API documentation from route/endpoint source files. Read the actual code to produce accurate documentation with request/response examples, status codes, and auth requirements.

## Step-by-Step Process

### 1. Identify Route Files

If the user doesn't specify files, search for them:

```bash
# Express / Node.js
find . -type f -name "*.js" -o -name "*.ts" | xargs grep -l "router\.\|app\.get\|app\.post\|app\.put\|app\.delete\|app\.patch"

# FastAPI / Python
find . -type f -name "*.py" | xargs grep -l "@app\.\|@router\.\|APIRouter"

# Flask
find . -type f -name "*.py" | xargs grep -l "@app\.route\|@bp\.route\|Blueprint"

# Django REST
find . -type f -name "*.py" | xargs grep -l "class.*ViewSet\|class.*APIView\|urlpatterns"
```

### 2. Parse Each Endpoint

For every route/endpoint found, extract:

| Field | How to Find It |
|-------|---------------|
| **Method** | `GET`, `POST`, `PUT`, `PATCH`, `DELETE` from decorator/method call |
| **Path** | URL pattern from route definition |
| **Description** | Docstring, comments above handler, or infer from function name |
| **Parameters** | Path params (`:id`, `{id}`), query params, request body schema |
| **Request body** | Look for body parsing, validation schemas (Zod, Pydantic, Joi) |
| **Response** | Look for `res.json()`, `return`, response models, serializers |
| **Status codes** | Look for explicit status codes in responses |
| **Auth** | Check for auth middleware, decorators (`@login_required`, `authenticate`) |
| **Middleware** | Note rate limiting, CORS, validation middleware |

### 3. Document Each Endpoint

Use this template for each endpoint:

```markdown
### METHOD /path/to/resource

Brief description of what this endpoint does.

**Authentication:** Required / Optional / None

**Parameters:**

| Name | In | Type | Required | Description |
|------|-----|------|----------|-------------|
| id | path | string | Yes | Resource identifier |
| page | query | integer | No | Page number (default: 1) |
| limit | query | integer | No | Items per page (default: 20) |

**Request Body:**

```json
{
  "field": "value",
  "nested": {
    "key": "value"
  }
}
```

**Response:**

`200 OK`
```json
{
  "id": "abc123",
  "field": "value",
  "createdAt": "2024-01-15T10:30:00Z"
}
```

**Error Responses:**

| Status | Description |
|--------|-------------|
| 400 | Invalid request body |
| 401 | Missing or invalid authentication |
| 404 | Resource not found |
| 409 | Resource already exists |
```

### 4. Organize the Documentation

Group endpoints logically:

```markdown
# API Reference

Base URL: `https://api.example.com/v1`

## Authentication
Describe the auth mechanism (Bearer token, API key, OAuth, etc.)

## Users
### POST /users
### GET /users
### GET /users/:id
### PUT /users/:id
### DELETE /users/:id

## Posts
### GET /posts
### POST /posts
...
```

Group by resource, not by HTTP method. Order CRUD operations logically: Create (POST), Read (GET list), Read (GET single), Update (PUT/PATCH), Delete (DELETE).

### 5. Add Global Information

Include at the top of the document:

- **Base URL**: The API base path
- **Authentication**: How auth works across the API
- **Rate Limiting**: If rate limiting middleware is present
- **Pagination**: Common pagination patterns used
- **Error Format**: Standard error response structure
- **Versioning**: API versioning strategy if apparent

## Output Format

Write the documentation as a single markdown file. Use h2 (`##`) for resource groups and h3 (`###`) for individual endpoints.

If the API is large (20+ endpoints), suggest splitting into separate files per resource group.

## Edge Cases

- **No validation schemas**: Infer request body shape from how the body is used in the handler code. Note that the schema was inferred and may be incomplete.
- **Dynamic routes**: Document route parameters clearly, including any regex constraints.
- **File uploads**: Note multipart/form-data content type and file field names.
- **WebSocket endpoints**: Document them separately with event names and payload formats.
- **GraphQL**: If the API is GraphQL, switch to documenting queries, mutations, and subscriptions instead of REST endpoints.
- **Undocumented middleware**: If auth or validation middleware is applied at the router level (not per-route), note that it applies to all endpoints in the group.
