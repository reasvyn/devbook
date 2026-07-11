# What Is Technical Writing?

## Description

Technical writing is the practice of translating complex technical information into clear, usable documentation. For software developers, it is not optional — code is read more often than it is written, and every API, README, and pull request is a technical writing exercise.

## Prerequisites

- [Why English Matters for Developers](../../intro/why-english-matters.md) — the importance of communication in engineering
- [A Brief History of Technical Communication](../../intro/history-of-technical-communication.md) — where the field came from

## Table of Contents

- [Defining Technical Writing](#defining-technical-writing)
- [Technical Writing vs. Other Writing](#technical-writing-vs-other-writing)
- [The Audience Problem](#the-audience-problem)
- [The Core Principles](#the-core-principles)
- [Types of Technical Documentation](#types-of-technical-documentation)
- [The Documentation Lifecycle](#the-documentation-lifecycle)
- [Technical Writing as Software Engineering](#technical-writing-as-software-engineering)
- [The Role of the Technical Writer](#the-role-of-the-technical-writer)
- [Tools of the Trade](#tools-of-the-trade)
- [Measuring Documentation Quality](#measuring-documentation-quality)
- [Common Anti-Patterns](#common-anti-patterns)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Defining Technical Writing

Technical writing communicates specialized knowledge to a specific audience for a practical purpose. Unlike creative writing (which entertains) or academic writing (which argues), technical writing instructs, informs, and enables action.

**The defining characteristics:**

- **Audience-centric** — every decision serves the reader's goals
- **Task-oriented** — enables the reader to do something
- **Precision-first** — ambiguity is a bug, not a feature
- **Structured** — information is organized for scanning and retrieval
- **Verifiable** — claims can be tested and demonstrated
- **Maintainable** — designed to be updated as the product evolves

**What technical writing is NOT:**

- Marketing copy (which persuades rather than informs)
- Academic prose (which argues rather than instructs)
- Creative writing (which entertains rather than enables)
- Code comments (which are internal to the codebase)
- Sales collateral (which exaggerates rather than specifies)

### Technical Writing vs. Other Writing

| Dimension | Technical writing | Academic writing | Creative writing |
|---|---|---|---|
| Purpose | Enable action | Demonstrate knowledge | Entertain, evoke |
| Audience | Practitioner | Peer scholar | General reader |
| Style | Clear, concise, precise | Formal, hedged | Varied, expressive |
| Structure | Scannable, hierarchical | Linear, argumentative | Narrative, free-form |
| Verification | Demonstratable | Peer-reviewed | Subjective |
| Maintenance | Versioned, updated | Static, cited | Static, editioned |
| Jargon | Defined on first use | Assumed shared knowledge | Avoided or contextual |

### The Audience Problem

Technical writing is defined by its audience. Before writing a single word, answer:

**Primary audience analysis:**

1. **Who will read this?** (Junior developers? Senior architects? DevOps? Product managers?)
2. **What do they already know?** (Assume too little: they get lost. Assume too much: they get frustrated.)
3. **What are they trying to do?** (Install? Configure? Troubleshoot? Understand?)
4. **What's their state of mind?** (Calm exploration? Emergency debugging? Sleep-deprived on-call?)
5. **What format do they prefer?** (Quick reference? Step-by-step? Video? Interactive?)

**The persona approach:**

Create reader personas for your documentation:

```
Persona: "Sarah, Mid-level Backend Developer"
- Familiar with REST APIs but new to your service
- Reading during a sprint planning session
- Needs to evaluate integration effort quickly
- Will skim first, read details only where confused
- Prefers code examples over prose explanations
```

**Audience levels:**

| Level | Description | Documentation approach |
|---|---|---|
| Novice | New to the domain | Tutorials, getting started, concepts |
| Intermediate | Some experience | How-to guides, common tasks |
| Expert | Deep domain knowledge | Reference, API docs, troubleshooting |
| Non-technical | Needs to understand without coding | Overviews, use cases, benefits |

One document cannot serve all audiences equally. A good documentation system serves different audiences through different content types (Diataxis).

### The Core Principles

**Principle 1: Clarity**

The reader should never need to re-read a sentence to understand it. Every sentence has one interpretation.

Bad: "The system may optionally be configured to use the alternative authentication mechanism which provides enhanced security features at the cost of additional latency in the initial connection establishment phase."

Good: "You can enable enhanced authentication. It is more secure but adds latency during connection setup."

**Principle 2: Conciseness**

Every word should earn its place. Remove:

- **Hedge words:** "basically," "essentially," "actually," "virtually"
- **Redundant phrases:** "in order to" → "to", "due to the fact that" → "because"
- **Empty nouns:** "the process of installation" → "installation"
- **Unnecessary qualifiers:** "very unique," "quite important"

**Principle 3: Scannability**

Readers do not read documentation linearly. They scan for relevant information.

Techniques:
- Descriptive headings (not "Introduction" but "Configuring the Database Connection")
- Bullet lists for parallel items
- Tables for comparisons
- Code blocks for examples
- Bold for keywords
- Consistent formatting

**Principle 4: Accuracy**

Documentation is a contract. If the docs say something works a certain way, it must. Inaccurate documentation is worse than no documentation — it actively wastes the reader's time.

**Principle 5: Completeness**

A document should contain everything the reader needs and nothing they do not. Missing steps cause frustration. Extraneous content causes confusion.

### Types of Technical Documentation

**Product documentation:**

| Type | Example | Characteristics |
|---|---|---|
| Getting Started Guide | "Build your first app in 5 minutes" | Tutorial, minimal explanation |
| User Guide | "Managing users and permissions" | How-to, task-oriented |
| Reference | "API specification" | Comprehensive, detailed |
| Troubleshooting | "Common errors and solutions" | Problem-solution format |
| Release Notes | "What's new in v2.0" | Change-oriented, dated |
| Architecture | "System overview and design decisions" | Explanatory, diagram-rich |

**Process documentation:**

| Type | Example | Characteristics |
|---|---|---|
| Runbook | "Incident response procedure" | Step-by-step, time-critical |
| Onboarding | "New developer setup guide" | Sequential, environment-specific |
| Playbook | "Database migration process" | Conditional, options-based |
| Contribution Guide | "How to submit a pull request" | Community-facing, standards |

**Developer documentation specifically:**

- **README:** The entry point. Tells readers what the project is, why they should care, and how to start.
- **API documentation:** The contract. Every endpoint, parameter, response, error code.
- **Integration guides:** How to connect this system with others.
- **Migration guides:** How to move from version N to version N+1.
- **CLI documentation:** Commands, flags, configuration files.
- **SDK/Client library docs:** Language-specific usage patterns.

### The Documentation Lifecycle

Documentation is not a one-time effort. It follows a lifecycle:

**1. Plan**

- Identify audience and their needs
- Choose the right documentation type (tutorial vs. reference vs. how-to)
- Define scope and success criteria

**2. Write**

- Follow the style guide
- Write the first draft quickly
- Focus on accuracy and completeness before polish

**3. Review**

- Technical review: does the content match the product behavior?
- Editorial review: is the writing clear and consistent?
- User review: can a target-audience reader follow it?

**4. Publish**

- Build and preview (if using a static site generator)
- Check links, code samples, and formatting
- Deploy to production

**5. Maintain**

- Track documentation alongside code changes
- Update docs in the same PR as code changes
- Audit documentation quarterly for outdated content
- Get feedback from users (analytics, surveys, support tickets)

**6. Retire**

- Archive documentation for obsolete features
- Redirect old URLs to relevant current content
- Remove outdated examples that mislead readers

### Technical Writing as Software Engineering

The Docs as Code philosophy treats documentation with the same rigor as software:

**Version control:**

- Documentation lives in the same repository as code
- Changes go through pull requests with review
- Documentation changes are tracked alongside feature changes

**Testing:**

- Link checkers verify all internal and external links
- Code snippet testers verify examples compile and run
- Spell checkers catch typos and inconsistent terminology
- Style checkers enforce consistency (Vale, write-good, markdownlint)
- Accessibility checkers verify screen reader compatibility

**CI/CD integration:**

```
Pull Request → Lint → Build → Test Links → Deploy Preview → Review → Merge → Deploy Production
```

Each step catches different types of documentation bugs:

| Stage | What it catches |
|---|---|
| Lint | Formatting, style violations, broken internal links |
| Build | Missing files, invalid markup, broken includes |
| Test | Broken code examples, failing snippets |
| Deploy Preview | Visual regressions, layout issues |

**Documentation debt:**

Like technical debt, documentation debt accumulates when:

- Code changes without documentation updates
- Quick fixes are documented with TODO comments instead of proper updates
- Outdated examples are left in place
- Deprecated features remain documented as current

Documentation debt should be tracked and triaged alongside technical debt.

### The Role of the Technical Writer

In software organizations, technical writers wear multiple hats:

**Information architect:** Designs the structure of the documentation system — where information lives, how it's organized, how readers navigate.

**Editor:** Refines developer-written documentation for clarity and consistency. Ensures every document follows the style guide.

**Advocate:** Represents the user's perspective. Pushes back on unclear terminology, inconsistent interfaces, and poor developer experience.

**Researcher:** Tests documentation with real users. Uses analytics to understand what readers need. Interviews support teams to identify common questions.

**Tools specialist:** Maintains the documentation toolchain — build system, linters, search, templates.

**The relationship with developers:**

- Technical writers are not "documenters" who transcribe what developers say
- They are collaborators who bring a different expertise (audience, clarity, structure)
- Best outcomes occur when writers and developers work together on documentation
- Developers should write the first draft of technical content; writers should edit, structure, and test

### Tools of the Trade

**Authoring tools:**

| Tool | Purpose |
|---|---|
| Visual Studio Code | Markdown editing with preview, linting |
| Obsidian | Knowledge management, linking |
| Notion | Collaborative documentation |
| Google Docs | Review and commenting |

**Static site generators:**

| Tool | Language | Use case |
|---|---|---|
| Docusaurus | React | Open source projects, SaaS docs |
| MkDocs | Python | Technical documentation |
| Jekyll | Ruby | GitHub Pages, blogs |
| Hugo | Go | High-performance sites |
| Sphinx | Python | Python ecosystem, API docs |

**Linting and testing tools:**

| Tool | What it checks |
|---|---|
| Vale | Style and consistency |
| markdownlint | Markdown syntax |
| Prettier | Code formatting |
| lychee | Broken links |
| write-good | Prose quality |
| Alex | Insensitive language |

**API documentation tools:**

| Tool | Purpose |
|---|---|
| Swagger/OpenAPI | REST API specification |
| Stoplight | API design and documentation |
| Postman | API testing and documentation |
| Slate | Static API documentation |
| Redoc | OpenAPI rendering |

**Collaboration and review:**

| Tool | Purpose |
|---|---|
| GitHub/GitLab | PR-based review |
| Reviewable | Code review of docs |
| Hypothesis | Collaborative annotation |
| Netlify/Vercel | Preview deployments |

### Measuring Documentation Quality

Quantifying documentation quality is difficult but essential:

**Quantitative metrics:**

- **Time-to-task:** How long until a reader finds what they need
- **Task success rate:** Percentage of readers who succeed at their goal
- **Search analytics:** What terms readers search for, and whether they find results
- **Support ticket volume:** How many questions are answered by existing documentation
- **Page views and bounce rate:** Which pages are used and which are abandoned
- **Feedback ratings:** Thumbs up/down, star ratings, comments

**Qualitative metrics:**

- **User testing:** Observe readers using the documentation
- **Surveys:** Ask readers about their experience
- **Interviews:** Deep conversations with representative users
- **Support ticket analysis:** What questions keep coming up despite existing docs

**The documentation quality matrix:**

| Criteria | Poor | Acceptable | Excellent |
|---|---|---|---|
| Accuracy | Outdated, incorrect | Correct but incomplete | Verified, tested, current |
| Completeness | Missing steps | Covers main cases | Covers edge cases, errors |
| Clarity | Confusing, ambiguous | Understandable | Crystal clear |
| Scannability | Walls of text | Some structure | Hierarchical, searchable |
| Consistency | Multiple styles | Loose style guide | Strict style enforcement |
| Maintainability | Manual updates | Semi-automated | CI/CD, automated testing |

### Common Anti-Patterns

**1. The wall of text:**

```
The configuration system allows users to modify the behavior of the application
through a series of environment variables that can be set either in the .env file
or passed directly to the process at startup. The key configuration options
include the database connection string, which must be set to a valid PostgreSQL
connection URL, the log level, which can be set to debug, info, warn, or error,
and the port number, which defaults to 3000 but can be overridden...
```

Better: Use a table, use code blocks, use headings.

**2. Assuming too much:**

"Simply configure the connection pool and inject the dependency."

"Simply" is the most dangerous word in technical writing. If it were simple, the reader would not need documentation.

**3. The kitchen sink:**

One document that tries to cover everything — tutorial, reference, troubleshooting, and architecture — for all audiences. This confuses everyone.

Better: One document per purpose and audience.

**4. Documentation by accident:**

Documentation that exists because someone had to write something, not because a user needs it. Produces content that is technically correct but practically useless.

**5. Copy-paste documentation:**

"I'll write the docs by copying what the last project did." Produces content that is almost relevant but subtly wrong in every detail.

**6. Documentation as an afterthought:**

"I'll write the docs after the code is done." This is the most expensive approach — by the time the code is done, the author has lost context and motivation.

Better: Write docs alongside code. Better still: write docs before code (documentation-driven development).

## Usage
node index.js input.csv output.json
```

After:
```
# myapp — CSV to JSON converter

Processes CSV files and outputs structured JSON for API consumption.

## Installation
npm install -g myapp

## Quick Start
myapp input.csv output.json

## Features
- Automatic delimiter detection
- Header-based key mapping
- Streaming for large files (>1GB)

## Documentation
https://myapp.dev/docs

## License
MIT
```

The after version tells the reader what the project does, why they'd use it, and how to start immediately.

## Glossary

| Term | Definition |
|---|---|
| Anti-pattern | A common but ineffective response to a recurring problem |
| Docs as Code | Treating documentation with the same tooling and workflows as software code |
| Documentation debt | Accumulated documentation quality issues from deferred maintenance |
| Getting Started Guide | Tutorial-focused documentation for first-time users |
| Hedge word | A word that reduces certainty without adding information (basically, essentially) |
| Information architecture | The structural design of information in a documentation system |
| Knowledge management | The practice of capturing, organizing, and sharing knowledge within an organization |
| Migration guide | Documentation explaining how to move from one version to another |
| Runbook | Operational documentation for routine or emergency procedures |
| Scannability | How easily a reader can find relevant information by scanning |
| Style guide | Documented conventions for writing, formatting, and terminology |
| Technical debt | The implied cost of additional rework caused by choosing an easy solution now instead of a better approach |

## Quick References

- [Google Developer Documentation Style Guide](https://developers.google.com/style) — comprehensive style reference
- [Diataxis Framework](https://diataxis.fr/) — documentation classification system
- [Write the Docs: Documentation Guide](https://www.writethedocs.org/guide/) — community best practices
- [Apple Style Guide](https://help.apple.com/applestyleguide/) — example of corporate style guide
- [Microsoft Style Guide](https://learn.microsoft.com/en-us/style-guide/) — technical writing standards
- [Vale: Prose Linter](https://vale.sh/) — automated style checking
- [Docusaurus Documentation](https://docusaurus.io/docs) — example of excellent documentation

## Next Steps

- [API Reference Documentation](../api-docs.md) — writing clear, usable API docs
- [Diataxis Framework in Practice](../diataxis.md) — applying the framework to real projects
