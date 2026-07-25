# AGENTS.md — AI Agent Navigation Hub

This file is the entry point for AI agents managing content in DevBook. It is a navigation hub — it points you to the right documents and skills. The actual rules, templates, and workflows live in dedicated files.

## Project Identity

DevBook is a Markdown-based learning library for developers. All content is in plain `.md` files — no build tools, no frameworks. Every document must be practical, concise, and developer-focused.

## Read These First

| Document | Purpose | When to read |
|----------|---------|--------------|
| [CONTENT-RULES.md](CONTENT-RULES.md) | All content rules, conventions, tiering, directory structure, index system, worldview integration | **Always** — before any task |
| [TEMPLATE.md](TEMPLATE.md) | Document template with all 9 sections and tiering guidance | When writing or reviewing content |
| [index.md](index.md) | Master learning path — the world map of all subjects | **Always** — before any task (Index-First Workflow) |

## Agent Skills

Skills provide specialized instructions and workflows for specific tasks. Load the matching skill when a task begins.

| Skill | When to use | Rules |
|-------|-------------|-------|
| **content-planning** | Planning, researching, outlining, proposing new modules/submodules, reorganizing index structure | [rules/structural-changes.md](.agents/skills/content-planning/rules/structural-changes.md), [rules/tiering-guide.md](.agents/skills/content-planning/rules/tiering-guide.md) |
| **content-writing** | Writing, expanding, editing, or reviewing any content `.md` file | [rules/format-rules.md](.agents/skills/content-writing/rules/format-rules.md), [rules/quality-checklist.md](.agents/skills/content-writing/rules/quality-checklist.md) |
| **career-journey** | Creating or modifying content in `careers/` | [rules/career-rules.md](.agents/skills/career-journey/rules/career-rules.md) |
| **leveling-up** | Creating or modifying content in `level-up/` | [rules/narrative-rules.md](.agents/skills/leveling-up/rules/narrative-rules.md) |

## Index-First Workflow

**Every workflow begins with the index.** Before any action — writing, editing, planning, researching, reorganizing — you must first read and understand the index documents. This is the mandatory first step.

1. **Read the root `index.md`** — Understand the full horizontal learning path and which phase the target subject belongs to.
2. **Read the subject `index.md`** — Understand the modules, their phase groupings, and the vertical progression within the subject.
3. **Read the module `index.md`** — Understand the learning path tree, existing files, and where new content fits.
4. **Read related module indexes** — Identify cross-references, prerequisite relationships, and potential overlap with other subjects.
5. **Read the `intro/` file** — Understand the module's stated purpose and rationale before adding to it.

Only after this context is established may you proceed to any task.

**When planning new modules or submodules**, the Index-First Workflow additionally requires:
- **Justification** — Articulate what specific body of knowledge the new module represents and why it constitutes a distinct unit of study (see [CONTENT-RULES.md](CONTENT-RULES.md#module-raison-dêtre)).
- **Phase determination** — Assign the new module to a learning phase in the subject index before writing content.
- **Relationship mapping** — Identify prerequisites, siblings, and extensions among existing modules.

## Workflows

### Creating New Content

1. **Index-First Workflow** — Complete the index analysis above.
2. **Load the content-writing skill** for format rules and quality checklist.
3. **Determine placement** — Identify the subject, module, submodule, and filename.
4. **Create directory structure** — Create all missing parent directories first.
5. **Write the content file** — Follow the mandatory document format from [TEMPLATE.md](TEMPLATE.md).
6. **Update indexes** — Create or update parent `index.md` files to include the new file.
7. **Verify** — File is reachable from root. All internal links resolve. Format is correct.

### Updating Existing Content

1. **Index-First Workflow** — Complete the index analysis above.
2. Read the file and its parent `index.md`.
3. Make the minimum change needed.
4. If the file's slug or path changes, update all links pointing to it (check `grep -r "old-path"`).
5. If the file is removed, remove its entry from the parent `index.md`.

### Reorganizing Content

1. **Index-First Workflow** — Read the full index chain.
2. **Load the content-planning skill** for structural change workflows.
3. **Document current state** — List all files that will move and their current paths.
4. **Map old paths to new paths** — Every move must be tracked.
5. **Update every index** that references moved files.
6. **Grep for broken links** — Search for every old path across all `.md` files.
7. **Verify the 4-level chain** — Root → subject → module → content must still hold.

## Critical Rules

- **Line count 400–800 lines per content file.** Every content `.md` file must be between 400 and 800 lines. If shorter, expand with more depth, examples, diagrams, or walkthroughs. If longer, first try trimming redundant or non-essential content. If the topic is genuinely complex and cannot be shortened, split into multiple focused documents and link them via Next Steps. Consider whether the topic should be tiered (basic/intermediate/advanced).
- **English only, academic register.** No colloquialisms, contractions, or conversational tone.
- **Mandatory 9-section format.** Every content file follows the structure in [TEMPLATE.md](TEMPLATE.md).
- **Tiering: maximum 3 levels.** If a topic spans multiple complexity levels, split into at most 3 tiered files (`-basic.md`, `-intermediate.md`, `-advanced.md`). Not all topics require tiering — simple topics that fit within 400–800 lines at a single level do not need to be split.
- **Every module must have an `intro/` directory.** The `intro/` contains background, philosophy, principles, history, or context about the field. Intro files are narrative and explanatory — they do not follow the mandatory 9-section format and do not need to meet the 400–800 line requirement. Every `intro/` must have an `index.md` listing its files.
- **`foundations/` modules are prerequisite entry points.** A `foundations/` module within any subject provides the minimum prerequisite knowledge before the rest of the subject becomes accessible. Foundations content is typically introductory and builds upward toward the subject's core modules.
- **Emoji usage throughout content.** Emojis are encouraged in section headings, list items, callout blocks, tables, and within prose to enhance readability and visual appeal. Use them consistently and moderately — never stack multiple emojis, never use them in code blocks or file paths, and always preserve the academic register.

## Quick Reference

- **Directory structure:** `{subject}/{module}/{submodule(optional),intro}/{short-description}.md`
- **File naming:** Hyphenated slugs, lowercase (e.g., `vector-operations.md`)
- **Tiered files:** `{topic}-basic.md`, `{topic}-intermediate.md`, `{topic}-advanced.md`
- **Index format:** Phased headings for root/subject, learning path trees for module/submodule
- **Language:** English only, academic register
- **Line count:** 400–800 lines per content file
