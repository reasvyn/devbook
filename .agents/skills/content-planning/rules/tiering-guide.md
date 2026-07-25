# Content Planning — Tiering Guide

This file provides guidance on when and how to apply the 3-level content tiering system. The full tiering rules are in [CONTENT-RULES.md](../../../../CONTENT-RULES.md).

## When to Tier

A topic requires tiering when:

1. **The content exceeds 800 lines** at a single complexity level, cannot be reasonably trimmed, and the excess content is genuinely complex enough to warrant a separate document.
2. **The topic naturally spans multiple audience levels** — from complete beginners to experienced practitioners.
3. **The prerequisite chain is steep** — understanding the advanced material requires conceptual groundwork that would bloat a single document.

A topic does **not** require tiering when:

1. It fits within 400–800 lines at a single level.
2. The audience is uniformly one skill level (e.g., all beginners).
3. The topic is self-contained and does not have deep prerequisite chains.

## How to Determine Tier Boundaries

### Basic Tier
- **Audience:** A developer encountering this topic for the first time.
- **Content:** What is it? Why does it exist? Core definitions. Simple examples. Mental models.
- **Scope:** The minimum viable understanding. A reader should be able to use the basic concepts in a simple context.
- **Prerequisites:** Only general prerequisites (e.g., basic programming literacy).

### Intermediate Tier
- **Audience:** A developer who has read the basic tier or has equivalent prior knowledge.
- **Content:** How is it used in practice? Real-world patterns. Trade-offs. Common pitfalls. Integration with other tools/concepts.
- **Scope:** Practical competence. A reader should be able to work with this topic in a real project.
- **Prerequisites:** The basic tier file, plus any other prerequisites.

### Advanced Tier
- **Audience:** A developer with working knowledge of the topic.
- **Content:** Architecture decisions. Edge cases. Performance optimization. Deep integration. Historical context. Research frontiers.
- **Scope:** Mastery and judgment. A reader should be able to make design decisions and teach others.
- **Prerequisites:** The intermediate tier file, plus any other prerequisites.

## How Many Tiers?

Use as many tiers as the topic demands — one, two, or three — but never more than three.

- **One tier (no split):** The topic fits within 400–800 lines at a single complexity level. Most topics fall here.
- **Two tiers:** The topic has a clear beginner vs. advanced distinction but does not need a middle tier. Example: a topic where basic concepts are simple but advanced applications are complex, with no meaningful intermediate ground.
- **Three tiers:** The topic has three distinct audience levels with different prerequisites and depth requirements. This is the maximum.

The decision is driven by content, not by a rule that every topic must be split. If you are unsure, start with one tier. Split later if the document exceeds 800 lines or the audience levels diverge significantly.

## Index Organization

Tiered content should be organized in the module index under descriptive phase headings that reflect the progression. Use headings that match your module's learning path.

**Single topic, tiered across phases** (illustrative):

```markdown
## 1. Introduction

- [Module intro](intro/your-topic.md)

## 2. [Descriptive Phase Name]

1. [Topic — Basic](topic-basic.md)

## 3. [Descriptive Phase Name]

1. [Topic — Intermediate](topic-intermediate.md)

## 4. [Descriptive Phase Name]

1. [Topic — Advanced](topic-advanced.md)
```

**Multiple topics, organized by tier** (illustrative):

```markdown
## 1. Introduction

- [Module intro](intro/your-topic.md)

## 2. [Descriptive Phase Name]

1. [Topic A — Basic](topic-a-basic.md)
2. [Topic B — Basic](topic-b-basic.md)

## 3. [Descriptive Phase Name]

1. [Topic A — Intermediate](topic-a-intermediate.md)
2. [Topic B — Intermediate](topic-b-intermediate.md)

## 4. [Descriptive Phase Name]

1. [Topic A — Advanced](topic-a-advanced.md)
2. [Topic B — Advanced](topic-b-advanced.md)
```

Choose the organization that best serves the learning path. Use topic-first when topics have strong interdependencies. Use tier-first when topics are more independent.

## Cross-Tier Linking

Each tier file should include:

- **In Prerequisites:** Link to the tier below (for intermediate and advanced tiers).
- **In Next Steps:** Link to the tier above (for basic and intermediate tiers) and to related topics.
- **In the Content section:** Explicitly state what knowledge is assumed from the prerequisite tier.

The tier files form a chain: basic → intermediate → advanced. Each tier's Prerequisites link downward, and its Next Steps link upward. Within each file, the content should be standalone — readable without the other tiers, but enriched by them.

Example chain (illustrative — use your actual file names):

```
topic-basic.md
  Prerequisites: [General prerequisite]
  Next Steps: [topic-intermediate.md]

topic-intermediate.md
  Prerequisites: [topic-basic.md], [Additional prerequisite]
  Next Steps: [topic-advanced.md]

topic-advanced.md
  Prerequisites: [topic-intermediate.md], [Additional prerequisite]
  Next Steps: [Related advanced topics]
```
