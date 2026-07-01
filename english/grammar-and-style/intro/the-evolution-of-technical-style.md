# The Evolution of Technical Style

## Description

Technical writing style has transformed from rigid, print-era manuals to principles-based, audience-aware guidance. This evolution reflects changes in technology (from mainframes to open-source ecosystems), medium (from paper to hypertext), and philosophy (from prescriptive rules to contextual principles). Understanding this history helps developers write documentation that fits modern conventions.

## Prerequisites

- [Why Grammar Matters for Developers](why-grammar-matters.md)

## Table of Contents

- [The Print Era: Manuals as Authority](#the-print-era-manuals-as-authority)
- [Unix and the Man Page Revolution](#unix-and-the-man-page-revolution)
- [RFCs: The Internet's Style Laboratory](#rfcs-the-internets-style-laboratory)
- [Corporate Style Guides: Apple HIG and Microsoft](#corporate-style-guides-apple-hig-and-microsoft)
- [The Google Style Guide Era](#the-google-style-guide-era)
- [Diataxis: A Framework for Documentation](#diataxis-a-framework-for-documentation)
- [From Prescriptive to Principles-Based](#from-prescriptive-to-principles-based)
- [How Open Source Changed Conventions](#how-open-source-changed-conventions)
- [Agile and Documentation Velocity](#agile-and-documentation-velocity)
- [The Modern Synthesis](#the-modern-synthesis)
- [Study Cases](#study-cases)
- [Glossary](#glossary)
- [Next Steps](#next-steps)

## Content / Material

### The Print Era: Manuals as Authority

Before software documentation was digital, it was printed. IBM, DEC, and other mainframe vendors shipped documentation sets that weighed more than the terminals they supported. These manuals followed strict conventions derived from technical publishing:

**Characteristics of print-era manuals:**

- **Linear structure.** Readers were expected to read sequentially from page one. Indexes existed but cross-referencing was limited by physical page constraints.
- **Formal register.** Third person, passive voice, no contractions. "The operator shall depress the ENTER key to initiate the transaction."
- **Comprehensive scope.** Manuals aimed to document every possible operation. A single IBM System/360 manual could exceed 1000 pages.
- **Long revision cycles.** A manual might go years between editions. Corrections required printing a new run.
- **Single voice.** A team of professional technical writers controlled all content. Users contributed nothing.

The IBM Style Guide (first published in 1967) codified these conventions. It prescribed spelling, punctuation, grammar, and formatting down to the level of how to write numbered steps. This approach worked for its time: readers were operators trained on the system, not developers modifying it.

**The limitation:** Print manuals treated readers as passive consumers of information. They could not search, annotate, or verify claims against running code. Documentation was authoritative not because it was correct but because it was printed.

### Unix and the Man Page Revolution

Unix introduced a radically different approach: documentation shipped with the source code, written by the same developers who wrote the code, accessed through a command rather than a bookshelf.

**The man page format:**

A Unix manual page follows a rigid structure:

```
NAME     — command name and one-line description
SYNOPSIS — usage syntax
DESCRIPTION — detailed explanation
OPTIONS  — flag descriptions
ENVIRONMENT — relevant environment variables
FILES    — files the command uses
EXAMPLES — usage examples
SEE ALSO — related commands
BUGS     — known issues
```

**What man pages changed:**

| Convention | Print era | Man pages |
|---|---|---|
| Author | Professional writer | Developer |
| Medium | Paper | Terminal (troff/nroff) |
| Access | Bookshelf | `man command` |
| Update cycle | Years | Ship with code |
| Tone | Formal, third-person | Direct, imperative |
| Completeness | Everything | Concise reference |
| Examples | Rare | Expected section |

**The philosophy:** Man pages embodied the Unix principle of small, focused tools. Each man page documented one command completely but briefly. The `SEE ALSO` section created a hypertext-like web of cross-references — a precursor to the linked documentation of the web.

**Limitations of man pages:**

- Designed for reference, not learning. Man pages assume you already know what the command does.
- Terse to the point of obscurity. Experienced Unix users love this; newcomers find it hostile.
- No diagrams, no images, no interactive elements.
- Versioning is ad-hoc — the man page ships with the command, but there is no guarantee the page matches the installed version.

### RFCs: The Internet's Style Laboratory

Request for Comments (RFCs) began in 1969 as informal technical notes among the researchers building the ARPANET. They evolved into the formal standard for internet protocol documentation.

**The RFC style:**

```
RFC 791: Internet Protocol
Status: INTERNET STANDARD

1. Introduction
2. Overview
3. Specification
4. Interfaces
5. Security Considerations
```

**Key stylistic innovations:**

- **Normative language.** RFC 2119 defined keywords (MUST, SHOULD, MAY) that specify requirement levels. This was a breakthrough: rather than describing what the protocol does, the spec says what implementors MUST do.
- **Open review.** Anyone can submit an RFC. Review is public. Documents are versioned and never deleted.
- **Living standards.** RFCs can be updated, obsoleted, or replaced. An RFC marked "Historic" tells implementors not to use it.
- **Structured sections.** Security Considerations (added as a mandatory section in the 1990s) forced protocol designers to think about security from the start.

**RFC influence on later style guides:** The RFC model of normative language, structured sections, and open review directly influenced the Google and Microsoft style guides. The idea that documentation should specify what MUST happen vs. what SHOULD happen is now standard in API documentation.

### Corporate Style Guides: Apple HIG and Microsoft

As personal computing expanded, companies needed consistent documentation for developers building on their platforms.

**Apple Human Interface Guidelines (HIG):**

First published in 1987 for the Macintosh, the HIG was revolutionary because it focused on the user experience of the interface, not just the technical specification of the API.

Apple HIG principles:
- **Consistency.** Similar operations should work similarly across all applications.
- **Feedback.** Every user action should produce visible feedback.
- **Forgiveness.** Users should be able to undo actions.
- **Metaphor.** Use real-world metaphors (desktop, files, folders, trash can).

The HIG writing style mirrored these principles: clear, direct, user-focused. It introduced conventions like:
- Use sentence case for UI labels (not title case)
- Address the reader as "you"
- Describe what the user does, not what the system does

**Microsoft Style Guide:**

Microsoft published its first style guide internally in the 1990s and publicly in the 2000s. It became the de facto standard for Windows ecosystem documentation.

Key tenets:
- Use "you" and "your"
- Use present tense ("The system saves the file" not "The system will save the file")
- Use active voice
- Write short sentences (25 words or fewer)
- Avoid jargon, slang, and idioms
- Write for a global audience (avoid culture-specific references)

The Microsoft Style Guide was the first major corporate guide to explicitly address international audiences — recognizing that software documentation was read worldwide by non-native English speakers.

### The Google Style Guide Era

Google published its Developer Documentation Style Guide in 2015, consolidating practices that had emerged in the open-source web development community.

**Google's contributions:**

- **Scannability.** Online readers scan, not read. Google's guide emphasizes headings, lists, tables, and code blocks that make information findable.
- **Code examples first.** Show code before explaining it. Developers learn from examples.
- **Inclusive language.** Explicit guidance on avoiding ableist and gendered language.
- **Markdown-native.** The guide assumes documentation is written in Markdown, not a word processor.
- **Tone: friendly but authoritative.** "You" throughout. Contractions are fine. But never vague or uncertain.

**Comparing the major guides:**

| Dimension | Apple HIG | Microsoft | Google |
|---|---|---|---|
| Audience | App developers | Windows developers | Web/platform developers |
| Tone | Direct, instructional | Clear, global | Friendly, concise |
| Voice | Second person | Second person | Second person |
| Headings | Phrase case | Sentence case | Sentence case |
| Serial comma | Yes | No | Yes |
| Contractions | Limited | Yes | Yes |
| Code focus | Moderate | Moderate | Heavy |
| Internationalization | Addressed | Central | Addressed |

Google's guide also popularized the principle of "one sentence = one idea" — a departure from the dense, multi-clause sentences of print-era manuals.

### Diataxis: A Framework for Documentation

In 2010, Daniele Procida proposed the Diataxis framework, which classifies documentation into four types based on audience need:

| Type | Goal | Mode | Example |
|---|---|---|---|
| Tutorials | Learning by doing | Study-oriented | "Build your first app" |
| How-to guides | Completing a task | Task-oriented | "Configure the database" |
| Reference | Looking up information | Information-oriented | "API specification" |
| Explanation | Understanding concepts | Understanding-oriented | "How authentication works" |

**Why Diataxis was a paradigm shift:**

Before Diataxis, style guides focused on *how* to write. Diataxis focused on *what* to write and *for whom*. It recognized that the same audience needs different documentation types depending on their goal.

**The stylistic implications:**

- Tutorials use welcoming, step-by-step language. No jargon without definition. Hand-holding is acceptable.
- How-to guides use imperative mood, numbered steps, and assume intermediate knowledge.
- Reference uses consistent formatting, alphabetical or categorical organization, and minimal prose.
- Explanation uses prose, diagrams, and analogies. It can be longer and more conceptual.

Modern documentation systems (Django, Kubernetes, Stripe) explicitly organize by Diataxis quadrants. The framework has become the default mental model for documentation architects.

### From Prescriptive to Principles-Based

The evolution of technical style reflects a deeper philosophical shift:

**Prescriptive era (print → 1990s):**

"Always use active voice. Never use passive voice. Always put the most important information first."

These rules were easy to enforce but fragile. A rule that served one context poorly was still a rule.

**Principles-based era (2000s → present):**

"Use active voice by default. Passive voice is acceptable when the actor is unknown or obvious. Choose the voice that makes your sentence clearest."

Principles require judgment, but they generalize across contexts better than rules.

**Why principles won:**

- **Context diversity.** Modern developers write READMEs, API docs, commit messages, design docs, Slack messages, and incident reports. Each context demands different style. One rule cannot serve all.
- **Tooling maturity.** Style linters (Vale, write-good) enforce principles, not rules. They flag "passive voice" and ask the writer to decide, rather than automatically correcting.
- **Cultural shift.** The developer community values pragmatism over pedantry. Style guidance that reads as a rulebook is ignored; style guidance that reads as advice is followed.

**The principle stack:**

```
Level 1: Core principles (clarity, conciseness, scannability)
Level 2: Context-specific guidance (tutorials use hand-holding; reference uses minimal prose)
Level 3: Mechanical rules (Oxford comma yes/no, sentence case for headings)
```

Most modern style guides focus on Level 1 and 2, with Level 3 delegated to linters and automatic formatters.

### How Open Source Changed Conventions

Open-source development introduced two new constraints that reshaped documentation style:

**1. Documentation by contributors, not by a team.**

Anyone can contribute documentation to an open-source project. This means:
- Documentation must be reviewable (like code).
- Style must be enforceable by automation (linters in CI).
- Writing must be accessible to non-native English speakers (the global open-source community).

**2. Documentation as community building.**

Open-source documentation serves not just as reference but as the project's front door. A welcoming README attracts contributors. A hostile one drives them away.

**Conventions that emerged:**

- **README-first.** Every project has a README that explains what, why, and how to start. This was not a print-era convention.
- **CONTRIBUTING.md.** Documentation that explains how to contribute, including writing style expectations.
- **Code of conduct in docs.** Projects embed community norms in their documentation.
- **Issue-driven documentation.** Bugs in documentation are tracked as issues, just like code bugs.
- **Docs-as-code.** Documentation lives in the same repository, goes through pull requests, and is tested in CI.

**The open-source style:**

| Aspect | Convention |
|---|---|
| Tone | Welcoming, inclusive |
| First interaction | README → Quick start |
| Contribution | CONTRIBUTING.md, PR review |
| Maintenance | Issues, CI checks, stale-bot |
| Audience | Global, non-native English speakers |
| Accessibility | Alt text, readable fonts, contrast |

### Agile and Documentation Velocity

Agile software development methodologies changed expectations around documentation velocity:

**The Agile Manifesto (2001):**

"Working software over comprehensive documentation."

This statement was widely misinterpreted as "documentation is unnecessary." The actual intent: documentation should not slow down software delivery. The result was a shift toward lighter, faster, more targeted documentation.

**Agile-era documentation practices:**

- **Just-in-time documentation.** Write docs when they are needed, not before. No speculative documentation for features that may change.
- **Documentation in the same sprint.** Documentation for a feature is completed in the same sprint as the feature.
- **Definition of done includes docs.** "Done" means documented.
- **Living documentation.** Documentation that changes as the code changes, updated continuously rather than in release cycles.
- **Acceptance criteria as documentation.** User stories and acceptance criteria double as lightweight specification documentation.

**The tension:**

Agile created a tension between comprehensiveness and velocity. Print-era documentation tried to be complete. Agile documentation tries to be sufficient. The modern resolution: write enough documentation that the team is not blocked, but no more. Add detail when questions arise.

### The Modern Synthesis

Current best practice in technical style draws from all these traditions:

| Source | Contribution |
|---|---|
| Print-era manuals | Structured formats, thoroughness |
| Unix man pages | Concise reference, `SEE ALSO` linking |
| RFCs | Normative language, open review |
| Apple HIG | User-centered design, consistency |
| Microsoft Style Guide | Global audience, active voice |
| Google Style Guide | Scannability, code-first, inclusive language |
| Diataxis | Documentation type classification |
| Open source | Docs-as-code, community ownership |
| Agile | Just-in-time, velocity-aware |

**The modern technical writer's toolkit includes:**

- Style guides (principles-based, not prescriptive)
- Linters (automated enforcement of mechanical rules)
- Frameworks (Diataxis for structure)
- Templates (for consistency across documents)
- Analytics (to measure what readers actually use)

**The unifying principle:** Documentation is software. It follows the same lifecycle, uses the same tooling, and deserves the same rigor as code. Style is not decoration — it is engineering practice applied to prose.

## Glossary

| Term | Definition |
|---|---|
| Agile Manifesto | 2001 document valuing working software over comprehensive documentation |
| Diataxis | Documentation classification framework: tutorials, how-to guides, reference, explanation |
| Docs-as-Code | Treating documentation with the same tooling and workflows as software code |
| HIG | Human Interface Guidelines — Apple's design and documentation standards |
| Normative language | Terms (MUST, SHOULD, MAY) that specify requirement levels in standards |
| Prescriptive style | Style guidance based on fixed rules that must be followed |
| Principles-based style | Style guidance based on general principles that require judgment |
| RFC | Request for Comments — internet standard document format |
| Scannability | How easily a reader can find relevant information by scanning |
| Troff/nroff | Document formatting systems used for Unix man pages |
| Velocity | The rate at which documentation can be produced and updated |
| AI hallucination | Generation of plausible but incorrect content by a language model |
| Style translation | Converting documentation between Diataxis quadrants using automated tools |

### The Impact of AI on Technical Style

Large language models (LLMs) are the newest force reshaping technical writing conventions. AI-assisted writing tools (GitHub Copilot, ChatGPT, Claude) generate documentation from code, translate between styles, and suggest improvements.

**How AI changes the writer's role:**

| Era | Writer's role | Primary output |
|---|---|---|
| Print | Author of authoritative manuals | Static documents |
| Web | Curator of linked content | Hypertext, searchable |
| Open source | Community documentation contributor | Docs-as-code |
| AI-assisted | Editor of generated content | Verified, reviewed generation |

**Opportunities:**

- **Documentation generation from code.** AI can produce initial drafts from source code, tests, and comments. The writer shifts from writing to reviewing and refining.
- **Style translation.** AI can convert a reference document into a tutorial or vice versa — adapting content to different Diataxis quadrants.
- **Consistency checking.** AI can enforce style rules more flexibly than linters, catching tone inconsistencies and structural problems.
- **Translation and localization.** AI-generated translations keep documentation up to date across languages without manual effort per language.

**Risks:**

- **Hallucination.** AI can generate plausible but incorrect documentation. Every AI-generated claim must be verified against the code.
- **Homogenization.** If all projects use AI with similar training data, documentation may lose its distinctive voice. Projects may all sound the same.
- **Loss of judgment.** Principles-based style requires human judgment about audience, context, and purpose. AI can suggest; it cannot decide.
- **Maintenance burden.** AI-generated documentation that is not understood by the human editors is harder to maintain when the code changes.

**The emerging best practice:**

Use AI for the mechanical work: first drafts, formatting, consistency, translation. Reserve human effort for: audience analysis, structural decisions, verification against code, and tone calibration. Style guides must now address how to review AI-generated content — a skill few writers currently have.

## Quick References

- [Google Developer Documentation Style Guide](https://developers.google.com/style) — modern principles-based style guide
- [Microsoft Style Guide](https://docs.microsoft.com/style-guide) — global-audience technical writing
- [Apple HIG](https://developer.apple.com/design/human-interface-guidelines/) — user-centered documentation principles
- [Diataxis Framework](https://diataxis.fr/) — documentation type classification
- [Vale: A Style Linter](https://vale.sh/) — automated style enforcement
- [Write the Docs](https://www.writethedocs.org/) — community of documentation practitioners

## Next Steps

- [Style Guides for Developers](../style-guides.md) — practical use of modern style guides
- [Diataxis Framework in Practice](../../technical-writing/diataxis.md) — applying the framework to real projects
