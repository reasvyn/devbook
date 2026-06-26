# HTTP Methods and Status Codes

## Description

Every HTTP request carries a method that tells the server what action to perform, and every response carries a status code that tells the client whether the action succeeded or failed. This document covers the semantics of each standard method and status code, when to use them, and how to handle them in real applications.

## Prerequisites

- [What Is HTTP?](intro/what-is-http.md) — the request-response model, URL structure, and the role of headers

## Table of Contents

- [HTTP Methods](#http-methods)
- [HTTP Status Codes](#http-status-codes)
- [Practical Examples](#practical-examples)
- [Study Cases](#study-cases)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### HTTP Methods

HTTP defines a set of request methods (also called verbs) that indicate the desired action for a given resource. Each method has specific semantics around safety and idempotency.

**Safe methods** are those that do not modify resources on the server. They are read-only. GET, HEAD, and OPTIONS are safe.

**Idempotent methods** produce the same server state regardless of how many times the request is repeated (assuming no concurrent interference). GET, HEAD, PUT, DELETE, OPTIONS, and TRACE are idempotent. POST and PATCH are not.

#### GET

GET retrieves the current representation of a resource. It is both safe and idempotent. GET requests must never change server state and may be cached, bookmarked, and freely retried.

```http
GET /users/42 HTTP/1.1
Host: api.example.com
```

```bash
curl https://api.example.com/users/42
curl "https://api.example.com/users?role=admin&page=2"
```

```python
import requests
response = requests.get("https://api.example.com/users/42")
print(response.json())
```

```javascript
const response = await fetch("https://api.example.com/users/42");
const data = await response.json();
```

A successful GET returns 200 OK. A GET for a non-existent resource returns 404 Not Found.

#### POST

POST submits data to be processed by the identified resource. It is neither safe nor idempotent. Multiple identical POST requests may create multiple resources or produce different outcomes.

```http
POST /users HTTP/1.1
Host: api.example.com
Content-Type: application/json

{"name": "Alice", "role": "admin"}
```

```bash
curl -X POST https://api.example.com/users \
  -H "Content-Type: application/json" \
  -d '{"name": "Alice", "role": "admin"}'
```

```python
import requests
response = requests.post(
    "https://api.example.com/users",
    json={"name": "Alice", "role": "admin"}
)
print(response.status_code)
```

```javascript
const response = await fetch("https://api.example.com/users", {
    method: "POST",
    headers: {"Content-Type": "application/json"},
    body: JSON.stringify({name: "Alice", role: "admin"})
});
const data = await response.json();
```

A successful creation returns 201 Created with a Location header pointing to the new resource. POST is also used for actions that do not fit other methods, such as form submission, triggering a computation, or complex search payloads.

#### PUT

PUT replaces the entire resource at the given URL with the request payload. It is idempotent — sending the same PUT request multiple times produces the same state.

```http
PUT /users/42 HTTP/1.1
Host: api.example.com
Content-Type: application/json

{"name": "Alice", "role": "admin"}
```

```bash
curl -X PUT https://api.example.com/users/42 \
  -H "Content-Type: application/json" \
  -d '{"name": "Alice", "role": "admin"}'
```

```python
import requests
response = requests.put(
    "https://api.example.com/users/42",
    json={"name": "Alice", "role": "admin"}
)
```

```javascript
const response = await fetch("https://api.example.com/users/42", {
    method: "PUT",
    headers: {"Content-Type": "application/json"},
    body: JSON.stringify({name: "Alice", role: "admin"})
});
```

If the resource does not exist, the server may create it (201 Created). If it exists and is replaced, the server returns 200 OK or 204 No Content. PUT replaces the entire resource — partial updates should use PATCH.

#### PATCH

PATCH applies a partial modification to a resource. It is not necessarily idempotent, though using a patch format like JSON Patch (RFC 6902) can make it so.

```http
PATCH /users/42 HTTP/1.1
Host: api.example.com
Content-Type: application/json

{"role": "moderator"}
```

```bash
curl -X PATCH https://api.example.com/users/42 \
  -H "Content-Type: application/json" \
  -d '{"role": "moderator"}'
```

```python
import requests
response = requests.patch(
    "https://api.example.com/users/42",
    json={"role": "moderator"}
)
```

```javascript
const response = await fetch("https://api.example.com/users/42", {
    method: "PATCH",
    headers: {"Content-Type": "application/json"},
    body: JSON.stringify({role: "moderator"})
});
```

#### DELETE

DELETE removes the identified resource. It is idempotent — a second DELETE on the same URL has no further effect.

```http
DELETE /users/42 HTTP/1.1
Host: api.example.com
```

```bash
curl -X DELETE https://api.example.com/users/42
```

```python
import requests
response = requests.delete("https://api.example.com/users/42")
```

```javascript
const response = await fetch("https://api.example.com/users/42", {
    method: "DELETE"
});
```

A successful deletion returns 200 OK or 204 No Content. If the resource does not exist, the server should return 404 Not Found.

#### HEAD

HEAD is identical to GET except the server must not return a response body. It returns only the headers that would accompany a GET response. Useful for checking resource existence, Content-Length, or Last-Modified without transferring the full payload.

```http
HEAD /users/42 HTTP/1.1
Host: api.example.com
```

```bash
curl --head https://api.example.com/users/42
```

```python
import requests
response = requests.head("https://api.example.com/users/42")
print(response.headers["Content-Length"])
```

HEAD is safe and idempotent.

#### OPTIONS

OPTIONS discovers the communication options for the target resource. The response includes an Allow header listing supported methods. It is heavily used in CORS preflight requests — browsers send an OPTIONS request before certain cross-origin requests to check server permissions.

```http
OPTIONS /users HTTP/1.1
Host: api.example.com
```

```bash
curl -X OPTIONS https://api.example.com/users -v
```

```python
import requests
response = requests.options("https://api.example.com/users")
print(response.headers.get("Allow"))
```

OPTIONS is safe and idempotent.

#### Method Selection Guidelines

| Method | Safe | Idempotent | Use Case |
|--------|------|------------|----------|
| GET | Yes | Yes | Read a resource |
| HEAD | Yes | Yes | Read resource headers only |
| OPTIONS | Yes | Yes | Discover allowed methods |
| POST | No | No | Create a resource or trigger an action |
| PUT | No | Yes | Replace an entire resource |
| PATCH | No | No* | Partially update a resource |
| DELETE | No | Yes | Remove a resource |

\* PATCH can be idempotent depending on the patch format used.

**When to use each method:**

- Use GET for all read-only queries and list endpoints.
- Use POST for creating new resources and for actions that do not fit other methods.
- Use PUT when the client sends the full resource state and wants to replace what exists at that URL.
- Use PATCH for partial updates — only send the fields that changed.
- Use DELETE for removing resources.
- Use HEAD to check availability, size, or headers without downloading the body.
- Use OPTIONS to query server capabilities, especially for CORS preflight.

**Common mistakes:**

- Using POST for idempotent operations like updates. Use PUT or PATCH instead.
- Using GET with a request body (most servers and proxies ignore it).
- Using PUT to send only changed fields — that is what PATCH is for.
- Using POST for everything and ignoring method semantics entirely.

### HTTP Status Codes

Every HTTP response includes a three-digit status code that categorizes the result. The first digit defines the class:

- 1xx Informational — request received, continuing processing
- 2xx Success — request understood and accepted
- 3xx Redirection — further action needed
- 4xx Client Error — request cannot be fulfilled due to client mistake
- 5xx Server Error — server failed to fulfill a valid request

#### 1xx Informational

**100 Continue** — The server has received the request headers and the client should proceed to send the body. Used with the Expect: 100-continue header to avoid sending large payloads when the server will reject the request based on headers alone.

```http
HTTP/1.1 100 Continue
```

**101 Switching Protocols** — The server agrees to switch to a different protocol as requested by the client (e.g., Upgrade: websocket). This is the response that initiates a WebSocket connection.

```http
HTTP/1.1 101 Switching Protocols
Upgrade: websocket
Connection: Upgrade
```

1xx codes are provisional and rarely need to be handled directly in application code. The HTTP library typically absorbs them.

#### 2xx Success

**200 OK** — The standard success response. For GET, the body contains the resource. For POST, the body describes or contains the result.

```http
HTTP/1.1 200 OK
Content-Type: application/json

{"id": 42, "name": "Alice"}
```

**201 Created** — The request succeeded and a new resource was created. The response should include a Location header with the URL of the new resource.

```http
HTTP/1.1 201 Created
Location: /users/42
Content-Type: application/json

{"id": 42, "name": "Alice"}
```

**202 Accepted** — The request was accepted for processing but processing is not complete. Used for async operations such as batch jobs or message queue submissions. The response may include a Location header pointing to a status endpoint.

```http
HTTP/1.1 202 Accepted
Location: /jobs/abc123
```

**204 No Content** — The request succeeded but there is no content to return. Commonly used for DELETE, PUT, and PATCH responses when no response body is needed.

```http
HTTP/1.1 204 No Content
```

**206 Partial Content** — The server is delivering only part of the resource as requested by a Range header. Used for resumable downloads and streaming.

```http
HTTP/1.1 206 Partial Content
Content-Range: bytes 0-1023/8192
Content-Length: 1024
```

#### 3xx Redirection

**301 Moved Permanently** — The resource has been permanently moved to a new URL. Future requests should use the new URL provided in the Location header. Search engines update their index accordingly.

```http
HTTP/1.1 301 Moved Permanently
Location: https://api.example.com/v2/users/42
```

**302 Found** — The resource is temporarily located at a different URL. The client should use the Location header for this request but continue using the original URL for future requests.

```http
HTTP/1.1 302 Found
Location: /login
```

**304 Not Modified** — The resource has not changed since the conditional request (using If-Modified-Since or If-None-Match). Response has no body. The client should use its cached copy.

```http
HTTP/1.1 304 Not Modified
```

**307 Temporary Redirect** — Like 302, but the client must preserve the HTTP method. A 302 may change POST to GET; 307 guarantees the method is not changed.

```http
HTTP/1.1 307 Temporary Redirect
Location: /new-endpoint
```

**308 Permanent Redirect** — Like 301, but the client must preserve the HTTP method. A 301 may change POST to GET; 308 guarantees the method is not changed.

```http
HTTP/1.1 308 Permanent Redirect
Location: https://api.example.com/v2/users/42
```

Client libraries like `requests` and `fetch` follow redirects automatically by default. Manual handling is needed when you must inspect the redirect target or when cookies/auth headers must be preserved across redirects.

```python
import requests
# Disable automatic redirects to inspect them
response = requests.get("https://api.example.com/users/42", allow_redirects=False)
if response.status_code == 301:
    new_url = response.headers["Location"]
    print(f"Redirected to: {new_url}")
```

#### 4xx Client Error

**400 Bad Request** — The server cannot process the request due to malformed syntax, invalid field values, or a missing required parameter. The response body should describe the problem.

```http
HTTP/1.1 400 Bad Request
Content-Type: application/json

{"error": "email must be a valid email address"}
```

```python
import requests
response = requests.get("https://api.example.com/users", params={"page": -1})
if response.status_code == 400:
    error = response.json()
    print(f"Bad request: {error['error']}")
```

**401 Unauthorized** — The request requires authentication. The response must include a WWW-Authenticate header describing the auth scheme. The name is misleading — it means unauthenticated, not unauthorized.

```http
HTTP/1.1 401 Unauthorized
WWW-Authenticate: Bearer
```

```python
import requests
response = requests.get("https://api.example.com/admin")
if response.status_code == 401:
    print("Authentication required")
```

**403 Forbidden** — The server understood the request but refuses to authorize it. Unlike 401, authentication will not help — the client is known but lacks permission.

```http
HTTP/1.1 403 Forbidden
Content-Type: application/json

{"error": "insufficient permissions"}
```

**404 Not Found** — The server cannot find the requested resource. This may be permanent (resource never existed) or temporary (resource was moved or deleted).

```http
HTTP/1.1 404 Not Found
```

```python
import requests
response = requests.get("https://api.example.com/users/99999")
if response.status_code == 404:
    print("User not found")
```

**405 Method Not Allowed** — The request method is not supported for the target resource. The response must include an Allow header listing the supported methods.

```http
HTTP/1.1 405 Method Not Allowed
Allow: GET, HEAD, PUT
```

**409 Conflict** — The request conflicts with the current state of the server. Common in concurrent edit scenarios, versioning conflicts, or resource state constraints.

```http
HTTP/1.1 409 Conflict
Content-Type: application/json

{"error": "user already exists with this email"}
```

**422 Unprocessable Entity** — The request body is syntactically correct but semantically invalid. Used for validation errors where the format is fine but the values are not acceptable. Defined in RFC 4918 (WebDAV) but widely used in REST APIs.

```http
HTTP/1.1 422 Unprocessable Entity
Content-Type: application/json

{"errors": {"email": ["must be a valid email address"], "age": ["must be a positive integer"]}}
```

```python
import requests
response = requests.post(
    "https://api.example.com/users",
    json={"email": "not-an-email", "age": -5}
)
if response.status_code == 422:
    errors = response.json()["errors"]
    for field, messages in errors.items():
        print(f"{field}: {', '.join(messages)}")
```

**429 Too Many Requests** — The client has exceeded the rate limit. The response should include a Retry-After header indicating how long to wait.

```http
HTTP/1.1 429 Too Many Requests
Retry-After: 120
Content-Type: application/json

{"error": "rate limit exceeded, retry after 120 seconds"}
```

**Common 4xx mistakes:**

- Returning 401 instead of 403 when the client is authenticated but lacks permission.
- Returning 403 instead of 401 when no credentials were provided.
- Returning 200 with an error body for validation errors — use 400 or 422.
- Returning 500 for client errors — correctly classify the error as 4xx.
- Not including a descriptive error body with validation details.
- Returning 404 when the resource exists but the client lacks permission (this is sometimes intentional to avoid leaking information, but 403 is more honest).

#### 5xx Server Error

**500 Internal Server Error** — A generic server error when no more specific code applies. The client cannot fix this — the server has encountered an unexpected condition.

```http
HTTP/1.1 500 Internal Server Error
Content-Type: application/json

{"error": "an unexpected error occurred"}
```

**502 Bad Gateway** — The server, acting as a gateway or proxy, received an invalid response from an upstream server. Common in reverse proxy setups (nginx, Envoy) when the backend crashes or times out.

```http
HTTP/1.1 502 Bad Gateway
```

**503 Service Unavailable** — The server is temporarily unable to handle the request, typically due to maintenance or overload. The response should include a Retry-After header.

```http
HTTP/1.1 503 Service Unavailable
Retry-After: 3600
Content-Type: application/json

{"error": "service under maintenance"}
```

**504 Gateway Timeout** — The server, acting as a gateway or proxy, did not receive a timely response from an upstream server.

```http
HTTP/1.1 504 Gateway Timeout
```

**Common 5xx mistakes:**

- Returning 500 for validation errors — validation is a client error (400 or 422).
- Exposing stack traces or internal details in 500 response bodies.
- Not returning Retry-After with 503, leaving clients guessing when to retry.
- Using 502 when the upstream response is actually meaningful (e.g., a 400 from upstream should be relayed, not converted to 502).
- Never returning 5xx codes for client-correctable issues — they indicate server-side failure.

### Practical Examples

#### Handling status codes in client code

```python
import requests

def fetch_user(user_id):
    response = requests.get(f"https://api.example.com/users/{user_id}")
    if response.status_code == 200:
        return response.json()
    elif response.status_code == 401:
        # Token expired or missing — re-authenticate
        raise PermissionError("Authentication required")
    elif response.status_code == 403:
        raise PermissionError("Insufficient permissions")
    elif response.status_code == 404:
        return None
    elif response.status_code == 429:
        retry_after = int(response.headers.get("Retry-After", 60))
        raise RateLimitError(retry_after)
    else:
        response.raise_for_status()  # raises for 4xx/5xx
```

```javascript
async function fetchUser(userId) {
    const response = await fetch(`https://api.example.com/users/${userId}`);
    if (response.ok) {
        return await response.json();
    }
    switch (response.status) {
        case 401:
            throw new Error("Authentication required");
        case 403:
            throw new Error("Insufficient permissions");
        case 404:
            return null;
        case 429:
            const retryAfter = response.headers.get("Retry-After") || 60;
            throw new RateLimitError(retryAfter);
        default:
            throw new Error(`HTTP ${response.status}: ${response.statusText}`);
    }
}
```

## Study Cases

### Case 1: Using POST for an update operation

**Problem:** A developer implements an endpoint for updating a user's email address using POST.

```
POST /users/42/update-email
Body: {"email": "new@example.com"}
```

If the client retries the request (network timeout, user double-click), the email might be updated twice, or the server might process the same update multiple times. Because POST is not idempotent, there is no guarantee about the outcome of repeated requests.

**Solution:** Use PUT if the full resource is being replaced, or PATCH for partial updates. Both PUT and PATCH should be designed to be idempotent. For PUT, sending the full resource state produces the same result every time. For PATCH, using a format like JSON Patch (RFC 6902) ensures that applying the same patch twice is a no-op on the second application.

```
PUT /users/42
Body: {"name": "Alice", "email": "new@example.com", "role": "admin"}
```

### Case 2: Returning 200 for validation errors

**Problem:** An API returns HTTP 200 with `{"error": "email is invalid"}` when a user submits a bad email. The client code checks `response.ok` (or `response.status_code == 200`) and assumes success, proceeding to use the response data, which leads to a runtime error when the data is missing.

```python
# Client code that breaks
response = requests.post(url, json=data)
if response.ok:  # True for 200, but there's an error!
    user = response.json()
    print(user["name"])  # KeyError — "name" not in error response
```

**Solution:** Return 422 Unprocessable Entity with a structured error body. Client code can then distinguish between success and business-logic failures by checking the status code.

```python
response = requests.post(url, json=data)
if response.status_code == 422:
    errors = response.json()["errors"]
    # Handle validation errors
elif response.status_code == 201:
    user = response.json()
    # Proceed with created user
```

Using the correct status code makes the API more predictable and the client code more robust.

### Case 3: Not handling 429 rate limiting

**Problem:** A client makes requests in a tight loop without checking for 429 responses. When the rate limit is hit, the server starts returning 429, but the client ignores it and continues making requests. The server may eventually block the client entirely.

```python
# Naive client — 429s are ignored or treated as fatal
for user_id in user_ids:
    response = requests.get(f"https://api.example.com/users/{user_id}")
    user = response.json()  # Crashes if response was 429
```

**Solution:** Check for 429 and wait the specified time before retrying. Use exponential backoff for retries.

```python
import time

def fetch_with_retry(url, max_retries=5):
    for attempt in range(max_retries):
        response = requests.get(url)
        if response.status_code == 429:
            retry_after = int(response.headers.get("Retry-After", 2 ** attempt))
            print(f"Rate limited, waiting {retry_after}s")
            time.sleep(retry_after)
            continue
        response.raise_for_status()
        return response.json()
    raise Exception("Max retries exceeded")
```

```javascript
async function fetchWithRetry(url, maxRetries = 5) {
    for (let attempt = 0; attempt < maxRetries; attempt++) {
        const response = await fetch(url);
        if (response.status === 429) {
            const retryAfter = parseInt(response.headers.get("Retry-After") || Math.pow(2, attempt));
            console.log(`Rate limited, waiting ${retryAfter}s`);
            await new Promise(r => setTimeout(r, retryAfter * 1000));
            continue;
        }
        if (!response.ok) throw new Error(`HTTP ${response.status}`);
        return await response.json();
    }
    throw new Error("Max retries exceeded");
}
```

## Glossary

| Term | Definition |
|------|------------|
| GET | HTTP method that retrieves a resource representation; safe and idempotent |
| POST | HTTP method that submits data for processing; neither safe nor idempotent |
| PUT | HTTP method that replaces an entire resource; idempotent |
| PATCH | HTTP method that applies a partial modification to a resource |
| DELETE | HTTP method that removes a resource; idempotent |
| HEAD | HTTP method that retrieves only the response headers, identical to GET without the body |
| OPTIONS | HTTP method that discovers allowed communication methods for a resource |
| Safe method | An HTTP method that does not modify server state (GET, HEAD, OPTIONS) |
| Idempotent | A property where making the same request multiple times produces the same server state as making it once |
| 2xx | Class of HTTP status codes indicating success (e.g., 200 OK, 201 Created) |
| 3xx | Class of HTTP status codes indicating redirection (e.g., 301, 302, 304) |
| 4xx | Class of HTTP status codes indicating client error (e.g., 400, 404, 422) |
| 5xx | Class of HTTP status codes indicating server error (e.g., 500, 502, 503) |
| Redirect | An HTTP response (3xx) that tells the client to make a new request to a different URL |
| CORS preflight | An OPTIONS request sent by browsers before certain cross-origin requests to check server permissions |
| Rate limit | A server-enforced cap on the number of requests a client can make in a given time window |
| 201 Created | Success status code returned when a new resource has been created |
| 204 No Content | Success status code indicating the server completed the request but returns no body |
| 304 Not Modified | Redirection status indicating the cached resource is still valid |
| 400 Bad Request | Client error for malformed syntax or invalid parameters |
| 401 Unauthorized | Client error indicating missing or invalid authentication |
| 403 Forbidden | Client error indicating the authenticated user lacks permission |
| 404 Not Found | Client error indicating the resource does not exist |
| 405 Method Not Allowed | Client error indicating the HTTP method is not supported for this resource |
| 409 Conflict | Client error indicating a state conflict (e.g., duplicate resource) |
| 422 Unprocessable Entity | Client error for semantically invalid request bodies |
| 429 Too Many Requests | Client error indicating rate limit exceeded |
| 500 Internal Server Error | Server error for unexpected conditions |
| 502 Bad Gateway | Server error when an upstream server returns an invalid response |
| 503 Service Unavailable | Server error when the service is temporarily unavailable |
| 504 Gateway Timeout | Server error when an upstream server times out |

## Quick References

- [RFC 7231 — HTTP Semantics and Content](https://datatracker.ietf.org/doc/html/rfc7231) — defines the standard HTTP methods and status codes
- [RFC 5789 — PATCH Method for HTTP](https://datatracker.ietf.org/doc/html/rfc5789) — specification of the PATCH method
- [RFC 6585 — Additional HTTP Status Codes](https://datatracker.ietf.org/doc/html/rfc6585) — defines 428 Precondition Required and 429 Too Many Requests
- [MDN HTTP Request Methods](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods) — browser-friendly reference for all standard methods
- [MDN HTTP Status Codes](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status) — comprehensive reference for every status code

## Next Steps

- [Headers, Body & JSON](../headers-body-and-json.md) — how request and response bodies are structured with MIME types and content negotiation
