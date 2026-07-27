# Content Planning — Structural Changes

This file defines the workflow for creating, moving, or removing modules, submodules, or reorganizing the index structure. The full planning workflow is in the parent [SKILL.md](../SKILL.md).

## Creating a New Module or Submodule

**Before proceeding, exhaust existing options.** A new module must NOT be created just because content is "related" or "convenient to group." A module is a learning dependency boundary, not a folder. The mandatory sequence is:

0. **Search existing content.** Use glob and grep to find any file that already covers the topic. Check every subject's `intro/` directory — biographical, historical, and contextual content belongs there. Check `foundations/` — introductory content belongs there. If existing content can be expanded to include the new material, expand it. Do NOT create a new module.
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

## Renaming a Module or Submodule

Module and submodule names may be renamed when the current name **does not accurately reflect the content**. Subject names are **never renamed**. See [CONTENT-RULES.md](../../../CONTENT-RULES.md#module--submodule-renaming) for justification criteria.

**Before proceeding, confirm the rename is justified.** A rename is NOT a reorganization. Renaming changes the label, not the position. If the content needs to move to a different module, that is reorganization, not renaming.

### Justification Requirements

The agent must document the following in writing before executing the rename:

1. **Current name and its inadequacy.** What does the current name suggest the module contains? What does it actually contain? Where is the mismatch?
2. **Proposed name and its accuracy.** What does the proposed name suggest? How does this match the actual content?
3. **Evidence of mismatch.** Reference specific files, sections, or content within the module that the current name fails to describe.
4. **Why not reorganization?** Confirm that the content belongs in this module — only the name needs to change.

### Workflow

1. **Index-First Workflow** — Read the full index chain (root → subject → all modules) to understand all affected paths.
2. **Document the mismatch** — Write the justification from the requirements above.
3. **Verify no naming collision** — Use `ls` on the parent subject directory to confirm the proposed name does not already exist as a module or submodule within the same subject.
4. **Rename the directory** — `git mv old-name new-name`.
5. **Update the parent `index.md`** — Change both the link text and the link path to reflect the new name.
6. **Update all cross-references** — Search for every occurrence of the old directory name across all `.md` files:
   ```bash
   grep -r "old-name" --include="*.md" .
   ```
   Update every link found. Do not skip any.
7. **Update internal file references** — If the module's own `index.md` or content files contain the old name in headings, titles, or prose, update them.
8. **Verify the 4-level chain** — Root → subject → module → content must still hold.
9. **Verify no broken links** — All relative paths must resolve correctly. Run the link verification script or manually check every link in every affected file.

### Example Justification

**Current name:** `emotional-intelligence`
**Proposed name:** `emotion-regulation`

**Justification:** The module contains six content files. Five of six focus specifically on regulation strategies (cognitive reappraisal, distress tolerance, impulse control, arousal management, and recovery techniques). Only one file covers emotional recognition (a sub-skill of regulation). The name "emotional intelligence" suggests a broader scope (including social awareness, relationship management, and empathy) that the module does not cover. The name "emotion-regulation" accurately describes the module's actual content: the practical techniques for managing emotional responses in professional contexts.

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
