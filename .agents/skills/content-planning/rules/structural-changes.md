# Content Planning — Structural Changes

This file defines the workflow for creating, moving, or removing modules, submodules, or reorganizing the index structure. The full planning workflow is in the parent [SKILL.md](../SKILL.md).

## Creating a New Module or Submodule

1. **Index-First Workflow** — Read the full index chain (root → subject → all modules).
2. **Justification** — Answer these questions in writing:
   - What specific body of knowledge does this represent?
   - Why is it a distinct unit of study rather than a section within a broader module?
   - Is it a valid academic discipline? (Exception: narrative and career-exploration subjects are not academic disciplines but are valid subjects.)
   - What must the reader know before entering this module?
   - What does this module unlock afterward?
3. **Phase determination** — Assign the new module to a learning phase in the subject index.
4. **Relationship mapping** — Identify prerequisites, siblings, and extensions among existing modules.
5. **Draft the module `index.md`** with:
   - A `# Title` matching the module name.
   - A `## 1. Introduction` section linking to the `intro/` file.
   - `## 2. ...` sections forming the initial learning path (mark unplanned entries as `(planned)`).
6. **Draft the `intro/` file** with the justification paragraph from step 2.
7. **Update the parent index** — add a link to the new module in the correct phase.
8. **Verify the 4-level chain** — root → subject → module → content. Every level must link down.

## Reorganizing Existing Content

1. **Index-First Workflow** — Read the full index chain to understand all affected files.
2. **Document current state** — List all files that will move and their current paths.
3. **Map old paths to new paths** — Every move must be tracked.
4. **Update every index** that references moved files.
5. **Grep for broken links** — Search for every old path across all `.md` files.
6. **Verify the 4-level chain** still holds after changes.

## Proposing Index Restructuring

1. **Read the full index tree** — root → all subjects → all modules.
2. **Document the current phase assignments** — which subjects/modules are in which phases.
3. **Propose the new phase assignments** with justification for each move.
4. **Identify cascade effects** — does moving one subject affect the coherence of its source and destination phases?
5. **Preserve vertical order** — when splitting or merging, the module `index.md` must continue the progression the phases imply.

## Removing a Module or Submodule

1. **Verify no content will be orphaned.** Check that all files within the module have alternative homes or are intentionally being deleted.
2. **Update all indexes** that link to the removed module.
3. **Grep for broken links** — Search for every path within the removed module across all `.md` files.
4. **Update Prerequisites and Next Steps** in files that referenced the removed module.
