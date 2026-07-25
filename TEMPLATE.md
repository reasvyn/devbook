# Document Template

This file defines the mandatory structure for every content `.md` file in DevBook. Copy this template when creating new documents. All 9 sections are required unless marked optional.

---

## Template

```markdown
# {Title}

## Description

{Brief overview — what this covers and why a developer should care. 1–3 sentences. Write this as if answering the question: "Why should I keep reading?"}

## Prerequisites

- [{Concept A}]({relative/path.md}) — what the reader should know first
- [{Concept B}]({relative/path.md})

{Index files are valid prerequisite targets when the reader must understand the entire subject, module, or submodule. Example: a document about distributed consensus might list [Networks](networks/index.md) because it assumes mastery of all networking concepts.}

## Table of Contents

- [{Section 1}](#section-1)
- [{Section 2}](#section-2)

## Content / Material

{The core material. Use headings, paragraphs, code blocks, diagrams (Mermaid), and math (LaTeX `$$`) as needed. Be concise but complete. Integrate real-world scenarios, examples, and walkthroughs directly into this section rather than separating them.}

## Learning Tips

{Practical advice for studying and retaining the material. Common pitfalls to avoid, memory aids, practice strategies, or ways to apply the concepts. This section is optional but strongly recommended.}

## Glossary

| Term | Definition |
|------|------------|
| {Term} | {One-line definition} |

## Quick References

{External resources — journals, books, blog posts, websites. Every link must be verified and accessible before inclusion. This section is optional but recommended when authoritative external references exist.}

- [{Title}]({URL}) — one-line description
- [{Title}]({URL})

## Next Steps

{Where to go next. Link to related documents or suggest practice exercises. This section is mandatory — every document must point the reader forward.}

- [{Related Topic A}]({relative/path.md})
- [{Related Topic B}]({relative/path.md})
```

## Section Rules

| # | Section | Required | Purpose |
|---|---------|----------|---------|
| 1 | **Title** | Yes | Document title — must match the file's topic, not generic |
| 2 | **Description** | Yes | Brief overview — what this covers and why a developer should care |
| 3 | **Prerequisites** | Yes | What the reader should know first, with links to related documents |
| 4 | **Table of Contents** | Yes | Section navigation with anchor links |
| 5 | **Content / Material** | Yes | Core material — explanations, code, diagrams, examples, walkthroughs |
| 6 | **Learning Tips** | Optional | Study advice, memory aids, practice strategies, common pitfalls |
| 7 | **Glossary** | Yes | Key terms and definitions introduced in the document |
| 8 | **Quick References** | Optional | Verified external links to journals, books, blog posts, websites |
| 9 | **Next Steps** | Yes | Where to go from here — related documents, practice exercises |

> Do not skip, reorder, or rename sections. Sections marked "Optional" may be omitted if genuinely not needed. Quick References items must be verified — every link must be valid and accessible before inclusion.

## File Naming Convention

Content files use hyphenated slugs, lowercase:

```
{topic}-basic.md
{topic}-intermediate.md
{topic}-advanced.md
```

## Tiered Content

Topics that span multiple complexity levels must be split into at most three tiers. Use one, two, or three tiers as the topic demands — but never more than three.

| Tier | File suffix | Audience | Depth |
|------|-------------|----------|-------|
| **Basic** | `-basic.md` | Beginners with no prior knowledge of this specific topic | Definitions, core concepts, simple examples |
| **Intermediate** | `-intermediate.md` | Developers with basic understanding | Applied techniques, real-world patterns, trade-offs |
| **Advanced** | `-advanced.md` | Developers with working knowledge | Architecture decisions, edge cases, optimization, integration |

Example files for a topic on arithmetic:

```
arithmetic-basic.md          ← what is addition, subtraction, order of operations
arithmetic-intermediate.md   ← fractions, decimals, percentages, ratios
arithmetic-advanced.md       ← combinatorics, number theory applications in CS
```

### Tier Rules

- **Maximum three tiers.** Use as many as needed — one, two, or three — but never more than three. A topic that fits in a single 400–800 line file does not need tiering.
- **Each tier is a standalone document.** A reader should be able to read only the tier they need without depending on other tiers.
- **Prerequisites chain upward.** The intermediate file lists the basic file as a prerequisite. The advanced file lists the intermediate file.
- **Not all topics require tiering.** Simple topics that fit within 400–800 lines at a single level do not need to be split. Tiering is required only when a topic naturally spans multiple complexity levels.
- **Tier suffix goes in the filename, not the title.** The `# Title` inside the file should read `# Arithmetic`, not `# Arithmetic (Basic)`. The tier is indicated by the filename and by context in the module index.

## Index Integration

Every content file must appear in its parent index. Module indexes organize tiered content under descriptive phase headings:

```markdown
# Mathematics

## 1. Introduction

- [Why Mathematics](intro/why-mathematics.md)

## 2. Arithmetic Foundations

1. [Arithmetic — Basic](arithmetic-basic.md)
2. [Arithmetic — Intermediate](arithmetic-intermediate.md)

## 3. Advanced Mathematical Reasoning

1. [Arithmetic — Advanced](arithmetic-advanced.md)
```
