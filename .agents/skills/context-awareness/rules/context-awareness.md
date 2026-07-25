# Context Awareness Rules

These rules define how the agent maintains correct context throughout every session. They are always active and apply to every task regardless of type.

## Core Rules

### 1. Read Before Acting

Never act without first reading the relevant files. This includes:
- Project rules (`AGENTS.md`, `CONTENT-RULES.md`, `TEMPLATE.md`)
- Index files (root, subject, module)
- Files that will be modified
- Files that will be linked to

### 2. Verify Before Assuming

Never assume file existence, file content, directory structure, or naming conventions. Always verify with `read`, `glob`, `ls`, or `grep`.

### 3. Follow the Index-First Workflow

The Index-First Workflow is mandatory for every task. Read the full index chain before any action. No steps may be skipped.

### 4. Cross-Reference Before Linking

Before creating any internal link, verify the target exists and the path is correct. Never create links from memory.

### 5. Preserve Existing Structure

When modifying files, preserve the existing structure unless the task explicitly requires changing it. Make the minimum change needed.

### 6. Avoid Stale Information

Do not include information that may be outdated or that has not been verified in the current session. Read the current state before making claims.

### 7. Maintain Session Continuity

At the end of a session, summarize what was done and what remains. This supports continuity for future sessions.

## Anti-Patterns

| Pattern | Problem | Solution |
|---------|---------|----------|
| Assumption without verification | Relying on memory instead of reading | Read the file before referencing it |
| Path construction from memory | Guessing file paths | Use `glob` or `ls` to verify paths |
| Link creation without verification | Creating broken links | Read the target before linking |
| Skipping Index-First Workflow | Working without context | Read the full index chain first |
| Relying on prior session knowledge | Using stale information | Read the current state in this session |
| Modifying without reading | Changing files without understanding them | Read the file before modifying |

## Integration

This skill integrates with all other skills by ensuring the foundational context is established before any skill-specific workflow begins. Every skill's Index-First Workflow, quality checklist, and critical rules depend on context awareness being maintained.
