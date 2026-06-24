# Reading and Thinking Critically

## Description

Reading technical content is not passive consumption — it's an active, critical process. Developers constantly evaluate documentation, blog posts, RFCs, and code reviews. This document covers the principles of critical reading and thinking: questioning assumptions, evaluating sources, and forming reasoned judgments.

## Prerequisites

- [Technical English for Developers](technical-english.md)

## Table of Contents

- [Active Reading](#active-reading)
- [Question Everything](#question-everything)
- [Evaluating Sources](#evaluating-sources)
- [Arguments and Evidence](#arguments-and-evidence)

## Content / Material

### Active Reading

Passive reading: scanning words hoping they stick.
Active reading: engaging with the text.

- **Skim first** — read headings, summaries, and code blocks to get the map.
- **Ask questions** — before each section, ask "what will this say?" When done, ask "what did I learn?"
- **Take notes** — paraphrase in your own words. If you can't explain it simply, you haven't understood it.

### Question Everything

Critical thinking starts with questions:

- What problem is being solved?
- What assumptions does this rely on?
- What evidence supports the claims?
- Are there alternative approaches?
- What is not being said?

In code reviews, these questions become:

- Does this change handle edge cases?
- Are there security implications?
- Is this the right abstraction? Too much? Too little?

### Evaluating Sources

Not all technical content is equal. Consider:

| Factor | What to Ask |
|--------|-------------|
| **Authority** | Who wrote this? Are they credible? |
| **Currency** | When was this published? Is it still accurate? |
| **Bias** | Does the author have a vested interest (e.g., vendor docs)? |
| **Depth** | Does it cover trade-offs or just one side? |
| **Community signal** | Comments, stars, citations, adopters |

### Arguments and Evidence

In a good technical argument, claims are supported by evidence:

- **Claim**: "PostgreSQL is faster than MySQL for this workload."
- **Evidence**: benchmark results, query plans, real-world case studies.

Watch out for:
- **Anecdote as evidence** — "We switched and it was faster" without data.
- **Appeal to authority** — "Google uses it" (irrelevant to your use case).
- **False dichotomy** — "Either monolith or microservices, nothing in between."

## Glossary

| Term | Definition |
|------|------------|
| Critical reading | Engaging with a text to evaluate its claims and reasoning |
| False dichotomy | Presenting only two options when more exist |
| Appeal to authority | Using an authority figure's opinion as evidence without supporting reasoning |

## Next Steps

- [Writing Effective Documentation](../beginner/writing-docs.md) — practical guide for READMEs and API docs
