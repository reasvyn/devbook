---
name: content-writing
description: Use when the user asks to write, expand, edit, or review any content .md file in the DevBook project. Guides the mandatory 9-section format, English-only rule, index chain requirement, and linking conventions.
---

# Skill: Content Writing

This skill governs the creation and maintenance of ALL content `.md` files in DevBook. It applies regardless of subject.

## Index-First Workflow

**Every action begins with the index.** Before writing, editing, reviewing, or planning any content, the agent must first read and understand the index documents. This is non-negotiable.

1. **Read the root `index.md`** — Understand the full learning path and where the target subject sits within the horizontal phases.
2. **Read the subject `index.md`** — Understand the modules, their phase groupings, and the vertical progression within the subject.
3. **Read the module `index.md`** — Understand the learning path tree, existing files, and where new content fits.
4. **Read the `intro/` file** — Understand the module's stated purpose and raison d'être before adding to it.
5. **Then act.** Write, edit, or plan content only after the index context is clear.

This applies to **all scenarios**:
- **Writing a new content file** — the index chain reveals prerequisites, siblings, and next steps.
- **Planning a new module or submodule** — start from the subject index to determine the phase, justify the module's existence, and identify where it fits in the learning path.
- **Reorganizing existing content** — the index chain must be read first to avoid breaking links or orphaning files.
- **Reviewing content** — check that the file is properly positioned in the index and follows the learning path progression.

## Two-Phase Writing Workflow

When creating multiple new content files, **never write and expand one file at a time.** This is extremely slow because the `Write` tool truncates long content (~150 lines) and the `Edit` tool adds only small increments. Instead, use a two-phase approach:

### Phase 1: Write All Files

Write every planned file in a single pass using the `Write` tool. Accept that each file will be truncated at ~150 lines. **Do NOT stop to expand individual files during this phase.** Focus on:

- Correct structure (9-section format)
- Correct content (real, substantive prose — not placeholders)
- Correct cross-references (Prerequisites, Next Steps link to real files)
- Correct directory structure (all parent dirs created first)

This phase produces short files. That is expected and acceptable.

### Phase 2: Expand All Files

After all files are written, expand each file to 400–800 lines using the `Edit` tool. Use `wc -l` to verify each file after expansion. Tips for efficient expansion:

- Use the task tool to expand files in parallel when possible
- Add new paragraphs within existing sections — do NOT rewrite from scratch
- Focus expansion on the Content sections, not the boilerplate sections
- Verify line count after each expansion batch

### Why This Works

The `Write` tool truncates at ~150 lines regardless of how much content you provide. Writing a 600-line file produces the same 150-line output as writing a 150-line file. Expanding requires the `Edit` tool regardless. By separating the phases, you:

1. **Reduce context switching** — write mode and expand mode are different mental states
2. **Avoid re-reading** — each file is read once for writing, once for expansion
3. **Enable parallelism** — multiple files can be expanded simultaneously via task tool
4. **Catch structural issues early** — if a file needs to be restructured, you discover it before investing in expansion

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
- **Line count.** Every content file must meet the project's length requirement. If too short, expand with more depth in the Content section. If too long, first try trimming redundant or non-essential content. If the topic is genuinely complex and cannot be shortened, split into multiple focused documents and link them via Next Steps.
- **No orphans.** Every content file must be referenced by its parent index file. Verify the full index chain: root → subject → module/submodule → file.
- **No placeholder text.** No `TODO`, `FIXME`, `[planned]`, or empty sections. Write real content or omit the section.
- **No fluff.** No "in this article", "welcome to", "let's dive in". Get straight to the material.
- **Code when it clarifies.** Use language-identified fenced code blocks in popular languages (Python, JavaScript, TypeScript, Go, Rust, etc.) when a code example genuinely adds clarity. Do not force code into topics where prose or diagrams communicate better.
- **Define terms on first use**, then add them to the Glossary.
- **Use relative paths only** for internal links. Never absolute URLs for internal content.
- **Mermaid for diagrams**, LaTeX (`$`/`$$`) for math.
- **Cross-link** between related topics in Prerequisites and Next Steps.
- **RPG-like learning experience.** Structure content as a quest-based journey. The Description sets the mission, the Prerequisites define what you need before attempting, the Content is the challenge, the Learning Tips and Glossary are the rewards, and the Next Steps point to the next quest. The module index is the world map. Cross-references are the skill tree. This framing makes progression feel natural and rewarding — but never sacrifice scientific accuracy for the sake of the metaphor. All mandatory section headings remain unchanged.
