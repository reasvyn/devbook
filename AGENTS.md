# AGENTS.md — AI Agent Guide for DevBook

This file is the single source of truth for AI agents managing content in DevBook. Read it before any task execution.

## Project Identity

DevBook is a Markdown-based learning library for developers. All content is in plain `.md` files — no build tools, no frameworks. Every document must be practical, concise, and developer-focused.

## Core Constraints

| Constraint | Rule |
|---|---|
| **Language** | All content must be written in **English**. |
| **Format** | Every document must follow the 9-section mandatory format (see below). |
| **Structure** | `{subject}/{module}/{submodule(optional),intro}/{short-description}.md` |
| **Indexes** | Every module, submodule, and `intro/` must have an `index.md`. The root also has an `index.md`. |
| **No orphans** | A content file must be referenced by its parent `index.md`. |
| **No build step** | Content is plain Markdown. Do not introduce tooling, bundlers, or generators. |

## Directory Structure Rules

```
{subject}/
├── index.md
├── intro/                         ← subject-level intro (required)
│   ├── index.md                   ← lists all intro files
│   └── {short-description}.md
└── {module}/
    ├── index.md
    ├── intro/                     ← module-level intro (required)
    │   ├── index.md               ← lists all intro files
    │   └── {short-description}.md
    ├── {short-description}.md     ← content files (flat)
    └── {submodule}/               ← optional
        ├── index.md
        └── {short-description}.md
```

> The more modular your material, the better. Split large topics into smaller, focused files — each covering one coherent concept.

- **Subject** — top-level directory (e.g., `mathematics`, `networks`). Must match the Topics table in README.md.
- **Module** — grouping within a subject (e.g., `linear-algebra` inside `mathematics`). Every subject must consist of one or more modules.
- **Submodule** — optional grouping within a module (e.g., `vector-spaces` inside `linear-algebra`). A submodule cannot contain deeper submodules. Module and submodule names must not be the same.
- **Must be a real branch of knowledge.** Subjects, modules, and submodules must represent established fields of study or practice (e.g., mathematics, linear algebra, vector spaces). Do not create entities for job roles, positions, or personas (e.g., `ceo-founders`, `frontend-developers`). Role-based content belongs in `roadmaps/`, not in content modules.
- **`intro/`** — a special directory containing background, philosophy, principles, history, ethics, key events, or official organizations about the field. Every subject and module **must** have an `intro/` directory. Every `intro/` must have an `index.md` listing its files.
- **Short description** — hyphenated slug, lowercase (e.g., `why-math.md`, `vector-operations.md`).
- Content files sit directly under the module or submodule (flat). `intro/` is the only directory allowed at these levels.

## Roadmaps Directory

The `roadmaps/` directory is a special directory that does **not** follow the subject/module/submodule convention. It contains role-based learning paths organized by career level.

### Rules

- Structure: `{level}/{role}.md` where `level` is one of `junior`, `mid`, `senior`, `manager`, `specialist`, `executive`.
- Each level has an `index.md` listing all roles available at that level.
- Each role file is a learning path using priority labels: `🔴 CRITICAL`, `🟠 HIGH`, `🟡 MEDIUM`, `🟢 LOW`.
- `roadmaps/index.md` lists all levels (not individual roles).
- They are guides, not content modules — they reference existing DevBook files where possible.
- The 9-section mandatory format does **not** apply to roadmap files.
- **Index references allowed.** Roadmaps may link to `index.md` files (subject, module, or submodule) to imply the reader should cover all content under that index. For example, a backend developer roadmap might list `[Networks](networks/index.md)` as a CRITICAL prerequisite, meaning every document under `networks/` must be mastered. This avoids listing every file individually when a whole module is required.

### Level reference

| Level | Experience | Focus |
|-------|-----------|-------|
| `junior/` | 0–2 years | Fundamentals, tooling basics, completing defined tasks with guidance |
| `mid/` | 2–5 years | Independence, testing, design patterns, delivering features end-to-end |
| `senior/` | 5+ years | Architecture, system design, mentoring, cross-team influence |
| `manager/` | varies | Leading teams, product, and processes — EM, PM, People Ops, TPM |
| `specialist/` | varies | Deep expertise (architect, SRE, security, data engineer) — requires senior-level experience in a domain |
| `executive/` | varies | C-level, founders, investors, board members — requires extensive leadership experience |

