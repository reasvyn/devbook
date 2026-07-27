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

Skills provide specialized instructions and workflows for specific tasks. **Loading the matching skill(s) is mandatory at the start of every task** — the agent must never act without first loading the relevant skill, even if the user does not explicitly request it.

| Skill | When to use | Rules |
|-------|-------------|-------|
| **context-awareness** | **Always active** — ensures correct context, rule compliance, and stale-information avoidance for every task | [rules/context-awareness.md](.agents/skills/context-awareness/rules/context-awareness.md) |
| **content-planning** | Planning, researching, outlining, proposing new modules/submodules, reorganizing index structure | [rules/structural-changes.md](.agents/skills/content-planning/rules/structural-changes.md), [rules/tiering-guide.md](.agents/skills/content-planning/rules/tiering-guide.md) |
| **content-writing** | Writing, expanding, editing, or reviewing any content `.md` file | [rules/format-rules.md](.agents/skills/content-writing/rules/format-rules.md), [rules/quality-checklist.md](.agents/skills/content-writing/rules/quality-checklist.md) |
| **career-journey** | Creating or modifying content in `careers/` | [rules/career-rules.md](.agents/skills/career-journey/rules/career-rules.md) |
| **leveling-up** | Creating or modifying content in `level-up/` | [rules/narrative-rules.md](.agents/skills/leveling-up/rules/narrative-rules.md) |

### Mandatory Skill Loading Protocol

At the **start of every task** — regardless of whether the user mentions skills — the agent must:

1. **Read `AGENTS.md`** — the navigation hub. Always the first action.
2. **Read `CONTENT-RULES.md`** — the single source of truth for all conventions.
3. **Read the root `index.md`** — the master learning path.
4. **Load the `context-awareness` skill** — always active, ensures correct context throughout.
5. **Identify and load the matching content skill(s)** — `content-writing`, `content-planning`, `career-journey`, or `leveling-up` as applicable.
6. **Read the subject and module indexes** — the Index-First Workflow.

This protocol is **non-negotiable**. It applies to every task: writing, editing, planning, researching, reorganizing, answering questions, and reviewing content. No task may proceed without completing this protocol.

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
5. **Write all content files** — Write every planned file in a single pass. Accept that the `Write` tool may truncate long files (~150 lines). Do NOT stop to expand individual files during this phase — focus on getting all files created with correct structure, content, and cross-references.
6. **Expand all files to 400–800 lines** — After all files are written, use the `Edit` tool to expand each file to the required line count. Use `wc -l` to verify each file after expansion. See [format-rules.md](.agents/skills/content-writing/rules/format-rules.md) for the two-phase expansion strategy.
7. **Update indexes** — Create or update parent `index.md` files to include the new files.
8. **Verify** — Files are reachable from root. All internal links resolve. Format is correct. Line counts are within 400–800.

### Updating Existing Content

1. **Index-First Workflow** — Complete the index analysis above.
2. Read the file and its parent `index.md`.
3. Make the minimum change needed.
4. If the file's slug or path changes, update all links pointing to it (check `grep -r "old-path"`).
5. If the file is removed, remove its entry from the parent `index.md`.

### Renaming a Module or Submodule

