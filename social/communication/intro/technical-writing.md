# Technical Writing

## Description

Technical writing is the practice of explaining complex information clearly and concisely. For developers, it's the skill behind documentation, RFCs, code comments, commit messages, and every piece of text that helps others understand what you've built.

## Prerequisites

- [Why Communication?](why-communication.md)

## Table of Contents

- [What Makes Technical Writing Different?](#what-makes-technical-writing-different)
- [Writing Documentation](#writing-documentation)
- [Writing RFCs](#writing-rfcs)
- [Writing Code Comments](#writing-code-comments)
- [Writing Commit Messages](#writing-commit-messages)

## Content / Material

### What Makes Technical Writing Different?

Technical writing is not creative writing. The goal is not to entertain but to **transfer understanding** as efficiently as possible.

**Characteristics of good technical writing:**

| Characteristic | Meaning |
|----------------|---------|
| **Concise** | Every word earns its place |
| **Precise** | Specific over vague — "503 errors" not "problems" |
| **Structured** | Headings, lists, tables — make it scannable |
| **Audience-aware** | Written for a specific reader's knowledge level |
| **Actionable** | Tells the reader what to do or what they need to know |

### Writing Documentation

**Types of developer documentation (Diátaxis framework):**

| Type | Focus | Example |
|------|-------|---------|
| **Tutorials** | Learning-oriented, step-by-step | "Getting started with React" |
| **How-to guides** | Goal-oriented, practical | "Deploy a Node.js app to Fly.io" |
| **Reference** | Information-oriented, precise | "API reference for Stripe SDK" |
| **Explanation** | Understanding-oriented, conceptual | "How TCP handshakes work" |

**Documentation principles:**

- Start with a one-sentence summary of what the doc covers
- Show, don't just tell — include code snippets
- Write examples that work (test them)
- Keep it up to date — outdated docs are worse than no docs
- Use consistent terminology throughout

### Writing RFCs

Request for Comments (RFCs) are design documents used to propose and align on technical decisions.

**RFC structure:**

```
Title: Short, descriptive
Status: Draft → Review → Accepted/Rejected
Author: Who wrote it

1. Problem statement — what problem are we solving?
2. Proposed solution — what's the approach?
3. Alternatives considered — what else did we look at?
4. Trade-offs — pros and cons of the proposal
5. Implementation plan — how will it be built?
6. Open questions — what's not yet decided
```

**Effective RFC tips:**

- Write the problem statement before the solution — the right problem makes the solution obvious
- List alternatives explicitly — shows you've done your homework
- Include concrete examples — abstract designs hide ambiguity
- Keep it short — if it's longer than a few pages, it's too long

### Writing Code Comments

**Good comments explain why, not what.** The code itself should make the "what" clear.

| Type | Example |
|------|---------|
| **Why** | `// Using a hash join because the probe table fits in memory` |
| **What** | `// Loop through all users` (redundant — the code shows this) |
| **Warning** | `// This regex breaks on Unicode — fix before launch` |
| **Reference** | `// See RFC 2616 section 14 for cache-control semantics` |

**Self-documenting code is better than comments.** Good variable names, small functions, and clear structure make most comments unnecessary. Save comments for the non-obvious.

### Writing Commit Messages

A good commit message is a communication to your future self and your teammates.

**Structure:**

```
<type>: <short summary> (max 50 chars)

<optional body — explain what and why, not how>
```

**Conventional commits:**

| Type | When |
|------|------|
| `feat` | New feature |
| `fix` | Bug fix |
| `refactor` | Code change that doesn't add feature or fix bug |
| `docs` | Documentation only |
| `test` | Adding or fixing tests |
| `chore` | Maintenance, tooling, config |

**Bad:** `fixed stuff`
**Good:** `fix(auth): prevent race condition on token refresh`

## Glossary

| Term | Definition |
|------|------------|
| RFC | Request for Comments — a design document for proposing technical decisions |
| Diátaxis | A framework for classifying documentation into tutorials, guides, reference, and explanation |
| Conventional commits | A specification for standardized commit message formats |

## Next Steps

- [Why Psychology?](../../psychology/intro/why-psychology.md) — understanding how readers process information
- [Why Human Relations?](../../human-relations/intro/why-human-relations.md) — communication in relationships and feedback
