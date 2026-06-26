# What is HTTP

## Description

HTTP is the protocol that powers the web. Every API call, every page load, every image fetch happens over HTTP. Understanding it is essential for any developer.

## Prerequisites

- [Programming Fundamentals](../../programming/fundamentals/index.md) — basic programming concepts

## Table of Contents

- [What is HTTP](#what-is-http)
- [The Client-Server Model](#the-client-server-model)
- [A Brief History of HTTP](#a-brief-history-of-http)
- [The Request-Response Cycle](#the-request-response-cycle)
- [URL Structure](#url-structure)
- [DNS Resolution](#dns-resolution)
- [TCP Connection: The Three-Way Handshake](#tcp-connection-the-three-way-handshake)
- [TLS / HTTPS](#tls--https)
- [Statelessness](#statelessness)
- [Idempotency and Safety](#idempotency-and-safety)
- [HTTP Versions Comparison](#http-versions-comparison)

## Content / Material

### What is HTTP

HTTP stands for **Hypertext Transfer Protocol**. It is an **application layer protocol** that defines how messages are formatted and transmitted between clients and servers on the web. It runs on top of TCP/IP (or QUIC in HTTP/3) and follows a simple request-response pattern.

The protocol is text-based in its earlier versions (HTTP/1.x) and binary-framed in HTTP/2 and HTTP/3. Despite these differences, the core concepts remain the same: a client sends a request, a server returns a response.

HTTP is the foundation of data communication on the World Wide Web. Every time you open a browser, load a mobile app, or call a REST API, you are using HTTP.

At its simplest, an HTTP exchange looks like this:

```
Client: "Give me the resource at /index.html"
Server: "Here is the resource (status 200), and here is the content."
```

The protocol is **stateless**, meaning each request is independent with no memory of previous ones. Mechanisms like cookies and session tokens work on top of HTTP to simulate state, but the protocol itself has no built-in memory.

### The Client-Server Model

HTTP operates on a **client-server architecture**. The client initiates a request, and the server listens for requests and responds.

- **Client:** Any device or program that sends an HTTP request. Common clients: web browsers, curl, Python requests, JS fetch, mobile apps, Postman.
- **Server:** A program that listens for incoming HTTP requests and returns responses. Common servers: Nginx, Apache, Express.js, Django, Flask, FastAPI.

A single server can handle thousands of concurrent clients. The client and server communicate over a network, usually the internet.

```
+--------+   HTTP Request    +--------+
|        | -----------------> |        |
| Client |                    | Server |
|        | <----------------- |        |
+--------+   HTTP Response   +--------+
```

The client-server model decouples the frontend (user interface) from the backend (data storage, business logic). This separation allows both sides to evolve independently.

Example of a client sending a request with curl:

```bash
curl -X GET https://api.example.com/users/1
```

Same request with Python requests:

```python
import requests

response = requests.get("https://api.example.com/users/1")
print(response.status_code)
print(response.json())
```

Same request with JavaScript fetch:

```javascript
fetch("https://api.example.com/users/1")
  .then(response => response.json())
  .then(data => console.log(data));
```

In all three cases, the same HTTP request is constructed and sent to the server. The server processes it and returns a response.

### A Brief History of HTTP

**HTTP/0.9 (1991):** Single-line requests with `GET /index.html`, raw HTML responses. No headers, status codes, or version info.

**HTTP/1.0 (1996):** Introduced headers, status codes, content types, and methods beyond GET (HEAD, POST). Each request opened a new TCP connection — inefficient for pages with many resources.

```
GET /index.html HTTP/1.0
Host: example.com
Accept: text/html

HTTP/1.0 200 OK
Content-Type: text/html
Content-Length: 1234
```

**HTTP/1.1 (1997):** The most widely deployed version for decades. Key improvements: persistent connections (reuse TCP connection), chunked transfer encoding (stream without known length), required Host header (virtual hosting), additional methods (PUT, DELETE, OPTIONS, TRACE), and pipelining. Suffers from head-of-line (HOL) blocking — one slow request blocks the queue.

```http
GET /index.html HTTP/1.1
Host: example.com
Connection: keep-alive

HTTP/1.1 200 OK
Content-Type: text/html
Transfer-Encoding: chunked
```

**HTTP/2 (2015):** Based on Google's SPDY. Binary framing replaces text, multiplexing enables multiple streams on one connection (eliminating HTTP-level HOL blocking), HPACK header compression reduces overhead, and server push allows proactive resource delivery. Semantics remain backward-compatible with HTTP/1.1.

```
HTTP/1.1:  [REQ1] -> [RES1] -> [REQ2] -> [RES2]
HTTP/2:    [REQ1, REQ2] -> [RES2, RES1] (any order)
```

**HTTP/3 (2022):** Runs over QUIC (UDP-based transport) instead of TCP. Key benefits: 0-RTT connection establishment, no transport-layer HOL blocking (per-stream loss handling), connection migration across networks, and mandatory TLS 1.3 encryption. Particularly beneficial for mobile and lossy networks.

### The Request-Response Cycle

When you type a URL into a browser and press Enter:

1. **URL parsing** -- the browser parses the URL into scheme, host, port, path, query, and fragment.
2. **DNS lookup** -- the browser resolves the hostname to an IP address via DNS.
3. **TCP handshake** -- the browser and server perform a three-way handshake (SYN, SYN-ACK, ACK).
4. **TLS handshake (if HTTPS)** -- the browser and server negotiate encryption and exchange certificates.
5. **HTTP request** -- the browser sends an HTTP request (typically GET for the page).
6. **Server processing** -- the server processes the request and prepares a response.
7. **HTTP response** -- the server sends back a status code, headers, and body.
8. **Browser rendering** -- the browser parses HTML, discovers additional resources (CSS, JS, images), and fetches them.
9. **Connection reuse or close** -- kept alive for subsequent requests or closed.

Tracing `https://example.com` with curl verbose output:

```bash
curl -v https://example.com
```

The output reveals DNS resolution, TCP connection, TLS negotiation, HTTP request headers, response headers, and the body. Python and JavaScript equivalents show the same cycle at a higher level:

```python
import requests
response = requests.get("https://example.com")
print(f"Status: {response.status_code}, Body: {len(response.text)} bytes")
```

```javascript
fetch("https://example.com")
  .then(r => r.text())
  .then(body => console.log("Body length:", body.length));
```

### URL Structure

A URL (Uniform Resource Locator) is the address of a resource on the web. It has several components:

```
  https://www.example.com:8080/path/to/page?name=value&key=val#section
  \____/ \________________/\___/\_____________/\________________/\_____/
   |            |           |         |               |             |
 scheme        host       port      path           query        fragment
```

1. **Scheme:** The protocol to use. Common values: `http`, `https`, `ftp`, `file`.
2. **Host:** The domain name or IP address of the server. Example: `www.example.com`, `192.168.1.1`.
3. **Port:** The TCP port number. Defaults: 80 for HTTP, 443 for HTTPS. Non-default ports are specified explicitly (e.g., `:8080`).
4. **Path:** The specific resource on the server. Example: `/users/123`, `/index.html`.
5. **Query:** Key-value pairs starting with `?`, separated by `&`. Used for sending data to the server, typically for search parameters, filters, or pagination. Example: `?page=2&limit=10`.
6. **Fragment:** An anchor within the resource, starting with `#`. The fragment is never sent to the server — it is used by the client to scroll to a specific section of the page.

Full URL breakdown in code:

```python
from urllib.parse import urlparse

url = "https://www.example.com:8080/path/to/page?name=value&key=val#section"
parsed = urlparse(url)

print(f"Scheme:   {parsed.scheme}")    # https
print(f"Hostname: {parsed.hostname}")  # www.example.com
print(f"Port:     {parsed.port}")      # 8080
print(f"Path:     {parsed.path}")      # /path/to/page
print(f"Query:    {parsed.query}")     # name=value&key=val
print(f"Fragment: {parsed.fragment}")  # section
```

```javascript
const url = new URL("https://www.example.com:8080/path/to/page?name=value&key=val#section");

console.log("Scheme:", url.protocol);     // https:
console.log("Hostname:", url.hostname);   // www.example.com
console.log("Port:", url.port);           // 8080
console.log("Path:", url.pathname);       // /path/to/page
console.log("Query:", url.search);        // ?name=value&key=val
console.log("Fragment:", url.hash);       // #section
```

Note: URI (Uniform Resource Identifier) is a broader term. Every URL is a URI, but not every URI is a URL. A URL tells you both the name and how to locate it. A URI can be just a name (URN) without location information.

### DNS Resolution

Before a client can send an HTTP request, it needs to convert the human-readable domain name (e.g., `example.com`) into a machine-readable IP address (e.g., `93.184.216.34`). This is the job of the **Domain Name System (DNS)**.

The resolution process:

1. **Browser cache check:** The browser checks its local DNS cache.
2. **OS cache check:** If not found, the operating system checks its cache.
3. **Router cache check:** If not found, the router's DNS cache is checked.
4. **ISP DNS resolver:** If not found, the query is sent to the ISP's recursive DNS resolver (or a public resolver like 8.8.8.8 or 1.1.1.1).
5. **Root name server:** The resolver queries a root name server to find the TLD (top-level domain) name server (e.g., `.com`).
6. **TLD name server:** The resolver asks the TLD name server for the authoritative name server for the domain.
7. **Authoritative name server:** The resolver queries the authoritative name server (e.g., `ns1.example.com`) for the actual IP address.
8. **Response:** The IP address is returned to the client, which caches it for future use.

```
Client -> Browser Cache -> OS Cache -> Router Cache -> ISP Resolver -> Root NS -> TLD NS -> Authoritative NS
```

You can see DNS resolution in action with the `dig` command:

```bash
dig example.com

# Output (simplified):
# example.com.        3600    IN    A    93.184.216.34
```

Or with Python:

```python
import socket

ip = socket.gethostbyname("example.com")
print(ip)  # 93.184.216.34
```

DNS resolution typically takes 10-50 milliseconds, but complex setups with multiple redirects or slow resolvers can take longer. CDNs (Content Delivery Networks) use DNS to route users to the nearest server by returning different IPs based on geographic location.

### TCP Connection: The Three-Way Handshake

Before HTTP data can flow, the client and server must establish a **TCP connection**. TCP (Transmission Control Protocol) provides reliable, ordered delivery of bytes between two hosts.

The connection is established through a **three-way handshake**:

1. **SYN:** The client sends a TCP packet with the SYN (synchronize) flag set, along with an initial sequence number.
2. **SYN-ACK:** The server responds with a packet that has both SYN and ACK flags set, acknowledging the client's sequence number and sending its own.
3. **ACK:** The client sends a packet with the ACK flag set, acknowledging the server's sequence number.

```
Client                          Server
  |                               |
  |---------- SYN (seq=100) ----->|
  |                               |
  |<------- SYN-ACK (seq=200, ack=101) ----|
  |                               |
  |------- ACK (seq=101, ack=201) -------->|
  |                               |
  |<========= Data Transfer ======>|
```

This handshake adds one round trip time (RTT) to every new connection. This is why persistent connections (HTTP/1.1 keep-alive) and multiplexing (HTTP/2) are important — they avoid repeating the handshake for every request.

You can observe the handshake using tcpdump or Wireshark:

```bash
# Capture TCP handshake packets (simplified)
sudo tcpdump -i any 'host example.com and tcp[tcpflags] & (tcp-syn) != 0'
```

In Python, the socket-level view (not something you'd typically do, but illustrative):

```python
import socket

sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
sock.settimeout(5)
result = sock.connect_ex(("example.com", 80))  # This triggers the TCP handshake
sock.close()
```

The total time to establish a new HTTPS connection includes:

- DNS resolution: ~20-50ms
- TCP handshake: 1 RTT (~20-100ms depending on distance)
- TLS handshake: 1-2 RTT (~20-200ms)

For a user far from the server (e.g., 200ms RTT), that is 520-850ms before any HTTP data is exchanged. This is why connection reuse and multiplexing are critical performance optimizations.

### TLS / HTTPS

**HTTPS** (HTTP Secure) is HTTP over TLS (Transport Layer Security). TLS provides encryption, authentication, and integrity for the data exchanged between client and server.

Without HTTPS, everything is sent in plain text:

```bash
# Without encryption (HTTP) - anyone on the network can read this
curl -v http://example.com
```

With HTTPS, the data is encrypted:

```bash
# With encryption (HTTPS)
curl -v https://example.com
```

#### The TLS Handshake

The TLS handshake (simplified for TLS 1.3):

1. **Client Hello:** The client sends its supported cipher suites, TLS version, and a random number.
2. **Server Hello:** The server picks a cipher suite, sends its certificate, and a random number.
3. **Certificate verification:** The client checks the server's certificate against trusted Certificate Authorities (CAs).
4. **Key exchange:** Using the server's public key (from the certificate), the client generates a shared secret. Both sides derive the same symmetric encryption key.
5. **Secure connection established:** All subsequent data is encrypted with the symmetric key.

```
Client (TLS 1.3)               Server
  |                               |
  |--- ClientHello + key_share -->|
  |<-- ServerHello + cert + finish ---|
  |--- Finished + HTTP data ------>|
  |<--- Encrypted HTTP response ---|
```

In TLS 1.3, the handshake completes in 1 RTT (down from 2 RTT in TLS 1.2). Combined with TCP's 1 RTT, a new HTTPS connection takes 2 RTT before data can flow.

#### Why HTTPS Matters

- **Confidentiality:** Data is encrypted. An attacker on the same Wi-Fi network cannot read the contents of your requests.
- **Integrity:** Data cannot be modified in transit. If someone alters a response, the client will detect it.
- **Authentication:** The server's certificate proves its identity. You can be sure you are talking to `example.com` and not a phishing site.

Modern browsers mark HTTP sites as "Not Secure" and many features (geolocation, service workers, web crypto) require HTTPS.

Checking a certificate with curl:

```bash
curl -vI https://example.com 2>&1 | grep -A 5 "Server certificate"
```

Checking with Python:

```python
import requests

response = requests.get("https://example.com")
print(response.status_code)  # Works fine because requests verifies certs by default

# To see certificate details:
cert_info = response.raw.connection.sock.getpeercert()
print(cert_info)
```

#### Certificate Authorities

Certificates are issued by **Certificate Authorities (CAs)** like Let's Encrypt, DigiCert, and GlobalSign. A certificate binds a domain name to a public key. The CA digitally signs the certificate, and clients trust it because they have the CA's root certificate installed.

```
Certificate chain:
  Root CA (self-signed, built into OS/browser)
    -> Intermediate CA (signed by Root CA)
      -> Server certificate (signed by Intermediate CA)
```

Let's Encrypt provides free automated certificates via the ACME protocol, making HTTPS accessible to everyone.

### Statelessness

HTTP is **stateless**: each request-response pair is independent. The server does not retain any information about previous requests from the same client.

This design choice simplifies server implementation:

- Any server can handle any request without needing shared state.
- Horizontal scaling is straightforward: add more servers behind a load balancer, and any server can handle any request.
- If a server crashes, another server can pick up the next request without data loss.

However, many applications need state. E-commerce sites need to remember what is in your cart. Authentication needs to persist across pages. These needs are addressed by mechanisms built on top of HTTP:

- **Cookies:** Small pieces of data set by the server and stored by the client. Sent with every subsequent request to the same domain.
- **Session tokens:** A unique identifier stored in a cookie or header that maps to server-side session data.
- **JWT (JSON Web Tokens):** Self-contained tokens that encode user information, sent in the `Authorization` header.

Example of cookies in HTTP:

```
Server response:
HTTP/1.1 200 OK
Set-Cookie: session_id=abc123; HttpOnly; Secure; SameSite=Strict

Client's subsequent request:
GET /profile HTTP/1.1
Cookie: session_id=abc123
```

The protocol itself remains stateless; the state is carried in the data exchanged by each message.

Statelessness in API design:

```python
# Stateless API call - each request is self-contained
import requests

# No session needed - send credentials with every request
response = requests.get(
    "https://api.example.com/users/me",
    headers={"Authorization": "Bearer eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiIxMjM0NTY3ODkwIn0"}
)
```

```javascript
// Stateless fetch - token goes with every request
fetch("https://api.example.com/users/me", {
  headers: { Authorization: "Bearer eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiIxMjM0NTY3ODkwIn0" }
});
```

### Idempotency and Safety

Two important concepts in HTTP that directly affect API design.

#### Safe Methods

A method is **safe** if it does not modify any resource on the server. Safe methods are read-only.

- **GET:** Retrieve a resource. Safe and idempotent.
- **HEAD:** Retrieve headers only. Safe and idempotent.
- **OPTIONS:** Discover allowed methods. Safe and idempotent.
- **TRACE:** Echo back the request (diagnostic). Safe and idempotent.

Safe methods should have no side effects. A GET request should not create, update, or delete any data.

```bash
# Safe - no side effects
curl -X GET https://api.example.com/users/1

# Unsafe - changes server state
curl -X POST https://api.example.com/users -d '{"name": "Alice"}'
```

```python
# Safe
requests.get("https://api.example.com/users/1")

# Unsafe
requests.post("https://api.example.com/users", json={"name": "Alice"})
```

#### Idempotent Methods

A method is **idempotent** if making the same request multiple times produces the same result as making it once (aside from the first request's side effects).

- **GET, HEAD, OPTIONS, TRACE:** Safe, therefore idempotent.
- **PUT:** Idempotent. Updating a resource with the same data twice has the same effect.
- **DELETE:** Idempotent. Deleting a resource that is already gone still returns the same response (typically 200 or 404).
- **POST:** Not idempotent. Creating a resource twice creates two resources.
- **PATCH:** Not necessarily idempotent (depends on the patch format). Using JSON Patch (RFC 6902) can be non-idempotent.

```bash
# Idempotent - delete once or ten times, final state is the same
curl -X DELETE https://api.example.com/users/1
curl -X DELETE https://api.example.com/users/1  # Same result

# Non-idempotent - creates a new user each time
curl -X POST https://api.example.com/users -d '{"name": "Alice"}'
curl -X POST https://api.example.com/users -d '{"name": "Alice"}'  # Creates another user
```

Why this matters:

- **Retry safety:** If a request times out, knowing it is idempotent means you can safely retry without side effects.
- **Network reliability:** On unreliable networks, idempotent operations are safer because they can be retried automatically.
- **API design:** REST APIs should align method semantics with their idempotency guarantees.

```python
# Safe to retry on timeout because DELETE is idempotent
def delete_user(user_id, max_retries=3):
    for attempt in range(max_retries):
        response = requests.delete(f"https://api.example.com/users/{user_id}")
        if response.status_code in (200, 204, 404):
            return response
    raise Exception("Max retries exceeded")
```

```javascript
async function deleteUser(userId, maxRetries = 3) {
  for (let i = 0; i < maxRetries; i++) {
    const response = await fetch(`https://api.example.com/users/${userId}`, {
      method: "DELETE"
    });
    if ([200, 204, 404].includes(response.status)) return response;
  }
  throw new Error("Max retries exceeded");
}
```

### HTTP Versions Comparison

| Feature | HTTP/0.9 | HTTP/1.0 | HTTP/1.1 | HTTP/2 | HTTP/3 |
|---------|----------|----------|----------|--------|--------|
| Year | 1991 | 1996 | 1997 | 2015 | 2022 |
| Transport | TCP | TCP | TCP | TCP | QUIC (UDP) |
| Encoding | Text | Text | Text | Binary | Binary |
| Persistent connections | No | No | Yes (default) | Yes | Yes |
| Multiplexing | No | No | No (pipelining available but broken) | Yes (streams) | Yes (streams) |
| Header compression | No | No | No | HPACK | QPACK |
| Server push | No | No | No | Yes | Yes |
| HOL blocking at HTTP layer | Yes | Yes | Yes (queue per connection) | No (multiplexed) | No |
| HOL blocking at transport layer | Yes (TCP) | Yes (TCP) | Yes (TCP) | Yes (TCP) | No (QUIC) |
| Connection migration | No | No | No | No | Yes |
| Security | None | None | Optional (HTTPS) | Optional (HTTPS) | Mandatory (TLS 1.3) |
| Methods supported | GET | GET, HEAD, POST | All standard methods | All standard methods | All standard methods |
| RTT to first byte (new conn) | 1 (TCP) | 1 (TCP) | 1 (TCP) | 1-2 (TCP + TLS) | 0-1 (QUIC + TLS 1.3) |
| Caching support | None | Basic (Expires, Pragma) | Advanced (ETag, Cache-Control) | Advanced | Advanced |
| Virtual hosting | No | No | Yes (Host header) | Yes | Yes |
| Chunked transfer | No | No | Yes | Replaced by stream frames | Replaced by stream frames |

**Key takeaways from the comparison:**

- HTTP/0.9 and HTTP/1.0 are obsolete. No modern client or server should implement them.
- HTTP/1.1 is still widely used, especially for small-scale applications and internal APIs. Its main weakness is head-of-line blocking and lack of multiplexing.
- HTTP/2 is the modern standard for web applications. It solves multiplexing and header compression but still suffers from TCP-level HOL blocking.
- HTTP/3 solves the TCP HOL blocking problem by using QUIC over UDP. It is the future of web transport, especially for mobile and lossy networks.

Adoption as of 2026:

- HTTP/1.1: Still powering many legacy systems and simple APIs.
- HTTP/2: Dominant for web browsers and modern APIs.
- HTTP/3: Growing rapidly, supported by all major browsers and CDNs. About 40-50% of web traffic uses HTTP/3.

Checking which version a server supports:

```bash
curl -v --http1.1 https://example.com 2>&1 | grep "HTTP/"
curl -v --http2 https://example.com 2>&1 | grep "HTTP/"
curl -v --http3 https://example.com 2>&1 | grep "HTTP/"
```

```python
import requests

# Requests library uses HTTP/1.1 by default
response = requests.get("https://example.com")
print(response.raw.version)  # 11 for HTTP/1.1

# For HTTP/2, you need httpx or another library
```

```javascript
// Browsers automatically negotiate the best HTTP version
fetch("https://example.com").then(response => {
  console.log("HTTP version negotiation is handled by the browser");
});
```

## Study Cases

### Case 1: Tracing an HTTP Request from Browser to Server and Back

A user opens `https://shop.example.com/products?category=books&page=1` in their browser.

**URL parsing:** scheme=`https`, host=`shop.example.com`, path=`/products`, query=`category=books&page=1`.

**DNS resolution:** The browser queries DNS and receives `198.51.100.23` for `shop.example.com`.

**TCP handshake:** Three-way handshake to `198.51.100.23:443`: SYN, SYN-ACK, ACK.

**TLS handshake:** ClientHello, ServerHello with certificate (valid for `*.example.com`, issued by Let's Encrypt), key exchange, Finished.

**HTTP request (HTTP/2):**
```
:method: GET
:path: /products?category=books&page=1
:authority: shop.example.com
:scheme: https
cookie: session_id=xyz789
```

**Server processing:** Nginx receives the request and forwards it to a backend application, which queries a database and renders an HTML response.

**HTTP response:** status 200, compressed HTML body, cookies for session tracking.

**Browser rendering:** The browser decompresses the HTML, discovers CSS and image resources, and fetches them over additional HTTP/2 streams on the same connection.

**Total time:** ~200-500ms depending on network conditions.

### Case 2: Why HTTP/2 Multiplexing Improves Page Load Time

A page needs `document.html` (50 KB), `styles.css` (20 KB), `app.js` (100 KB), `logo.png` (30 KB), and `hero.jpg` (200 KB).

**Under HTTP/1.1:** Requests are queued per connection. If `styles.css` is slow, `app.js` is blocked behind it (HOL blocking). Browsers open 6-8 parallel connections, but each has its own queue.

```
HTTP/1.1:  [REQ doc] -> [RES doc] -> [REQ css] -> [RES css] -> [REQ js] -> ...
```

**Under HTTP/2:** All 5 requests send simultaneously on one connection. The server responds in any order. The large `hero.jpg` does not block the small `styles.css`.

```
HTTP/2:  Stream 1: [REQ doc]  -> ... -> [RES doc]
         Stream 3: [REQ css]  -> ... -> [RES css]
         Stream 5: [REQ js]   -> ... -> [RES js]
```

**Result:** HTTP/1.1 total time = serialized sum of all responses. HTTP/2 total time = time of the slowest resource. This translates to 30-60% faster page loads on real-world sites, especially over high-latency connections.

### Case 3: What Happens When an HTTPS Certificate is Invalid

An invalid certificate triggers errors during the TLS handshake, and the client refuses to establish a secure connection.

**Expired certificate:** The server presents a certificate whose validity period has passed. The browser shows a full-page warning (e.g., `NET::ERR_CERT_DATE_INVALID` in Chrome). curl returns `SSL certificate problem: certificate has expired`. Python requests raises `requests.exceptions.SSLError`.

**Self-signed certificate:** The certificate is not signed by a trusted CA. The client rejects it with `CERTIFICATE_VERIFY_FAILED`. Bypassing with `verify=False` (Python) or `-k` (curl) is possible but dangerous and should only be used for local development.

**Hostname mismatch:** The certificate is issued for `other-site.com` but the URL points to `shop.example.com`. Error: `SSL: no alternative certificate subject name matches target host name`.

**Why this matters:** Without certificate validation, a man-in-the-middle (MITM) attack is trivial. An attacker on the same Wi-Fi network can intercept the connection, present their own certificate, and decrypt all traffic. Validation ensures you are talking to the real server, not an impostor.

## Glossary

| Term | Definition |
|------|------------|
| HTTP | Hypertext Transfer Protocol — the application-layer protocol for distributed, collaborative hypermedia systems. Foundation of data communication on the web. |
| HTTPS | HTTP over TLS — encrypted version of HTTP that provides confidentiality, integrity, and authentication. |
| Client | The entity that initiates an HTTP request (browser, app, curl, etc.). |
| Server | The entity that receives, processes, and responds to HTTP requests. |
| Request | An HTTP message sent by the client to the server, consisting of a method, URL, headers, and optionally a body. |
| Response | An HTTP message sent by the server back to the client, consisting of a status code, headers, and optionally a body. |
| URL | Uniform Resource Locator — the address of a resource on the web (scheme, host, port, path, query, fragment). |
| URI | Uniform Resource Identifier — a broader term that includes URLs (locators) and URNs (names). |
| DNS | Domain Name System — translates human-readable domain names to machine-readable IP addresses. |
| TCP | Transmission Control Protocol — a connection-oriented transport protocol providing reliable, ordered delivery of bytes. |
| TLS | Transport Layer Security — cryptographic protocol providing encryption, authentication, and integrity for data in transit. |
| SSL | Secure Sockets Layer — predecessor to TLS. Still used colloquially to refer to TLS. |
| Certificate | A digital document that binds a public key to an entity (domain name). Issued by Certificate Authorities. |
| Handshake | The initial exchange between client and server to establish a connection (TCP handshake, TLS handshake). |
| Multiplexing | Sending multiple streams of data simultaneously over a single connection (feature of HTTP/2 and HTTP/3). |
| Stateless | A protocol where each request is independent and the server retains no information about previous requests. |
| Persistent connection | Reusing the same TCP connection for multiple HTTP request-response cycles (default in HTTP/1.1). |
| Chunked transfer | An HTTP/1.1 feature allowing the server to send data in chunks without knowing the total content length upfront. |
| QUIC | Quick UDP Internet Connections — a transport protocol developed by Google, used by HTTP/3. Built on UDP. |
| Idempotent | An operation that produces the same result whether executed once or multiple times (GET, PUT, DELETE). |
| Safe method | An HTTP method that does not modify server state (GET, HEAD, OPTIONS, TRACE). |

## Quick References

- [MDN: HTTP Overview](https://developer.mozilla.org/en-US/docs/Web/HTTP/Overview) — Mozilla's comprehensive guide to the HTTP protocol, covering messages, headers, methods, and status codes.
- [HTTP: The Definitive Guide](https://www.oreilly.com/library/view/http-the-definitive/1565925092/) — O'Reilly book by David Gourley and Brian Totty covering HTTP in exhaustive detail.
- [High Performance Browser Networking](https://hpbn.co/) — Ilya Grigorik's book covering HTTP/2, QUIC, and web performance in depth. Free to read online.
- [RFC 7230](https://datatracker.ietf.org/doc/html/rfc7230) — The official HTTP/1.1 specification (message syntax and routing).
- [RFC 7540](https://datatracker.ietf.org/doc/html/rfc7540) — The official HTTP/2 specification.
- [RFC 9000](https://datatracker.ietf.org/doc/html/rfc9000) — The official QUIC transport protocol specification.

## Next Steps

- [HTTP Methods & Status Codes](../http-methods-and-status.md) — learn about the standard methods (GET, POST, PUT, DELETE, PATCH) and the full range of status codes (1xx through 5xx)