## Index System

Every `index.md` is a navigation hub. It must contain:

1. A heading matching its position (e.g., `# Mathematics`, `# Linear Algebra`).
2. A brief description of what the module/submodule covers.
3. An unordered list of relative links to all children.

### Root `/index.md`

```markdown
# DevBook

Markdown-based learning resources for developers.

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

### Subject `/mathematics/index.md`

```markdown
# Mathematics

Foundations every developer needs — from set theory to linear algebra.

- [Linear Algebra](linear-algebra/index.md)
- [Calculus](calculus/index.md)
- ...
```

### Module `/mathematics/linear-algebra/index.md`

```markdown
# Linear Algebra

Vectors, matrices, and their applications in computing.

- [Vectors & Matrices](vectors-and-matrices/index.md)
- [Matrix Operations](matrix-operations/index.md)
- [Eigendecomposition](eigendecomposition/index.md)
```

### Submodule `/mathematics/linear-algebra/vectors-and-matrices/index.md`

```markdown
# Vectors & Matrices

Representing data and transformations with arrays of numbers.

- [Introduction to Vectors & Matrices](intro/index.md)
- [Vector Operations](vector-operations.md)
```

### `intro/index.md`

```markdown
# Vectors & Matrices: Introduction

Background, history, and philosophy of vector spaces.

- [What Are Vectors & Matrices?](vectors-and-matrices.md)
- [History of Linear Algebra](history-of-linear-algebra.md)
```

### The 4-Level Index Chain

Every content file must be reachable from the root `index.md` through four levels of navigation:

```
Level 1  Master (root)     /index.md
                             └── [Subject](subject/index.md)
Level 2  Subject           subject/index.md
                             └── [Module](subject/module/index.md)
Level 3  Module            subject/module/index.md
                             ├── [Intro](subject/module/intro/index.md)
                             └── [Submodule](subject/module/submodule/index.md)  (optional)
Level 4  Submodule/Intro   subject/module/intro/index.md
                            subject/module/submodule/index.md
                             └── {short-description}.md
```

Each level must link **down** to the next. A content file is only considered published when it appears in all four levels:

| Level | File | Links To |
|---|---|---|
| 1 | `/index.md` | `subject/index.md` |
| 2 | `subject/index.md` | `subject/module/index.md` |
| 3 | `subject/module/index.md` | `subject/module/intro/index.md` or `subject/module/submodule/index.md` |
| 4 | `subject/module/intro/index.md` or `subject/module/submodule/index.md` | actual `.md` content files |

A missing link at any level means the content is orphaned. Always verify the full chain when creating or moving files.

### Rules for index files

- **Every directory must be listed.** If `linear-algebra/vectors-and-matrices/` has a file, `vectors-and-matrices/index.md` must link to it.
- **Index as planning tool.** An `index.md` may list files that do not yet exist by appending `(planned)` to the link text. This marks the topic as planned but not yet created. Planned entries must **not** have a link (the file does not exist yet). Once the file is written, add the link and remove the `(planned)` label.
- **Do not list directories** — list actual `.md` files.
- **Use relative paths** only. Never absolute or full URLs for internal links.
- **Keep the list ordered** by recommended reading order. Use section headings in the index to group related topics.

## Mandatory Document Format

Every content `.md` file must follow this exact structure, in order:

```markdown
# Title

## Description

Brief overview — what this covers and why a developer should care. 1–3 sentences.

## Prerequisites

- [Concept A](relative/path.md) — what the reader should know first
- [Concept B](relative/path.md)

Index files are valid prerequisite targets when the reader must understand the entire subject, module, or submodule. Example: a document about distributed consensus might list `[Networks](networks/index.md)` because it assumes mastery of all networking concepts.

## Table of Contents

