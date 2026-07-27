# Content Writing — Format Rules

This file contains the format-specific rules for content writing. The full document template is in [TEMPLATE.md](../../../../TEMPLATE.md). The complete rule set is in [CONTENT-RULES.md](../../../../CONTENT-RULES.md).

## Mandatory Format

Every content `.md` file must follow the 9-section structure defined in [TEMPLATE.md](../../../../TEMPLATE.md). Do not skip, reorder, or rename sections.

## Critical Rules

- **Language: English only.** Never write content in any other language.
- **Register: Academic English.** No colloquialisms, contractions, or conversational tone. **Exception:** `level-up/` uses literary nonfiction — see [narrative-rules.md](../../leveling-up/rules/narrative-rules.md).
- **Line count 400–800.** If too short, expand with more depth in the Content section. If too long, first try trimming redundant or non-essential content. If the topic is genuinely complex and cannot be shortened, split into multiple focused documents and link them via Next Steps.
- **Two-phase writing workflow.** When creating multiple new files, write ALL files first (accepting ~150-line truncation), THEN expand each to 400–800 lines. See [SKILL.md](../SKILL.md#two-phase-writing-workflow) for details. Never write-and-expand one file at a time.
- **No orphans.** Every content file must be referenced by its parent index file. Verify the full index chain: root → subject → module/submodule → file.
- **No placeholder text.** No `TODO`, `FIXME`, `[planned]`, or empty sections. Write real content or omit the section.
- **No fluff.** No "in this article", "welcome to", "let's dive in". Get straight to the material.
- **Code when it clarifies.** Use language-identified fenced code blocks in popular languages (Python, JavaScript, TypeScript, Go, Rust, etc.) when a code example genuinely adds clarity.
- **Define terms on first use**, then add them to the Glossary.
- **Use relative paths only** for internal links. Never absolute URLs for internal content.
- **Mermaid for diagrams**, LaTeX (`$`/`$$`) for math.
- **Cross-link** between related topics in Prerequisites and Next Steps.
- **RPG-like learning experience.** Structure content as a quest-based journey — Description as mission, Prerequisites as requirements, Content as challenge, Learning Tips and Glossary as rewards, Next Steps as the next quest.
- **Tiering.** If the topic spans multiple complexity levels, split into at most 3 tiered files: `{topic}-basic.md`, `{topic}-intermediate.md`, `{topic}-advanced.md`. Not all topics need all three.
- **Emoji usage.** Use emojis naturally throughout content — in headings, lists, callouts, tables, and within prose where they add visual clarity. One emoji per item, keep them relevant and consistent within a file.

## Section-Specific Rules

### Description
- 1–3 sentences maximum.
- Answer: "Why should I keep reading?"
- No filler — get to the value proposition.

### Prerequisites
- Every prerequisite must link to an existing file using relative paths.
- Index files are valid prerequisite targets when the reader must understand the entire subject, module, or submodule.
- Order prerequisites by importance — most critical first.

### Content / Material
- Integrate real-world scenarios, examples, and walkthroughs directly.
- Use headings to create a logical flow within the section.
- Code blocks must have language identifiers.
- Diagrams should use Mermaid syntax.
- Math should use LaTeX (`$` for inline, `$$` for display).

### Glossary
- Define every non-trivial term introduced in the document.
- One-line definitions only — keep it scannable.
- Alphabetical order recommended but not required.

### Quick References
- Every link must be verified and accessible before inclusion.
- One-line description per link.
- Prefer primary sources (official docs, specifications, academic papers).

### Next Steps
- Mandatory — every document must point the reader forward.
- Link to related documents, not generic "read more" suggestions.
- Suggest practice exercises when applicable.