Module and submodule names may be renamed when the current name **does not accurately reflect the content**. Subject names are **never renamed**. See [CONTENT-RULES.md](CONTENT-RULES.md#module--submodule-renaming) for justification criteria.

1. **Index-First Workflow** — Read the full index chain to understand all affected paths.
2. **Document the mismatch** — Write a specific justification explaining why the current name is inadequate and why the proposed name is more accurate. The justification must reference the actual content, not aesthetic preference.
3. **Verify no naming collision** — Confirm the proposed name does not duplicate any existing module or submodule within the same subject.
4. **Rename the directory** — `git mv old-name new-name`.
5. **Update the parent `index.md`** — Change the link text and path to reflect the new name.
6. **Update all cross-references** — Search for every occurrence of the old directory name across all `.md` files: `grep -r "old-name" --include="*.md"`. Update every link.
7. **Update internal file references** — If the module's own `index.md` or content files reference the old name (e.g., in `# Title` headings), update them.
8. **Verify the 4-level chain** — Root → subject → module → content must still hold.
9. **Verify no broken links** — All relative paths must resolve correctly.

### Reorganizing Content

1. **Index-First Workflow** — Read the full index chain.
2. **Load the content-planning skill** for structural change workflows.
3. **Document current state** — List all files that will move and their current paths.
4. **Map old paths to new paths** — Every move must be tracked.
5. **Update every index** that references moved files.
6. **Grep for broken links** — Search for every old path across all `.md` files.
7. **Verify the 4-level chain** — Root → subject → module → content must still hold.

## Critical Rules

- **NEVER mix contributor guides and AI agent guides.** These are separate subjects that serve different audiences. `CONTRIBUTING.md` is for human contributors — it defines project standards, writing conventions, and contribution workflows. `AGENTS.md` and skill files are for AI agents — they define navigation, task execution, and content management logic. Rules, conventions, and workflows must NOT be duplicated or cross-pollinated between the two. When a rule applies to both, it must be defined once in `CONTRIBUTING.md` (the authoritative source) and referenced — not copied — in AI agent files.

- **AI agent guides are fully subordinate to contributor/human guides.** `CONTRIBUTING.md` is the single source of truth for all project standards. If a conflict arises between `CONTRIBUTING.md` and any AI agent file (`AGENTS.md`, skill files, `CONTENT-RULES.md`), `CONTRIBUTING.md` wins. The agent must not override, reinterpret, or simplify human-defined rules.

- **DO NOT create new subjects or modules without exhausting existing options.** Before proposing any new subject, module, or submodule, the agent MUST:
  1. **Search the entire project** for existing content that already covers the topic (use glob and grep).
  2. **Check every subject's `intro/` directory** — history, biographical, and contextual content belongs in `intro/` files, NOT in new modules.
  3. **Check every subject's `foundations/` directory** — introductory and prerequisite content belongs in `foundations/`, NOT in new modules.
  4. **Ask: "Can this content fit within an existing module?"** If yes, add it there. If no, articulate WHY in writing using the justification questions from [structural-changes.md](.agents/skills/content-planning/rules/structural-changes.md).
  5. **Ask: "Am I creating this module just to group related material?"** If yes, the module should NOT exist — grouping is not justification. Fold the content into the appropriate existing module.

  **Modules exist to delineate distinct bodies of knowledge that require separate study paths — not to organize content by topic.** A folder of related files is not a module. A module is a learning dependency boundary: it defines what you must know before entering and what it unlocks afterward.

- **Module and submodule names must be academically grounded.** Every module and submodule name must represent an established branch of knowledge — a field with its own academic literature, research traditions, and institutional recognition. A name is NOT justified when:
  - It describes a persona, role, or behavioral archetype (e.g., `silent-mover`, `frontend-developer`, `ceo-founders`).
  - It groups related topics without constituting a distinct body of knowledge.
  - It is a popular term that lacks academic standing as a field of study.
  
  A name IS justified when:
  - It corresponds to an established academic discipline (e.g., `linear-algebra`, `cognitive-psychology`).
  - It represents a recognized subfield with its own literature and research traditions (e.g., `self-compassion`, `ego-state-theory`).
  - It describes a distinct unit of study that requires separate learning paths from adjacent modules.

  **Exception:** `level-up/` and `careers/` are special subjects with custom organizational logic. Module names in these subjects may use non-academic naming when justified by the narrative or career-exploration purpose.

- **Line count 400–800 lines per content file.** Every content `.md` file must be between 400 and 800 lines. If shorter, expand with more depth, examples, diagrams, or walkthroughs. If longer, first try trimming redundant or non-essential content. If the topic is genuinely complex and cannot be shortened, split into multiple focused documents and link them via Next Steps. Consider whether the topic should be tiered (basic/intermediate/advanced).
- **English only, academic register.** No colloquialisms, contractions, or conversational tone. **Exception:** `level-up/` uses literary nonfiction — a prose style that applies techniques of fiction to real experience. See [narrative-rules.md](.agents/skills/leveling-up/rules/narrative-rules.md) for the full style definition.
- **Mandatory 9-section format.** Every content file follows the structure in [TEMPLATE.md](TEMPLATE.md).
- **Tiering: maximum 3 levels.** If a topic spans multiple complexity levels, split into at most 3 tiered files (`-basic.md`, `-intermediate.md`, `-advanced.md`). Not all topics require tiering — simple topics that fit within 400–800 lines at a single level do not need to be split.
- **Every module must have an `intro/` directory.** The `intro/` contains background, philosophy, principles, history, or context about the field. Intro files follow all the same rules as content files — 9-section format, 400–800 lines, English only, academic register. The only difference is content scope: intro files address philosophy, background, history, and context rather than technical material. Every `intro/` must have an `index.md` listing its files.
- **`foundations/` modules are prerequisite entry points.** A `foundations/` module within any subject provides the minimum prerequisite knowledge before the rest of the subject becomes accessible. Foundations content is typically introductory and builds upward toward the subject's core modules.
- **Emoji usage throughout content.** Emojis are encouraged in section headings, list items, callout blocks, tables, and within prose to enhance readability and visual appeal. Use them consistently and moderately — never stack multiple emojis, never use them in code blocks or file paths, and always preserve the academic register.

## Quick Reference

- **Directory structure:** `{subject}/{module}/{submodule(optional),intro}/{short-description}.md`
- **File naming:** Hyphenated slugs, lowercase (e.g., `vector-operations.md`)
- **Tiered files:** `{topic}-basic.md`, `{topic}-intermediate.md`, `{topic}-advanced.md`
- **Index format:** Phased headings for root/subject, learning path trees for module/submodule
- **Language:** English only, academic register
- **Line count:** 400–800 lines per content file
- **Module renaming:** Allowed when name does not match content. Subject names are immutable. See [CONTENT-RULES.md](CONTENT-RULES.md#module--submodule-renaming).
