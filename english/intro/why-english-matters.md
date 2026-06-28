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

### Common Misconceptions

**"I'll learn English by writing code."** — Code uses a tiny subset of English vocabulary. You need active practice in reading, writing, speaking, and listening outside of code.

**"My English is good enough; my code is what matters."** — As you advance, your code becomes a smaller part of your impact. Communication determines your reach.

**"Grammar checkers fix my writing."** — Tools catch some errors but cannot fix structure, logic, or clarity. Grammarly and LanguageTool are aids, not crutches.

**"Native speakers are naturally good communicators."** — No. Effective technical communication is a learned skill that requires deliberate practice regardless of native language.

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

### Example 3: Commit message

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
|---|---|
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

## Quick References

- [Google Developer Documentation Style Guide](https://developers.google.com/style) — reference for technical writing
- [RFC 2119: Key words for use in RFCs](https://datatracker.ietf.org/doc/html/rfc2119) — MUST, SHOULD, MAY definitions
- [Stack Overflow Annual Developer Survey](https://survey.stackoverflow.co/) — language and communication trends
- [The Cost of Poor Communication](https://www.shrm.org/executive-network/insights/the-cost-of-poor-communications) — business impact study
- [Write the Docs](https://www.writethedocs.org/) — community for technical writers

## Next Steps

- [Technical Writing & Documentation](../technical-writing/index.md) — learn to write clear docs
- [Grammar & Style](../grammar-and-style/index.md) — build a foundation in English grammar