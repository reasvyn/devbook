# Junior Backend Developer

## Description

What a junior backend developer should know — server-side fundamentals, API basics, database operations, and shipping simple backend features.

## Prerequisites

- Basic programming knowledge — variables, functions, control flow

## Learning Path

### Language Fundamentals

Pick **one** primary language:
- `🔴 CRITICAL` **Node.js / TypeScript** — modules, async/await, file system, HTTP module
- `🔴 CRITICAL` **Python** — functions, modules, virtual environments, pip
- `🔴 CRITICAL` **Go** — packages, structs, interfaces, goroutines basics
- `🔴 CRITICAL` **Java / C#** — classes, interfaces, dependency injection, build tools (Maven/Gradle)

### Database Basics

- `🔴 CRITICAL` [SQL](../../data-databases/sql/querying-data.md) — SELECT, INSERT, UPDATE, DELETE, WHERE, JOINs
- `🔴 CRITICAL` [Database design](../../data-databases/sql/database-design.md) — tables, columns, primary keys, foreign keys
- `🟠 HIGH` Connecting to a database from code — connection strings, querying
- `🟢 LOW` [Basic indexing](../../data-databases/sql/database-design.md) — what it is and why it matters

### API Development

- `🔴 CRITICAL` [HTTP methods](../../networks/http-api/http-methods-and-status.md) — GET, POST, PUT, DELETE, PATCH
- `🔴 CRITICAL` [RESTful API design](../../networks/http-api/rest-api-design.md) — routes, status codes, request/response
- `🔴 CRITICAL` Request handling — query params, path params, request body
- `🔴 CRITICAL` [JSON](../../networks/http-api/headers-body-and-json.md) — parsing, serialization, content-type headers
- `🟠 HIGH` Input validation — sanitizing and validating request data
- `🟠 HIGH` Error handling — meaningful error messages, error status codes

### Version Control

- `🔴 CRITICAL` [Git basics](../../software/version-control/git-basics.md) — add, commit, push, pull, branch, merge
- `🔴 CRITICAL` Pull requests — creating and responding to review feedback
- `🟠 HIGH` .gitignore — what to exclude from version control

### Testing

- `🟠 HIGH` [Writing unit tests](../../software/software-testing/test-types.md) for business logic
- `🟡 MEDIUM` API testing with Supertest, pytest, or httptest

### Security Basics

- `🔴 CRITICAL` Never trust user input — validation, sanitization
- `🔴 CRITICAL` SQL injection — what it is and how to prevent it (parameterized queries)
- `🟠 HIGH` Password hashing — bcrypt, never store plaintext
- `🟠 HIGH` HTTPS — why it matters for APIs

### Deployment Basics

- `🟠 HIGH` Environment variables — storing config outside code
- `🟡 MEDIUM` Deploying a simple API to a cloud platform (Render, Railway, Fly.io)

### Soft Skills

- `🔴 CRITICAL` Reading and understanding logs
- `🔴 CRITICAL` Asking for help effectively — what to include in a question
- `🟠 HIGH` Documenting APIs — README, Swagger basics

## Next Steps

- [Mid Backend Developer](../mid/backend-developer.md) — deeper architecture, caching, scaling
