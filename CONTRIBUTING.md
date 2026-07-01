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

Index files are valid prerequisite targets when the reader must understand the entire subject, module, or submodule. Example: a document about distributed consensus might list [Networks](networks/index.md) because it assumes mastery of all networking concepts.

## Table of Contents

- [Section 1](#section-1)
- [Section 2](#section-2)

## Content / Material

The core material — explanations, diagrams, code snippets, and everything in between. Integrate real-world scenarios, examples, and walkthroughs directly into this section rather than separating them.

## Learning Tips (Optional)

Practical advice for studying and retaining the material. Common pitfalls to avoid, memory aids, practice strategies, or ways to apply the concepts.

## Glossary

| Term | Definition |
|------|------------|
| Term A | Definition of term A |
| Term B | Definition of term B |

## Quick References (Optional)

External resources — journals, books, blog posts, websites. Items must be verified — every link must be valid and accessible before inclusion.

- [Title](URL) — one-line description
- [Title](URL)

## Next Steps

Where to go from here — related topics, recommended reading order, or practice exercises.
```

> **Note:** All documents must be written in **English**. Sections marked "(Optional)" may be omitted if genuinely not needed. Quick References items must be verified — every link must be valid and accessible before inclusion.

## Directory Structure Convention

Every file and directory **must** follow this pattern:

```
{subject}/{intro(optional),module}/{submodule(optional),intro(optional)}/{short-description}.md
```

A file can live at any valid combination:
- `subject/intro/short-description.md` — subject-level intro
- `subject/module/short-description.md` — content under a module
- `subject/module/intro/short-description.md` — module-level intro
- `subject/module/submodule/short-description.md` — content under a submodule

### Rules

- **Subject** — top-level directory (e.g., `mathematics`, `networks`).
- **Module** — grouping within a subject (e.g., `linear-algebra` inside `mathematics`).
- **Submodule** — optional grouping within a module (e.g., `vector-spaces` inside `linear-algebra`). A submodule cannot contain deeper submodules. Module and submodule names must not be the same.
- **`intro/`** — a special directory for background, philosophy, principles, history, ethics, or official organizations about the field. Every subject and module **must** have an `intro/` directory.
- **Short description** is a hyphenated slug, lowercase (e.g., `why-math.md`, `vector-operations.md`).
- Content files sit directly under the module or submodule (flat). `intro/` is the only directory allowed at these levels.
- **Must be a real branch of knowledge.** Subjects, modules, and submodules must represent established fields of study or practice. Do not create entities for job roles, positions, or personas — role-based profession explorations belong in `careers/`.
- **Line count 400–800.** Every content file must be at least 400 lines. If shorter, expand with more depth, examples, or diagrams directly in the Content section. If longer than 800 lines, split into multiple focused documents.

### Careers

The `careers/` directory follows the same subject/module convention as other top-level directories. Each career is a module that explores a profession in depth — what the role entails, its responsibilities, specializations, career progression, and industry context.

- Structure: `{career}/{module}/{short-description}.md`
- Each career module has an `index.md`, an `intro/` with background content, and content files covering different aspects of the profession.
- Content files follow the 9-section mandatory format.
- Career modules are about the *profession itself*, not learning paths for individuals.

### Index System

Every directory **must** have an `index.md` that lists and links to its children.

```
mathematics/           ← subject
└── linear-algebra/    ← module
    └── vectors-and-matrices/  ← submodule
        └── index.md   ← lists actual .md files
```

Rules for index files:
- **Every directory must be listed.** If a directory has content files, its `index.md` must link to them.
- **Do not list directories** — list actual `.md` files.
- **Use relative paths** only. Never absolute or full URLs for internal links.
- **Keep the list ordered** by recommended reading order.

The master `index.md` at the repository root references every subject:

```markdown
# DevBook

Markdown-based learning resources for developers. Explore careers, then dive into any subject.

- [Careers](careers/index.md)
- [Computer Science](computer-science/index.md)
- [Programming](programming/index.md)
- [Mathematics](mathematics/index.md)
- [Networks](networks/index.md)
- [Systems Design](systems-design/index.md)
- [Security](security/index.md)
- [Data & Databases](data-databases/index.md)
- [AI / Machine Learning](ai-ml/index.md)
- [Cloud & DevOps](cloud-devops/index.md)
- [Software Engineering](software/index.md)
- [UI/UX Design](ui-ux/index.md)
- [Physics](physics/index.md)
- [Hardware](hardware/index.md)
- [Governance](governance/index.md)
- [English](english/index.md)
- [Business & Entrepreneurship](business/index.md)
- [Social & Psychology](social/index.md)
```

A module `index.md` references its children:

```markdown
# Linear Algebra

Vectors, matrices, and their applications in computing.

- [Vectors & Matrices](vectors-and-matrices/index.md)
- [Matrix Operations](matrix-operations/index.md)
```

A submodule `index.md` references actual content files:

```markdown
# Vectors & Matrices

Representing data and transformations with arrays of numbers.

- [Introduction](intro/vectors-and-matrices.md)
- [Vector Operations](vector-operations.md)
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
