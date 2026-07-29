---
name: context-awareness
description: Always active. Ensures the agent maintains correct context, reads project rules before acting, and follows established conventions for every task regardless of type.
---

# Skill: Context Awareness

> **About DevBook:** This project is a Markdown-based learning library for developers — content is organized as narrative-driven learning paths, not reference docs. It is interdisciplinary (technical + personal development + philosophy), uses academic English (except `level-up/` which uses literary nonfiction), and integrates Christian worldview implicitly. All content follows a mandatory 9-section format and is organized in a 4-level index chain (root → subject → module → content). See `AGENTS.md` for the full project identity.

This skill is **always active**. It is not loaded on demand — it is the foundation on which all other skills operate. It ensures the agent maintains correct context, follows project conventions, and avoids stale assumptions for every task, regardless of type.

## Purpose

The agent must never act without first establishing correct context. Context awareness is not optional. It is the prerequisite for every action. Without it, the agent produces work that is technically correct but structurally wrong — content that follows the template but breaks the index, files that are well-written but orphaned, changes that solve the immediate problem but create cascading issues.

## Mandatory Pre-Action Checklist

Before performing **any** action — writing, editing, planning, researching, reorganizing, or answering questions — the agent must complete the following steps. No exceptions.

### Step 1: Read the Project Rules

Read these files in order, before any other action:

1. `AGENTS.md` — the navigation hub. Always read this first.
2. `CONTENT-RULES.md` — the single source of truth for all content conventions.
3. `TEMPLATE.md` — the mandatory document format (when writing or reviewing content).

**Why this matters:** The rules evolve. File counts change. Conventions shift. New sections are added. The agent cannot rely on memory or prior sessions. Every session begins by reading the current state of the rules.

### Step 2: Read the Index Chain

Follow the Index-First Workflow defined in AGENTS.md:

1. Read the root `index.md`.
2. Read the subject `index.md`.
3. Read the module `index.md`.
4. Read related module indexes if cross-references are relevant.
5. Read the `intro/` file if it exists.

**Why this matters:** The index is the map of the project. Without reading it, the agent cannot know where content fits, what already exists, or what gaps remain. The index changes as content is added. Reading it at the start of every session ensures the agent works from current state, not stale memory.

### Step 3: Load the Matching Skill

Identify which content skill(s) apply to the current task and load them:

| Task Type | Skill to Load |
|-----------|---------------|
| Writing, expanding, editing content `.md` files | `content-writing` |
| Planning, researching, outlining, proposing new modules | `content-planning` |
| Creating/modifying content in `careers/` | `career-journey` |
| Creating/modifying content in `level-up/` | `leveling-up` |

Multiple skills may apply. Load all that are relevant.

**Why this matters:** Each skill contains rules, workflows, and quality checklists specific to its domain. Loading the skill injects those rules into the agent's context, ensuring compliance with conventions that the agent might otherwise miss.

### Step 4: Verify Current State

Before making changes, verify the current state of the files involved:

- Read the files that will be modified.
- Check for existing content that may conflict with planned changes.
- Identify all links that reference files being changed (use `grep`).
- Confirm directory structure exists before creating files within it.

**Why this matters:** Assumptions about file state are the most common source of errors. A file may have been modified since the last session. A link may have been updated. A directory may have been renamed. Verification prevents cascading breakage.

## Context Construction Rules

These rules govern how the agent builds and maintains context throughout a session.

### Rule 1: Never Assume File State

Do not assume a file exists, does not exist, or contains specific content without reading it first. The project is actively evolving. Files are added, modified, moved, and removed between sessions.

**Before any file operation:**
- Read the file to confirm its current state.
- Do not rely on descriptions from previous sessions or prior context.
- Do not assume directory structure — verify with `ls` or `glob`.

### Rule 2: Never Assume Naming Conventions from Memory

Do not assume the names of modules, subjects, files, or directories from memory. Always read the index or directory listing to confirm current names.

**Before referencing any file path:**
- Read the parent index to confirm the file's actual name.
- Use `glob` or `ls` to verify the path exists.
- Do not construct paths from memory — construct them from verified source material.

### Rule 3: Always Follow the Index-First Workflow

