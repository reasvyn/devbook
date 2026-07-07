---
name: content-writing
description: Use when the user asks to write, expand, edit, or review any content .md file in the DevBook project. Guides the mandatory 9-section format, 400-800 line limit, English-only rule, index chain, and linking conventions.
---

# Skill: Content Writing

This skill governs the creation and maintenance of ALL content `.md` files in DevBook. It applies regardless of which subject the file belongs to.

## Mandatory Format

Every content `.md` file must follow this exact structure, in order:

```markdown
# Title

## Description

Brief overview — what this covers and why a developer should care. 1–3 sentences.

## Prerequisites

- [Concept A](relative/path.md) — what the reader should know first
- [Concept B](relative/path.md)

## Table of Contents

- [Section 1](#section-1)
- [Section 2](#section-2)

## Content / Material

The core material. Use headings, paragraphs, code blocks, diagrams (Mermaid), and math (LaTeX $$) as needed.

## Learning Tips (Optional)

Practical advice for studying and retaining the material.

## Glossary

| Term | Definition |
|------|------------|
| Term | One-line definition |

## Quick References (Optional)

External resources — every link must be verified and accessible.

- [Title](URL) — one-line description

## Next Steps

Where to go next. Link to related documents.

- [Related Topic A](relative/path.md)
```

## Critical Rules

- **Language: English only.** Never write content in any other language.
- **Line count: 400–800.** Every content file must be at least 400 lines. If shorter, expand. If longer than 800, split into multiple focused documents.
- **No orphans.** Every content file must be referenced by its parent `index.md`. Verify the 4-level index chain: root → subject → module/submodule → file.
- **No placeholder text.** No `TODO`, `FIXME`, `[planned]`, or empty sections. Write real content or omit the section.
- **No fluff.** No "in this article", "welcome to", "let's dive in". Get straight to the material.
- **Code over explanation.** Use language-identified fenced code blocks. A snippet is worth a paragraph.
- **Define terms on first use**, then add them to the Glossary.
- **Use relative paths only** for internal links. Never absolute URLs for internal content.
- **Mermaid for diagrams**, LaTeX (`$`/`$$`) for math.
- **Cross-link** between related topics in Prerequisites and Next Steps.

## Index Chain Verification

When creating a new file, always verify:

| Level | File | Links To |
|---|---|---|
| 1 | `/index.md` | `subject/index.md` |
| 2 | `subject/index.md` | `subject/module/index.md` |
| 3 | `subject/module/index.md` | `subject/module/intro/index.md` or `subject/module/submodule/index.md` |
| 4 | `subject/module/intro/index.md` or `subject/module/submodule/index.md` | actual `.md` content file |
