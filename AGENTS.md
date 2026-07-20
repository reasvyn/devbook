# AGENTS.md — AI Agent Guide for DevBook

This file is the single source of truth for AI agents managing content in DevBook. Read it before any task execution.

## 1. Project Identity

DevBook is a Markdown-based learning library for developers. All content is in plain `.md` files — no build tools, no frameworks. Every document must be practical, concise, and developer-focused.

## 2. Core Constraints

| Constraint | Rule |
|---|---|
| **Language** | All content must be written in **English**. |
| **Register** | All content must be written in **professional, technical, academic English** — the register of scholarly papers, technical documentation, and peer-reviewed journals. Avoid colloquialisms, casual phrasing, contractions, and conversational tone. Prefer precise terminology over everyday language. Sentences should be structured, formal, and rigorous. |
| **Format** | Every document must follow the 9-section mandatory format (see below). |
| **Structure** | `{subject}/{module}/{submodule(optional),intro}/{short-description}.md` |
| **Indexes** | Every module, submodule, and `intro/` must have an `index.md`. The root also has an `index.md`. |
| **No orphans** | A content file must be referenced by its parent `index.md`. |
| **No build step** | Content is plain Markdown. Do not introduce tooling, bundlers, or generators. |
| **IT & education context** | Every document must be implicitly connected to IT (software, computing, technology) or education (learning, teaching, skill development). A topic must be framed in a way that relates to how developers learn, build, or grow. Pure content from unrelated fields (e.g., medicine, law, agriculture) must not appear unless it directly serves an IT or educational purpose. |
| **Christian worldview integration** | Every document must implicitly integrate Christian theological principles as a foundational worldview layer. This integration must be subtle, organic, and never explicit — woven into the narrative as a natural philosophical underpinning rather than presented as doctrine. See the [Christian Integration Approach](#3-christian-integration-approach) section for detailed guidance. |

## 3. Christian Integration Approach

Every document in DevBook must reflect a Christian worldview as its implicit philosophical foundation. This integration must never be overt, preachy, or doctrinal. The reader should absorb theological principles through the narrative without encountering explicit religious assertions.

### 3.A. Principles

- **Implicit over explicit.** Never state "the Bible says" or "Christianity teaches." Instead, embody the principle through the narrative voice, the values assumed, and the philosophical framing.
- **Natural, not forced.** Christian concepts should emerge organically from the subject matter. If an integration feels artificial or bolted on, it is poorly executed — revise until it flows as a natural extension of the topic.
- **Worldview, not evangelism.** The purpose is not to convert but to reflect a coherent Christian philosophical tradition. The documents operate from the assumption that creation is ordered, that humans bear inherent dignity, that redemption is possible, and that suffering can be purposeful — without ever stating these as propositions.
- **Universality of application.** The principles integrated should be universally resonant. Concepts like grace, forgiveness, hope, humility, stewardship, community, and redemption are accessible to all readers regardless of faith background.

### 3.B. Theological Concepts Mapped to Content Domains

| Content Domain | Theological Concept | How to Integrate |
|---|---|---|
| **Self-awareness & honesty** | Conviction and confession | Frame honest self-examination as an act of courage rooted in the belief that truth is redemptive, not punishing. The impulse toward truth reflects an order in creation that rewards alignment with reality. |
| **Forgiveness & self-compassion** | Grace and forgiveness | Present self-forgiveness not as indulgence but as participation in a grace that precedes performance. The assumption that one's worth is not contingent on achievement is a theological claim presented as psychological insight. |
| **Suffering & resilience** | Redemptive suffering | Frame suffering as potentially purposeful without being masochistic. The idea that endurance produces character, and character produces hope, is embedded in the narrative of growth through adversity. |
| **Community & support** | Bearing one another's burdens | Present relational support not merely as a psychological need but as a reflection of designed interdependence. The human person is constituted for communion, not isolation. |
| **Purpose & calling** | Stewardship and vocation | Frame purpose as something discovered through faithful attention to one's gifts and the world's needs, not merely constructed through willpower. Purpose is found, not invented. |
| **Hope** | Theological hope | Present hope not as optimism (which is circumstantial) but as a grounded trust in the meaningfulness of existence even when circumstances are dark. Hope as an anchor, not a feeling. |
| **Habits & discipline** | Formation and sanctification | Frame habit formation as a process of gradual transformation into one's intended design, not mere productivity optimization. The compounding of small faithful actions reflects an ordered moral universe. |
| **Mortality & meaning** | Eternal perspective | When discussing death, finitude, or legacy, carry the implicit conviction that temporal actions have eternal significance. Not as a doctrinal statement, but as a philosophical posture that gives weight to choices. |

### 3.C. Execution Guidelines

1. **During writing:** After completing a draft, review every section for opportunities to deepen the philosophical foundation through implicit Christian principles. Ask: "Does this section assume a universe where meaning is real, where persons are dignified, where redemption is possible?" If the answer is no, deepen the framing.
2. **During review:** Check that no integration is explicit or preachy. If a sentence could be read as proselytizing, rewrite it as narrative or philosophical reflection. The reader should feel the weight of the worldview, not be told about it.
3. **Tone test:** The integration should feel like the difference between a sermon and a well-written novel. A novel can embody a worldview profoundly without ever stating it. Aim for that register.

## 4. Directory Structure Rules

```
{subject}/
├── index.md
├── intro/                         ← subject-level intro (required)
│   ├── index.md                   ← lists all intro files
│   └── {short-description}.md
└── {module}/
    ├── index.md
    ├── intro/                     ← module-level intro (required)
    │   ├── index.md               ← lists all intro files
    │   └── {short-description}.md
    ├── {short-description}.md     ← content files (flat)
    └── {submodule}/               ← optional
        ├── index.md
        └── {short-description}.md
```

> The more modular your material, the better. Split large topics into smaller, focused files — each covering one coherent concept.

- **Subject** — top-level directory (e.g., `mathematics`, `networks`). Must match the Topics table in README.md.
- **Module** — grouping within a subject (e.g., `linear-algebra` inside `mathematics`). Every subject must consist of one or more modules.
- **Submodule** — optional grouping within a module (e.g., `vector-spaces` inside `linear-algebra`). A submodule cannot contain deeper submodules. Module and submodule names must not be the same.
- **Must be a real branch of knowledge.** Subjects, modules, and submodules must represent established fields of study or practice (e.g., mathematics, linear algebra, vector spaces). Do not create entities for job roles, positions, or personas (e.g., `ceo-founders`, `frontend-developers`). Role-based profession explorations belong in `careers/`, not in content modules.
- **`intro/`** — a special directory containing background, philosophy, principles, history, ethics, key events, or official organizations about the field. Every subject and module **must** have an `intro/` directory. Every `intro/` must have an `index.md` listing its files.
- **Short description** — hyphenated slug, lowercase (e.g., `why-math.md`, `vector-operations.md`).
- Content files sit directly under the module or submodule (flat). `intro/` is the only directory allowed at these levels.

### 4.A. Careers Directory

The `careers/` directory follows the same subject/module convention as other top-level directories. Each career is a module that explores a profession in depth — what the role entails, its responsibilities, specializations, career progression, and industry context. These documents discuss the *profession itself*, not what someone should learn to enter it.

- Structure: `{career}/{module}/{short-description}.md`
- Each career module has an `index.md`, an `intro/` with background content, and content files covering different aspects of the profession.
- Content files follow the 9-section mandatory format.
- Career modules are subjects of study about professions, not learning paths for individuals.
- Unlike the old `roadmaps/` directory, careers are not organized by experience level — they are standalone profession profiles.

## 5. Index System

Every `index.md` is a navigation hub. The format depends on the index level.

### 5.A. Index Levels

- **Root `/index.md`** — A categorized list of all subjects. No learning path — subjects are independent.
- **Subject `subject/index.md`** — A categorized list of modules. If modules have a natural progression (e.g., Fundamentals → Advanced), use numbered phase headings. Otherwise, use a flat list.
- **Module `module/index.md`** — A learning path tree with phases and optional branches. The index itself IS the learning path — numbered lists for sequential progression, labeled branches for diverging paths.
- **Submodule `submodule/index.md`** — Same format as module index — learning path tree if content has dependencies, flat list if independent.
- **`intro/index.md`** — A flat list of intro files. No learning path — intro files are background reading, not sequential.

### 5.B. The 4-Level Index Chain

Every content file must be reachable from the root `index.md` through four levels of navigation:

```
Level 1  Master (root)     /index.md
                             └── [Subject](subject/index.md)
Level 2  Subject           subject/index.md
                             └── [Module](subject/module/index.md)
Level 3  Module            subject/module/index.md
                             ├── [Intro](subject/module/intro/index.md)
                             └── [Submodule](subject/module/submodule/index.md)  (optional)
Level 4  Submodule/Intro   subject/module/intro/index.md
                            subject/module/submodule/index.md
                             └── {short-description}.md
```

Each level must link **down** to the next. A content file is only considered published when it appears in all four levels:

| Level | File | Links To |
|---|---|---|
| 1 | `/index.md` | `subject/index.md` |
| 2 | `subject/index.md` | `subject/module/index.md` |
| 3 | `subject/module/index.md` | `subject/module/intro/index.md` or `subject/module/submodule/index.md` |
| 4 | `subject/module/intro/index.md` or `subject/module/submodule/index.md` | actual `.md` content files |

A missing link at any level means the content is orphaned. Always verify the full chain when creating or moving files.

### 5.C. Rules for Index Files

- **Every directory must be listed.** If `linear-algebra/vectors-and-matrices/` has a file, `vectors-and-matrices/index.md` must link to it.
- **Index as planning tool.** An `index.md` may list files that do not yet exist by appending `(planned)` to the link text. This marks the topic as planned but not yet created. Planned entries must **not** have a link (the file does not exist yet). Once the file is written, add the link and remove the `(planned)` label.
- **Do not list directories** — list actual `.md` files.
- **Use relative paths** only. Never absolute or full URLs for internal links.
- **Module indexes use learning path trees.** Use numbered lists for sequential progression, labeled branches for diverging paths, and phase headings for major transitions.
- **Subject and root indexes use flat lists.** Learning path trees apply to module-level indexes where content has clear dependencies.
- **Every list must be under a heading.** Content list items (`1. [File](path.md)`) must never appear without a preceding `## ` heading. Every group of content links belongs under a named section. No orphaned lists.

### 5.D. Heading Rules for Module and Submodule Indexes

These rules apply to every `index.md` inside a module or submodule directory (not `intro/`, not root, not subject-level).

#### 5.D.1. Mandatory Structure

1. `# Title` — the module or submodule name.
2. `## 1. Introduction` — always the first section. Contains a bullet list linking to the intro file(s).
3. `## 2. ...` through `## N. ...` — content sections forming the learning path. Each section is a numbered phase.

#### 5.D.2. `## 1. Introduction` Is Always First

Every module and submodule `index.md` must begin its learning path with `## 1. Introduction`. This section contains a single bullet list linking to the intro file(s). No content links appear here — only intro links.

#### 5.D.3. Every `##` Heading Must Be Descriptive

A heading must describe **what the reader learns or does** in that phase. It must function as a signpost in the learning path — the reader should understand the phase's purpose from the heading alone.

**Forbidden headings** (too generic to be useful):

| Forbidden | Why |
|---|---|
| `## 2. Core` | Says nothing about what "core" means in this context |
| `## 2. Topics` | Every section is topics — this is meaningless |
| `## 2. Content` | Same as above |
| `## 2. Modules` | Module-level indexes should not use this label |
| `## 2. Basics` | Too vague — basics of what? |
| `## 2. Advanced` | Too vague — advanced what? |

**Required pattern:** The heading must name the specific domain, skill, or conceptual territory covered. Examples:

| Module | Bad heading | Good heading |
|---|---|---|
| `mathematics/calculus` | `## 2. Topics` | `## 2. Analysis of Change` |
| `mathematics/linear-algebra` | `## 2. Topics` | `## 2. Vectors, Matrices & Decompositions` |
| `physics/quantum-mechanics` | `## 2. Topics (Planned)` | `## 2. Quantum States & Computation (Planned)` |
| `programming/fundamentals` | `## 2. Core` | `## 2. Language Fundamentals` |
| `data-databases/sql` | `## 2. Core` | `## 2. Querying, Joining & Designing` |
| `english/professional-communication` | `## 2. Core` | `## 2. Engineering Communication Practices` |
| `level-up/meaning` | `## 2. Core` | `## 2. Recognizing the Need for Change` |

#### 5.D.4. No Heading May Group Unrelated Topics

A single `##` section must contain a **coherent, focused cluster** of related content. If the items under a heading belong to distinct conceptual territories, split them into separate numbered sections.

**Decision rule:** Ask "Could a reader complete this section and have a self-contained understanding of one topic?" If the answer is no because the items cover unrelated domains, the heading is too broad.

**Example — too broad:**

```markdown
## 2. Core

1. [Querying Data](querying-data.md)
2. [SQL Functions](sql-functions.md)
3. [Joins & Relationships](joins-and-relationships.md)
4. [Normalization](normalization.md)
5. [Database Design](database-design.md)
```

**Example — properly split:**

```markdown
## 2. Retrieving & Manipulating Data

1. [Querying Data](querying-data.md)
2. [SQL Functions](sql-functions.md)
3. [Joins & Relationships](joins-and-relationships.md)

## 3. Modeling & Designing Databases

1. [Normalization](normalization.md)
2. [Database Design](database-design.md)
```

#### 5.D.5. Heading Sequence Must Reflect Learning Progression

The numbered `##` sections should follow a logical order: foundation before application, theory before practice, simple before complex. The heading labels should make this progression obvious.

**Good progression pattern:**

```markdown
## 1. Introduction
## 2. Foundations of X        ← prerequisites, core concepts
## 3. Techniques & Practice   ← applied skills, methods
## 4. Integration & Mastery   ← combining skills, real-world use
```

**Bad progression pattern** (headings reveal no structure):

```markdown
## 1. Introduction
## 2. Core
## 3. More Stuff
## 4. Advanced
```

#### 5.D.6. `###` Branches Within a Section

When a `##` section has more than 3 items that naturally subdivide, use `### N.A.`, `### N.B.` branches. Branches must also have descriptive names.

**Example:**

```markdown
## 3. Probability & Distributions

### 3.A. Discrete Distributions

1. Bernoulli & Binomial
2. Poisson

### 3.B. Continuous Distributions

1. Normal & Gaussian
2. Exponential
```

#### 5.D.7. "(Planned)" Suffix for Unbuilt Sections

If all items in a section are planned (no files exist yet), append `(Planned)` to the heading. Do not append `(Planned)` to individual items that exist as real files.

```markdown
## 2. Newtonian Mechanics & Rigid Bodies (Planned)

- Kinematics (planned)
- Newton's Laws (planned)
```

#### 5.D.8. No Generic Fallbacks

If you cannot find a descriptive heading for a section, the section likely needs to be reorganized — not labeled generically. Every heading is a promise to the reader about what they will learn.

## 6. Mandatory Document Format

Every content `.md` file must follow this exact structure, in order:

```markdown
# Title

## Description

Brief overview — what this covers and why a developer should care. 1–3 sentences.

## Prerequisites

- [Concept A](relative/path.md) — what the reader should know first
- [Concept B](relative/path.md)

Index files are valid prerequisite targets when the reader must understand the entire subject, module, or submodule. Example: a document about distributed consensus might list `[Networks](networks/index.md)` because it assumes mastery of all networking concepts.

## Table of Contents

- [Section 1](#section-1)
- [Section 2](#section-2)

## Content / Material

The core material. Use headings, paragraphs, code blocks, diagrams (Mermaid), and math (LaTeX `$$`) as needed. Be concise but complete. Integrate real-world scenarios, examples, and walkthroughs directly into this section rather than separating them.

## Learning Tips (Optional)

Practical advice for studying and retaining the material. Common pitfalls to avoid, memory aids, practice strategies, or ways to apply the concepts.

## Glossary

| Term | Definition |
|------|------------|
| Term | One-line definition |

## Quick References (Optional)

External resources — journals, books, blog posts, websites. Every link must be verified and accessible before inclusion.

- [Title](URL) — one-line description
- [Title](URL)

## Next Steps

Where to go next. Link to related documents or suggest practice exercises.

- [Related Topic A](relative/path.md)
- [Related Topic B](relative/path.md)
```

> Do not skip, reorder, or rename sections. Sections marked "(Optional)" may be omitted if genuinely not needed. Quick References items must be verified — every link must be valid and accessible before inclusion.

## 7. Content Generation Rules

### 7.A. Do's

- **Be practical.** Connect every concept to how it matters in real development.
- **Use code when it clarifies.** A well-chosen snippet can replace a paragraph of explanation. Use language-identified fenced blocks in popular languages (Python, JavaScript, TypeScript, Go, Rust, etc.) — but only when a code example genuinely adds clarity. Do not force code into topics where prose or diagrams communicate better.
- **Define terms** on first use, then add them to the Glossary.
- **Link internally** to prerequisites and next steps. Cross-link between related topics.
- **Use Mermaid** for diagrams (flowcharts, sequence diagrams, graphs).
- **Use LaTeX** (`$` inline, `$$` display) for math.
- **Keep files focused.** A single document should cover one coherent topic. Split if it grows too long.
- **Line count 400–800.** Every content file must be at least 400 lines. If shorter, expand with more depth, examples, or diagrams directly in the Content section. If longer than 800 lines, split into multiple focused documents (e.g., a related sub-topic) and link them via Next Steps.
- **RPG-like learning experience.** Structure content as a quest-based journey. The Description sets the mission, the Prerequisites define what you need before attempting, the Content is the challenge, the Learning Tips and Glossary are the rewards, and the Next Steps point to the next quest. The module index is the world map. Cross-references are the skill tree. This framing makes progression feel natural and rewarding — but never sacrifice scientific accuracy for the sake of the metaphor. All mandatory section headings remain unchanged.

### 7.B. Don'ts

- **No fluff.** No "in this article we will learn", "welcome to", or "let's dive in".
- **No assumptions about tooling.** Do not assume the reader uses a specific OS, editor, or package manager unless the topic requires it.
- **No external dependencies.** Do not reference npm packages, libraries, or frameworks that the document does not explain.
- **No placeholder text.** Every section must have real content. If a topic is genuinely too short for a section, omit it rather than fill with filler.
- **No absolute internal links.** Use `../` relative paths only.
- **No generated placeholders like `[TODO]`** — if content cannot be written now, do not create the file.

### 7.C. Emoji Usage (Recommended)

Emoji usage is **strongly recommended** across all content files to enhance readability, visual appeal, and engagement. Emojis serve as visual anchors that help readers scan sections, identify themes, and maintain interest during long-form reading.

**Placement rules:**

- **Section headings** — Place a single emoji before or after the heading text to signal the section's theme. Examples: `## 🧠 Cognitive Frameworks`, `## 3. Sleep Architecture 🌙`, `## ⚠️ Common Pitfalls`.
- **List items** — Use emojis to differentiate list categories or mark priority levels. Examples: `🟢 Low impact`, `🟡 Medium impact`, `🔴 High impact`.
- **Callout blocks** — Use emojis to signal the type of information. Examples: `💡 Key Insight`, `🔬 Scientific Basis`, `🛠️ Practical Exercise`, `📚 Reference`.
- **Table headers** — Use emojis in the first column to visually distinguish row categories.

**Style rules:**

- **One emoji per heading or list item.** Never stack multiple emojis (e.g., avoid `🧠🔬💡`).
- **Relevance over decoration.** Every emoji must thematically match its content. Use `💤` for sleep, `🍎` for nutrition, `🏋️` for exercise — not random cheerful symbols.
- **Consistency within a file.** If a file uses `🌙` for sleep-related sections, do not switch to `😴` elsewhere in the same document.
- **Professional tone preserved.** Emojis supplement the text; they do not replace words or diminish the academic register. The writing remains formal and rigorous — emojis are visual signposts, not personality markers.
- **No emojis in code blocks or file paths.** Emoji usage applies to human-readable prose only.
- **Choose freely.** There is no fixed emoji palette. Select emojis that naturally fit the content and tone of each document.

## 8. Workflows

### 8.A. Creating New Content

Follow these steps in order:

1. **Check existing content** — Read the parent `index.md` and sibling files to understand what already exists. Avoid duplication.
2. **Determine placement** —
   ```
   mathematics/           ← subject (check README Topics table)
   └── calculus/          ← module (create if needed)
       └── derivatives/   ← submodule (create if needed) or `intro/`
           └── chain-rule.md  ← slug
   ```
3. **Create directory structure** — Create all missing parent directories first.
4. **Write the content file** — Follow the mandatory document format. Write the full `.md` file.
5. **Update indexes** —
   - Create or update the parent submodule `index.md` to include a link to the new file.
   - If the submodule `index.md` did not exist, create it and update the module `index.md` to link to it.
   - If the module `index.md` did not exist, create it and update the subject `index.md`.
   - If the subject `index.md` did not exist, create it and update the root `index.md`.
   - If the file is in `intro/`, update or create `intro/index.md` to include the new link.
6. **Verify** —
   - The new file is reachable from root by following `index.md` links.
   - All internal links in the new file resolve to existing files.
   - The file follows the mandatory format completely.

### 8.B. Updating Existing Content

1. Read the file and its parent `index.md`.
2. Make the minimum change needed.
3. If the file's slug or path changes, update all links pointing to it (check `grep -r "old-path"`).
4. If the file is removed, remove its entry from the parent `index.md`.

## 9. Quality Checklist

Before considering a task complete, verify:

- [ ] All content is in **English**.
- [ ] All content uses **professional, technical, academic English** — no colloquialisms, contractions, or conversational tone.
- [ ] The **mandatory format** is followed exactly (all required sections present, in order).
- [ ] **Title** matches the file's topic — not generic.
- [ ] **Description** explains why a developer should care.
- [ ] **Prerequisites** links exist and point to real files.
- [ ] **Table of Contents** matches the actual section headings.
- [ ] **Content** is substantive, practical, and code-rich where applicable.
- [ ] **Line count 400–800** — every content file meets the minimum and is not too long.
- [ ] **Glossary** defines every non-trivial term introduced.
- [ ] **Quick References** items (if present) are verified — all external links are valid and accessible.
- [ ] **Next Steps** link to existing related documents.
- [ ] **index.md** files are updated — every new file is linked from its parent index.
- [ ] **Directory structure** follows the convention exactly.
- [ ] **No broken links** — all relative paths resolve.
- [ ] **No placeholder content** — no `TODO`, `FIXME`, or empty sections.
- [ ] **Christian worldview integration** — theological principles are implicitly woven into the narrative (never explicit or preachy).
