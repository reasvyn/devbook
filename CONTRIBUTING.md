# Contributing to DevBook

Thanks for your interest in contributing! DevBook is a community-driven resource, and every improvement — whether it's fixing a typo, adding a new topic, or refining an existing explanation — is welcome.

## Table of Contents

- [Types of Contributions](#types-of-contributions)
- [Getting Started](#getting-started)
- [Quick Reference](#quick-reference)
- [Writing Guidelines](#writing-guidelines)
- [Pull Request Process](#pull-request-process)
- [Code of Conduct](#code-of-conduct)

## Types of Contributions

| Type | Description |
|------|-------------|
| **Fix** | Correct an error, improve clarity, or update outdated information |
| **Expand** | Add missing subtopics, examples, or diagrams to an existing document |
| **New topic** | Propose and write a new subject area under an existing category |
| **New category** | Suggest a new top-level directory (e.g., Blockchain, DevOps) |
| **Review** | Read existing content and open issues with suggestions |

## Getting Started

1. **Read the rules.** Before writing anything, read these documents in order:
   - [CONTENT-RULES.md](CONTENT-RULES.md) — All rules, conventions, and requirements
   - [TEMPLATE.md](TEMPLATE.md) — The mandatory document format
   - [AGENTS.md](AGENTS.md) — The Index-First Workflow (you must understand the index before creating content)

2. **Understand the index.** Read the root [index.md](index.md) to understand the learning path. Then read the subject and module indexes for the area you want to contribute to.

3. **Fork and branch:**
   ```bash
   git clone https://github.com/your-username/devbook.git
   cd devbook
   git checkout -b fix/quick-description    # for fixes
   git checkout -b topic/your-topic         # for new content
   ```

4. **Write your content** following the mandatory format in [TEMPLATE.md](TEMPLATE.md).

5. **Commit and open a pull request.**

## Quick Reference

| Rule | Value |
|------|-------|
| **Language** | English only, academic register |
| **Format** | 9-section mandatory format ([TEMPLATE.md](TEMPLATE.md)) |
| **Line count** | 400–800 lines per content file |
| **Directory** | `{subject}/{module}/{submodule(optional),intro}/{short-description}.md` |
| **File naming** | Hyphenated slugs, lowercase (e.g., `vector-operations.md`) |
| **Tiered files** | `{topic}-basic.md`, `{topic}-intermediate.md`, `{topic}-advanced.md` |
| **Indexes** | Every directory must have an `index.md` |
| **Links** | Relative paths only for internal content |
| **Code** | Fenced blocks with language identifiers |

## Writing Guidelines

- **Be concise.** Developers have limited time — get to the point.
- **Prefer code over prose.** A short snippet is worth a paragraph of explanation.
- **Explain *why*.** Don't just state facts; connect them to real-world development.
- **Assume a technical audience.** Basic programming literacy is expected.
- **Minimize prerequisites.** Link to prior knowledge rather than re-explaining.
- **Skip fluff.** No "in this article we will learn" — just teach.
- **Follow the Index-First Workflow.** Read the index chain before writing. Understand where your content fits in the learning path.

For the complete rule set, see [CONTENT-RULES.md](CONTENT-RULES.md).

## Pull Request Process

1. Ensure your branch is up to date with the main branch.
2. Verify that any new files follow the directory naming convention (lowercase, hyphen-separated).
3. Verify the new file is reachable from root through the index chain.
4. Open a pull request with a descriptive title and summary of changes.
5. Maintainers will review and may request edits before merging.
6. Squash commits on merge to keep history clean.

## Code of Conduct

Be respectful, constructive, and patient. This is a learning resource first — every contribution should make knowledge more accessible, not less.
