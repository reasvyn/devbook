# Technical English for Developers

## Description

English is the lingua franca of software development. Code comments, documentation, commit messages, RFCs, pull requests, and conference talks all rely on clear technical English. This document covers the fundamentals of writing well as a developer.

## Prerequisites

None.

## Table of Contents

- [Why English Matters in Tech](#why-english-matters-in-tech)
- [Principles of Technical Writing](#principles-of-technical-writing)
- [Common Pitfalls](#common-pitfalls)
- [Reading Code vs. Reading Prose](#reading-code-vs-reading-prose)

## Content / Material

### Why English Matters in Tech

- Open-source collaboration happens in English.
- API docs, RFCs, and standards are written in English.
- Code comments communicate intent to your future self and teammates.
- Clear writing == clear thinking.

### Principles of Technical Writing

| Principle | Explanation |
|-----------|-------------|
| **Be concise** | Every word must earn its place. Omit needless words. |
| **Be precise** | Avoid vague terms like "thing" or "stuff". Name concepts explicitly. |
| **Use active voice** | "The function returns a list" not "A list is returned by the function". |
| **Structure visually** | Use lists, tables, and code blocks to break dense text. |
| **Define terms** | First use of a term should include a brief definition. |

### Common Pitfalls

- **Jargon without explanation** — not every reader knows your acronyms.
- **Over-explaining** — trust your reader's intelligence.
- **Inconsistent terminology** — pick one term and stick with it.
- **Ignoring the audience** — a junior dev needs different context than a senior.

### Reading Code vs. Reading Prose

Code and prose serve different purposes:

```
Code:  if (user.isActive) { grantAccess(); }
Prose: "Active users are granted access automatically."
```

- Code is **logic** — it must be exact and executable.
- Prose is **context** — it explains why, when, and under what constraints.

## Glossary

| Term | Definition |
|------|------------|
| Active voice | A sentence structure where the subject performs the action |
| RFC | Request for Comments — a formal document for defining standards |
| Technical writing | Writing that conveys technical information clearly and efficiently |

## Next Steps

- [Writing Effective Documentation](../beginner/writing-docs.md) — practical guide for READMEs and API docs
