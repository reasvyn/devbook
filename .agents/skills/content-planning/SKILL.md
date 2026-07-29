---
name: content-planning
description: Use when the user asks to plan, research, outline, or prepare content. Also use when proposing new subjects, modules, submodules, or reorganizing the index structure. Covers the full lifecycle from index analysis through research to writing blueprint.
---

# Skill: Content Planning

> **About DevBook:** A Markdown-based learning library for developers. Content is organized as narrative-driven learning paths with prerequisites, progressive depth, and next steps — interdisciplinary by design, covering technical skills, personal transformation, and human wisdom. See `AGENTS.md` for the full identity.

This skill governs the entire planning phase of content creation in DevBook — from understanding where content fits in the learning path, through research and synthesis, to producing a writing blueprint. It covers not just individual documents but also structural decisions: new modules, submodules, reorganization, and index maintenance.

## Index-First Workflow

**Every action begins with the index.** Before researching, planning, or proposing any structural change, the agent must first read and understand the index documents to map the knowledge landscape.

1. **Read the root `index.md`** — Understand the full horizontal learning path and which phase the target subject belongs to.
2. **Read the subject `index.md`** — Understand the modules, their phase groupings, and the vertical progression within the subject.
3. **Read the module `index.md`** — Understand the existing learning path tree, what content already exists, and what gaps remain.
4. **Read related module indexes** — Identify cross-references, prerequisite relationships, and potential overlap with other subjects.
5. **Read the `intro/` file** — Understand the module's stated purpose and rationale before planning additions.
6. **Then plan.** Begin research and outlining only after the index context reveals where the new content will live and what it must connect to.

This applies to **all scenarios**:
- **Planning a new content file** — the index chain reveals prerequisites, siblings, next steps, and the learning phase.
- **Planning a new module or submodule** — start from the subject index to justify the module's existence, determine its phase, and identify its relationship to existing modules.
- **Reorganizing existing content** — the index chain must be read first to understand the impact on the learning path.
- **Proposing index restructuring** — the full index tree must be understood before any phase or grouping changes.

## Planning Workflow

The planning workflow has four phases. Every content creation or structural change must pass through all four before any file is written.

### Phase 1: Index Analysis

**Goal:** Understand the current state of the knowledge structure.

1. Read the full index chain (root → subject → module → submodule/intro).
2. Map the horizontal phases — where does the target subject sit in the learning path?
3. Map the vertical depth — what already exists within the subject and module?
4. Identify gaps — what topics are missing? What prerequisites are unlinked?
5. Identify conflicts — does the proposed content overlap with existing files?

**Output:** A positioning statement that answers:
- What phase does this content belong to?
- What is its relationship to existing modules (prerequisite, sibling, extension)?
- What gap does it fill?

### Phase 2: Justification

**Goal:** Prove the content must exist before writing it.

Before planning any new module or submodule, answer these questions:

1. **What specific body of knowledge does this represent?** Not "a bunch of related topics" — a coherent, named field of study.
2. **Why does it constitute a distinct unit of study?** Why can it not be a section within a broader module?
3. **Is it a valid academic discipline?** Does it have its own literature, research traditions, and institutional recognition? (Exception: narrative and career-exploration subjects are not academic disciplines but are valid subjects.)
4. **What is the learning dependency?** What must the reader know before entering this module? What does this module unlock afterward?

If any answer is weak or vague, the module should not exist — fold its content into the parent or a sibling.

**Output:** A justification paragraph that can be placed in the module's `intro/` file.

### Phase 3: Research

**Goal:** Gather sufficient reference material to write with authority.

1. **Scope Definition**
   - What specific question or concept does this document address?
   - Who is the reader (a developer — what level of prior knowledge)?
   - What existing DevBook content relates to this topic? (Confirmed by index analysis.)

