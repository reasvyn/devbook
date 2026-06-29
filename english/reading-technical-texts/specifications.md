# Reading Technical Specifications

## Description

Technical specifications define how systems behave, how APIs work, how protocols communicate, and how languages are structured. This document covers the major types of specifications — API specs, protocol specs, language specs, and design documents — and teaches developers how to extract relevant information quickly, read formal grammars, and navigate specifications written by standards bodies, open-source projects, and internal teams.

## Prerequisites

- [Reading RFCs & Internet Standards](rfcs.md) — RFCs are a type of specification, and the reading strategies overlap
- [Formal Languages & Grammars](../linguistics-for-computing/formal-languages.md) — understanding BNF/EBNF grammars

## Table of Contents

- [Types of Specifications](#types-of-specifications)
- [Reading Strategy for Specifications](#reading-strategy-for-specifications)
- [API Specifications: OpenAPI / Swagger](#api-specifications-openapi--swagger)
- [Protocol Specifications](#protocol-specifications)
- [Language and Grammar Specifications](#language-and-grammar-specifications)
- [Reading BNF and EBNF Grammars](#reading-bnf-and-ebnf-grammars)
- [Design Documents](#design-documents)
- [Architecture Decision Records (ADRs)](#architecture-decision-records-adrs)
- [Extracting Information Quickly](#extracting-information-quickly)
- [Specification vs. Implementation](#specification-vs-implementation)
- [Versioning and Compatibility](#versioning-and-compatibility)
- [Writing Effective Specifications](#writing-effective-specifications)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Types of Specifications

Specifications come in many forms, each with a different structure and reading strategy.

| Type | Purpose | Examples | Typical length |
|---|---|---|---|
| API specification | Defines an API's endpoints, request/response formats, and behavior | OpenAPI spec, GraphQL schema | 100-5000 lines of YAML/JSON |
| Protocol specification | Defines the wire format, state machine, and semantics of a network protocol | HTTP/1.1 (RFC 9112), WebSocket (RFC 6455) | 30-300 pages |
| Language specification | Defines syntax, semantics, and standard library of a programming language | ECMAScript spec, C++ standard, Python language reference | 500-2000 pages |
| Data format specification | Defines the structure and encoding of a data format | JSON (RFC 8259), Protocol Buffers (proto3 spec) | 10-100 pages |
| Design document | Describes the architecture and design decisions for a system | Google Design Doc, RFC on architecture | 5-20 pages |
| ADR (Architecture Decision Record) | Documents a specific architectural decision and its rationale | Any ADR in a project repository | 1-3 pages |
| Formal specification | Defines system behavior using mathematical formalism | TLA+ spec, Alloy spec | Variable |

**Reading by purpose:**

| Purpose | Type of spec | What to read |
|---|---|---|
| Implementing a client | API spec | Endpoints, request/response formats, error handling |
| Implementing a server | Protocol spec | Wire format, state machine, security considerations |
| Understanding language behavior | Language spec | Grammar rules, type system, execution model |
| Making an architecture decision | ADR | Context, decision, consequences |
| Reviewing a system design | Design doc | Problem statement, design, alternatives considered |

### Reading Strategy for Specifications

Specifications are reference documents, not tutorials. Reading them cover-to-cover is rarely the right approach.

**The specification reading workflow:**

1. **Identify your goal.** What specific question are you trying to answer? What do you need to build or verify?

2. **Scan the structure.** Read the table of contents to locate relevant sections. Most specifications are organized hierarchically — find the chapter or section that corresponds to your question.

3. **Read the relevant section thoroughly.** Unlike a blog post, a specification may require re-reading a paragraph multiple times to extract the precise meaning.

4. **Check for normative language.** Look for MUST, SHOULD, MAY, REQUIRED, and OPTIONAL keywords. These define what is mandatory, recommended, or optional.

5. **Verify with examples.** Many specifications include examples. Compare your understanding against the examples.

6. **Check references.** Specifications often reference other specifications. Follow references when the current document assumes knowledge defined elsewhere.

7. **Note what is not specified.** Gaps in a specification are as important as what is defined. What error codes are possible? What happens in edge cases?

**The anti-patterns:**

| Anti-pattern | Why it fails | Better approach |
|---|---|---|
| Reading linearly | Most specs are reference documents, not narratives | Use the TOC to jump to relevant sections |
| Assuming completeness | No spec covers every edge case | Test against the implementation |
| Ignoring the status | Draft specs change without notice | Check whether the spec is stable |
| Confusing spec with implementation | The spec says what should happen, not what does | Verify against real behavior |

### API Specifications: OpenAPI / Swagger

OpenAPI (formerly Swagger) is the most widely used format for describing REST APIs. An OpenAPI specification is a machine-readable YAML or JSON file that defines every endpoint, request parameter, response format, and authentication method.

**OpenAPI structure:**

```yaml
openapi: 3.1.0
info:
  title: User Management API
  version: 1.0.0
  description: API for creating and managing user accounts
servers:
  - url: https://api.example.com/v1
paths:
  /users:
    get:
      summary: List all users
      parameters:
        - name: limit
          in: query
          schema:
            type: integer
            maximum: 100
      responses:
        "200":
          description: Successful response
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: "#/components/schemas/User"
    post:
      summary: Create a new user
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: "#/components/schemas/CreateUserRequest"
      responses:
        "201":
          description: User created
components:
  schemas:
    User:
      type: object
      properties:
        id:
          type: integer
        name:
          type: string
        email:
          type: string
```

**Key sections of an OpenAPI spec:**

| Section | Purpose | Questions to ask |
|---|---|---|
| info | Metadata about the API | What version? Who maintains it? |
| servers | Base URLs for the API | Are there multiple environments (staging, prod)? |
| paths | All API endpoints and methods | Are all endpoints documented? Any missing? |
| parameters | Query, path, header, cookie parameters | Which are required? What are the constraints (min, max, pattern)? |
| requestBody | Request payload format | What content types? What schema? |
| responses | Response format for each status code | Are all error codes documented? What does error responses look like? |
| security | Authentication methods | What auth is required? API key? OAuth? JWT? |
| components | Reusable schemas, parameters, responses | Are models well-structured? Is there inheritance or composition? |

**Reading an OpenAPI spec efficiently:**

1. Start with the paths — this is the API surface
2. For each endpoint you need: read parameters, request body, and expected responses
3. Trace $ref references to understand the data models
4. Check security requirements for endpoints
5. Look for pattern in error responses — is there a consistent error format?

**Common issues with OpenAPI specs:**

| Issue | Symptoms | Impact |
|---|---|---|
| Out of date | Spec describes an older version | Implementation diverges from spec |
| Missing error schemas | Error responses are "description" only with no schema | Cannot programmatically handle errors |
| Inconsistent naming | Same concept has different names in different places | Confusion, bugs |
| Overly permissive | Sparse use of pattern, minLength, maxLength | Impossible to validate inputs |
| No examples | Schemas lack example values | Hard to understand expected formats |

**Other API specification formats:**

| Format | Use case | Key features |
|---|---|---|
| GraphQL Schema Definition Language | GraphQL APIs | Type system, queries, mutations, subscriptions |
| gRPC / Protocol Buffers (proto) | gRPC services | Service definitions, message types, streaming |
| AsyncAPI | Event-driven APIs | Pub/sub, message channels, WebSocket, Kafka |
| JSON Schema | JSON data validation | JSON data structure description, validation keywords |
| RAML | REST APIs (older) | YAML-based, similar to OpenAPI |

### Protocol Specifications

Protocol specifications define the wire format and behavior of network protocols. They are the most precise type of specification — often including formal grammars, state machines, and message sequence diagrams.

**Common elements of protocol specs:**

| Element | Description | Example |
|---|---|---|
| Wire format | Byte-level layout of messages | "The first 4 bytes are the length, followed by the message payload" |
| State machine | Valid states and transitions | "LISTEN -> SYN_SENT -> ESTABLISHED -> CLOSE_WAIT -> LAST_ACK" |
| Message types | Defined messages and their structure | "Request, Response, Push, Goaway in HTTP/2" |
| Timing | Timeouts, retransmission, backoff | "RTO is computed using Karn's algorithm" |
| Error handling | How errors are communicated | "RST_STREAM for stream-level errors" |
| Extensibility | How the protocol can be extended | "TLS uses extensions in the ClientHello" |
| Security | Threats and mitigations | "TLS 1.3 removes static RSA key exchange" |

**Reading a protocol spec for implementation:**

1. Identify the message format — what bytes go on the wire
2. Identify the state machine — what states can the implementation be in
3. Identify the message flow — which messages are sent when
4. Identify error conditions — what happens when something goes wrong
5. Identify edge cases — what are the boundary conditions

**Message format notation:**

Protocol specs often use diagrams to show wire formats:

```text
 0                   1                   2                   3
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|Version|  IHL  |Type of Service|          Total Length         |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|         Identification        |Flags|      Fragment Offset    |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|  Time to Live |    Protocol   |         Header Checksum       |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                       Source Address                          |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                    Destination Address                        |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                    Options                    |    Padding    |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

Reading these diagrams: each row is 32 bits (4 bytes), and fields are positioned by their bit offsets.

### Language and Grammar Specifications

Language specifications define the syntax and semantics of programming languages. They are the final authority on what a correct program is and how it executes.

**Parts of a language specification:**

| Component | Description | Example |
|---|---|---|
| Lexical grammar | Defines tokens (keywords, operators, literals) | "Identifier ::= IdentifierStart IdentifierPart*" |
| Syntactic grammar | Defines valid program structure | "IfStatement ::= 'if' '(' Expression ')' Statement" |
| Type system | Defines types and type rules | "String is a subtype of Object" |
| Execution model | How the program executes | "Evaluation of function arguments is left-to-right" |
| Standard library | Built-in objects and functions | "Array.prototype.map" |
| Memory model | Concurrency and memory ordering | "Happens-before relationship" |

**Reading strategies for language specs:**

| Goal | Sections to read | Reading style |
|---|---|---|
| Understanding syntax | Lexical and syntactic grammar | Scan grammar rules, check examples |
| Debugging unexpected behavior | Execution model, type coercion rules | Focused search for the specific behavior |
| Implementing a parser | Full grammar, precedence rules | Exhaustive reading of grammar sections |
| Writing correct code | Semantics of specific constructs | Reference reading for specific questions |

**Navigating the ECMAScript specification:**

The ECMAScript specification is the definitive reference for JavaScript. It is available online and organized into chapters:

| Section | Content |
|---|---|
| Chapter 5 | Notational conventions — understand how the spec is written |
| Chapter 6 | ECMAScript data types and values |
| Chapter 7 | Abstract operations |
| Chapter 8 | Executable code and execution contexts |
| Chapter 9 | Ordinary and exotic object behaviors |
| Chapter 10 | Lexical grammar |
| Chapter 11 | Expressions |
| Chapter 12 | Statements |
| Chapter 13 | Functions and classes |
| Chapter 14 | Module system |
| Chapter 15-19 | Standard library (Array, Promise, Map, etc.) |

**The notation used in the ECMAScript spec:**

```text
Runtime Semantics: Evaluation
  MemberExpression : MemberExpression [ Expression ]
    1. Let baseReference be ? Evaluation of MemberExpression.
    2. Let baseValue be ? GetValue(baseReference).
    3. Let propertyNameReference be ? Evaluation of Expression.
    4. Let propertyNameValue be ? GetValue(propertyNameReference).
    5. Let bIsString be IsPropertyKey(propertyNameValue).
    6. If bIsString is true, then
         a. Return ? GetValue(baseValue, propertyNameValue).
    7. Else
         a. Return ? GetValue(baseValue, ToPropertyKey(propertyNameValue)).
```

This defines the semantics of bracket notation (`obj[expr]`). The `?` prefix indicates the operation may throw (and the error propagates). Steps are executed sequentially.

### Reading BNF and EBNF Grammars

BNF (Backus-Naur Form) and EBNF (Extended BNF) are formal notations for defining the syntax of languages. RFCs and language specifications use these to define grammar rules.

**EBNF notation (ISO 14977):**

| Symbol | Meaning | Example |
|---|---|---|
| `::=` | Is defined as | `digit ::= "0" | "1" | ... | "9"` |
| `|` | Alternation (choice) | `digit ::= "0" | "1" | ... | "9"` |
| `*` | Zero or more repetitions | `digits ::= digit*` |
| `+` | One or more repetitions | `positive-digits ::= digit+` |
| `?` | Optional (zero or one) | `sign ::= ("+" | "-")?` |
| `"..."` | Literal terminal string | `keyword ::= "if" | "while" | "for"` |
| `(...)` | Grouping | `abbreviation ::= ("Mr" | "Ms") "."` |
| `;` | End of rule | `integer ::= digit+;` |

**ABNF notation (RFC 5234):**

RFCs use ABNF, a slightly different variant:

| Symbol | Meaning | Example |
|---|---|---|
| `=` | Is defined as | `digit = %x30-39` |
| `/` | Alternation (choice) | `digit = %x30 / %x31 / ... / %x39` |
| `*` | Repetition range | `3*5DIGIT` (3 to 5 digits) |
| `*` | Zero or more | `*( "," SP )` (zero or more comma-space) |
| `1*` | One or more | `1*( DIGIT )` (one or more digits) |
| `[...]` | Optional | `[ SP ]` (optional space) |
| `%x` | Hexadecimal byte value | `%x30` is ASCII '0' |
| `%d` | Decimal byte value | `%d48` is ASCII '0' |
| `"..."` | Literal string (case-insensitive) | `"HTTP"` |

**Reading a grammar rule:**

```text
ABNF example from RFC 5321 (SMTP):

Mailbox    = Local-part "@" Domain
Local-part = Dot-string / Quoted-string
Domain     = Dot-string / Address-literal
Dot-string = Atom *( "." Atom )
Atom       = 1*Atom-char
Atom-char  = %x21-3F / %x41-7E  ; printable ASCII except space and specials
```

Reading this:

1. A Mailbox consists of a Local-part, followed by "@", followed by a Domain
2. Local-part is either a Dot-string or a Quoted-string
3. Domain is either a Dot-string or an Address-literal
4. Dot-string is an Atom, followed by zero or more dot-Atom sequences
5. Atom is one or more Atom-char characters
6. Atom-char is any printable ASCII character except space and certain special characters

**How to parse grammar rules mentally:**

1. Start from the rule you care about (the top-level rule)
2. Expand each non-terminal by substituting its definition
3. Follow alternation (`|` or `/`) to see the options
4. Follow repetition (`*`, `+`) to see how many times an element can repeat
5. The result is the set of valid strings for that rule

**Common grammar patterns:**

```text
// Alternation — choose one
Color ::= "red" | "green" | "blue"

// Sequence — all must appear in order
Greeting ::= "Hello" "," Space Name

// Repetition — zero or more
Digit ::= "0" | "1" | "2" | "3" | "4" | "5" | "6" | "7" | "8" | "9"
Integer ::= Digit+

// Optional
SignedInteger ::= ["+" | "-"] Integer

// Recursive definition
Expression ::= Term (("+" | "-") Term)*
Term ::= Factor (("*" | "/") Factor)*
Factor ::= Number | "(" Expression ")"
```

### Design Documents

Design documents describe the architecture and design of a system before it is built. They are a key tool for planning and collaboration.

**The Google Design Doc template:**

| Section | Purpose |
|---|---|
| Title and author | Who wrote this and when |
| Context and scope | What problem are we solving? What is in and out of scope? |
| Goals and non-goals | What we want to achieve (and explicitly what we do not) |
| Design | The proposed architecture, components, interfaces, data flow |
| Alternatives considered | Other approaches and why they were rejected |
| Scalability and performance | How the design handles growth |
| Security | Security implications and mitigations |
| Operations | Deployment, monitoring, incident response |
| Open questions | Issues not yet resolved |
| Timeline | When will this be delivered? |

**Reading a design document critically:**

1. **Check the goals.** Do the goals define measurable success criteria? Without clear goals, you cannot evaluate the design.
2. **Check the scope.** What is explicitly excluded? Out-of-scope items often become problems later.
3. **Check the alternatives.** Were reasonable alternatives considered? If not, the author may have a predetermined solution.
4. **Check for trade-offs.** Every design involves trade-offs. A design that lists no trade-offs has not been thought through.
5. **Check for assumptions.** What assumptions does the design make about the system, users, or environment?
6. **Check the open questions.** Are there unresolved issues that could block or change the design?

### Architecture Decision Records (ADRs)

ADRs document specific architectural decisions and their rationale. They are lightweight, version-controlled records that live alongside the code.

**ADR format (based on Michael Nygard's template):**

```markdown
# ADR-001: Use PostgreSQL for the User Database

## Status
Accepted

## Context
We need to store user profiles for 10M+ users.
We require ACID transactions, full-text search, and geospatial queries.
The team has experience with SQL databases.

## Decision
We will use PostgreSQL 15 as our primary user database.

## Consequences
- Strong consistency guarantee
- Rich query capabilities (full-text, geospatial) without additional services
- Operational overhead of database administration
- Vertical scaling limit for write-heavy workloads
```

**Reading ADRs:**

When joining a project or reviewing changes, read the ADR directory to understand:

- What decisions have been made and why
- What alternatives were considered
- What the consequences of each decision are
- Which decisions are still open

**ADR maturity levels:**

| Status | Meaning |
|---|---|
| Proposed | Under discussion, not yet accepted |
| Accepted | Decision made, moving forward |
| Deprecated | Replaced by a newer ADR |
| Superseded | Replaced by a different approach |
| Amended | Modified by a follow-up ADR |

### Extracting Information Quickly

Specifications are large. You will rarely need to read an entire specification.

**The quick extraction workflow:**

1. **Define your query.** "What is the maximum length of a hostname in HTTP/1.1?"

2. **Locate the relevant section.** Use the table of contents, index, or search function.

3. **Read the specific paragraph.** Not the entire section. Not the surrounding context. Just the paragraph that answers your question.

4. **Verify with examples.** Does an example confirm your understanding?

5. **Check for caveats.** Are there cross-references or notes that modify the answer?

6. **Extract and record.** Write the answer in your own words, with a reference to the spec and section number.

**Quick reference cards:**

Some specifications are so frequently referenced that creating a quick reference card is worthwhile:

```text
HTTP Status Codes (RFC 9110):

1xx: Informational — request received, continuing
2xx: Success — action received, understood, accepted
3xx: Redirection — further action needed
4xx: Client Error — request contains bad syntax or cannot be fulfilled
5xx: Server Error — server failed to fulfill valid request

Common codes:
200 OK, 201 Created, 204 No Content
301 Moved Permanently, 302 Found, 304 Not Modified
400 Bad Request, 401 Unauthorized, 403 Forbidden, 404 Not Found
500 Internal Server Error, 502 Bad Gateway, 503 Service Unavailable
```

### Specification vs. Implementation

The relationship between a specification and its implementation is nuanced.

**When specifications diverge from implementations:**

| Scenario | Which is right? | How to proceed |
|---|---|---|
| Implementation does not match spec | Implementation is wrong | Fix implementation |
| Spec describes impossible behavior | Spec is wrong | Report issue, update spec |
| Spec is ambiguous | Both implementations are defensible | Clarify spec, pick one behavior |
| Implementation discovers new edge case | Spec did not cover this case | Extend spec, update implementation |
| Security fix required novel behavior | Spec may need security update | Report to standards body |

**Testing against specifications:**

The most reliable way to verify a specification is to test against it:

1. Identify the normative requirements (MUST, SHOULD, MAY)
2. Write tests that verify each requirement
3. Run tests against multiple implementations
4. Where implementations differ, check the spec for the definitive answer

### Versioning and Compatibility

Specifications version to manage change. Understanding versioning helps you know what to expect.

**Versioning strategies:**

| Strategy | Examples | Semantics |
|---|---|---|
| Major.minor.patch | OpenAPI 3.1.0 | Major = breaking, minor = additive, patch = fixes |
| Date-based | ECMAScript 2024 | Annual releases with accumulated changes |
| Sequential | HTTP/1.0, HTTP/1.1, HTTP/2, HTTP/3 | New version numbers for significant changes |
| Feature-based | TLS 1.0, TLS 1.1, TLS 1.2, TLS 1.3 | Version increments with feature additions |

**Compatibility considerations when reading specs:**

- Does the spec describe the latest version or a legacy version?
- Are there migration guides from previous versions?
- Are deprecated features marked?
- Does the spec define version negotiation?

## Study Cases

### Case 1: Implementing from an OpenAPI Spec

A developer needs to implement a client for a third-party REST API. They have only the OpenAPI spec.

Steps:

1. Open the spec in Swagger Editor or a similar viewer
2. Find the endpoints needed for the use case
3. For each endpoint: read parameters, request body, and response schema
4. Check security: the spec shows Bearer token authentication
5. Generate client code using openapi-generator

Issues found:

- The spec defines response schemas but not error schemas
- One endpoint's response is marked as "application/json" but returns "text/plain" in practice
- The pagination parameters are not documented — the developer must reverse-engineer them

The developer submits a PR to the API provider with the missing error schemas and pagination parameters.

### Case 2: Resolving a JavaScript Mystery

A developer encounters strange behavior: `Array(3)` creates `[empty x 3]` but `Array(3, "a")` creates `["a"]`. The developer checks the ECMAScript specification.

Reading the spec:

- Section 22.1.1.1: `Array(0)` — reads the definition of `Array()` with a single argument
- Section 22.1.1.2: `Array(...items)` — reads the definition with multiple arguments
- The single-argument form sets `length` but does not create elements
- The multi-argument form creates elements from the arguments

Understanding the spec reveals:

- `Array(3)` uses the single-argument form — sets length to 3, no elements
- `Array(3, "a")` uses the multi-argument form — creates array with elements 3 and "a"
- This is not a bug, it is the specified behavior

Had the developer inferred from examples alone, they might have assumed it was a bug. Reading the spec clarifies the design.

### Case 3: Reading an ADR for System Context

A developer joins a new team. Before making any changes, they read the ADRs:

- ADR-001: Use PostgreSQL for persistence — explains the data store choice
- ADR-009: Use Kafka for event-driven communication — explains the messaging system
- ADR-015: Adopt CQRS for the order management system — explains the query model

With this context, the developer understands why certain architectural choices were made and can make decisions that align with the existing direction.

Later, the developer proposes a change. They write ADR-021: Move from Kafka to RabbitMQ for low-latency order processing.

In the ADR, they reference:

- ADR-009 (superseded) — the original decision they are now changing
- Benchmark data showing RabbitMQ's latency advantage
- The trade-off: losing Kafka's long-term retention

The ADR process enables the team to make data-driven, documented decisions.

## Examples

### Example 1: Reading an ABNF Rule

ABNF for an HTTP header field value:

```text
field-value    = *( field-content / obs-fold )
field-content  = field-vchar [ 1*( SP / HTAB ) field-vchar ]
field-vchar    = VCHAR / obs-text
obs-fold       = CRLF 1*( SP / HTAB )
```

Reading this:

1. A field-value is zero or more occurrences of field-content or obs-fold
2. A field-content is a field-vchar, optionally followed by whitespace and another field-vchar
3. obs-fold is a continuation line (deprecated but allowed for backwards compatibility)
4. This means header values can span multiple lines using obs-fold, but modern implementations should not produce them

### Example 2: ECMAScript Type Coercion

The `==` operator in JavaScript is notoriously confusing. The ECMAScript spec defines the abstract operation `Abstract Equality Comparison` in section 7.2.14:

```text
1. If Type(x) is the same as Type(y), then
   a. Return the result of performing Strict Equality Comparison x === y.
2. If x is null and y is undefined, return true.
3. If x is undefined and y is null, return true.
4. If Type(x) is Number and Type(y) is String, return x == ! ToNumber(y).
5. If Type(x) is String and Type(y) is Number, return ! ToNumber(x) == y.
6. ...
```

Reading this explains why `1 == "1"` is true (step 4 converts the string to a number) but `1 === "1"` is false (step 1 requires same type). The spec is the authoritative resource for understanding these behaviors.

### Example 3: Design Document Evaluation

A design document proposes using WebSockets for real-time updates.

Reading critically:

- **Goals:** "Real-time updates with sub-second latency" — measurable, good
- **Alternatives considered:** Server-Sent Events (SSE) and polling are mentioned but dismissed without benchmarks — weak analysis
- **Trade-offs:** The document does not discuss connection limits, reconnection logic, or message ordering guarantees
- **Assumptions:** Assumes all clients support WebSockets (requires HTTP/1.1 upgrade or HTTP/2)
- **Open questions:** No discussion of authentication during WebSocket handshake or reconnection

The reviewer asks for SSE benchmarks and a discussion of reconnection handling before approving.

### Example 4: Quick Information Extraction

Question: "What is the maximum allowed size of an HTTP header in HTTP/1.1?"

Steps:

1. Search RFC 9112 for "limit" or "maximum"
2. Section 5.6.2: "There is no inherent limit to the length of any header field-value"
3. But Section 5.6.2 continues: "To protect against denial-of-service attacks, implementations MUST support a minimum of at least 8000 bytes"
4. Note: The spec says implementations must support at least 8KB, but individual implementations may impose their own limits

Answer: There is no hard limit in the spec, but implementations must support at least 8KB, and servers may reject larger headers.

## Glossary

| Term | Definition |
|---|---|
| ABNF | Augmented Backus-Naur Form — notation for defining protocol grammars in RFCs |
| ADR | Architecture Decision Record — document capturing a specific architectural decision |
| Alternation | Choice between alternatives in a grammar (represented by `|` or `/`) |
| BNF | Backus-Naur Form — notation for describing the syntax of formal languages |
| EBNF | Extended Backus-Naur Form — BNF with additional operators for repetition and optionality |
| Formal grammar | A set of production rules that define a language |
| IANA | Internet Assigned Numbers Authority — manages protocol parameter registries |
| Normative language | Keywords (MUST, SHOULD, MAY, etc.) that define requirement levels in specifications |
| Production rule | A rule in a formal grammar that defines how symbols can be rewritten |
| Schema | Formal definition of data structure and constraints |
| Semantics | The meaning of a program or protocol — what it does, not just its syntax |
| Specification | A formal document that defines the behavior of a system, protocol, or language |
| Syntax | The structure of valid statements in a language |
| Terminal | A literal symbol in a grammar (as opposed to a non-terminal that must be expanded) |

## Quick References

- [OpenAPI Specification](https://spec.openapis.org/oas/latest.html) — current OpenAPI specification
- [JSON Schema](https://json-schema.org/) — JSON data validation specification
- [ECMAScript Specification](https://tc39.es/ecma262/) — JavaScript language specification
- [ISO 14977: EBNF](https://www.iso.org/standard/26153.html) — standard EBNF notation
- [RFC 5234: ABNF](https://www.rfc-editor.org/rfc/rfc5234) — augmented BNF for RFCs
- [Google Design Doc Template](https://www.industrialempathy.com/posts/design-docs-at-google/) — design document approach
- [Michael Nygard's ADR Template](https://github.com/joelparkerhenderson/architecture-decision-record) — original ADR format
- [AsyncAPI Specification](https://www.asyncapi.com/docs/specifications/latest) — event-driven API specification
- [Protocol Buffers Language Guide](https://protobuf.dev/programming-guides/proto3/) — proto3 specification

## Next Steps

- [Evaluating Sources & Claims](evaluating-sources.md) — assessing the credibility of specifications and technical content
