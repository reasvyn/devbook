# Content Rules & Conventions

This document is the single source of truth for all content rules, writing conventions, and structural requirements in DevBook. Read it before writing, editing, or reviewing any content file.

For the document template, see [TEMPLATE.md](TEMPLATE.md). For AI agent workflows, see [AGENTS.md](AGENTS.md). For contributor guidelines, see [CONTRIBUTING.md](CONTRIBUTING.md).

## Table of Contents

- [Core Constraints](#core-constraints)
- [IT & Education Context](#it--education-context)
- [Christian Worldview Integration](#christian-worldview-integration)
- [Language & Register](#language--register)
- [Content Tiering: Basic, Intermediate, Advanced](#content-tiering-basic-intermediate-advanced)
- [Directory Structure](#directory-structure)
- [Index System](#index-system)
- [Content Generation Rules](#content-generation-rules)
- [Emoji Usage](#emoji-usage)
- [Style Guide](#style-guide)
- [Line Count & Document Length](#line-count--document-length)
- [Quality Checklist](#quality-checklist)

---

## Core Constraints

| Constraint | Rule |
|---|---|
| **Language** | All content must be written in **English**. |
| **Register** | Professional, technical, academic English — the register of scholarly papers, technical documentation, and peer-reviewed journals. No colloquialisms, casual phrasing, contractions, or conversational tone. |
| **Format** | Every document must follow the 9-section mandatory format defined in [TEMPLATE.md](TEMPLATE.md). |
| **Structure** | `{subject}/{module}/{submodule(optional),intro}/{short-description}.md` |
| **Indexes** | Every module, submodule, and `intro/` must have an `index.md`. The root also has an `index.md`. |
| **No orphans** | A content file must be referenced by its parent `index.md`. |
| **No build step** | Content is plain Markdown. Do not introduce tooling, bundlers, or generators. |
| **Line count** | Every content file must be 400–800 lines. See [Line Count & Document Length](#line-count--document-length). |

---

## IT & Education Context

Every document must be implicitly connected to IT (software, computing, technology) or education (learning, teaching, skill development). A topic must be framed in a way that relates to how developers learn, build, or grow. Pure content from unrelated fields (e.g., medicine, law, agriculture) must not appear unless it directly serves an IT or educational purpose.

---

## Christian Worldview Integration

Every document must implicitly integrate Christian theological principles as a foundational worldview layer. This integration must be subtle, organic, and never explicit — woven into the narrative as a natural philosophical underpinning rather than presented as doctrine.

### Principles

- **Implicit over explicit.** Never state "the Bible says" or "Christianity teaches." Instead, embody the principle through the narrative voice, the values assumed, and the philosophical framing.
- **Natural, not forced.** Christian concepts should emerge organically from the subject matter. If an integration feels artificial or bolted on, it is poorly executed — revise until it flows as a natural extension of the topic.
- **Worldview, not evangelism.** The purpose is not to convert but to reflect a coherent Christian philosophical tradition. The documents operate from the assumption that creation is ordered, that humans bear inherent dignity, that redemption is possible, and that suffering can be purposeful — without ever stating these as propositions.
- **Universality of application.** The principles integrated should be universally resonant. Concepts like grace, forgiveness, hope, humility, stewardship, community, and redemption are accessible to all readers regardless of faith background.

### Theological Concepts Mapped to Content Domains

| Content Domain | Theological Concept | How to Integrate |
|---|---|---|
| **Self-awareness & honesty** | Conviction and confession | Frame honest self-examination as an act of courage rooted in the belief that truth is redemptive, not punishing. |
| **Forgiveness & self-compassion** | Grace and forgiveness | Present self-forgiveness not as indulgence but as participation in a grace that precedes performance. |
| **Suffering & resilience** | Redemptive suffering | Frame suffering as potentially purposeful without being masochistic. Endurance produces character, and character produces hope. |
| **Community & support** | Bearing one another's burdens | Present relational support as a reflection of designed interdependence. The human person is constituted for communion, not isolation. |
| **Purpose & calling** | Stewardship and vocation | Frame purpose as something discovered through faithful attention to one's gifts and the world's needs, not constructed through willpower. |
| **Hope** | Theological hope | Present hope not as optimism but as a grounded trust in the meaningfulness of existence even when circumstances are dark. |
| **Habits & discipline** | Formation and sanctification | Frame habit formation as gradual transformation into one's intended design, not mere productivity optimization. |
| **Mortality & meaning** | Eternal perspective | When discussing death, finitude, or legacy, carry the implicit conviction that temporal actions have eternal significance. |

### Execution Guidelines

1. **During writing:** After completing a draft, review every section for opportunities to deepen the philosophical foundation. Ask: "Does this section assume a universe where meaning is real, where persons are dignified, where redemption is possible?" If the answer is no, deepen the framing.
2. **During review:** Check that no integration is explicit or preachy. If a sentence could be read as proselytizing, rewrite it as narrative or philosophical reflection.
3. **Tone test:** The integration should feel like the difference between a sermon and a well-written novel. A novel can embody a worldview profoundly without ever stating it. Aim for that register.

---

## Language & Register

All content must be written in **professional, technical, academic English**. This is the register of scholarly papers, technical documentation, and peer-reviewed journals.

**Forbidden:**
- Colloquialisms ("gonna", "wanna", "pretty cool")
- Casual phrasing ("let's dive in", "basically", "stuff")
- Contractions ("don't", "can't", "it's")
- Conversational tone ("you know", "right?", "so basically")

**Required:**
- Precise terminology over everyday language
- Structured, formal, and rigorous sentences
- Define technical terms on first use

---

## Content Tiering: Basic, Intermediate, Advanced

Topics that span multiple complexity levels **must** be split into at most three tiers. Use one, two, or three tiers as the topic demands — but never more than three.

| Tier | File suffix | Audience | Depth |
|------|-------------|----------|-------|
| **Basic** | `-basic.md` | Beginners with no prior knowledge | Definitions, core concepts, simple examples |
| **Intermediate** | `-intermediate.md` | Developers with basic understanding | Applied techniques, real-world patterns, trade-offs |
| **Advanced** | `-advanced.md` | Developers with working knowledge | Architecture decisions, edge cases, optimization, integration |

### Tier Rules

1. **Maximum three tiers.** Use as many as needed — one, two, or three — but never more than three. A topic that fits in a single 400–800 line file does not need tiering.
2. **Each tier is a standalone document.** A reader can read only the tier they need without depending on other tiers.
3. **Prerequisites chain upward.** Intermediate lists basic as prerequisite. Advanced lists intermediate.
4. **Not all topics require tiering.** Simple topics that fit within 400–800 lines at a single level do not need to be split. Tiering is required only when a topic naturally spans multiple complexity levels.
5. **Tier suffix goes in the filename, not the title.** The `# Title` inside the file reads `# Arithmetic`, not `# Arithmetic (Basic)`. The tier is indicated by the filename and by context in the module index.

### Examples

```
arithmetic-basic.md          ← what is addition, subtraction, order of operations
arithmetic-intermediate.md   ← fractions, decimals, percentages, ratios
arithmetic-advanced.md       ← combinatorics, number theory applications in CS
```

### Index Integration

Module indexes organize tiered content under descriptive phase headings:

```markdown
## 2. Arithmetic Foundations

1. [Arithmetic — Basic](arithmetic-basic.md)
2. [Arithmetic — Intermediate](arithmetic-intermediate.md)

## 3. Advanced Mathematical Reasoning

1. [Arithmetic — Advanced](arithmetic-advanced.md)
```

---

## Directory Structure

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

### Entity Definitions

- **Subject** — top-level directory (e.g., `mathematics`, `networks`). Must match the Topics table in README.md.
- **Module** — grouping within a subject (e.g., `linear-algebra` inside `mathematics`). Every subject must consist of one or more modules.
- **Submodule** — optional grouping within a module (e.g., `vector-spaces` inside `linear-algebra`). A submodule cannot contain deeper submodules. Module and submodule names must not be the same.
- **`intro/`** — a special directory containing background, philosophy, principles, history, ethics, key events, or official organizations about the field. Every subject and module **must** have an `intro/` directory. Every `intro/` must have an `index.md` listing its files.
- **Short description** — hyphenated slug, lowercase (e.g., `why-math.md`, `vector-operations.md`).
- Content files sit directly under the module or submodule (flat). `intro/` is the only directory allowed at these levels.

### Academic Discipline Requirement

Subjects, modules, and submodules must represent **established branches of knowledge** — fields that have their own academic literature, research traditions, and institutional recognition (e.g., mathematics, linear algebra, vector spaces, operating systems). Do not create entities for job roles, positions, or personas (e.g., `ceo-founders`, `frontend-developers`). Role-based profession explorations belong in `careers/`, not in content modules. The only exceptions are `level-up/` and `careers/`, which are special subjects with custom organizational logic.

### Module Raison d'Être

Every module and submodule must have a justified reason for existing. Before creating one, articulate: *What specific body of knowledge does this represent? Why does it constitute a distinct unit of study rather than a section within a broader module?* If the answer is "it's just a convenient folder," the module should not exist — fold its content into the parent or a sibling. The `intro/` file for each module and submodule must open with a clear statement of purpose that justifies its existence as an independent unit of study.

### Careers Directory

The `careers/` directory follows the same subject/module convention. Each career is a module that explores a profession in depth — what the role entails, its responsibilities, specializations, career progression, and industry context. These documents discuss the *profession itself*, not what someone should learn to enter it. Career modules are subjects of study about professions, not learning paths for individuals.

---

## Index System

### Index Levels

| Index | Location | Format | Purpose |
|-------|----------|--------|---------|
| **Master** | `/index.md` | Numbered learning phases | The world map — defines the full learning journey |
| **Subject** | `subject/index.md` | Numbered learning phases | Regional map — modules grouped by progression |
| **Module** | `module/index.md` | Learning path tree | Dungeon map — sequential depth within one domain |
| **Submodule** | `submodule/index.md` | Learning path tree or flat list | Fine-grained path within a module section |
| **Intro** | `intro/index.md` | Flat list | Background reading — not sequential |

### The 4-Level Index Chain

Every content file must be reachable from the root through four levels:

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

A missing link at any level means the content is orphaned.

### Rules for Index Files

- **Every directory must be listed.** If a directory has content files, its `index.md` must link to them.
- **Do not list directories** — list actual `.md` files.
- **Use relative paths** only. Never absolute or full URLs for internal links.
- **Every list must be under a heading.** No orphaned lists — every group of links belongs under a `## ` heading.
- **Root and subject indexes must use learning phases.** Never output a flat list. Group items under numbered `## ` phase headings that form a coherent learning path.
- **Phase headings must be descriptive.** Forbidden: `## 1. Phase 1`. Required: `## 1. Personal Foundation`, `## 2. Foundational Knowledge`.
- **Subjects/modules within a phase must be ordered by learning dependency.** Place prerequisites before items that build upon them.
- **Never flatten a phased index into a bullet list.** If a root or subject index loses its phase headings, the horizontal dimension collapses.
- **Never skip a phase.** Every phase must contain at least one subject or module. An empty phase is a planning gap — mark it `(Planned)`.
- **Index as planning tool.** An `index.md` may list files that do not yet exist by appending `(planned)` to the link text. Planned entries must **not** have a link. Once the file is written, add the link and remove the `(planned)` label.

### Learning Path Design: Horizontal & Vertical

Every index operates along two dimensions that together form the **learning grid**.

**Horizontal (Breadth):** The root and subject indexes define what the learner encounters at each stage. Phases group subjects or modules that are thematically related and share approximate difficulty. The horizontal dimension answers: *"What should I be aware of at this stage?"*

**Vertical (Depth):** Module and submodule indexes define how the learner masters a specific domain. Numbered phases progress from foundational concepts through intermediate techniques to advanced integration. The vertical dimension answers: *"How do I master this specific topic?"*

```
                    Phase 1        Phase 2        Phase 3        Phase 4
                    (Foundation)   (Building)     (Infrastructure)(Professional)
Root Index          ─────────────────────────────────────────────────────────►
                     │              │              │              │
Subject Index       ─┤──►           ─┤──►           ─┤──►           ─┤──►
                     │              │              │              │
Module Index         ▼              ▼              ▼              ▼
                   [vertical       [vertical       [vertical       [vertical
                    depth]          depth]          depth]          depth]
```

### Heading Rules for Module and Submodule Indexes

#### Mandatory Structure

1. `# Title` — the module or submodule name.
2. `## 1. Introduction` — always the first section. Contains a bullet list linking to the intro file(s).
3. `## 2. ...` through `## N. ...` — content sections forming the learning path.

#### Every `##` Heading Must Be Descriptive

A heading must describe **what the reader learns or does** in that phase. It must function as a signpost — the reader should understand the phase's purpose from the heading alone.

**Forbidden headings:** `## 2. Core`, `## 2. Topics`, `## 2. Content`, `## 2. Modules`, `## 2. Basics`, `## 2. Advanced`.

**Required pattern:** The heading must name the specific domain, skill, or conceptual territory covered.

#### No Heading May Group Unrelated Topics

A single `##` section must contain a **coherent, focused cluster** of related content. If the items under a heading belong to distinct conceptual territories, split them into separate numbered sections.

#### Heading Sequence Must Reflect Learning Progression

The numbered `##` sections should follow a logical order: foundation before application, theory before practice, simple before complex.

#### `###` Branches Within a Section

When a `##` section has more than 3 items that naturally subdivide, use `### N.A.`, `### N.B.` branches. Branches must also have descriptive names.

#### "(Planned)" Suffix for Unbuilt Sections

If all items in a section are planned (no files exist yet), append `(Planned)` to the heading.

#### No Generic Fallbacks

If you cannot find a descriptive heading for a section, the section likely needs to be reorganized — not labeled generically. Every heading is a promise to the reader about what they will learn.

---

## Content Generation Rules

### Do's

- **Be practical.** Connect every concept to how it matters in real development.
- **Use code when it clarifies.** A well-chosen snippet can replace a paragraph of explanation. Use language-identified fenced blocks in popular languages (Python, JavaScript, TypeScript, Go, Rust, etc.) — but only when a code example genuinely adds clarity.
- **Define terms** on first use, then add them to the Glossary.
- **Link internally** to prerequisites and next steps. Cross-link between related topics.
- **Use Mermaid** for diagrams (flowcharts, sequence diagrams, graphs).
- **Use LaTeX** (`$` inline, `$$` display) for math.
- **Keep files focused.** A single document should cover one coherent topic. Split if it grows too long.
- **RPG-like learning experience.** Structure content as a quest-based journey. The Description sets the mission, the Prerequisites define what you need before attempting, the Content is the challenge, the Learning Tips and Glossary are the rewards, and the Next Steps point to the next quest.

### Don'ts

- **No fluff.** No "in this article we will learn", "welcome to", or "let's dive in".
- **No assumptions about tooling.** Do not assume the reader uses a specific OS, editor, or package manager unless the topic requires it.
- **No external dependencies.** Do not reference npm packages, libraries, or frameworks that the document does not explain.
- **No placeholder text.** Every section must have real content. If a topic is genuinely too short for a section, omit it rather than fill with filler.
- **No absolute internal links.** Use `../` relative paths only.
- **No generated placeholders like `[TODO]`** — if content cannot be written now, do not create the file.

---

## Emoji Usage

Emoji usage is **strongly recommended** across all content files to enhance readability, visual appeal, and engagement.

### Placement Rules

- **Section headings** — Place a single emoji before or after the heading text. Examples: `## 🧠 Cognitive Frameworks`, `## 3. Sleep Architecture 🌙`.
- **List items** — Use emojis to differentiate list categories or mark priority levels. Examples: `🟢 Low impact`, `🟡 Medium impact`, `🔴 High impact`.
- **Callout blocks** — Use emojis to signal the type of information. Examples: `💡 Key Insight`, `🔬 Scientific Basis`, `🛠️ Practical Exercise`.
- **Table headers** — Use emojis in the first column to visually distinguish row categories.

### Style Rules

- **One emoji per heading or list item.** Never stack multiple emojis.
- **Relevance over decoration.** Every emoji must thematically match its content.
- **Consistency within a file.** If a file uses `🌙` for sleep-related sections, do not switch to `😴` elsewhere.
- **Professional tone preserved.** Emojis supplement the text; they do not replace words or diminish the academic register.
- **No emojis in code blocks or file paths.**

---

## Style Guide

- **Headings** — ATX-style (`##`, not underlines).
- **Code** — Fenced with language identifiers. Use `text` or `plain` for non-code blocks.
- **Math** — LaTeX inline with `$` and display with `$$`.
- **Diagrams** — Mermaid syntax where helpful.
- **Links** — Relative links for internal documents, full URLs for external resources.
- **Lists** — Use `-` for unordered, `1.` for ordered.
- **Line breaks** — Hard wrap at ~100 characters for readable diffs.
- **Tone** — Direct, instructive, jargon-aware. Define terms on first use.

---

## Line Count & Document Length

Every content file must be **400–800 lines**.

- **If shorter than 400 lines:** Expand with more depth, examples, diagrams, or walkthroughs directly in the Content section.
- **If longer than 800 lines:** Split into multiple focused documents (e.g., a related sub-topic) and link them via Next Steps. Consider whether the topic should be tiered (basic/intermediate/advanced).

---

## Quality Checklist

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
- [ ] **Tiering** — if the topic spans multiple complexity levels, it has been split into at most 3 tiered files.
