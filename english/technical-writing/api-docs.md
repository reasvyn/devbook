# API Reference Documentation

## Description

API reference documentation is the contract between your service and its consumers. This document covers how to write clear, comprehensive API docs — from endpoint definitions and request/response schemas to authentication, error handling, and versioning strategies.

## Prerequisites

- [Sentence Structure](../grammar-and-style/sentence-structure.md) — clear sentences are essential for precise API documentation
- [Style Guides](../grammar-and-style/style-guides.md) — understanding style conventions before writing API docs at scale

## Table of Contents

- [API Reference as a Contract](#api-reference-as-a-contract)
- [Documenting Endpoints](#documenting-endpoints)
- [Parameters](#parameters)
- [Request Bodies](#request-bodies)
- [Responses](#responses)
- [Error Handling and Status Codes](#error-handling-and-status-codes)
- [Authentication in API Docs](#authentication-in-api-docs)
- [Versioning Documentation](#versioning-documentation)
- [OpenAPI / Swagger Specification](#openapi--swagger-specification)
- [SDK and Client Library Docs](#sdk-and-client-library-docs)
- [Code Examples and Snippets](#code-examples-and-snippets)
- [Tools for API Documentation](#tools-for-api-documentation)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### API Reference as a Contract

An API reference is more than documentation — it is a contract. It specifies exactly what consumers can expect from your service. When the API and its documentation disagree, the contract is broken.

**The principle of truthful documentation.** Every statement in your API reference must be verifiable against the actual behavior of the API. Automated testing should enforce this:

```text
test("GET /users/:id returns 200 with user object") {
  const response = await api.get("/users/42");
  assert(response.status === 200);
  assert(response.body).toMatchSchema(userSchema);
}
```

**Consequences of broken contracts:**

- Consumer trust erodes when documented behavior does not match actual behavior
- Support costs rise as developers file tickets about undocumented behavior
- Integration bugs multiply when consumers code against inaccurate documentation
- API adoption slows as developers hesitate to depend on an unreliable interface

**Design-first vs. code-first documentation:**

```text
Design-first:          Code-first:
Write spec first       Write code first
Generate code from spec  Generate docs from code
Examples: OpenAPI, RAML   Examples: Swagger, Javadoc
Advantage: contract      Advantage: always in sync
always intentional      with the implementation
```

Aim for design-first when possible. The spec becomes the source of truth and code is generated from it. This approach prevents drift between documentation and implementation.

### Documenting Endpoints

Every endpoint needs four pieces of information stated clearly at the top of its documentation block:

```text
Endpoint: GET /api/v2/users/{userId}
Summary:  Retrieve a user by their unique identifier
Operation ID: getUserById
```

**Path construction.** Show the full URL template with path parameters in curly braces:

```text
https://api.example.com/v2/users/{userId}
https://api.example.com/v2/users/{userId}/orders/{orderId}
```

**HTTP methods and their semantics:**

```text
GET    — Retrieve a resource (idempotent, safe)
POST   — Create a resource (not idempotent)
PUT    — Replace a resource entirely (idempotent)
PATCH  — Partially update a resource (not necessarily idempotent)
DELETE — Remove a resource (idempotent)
```

**Organizing endpoints by resource:**

```text
Users
  GET    /users          — List all users
  POST   /users          — Create a user
  GET    /users/{id}     — Get a user by ID
  PATCH  /users/{id}     — Update a user
  DELETE /users/{id}     — Delete a user

Orders
  GET    /users/{id}/orders    — List a user's orders
  POST   /users/{id}/orders    — Create an order for a user
  GET    /orders/{id}          — Get an order by ID
```

### Parameters

Document every parameter with name, location, type, required/optional, default value, and description.

**Parameter locations:**

```text
Path:     /users/{userId}           — part of the URL path
Query:    /users?role=admin         — after the ? in the URL
Header:   X-Request-ID: abc123      — HTTP header
Cookie:   sessionId=xyz789          — HTTP cookie
```

**Parameter documentation table:**

```text
| Parameter | Location | Type   | Required | Default | Description                         |
|-----------|----------|--------|----------|---------|-------------------------------------|
| userId    | path     | string | yes      | —       | Unique identifier for the user      |
| role      | query    | string | no       | all     | Filter users by role                |
| page      | query    | integer| no       | 1       | Page number for paginated results   |
| limit     | query    | integer| no       | 20      | Number of results per page (max 100)|
```

### Request Bodies

For POST, PUT, and PATCH endpoints, document the request body schema:

```text
POST /api/v2/users
Content-Type: application/json

{
  "email": "user@example.com",
  "name": "Jane Smith",
  "role": "editor"
}
```

**Schema documentation table:**

```text
| Field     | Type   | Required | Description                           |
|-----------|--------|----------|---------------------------------------|
| email     | string | yes      | User's email address. Must be unique. |
| name      | string | yes      | User's full display name.             |
| role      | string | no       | User role. Default: "viewer".         |
| avatarUrl | string | no       | URL to the user's avatar image.       |
```

### Responses

Document every possible response with status code, body schema, and headers.

**Success response:**

```text
HTTP/1.1 200 OK
Content-Type: application/json

{
  "id": "usr_abc123",
  "email": "jane@example.com",
  "name": "Jane Smith",
  "role": "editor",
  "createdAt": "2025-03-15T10:30:00Z"
}
```

**Response schema documentation:**

```text
| Field      | Type   | Description                                |
|------------|--------|--------------------------------------------|
| id         | string | Unique user identifier (prefix: usr_)      |
| email      | string | User's email address                       |
| name       | string | User's display name                        |
| role       | string | User role (enum: admin, editor, viewer)    |
| createdAt  | string | ISO 8601 timestamp of account creation     |
```

**Paginated list response:**

```text
HTTP/1.1 200 OK
Content-Type: application/json

{
  "data": [
    { "id": "usr_001", "name": "Alice" },
    { "id": "usr_002", "name": "Bob" }
  ],
  "pagination": {
    "page": 1,
    "limit": 20,
    "total": 142,
    "hasMore": true
  }
}
```

### Error Handling and Status Codes

Document every error response your API can return. Use standard HTTP status codes consistently.

**Standard status codes for REST APIs:**

```text
200 OK          — Request succeeded
201 Created     — Resource created successfully
204 No Content  — Request succeeded, no response body (DELETE)
400 Bad Request — Invalid input from client
401 Unauthorized — Missing or invalid authentication
403 Forbidden   — Authenticated but not authorized
404 Not Found   — Resource does not exist
409 Conflict    — Resource state conflict (duplicate, stale)
422 Unprocessable Entity — Validation failed
429 Too Many Requests — Rate limit exceeded
500 Internal Server Error — Unexpected server error
503 Service Unavailable — Temporary outage
```

**Error response format:**

```text
HTTP/1.1 400 Bad Request
Content-Type: application/json

{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "The request body contains invalid fields.",
    "details": [
      {
        "field": "email",
        "message": "Must be a valid email address",
        "code": "INVALID_FORMAT"
      }
    ],
    "requestId": "req_abc123"
  }
}
```

**Best practices for error responses:**

```text
// Good — actionable error messages
"message": "The 'email' field must be a valid email address. Received: 'not-an-email'"

// Bad — vague error messages
"message": "Invalid input"

// Good — machine-readable error codes
"code": "VALIDATION_ERROR"
```

### Authentication in API Docs

Document how consumers authenticate. Show complete examples for each supported method.

**API key authentication:**

```text
Authorization: Bearer sk_live_abc123def456
```

**OAuth 2.0 authentication:**

```text
1. Direct user to authorization URL
2. User authorizes the application
3. Exchange code for access token
4. Use token: Authorization: Bearer <token>
```

### Versioning Documentation

API versioning strategies affect how you structure your documentation.

**URL path versioning:**

```text
https://api.example.com/v1/users
https://api.example.com/v2/users
```

**Header versioning:**

```text
Accept: application/vnd.example.v2+json
```

**Documenting breaking changes:**

```text
Changelog: API v2.1 to v2.2 (2025-06-01)

Additions:
- GET /users/{id}/preferences — retrieve user preferences

Breaking changes:
- None in this release.

Deprecations:
- The `title` field in user profiles is deprecated.
  It will be removed in v3. Use `displayName` instead.
```

**Sunsetting policy documentation:**

```text
Version 1 End of Life Timeline:
- 2025-03-01: Deprecation announced
- 2025-06-01: Last day for new integrations
- 2025-09-01: Last day for existing integrations
- 2025-12-01: v1 shutdown

Migration resources:
- Migration guide from v1 to v2
- Changelog of breaking changes
- Support contact for migration assistance
```

### OpenAPI / Swagger Specification

The OpenAPI Specification (formerly Swagger) is the industry standard for describing REST APIs.

**Basic OpenAPI structure:**

```yaml
openapi: "3.1.0"
info:
  title: Example API
  description: API for managing users and orders
  version: "2.0.0"
  contact:
    name: API Support
    email: support@example.com
servers:
  - url: https://api.example.com/v2
    description: Production server
paths:
  /users:
    get:
      summary: List all users
      parameters:
        - name: role
          in: query
          schema:
            type: string
            enum: [admin, editor, viewer]
      responses:
        "200":
          description: A list of users
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: "#/components/schemas/User"
```

**Component schemas:**

```yaml
components:
  schemas:
    User:
      type: object
      properties:
        id:
          type: string
          description: Unique user identifier
          example: usr_abc123
        email:
          type: string
          format: email
        name:
          type: string
        role:
          type: string
          enum: [admin, editor, viewer]
      required:
        - id
        - email
        - name
    Error:
      type: object
      properties:
        error:
          type: object
          properties:
            code:
              type: string
            message:
              type: string
            requestId:
              type: string
```

**Security definitions:**

```yaml
components:
  securitySchemes:
    BearerAuth:
      type: http
      scheme: bearer
      bearerFormat: JWT
security:
  - BearerAuth: [read, write]
```

**Best practices for OpenAPI files:**

```text
Do:
- Use reusable components ($ref) to avoid duplication
- Include examples for every schema
- Document all error responses
- Use operationId for code generation
- Keep the spec in version control

Don't:
- Hardcode server URLs in examples
- Leave descriptions empty
- Use vague type definitions (type: object with no properties)
```

### SDK and Client Library Docs

If your API has official SDKs, document them alongside the API reference.

**SDK documentation structure:**

```text
Installation:
  npm install @example/api-client

Authentication:
  import { ExampleClient } from "@example/api-client";
  const client = new ExampleClient({ apiKey: "sk_live_..." });

Usage:
  const user = await client.users.get("usr_abc123");
  console.log(user.name); // "Jane Smith"
```

**Client method naming conventions:**

```text
GET    /users              -> client.users.list()
POST   /users              -> client.users.create(data)
GET    /users/{id}         -> client.users.get(id)
PATCH  /users/{id}         -> client.users.update(id, data)
DELETE /users/{id}         -> client.users.delete(id)
```

**Error handling in SDKs:**

```text
try {
  const user = await client.users.get("invalid_id");
} catch (error) {
  if (error instanceof ExampleApiError) {
    console.error(error.status);   // 404
    console.error(error.code);     // "RESOURCE_NOT_FOUND"
  }
}
```

### Code Examples and Snippets

Code examples are the most-read part of API documentation. Invest heavily in them.

**Multi-language examples.** Show the same request in every supported language:

```text
// cURL
curl -X GET "https://api.example.com/v2/users/usr_abc123" \
  -H "Authorization: Bearer sk_live_abc123"

// JavaScript (fetch)
const response = await fetch("https://api.example.com/v2/users/usr_abc123", {
  headers: { Authorization: "Bearer sk_live_abc123" }
});
const user = await response.json();

// Python
import requests
response = requests.get(
    "https://api.example.com/v2/users/usr_abc123",
    headers={"Authorization": "Bearer sk_live_abc123"}
)
user = response.json()
```

**Example quality guidelines:**

```text
Good: Realistic data, copy-pasteable, show request and response, tested against API
Bad: Placeholder data, truncated, only happy path, syntax errors
```

### Tools for API Documentation

**Swagger UI.** Renders OpenAPI specs as interactive documentation:

```text
Features:
- Renders endpoints with parameters, request bodies, and responses
- "Try it out" button to execute requests from the browser
- Code sample generation in multiple languages
- Authentication input fields

Setup: npm install swagger-ui-dist
```

**Redoc.** Clean, static rendering of OpenAPI specs:

```text
Features:
- Three-panel layout: sidebar, content, request/response
- Search functionality
- Auto-generated code samples
- Performance-focused (static HTML)

Setup: npx redoc-cli bundle spec.yaml
```

**ReadMe.** Hosted API documentation platform with OpenAPI support:

```text
Features:
- API reference from OpenAPI spec
- Dynamic code examples (multi-language)
- Changelog and version management
- User analytics
```

**Stoplight.** Full API design and documentation platform:

```text
Features:
- Visual API designer
- OpenAPI spec editor with validation
- Hosted documentation
- Mock servers
```

## Glossary

| Term | Definition |
|------|------------|
| API | Application Programming Interface — a set of defined rules for software communication |
| OpenAPI | A specification standard for describing REST APIs |
| Swagger | The original name for the OpenAPI Specification; also refers to a set of API tools |
| Endpoint | A specific URL path and HTTP method combination in an API |
| Path parameter | A variable in the URL path, denoted by curly braces: {userId} |
| Query parameter | A key-value pair appended to the URL after the ? character |
| Request body | The data sent to the server in a POST, PUT, or PATCH request |
| Response body | The data returned by the server in response to a request |
| Status code | A three-digit HTTP code indicating the result of a request |
| Schema | A definition of the structure and types of data in a request or response |
| Contract | The agreed-upon behavior between an API and its consumers |
| Design-first | Writing the API specification before implementing the code |
| Code-first | Implementing the API code and generating documentation from it |
| Idempotent | An operation that produces the same result regardless of how many times it is applied |
| SDK | Software Development Kit — a set of tools and libraries for using an API |
| Rate limiting | Restricting the number of API requests within a time window |
| OAuth 2.0 | An authorization framework for delegated access to APIs |
| Versioning | Managing changes to an API over time with numbered releases |
| Deprecation | Marking a feature as obsolete before its eventual removal |
| Contract testing | Automated tests that verify an API matches its specification |

## Quick References

- [OpenAPI Specification](https://spec.openapis.org/oas/latest.html) — official specification for describing REST APIs
- [Swagger UI](https://swagger.io/tools/swagger-ui/) — interactive API documentation renderer
- [Redoc](https://redocly.com/redoc) — clean static API documentation from OpenAPI
- [ReadMe](https://readme.com/) — hosted API documentation platform
- [Stoplight](https://stoplight.io/) — API design and documentation platform
- [HTTP Status Codes](https://httpstatuses.io/) — reference for HTTP status codes
- [JSON Schema](https://json-schema.org/) — vocabulary for annotating JSON documents

## Next Steps

- [READMEs](readmes.md) — using READMEs to introduce and document your projects
- [Tutorials](tutorials.md) — writing step-by-step guides that help developers get started
