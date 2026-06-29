# Style Guides & Consistency for Technical Writing

## Description

Creating and maintaining a project-level style guide goes beyond choosing a reference like Microsoft or Google. This document covers how to establish, enforce, and evolve your own style guide — from terminology governance and voice-and-tone decisions to automated style checking with tools like Vale and team adoption strategies.

## Prerequisites

- [Style Guides](../grammar-and-style/style-guides.md) — understanding the major reference style guides before creating a project-level one
- [Docs as Code](docs-as-code.md) — automated style enforcement relies on the docs-as-code toolchain

## Table of Contents

- [Why a Project-Level Style Guide?](#why-a-project-level-style-guide)
- [Different from Reference Style Guides](#different-from-reference-style-guides)
- [Consistency in Terminology](#consistency-in-terminology)
- [Voice and Tone](#voice-and-tone)
- [Formatting Conventions](#formatting-conventions)
- [Code Style in Documentation](#code-style-in-documentation)
- [Automated Style Checking with Vale](#automated-style-checking-with-vale)
- [Other Style Checking Tools](#other-style-checking-tools)
- [Style Guide Versioning](#style-guide-versioning)
- [Team Adoption of Style Guides](#team-adoption-of-style-guides)
- [Creating a Style Guide from Scratch](#creating-a-style-guide-from-scratch)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Why a Project-Level Style Guide?

Reference style guides like Microsoft, Google, and Apple cover general technical writing conventions. They do not cover your project's specific terminology, brand voice, or product names.

A project-level style guide fills this gap. It answers questions that no external guide can:

```text
- What do we call our users? ("developers", "engineers", "customers")
- How do we refer to our product? ("The Platform", "our platform")
- What is the approved spelling of our proprietary terms?
- How do we format error messages in documentation versus in the UI?
- How do we handle version references? ("v2.0" or "version 2.0")
```

**The cost of not having a project style guide:**

```text
- Inconsistent documentation confuses readers
- Each writer uses different conventions
- Review cycles are longer (same issues flagged repeatedly)
- Brand perception suffers
- Translation costs increase (inconsistent terminology)
- New writer onboarding takes longer
```

### Different from Reference Style Guides

A project-level style guide is not a replacement for a reference guide. It is a supplement.

**Relationship between reference and project guides:**

```text
Reference guide: "Use sentence case for headings."
Project guide: "Use sentence case. Exception: proper nouns in product names
keep their capitalization (GitHub Actions, VS Code)."

Reference guide: "Use active voice."
Project guide: "Use active voice. Exception: passive when the actor is unknown."
```

**What a project style guide covers that a reference guide does not:**

```text
Product-specific: Official product name, approved abbreviations, deprecated terms
Process-specific: Review workflow, approval requirements, release cycle
Tool-specific: Markdown formatting, screenshot naming, link format
```

### Consistency in Terminology

Terminology consistency is the most visible effect of a good style guide.

**Terminology governance process:**

```text
1. Audit existing documentation for inconsistent terminology
2. Decide on the approved term for each concept
3. Document decisions in a terminology table
4. Add deprecated terms to a "do not use" list
5. Configure automated tools to flag deprecated terms
```

**Terminology table format:**

```text
| Approved term | Deprecated terms | Context |
|--------------|------------------|---------|
| sign in      | log in, login    | All authentication actions |
| API key      | API token, secret key | Use consistently across docs |
| set up (verb)| setup (noun ok)  | "Set up the application." vs. "The setup process." |
| runtime      | run-time         | Always one word for noun form |
```

**Handling product names:**

```text
Rule: Always use the official product name exactly as trademarked.
"GitHub Actions" not "Github Actions" or "GH Actions"
"Node.js" not "NodeJS" or "node.js"
"VS Code" not "VSCode" or "vs code"
```

**New term introduction process:**

```text
1. Product team proposes a name
2. Docs team reviews for: consistency, translatability, searchability, conflicts
3. Term is added to the terminology table
4. Documentation is updated
5. Automated tools are configured to flag the old term
```

### Voice and Tone

Voice is the consistent personality of your documentation. Tone varies by context within that voice.

**Defining your documentation voice:**

```text
Voice dimensions to define:

Formal vs. casual:
"You must restart the server." (formal)
"Restart the server." (neutral)

Our voice: Expert peer
- Knowledgeable but not condescending
- Address the reader as "you"
- Prefer imperative mood for instructions
- Use precise technical terms, avoid jargon
- No humor, sarcasm, or cultural references
```

**Tone variations by document type:**

```text
Tutorial: Encouraging and patient. "You will learn how to..."
How-to guide: Direct and efficient. "To configure authentication..."
Explanation: Analytical and thoughtful. "The architecture is designed this way because..."
Reference: Neutral and precise. "Parameter: type, required, default"
```

**Tone pitfalls to document:**

```text
Avoid:
- Apologetic: "Sorry for the inconvenience..."
- Dismissive: "Just run the command..."
- Overly casual: "Go ahead and..."
- Uncertain: "You may want to try..."
- Promotional: "Our amazing platform..."
```

### Formatting Conventions

**Heading rules:**

```text
- Sentence case: "Installing the package" (not "Installing The Package")
- Only one H1 per page (the page title)
- Do not skip heading levels (H2 to H3, not H2 to H4)
- Headings must be unique within a page
```

**Code formatting:**

```text
Inline code (backticks): variable names, function names, file names, commands
Code blocks: Fenced with language identifier
```javascript
const x = 1;
```

UI elements: Bold for labels — "Click **Save**."
Error messages: In quotes — "File not found" message appears.
```

**List conventions:**

```text
Numbered lists: Sequential steps (each item is a complete sentence)
Bullet lists: Non-sequential items (parallel structure, capital first word)
Definition lists: Term on one line, definition on the next
```

**Image guidelines:**

```text
- Maximum width: 800px, maximum size: 200KB
- Format: PNG for screenshots, SVG for diagrams
- Alt text: Required for every image
- Crop to relevant area, no full-screen shots
- Naming: {section}-{description}-{step}.png
```

### Code Style in Documentation

**Code example principles:**

```text
Readable over clever:
// Good
const result = await api.get("/users");

// Bad — clever but confusing
await Promise.resolve(api.get("/users")).then(r => ...)

Complete but minimal:
// Good — includes imports and setup
import { Client } from "@example/sdk";
const client = new Client({ apiKey: process.env.API_KEY });
const users = await client.users.list();

// Bad — missing context
const users = await client.users.list();
```

**Language identifiers for code blocks:**

```text
Always specify: javascript, python, bash, yaml, json, sql, html, css, go, ruby
Use `text` or `console` for command output:

```bash
node --version
```
```console
v18.12.0
```

```

**Code comments in examples:**

```text
Explain the business logic, not the language syntax:
// Good: Fetch the user and display their name
// Bad: Awaits the result of api.get and assigns it to user

Use "// Result:" or "// Output:" to show expected output:
const sum = 1 + 2;
// Result: 3
```

**Placeholder conventions:**

```text
Use curly braces for placeholders:
curl -X GET "https://api.example.com/{version}/users/{userId}"

Document what each placeholder means below the code block.
```

### Automated Style Checking with Vale

Vale is the industry-standard tool for automated style checking in documentation.

**Vale architecture:**

```text
Vale works with "styles" — collections of rules. Each rule checks for a
specific pattern and alerts at a configurable level (suggestion, warning, error).

Built-in styles: Microsoft, Google, write-good, alex, Joblint
Custom styles: Project-specific terminology, voice rules, naming conventions
```

**Setting up Vale:**

```text
1. Install: brew install vale (macOS) or download from vale.sh
2. Create .vale.ini:
StylesPath = .vale/styles
MinAlertLevel = suggestion
[*.md]
BasedOnStyles = Google, write-good, ProjectStyle

3. Create project rules:
.vale/styles/ProjectStyle/Terms.yml:
extends: substitution
message: "Use '%s' instead of '%s'"
level: error
swap:
  "log in": "sign in"
  "execute": "run"
```

**Custom Vale rule types:**

```text
Substitution: Replace one term with another
extends: substitution
swap:
  "login": "sign in"

Existence: Flag terms that should not appear
extends: existence
message: "Avoid 'simply'"
level: warning
tokens:
  - simply
  - just

Capitalization: Check heading case
extends: capitalization
match: $sentence
```

**Vale in CI:**

```yaml
name: Prose Lint
on: [pull_request]
jobs:
  vale:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: errata-ai/vale-action@v2
        with:
          styles: |
            https://github.com/errata-ai/Google/releases/latest/download/Google.zip
```

### Other Style Checking Tools

**write-good:**

```text
npm install -g write-good
write-good document.md
Catches: weasel words, passive voice, cliches
```

**alex:**

```text
npm install -g alex
alex document.md
Catches: insensitive language, gender bias, ableism
```

**markdownlint:**

```text
npm install -g markdownlint-cli
markdownlint docs/
Catches: Markdown formatting and syntax issues
```

**proselint:**

```text
pip install proselint
proselint document.md
Catches: redundancy, jargon, cliches
```

**cSpell:**

```text
npm install -g cspell
cspell "docs/**/*.md"
Catches: spelling errors with custom dictionary
```

**Integration strategy for CI:**

```text
Run in this order (fastest first):
1. markdownlint — formatting (fast)
2. cspell — spelling (fast)
3. Vale — style (comprehensive)
4. alex — inclusivity (specific)
```

### Style Guide Versioning

Your style guide evolves over time. Version it like code.

**Semver for style guides:**

```text
Major (1.0.0 -> 2.0.0): Breaking style changes, terminology migrations
Minor (1.0.0 -> 1.1.0): New rules, new terminology, clarifications
Patch (1.0.0 -> 1.1.1): Corrections, additional examples, typos
```

**Style guide file structure:**

```text
/style-guide
  /v1
    index.md
    voice-and-tone.md
    terminology.md
  /v2
    index.md
    ...
  CHANGELOG.md

CHANGELOG format:

## 2.0.0 (2025-06-01)
### Changed
- Voice from "friendly expert" to "expert peer"
- Terminology: "user" changed to "developer"

### Added
- New section: screenshot conventions
- New Vale rule: product name enforcement
```

**Communicating version updates:**

```text
1. Update CHANGELOG
2. Announce in team channels
3. Update Vale config for new version
4. Run bulk scan of existing docs
5. Archive previous version
```

### Team Adoption of Style Guides

A style guide is only effective if the team uses it.

**Barriers to adoption:**

```text
- Guide is too long (nobody reads 100-page guides)
- Guide is hard to find (buried in wiki)
- Guide is not enforced (nobody checks compliance)
- Guide is irrelevant (covers things team does not do)
- Guide changes too often (writers cannot keep up)
```

**Strategies for adoption:**

```text
1. Start small. Publish a one-page quick reference first.
2. Make it discoverable. Link prominently in the README.
3. Automate enforcement. Vale catches violations automatically.
4. Involve the team. Let writers contribute rules to the guide.
5. Celebrate consistency. Acknowledge PRs with zero style violations.
6. Iterate based on feedback. Revise rules that cause confusion.
7. Provide templates. Pre-configured templates reduce compliance effort.
8. Include examples. Every rule needs a good and bad example.
```

**New writer onboarding:**

```text
Day 1: Read quick reference, set up Vale in editor
Week 1: Write draft using templates, submit for style review
Month 1: Contribute a rule, review another writer's doc for style
```

**Measuring adoption:**

```text
- Vale violation rate (violations per 1000 words, target: decreasing)
- Style review time in PRs (target: decreasing as Vale catches more)
- Deprecated term usage in new documents (target: zero)
- Writer satisfaction survey (target: 4+/5)
```

### Creating a Style Guide from Scratch

**Step 1: Define scope**

```text
What does the guide cover? All docs or just certain types?
All products or just one? A focused guide is better than a comprehensive
one that nobody reads.
```

**Step 2: Choose a base reference**

```text
Select a reference (Google, Microsoft, or Apple) and state that
the project guide supplements this reference.
```

**Step 3: Document decisions, not debates**

```text
Record the decision and rationale, not the debate:

Decision: Use "sign in" not "log in"
Rationale: Consistent with industry standard.
```

**Step 4: Provide templates**

```text
Include templates that follow the style guide:
- Tutorial template
- How-to guide template
- Reference page template
- Explanation page template
```

**Step 5: Automate enforcement**

```text
Configure Vale with style rules before publishing the guide.
Create rules for: terminology, prohibited terms, voice, formatting.
```

**Step 6: Review and iterate**

```text
Monthly: review new style questions
Quarterly: review violation data, adjust rules
Annually: full review and version update
```

**Minimum viable style guide (one page):**

```markdown
# Project Style Guide (Quick Reference)

## Voice
Write as an expert peer. Direct, precise, practical.

## Terminology
- "sign in" (not "log in" or "login")
- "set up" (verb), "setup" (noun)
- "API key" (not "API token")

## Formatting
- Sentence case for headings
- Bold for UI elements
- Code font for code, commands, file paths
- Serial (Oxford) comma in lists

## Code examples
- Language identifier in code blocks
- Realistic data (not "foo" and "bar")
- Show both input and output

## Vale
This style is enforced by Vale. Install the Vale extension in your editor.
```

## Study Cases

### Case 1: Terminology Inconsistency Cleanup

A company used "delete", "remove", "archive", "deactivate", and "suspend" interchangeably.

```text
Problem: Users did not know which action permanently deleted data versus
temporarily hiding it. Support tickets about data loss were common.

Solution:
1. Defined each term precisely: delete (permanent), archive (hidden but restorable),
   deactivate (disabled but retained), suspend (temporary due to policy)
2. Updated UI to match terminology
3. Created Vale rules to flag misuse
4. Audited all documentation

Result: Support tickets about data deletion dropped by 80%.
```

### Case 2: Vale Rollout to a Large Team

A team of 15 writers adopted Vale after years of inconsistent style.

```text
Approach:
1. Selected Google as base style
2. Created custom rules for project-specific terminology
3. Added Vale to CI (non-blocking for first 3 months)
4. Ran automated reports showing violation trends
5. Celebrated teams that reduced violation counts

Results at 6 months:
- Violations per document: from 24 to 3
- Style review time in PRs: reduced by 65%
- Writer satisfaction: from 2.8/5 to 4.2/5
```

## Examples

### Example 1: Project Style Guide Entry

```text
Term: "data store"

Use "data store" when referring to the abstract storage concept.
Use the specific product name (PostgreSQL, Redis) when the technology
matters to the reader.

Correct: "The data store persists user session data."
Incorrect: "The database persists user session data." (when not confirmed)

Approved: data store
Deprecated: DB, datastore, data-source
```

### Example 2: Vale Custom Rule

```yaml
# .vale/styles/ProjectStyle/Terms.yml
extends: substitution
message: "Use '%s' instead of '%s'"
level: error
ignorecase: true
swap:
  "log in": "sign in"
  "execute": "run"
  "leverage": "use"
  "utilize": "use"
  "drill down": "explore"
  "circle back": "return"
```

### Example 3: Voice and Tone Statement

```markdown
# Voice and Tone

## Our Voice: Expert Peer

We write as an experienced colleague helping another developer.

## Tone by Document Type

| Type | Tone | Example opening |
|------|------|-----------------|
| Tutorial | Encouraging | "In this tutorial, you will build..." |
| How-to guide | Direct | "To deploy your application..." |
| Explanation | Analytical | "The system processes requests through..." |
| Reference | Neutral | "Parameter: type, required, default" |

## Prohibited Language

- "Just" or "simply" (implies task is easy)
- "Obviously" or "of course" (condescending)
- "Please" in instructions (use imperative mood)
```

### Example 4: Code Style in Documentation

```markdown
## Code Example Standards

Always specify the language:
```javascript
// Good
const x = 1;
```

```
// Bad — no syntax highlighting
const x = 1;
```

Show imports so examples are copy-pasteable:
```javascript
import { Client } from "@example/sdk";
const client = new Client();
```

Show expected output:
```javascript
console.log(1 + 1);
// Result: 2
```
```

### Example 5: Style Guide Quick Reference Card

```markdown
## Quick Reference

### Capitalization
- Headings: Sentence case ("Installing the package")
- Product names: As trademarked ("GitHub Actions")
- UI elements: As they appear ("Select **Save**")

### Punctuation
- Oxford comma: Yes ("a, b, and c")
- Dashes: Em dash (--) for parentheticals

### Terminology
- sign in (not log in, login)
- set up (verb), setup (noun)
- run (not execute)
- use (not leverage)

### Code
- Inline: Backticks for code, file names, commands
- Blocks: Fenced with language identifier
- Line length: 80 characters maximum

### Images
- Max width: 800px, max size: 200KB
- Alt text: Required
- Format: PNG for screenshots, SVG for diagrams
```

## Glossary

| Term | Definition |
|------|------------|
| Project style guide | A document defining writing conventions specific to a project or organization |
| Voice | The consistent personality and tone of documentation across all content |
| Terminology governance | The process of managing approved and deprecated terms |
| Vale | An open-source prose linter that enforces style guide rules automatically |
| Style | A collection of Vale rules implementing a specific style guide |
| Substitution rule | A Vale rule that replaces one term with another |
| Existence rule | A Vale rule that flags specific terms or patterns |
| Changelog | A log of changes between versions of the style guide |
| Quick reference | A concise summary of the most important style guide rules |
| Onboarding | The process of introducing new writers to the style guide and tools |
| Violation rate | A metric measuring style guide compliance (violations per word) |
| Base reference | The primary external style guide that the project guide supplements |

## Quick References

- [Vale](https://vale.sh/) — open-source prose linter
- [write-good](https://github.com/btford/write-good) — weasel word detection
- [alex](https://alexjs.com/) — insensitive language detection
- [markdownlint](https://github.com/DavidAnson/markdownlint) — Markdown linting
- [cSpell](https://cspell.org/) — code spell checker
- [proselint](http://proselint.com/) — prose linter
- [Google Developer Documentation Style Guide](https://developers.google.com/style) — reference style guide
- [Microsoft Style Guide](https://learn.microsoft.com/en-us/style-guide/welcome/) — reference style guide
- [Apple Style Guide](https://support.apple.com/guide/applestyleguide/) — reference style guide

## Next Steps

- [Diataxis Framework](diataxis.md) — organizing documentation into modes that style guide rules apply across
