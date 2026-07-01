# Diataxis Framework in Practice

## Description

Diataxis is a documentation framework that organizes content into four modes: tutorials, how-to guides, explanation, and reference. Understanding and applying Diataxis helps you structure documentation that serves readers at every stage of their learning journey, from first exploration to daily reference.

## Prerequisites

- [Documentation Architecture](doc-architecture.md) — Diataxis is an architectural pattern for organizing documentation
- [Tutorials](tutorials.md) — understanding the tutorial mode before seeing how it fits into the full framework

## Table of Contents

- [What Is Diataxis?](#what-is-diataxis)
- [The Four Quadrants](#the-four-quadrants)
- [Theory vs. Practice of Each Mode](#theory-vs-practice-of-each-mode)
- [Tutorials in Depth](#tutorials-in-depth)
- [How-To Guides in Depth](#how-to-guides-in-depth)
- [Explanation in Depth](#explanation-in-depth)
- [Reference in Depth](#reference-in-depth)
- [Audience Orientation: Study vs. Work](#audience-orientation-study-vs-work)
- [Common Mistakes: Mixing Modes](#common-mistakes-mixing-modes)
- [Applying Diataxis to Real Projects](#applying-diataxis-to-real-projects)
- [Tools and Templates for Each Mode](#tools-and-templates-for-each-mode)
- [Case Studies of Docs Restructured with Diataxis](#case-studies-of-docs-restructured-with-diataxis)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### What Is Diataxis?

Diataxis is a documentation framework created by Daniele Procida. It divides documentation into four modes based on two dimensions: the reader's goal (study vs. work) and the type of content needed (theory vs. practice).

**The core insight of Diataxis:**

Most documentation fails because it mixes purposes. A single page tries to teach, guide, explain, and reference all at once. Readers cannot find what they need because the content serves too many masters.

Diataxis solves this by separating documentation into four distinct modes, each with its own purpose, audience, and format.

**The four modes at a glance:**

```text
| Mode        | Audience is... | Content is...  | Typical reader question |
|-------------|----------------|----------------|-------------------------|
| Tutorials   | Learning       | Step-by-step   | "How do I learn this?"  |
| How-to guides | Working     | Task-oriented  | "How do I solve X?"     |
| Explanation | Understanding  | Conceptual     | "Why does this work?"   |
| Reference   | Referring      | Factual        | "What is the syntax?"   |
```

### The Four Quadrants

Diataxis organizes documentation along two axes:

```text
                  Study (learning)              |            Work (doing)
                  Goal: understanding           |        Goal: completing a task
-----------------------------------------------|-------------------------------
Tutorials:                                     | How-to guides:
  Learning-oriented                            |  Task-oriented
  Guided, step-by-step lessons                 |  Practical solutions
  "Teach a beginner to do something"           |  "Show an experienced user how"
  Example: "Build your first web app"          |  Example: "Deploy to production"
-----------------------------------------------|-------------------------------
Explanation:                                   | Reference:
  Understanding-oriented                       |  Information-oriented
  Background, discussion, analysis             |  Facts, parameters, commands
  "Why is it designed this way?"               |  "What is the exact syntax?"
  Example: "Architecture overview"             |  Example: "API endpoint reference"
```

**The key distinction — study vs. work:**

```text
Study (Tutorials + Explanation):
- Reader wants to learn and understand
- Time investment is expected
- Sequential reading makes sense
- Curiosity-driven exploration

Work (How-to guides + Reference):
- Reader wants to accomplish a task
- Time is limited
- Non-sequential reading is the norm
- Problem-driven retrieval
```

### Theory vs. Practice of Each Mode

Understanding the theory of Diataxis is easy. Applying it to real documentation requires navigating edge cases, overlapping content, and organizational constraints.

**Theory says:** Each mode is pure and distinct.

```text
Tutorial: Only step-by-step learning, no reference tables
How-to: Only task solutions, no background explanation
Explanation: Only concepts, no step-by-step
Reference: Only facts, no teaching
```

**Practice says:** Modes often overlap at the boundaries.

```text
Tutorial: May include brief concept explanations inline
How-to: May link to explanation for context
Explanation: May include small reference tables for clarity
Reference: May include brief usage notes

The goal is not purity but clarity. If a reference entry needs
a one-sentence explanation of the concept, that is better than
forcing the reader to open a separate explanation page.
```

**When to be strict:**

```text
Be strict about mode separation when:
- The project has a dedicated writer or team
- The documentation is large (100+ pages)
- The audience includes distinct user groups
- Each mode has its own section or site area

Be flexible when:
- The project is a single README
- One person maintains the docs
- The audience is narrow and knows what they want
- Mixing modes makes the content more useful
```

### Tutorials in Depth

Tutorials are the first mode in the Diataxis framework. They are learning-oriented and guided.

**Purpose:** To give the reader a successful first experience with the product or concept.

**Diataxis says about tutorials:**

```text
A tutorial:
- Is a lesson, not a problem-solving guide
- Leads the reader step by step
- Ensures success at every stage
- Does not assume prior knowledge
- Has a clear endpoint where something is achieved

A tutorial is NOT:
- A how-to guide (solving a specific problem)
- A reference (looking up a fact)
- An explanation (understanding the theory)
```

**How to recognize a tutorial disguised as a how-to guide:**

```text
Title: "How to build a web application with Framework X"
This is a tutorial if:
- It assumes no prior knowledge of Framework X
- It takes the reader from zero to a working app
- Steps are ordered sequentially and build on each other

Title: "How to configure authentication for Framework X"
This is a how-to guide if:
- It assumes the reader has Framework X installed
- It focuses on one specific task
- It can be completed independently of other tasks
```

**Tutorial checklist in the Diataxis framework:**

```text
- [ ] Does it assume the minimum viable prior knowledge?
- [ ] Does each step build on the previous one?
- [ ] Can the reader verify they are on track after each step?
- [ ] Does it produce a tangible result?
- [ ] Is it achievable in a single session (15-30 minutes)?
- [ ] Does it avoid distractions (optional features, edge cases)?
- [ ] Does it end with a clear "you have accomplished X"?
```

### How-To Guides in Depth

How-to guides are work-oriented. They solve specific problems for readers who already know the basics.

**Purpose:** To help an experienced user accomplish a specific task.

**Diataxis says about how-to guides:**

```text
A how-to guide:
- Is task-oriented, not learning-oriented
- Solves a real problem
- Assumes the reader has basic familiarity
- Is skimmable and non-sequential
- Provides one or more approaches to the problem

A how-to guide is NOT:
- A tutorial (comprehensive, sequential learning)
- A reference (listing every parameter)
- An explanation (discussing why things work)
```

**How-to guide structure:**

```text
Title: "How to [accomplish a specific task]"

1. Context (1-2 sentences)
   "When you need to [task], follow these steps."

2. Prerequisites (list)
   What the reader needs before starting.

3. Steps (numbered or bulleted)
   Each step is a discrete action.
   Steps can sometimes be done in any order.

4. Expected outcome
   What success looks like.

5. Troubleshooting (if applicable)
   Common problems and solutions.

6. Related guides
   What to try next.
```

**The difference between a how-to guide and a tutorial:**

```text
How-to guide:
"How to reset a user's password"
- Reader has an account system
- Reader knows the basics of the system
- Focus: complete the reset task
- Time: 5 minutes

Tutorial:
"Build a user authentication system"
- Reader has no auth system
- Reader may be new to the framework
- Focus: understand authentication
- Time: 45 minutes
```

### Explanation in Depth

Explanation is understanding-oriented. It provides background, context, and design rationale.

**Purpose:** To help the reader understand why things work the way they do, so they can make informed decisions.

**Diataxis says about explanation:**

```text
Explanation:
- Is understanding-oriented, not task-oriented
- Provides background and context
- Discusses design decisions and trade-offs
- Helps readers build mental models
- Is often read before or after doing a task

Explanation is NOT:
- A tutorial (how to do something)
- A how-to guide (solving a specific problem)
- A reference (listing facts)
```

**Examples of good explanation topics:**

```text
- Architecture overview: "How the system processes requests"
- Design decisions: "Why we chose PostgreSQL over MongoDB"
- Concepts: "What eventual consistency means for your data"
- Comparison: "SQL vs. NoSQL: when to use each"
- Deep dives: "How the caching layer works internally"
- History: "Why REST became the dominant API pattern"
```

**Explanation does not have to be long:**

```text
Some explanation can be a single paragraph within another document:

"Database migrations are version-controlled changes to your database
schema. They allow you to track changes over time, apply them
reproducibly across environments, and roll back if something goes wrong.
Think of them like Git for your database structure."

This one-paragraph explanation gives the reader a mental model
before they run their first migration command.
```

**Guidelines for writing explanation:**

```text
Do:
- Connect concepts to what the reader already knows
- Use analogies and metaphors
- Include diagrams and visualizations
- Discuss alternatives and trade-offs
- Write in a narrative, engaging style

Don't:
- Include step-by-step instructions (that is a how-to guide)
- List every parameter or option (that is reference)
- Assume the reader has deep prior knowledge
- Use the explanation as a tutorial
```

### Reference in Depth

Reference is information-oriented. It provides accurate, complete facts about the system.

**Purpose:** To give the reader precise and authoritative information about the system's components.

**Diataxis says about reference:**

```text
Reference:
- Is information-oriented, not learning-oriented
- Provides exact facts (parameters, return types, syntax)
- Is complete and authoritative
- Is structured for lookup, not sequential reading
- Assumes the reader knows what they are looking for

Reference is NOT:
- A tutorial (teaching how to use the system)
- A how-to guide (solving a problem)
- An explanation (discussing why)
```

**Types of reference documentation:**

```text
- API endpoint reference (path, method, parameters, responses)
- Configuration file reference (all keys, types, defaults)
- CLI command reference (all commands, flags, arguments)
- SDK/API reference (classes, methods, properties, events)
- Error code reference (codes, messages, causes, fixes)
- Changelog (version history, breaking changes, additions)
- Glossary (terms and definitions)
```

**Reference documentation style:**

```text
Reference entries should be:
- Consistent: Same structure for every entry
- Complete: Every parameter documented, no gaps
- Precise: Exact types, defaults, constraints
- Verifiable: Can be tested against the actual system
- Minimal: No fluff, no teaching, no context beyond what is needed

Example reference entry:

### createUser(email, password, options?)

Creates a new user account.

| Parameter     | Type   | Required | Default | Description           |
|---------------|--------|----------|---------|-----------------------|
| email         | string | yes      | —       | User's email address  |
| password      | string | yes      | —       | Minimum 8 characters  |
| options       | object | no       | {}      | Additional options    |
| options.role  | string | no       | viewer  | User role             |

Returns: Promise<User>

Throws:
- ValidationError: If email or password are invalid
- ConflictError: If email is already registered
```

**When reference becomes explanation:**

```text
A common mistake is adding explanation to reference entries:

"email (string): The user's email address. We chose to use email as
the primary identifier because it is universal and easy to remember
unlike usernames which..."

The explanation belongs in a separate "User identification" concept page.
The reference entry should just say:

"email (string, required): User's email address. Must be unique."
```

### Audience Orientation: Study vs. Work

The study vs. work axis is the most important concept in Diataxis. It determines how you write, structure, and present each mode.

**Study-oriented content (Tutorials + Explanation):**

```text
Reader mindset: "I want to learn and understand."
Writing style: Patient, thorough, contextual
Reading pattern: Sequential, front-to-back
Format: Prose-heavy with examples
Time investment: 15-60 minutes
Success metric: Reader understands the concept
```

**Work-oriented content (How-to + Reference):**

```text
Reader mindset: "I want to get something done."
Writing style: Direct, scannable, actionable
Reading pattern: Non-sequential, search-first
Format: Lists, tables, code blocks
Time investment: 30 seconds to 5 minutes
Success metric: Reader completes the task
```

**Why the distinction matters:**

```text
Putting study-oriented content in work-oriented sections frustrates readers
who just want to complete a task quickly. Putting work-oriented content
in study-oriented sections interrupts the learning flow.

A tutorial that includes the full API reference table: bad for both audiences.
The learner is overwhelmed by details they do not need yet.
The worker cannot find the reference table because it is buried in a tutorial.
```

### Common Mistakes: Mixing Modes

The most common documentation problem Diataxis identifies is mixing modes — putting reference in a tutorial, or explanation in a how-to guide.

**Mistake 1: Reference in a tutorial**

```text
Tutorial: "Build a REST API with Express"

Contains:
- Step-by-step instructions (good for tutorial)
- A full table of HTTP status codes (reference content)

Problem: The learner stops at the table to read it, losing momentum.
The person looking for HTTP status codes cannot find them because
they are hiding in a tutorial about Express.

Fix: Link to a reference page for HTTP status codes.
Keep the tutorial focused on the learning goal.
```

**Mistake 2: Tutorial in a reference page**

```text
Reference: "POST /users endpoint"

Contains:
- Parameters, response, errors (good for reference)
- A 15-step guide on how to sign up a user (tutorial content)

Problem: The person who just wants to see the parameter types
has to scroll through a tutorial. The person who needs the tutorial
cannot find it in the reference section.

Fix: Reference page has only reference content.
Link to a separate "User registration tutorial" from the reference page.
```

**Mistake 3: Explanation in a how-to guide**

```text
How-to guide: "How to deploy to production"

Contains:
- Deployment steps (good for how-to)
- Three paragraphs on why blue-green deployment is better than
  rolling deployment (explanation content)

Problem: The reader who is deploying at 2 AM does not care about
deployment strategy theory. They just want the commands to run.

Fix: Move the explanation to a "Deployment strategies" concept page.
Link to it from the how-to guide for readers who want context.
```

**Mistake 4: The everything page**

```text
A single page that contains:
- A tutorial at the top
- A how-to guide in the middle
- Reference tables at the bottom
- Explanation section at the very end

This is the most common problem in community documentation.
One page tries to be everything to everyone and serves no one well.

Fix: Split into four pages, one per mode. Link them together.
```

### Applying Diataxis to Real Projects

**For different project sizes:**

```text
Small (single README): Structure sections within README by mode.
  Quick start (tutorial) -> Common tasks (how-to) -> API (reference) -> Architecture (explanation)

Medium (10-50 pages): Top-level navigation by mode.
  /getting-started -> /guides -> /concepts -> /reference

Large (100+ pages): Top-level sections with subsections by topic.
  /tutorials -> /guides -> /concepts -> /reference -> /changelog -> /faq
```

### Tools and Templates for Each Mode

**Tutorial template:**

```markdown
# {Tutorial Title}

{One-paragraph description of what the reader will build or learn}

## Prerequisites

- Tool X version Y
- Account at Z
- Basic knowledge of A

## Objectives

By the end of this tutorial, you will have:
1. {Objective 1}
2. {Objective 2}

## Step 1: {Action title}

{Instructions with verification}

## Step 2: {Action title}

{Instructions with verification}

## Summary

{tutorial outcome. Link to next tutorial or related guide.}
```

**How-to guide template:**

```markdown
# How to {Task Description}

{1-2 sentence context: when and why you would do this}

## Prerequisites

- {List what the reader needs}

## Steps

1. {Action}
2. {Action}
3. {Action}

## Verification

{How to confirm the task was completed successfully}

## Troubleshooting

- {Common problem}: {Solution}

## Related

- {Related how-to guide}
```

**Explanation template:**

```markdown
# {Concept Title}

{1-2 paragraph overview of the concept}

## How It Works

{Detailed explanation with diagrams}

## Why This Approach

{Design decisions and trade-offs}

## Related Concepts

- {Concept A}: {Brief relationship}
- {Concept B}: {Brief relationship}
```

**Reference template:**

```markdown
# {Component / Command / Endpoint}

{One-line summary}

## Syntax

{Exact syntax or signature}

## Parameters

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|

## Returns

{Return type and description}

## Errors

| Error | Condition |
|-------|-----------|

## Glossary

| Term | Definition |
|------|------------|
| Diataxis | A documentation framework with four modes: tutorials, how-to guides, explanation, and reference |
| Tutorial | Learning-oriented, step-by-step lessons that build understanding |
| How-to guide | Task-oriented, practical solutions to specific problems |
| Explanation | Understanding-oriented, background and conceptual discussion |
| Reference | Information-oriented, authoritative facts about a system |
| Study orientation | Content designed for readers who want to learn and understand |
| Work orientation | Content designed for readers who want to complete a task |
| Mode mixing | Combining content from different Diataxis modes on the same page |
| Cross-referencing | Linking between related content in different Diataxis sections |
| Scaffolding | Providing structure and guidance that is gradually removed as the learner progresses |

## Quick References

- [Diataxis Documentation Framework](https://diataxis.fr/) — the original framework by Daniele Procida
- [Write the Docs: Diataxis](https://www.writethedocs.org/guide/) — community discussions and resources
- [Everything You Need to Know About Diataxis](https://diataxis.fr/what-is-diataxis/) — comprehensive overview

## Next Steps

- [Docs as Code](docs-as-code.md) — implementing Diataxis-structured documentation in a docs-as-code workflow
- [Style Guides](../grammar-and-style/style-guides.md) — applying style guide rules consistently across all four Diataxis modes
