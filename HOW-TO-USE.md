# How to Use DevBook

A practical guide to reading, navigating, and getting the most out of this library — written for developers who want to learn efficiently, whether you are a beginner or a seasoned engineer exploring new territory.

## Description

DevBook is a Markdown-based library of technical and professional knowledge for developers. It has no search bar, no recommendation engine, no interactive tutorial system. It is a collection of plain-text files organized into subjects and modules, connected by a chain of indexes. This guide teaches you how to use that system effectively.

## Prerequisites

- None. This document is the entry point for learning how to use DevBook.

## Table of Contents

- [The Big Picture: What DevBook Is](#the-big-picture-what-devbook-is)
- [Quick Start: Your First Session](#quick-start-your-first-session)
- [The Anatomy of a Document](#the-anatomy-of-a-document)
- [Finding Your Path](#finding-your-path)
  - [Method 1: Browse by Subject](#method-1-browse-by-subject)
  - [Method 2: Follow a Learning Path](#method-2-follow-a-learning-path)
  - [Method 3: Search for a Specific Topic](#method-3-search-for-a-specific-topic)
  - [Method 4: Jump Between Related Topics](#method-4-jump-between-related-topics)
- [Efficient Navigation](#efficient-navigation)
  - [The 4-Level Index Chain](#the-4-level-index-chain)
  - [Reading a Module Index](#reading-a-module-index)
  - [Leveraging Prerequisites and Next Steps](#leveraging-prerequisites-and-next-steps)
- [Reading Strategies for Different Subjects](#reading-strategies-for-different-subjects)
  - [Technical Subjects (Programming, CS, Math, Physics)](#technical-subjects-programming-cs-math-physics)
  - [Academic Subjects (Philosophy, Psychology)](#academic-subjects-philosophy-psychology)
  - [Level-Up Subject (Narrative Nonfiction)](#level-up-subject-narrative-nonfiction)
  - [Career Subjects](#career-subjects)
- [Tips for Specific Goals](#tips-for-specific-goals)
  - [I Want to Learn a New Topic from Scratch](#i-want-to-learn-a-new-topic-from-scratch)
  - [I Need a Quick Reference for Something I Already Know](#i-need-a-quick-reference-for-something-i-already-know)
  - [I Want to Explore Randomly](#i-want-to-explore-randomly)
  - [I Am an AI Agent Generating Content](#i-am-an-ai-agent-generating-content)
- [What to Read First (Suggested Starting Points)](#what-to-read-first-suggested-starting-points)
- [Common Pitfalls](#common-pitfalls)
- [Glossary](#glossary)
- [Next Steps](#next-steps)

## The Big Picture: What DevBook Is

DevBook is organized on three principles:

**1. Subjects are disciplines.** Each top-level directory is an academic or professional subject: mathematics, philosophy, programming, psychology, English, and so on. A subject is not a book — it is a category of knowledge with multiple modules inside it.

**2. Modules are learning dependencies.** Each module represents a distinct body of knowledge with a clear boundary. You must understand one module before you can understand another. The module index tells you which order to read them in.

**3. Documents are units of learning.** Each `.md` file covers one focused topic — one concept, one skill, one technique. Documents follow a consistent 9-section format so you always know where to find the summary, the prerequisites, the glossary, and the next steps.

The entire library has no prescribed starting point and no fixed endpoint. You choose where to begin based on what you want to learn. The indexes and cross-references do the rest.

## Quick Start: Your First Session

If you have just cloned the repository, here is the fastest way to get oriented:

```bash
# 1. Open the root index — it is your world map
open index.md

# 2. Pick a subject that interests you
#    (Each line in the root index links to a subject)
open programming/index.md

# 3. Pick a module within that subject
#    (Each section in the subject index links to a module)
open programming/foundations/index.md

# 4. Pick a document within that module
#    (Each item in the module index links to a file)
open programming/foundations/variables-and-data.md
```

That is the core navigation pattern: **root → subject → module → document**. You repeat it every time you start a new topic.

## The Anatomy of a Document

Every content document in DevBook follows the same 9-section structure. Knowing this structure means you can jump directly to the part you need:

| Section | What It Contains | When to Use It |
|---------|-----------------|----------------|
| **Description** | What the document covers and why it matters | Decide whether to read this document at all |
| **Prerequisites** | Links to documents you should read first | Confirm you are ready for this material |
| **Table of Contents** | All section headings with anchor links | Jump to a specific section |
| **Content / Material** | The core teaching | Read from start to finish for deep learning |
| **Learning Tips** | Study advice and common pitfalls | Optimize how you learn the material |
| **Glossary** | Key terms and their definitions | Look up unfamiliar terms quickly |
| **Quick References** | External links to verified resources | Dive deeper or find alternative explanations |
| **Next Steps** | Where to go after this document | Continue your learning path |

**Pro tip:** If you are reading for understanding, start at Description, skim Prerequisites, then read Content from start to finish. If you are looking for a specific fact, use the Table of Contents to navigate directly.

## Finding Your Path

There are four ways to find content in DevBook. Use whichever matches your current goal.

### Method 1: Browse by Subject

Open the root `index.md` and scan the subject list. Each subject has a short description of what it covers. When a subject catches your interest, open its `index.md` to see the modules inside it.

This method works best when you do not know what you want to learn yet — you are exploring.

### Method 2: Follow a Learning Path

Each module `index.md` is ordered as a learning path. The sections (numbered 1, 2, 3...) represent the recommended reading order. If you see:

```
## 2. Core Concepts
1. [Variables](variables.md)
2. [Types](types.md)
3. [Functions](functions.md)
```

...the intended order is Variables → Types → Functions. You can skip items you already know, but the order reflects increasing complexity and dependency.

This method works best when you want to learn a subject systematically.

### Method 3: Search for a Specific Topic

DevBook has no built-in search, but your operating system does. Use `grep` to find any keyword across all files:

```bash
# Search across all MD files
grep -ri "recursion" --include="*.md" .

# Search only in titles (level-1 headings)
grep -r "^# " --include="*.md" . | grep -i "recursion"

# Search in module indexes for broader topics
grep -r "recursion\|sorting\|dynamic" --include="index.md" .
```

This method works best when you know exactly what you want and need to find which module covers it.

### Method 4: Jump Between Related Topics

Every document has Prerequisites (what to read before) and Next Steps (what to read after). These links form a web of related topics. To explore a subject in depth, follow the Next Steps chain:

1. Read a document.
2. Open every link in its Next Steps section.
3. For each of those documents, read their Next Steps.
4. Continue until you have covered the area you care about.

This method works best when you want to build connected knowledge rather than isolated facts.

## Efficient Navigation

### The 4-Level Index Chain

Every document in DevBook is reachable through exactly four levels of navigation:

```
Level 1: /index.md                    ← Master index (root)
Level 2: {subject}/index.md           ← Subject index
Level 3: {subject}/{module}/index.md  ← Module index
Level 4: {subject}/{module}/{file}.md ← Content document
```

If a document is not linked at all four levels, it is considered orphaned and may be incomplete. When browsing, you can trust that every link in an index leads to a real document.

**Shortcut:** If you know the subject and module, you can bypass the indexes and open a document directly:

```bash
open mathematics/linear-algebra/vector-operations.md
```

### Reading a Module Index

A module `index.md` is the most important navigation page. It tells you:

- **The module's purpose** — the opening paragraph explains what this module is for.
- **The learning path** — numbered sections show the recommended order.
- **Intro files** — the `## Introduction` section links to background and philosophy.
- **Prerequisites** — the learning path starts from where earlier modules left off.

To get the most out of a module index, read it from top to bottom once, then use it as a table of contents for the rest of your study.

### Leveraging Prerequisites and Next Steps

The Prerequisites link at the top of each document is not optional decoration. If a document lists a prerequisite, it assumes you already understand that material. If you skip the prerequisite, you may encounter terms or concepts that are not explained in the current document.

The Next Steps section at the bottom of each document is your guided exit. It links to:
- **Related documents within the same module** — deeper or adjacent topics.
- **Documents in other modules** — cross-disciplinary connections.
- **Documents in other subjects** — broader context for the topic.

To build a custom curriculum, start with a document you want to understand, read its prerequisites backward, then read the document, then follow its next steps forward.

## Reading Strategies for Different Subjects

DevBook uses different writing styles depending on the subject. Knowing which style you are reading helps you adjust your reading strategy.

### Technical Subjects (Programming, CS, Math, Physics)

**Style:** Direct, explanatory, with code examples, formulas, and diagrams.

**Strategy:** Read actively. Run the code examples. Work through the formulas. Pause after each section and ask yourself whether you could explain it to someone else. If a section includes practice exercises, complete them before moving on.

### Academic Subjects (Philosophy, Psychology)

**Style:** Academic register — formal vocabulary, complex sentences, abstract concepts, theoretical frameworks.

**Strategy:** Read slowly. Academic prose packs more meaning per sentence than technical prose. After each paragraph, restate the main point in your own words. If you encounter a term you do not recognize, check the Glossary section of the document first, then the broader subject vocabulary guides in the `english/academic-english/` module.

Academic subjects assume comfort with:
- Complex sentence structures (subordinate clauses, conditionals)
- Abstract vocabulary (phenomenological, epistemological, teleological)
- Formal register (nominalization, passive voice, technical terminology)

If you find these difficult, read the Academic English module under `english/academic-english/` first.

### Level-Up Subject (Narrative Nonfiction)

**Style:** Literary nonfiction — vivid scenes, sensory detail, emotional depth, metaphors, narrative voice. This is the only subject that breaks from academic register.

**Strategy:** Read for experience, not information. The level-up documents are meant to be felt, not just understood. Do not skim — the emotional arc is the content. If a passage resonates, pause and sit with it. If a passage confuses you, it may be using metaphor or analogy that requires reflection rather than clarification.

Level-up documents use literary techniques:
- **Metaphor** — comparing one thing to another to create meaning ("the liminal space is a forge").
- **Scene** — placing you inside a moment rather than describing it from outside.
- **Emotional progression** — the document moves through emotional states, not just concepts.

These documents link to academic subjects (philosophy, psychology) for the theoretical backing. If a level-up document references a concept you want to understand more deeply, follow the link to the source subject.

### Career Subjects

**Style:** Descriptive and analytical — exploring the realities of professional roles without prescribing a specific path.

**Strategy:** Read reflectively. Career documents are not instructions — they are explorations. Compare what you read against your own experience and goals. The value comes from the dialogue between the document and your situation.

## Tips for Specific Goals

### I Want to Learn a New Topic from Scratch

1. Open the root `index.md` and find the subject you want to learn.
2. Open the subject's `index.md` and find the module called `foundations/` (if it exists).
3. Read the foundations module in order — it is designed for beginners.
4. After foundations, follow the learning path in each subsequent module.
5. Use Prerequisites to identify any knowledge gaps and fill them before proceeding.

### I Need a Quick Reference for Something I Already Know

1. Search for the topic using `grep` (see Method 3 above).
2. Open the matching document.
3. Skip directly to the Content section via the Table of Contents.
4. Use the Glossary for term definitions.
5. Use Quick References for external resources.

### I Want to Explore Randomly

1. Open the root `index.md`.
2. Close your eyes and pick a subject at random.
3. Open that subject's `index.md` and read the module descriptions.
4. Pick a module whose name sounds interesting.
5. Read the first document in the module's learning path.
6. If you enjoy it, continue. If not, pick another subject.

### I Am an AI Agent Generating Content

If you are an AI agent tasked with creating or maintaining DevBook content, see [AGENTS.md](AGENTS.md). The agent system has its own navigation hub, workflows, and skill files. Key points:

- **Always start with the Index-First Workflow** — read the full index chain before writing.
- **Load the matching skill** — content-writing, content-planning, leveling-up, or career-journey.
- **Follow the 9-section format** defined in [TEMPLATE.md](TEMPLATE.md).
- **Respect the line count** — every content file must be 400–800 lines.
- **No orphans** — every file must be linked from its parent index.

## What to Read First (Suggested Starting Points)

| Your Goal | Start Here |
|-----------|------------|
| Understand the library structure | [README.md](README.md) (this file's parent doc) |
| Find something interesting | [/index.md](index.md) — the master index |
| Learn how to learn better | [How Learning Works](../psychology/learning-science/how-learning-works.md) |
| Learn to read English for DevBook | [Why English Matters](english/intro/why-english-matters.md) |
| Master DevBook's English style | [Academic English module](english/academic-english/index.md) |
| Start the personal journey | [level-up/index.md](level-up/index.md) — the beginning of the journey |
| Refresh programming basics | [Programming Foundations](programming/foundations/index.md) |
| Explore a specific technology | Search for it with `grep` first, then follow the path |

## Common Pitfalls

**Skipping prerequisites.** The most common mistake. A document's prerequisites exist because later sections depend on that knowledge. If you skip a prerequisite, you will encounter unexplained terms and may misunderstand the material.

**Reading everything linearly.** DevBook is not a novel. You do not need to read every subject from start to finish. Jump between topics, skip what you already know, and follow the cross-references that interest you.

**Treating indexes as tables of contents only.** An index is more than a list of files. The order of items within each section is deliberate — it reflects the learning path. The descriptions next to each link summarize the document's value. Read the descriptions before choosing which document to open.

**Expecting a single path.** There is no single "correct" path through DevBook. Different readers have different goals, backgrounds, and learning styles. The index chain provides structure, but the structure is a scaffold, not a railroad. You decide where to go.

**Forgetting you can search.** When you cannot find a topic through browsing, use `grep`. A five-second search is faster than fifteen minutes of clicking through indexes.

## Glossary

| Term | Definition |
|------|------------|
| **Subject** | A top-level category of knowledge (e.g., Mathematics, Philosophy, Programming). Each subject is a directory containing modules. |
| **Module** | A distinct unit of study within a subject. Each module represents a body of knowledge with clear learning dependencies. |
| **Index** | A file named `index.md` that lists and describes the files in a directory. Indexes are the navigation backbone of the library. |
| **Index Chain** | The four-level navigation path from root to document: root → subject → module → content. |
| **Prerequisites** | Links at the top of a document to material that should be understood before reading the current document. |
| **Next Steps** | Links at the bottom of a document to related material that builds on the current document. |
| **Learning Path** | The recommended reading order within a module, indicated by numbered sections in the module's index. |
| **Orphan** | A content file that is not linked from its parent index. Orphans are inaccessible through normal navigation. |
| **Tiered File** | A file split into multiple levels (basic, intermediate, advanced) when a topic spans multiple complexity levels. |
| **Academic Register** | Formal English used in scholarly writing — complex sentences, technical vocabulary, abstract concepts. |
| **Literary Nonfiction** | Prose that applies fiction techniques (scene, metaphor, emotional depth) to real experience. Used only in the level-up subject. |

## Next Steps

- [Master Index](index.md) — your starting point for browsing all subjects
- [English Subject Index](english/index.md) — the full English learning path, from reading fundamentals to academic fluency
- [English: Why English Matters](english/intro/why-english-matters.md) — understand why English proficiency is essential for developers
- [Academic English Module](english/academic-english/index.md) — bridge from basic English to the academic register used across DevBook
- [CONTRIBUTING.md](CONTRIBUTING.md) — how to contribute content to DevBook
- [AGENTS.md](AGENTS.md) — navigation hub for AI agents managing content
- [TEMPLATE.md](TEMPLATE.md) — the mandatory document format for all content files