The Index-First Workflow is not a suggestion. It is a mandatory prerequisite for every task. It applies to writing, editing, planning, reorganizing, and answering questions about content.

**The workflow is:**
1. Read root `index.md`.
2. Read subject `index.md`.
3. Read module `index.md`.
4. Read related indexes.
5. Read `intro/` file.
6. Then act.

**No step may be skipped.** The workflow exists because the project's structure is complex, interconnected, and evolving. Skipping steps produces work that is locally correct but globally broken.

### Rule 4: Cross-Reference Before Linking

Before creating any internal link, verify the target exists and the path is correct. Do not create links from memory or assumption.

**Before adding a link:**
- Read the target file to confirm it exists.
- Construct the relative path from the current file's location.
- Verify the path resolves correctly.
- After creating the link, verify it works.

### Rule 5: Preserve Existing Structure

When modifying files, preserve the existing structure unless the task explicitly requires changing it. Do not reorder sections, rename headings, or reorganize content unless specifically instructed.

**Preservation rules:**
- Keep existing section headings in their current order.
- Keep existing links unless they are broken.
- Keep existing formatting conventions (emoji usage, code block styles, etc.).
- Make the minimum change needed to accomplish the task.

### Rule 6: Avoid Stale Information in Output

When producing output — whether writing content, answering questions, or planning changes — do not include information that may be outdated or that the agent cannot verify in the current session.

**Stale information includes:**
- File counts or line counts from prior sessions.
- Specific module names, subject names, or directory structures that have not been verified.
- Claims about what exists in the project that have not been confirmed by reading.
- Dates, versions, or numerical data that may have changed.

**Instead:**
- Read the current state before making claims.
- Use present tense for verified facts.
- Avoid specific counts unless verified in the current session.
- Reference documents by their verified paths, not by memory.

### Rule 7: Maintain Session Continuity

At the end of a session or when the task context shifts, summarize what was done and what remains. This summary becomes the starting point for the next session, reducing the need to re-read everything from scratch.

**Continuity rules:**
- Note which files were modified.
- Note which indexes were updated.
- Note any broken links that were found and fixed.
- Note any tasks that were started but not completed.

## Anti-Patterns

These are common context-awareness failures. The agent must actively avoid them.

### Assumption Without Verification

**Wrong:** "The `level-up/index.md` has 7 stages." (Assumed from memory.)
**Right:** Read `level-up/index.md` and count the stages.

### Path Construction From Memory

**Wrong:** "The file is at `level-up/intro/the-beginning.md`." (Constructed from memory.)
**Right:** Use `glob` or read the directory to find the actual filename.

### Link Creation Without Verification

**Wrong:** "Link to `../foundations/skills.md`." (Assumed the file exists.)
**Right:** Read the target directory, confirm the file exists, then create the link.

### Skipping the Index-First Workflow

**Wrong:** "I know where this content fits — I'll write it directly."
**Right:** Read the full index chain first, then write.

### Relying on Prior Session Knowledge

**Wrong:** "In the previous session, we established that..." (Stale context.)
**Right:** "Based on the current state of the files, we see that..."

### Modifying Without Reading

**Wrong:** "I'll add a new section to this file." (Without reading it first.)
**Right:** Read the file, understand its current structure, then modify.

## Integration with Other Skills

This skill is the foundation that all other skills build upon. Every skill's Index-First Workflow, quality checklist, and critical rules depend on context awareness being maintained.

- `content-planning` assumes the agent has read the project rules and index chain.
- `content-writing` assumes the agent knows the current file state and conventions.
- `career-journey` assumes the agent has read the career subject index.
- `leveling-up` assumes the agent has read the level-up subject index.

**Context awareness is not a separate step.** It is the continuous process of maintaining correct understanding of the project's current state, conventions, and structure throughout the entire session.

## Quality Checklist

Before completing any task, verify:

- [ ] All project rules were read at the start of the session.
- [ ] The full index chain was read before any action.
- [ ] The matching content skill(s) were loaded.
- [ ] All files were read before being modified.
- [ ] All links point to verified, existing files.
- [ ] No stale information was included in output.
- [ ] The minimum change was made to accomplish the task.
- [ ] Existing structure was preserved unless explicitly changed.
