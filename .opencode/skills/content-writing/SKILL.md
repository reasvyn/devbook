---
name: content-writing
description: Use when the user asks to write, expand, edit, or review any content .md file in the DevBook project. Guides the mandatory 9-section format, English-only rule, index chain requirement, and linking conventions.
---

# Skill: Content Writing

This skill governs the creation and maintenance of ALL content `.md` files in DevBook. It applies regardless of subject.

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
- **Line count.** Every content file must meet the project's length requirement. If too short, expand with more depth in the Content section. If too long, split into multiple focused documents and link them via Next Steps.
- **No orphans.** Every content file must be referenced by its parent index file. Verify the full index chain: root → subject → module/submodule → file.
- **No placeholder text.** No `TODO`, `FIXME`, `[planned]`, or empty sections. Write real content or omit the section.
- **No fluff.** No "in this article", "welcome to", "let's dive in". Get straight to the material.
- **Code over explanation.** Use language-identified fenced code blocks. A snippet is worth a paragraph.
- **Define terms on first use**, then add them to the Glossary.
- **Use relative paths only** for internal links. Never absolute URLs for internal content.
- **Mermaid for diagrams**, LaTeX (`$`/`$$`) for math.
- **Cross-link** between related topics in Prerequisites and Next Steps.