2. **Source Collection**
   Use the WebFetch tool to gather information from:
   - Authoritative references (specifications, academic papers, official documentation)
   - Established tutorials and guides from recognized experts
   - Wikipedia or similar encyclopedic sources for foundational context
   - Primary sources when available (original papers, specification authors)

   Prioritize:
   1. Primary sources (specifications, original research, official docs)
   2. Authoritative secondary sources (established textbooks, respected tutorials)
   3. Encyclopedic summaries for broad context

3. **Synthesis**
   After gathering sources:
   - Identify the key concepts, their relationships, and their practical significance to developers.
   - Note areas of consensus and disagreement among sources.
   - Identify prerequisite concepts the reader needs to understand first.
   - Determine the logical structure: what order best builds understanding?

**Quality Gates:**
- At least 2-3 independent sources should inform the document, unless the topic has a single authoritative source.
- Every factual claim should be traceable to at least one source from the research phase.
- External links in Quick References must be verified by fetching them — not assumed to exist.

### Phase 4: Blueprint

**Goal:** Produce a writing blueprint that makes drafting straightforward.

Before writing the document, produce a structured blueprint containing:

```markdown
# Blueprint: {Document Title}

## Positioning
- **Phase:** [learning phase in the horizontal path]
- **Module:** [which module this belongs to]
- **Relationship:** [prerequisite of / sibling to / extension of {existing file}]

## Justification
[Why this content must exist as a separate document]

## Core Thesis
[One sentence: what does this document teach and why does it matter?]

## Prerequisites
- [Existing DevBook doc 1]
- [Existing DevBook doc 2]

## Section Outline
1. **[Section Name]** — [what the reader learns here]
2. **[Section Name]** — [what the reader learns here]
3. ...

## Cross-References
- **Prerequisites:** [links to existing files the reader must know first]
- **Next Steps:** [links to files this document leads to]
- **Related:** [links to files in other subjects that provide context]

## Source Summary
- [Source 1] — [key insight extracted]
- [Source 2] — [key insight extracted]
- ...
```

**Output:** A complete blueprint document that serves as the contract for the writing phase.

## When Planning Structural Changes

When the task involves creating, moving, or removing modules, submodules, or reorganizing the index, additional steps apply:

### Creating a New Module or Submodule

1. Complete Phase 1 (Index Analysis) and Phase 2 (Justification) above.
2. **Draft the module `index.md`** with:
   - A `# Title` matching the module name.
   - A `## 1. Introduction` section linking to the `intro/` file.
   - `## 2. ...` sections forming the initial learning path (mark unplanned entries as `(planned)`).
3. **Draft the `intro/` file** with the justification paragraph from Phase 2.
4. **Update the parent index** — add a link to the new module in the correct phase.
5. **Verify the 4-level chain** — root → subject → module → content. Every level must link down.

### Reorganizing Existing Content

1. Read the full index chain to understand all affected files.
2. **List all files that will move** and their current paths.
3. **Map old paths to new paths** — every move must be tracked.
4. **Update every index** that references moved files.
5. **Grep for broken links** — search for every old path across all `.md` files.
6. **Verify the 4-level chain** still holds after changes.

### Proposing Index Restructuring

1. Read the full index tree (root → all subjects → all modules).
2. **Document the current phase assignments** — which subjects/modules are in which phases.
3. **Propose the new phase assignments** with justification for each move.
4. **Identify cascade effects** — does moving one subject affect the coherence of its source and destination phases?
5. **Preserve vertical order** — when splitting or merging, the module `index.md` must continue the progression the phases imply.

## Quality Checklist

Before handing off to the writing phase, verify:

- [ ] Index chain has been fully read and understood.
- [ ] The content's position in the learning path is clear (phase and relationship).
- [ ] The justification for existence is documented (for modules/submodules).
- [ ] At least 2-3 sources have been researched and synthesized.
- [ ] A structured blueprint has been produced.
- [ ] Cross-references to existing DevBook content are identified.
- [ ] The 4-level index chain will remain intact after implementation.
