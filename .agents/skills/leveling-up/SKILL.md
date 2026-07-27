---
name: leveling-up
description: Use when the user mentions 'level-up', 'leveling up', 'awakening', 'rebuilding', 'systematizing', 'thriving', or references personal transformation, existential recovery, resilience, habits, or purpose. Guides the creation and maintenance of narrative content in the level-up/ subject.
---

# Skill: Leveling Up

This skill governs all work in the `level-up/` subject of DevBook.

## Index-First Workflow

**Every action begins with the index.** Before creating, editing, or planning any level-up content, the agent must first read and understand the index documents.

1. **Read the root `index.md`** — Understand where `level-up/` sits in the learning path and its role as the entry point of the entire journey.
2. **Read the `level-up/index.md`** — See all existing modules, their groupings, and the current journey structure.
3. **Read the target module's `index.md`** — Understand the learning path tree before adding or modifying files.
4. **Read the `intro/` file** — Understand the module's stated purpose and its place in the journey.
5. **Then act.** Create, edit, or plan content only after the index context is clear.

This applies to **all scenarios**:
- **Adding a new module** — the level-up index reveals the current journey structure and where the new module fits.
- **Modifying existing content** — the index chain shows the progression and cross-references to other subjects.
- **Planning content** — start from the index to ensure the journey remains coherent and the module's existence is justified.

## Core Principles

- **Literary, not academic.** `level-up/` uses the prose techniques of fiction (vivid scenes, sensory detail, emotional depth) to explore real experience. It does NOT teach academic knowledge through academic prose. The theory lives in other DevBook subjects. Here, we walk the path.
- **Reference, don't duplicate.** Every file links to other DevBook subjects for the actual knowledge and theoretical grounding. Use Prerequisites and Next Steps to cross-reference broadly — philosophy, psychology, biology, neuroscience, sociology, or any subject that provides depth.
- **Open-ended journey.** New modules, themes, and stages may emerge as the journey deepens. Do not assume the current structure is final. The journey is the destination.

For the full style definition, perspective rules, and character rules, read [narrative-rules.md](rules/narrative-rules.md).

## Code Examples

Code examples are **optional** in level-up content. The narrative is primary. Use code only when it serves as a metaphor or illustration that prose cannot achieve as effectively. When code is used, write it in a popular language (Python, JavaScript, TypeScript, Go, Rust, etc.) — do not default to a single language. Never force code into a section where it feels artificial.

## When Creating Content

1. Follow the **Index-First Workflow** above — read the full index chain before any action.
2. **Write all planned files first** using the `Write` tool. Accept truncation (~150 lines). Do NOT expand individual files during this phase. See [content-writing SKILL.md](../content-writing/SKILL.md#two-phase-writing-workflow) for the full two-phase workflow.
3. **Expand all files to 400–800 lines** using the `Edit` tool after all files are written. Verify with `wc -l`.
4. Write narrative content following the mandatory 9-section format.
5. In Prerequisites, link to the relevant knowledge files from ANY subject in DevBook that supports the narrative — not just philosophy or psychology, but also biology, neuroscience, social sciences, or domain-specific knowledge as needed.
6. In Next Steps, link to the next stage in the journey and to any related knowledge elsewhere in DevBook.
7. Keep the focus on the *experience* of the stage, not the theory behind it.
