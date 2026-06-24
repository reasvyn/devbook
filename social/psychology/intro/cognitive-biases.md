# Cognitive Biases

## Description

Cognitive biases are systematic patterns of deviation from rational judgment — the brain's shortcuts that lead to predictable errors. Developers who understand biases make better technical decisions, design better products, and collaborate more effectively.

## Prerequisites

- [Why Psychology?](why-psychology.md)

## Table of Contents

- [What Are Cognitive Biases?](#what-are-cognitive-biases)
- [Biases in Software Development](#biases-in-software-development)
- [Countering Bias](#countering-bias)

## Content / Material

### What Are Cognitive Biases?

The brain processes enormous amounts of information using mental shortcuts (heuristics). These shortcuts work most of the time, but they also produce systematic errors — biases. Recognizing these patterns is the first step to countering them.

**Why biases exist:** The brain conserves energy by automating decisions. This was useful on the savanna but is often harmful when building software systems.

### Biases in Software Development

| Bias | What It Is | Dev Impact |
|------|------------|------------|
| **Confirmation bias** | Seeking evidence that supports existing beliefs | Ignoring signs that your architecture is wrong |
| **Dunning-Kruger effect** | Overestimating ability in unfamiliar domains | Junior devs think they're ready for microservices |
| **Sunk cost fallacy** | Continuing because you've already invested | Sticking with a bad framework because you've used it for months |
| **Planning fallacy** | Underestimating time and cost | Every sprint goes over because estimates ignore unknowns |
| **Availability bias** | Overweighting recent, dramatic events | Rewriting everything after one production incident |
| **Anchoring** | Relying on the first piece of information | First estimate given in a meeting anchors all discussion |
| **Survivorship bias** | Focusing on successes, ignoring failures | Studying unicorn startups while ignoring the 90% that fail |
| **Not-invented-here** | Distrusting external solutions | Building your own auth system instead of using a library |
| **Status quo bias** | Preferring the current state of affairs | Staying with a language you know rather than learning a better one |
| **Hindsight bias** | Seeing past events as predictable | "I knew that microservice would fail" — after it failed |

### Countering Bias

Bias cannot be eliminated — the brain is wired for shortcuts. But it can be mitigated.

**Techniques for developers:**

- **Pre-mortems** — assume the project failed; work backward to find why
- **External estimates** — compare your estimates to industry benchmarks
- **Red team** — assign someone to argue against the decision
- **Data over intuition** — measure before acting, especially for performance and UX decisions
- **Diverse perspectives** — different backgrounds produce different cognitive patterns
- **Written RFCs** — writing forces structured thinking and makes assumptions visible
- **Retrospectives** — blameless analysis of what actually happened vs what was expected

## Glossary

| Term | Definition |
|------|------------|
| Cognitive bias | A systematic pattern of deviation from rational judgment |
| Heuristic | A mental shortcut for quick decision-making |
| Confirmation bias | Tendency to favor information that confirms existing beliefs |
| Sunk cost fallacy | Continuing a course of action because of past investment |
| Planning fallacy | Underestimating time, costs, and risks of future actions |
| Survivorship bias | Focusing on surviving examples while ignoring failures |

## Next Steps

- [Why Human Relations?](../../human-relations/intro/why-human-relations.md) — biases affect how we interact with teammates
- [Why Communication?](../../communication/intro/why-communication.md) — biases distort how we communicate and interpret messages
