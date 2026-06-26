# Headers, Body, Content Types, and JSON

## Description

HTTP headers control caching, authentication, content negotiation, and more. The request and response body carries data between client and server. Understanding MIME types, JSON, form data, cookies, and CORS is essential for building and debugging web APIs.

## Prerequisites

- [HTTP Methods & Status Codes](../http-methods-and-status.md) -- familiarity with GET, POST, PUT, DELETE and common status codes (200, 201, 400, 404, 500)

## Table of Contents

- [HTTP Headers](#http-headers)
- [Content Types and MIME Types](#content-types-and-mime-types)
- [JSON](#json)
- [Request Body Formats](#request-body-formats)
- [Response Body](#response-body)
- [Cookies](#cookies)
- [CORS](#cors-cross-origin-resource-sharing)
- [Study Cases](#study-cases)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### HTTP Headers

Headers are key-value pairs in HTTP messages. Every header follows `Name: Value`. Names are case-insensitive but conventionally use initial capitals.

#### General Headers

Apply to both requests and responses.

**Date** -- when the message was generated. `Date: Wed, 01 Jun 2024 12:00:00 GMT`

**Connection** -- whether the connection stays open. `Connection: keep-alive`. Obsolete in HTTP/2.

**Cache-Control** -- caching directives for clients and intermediaries. `Cache-Control: no-cache, no-store, must-revalidate`. Directives: `public`, `private`, `no-cache`, `no-store`, `max-age=<seconds>`.

#### Request Headers

Originate from the client.

**Host** -- mandatory in HTTP/1.1, identifies the virtual host. `Host: api.example.com`

**User-Agent** -- identifies client software. Never trust it for security. `User-Agent: Mozilla/5.0`

**Accept** -- media types the client can process. `Accept: application/json, text/html;q=0.9, */*;q=0.8`. `q` is a quality weight.

**Accept-Encoding** -- supported compression. `Accept-Encoding: gzip, deflate, br`. Server sets `Content-Encoding: gzip` on response.

**Authorization** -- credentials. `Authorization: Bearer eyJhbGciOiJIUzI1NiJ9`. Format: `Scheme Credential` (Basic, Bearer, Digest).

**Content-Type** -- body format in requests. `Content-Type: application/json`

**Content-Length** -- body size in bytes. `Content-Length: 42`

**Referer** -- previous page URL. `Referer: https://example.com/products`

**Cookie** -- stored cookies sent to server. `Cookie: session_id=abc123; theme=dark`

**Origin** -- request source for CORS. `Origin: https://my-frontend.com`. Carries only scheme, host, and port.

#### Response Headers

Sent by the server.

**Content-Type** -- how to interpret the body. `Content-Type: application/json; charset=utf-8`

**Content-Length** -- body size in bytes. `Content-Length: 128`

**Set-Cookie** -- instructs browser to store a cookie. `Set-Cookie: session_id=abc123; Path=/; HttpOnly; Secure; SameSite=Lax`

**Cache-Control** -- caching directives. `Cache-Control: public, max-age=3600`

**Access-Control-Allow-Origin** -- grants cross-origin access. `Access-Control-Allow-Origin: https://my-frontend.com`. `*` allows any origin but disables credentials.

**Location** -- redirect URL. `Location: https://api.example.com/users/42`. Used with 3xx status codes.

#### Custom Headers

Historically prefixed with `X-` (e.g., `X-Request-ID`). RFC 6648 deprecated this practice. Use descriptive names without prefix.

```http
Request-Id: 550e8400-e29b-41d4-a716-446655440000
```

Useful for tracing, rate-limit metadata, and debugging.

#### How Headers Affect Behavior

**Caching.** `Cache-Control`, `ETag`, `Last-Modified`, `Expires` control browser and CDN caching.

**Content negotiation.** `Accept*` headers let the client specify preferences. The server picks the best match and responds with `Content-Type`, `Content-Language`, `Content-Encoding`.

**Authentication.** `Authorization` carries credentials. Server validates and may respond with `WWW-Authenticate`.

**Session management.** `Set-Cookie` and `Cookie` maintain session state.

### Content Types and MIME Types

MIME (Multipurpose Internet Mail Extensions) types identify document formats. Format: `type/subtype`.

#### Common MIME Types

| MIME Type | Description |
|-----------|-------------|
| `text/html` | HTML documents |
| `application/json` | JSON data |
| `application/xml` | XML data |
| `multipart/form-data` | Forms with file uploads |
| `application/octet-stream` | Arbitrary binary data |
| `text/plain` | Plain text |
| `text/css` | CSS |
| `application/javascript` | JavaScript |
| `image/png` | PNG images |
| `image/jpeg` | JPEG images |

#### Content-Type Header

Tells the receiver the body format.

```http
Content-Type: application/json; charset=utf-8
```

The charset parameter is recommended for text types.

#### Accept Header

Tells the server what formats the client can handle.

```http
Accept: application/json, text/plain;q=0.5
```

Server picks the best match. Returns 406 Not Acceptable if no match.

#### Content Negotiation

Client and server agree on a representation.

```
Client sends:   Accept: application/json
Server checks:  available formats: JSON, XML, HTML
Server chooses: JSON
Server responds: Content-Type: application/json
```

```python
from flask import Flask, request, jsonify

app = Flask(__name__)

@app.route('/api/data')
def get_data():
    data = {'message': 'hello'}
    best = request.accept_mimetypes.best_match(
        ['application/json', 'application/xml']
    )
    if best == 'application/xml':
        xml = '<root><message>hello</message></root>'
        return (xml, 200, {'Content-Type': 'application/xml'})
    return jsonify(data)
```

### JSON

JSON (JavaScript Object Notation) is a lightweight data-interchange format. It is language-independent and the dominant format for REST APIs.

#### Syntax

Six value types:

**Objects** -- key-value pairs in curly braces.

```json
{
  "name": "Alice",
  "age": 30,
  "active": true
}
```

Keys must be double-quoted.

**Arrays** -- ordered lists in square brackets.

```json
[1, 2, 3, 4, 5]
```

```json
[
  {"id": 1, "name": "Alice"},
  {"id": 2, "name": "Bob"}
]
```

**Strings** -- double-quoted Unicode with backslash escaping.

```json
"Hello, world!"
```

**Numbers** -- integer or floating-point.

```json
42
3.14
-7
1.5e10
```

**Booleans** -- `true` or `false`.

**Null** -- absence of a value.

```json
{"middle_name": null}
```

#### JSON vs XML vs YAML

| Feature | JSON | XML | YAML |
|---------|------|-----|------|
| Syntax | Braces | Tags | Indentation |
| Comments | No | Yes | Yes |
| Types | String, number, bool, null, array, object | Everything is text | Similar to JSON |
| Parse speed | Fast | Slow | Moderate |
| Use case | APIs, config | SOAP, SVG, docs | Config files |

JSON dominates APIs due to simplicity and native JavaScript support.

#### Serialization (Encoding)

Object to JSON string.

```python
import json
data = {'user': 'alice', 'roles': ['admin'], 'prefs': {'theme': 'dark'}}
json_string = json.dumps(data, indent=2)
```

```javascript
const data = {user: 'alice', roles: ['admin'], prefs: {theme: 'dark'}};
const jsonString = JSON.stringify(data, null, 2);
```

Omit indentation in production to minimize payload size.

#### Deserialization (Decoding)

JSON string to object.

```python
import json
data = json.loads('{"user": "alice", "count": 42}')
print(data['user'])
```

```javascript
const data = JSON.parse('{"user": "alice", "count": 42}');
console.log(data.user);
```

APIs often use `snake_case` keys regardless of client language.

#### Common Pitfalls

**Trailing commas** are invalid.

```json
{"name": "Alice",}  // Invalid
```

**Comments** are not allowed. Use preprocessors or `"_comment"` keys.

**No `undefined`.** JavaScript's `undefined` is omitted during serialization.

**Duplicate keys** cause undefined behavior across parsers.

**Large integers lose precision** beyond 2^53. Send as strings.

```json
{"id": "9007199254740993"}
```

#### JSON in APIs

Request body:

```bash
curl -X POST https://api.example.com/users \
  -H "Content-Type: application/json" \
  -d '{"name": "Alice", "email": "alice@example.com"}'
```

```python
import requests
r = requests.post('https://api.example.com/users',
                  json={'name': 'Alice', 'email': 'alice@example.com'})
print(r.json())
```

```javascript
const res = await fetch('https://api.example.com/users', {
  method: 'POST',
  headers: {'Content-Type': 'application/json'},
  body: JSON.stringify({name: 'Alice', email: 'alice@example.com'})
});
const data = await res.json();
```

Response body:

```python
from flask import Flask, jsonify
app = Flask(__name__)

@app.route('/api/users/<int:uid>')
def get_user(uid):
    return jsonify({'id': uid, 'name': 'Alice'})
```

```javascript
app.get('/api/users/:id', (req, res) => {
  res.json({id: req.params.id, name: 'Alice'});
});
```

`jsonify` and `res.json` set `Content-Type: application/json` automatically.

### Request Body Formats

#### JSON (application/json)

Most common for REST APIs. Supports nested structures.

```bash
curl -X POST https://api.example.com/orders \
  -H "Content-Type: application/json" \
  -d '{"product_id": 101, "quantity": 2}'
```

#### URL-Encoded Form Data (application/x-www-form-urlencoded)

Key-value pairs as query-string text.

```bash
curl -X POST https://api.example.com/login \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -d "username=alice&password=secret123"
```

```python
requests.post('https://api.example.com/login',
              data={'username': 'alice', 'password': 'secret123'})
```

HTML forms default to this format.

#### Multipart Form Data (multipart/form-data)

For file uploads and mixed fields. Each part has a boundary separator.

```bash
curl -X POST https://api.example.com/upload \
  -F "file=@photo.jpg" \
  -F "description=Vacation photo"
```

```python
requests.post('https://api.example.com/upload',
              files={'file': open('photo.jpg', 'rb')},
              data={'description': 'Vacation photo'})
```

```javascript
const fd = new FormData();
fd.append('file', fileInput.files[0]);
fd.append('description', 'Vacation photo');
await fetch('https://api.example.com/upload', {method: 'POST', body: fd});
```

Wire format:

```
--boundary123
Content-Disposition: form-data; name="file"; filename="photo.jpg"
Content-Type: image/jpeg

[binary]
--boundary123
Content-Disposition: form-data; name="description"

Vacation photo
--boundary123--
```

#### Plain Text and Binary

Plain text for webhooks; binary for arbitrary files.

```bash
curl -X POST https://api.example.com/webhook \
  -H "Content-Type: text/plain" \
  -d "raw payload"
```

```bash
curl -X PUT https://api.example.com/files/doc.pdf \
  -H "Content-Type: application/pdf" \
  --data-binary @doc.pdf
```

### Response Body

The response body carries the server's data. `Content-Type` indicates the format.

#### JSON Responses

```json
{
  "id": 42,
  "name": "Alice",
  "email": "alice@example.com",
  "created_at": "2024-06-01T12:00:00Z"
}
```

#### Pagination Metadata

List endpoints should include pagination.

```json
{
  "data": [
    {"id": 1, "name": "Alice"},
    {"id": 2, "name": "Bob"}
  ],
  "pagination": {
    "page": 1,
    "per_page": 20,
    "total": 47,
    "total_pages": 3,
    "next_url": "https://api.example.com/users?page=2",
    "prev_url": null
  }
}
```

**Page-based pagination.** `?page=2&per_page=20`. Simple but unstable if data changes.

```python
@app.route('/api/users')
def list_users():
    page = request.args.get('page', 1, type=int)
    per_page = request.args.get('per_page', 20, type=int)
    items, total = get_page(page, per_page)
    total_pages = (total + per_page - 1) // per_page
    return jsonify({
        'data': items,
        'pagination': {
            'page': page,
            'per_page': per_page,
            'total': total,
            'total_pages': total_pages
        }
    })
```

**Cursor-based pagination.** `?cursor=eyJpZCI6NDJ9`. Stable across data changes.

```json
{
  "data": [...],
  "pagination": {
    "next_cursor": "eyJpZCI6NDJ9",
    "has_more": true
  }
}
```

#### Error Response Format

Consistent error structure simplifies client error handling.

```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Email is required",
    "details": [
      {"field": "email", "issue": "must not be blank"}
    ]
  }
}
```

```python
@app.errorhandler(400)
def bad_request(error):
    return jsonify({
        'error': {
            'code': 'BAD_REQUEST',
            'message': str(error),
            'details': []
        }
    }), 400
```

Best practices: machine-readable `code`, human-readable `message`, `details` for field errors, appropriate status codes, never leak stack traces.

### Cookies

Cookies are small data stored by the browser and sent with requests. Used for sessions, personalization, and tracking.

#### Set-Cookie and Cookie Headers

Server sets a cookie:

```http
Set-Cookie: session_id=abc123
```

Browser sends it back:

```http
Cookie: session_id=abc123
```

#### Attributes

**Domain** limits which hosts receive the cookie.

```
Set-Cookie: id=abc123; Domain=.example.com
```

Includes subdomains. Defaults to origin only.

**Path** restricts to URL prefixes.

```
Set-Cookie: id=abc123; Path=/app
```

**Expires / Max-Age** sets lifetime. Without it, cookie is session-only.

```
Set-Cookie: id=abc123; Max-Age=86400
```

**HttpOnly** prevents JavaScript access via `document.cookie`. Mitigates XSS.

```
Set-Cookie: id=abc123; HttpOnly
```

**Secure** restricts to HTTPS.

```
Set-Cookie: id=abc123; Secure
```

**SameSite** controls cross-site behavior.

```
Set-Cookie: id=abc123; SameSite=Lax
```

Values: `Strict` (no cross-site), `Lax` (top-level GET only, default in modern browsers), `None` (all cross-site, requires `Secure`).

### CORS (Cross-Origin Resource Sharing)

CORS is a browser security mechanism controlling cross-origin resource access.

#### Same-Origin Policy

A page can only request resources from the same origin (protocol + host + port).

```
https://example.com
http://example.com          -- different protocol
https://api.example.com     -- different host
https://example.com:8443    -- different port
```

CORS lets servers opt into cross-origin requests.

#### Preflight Requests

Browser sends OPTIONS before certain cross-origin requests. Triggered by:
- Non-standard methods (not GET, HEAD, POST)
- Custom headers (Authorization, etc.)
- Non-standard Content-Type

```http
OPTIONS /data HTTP/1.1
Origin: https://my-frontend.com
Access-Control-Request-Method: POST
Access-Control-Request-Headers: Authorization
```

```http
HTTP/1.1 204 No Content
Access-Control-Allow-Origin: https://my-frontend.com
Access-Control-Allow-Methods: GET, POST, PUT, DELETE
Access-Control-Allow-Headers: Authorization
Access-Control-Max-Age: 86400
```

#### CORS Headers

**Access-Control-Allow-Origin** -- allowed origin (or `*`).

**Access-Control-Allow-Methods** -- allowed HTTP methods.

**Access-Control-Allow-Headers** -- allowed request headers.

**Access-Control-Allow-Credentials** -- when `true`, cookies and auth headers work. Prohibits `*` for origin.

**Access-Control-Expose-Headers** -- custom headers exposed to JavaScript.

#### Enabling CORS

```python
from flask_cors import CORS
CORS(app, origins=['https://my-frontend.com'],
     supports_credentials=True)
```

Manual:

```python
@app.after_request
def add_cors(response):
    response.headers['Access-Control-Allow-Origin'] = 'https://my-frontend.com'
    response.headers['Access-Control-Allow-Headers'] = 'Authorization'
    response.headers['Access-Control-Allow-Methods'] = 'GET, POST'
    response.headers['Access-Control-Allow-Credentials'] = 'true'
    return response
```

```javascript
app.use(cors({
  origin: 'https://my-frontend.com',
  credentials: true
}));
```

#### Common CORS Scenarios

Frontend at `localhost:3000`, backend at `localhost:8000`. Different ports = different origins. Browser error:

```
Access to fetch at 'http://localhost:8000/data' from origin
'http://localhost:3000' has been blocked by CORS policy.
```

Only the server can fix this by including CORS headers.

## Study Cases

### Case 1: CORS Error -- Frontend Calls Different API Domain

React frontend at `https://app.myapp.com` calls `https://api.myapp.com/users` with an Authorization header.

```javascript
fetch('https://api.myapp.com/users', {
  headers: {'Authorization': 'Bearer token123'}
}).catch(e => console.error(e));
```

Preflight triggered by `Authorization` header. Server must respond with CORS headers.

**Solution.**

```javascript
app.use(cors({
  origin: 'https://app.myapp.com',
  methods: ['GET', 'POST'],
  allowedHeaders: ['Authorization']
}));
```

### Case 2: Content-Type Mismatch

Client POSTs JSON without setting Content-Type.

```bash
curl -X POST https://api.example.com/users \
  -d '{"name": "Alice"}'  # defaults to form-urlencoded
```

Server does not parse the body as JSON.

**Solution.** Set Content-Type explicitly.

```bash
curl -X POST https://api.example.com/users \
  -H "Content-Type: application/json" \
  -d '{"name": "Alice"}'
```

Server should validate and return 415 if Content-Type is wrong.

```python
@app.route('/api/users', methods=['POST'])
def create_user():
    if request.content_type != 'application/json':
        return jsonify({'error': 'Must be application/json'}), 415
    return jsonify({'created': request.get_json()['name']}), 201
```

### Case 3: Cookie Not Sent Due to SameSite

Frontend at `https://frontend.com` calls `https://api.backend.com`. Server sets a cookie but it is not sent cross-origin.

**Solution.** SameSite=None, Secure on the cookie; credentials: include on fetch; CORS with credentials.

```python
response.set_cookie('session', 'abc123',
                     httponly=True, secure=True, samesite='None')
```

```javascript
fetch('https://api.backend.com/data', {
  credentials: 'include'
});
```

All three pieces required: server cookie attributes, CORS credentials config, and client credentials opt-in.

## Glossary

| Term | Definition |
|------|------------|
| Header | Key-value pair in HTTP messages carrying metadata |
| MIME type | Standard identifier for document formats (type/subtype) |
| Content-Type | Header indicating the media type of the message body |
| Content-Length | Header indicating body size in bytes |
| JSON | JavaScript Object Notation, lightweight data interchange format |
| Serialize | Convert an object to a JSON string |
| Deserialize | Convert a JSON string to an object |
| CORS | Cross-Origin Resource Sharing, browser mechanism for cross-origin access control |
| Preflight | OPTIONS request the browser sends before certain cross-origin requests |
| Cookie | Browser-stored data sent with requests, used for sessions |
| Session | Server-side state tracked via a cookie |
| Content negotiation | Process where client and server agree on representation format |
| Charset | Character encoding specification (e.g., UTF-8) |
| UTF-8 | Variable-width Unicode encoding, the web standard |
| Multipart | Message with multiple parts separated by boundaries, used for uploads |
| Form data | HTML form submission data (URL-encoded or multipart) |
| URL encoding | Percent-encoding special characters in query strings and form data |

## Quick References

- [MDN: HTTP Headers](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers) -- complete header reference
- [MDN: CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) -- detailed CORS guide
- [RFC 8259: JSON](https://datatracker.ietf.org/doc/html/rfc8259) -- JSON specification
- [MDN: Set-Cookie](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Set-Cookie) -- cookie attribute reference
- [JSON.org](https://www.json.org/) -- JSON syntax overview

## Next Steps

- [REST API Design](../rest-api-design.md) -- apply headers, status codes, and JSON to design clean RESTful APIs
