# Writing Effective Code Review Comments

## Description

Code review is a core engineering practice, and the quality of your comments determines whether the review process is collaborative or confrontational. This document covers how to write clear, constructive, and psychologically safe code review comments that improve code quality while maintaining team relationships.

## Prerequisites

- [Active vs Passive Voice](../grammar-and-style/active-vs-passive.md) — using direct language in review comments
- [Conciseness](../grammar-and-style/conciseness.md) — writing short, focused review feedback

## Table of Contents

- [Why Code Review Comments Matter](#why-code-review-comments-matter)
- [Principles of Constructive Feedback](#principles-of-constructive-feedback)
- [Comment Tone and Psychology](#comment-tone-and-psychology)
- [Distinguishing Nits from Blockers](#distinguishing-nits-from-blockers)
- [Asking Questions vs. Making Demands](#asking-questions-vs-making-demands)
- [Providing Context for Suggestions](#providing-context-for-suggestions)
- [Linking to Style Guides and Best Practices](#linking-to-style-guides-and-best-practices)
- [Code Review Etiquette](#code-review-etiquette)
- [Handling Disagreements](#handling-disagreements)
- [The Anatomy of a Good Comment](#the-anatomy-of-a-good-comment)
- [Comment Anti-Patterns](#comment-anti-patterns)
- [Reviewing Different Types of Changes](#reviewing-different-types-of-changes)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Why Code Review Comments Matter

Code review serves multiple purposes beyond catching bugs. It is a knowledge-sharing mechanism, a quality gate, and a team-building practice. The comments you write directly affect all three.

**Knowledge sharing:** Every comment explains a design decision, teaches a pattern, or points to a resource. Junior engineers learn from senior engineers through these comments. Senior engineers also learn when they see a novel approach from a junior teammate.

**Quality gate:** Comments catch logic errors, security vulnerabilities, performance issues, and style violations that automated tools miss. A well-written comment can prevent a production incident.

**Team building:** The tone and content of your comments set the cultural norms for your team. Respectful, constructive comments build psychological safety. Dismissive or overly critical comments erode trust.

### Principles of Constructive Feedback

**Focus on the code, not the author.** Separate the person from the work product. Instead of "You forgot to handle the null case," write "This function does not handle the null case."

**Be specific.** Vague feedback is not actionable. Instead of "This could be better," write "Extracting this validation logic into a separate function would improve testability."

**Provide rationale.** Explain why you are suggesting a change. A comment without rationale is a command. A comment with rationale is a teaching moment.

```text
// Without rationale
Use early return here.

// With rationale
Use early return here to reduce nesting and make the happy path immediately visible.
```

**Offer alternatives, not ultimatums.** Present your suggestion as one option among several. The author may have context you lack.

```text
// Ultimatum
Change this to use async/await.

// Alternative
Consider using async/await here for consistency with the rest of the codebase. The Promise.then pattern works too, but mixing styles can confuse readers.
```

**Acknowledge good code.** Positive reinforcement encourages good practices. A simple "Nice use of the strategy pattern here" goes a long way.

**Be humble.** You might be wrong. Frame your comments to invite discussion.

```text
// Dogmatic
This is wrong.

// Humble
I think this might have a race condition. Let me know if I'm missing something.
```

### Comment Tone and Psychology

Code review is an inherently vulnerable activity. The author has invested time and mental energy into their work. Criticism, even when justified, triggers a defensive response. Understanding this psychology helps you write comments that land well.

**The defensive response:** When someone receives criticism, their brain activates the same regions as a physical threat. This is evolutionary. Comments that feel like personal attacks trigger fight-or-flight.

**Mitigation strategies:**

Use "we" and "our" instead of "you" and "your." This frames the code as a shared responsibility.

```text
// "You" language
You should move this validation earlier.

// "We" language
We should move this validation earlier to fail fast.
```

Avoid absolute language. Words like "never," "always," "obviously," and "clearly" signal contempt for the author's choices.

```text
// Absolute
This is obviously wrong.

// Measured
This approach will not work when the input is empty. Let's discuss.
```

Use the "sandwich" method sparingly. Sandwiching criticism between two compliments feels manipulative when overused. Instead, be direct but kind.

Acknowledge context. The author may have been working under constraints you do not see. Recognize that trade-offs exist.

```text
I know this deadline is tight. The current approach works, but I think we could
reduce technical debt by factoring out the database logic. What do you think?
```

### Distinguishing Nits from Blockers

Not all comments carry the same weight. Clearly labeling the severity of your feedback helps the author prioritize.

**Blockers** are issues that must be addressed before merging. These include logic errors, security vulnerabilities, data loss risks, API contract violations, and test failures.

```text
[Blocker] This endpoint does not validate the user's permissions before
returning sensitive data. An authenticated user from any organization could
access this information. Please add authorization checks.
```

**Nits** are style preferences or minor improvements. They should not block the merge. Clearly marking them as optional gives the author agency.

```text
[Nit] The variable name `x` is not descriptive. Consider `retryCount` or
something similar. Feel free to ignore if you prefer the shorter name.
```

**Questions** seek clarification. They are not demands. Labeling them as questions reduces pressure on the author to change anything.

```text
[Question] Why did you choose a linked list over an array here? Is there a
performance consideration I'm missing?
```

**Suggestions** are ideas for improvement that are neither blockers nor nits. They offer optional directions.

```text
[Suggestion] Have you considered using a builder pattern here? It might reduce
the constructor parameter count. No strong opinion.
```

Some teams use labels like `blocking:`, `nit:`, `question:`, `suggestion:`, `thought:` in comment prefixes. Choose a convention and apply it consistently.

### Asking Questions vs. Making Demands

The difference between a question and a demand often comes down to phrasing. Questions invite collaboration. Demands shut it down.

**Demands typically:**

- Use imperative mood without explanation
- Assume the reviewer's preference is objectively correct
- Leave no room for discussion

```text
// Demand
Use dependency injection here instead of the service locator.

// Question
Could dependency injection work better here? The service locator pattern makes it
harder to unit test this class. What trade-offs did you consider?
```

**Good questions:**

- Express genuine curiosity
- Acknowledge the author may have valid reasons
- Offer a specific concern behind the question

```text
I see you used a mutable default argument here. Was there a specific reason?
This pattern can lead to surprising behavior when the function is called
multiple times without an explicit argument.
```

**Leading questions** can feel like accusations disguised as questions. Avoid them.

```text
// Leading (accusatory)
Why didn't you just use the built-in sort function?

// Genuine
Was there a reason you implemented a custom sort instead of using the built-in?
```

### Providing Context for Suggestions

A comment with context is ten times more valuable than a comment without. Context helps the author understand the reasoning behind the suggestion and internalize the lesson for future work.

**Types of context to include:**

**Logical context:** Explain the reasoning step by step.

```text
This loop runs in O(n^2) because for every element, you scan the entire list
again. Using a hashmap lookup reduces this to O(n). Here's how:
```

**Historical context:** Reference past incidents or discussions.

```text
We had a production incident last quarter because of a similar pattern. The
connection pool was exhausted because connections were not released in the
finally block. Let's use a try-with-resources here.
```

**Architectural context:** Connect the suggestion to the broader system.

```text
This module is part of the payment pipeline, which runs on a strict SLA. Any
blocking I/O in this path can cause timeouts. Consider using the asynchronous
version of this library.
```

**Standard context:** Reference established patterns in the codebase.

```text
The rest of the codebase uses camelCase for function names. This file uses
snake_case, which introduces inconsistency. Let's align with the project
convention.
```

**External context:** Reference documentation, articles, or known best practices.

```text
PostgreSQL recommends using `LIKE` with a prefix pattern (e.g., `term%`) for
index usage. Using a leading wildcard `%term%` forces a full table scan. See:
https://www.postgresql.org/docs/current/indexes-types.html
```

### Linking to Style Guides and Best Practices

A code review comment that links to a style guide or documented best practice is more persuasive and less personal. It shifts the authority from your personal opinion to a team-agreed standard.

**Link to internal style guides:**

```text
Our Python style guide specifies 88 characters per line (PEP 8 compliant).
This line is 120 characters. Please wrap it.
```

**Link to external documentation:**

```text
The React documentation recommends lifting state up when multiple components
need to share the same data. In this case, both Header and Sidebar read from
`userPreferences`.
```

**Link to linter rules:**

```text
ESLint rule `no-unused-vars` flags this import. Either use it or remove it.
You can check by running `npm run lint`.
```

**Link to prior art:**

```text
There is a similar implementation in the `auth-service` module that handles
this pattern. Here is the link for reference.
```

**When linking is not possible,** describe the rule in your own words and mention that it should be documented.

### Code Review Etiquette

Review etiquette covers the social norms that make code review productive and respectful.

**Timeliness:**
- Review requests within 24 hours on business days
- Acknowledge receipt even if you cannot review immediately
- Do not leave reviews hanging for days

**Scope:**
- Review what was changed, not the entire file
- Do not request unrelated changes that belong in a separate PR
- Focus on the diff, not stylistic preferences outside the diff

**Preparation:**
- Read the PR description and linked tickets before commenting
- Understand the context and intent of the change
- Run the code if the change is complex

**Tone:**
- Assume good intent
- Use polite language
- Thank the author for good work

**Response:**
- Respond to all comments, even to say "Fixed" or "Addressed"
- If you disagree, explain your reasoning
- Do not take comments personally

**Escalation:**
- If you cannot reach consensus, involve a third party
- Do not block a PR indefinitely over a disagreement
- Document the disagreement and move forward

### Handling Disagreements

Disagreements in code review are a sign of a healthy team with strong opinions. The goal is not to eliminate disagreement but to resolve it constructively.

**Steps for resolution:**

**Acknowledge the disagreement.** Explicitly state that you see the difference in opinion.

```text
I see we have different perspectives on this error-handling strategy. Let me
share my reasoning more clearly.
```

**State your criteria.** What principles or constraints drive your position?

```text
My concern is operational: we have been bitten by silent failures in the past.
I prefer explicit error propagation so that incidents are caught by monitoring.
```

**Seek data.** Can you benchmark, test, or cite evidence?

```text
I benchmarked both approaches on a sample of 10,000 requests. The async
approach is 30% faster. Here are the numbers.
```

**Propose a compromise.** Can you split the difference or defer to a follow-up?

```text
What if we merge the current approach with a TODO to revisit after the
benchmarking sprint?
```

**Escalate when needed.** If the disagreement affects system architecture or team standards, bring it to the team or tech lead.

**Know when to yield.** Not every hill is worth dying on. If the issue is minor and the author feels strongly, let it go.

### The Anatomy of a Good Comment

A good code review comment has four parts:

1. **Location reference** — what code you are referring to
2. **Observation** — what you see
3. **Concern or rationale** — why it matters
4. **Suggestion** — what the author could do

```text
Line 42: checkUserStatus() is called inside a loop.

Each call makes a database query. This is an N+1 query problem that will cause
performance degradation as the user list grows.

Consider batch-loading all user statuses before the loop using a single query
with a WHERE IN clause.
```

**Short comments for obvious issues:**

```text
Typo: "recieve" -> "receive"
```

**Longer comments for complex topics:**

```text
The retry logic here does not use exponential backoff. If the downstream
service is degraded, retrying every second will compound the load. Our SLA
document recommends:

1. Initial retry after 1 second
2. Double the delay each retry (1s, 2s, 4s, 8s...)
3. Maximum of 5 retries
4. Jitter to avoid thundering herd

I can share the internal document on retry strategies if needed.
```

### Comment Anti-Patterns

**Drive-by commenting:** Leaving a comment and not engaging with the author's response. If you start a discussion, be available to continue it.

**Nitpicking everything:** Every comment should add value. If you leave fifty nits on a PR, the author will stop taking your feedback seriously.

**Bikeshedding:** Spending disproportionate time on trivial issues. Recognize when you are bikeshedding and stop.

**Rubber-stamping:** Approving without reviewing. This undermines the purpose of code review.

**Gatekeeping:** Blocking a PR for reasons unrelated to code quality, such as personal preference or process violations that could be fixed post-merge.

**Seagull reviewing:** Flying in, dropping comments, and leaving without engaging in discussion.

**Using code review as documentation:** Leaving multi-paragraph explanations that belong in a design doc or README.

**Reviewing with your ego:** Taking disagreement about code as a personal attack.

### Reviewing Different Types of Changes

**Bug fixes:** Focus on whether the fix correctly addresses the root cause, not just the symptom. Check for regression tests.

**Feature additions:** Evaluate design decisions, API contracts, test coverage, and integration points. Consider edge cases.

**Refactoring:** Verify that behavior is preserved. Check that tests still pass and that the new structure is an improvement.

**Configuration changes:** Check for typos, environment-specific values, and security implications (secrets, permissions).

**Documentation:** Review for clarity, accuracy, and completeness. Verify code examples.

**Dependency updates:** Check changelogs for breaking changes. Verify that tests pass. Be aware of security advisories.

## Study Cases

### Case 1: Poorly Written Comment Turned Constructive

```text
// Original comment from reviewer
Why are you doing this? This is completely wrong. You should know better.

// Revised comment
The approach on line 45 uses a synchronous HTTP call inside an async event
handler. This will block the event loop and degrade throughput. Let's discuss
alternatives like queuing the event or using an async HTTP client.
```

### Case 2: Handling a Subtle Performance Issue

An author submitted a PR that used string concatenation in a hot loop.

```text
// Reviewer comment (blocker)
The string concatenation in the render loop (line 33) creates a new string
object on every iteration. For a list of 10,000 items, this means 10,000
allocations. Using a StringBuilder reduces allocations to 1.

Would you like me to share the profiler output I took?

// Author response
Good catch. I was thinking about readability, but I see how this impacts
performance at scale. Will change to StringBuilder.
```

### Case 3: Resolving a Design Disagreement

An author used inheritance for a feature that the reviewer thought should use composition.

```text
// Initial exchange
Reviewer: Use composition instead of inheritance here.
Author: I think inheritance is cleaner for this use case.
Reviewer: It creates tight coupling.
Author: But the subclass only overrides one method.

// Resolution with third party
Tech lead: Both approaches work here. Given that the team wants to move toward
composition over inheritance, let's go with composition. The inheritance
approach is only marginally simpler and sets a pattern we want to avoid.

// Final comment
Reviewer: After discussing with the team, we agreed to use composition to stay
consistent with the project's architectural direction. I updated the PR
description to reflect this decision.
```

## Examples

### Example 1: Good Blocker Comment

```text
[Blocker] The SQL query on line 14 uses string interpolation with user input.

This creates a SQL injection vulnerability. An attacker could supply a
malicious value in the `username` field and execute arbitrary queries.

Use parameterized queries instead:
  db.query("SELECT * FROM users WHERE id = $1", [userId]);

See OWASP SQL Injection Prevention:
https://cheatsheetseries.owasp.org/cheatsheets/SQL_Injection_Prevention_Cheat_Sheet.html
```

### Example 2: Good Suggestion Comment

```text
[Suggestion] The validate() function returns a boolean, but the caller needs
to know which field failed validation.

Consider returning a ValidationResult object instead:
  {
    valid: false,
    field: "email",
    message: "Email format is invalid"
  }

The callers can then display specific error messages instead of a generic
"Validation failed" toast.
```

### Example 3: Good Question Comment

```text
[Question] The caching TTL is set to 3600 seconds.

Is this derived from any specific requirement or metric? Our analytics show
that user preferences change frequently. A shorter TTL like 300 might stale
less often while still reducing database load.
```

### Example 4: Good Praise Comment

```text
The test coverage on this PR is excellent. I particularly appreciate the edge
case tests for empty inputs and network timeouts. The parameterized test
pattern you used is worth replicating in other modules. Nice work.
```

### Example 5: Good Follow-Up Comment

```text
This fix resolves the immediate issue. However, the same pattern appears in
three other files. I created a follow-up ticket (TICKET-1234) to address them
systematically. I assigned it to this sprint's tech debt bucket.
```

### Example 6: Addressing a Missing Test

```text
The new rate-limiting logic is untested. Considering this is a security-critical
component, we should have:

- Unit tests for the rate calculation logic
- Integration tests for the middleware integration
- A load test that verifies the limit is enforced

I wrote a similar test suite for the auth module. I can share it as a reference.
```

## Glossary

| Term | Definition |
|------|------------|
| Blocker | A code review issue that must be resolved before merging |
| Nit | A minor style or preference issue that does not block merging |
| Bikeshedding | Spending disproportionate time on trivial issues |
| Drive-by comment | A comment left without engaging with the author's follow-up |
| Gatekeeping | Blocking a PR for reasons unrelated to code quality |
| Psychological safety | The belief that one can speak up without being punished or humiliated |
| Seagull review | Flying in, dropping comments, and leaving without discussion |
| N+1 query | A performance anti-pattern where a query is executed per item in a loop |
| Rubber-stamping | Approving a PR without thorough review |
| Threading | Using conversation threads in code review tools to keep discussions organized |

## Quick References

- [Google's Code Review Developer Guide](https://google.github.io/eng-practices/review/) — comprehensive guide from Google
- [SmartBear Code Review Best Practices](https://smartbear.com/learn/code-review/best-practices-for-peer-code-review/) — research-backed recommendations
- [How to Do Code Reviews Like a Human](https://mtlynch.io/human-code-reviews-1/) — blog post on the human side of code review
- [Conventional Comments](https://conventionalcomments.org/) — a standard for labeling review comments
- [Code Review Etiquette (GitHub)](https://github.com/blog/2256-code-review-etiquette)

## Next Steps

- [Incident Postmortems & Incident Reports](incident-reports.md) — writing clearly under pressure during incidents
- [RFCs & Technical Decision Documents](rfcs.md) — writing proposals that get buy-in from the team
