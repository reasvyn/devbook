# Why English Matters for Developers

## Description

Code is written in programming languages, but everything around it — docs, reviews, RFCs, emails, specs, postmortems — is written in human language. English proficiency determines how effectively an engineer communicates, collaborates, and contributes.

## Prerequisites

None. This is the entry point for the English subject.

## Table of Contents

- [Code Is Not Enough](#code-is-not-enough)
- [The Developer's Communication Stack](#the-developers-communication-stack)
- [English as the Lingua Franca of Technology](#english-as-the-lingua-franca-of-technology)
- [Reading: The Input Side](#reading-the-input-side)
- [Writing: The Output Side](#writing-the-output-side)
- [Speaking & Listening](#speaking--listening)
- [The Cost of Poor Communication](#the-cost-of-poor-communication)
- [English for Career Growth](#english-for-career-growth)
- [Common Misconceptions](#common-misconceptions)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Code Is Not Enough

A common belief among junior developers is that code speaks for itself. In practice, the most impactful engineers are those who communicate as clearly as they code. Code review comments, architecture proposals, documentation, and cross-team coordination all depend on language.

```python
# A well-named function with a clear docstring communicates intent
def calculate_monthly_interest(balance: Decimal, annual_rate: float) -> Decimal:
    """Return the monthly interest for a given balance and annual percentage rate.

    Uses the formula: monthly = balance * (annual_rate / 12 / 100).
    Assumes a standard 30/360 day count convention.
    """
    monthly_rate = Decimal(str(annual_rate)) / Decimal('12') / Decimal('100')
    return balance * monthly_rate
```

The difference between a maintainable codebase and an unmaintainable one often comes down to the comments, naming, and documentation — all written in English.

In remote and distributed teams, written communication becomes even more critical. Without hallway conversations or whiteboard sessions, every technical discussion, design decision, and process must be captured in text. Engineers who communicate clearly in writing have an outsized impact in distributed organizations.

Consider the difference between these two approaches to documenting the same decision:

```text
// Vague
We decided to use PostgreSQL.

// Clear, context-rich
Decision: Use PostgreSQL as the primary data store.

Context: We evaluated PostgreSQL, MySQL, and CockroachDB for
our multi-tenant SaaS platform. PostgreSQL was chosen because:
- Rich JSON support for flexible tenant schemas
- Mature full-text search, reducing external dependencies
- Stronger consistency guarantees than MySQL's group replication
- Team has existing PostgreSQL expertise from the previous project

Avoided: CockroachDB was ruled out due to operational complexity
for a team of three.
```


### The Developer's Communication Stack

Communication in software engineering happens at multiple layers:

| Layer | Medium | Audience | English Skill |
|---|---|---|---|
| Code | Comments, naming, commit messages | Future self, teammates | Clarity, conciseness |
| Code review | Pull request comments | Peers | Constructive feedback |
| Documentation | README, API docs, wiki | All developers | Technical writing |
| Design | RFCs, ADRs, specs | Engineering team | Persuasive writing |
| Coordination | Slack, email, meetings | Cross-functional | Professional communication |
| Incident | Postmortems, status updates | Stakeholders | Crisis communication |
| External | Blog posts, conference talks | Community | Storytelling |

Each layer requires a different register, vocabulary, and stylistic approach.

### English as the Lingua Franca of Technology

English is the de facto language of:

- **Programming languages** — keywords, error messages, standard library APIs are all in English
- **Documentation** — MDN, Python docs, AWS docs, Kubernetes docs
- **Stack Overflow** — 58 million questions and answers
- **Specifications** — RFCs, W3C standards, ECMAScript spec
- **Open source** — commit messages, issues, PRs, maintainer communication
- **Conferences** — PyCon, KubeCon, React Conf, Strange Loop
- **Academic papers** — arXiv, ACM, IEEE publications

For non-native speakers, investing in English directly translates to access to a larger body of knowledge and a wider professional network.

The dominance of English in technology creates a disparity: native speakers start with an advantage, while non-native speakers must invest extra effort to reach the same level of technical communication. Recognizing this gap is the first step toward closing it.

English is also the language of cutting-edge research. AI/ML papers on arXiv, programming language research at PLDI/OOPSLA, systems research at SOSP/OSDI — all published in English. A developer who reads English research papers has a 6-18 month head start on trends before they reach mainstream tutorials and blog posts.


### Reading: The Input Side

Developers read constantly:

```python
# Reading RFCs requires understanding complex conditional logic in natural language
"""
RFC 7230 Section 5.4:
"A client MUST send a Host header field in all HTTP/1.1 request messages.
 If the target URI includes an authority component, then the Host field
 value MUST be identical to that authority component."
"""

# The word "MUST" (RFC 2119) means it is a requirement, not a recommendation
```

Reading skills needed:
- **Scanning** — finding relevant information quickly
- **Critical reading** — evaluating arguments, detecting bias
- **Comprehension** — understanding complex conditional statements
- **Vocabulary** — technical terms, acronyms, jargon

**The SQ3R method for technical reading:**

| Step | Action | Example |
|---|---|---|
| **Survey** | Skim headings, diagrams, summaries | Read section headers of an RFC before diving in |
| **Question** | Turn headings into questions | "What authentication mechanisms does this API support?" |
| **Read** | Read actively, looking for answers | Underline relevant sections |
| **Recite** | Summarize in your own words | Write a 3-sentence summary of each section |
| **Review** | Revisit material periodically | Re-read notes before a related design discussion |

This method works particularly well for specifications, RFCs, and technical documentation — the primary reading material for developers.

### Writing: The Output Side

Writing is where most developers struggle. Common issues:

```python
# Poor commit message
def fix():
    pass

# Good commit message
def fix_login_redirect():
    """Fix login redirect loop when session cookie is malformed.

    The previous implementation did not validate the session cookie
    before attempting to decode it, causing a ValueError that was
    caught by the generic exception handler, which redirected back
    to the login page without an error message.

    The fix adds cookie validation and returns a meaningful error
    to the user instead of the redirect loop.
    """
```

Good technical writing principles:
- **Know your audience** — junior devs need more context than senior devs
- **One idea per paragraph** — break complex concepts into digestible pieces
- **Use active voice** — "the function returns" not "it is returned by the function"
- **Prefer short sentences** — aim for 15–20 words
- **Use lists** — bullet points for parallel items, numbered lists for sequences

**Common writing patterns in codebases:**

```python
# Bad: vague comment that restates the code
def add(a, b):
    # Add a and b together
    return a + b

# Good: explains why, not what
def add(a, b):
    """Return the sum of two numbers.
    
    This function exists to provide a typed wrapper around Python's
    native addition operator, ensuring both operands are numeric.
    """
    if not (isinstance(a, (int, float)) and isinstance(b, (int, float))):
        raise TypeError("Both arguments must be numeric")
    return a + b
```

**Documentation style comparison:**

```
// API doc: tells you what the function does
func Connect(address string, port int) (*Connection, error)

// Better API doc: tells you what, why, and how
// Connect establishes a TCP connection to the given address and port.
// It retries up to 3 times with exponential backoff.
// Returns an error if all retries are exhausted.
func Connect(address string, port int) (*Connection, error)
```

Good technical writing answers three questions: What does this do? Why would I use it? How does it handle edge cases?

### Speaking & Listening

Oral communication in engineering contexts:

- **Standups** — concise status updates (what I did, what I'll do, blockers)
- **Technical discussions** — explaining tradeoffs, proposing solutions
- **Presentations** — demos, talks, knowledge sharing sessions
- **Interviews** — system design, behavioral questions

Key techniques:
- Think before speaking — pause 1–2 seconds to organize thoughts
- Structure answers: context → approach → result
- Ask clarifying questions — "let me make sure I understand..."
- Paraphrase to confirm understanding

**Meeting communication patterns:**

```text
Standup update template:
"Yesterday I worked on [feature/bug]. Today I'll work on [next step].
I'm blocked by [issue]. I need help with [specific ask]."

Design discussion contribution:
"One approach would be [proposal]. The tradeoff is [pro/con].
An alternative is [alternative]. What does everyone think?"
```

**Handling difficult conversations:**
- Giving constructive feedback: "I noticed [specific behavior]. The impact was [result]. Instead, could we [alternative]?"
- Disagreeing respectfully: "I see it differently. Here's my reasoning: [evidence]. What am I missing?"
- Acknowledging mistakes: "I made an error in [area]. Here's what happened: [facts]. Here's my fix: [plan]."

**Async communication best practices:**
- Use descriptive subject lines: "[RFC] Database migration strategy for multi-tenancy" instead of "Question about database"
- Include context in the first message: don't make people ask "which database?"
- Define next steps clearly: "Could someone from the platform team review this by Friday?"
- Use formatting for scannability: headings, bullet lists, bold for key terms

### The Cost of Poor Communication

Quantifying the impact:

| Scenario | Cost of Poor Communication |
|---|---|
| Misunderstood requirements | Weeks of rework |
| Unclear code review feedback | Merged buggy code, team friction |
| Incomplete documentation | Hours of onboarding time per new hire |
| Vague postmortem | Repeat incidents, no learning |
| Unclear email | Back-and-forth clarification, delayed decisions |

A single ambiguous requirement can cost a team of four engineers one week of work — that is four engineering-weeks wasted because a sentence was unclear.

**Communication overhead by team size:**

| Team Size | Communication Channels | Overhead |
|-----------|---|----------|
| 2 | 1 | Minimal |
| 5 | 10 | Moderate |
| 10 | 45 | Significant |
| 20 | 190 | High |
| 50 | 1,225 | Requires structure |

As teams grow, the cost of unclear communication multiplies. What takes one email to clarify in a 5-person team might require a meeting, a follow-up doc, and multiple Slack threads in a 20-person team.

**Real-world examples of communication failures:**

- **The Martian units incident (1999):** NASA lost the $125M Mars Climate Orbiter because one team used imperial units and another used metric. The code was correct — the communication was not.
- **The Knight Capital bug (2012):** A deployment that should have been clearly documented and reviewed caused a $440M loss in 45 minutes. The ambiguity was in the deployment process documentation, not the code.
- **The Healthcare.gov launch (2013):** Poor communication between contractors, unclear specifications, and missing documentation contributed to one of the most expensive IT failures in US history.

### English for Career Growth

English proficiency correlates with career advancement:

- **Junior → Mid** — completing tasks, writing clear comments, asking good questions
- **Mid → Senior** — writing design docs, reviewing PRs with clear feedback, mentoring
- **Senior → Staff** — influencing without authority, writing RFCs, cross-team communication
- **Staff → Principal** — setting technical vision, writing strategy docs, public speaking

At each level, the ratio of code-writing to communication shifts dramatically:

| Level | Code | Communication |
|---|---|---|
| Junior | 80% | 20% |
| Mid | 60% | 40% |
| Senior | 40% | 60% |
| Staff | 25% | 75% |
| Principal | 15% | 85% |

**Communication skills expected at each level:**

| Level | Written Communication | Verbal Communication | Reach |
|-------|----------------------|---------------------|-------|
| Junior | Clear comments, basic docs | Standup updates | Team |
| Mid | PR reviews, feature specs | Design discussions | Team + adjacent teams |
| Senior | RFCs, architecture docs | Presentations, mentoring | Organization |
| Staff | Strategy docs, cross-team RFCs | Leadership meetings, talks | Department |
| Principal | Technical vision, standards | Conference talks, exec briefings | Company + industry |

The pattern is clear: communication reach expands with seniority. A principal engineer's written RFC might influence decisions across the entire engineering organization. The ability to write clearly at that scale is not optional.

### Common Misconceptions

**"I'll learn English by writing code."** — Code uses a tiny subset of English vocabulary. You need active practice in reading, writing, speaking, and listening outside of code.

**"My English is good enough; my code is what matters."** — As you advance, your code becomes a smaller part of your impact. Communication determines your reach.

**"Grammar checkers fix my writing."** — Tools catch some errors but cannot fix structure, logic, or clarity. Grammarly and LanguageTool are aids, not crutches.

**"Native speakers are naturally good communicators."** — No. Effective technical communication is a learned skill that requires deliberate practice regardless of native language.

**"I'll improve by just reading more code."** — Code conveys logic but rarely explains rationale, tradeoffs, or design philosophy. Those are communicated in prose — in READMEs, RFCs, and design docs. Reading code alone will not improve your ability to write those documents.

**"Writing docs is someone else's job."** — In most engineering teams, every engineer owns documentation for the code they write. Dedicated technical writers exist at large companies, but even there, engineers provide the raw material.

**"My code is self-documenting."** — Self-documenting code reduces the need for comments but does not eliminate the need for documentation. The purpose of a module, the reason behind an architecture decision, the tradeoffs of an algorithm — these cannot be expressed in code alone.

## Study Cases

### Case 1: The Ambiguous Spec

A product manager writes: "The user should be able to delete their account." The developer implements soft-delete (sets a flag). The PM meant hard-delete (removes all data). Result: a week of rework and a missed deadline.

Fix: The developer should have written a clarification: "By 'delete,' do you mean soft-delete (mark as inactive, preserve data) or hard-delete (irreversibly remove all records)?" Then document the decision in the ticket.

### Case 2: The Code Review That Went Wrong

Original review comment: "This doesn't look right. Fix it."

Better: "The error handling on line 47 swallows the exception without logging. If this API call fails in production, we will have no trace of it. Consider logging the error with context before re-raising, or use a structured log statement like `logger.error('payment_failed', order_id=order.id)`."

The second version explains the problem, the consequence, and the solution — all in clear English.

### Case 3: The Postmortem

Poor postmortem: "The database went down and users couldn't log in. We restarted it."

Good postmortem: "Root cause: An unpatched memory leak in the connection pooler caused the primary database node to exhaust available memory after 72 hours of uptime, triggering OOM kill by the kernel. Detection: PagerDuty alert fired at 03:14 UTC. Mitigation: Restarted the database service after increasing `max_connections` from 200 to 300 as a temporary measure. Resolution: Deployed a fix that releases idle connections after 30 seconds (PR #4821). Preventative: Added memory usage monitoring to Grafana with a 60% threshold alert."

### Case 4: The RFC That Got Approved

Two engineers write RFCs proposing the same thing: adopting a message queue for async task processing.

**RFC A (vague):**
```
We should use RabbitMQ for async tasks. It's reliable and widely used.
```

**RFC B (detailed):**
```
Proposal: Adopt RabbitMQ for asynchronous task processing.

Motivation: Our current in-process task queue blocks the web server
during long-running operations, causing 5-second+ response times
under load. We need a message broker that:
- Decouples task producers from consumers
- Persists messages to survive process restarts
- Supports priority queuing for time-sensitive tasks
- Integrates with our existing stack (Python, Docker, Kubernetes)

Evaluation:
| Option | Durability | Throughput | Ops Burden | Team Familiarity |
|--------|-----------|-----------|-----------|-----------------|
| RabbitMQ | High | 50k/s | Moderate | High (used in data team) |
| Redis | Low | 100k/s | Low | High |
| SQS | High | Unlimited | None | Low |
| Kafka | High | 1M/s | High | Low |

Recommendation: RabbitMQ — balances durability with team familiarity.
Next: POC to validate throughput meets our 10k tasks/hour peak.
```

RFC B gets approved in the first review. RFC A requires three rounds of back-and-forth and eventually stalls. The difference is the quality of the written communication.

## Examples

### Example 1: Rewriting a vague error message

```python
# Before
raise Exception("Something went wrong")

# After
class InsufficientFundsError(Exception):
    """Raised when an account lacks sufficient balance for a transaction."""

raise InsufficientFundsError(
    f"Account {account_id} has balance ${balance:.2f}, "
    f"but transaction requires ${amount:.2f}"
)
```

### Example 2: Documentation snippet

```
# Bad:
To use the API, you need to send a request. It will return data.

# Good:
GET /api/v2/users/:id

Retrieves a user by their unique identifier.

Parameters:
  - id (string, required): The 24-character hex user ID.

Response (200):
  { "id": "...", "name": "...", "email": "..." }

Response (404):
  { "error": "user_not_found", "message": "No user with the given ID exists." }
```

### Example 3: Slack message clarity

```text
Bad:
"Hey, the build is failing. Can someone look at it?"

Good:
"@platform-team The CI build for the main branch is failing on
the 'test-migrations' step (build #1423). The error suggests a
schema mismatch between the latest migration and the test
database seed. Logs: https://ci.example.com/builds/1423
Looking for someone to investigate. I'm happy to help if you
point me in the right direction."
```

The good message includes: who (specific team), what (which build, which step), evidence (logs link), and a specific ask.

### Example 4: Architectural decision record

```text
# ADR-001: Adopt Protocol Buffers for service-to-service communication

Status: Accepted
Date: 2024-03-15

Context: Our REST API uses JSON for serialization. As we scale to
50+ microservices, we need a more efficient serialization format
that supports strong typing and schema evolution.

Decision: Use Protocol Buffers (protobuf) v3 for all internal
service-to-service RPC calls. External-facing APIs remain REST/JSON.

Consequences:
- (+) 10x faster serialization vs JSON
- (+) Strong typing reduces integration bugs
- (+) Schema evolution via field numbering
- (-) Requires schema registry
- (-) Learning curve for the team
- (-) Debugging requires additional tooling (protoc, grpcurl)
```

### Example 5: Commit message

```
fix(auth): validate session cookie before decoding

The login redirect loop occurred because SessionMiddleware.decode()
was called on an empty cookie value, which raised a ValueError.
The global exception handler caught this and redirected to /login,
creating an infinite loop.

Fix: check that the cookie exists and is non-empty before calling
decode(). If invalid, return a 401 with an explicit error message
instead of redirecting.
```

## Glossary

| Term | Definition |
|---|---|---|
| Lingua franca | A common language used for communication between people who speak different native languages |
| Register | Level of formality in language (casual, professional, technical) |
| RFC | Request for Comments — a document format for technical specifications and proposals |
| ADR | Architecture Decision Record — a document capturing an architectural decision and its rationale |
| Active voice | Sentence structure where the subject performs the action ("the function returns a value") |
| Passive voice | Sentence structure where the subject receives the action ("the value is returned") |
| Postmortem | A document analyzing an incident after resolution |
| Code review | Systematic examination of code changes by peers |
| Async communication | Communication that does not happen in real time (email, documents, comments) |
| Docstring | Documentation string embedded in source code |
| SQ3R | A reading comprehension method: Survey, Question, Read, Recite, Review |
| Executive summary | A brief overview of a document placed at the beginning for busy readers |
| RACI matrix | A responsibility assignment chart: Responsible, Accountable, Consulted, Informed |
| Blameless culture | An approach to incident analysis focused on systemic causes rather than individual faults |
| Stakeholder | Anyone with an interest or investment in a project outcome |
| Action item | A task assigned during a meeting with an owner and deadline |
| Deliverable | A tangible output or result produced as part of a project |
| ROI | Return on Investment — measure of benefit relative to cost |
| Scope creep | Uncontrolled expansion of a project's requirements without adjustments to resources |
| LGTM | "Looks Good To Me" — common code review approval shorthand |
| WIP | "Work In Progress" — indicates incomplete work |
| TL;DR | "Too Long; Didn't Read" — a brief summary of a longer text |
| SME | Subject Matter Expert — someone with deep knowledge in a specific area |
| OKR | Objectives and Key Results — a goal-setting framework |
| PR/PL | Pull Request / Patch List — code change submitted for review |
| ETA | Estimated Time of Arrival / Completion |
| FYI | "For Your Information" — used to share information without requiring action |
| AFAIK | "As Far As I Know" — caveat expressing limited certainty |
| IMO/IMHO | "In My Opinion" / "In My Humble Opinion" |

## Quick References

- [Google Developer Documentation Style Guide](https://developers.google.com/style) — reference for technical writing
- [RFC 2119: Key words for use in RFCs](https://datatracker.ietf.org/doc/html/rfc2119) — MUST, SHOULD, MAY definitions
- [Stack Overflow Annual Developer Survey](https://survey.stackoverflow.co/) — language and communication trends
- [The Cost of Poor Communication](https://www.shrm.org/executive-network/insights/the-cost-of-poor-communications) — business impact study
- [Write the Docs](https://www.writethedocs.org/) — community for technical writers
- [Plain English Campaign](https://www.plainenglish.co.uk/) — guides for clear communication
- [Hemingway Editor](https://hemingwayapp.com/) — tool for improving readability
- [Grammarly](https://www.grammarly.com/) — AI writing assistant for grammar and clarity
- [Dreyer's English: An Utterly Correct Guide to Clarity and Style](https://www.penguinrandomhouse.com/books/529341/dreyers-english-by-benjamin-dreyer/) — modern style guide for clear writing
- [The Elements of Style, Strunk & White](https://www.penguinrandomhouse.com/books/160938/the-elements-of-style-by-william-strunk-jr/) — classic guide to clear writing
- [On Writing Well, William Zinsser](https://www.harpercollins.com/products/on-writing-well-william-zinsser?variant=32207288025122) — principles of nonfiction writing
- [Amazon's 6-Page Memo](https://www.linkedin.com/pulse/how-structure-amazon-6-pager-team-gets-things-done-michael-schrems/) — template for structured written communication

## Next Steps

- [Technical Writing & Documentation](../technical-writing/index.md) — learn to write clear docs
- [Grammar & Style](../grammar-and-style/index.md) — build a foundation in English grammar