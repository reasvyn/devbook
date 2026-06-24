# Contributing to DevBook

Thanks for your interest in contributing! DevBook is a community-driven resource, and every improvement — whether it's fixing a typo, adding a new topic, or refining an existing explanation — is welcome.

## Table of Contents

- [Types of Contributions](#types-of-contributions)
- [Getting Started](#getting-started)
- [Writing Guidelines](#writing-guidelines)
- [Document Format](#document-format)
- [Style Guide](#style-guide)
- [Pull Request Process](#pull-request-process)
- [Code of Conduct](#code-of-conduct)

## Types of Contributions

| Type | Description |
|------|-------------|
| **Fix** | Correct an error, improve clarity, or update outdated information |
| **Expand** | Add missing subtopics, examples, or diagrams to an existing document |
| **New topic** | Propose and write a new subject area under an existing category |
| **New category** | Suggest a new top-level directory (e.g., Blockchain, DevOps) |
| **Review** | Read existing content and open issues with suggestions |

## Getting Started

1. Fork the repository.
2. Create a branch: `git checkout -b fix/quick-description` or `topic/your-topic`.
3. Commit your changes with a clear message.
4. Push and open a pull request.

## Writing Guidelines

- **Be concise.** Developers have limited time — get to the point.
- **Prefer code over prose.** A short snippet is worth a paragraph of explanation.
- **Explain *why*.** Don't just state facts; connect them to real-world development.
- **Assume a technical audience.** Basic programming literacy is expected; no need to explain what a variable is.
- **Minimize prerequisites.** If a concept depends on prior knowledge, link to it rather than re-explaining.
- **Skip fluff.** No "in this article we will learn" — just teach.

## Document Format (Mandatory)

Every document **must** follow this structure, in order:

```markdown
# Title

## Description

Brief overview — what this covers and why a developer should care.

## Prerequisites

- Concept A — link to related doc
- Concept B — link to related doc

## Table of Contents

- [Section 1](#section-1)
- [Section 2](#section-2)

## Content / Material

The core material — explanations, diagrams, code snippets, and everything in between. This is the main body of the document.

## Study Cases (Optional)

Real-world scenarios or walkthroughs that apply the concepts covered.

## Examples (Optional)

Additional practical examples beyond what's in the Content section.

## Glossary

| Term | Definition |
|------|------------|
| Term A | Definition of term A |
| Term B | Definition of term B |

## Next Steps

Where to go from here — related topics, recommended reading order, or practice exercises.
```

> **Note:** All documents must be written in **English**.

## Directory Structure Convention

Every file and directory **must** follow this pattern:

```
{module-name}/{submodule-name}/{level:intro|beginner|intermediate|advanced}/{short-description}.md
```

### Rules

- **Level** must be one of:
  - `intro` — pure concepts, principles, and philosophy of the field itself. Explains *why* the field exists, its core mindset, and its foundational axioms. **Not** for specific sub-topics (those go to `beginner`).
  - `beginner` — specific topics within the field, explained from the ground up.
  - `intermediate` — deeper dives, practical application, common patterns.
  - `advanced` — specialized, cutting-edge, or research-level content.
- **Short description** is a hyphenated slug (e.g., `why-math.md`, `matrix-operations.md`).
- Content files go only in level directories — never directly under a module or submodule.

### Roadmaps

The `roadmaps/` directory is a special directory that does **not** follow the module/submodule/level convention. It contains role-based learning paths organized by career level.

- Structure: `{level}/{role}.md` where `level` is one of `junior`, `mid`, `senior`, `specialist`.
- Each level has an `index.md` listing all roles at that level.
- Files use a custom format with priority labels: `[must-know]`, `[good-to-know]`, `[nice-to-have]`.
- Roadmaps reference existing DevBook files where possible.

### Index System

Every module and submodule **must** have an `index.md` that references its children.

```
mathematics/index.md                → links to submodule indexes
mathematics/linear-algebra/index.md → links to level indexes
```

An `index.md` uses relative links:

```markdown
# Mathematics

- [Why Math?](intro/why-math.md)
- [Set Theory](beginner/set-theory.md)
- [Linear Algebra](linear-algebra/index.md)
```

The master `index.md` at the repository root references every top-level module:

```markdown
# DevBook

- [Mathematics](mathematics/index.md)
- [Physics](physics/index.md)
- [English](english/index.md)
- [Computer Science](computer-science/index.md)
- [Networks](networks/index.md)
- [Programming](programming/index.md)
- [Systems Design](systems-design/index.md)
- [Security](security/index.md)
- [AI / ML](ai-ml/index.md)
```

## Style Guide

- **Headings** — ATX-style (`##`, not underlines).
- **Code** — Fenced with language identifiers. Use `text` or `plain` for non-code blocks.
- **Math** — LaTeX inline with `$` and display with `$$`.
- **Diagrams** — Mermaid syntax where helpful.
- **Links** — Relative links for internal documents, full URLs for external resources.
- **Lists** — Use `-` for unordered, `1.` for ordered.
- **Line breaks** — Hard wrap at ~100 characters for readable diffs.
- **Tone** — Direct, instructive, jargon-aware. Define terms on first use.

## Pull Request Process

1. Ensure your branch is up to date with the main branch.
2. Verify that any new files follow the directory naming convention (lowercase, hyphen-separated).
3. Open a pull request with a descriptive title and summary of changes.
4. Maintainers will review and may request edits before merging.
5. Squash commits on merge to keep history clean.

## Code of Conduct

Be respectful, constructive, and patient. This is a learning resource first — every contribution should make knowledge more accessible, not less.
