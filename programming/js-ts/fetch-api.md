# Fetch API & HTTP

## Description

The Fetch API provides a modern, Promise-based interface for making HTTP requests in the browser. It replaces `XMLHttpRequest` with a cleaner, more flexible API that works seamlessly with async/await.

## Prerequisites

- [Events](events.md) — understanding asynchronous callbacks
- [Promises & Async/Await](promises-async-await.md) — Promise chaining and async patterns
- [Values & Types](values-and-types.md) — JSON parsing, type conversion

## Table of Contents

- [HTTP in the Browser](#http-in-the-browser)
- [Basic GET Request](#basic-get-request)
- [Working with Responses](#working-with-responses)
- [HTTP Headers](#http-headers)
- [POST & Other Methods](#post--other-methods)
- [Request Objects](#request-objects)
- [Error Handling](#error-handling)
- [AbortController & Cancellation](#abortcontroller--cancellation)
- [JSON: The Server Language](#json-the-server-language)
- [Uploading Files](#uploading-files)
- [Downloading Files](#downloading-files)
- [CORS & Cross-Origin Requests](#cors--cross-origin-requests)
- [Credentials & Authentication](#credentials--authentication)
- [Caching & Cache Control](#caching--cache-control)
- [Progress Events](#progress-events)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### HTTP in the Browser

Every web page makes dozens of HTTP requests: loading scripts, fetching data, submitting forms. The Fetch API gives you full control over this process.

```javascript
fetch("https://api.example.com/data");
```

The simplest call returns a Promise that resolves to a `Response` object.

The Fetch API operates on the **request-response cycle**:

```
Client (Browser)          Server
     │                      │
     ├── GET /api/data ────►│
     │                      ├── 200 OK
     │◄── { "data": [] } ──┤
     │                      │
     ├── POST /api/data ───►│
     │   { "name": "Alice"} │
     │                      ├── 201 Created
     │◄── { "id": 1 } ─────┤
     │                      │
```

### Basic GET Request

```javascript
fetch("https://api.github.com/users/octocat")
    .then(response => response.json())
    .then(data => console.log(data))
    .catch(error => console.error("Request failed:", error));
```

**With async/await:**

```javascript
async function getUser(username) {
    const response = await fetch(`https://api.github.com/users/${username}`);
    const data = await response.json();
    return data;
}

// Usage
const user = await getUser("octocat");
console.log(user.name);
```

### Working with Responses

The `Response` object provides methods and properties to inspect the server's reply.

**Status information:**

```javascript
const response = await fetch(url);

response.status;             // 200, 404, 500, etc.
response.statusText;         // "OK", "Not Found", "Internal Server Error"
response.ok;                 // true if status is 200-299
response.redirected;         // true if the request was redirected
response.type;               // "basic", "cors", "opaque", "opaqueredirect"
response.url;                // the final URL after any redirects
```

**Body reading methods — call exactly one:**

```javascript
response.json();             // parse as JSON
response.text();             // as plain text
response.blob();             // as binary blob (images, files)
response.arrayBuffer();      // as ArrayBuffer (binary data)
response.formData();         // as FormData object
```

The body can only be consumed once. Calling `response.json()` then `response.text()` throws:

```javascript
const data = await response.json();      // consumes body
const text = await response.text();      // TypeError: Already read
```

**Clone if you need to read twice:**

```javascript
const clone = response.clone();
const data = await response.json();
const raw = await clone.text();          // read from clone
```

**Headers:**

```javascript
response.headers.get("Content-Type");    // "application/json"
response.headers.get("X-RateLimit-Remaining");
response.headers.has("Set-Cookie");

// Iterating headers
for (const [key, value] of response.headers) {
    console.log(`${key}: ${value}`);
}
```

### HTTP Headers

Set request headers via the `headers` option:

```javascript
fetch("https://api.example.com/data", {
    headers: {
        "Content-Type": "application/json",
        "Authorization": `Bearer ${token}`,
        "Accept": "application/json",
        "X-API-Key": apiKey
    }
});
```

**Common content types:**

| Content-Type | Usage |
|---|---|
| `application/json` | JSON request body |
| `application/x-www-form-urlencoded` | Form data (URL-encoded) |
| `multipart/form-data` | File uploads |
| `text/plain` | Raw text |
| `text/html` | HTML response |

**Using Headers object for case-insensitive access:**

```javascript
const headers = new Headers();
headers.set("Content-Type", "application/json");
headers.append("Accept", "application/json");
headers.append("Accept", "text/plain");   // multiple values

fetch(url, { headers });
```

### POST & Other Methods

**POST with JSON body:**

```javascript
async function createUser(userData) {
    const response = await fetch("https://api.example.com/users", {
        method: "POST",
        headers: {
            "Content-Type": "application/json"
        },
        body: JSON.stringify(userData)
    });

    if (!response.ok) {
        throw new Error(`HTTP ${response.status}: ${response.statusText}`);
    }

    return response.json();
}
```

**PUT (update):**

```javascript
fetch("https://api.example.com/users/42", {
    method: "PUT",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify({ name: "Updated Name" })
});
```

**PATCH (partial update):**

```javascript
fetch("https://api.example.com/users/42", {
    method: "PATCH",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify({ email: "new@example.com" })
});
```

**DELETE:**

```javascript
fetch("https://api.example.com/users/42", {
    method: "DELETE"
    // No body for DELETE
});
```

### Request Objects

Create a reusable `Request` object:

```javascript
const request = new Request("https://api.example.com/data", {
    method: "GET",
    headers: {
        "Accept": "application/json"
    },
    cache: "no-cache"
});

// Use it directly
const response1 = await fetch(request);

// Clone for reuse (Request body can only be read once)
const response2 = await fetch(request.clone());
```

### Error Handling

Fetch only rejects on **network errors** — not on HTTP error statuses like 404 or 500:

```javascript
async function safeFetch(url, options = {}) {
    try {
        const response = await fetch(url, options);

        if (!response.ok) {
            // Try to extract server error message
            const errorBody = await response.text();
            throw new Error(
                `HTTP ${response.status}: ${response.statusText}\n${errorBody}`
            );
        }

        return await response.json();

    } catch (error) {
        if (error.name === "AbortError") {
            console.log("Request was cancelled");
            return null;
        }
        console.error("Fetch failed:", error.message);
        throw error;
    }
}
```

**Network error vs. HTTP error:**

```javascript
// Network error: DNS failure, connection refused
try {
    await fetch("https://nonexistent-domain-xyz.com/api");
} catch (err) {
    // TypeError: Failed to fetch
    err.name;                               // "TypeError"
}

// HTTP error: server responds
const response = await fetch("https://api.github.com/not-found");
response.ok;                                // false
response.status;                            // 404
// No exception thrown
```

Always check `response.ok` or `response.status` explicitly.

### AbortController & Cancellation

Cancel in-flight requests with `AbortController`:

```javascript
const controller = new AbortController();
const { signal } = controller;

// Start request
const fetchPromise = fetch("https://api.example.com/slow-endpoint", { signal });

// Cancel after 3 seconds
setTimeout(() => controller.abort(), 3000);

try {
    const data = await fetchPromise;
    console.log("Completed:", data);
} catch (error) {
    if (error.name === "AbortError") {
        console.log("Request was cancelled due to timeout");
    } else {
        console.error("Request failed:", error);
    }
}
```

**Timeout wrapper:**

```javascript
async function fetchWithTimeout(url, options = {}, timeoutMs = 5000) {
    const controller = new AbortController();
    const timeoutId = setTimeout(() => controller.abort(), timeoutMs);

    try {
        const response = await fetch(url, {
            ...options,
            signal: controller.signal
        });
        return response;
    } finally {
        clearTimeout(timeoutId);
    }
}
```

**Race against timeout:**

```javascript
async function fetchOrTimeout(url, ms = 5000) {
    const controller = new AbortController();

    const result = await Promise.race([
        fetch(url, { signal: controller.signal }),
        new Promise((_, reject) =>
            setTimeout(() => {
                controller.abort();
                reject(new Error("Timeout"));
            }, ms)
        )
    ]);

    return result;
}
```

### JSON: The Server Language

JSON (JavaScript Object Notation) is the most common data format for web APIs.

**Sending JSON:**

```javascript
const data = { name: "Alice", email: "alice@example.com" };

fetch("/api/users", {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify(data)
});
```

**Receiving JSON:**

```javascript
const response = await fetch("/api/users");
const users = await response.json();           // parse once
```

**JSON parsing safety:**

```javascript
// Manual parsing with error handling
const text = await response.text();
try {
    const data = JSON.parse(text);
    // handle data
} catch {
    throw new Error(`Invalid JSON: ${text.slice(0, 100)}`);
}
```

### Uploading Files

**Single file upload with FormData:**

```javascript
async function uploadFile(fileInput) {
    const formData = new FormData();
    formData.append("avatar", fileInput.files[0]);

    const response = await fetch("/api/upload", {
        method: "POST",
        // Do NOT set Content-Type for FormData — browser sets it with boundary
        body: formData
    });

    return response.json();
}
```

**Multiple files:**

```javascript
const formData = new FormData();
for (const file of fileInput.files) {
    formData.append("photos", file);
}

await fetch("/api/photos", { method: "POST", body: formData });
```

**File with additional fields:**

```javascript
const formData = new FormData();
formData.append("title", "My Photo");
formData.append("description", "Sunset at the beach");
formData.append("photo", fileInput.files[0]);
```

### Downloading Files

```javascript
async function downloadFile(url, filename) {
    const response = await fetch(url);
    const blob = await response.blob();

    const link = document.createElement("a");
    link.href = URL.createObjectURL(blob);
    link.download = filename;
    link.click();

    // Clean up
    URL.revokeObjectURL(link.href);
}

// Usage
downloadFile("https://example.com/report.pdf", "report.pdf");
```

**Download with progress (via ReadableStream):**

```javascript
async function downloadWithProgress(url, onProgress) {
    const response = await fetch(url);
    const contentLength = response.headers.get("Content-Length");
    const total = parseInt(contentLength, 10);
    let loaded = 0;

    const reader = response.body.getReader();
    const chunks = [];

    while (true) {
        const { done, value } = await reader.read();
        if (done) break;

        chunks.push(value);
        loaded += value.length;
        onProgress(loaded, total);
    }

    // Combine chunks into a single Blob
    return new Blob(chunks);
}

downloadWithProgress("https://example.com/big-file.zip", (loaded, total) => {
    const percent = Math.round((loaded / total) * 100);
    console.log(`Downloaded: ${percent}%`);
});
```

### CORS & Cross-Origin Requests

**Same-origin policy** blocks scripts from reading resources from a different origin (protocol + domain + port).

```
https://myapp.com/api/data        ← same origin
https://api.myapp.com/data        ← different origin (subdomain)
http://myapp.com/api/data         ← different origin (different protocol)
https://myapp.com:3000/api/data   ← different origin (different port)
```

**CORS (Cross-Origin Resource Sharing)** — the server must include CORS headers to permit cross-origin requests:

```
Access-Control-Allow-Origin: https://myapp.com
Access-Control-Allow-Methods: GET, POST, PUT, DELETE
Access-Control-Allow-Headers: Content-Type, Authorization
Access-Control-Allow-Credentials: true
```

**Simple requests vs. preflighted:**

A request is **simple** if it uses GET/HEAD/POST with only simple headers. Otherwise, the browser sends a **preflight** `OPTIONS` request first:

```
Browser                          Server
  │                                │
  ├── OPTIONS /api/data          ──►
  │   Origin: https://myapp.com    │
  │◄── 204 No Content              │
  │   Access-Control-Allow-...     │
  │                                │
  ├── GET /api/data              ──►
  │   Origin: https://myapp.com    │
  │◄── 200 OK                      │
  │   Access-Control-Allow-...     │
```

**Mode options:**

```javascript
fetch(url, { mode: "cors" });          // default — requires CORS headers
fetch(url, { mode: "same-origin" });    // rejects cross-origin
fetch(url, { mode: "no-cors" });        // opaque response (can't read body)
```

### Credentials & Authentication

**Sending cookies:**

```javascript
fetch("https://api.example.com/me", {
    credentials: "include"              // send cookies cross-origin
});
```

Credentials options:

| Option | Behavior |
|--------|----------|
| `same-origin` (default) | Send cookies for same-origin only |
| `include` | Send cookies cross-origin (requires `Access-Control-Allow-Credentials: true`) |
| `omit` | Never send cookies |

**Bearer token authentication:**

```javascript
const token = localStorage.getItem("auth_token");

fetch("https://api.example.com/admin", {
    headers: {
        "Authorization": `Bearer ${token}`
    }
});
```

**Basic authentication:**

```javascript
const credentials = btoa("username:password");

fetch("https://api.example.com/protected", {
    headers: {
        "Authorization": `Basic ${credentials}`
    }
});
```

### Caching & Cache Control

The `cache` option controls how the browser HTTP cache is used:

```javascript
fetch(url, { cache: "default" });           // use cache if fresh, else network
fetch(url, { cache: "no-cache" });          // check server for freshness
fetch(url, { cache: "no-store" });          // never cache
fetch(url, { cache: "reload" });            // ignore cache, get from network
fetch(url, { cache: "force-cache" });       // use cache even if stale
fetch(url, { cache: "only-if-cached" });    // only from cache, fail if miss
```

**Server-driven caching via response headers:**

```
Cache-Control: max-age=3600
ETag: "abc123"
Last-Modified: Mon, 10 Jun 2024 12:00:00 GMT
```

The `If-None-Match` and `If-Modified-Since` request headers are handled automatically by the browser when `cache` is not `"no-store"`.

### Progress Events

Fetch does not have a built-in upload progress event. XHR does, but you can track upload progress manually:

```javascript
async function uploadWithProgress(file, onProgress) {
    return new Promise((resolve, reject) => {
        const xhr = new XMLHttpRequest();
        xhr.open("POST", "/api/upload");

        xhr.upload.addEventListener("progress", (e) => {
            if (e.lengthComputable) {
                onProgress(e.loaded, e.total);
            }
        });

        xhr.addEventListener("load", () => {
            resolve(xhr.response);
        });

        xhr.addEventListener("error", () => {
            reject(new Error("Upload failed"));
        });

        const formData = new FormData();
        formData.append("file", file);
        xhr.send(formData);
    });
}
```

## Glossary

| Term | Definition |
|------|------------|
| HTTP | Hypertext Transfer Protocol — the protocol for web communication |
| Request | A message from client to server asking for a resource or action |
| Response | The server's reply to an HTTP request |
| Status code | Three-digit code indicating the result (200 OK, 404 Not Found) |
| JSON | JavaScript Object Notation — lightweight data interchange format |
| CORS | Cross-Origin Resource Sharing — server headers that permit cross-origin requests |
| Preflight | An OPTIONS request sent before complex cross-origin requests |
| AbortController | API to cancel in-flight fetch requests |
| FormData | Browser API for constructing multipart form data bodies |
| Blob | Binary Large Object — raw binary data |
| ReadableStream | Streaming interface for reading data chunk by chunk |
| ETag | HTTP response header for cache validation |
| Origin | Protocol + host + port combination identifying a web origin |
| Opaque response | A cross-origin response that cannot be read (from `no-cors` mode) |

## Quick References

- [MDN: Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) — complete Fetch documentation
- [MDN: Using Fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch) — practical guide
- [MDN: Response](https://developer.mozilla.org/en-US/docs/Web/API/Response) — Response object reference
- [MDN: CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) — CORS explained
- [HTTP Status Codes](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status) — complete status code reference

## Next Steps

- [Promises & Async/Await](promises-async-await.md) — the async foundation Fetch is built on
- [Web Storage & Cookies](web-storage.md) — storing auth tokens and session data
- [Debugging with DevTools](debugging-devtools.md) — inspecting network requests in the browser