- [Section 1](#section-1)
- [Section 2](#section-2)

## Content / Material

The core material. Use headings, paragraphs, code blocks, diagrams (Mermaid), and math (LaTeX `$$`) as needed. Be concise but complete.

## Study Cases (Optional)

Real-world scenarios that apply the concepts. Walk through a problem and its solution.

## Examples (Optional)

Additional examples beyond the content section. Useful for edge cases or variations.

## Glossary

| Term | Definition |
|------|------------|
| Term | One-line definition |

## Quick References (Optional)

External resources — journals, books, blog posts, websites. Every link must be verified and accessible before inclusion.

- [Title](URL) — one-line description
- [Title](URL)

## Next Steps

Where to go next. Link to related documents or suggest practice exercises.

- [Related Topic A](relative/path.md)
- [Related Topic B](relative/path.md)
```

> Do not skip, reorder, or rename sections. Sections marked "(Optional)" may be omitted if genuinely not needed. Quick References items must be verified — every link must be valid and accessible before inclusion.

## Content Generation Rules

### Do's

- **Be practical.** Connect every concept to how it matters in real development.
- **Prefer code.** A snippet is worth a paragraph. Use language-identified fenced blocks.
- **Define terms** on first use, then add them to the Glossary.
- **Link internally** to prerequisites and next steps. Cross-link between related topics.
- **Use Mermaid** for diagrams (flowcharts, sequence diagrams, graphs).
- **Use LaTeX** (`$` inline, `$$` display) for math.
- **Keep files focused.** A single document should cover one coherent topic. Split if it grows too long.
- **Line count 400–800.** Every content file must be at least 400 lines. If shorter, expand with more depth, examples, study cases, or diagrams. If longer than 800 lines, split into multiple focused documents (e.g., a related sub-topic) and link them via Next Steps.

### Don'ts

- **No fluff.** No "in this article we will learn", "welcome to", or "let's dive in".
- **No assumptions about tooling.** Do not assume the reader uses a specific OS, editor, or package manager unless the topic requires it.
- **No external dependencies.** Do not reference npm packages, libraries, or frameworks that the document does not explain.
- **No placeholder text.** Every section must have real content. If a topic is genuinely too short for a section, omit it rather than fill with filler.
- **No absolute internal links.** Use `../` relative paths only.
- **No generated placeholders like `[TODO]`** — if content cannot be written now, do not create the file.

## Workflow for Creating New Content

Follow these steps in order:

### 1. Check existing content

Read the parent `index.md` and sibling files to understand what already exists. Avoid duplication.

### 2. Determine placement

```
mathematics/           ← subject (check README Topics table)
└── calculus/          ← module (create if needed)
    └── derivatives/   ← submodule (create if needed) or `intro/`
        └── chain-rule.md  ← slug
```

### 3. Create directory structure

Create all missing parent directories first.

### 4. Write the content file

Follow the mandatory document format. Write the full `.md` file.

### 5. Update indexes

- Create or update the parent submodule `index.md` to include a link to the new file.
- If the submodule `index.md` did not exist, create it and update the module `index.md` to link to it.
- If the module `index.md` did not exist, create it and update the subject `index.md`.
- If the subject `index.md` did not exist, create it and update the root `index.md`.
- If the file is in `intro/`, update or create `intro/index.md` to include the new link.

### 6. Verify

- The new file is reachable from root by following `index.md` links.
- All internal links in the new file resolve to existing files.
- The file follows the mandatory format completely.

## Workflow for Updating Existing Content

1. Read the file and its parent `index.md`.
2. Make the minimum change needed.
3. If the file's slug or path changes, update all links pointing to it (check `grep -r "old-path"`).
4. If the file is removed, remove its entry from the parent `index.md`.

## Quality Checklist

Before considering a task complete, verify:

- [ ] All content is in **English**.
- [ ] The **mandatory format** is followed exactly (all required sections present, in order).
- [ ] **Title** matches the file's topic — not generic.
- [ ] **Description** explains why a developer should care.
- [ ] **Prerequisites** links exist and point to real files.
- [ ] **Table of Contents** matches the actual section headings.
- [ ] **Content** is substantive, practical, and code-rich where applicable.
- [ ] **Line count 400–800** — every content file meets the minimum and is not too long.
- [ ] **Glossary** defines every non-trivial term introduced.
- [ ] **Quick References** items (if present) are verified — all external links are valid and accessible.
- [ ] **Next Steps** link to existing related documents.
- [ ] **index.md** files are updated — every new file is linked from its parent index.
- [ ] **Directory structure** follows the convention exactly.
- [ ] **No broken links** — all relative paths resolve.
- [ ] **No placeholder content** — no `TODO`, `FIXME`, or empty sections.
