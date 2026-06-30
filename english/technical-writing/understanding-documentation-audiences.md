# Understanding Documentation Audiences

## Description

Before writing a single line of documentation, you must know who will read it. Different audiences need different content, structure, depth, and tone. This document covers audience personas, developer journeys, skill-level assessment, and techniques for understanding who you are writing for — so you can create documentation that actually helps people.

## Prerequisites

- [Documentation Architecture & Information Design](doc-architecture.md) — audience analysis determines how content should be structured and organized

## Table of Contents

- [Why Audience Matters](#why-audience-matters)
- [Documentation Audience Personas](#documentation-audience-personas)
- [Developer Journeys](#developer-journeys)
- [Skill Level Assessment](#skill-level-assessment)
- [Audience-Driven Doc Planning](#audience-driven-doc-planning)
- [The Documentation Empathy Gap](#the-documentation-empathy-gap)
- [Techniques for Understanding Your Audience](#techniques-for-understanding-your-audience)
- [Writing for Mixed Audiences](#writing-for-mixed-audiences)
- [Accessibility in Documentation](#accessibility-in-documentation)
- [Persona Templates for Documentation Planning](#persona-templates-for-documentation-planning)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Why Audience Matters

Documentation fails when it answers questions the reader is not asking. A developer debugging a production outage does not want a tutorial. A beginner evaluating your API does not want a deep-dive into the architecture.

Every page should answer: "Who is this for and what are they trying to do?" Without this clarity, you write for nobody and frustrate everybody.

**The cost of ignoring audience:**

```text
Beginners:  Feel stupid, leave, write negative reviews
Power users: Feel slowed down, find alternatives
Integrators: File support tickets for missing specs
Evaluators:  Cannot assess fit, choose a competitor
Maintainers: Waste time updating docs they never use
```

**What changes when you write for a specific audience:**

```text
Vocabulary:      Level of jargon and assumed knowledge
Depth:           How much explanation vs. reference
Structure:       Tutorial path or quick lookup reference
Examples:        Simple toy examples or production patterns
Assumptions:     What the reader already knows
Goals:           Learning, completing, debugging, or contributing
```

### Documentation Audience Personas

Six core personas appear across most documentation projects. Each needs different content.

**Persona 1: The Newcomer**

```text
Context:  First interaction with your product
Goal:     Understand what it does and whether it is worth learning
Needs:    Overview, value proposition, quickstart, "hello world"
Fears:    Wasting time, installing something that does not work
Doc type: README, landing page, getting-started guide, overview
Behavior: Reads the README first; abandons if first example fails
```

**Persona 2: The Regular User**

```text
Context:  Uses the product regularly for common tasks
Goal:     Complete routine tasks efficiently
Needs:    How-to guides, reference docs, recipes, common workflows
Fears:    Breaking something, missing a best practice
Doc type: How-to guides, tutorials, reference pages, FAQs
Behavior: Returns to docs for infrequent tasks; complains when workflows change
```

**Persona 3: The Power User**

```text
Context:  Deep expertise with the product, uses advanced features
Goal:     Configure, optimize, extend, or automate
Needs:    Configuration reference, API docs, internals, migration guides
Fears:    Hitting undocumented limits, incompatible changes
Doc type: API reference, configuration docs, changelogs, internals
Behavior: Skips tutorials; goes directly to reference and changelogs
```

**Persona 4: The System Integrator**

```text
Context:  Integrating the product into a larger system
Goal:     Connect, authenticate, handle errors, maintain
Needs:    API specs, authentication docs, error codes, rate limits
Fears:    Breaking changes, undocumented edge cases, poor error handling
Doc type: API reference, SDK docs, integration guides, status codes
Behavior: Tests every documented endpoint; files detailed accuracy bugs
```

**Persona 5: The Evaluator**

```text
Context:  Comparing your product against alternatives
Goal:     Decide whether to adopt your product
Needs:    Feature comparisons, pricing, limitations, roadmap
Fears:    Locking in before discovering limitations
Doc type: Overview, comparisons, limitations, roadmap, use cases
Behavior: Tries the quickstart; abandons if docs look incomplete
```

**Persona 6: The Maintainer**

```text
Context:  Maintaining or contributing to the project
Goal:     Understand internals, contribute changes, update docs
Needs:    Contribution guide, architecture docs, testing guide
Fears:    Breaking the build, violating conventions unwittingly
Doc type: Contributing guide, developer docs, style guide, changelog
Behavior: Reads CONTRIBUTING.md before PRs; files doc debt issues
```

**Persona priority matrix:**

```text
| Phase              | Primary Persona          | Secondary Persona    |
|--------------------|--------------------------|----------------------|
| Pre-adoption       | Evaluator                | Newcomer             |
| First use          | Newcomer                 | Regular user         |
| Daily use          | Regular user             | Power user           |
| Advanced use       | Power user               | System integrator    |
| Integration        | System integrator        | Power user           |
| Maintenance        | Maintainer               | Power user           |
```

### Developer Journeys

Developers pass through journeys that change what they need from documentation.

**The Learning Journey:**

```text
Trigger:     New technology, new job, new project
Goal:        Build mental model and basic competence
Doc needed:  Tutorials, overviews, concept guides
Pitfall:     Tutorial hell — following steps without understanding
Good docs:   Scaffolded learning, verification points, progression
Path:        README → Quickstart → Tutorial → Concept → Next tutorial
```

**The Implementing Journey:**

```text
Trigger:     Need to complete a specific task
Goal:        Finish quickly with correct implementation
Doc needed:  How-to guides, reference pages, code examples
Pitfall:     Missing steps, undocumented edge cases
Good docs:   Copy-pasteable examples, complete workflows, error handling
Path:        Search → How-to guide → Reference → Code → Done
```

**The Debugging Journey:**

```text
Trigger:     Something is broken
Goal:        Diagnose and fix the problem
Doc needed:  Error code reference, troubleshooting guides, FAQs
Pitfall:     Unhelpful error messages, no search results
Good docs:   Searchable error codes, clear recovery steps
Path:        Search error → Error page → Troubleshooting → Solution
```

**The Upgrading Journey:**

```text
Trigger:     New version released, breaking change announced
Goal:        Upgrade without breaking existing work
Doc needed:  Changelog, migration guide, breaking changes, deprecation notices
Pitfall:     Undocumented breaking changes, incomplete migration guides
Good docs:   Before/after examples, automated migration tools, timelines
Path:        Changelog → Migration guide → Breaking changes → Updated ref
```

**The Contributing Journey:**

```text
Trigger:     Want to give back, fix a bug, add a feature
Goal:        Submit a correct, accepted contribution
Doc needed:  Contribution guide, code of conduct, style guide, testing guide
Pitfall:     Unclear process, hostile response, rejected PRs
Good docs:   Clear PR workflow, review criteria, first-issue guidance
Path:        CONTRIBUTING.md → Good first issues → PR template → Style guide
```

**Journey-doc mapping:**

```text
| Journey        | Primary Doc Type    | Urgency    | Depth Needed      |
|----------------|---------------------|------------|-------------------|
| Learning       | Tutorials           | Low        | High (concepts)   |
| Implementing   | How-to guides       | Medium     | Medium (steps)    |
| Debugging      | Troubleshooting     | High       | Low (facts)       |
| Upgrading      | Migration guides    | High       | Medium (changes)  |
| Contributing   | Contribution guides | Low        | High (process)    |
```

### Skill Level Assessment

Skill level determines what the reader already knows and what they need explained.

**Skill levels in developer documentation:**

```text
Junior (0-2 years experience):
  Needs: Step-by-step instructions, concept explanations, complete examples
  Assumptions: Basic programming knowledge, familiar with common tools
  Best format: Tutorials with verification points, glossary links

Mid-level (2-5 years):
  Needs: How-to guides, patterns, best practices, reference docs
  Assumptions: Comfortable with the domain, knows common patterns
  Best format: How-to guides with context, concise reference, code patterns

Senior (5+ years):
  Needs: Architecture docs, performance tuning, advanced configuration
  Assumptions: Deep domain knowledge, can fill gaps from reference
  Best format: Reference docs, internals, configuration specs, API contracts
```

**What each level needs from documentation:**

```text
| Aspect              | Junior                     | Mid-level                 | Senior                   |
|---------------------|----------------------------|---------------------------|--------------------------|
| Getting started     | 20-min tutorial            | Quickstart with key steps | CLI flags or API call    |
| How-to guides       | Every step explained       | Steps with context        | Minimal steps, edge cases|
| Reference           | Annotated with examples    | Clean spec                | Raw spec or OpenAPI      |
| Error handling      | Common errors with fixes   | Error codes with meaning  | Error spec, recovery API |
| Architecture        | High-level overview        | Component relationships   | Detailed internals       |
| Examples            | Copy-paste, full project   | Focused snippets          | Production patterns      |
```

### Audience-Driven Doc Planning

Once you know your audiences, you plan which documents serve which readers.

**Audience-doc type mapping:**

```text
| Doc Type           | Primary Audience        | Secondary Audience      |
|--------------------|-------------------------|-------------------------|
| README             | Newcomer, Evaluator     | Regular user            |
| Getting started    | Newcomer                | Evaluator               |
| Tutorials          | Newcomer, Junior        | Regular user            |
| How-to guides      | Regular user            | Mid-level developer     |
| Concept guides     | Junior, Mid-level       | Senior                  |
| API reference      | Power user, Integrator  | Regular user            |
| Configuration ref  | Power user              | System integrator       |
| Error reference    | All (during debugging)  | -                       |
| Migration guide    | Regular user, Power     | System integrator       |
| Changelog          | Maintainer, Power       | Regular user            |
| Contribution guide | Maintainer              | Contributor             |
| Architecture docs  | Senior, Maintainer      | Mid-level               |
```

**Planning questions for each document:**

```text
Before writing any document, answer:
1. Who is the primary audience? (pick one persona)
2. What journey are they on? (learning, implementing, etc.)
3. What is their skill level? (junior, mid, senior)
4. What question does this document answer?
5. What is the minimum they need to know?
6. What do they already know? (so you do not repeat it)
7. What do they NOT need? (so you do not distract them)
```

### The Documentation Empathy Gap

The empathy gap is the difference between what the writer assumes the reader knows and what the reader actually knows. Developers writing for developers are especially prone to this.

**Why developers write for themselves:**

```text
Curse of knowledge: Once you understand something, it is impossible
to remember what it was like not to understand it.

Assumed context: The writer knows the product intimately and assumes
the reader shares that context.

Speed bias: Writing "run the command" is faster than explaining what
the command does and why.

Jargon blindness: Terms that feel natural to the team are foreign to
outsiders.

Recency illusion: If the writer just learned something, they assume
it is common knowledge.
```

**Symptoms of the empathy gap:**

```text
- Documentation starts with technical details before explaining the problem
- Prerequisites are assumed but not listed
- Error messages say "something went wrong" without guidance
- Tutorials skip the first two steps because "everyone knows that"
- Examples use internal terminology without definitions
```

**Bridging the gap:**

```text
Technique 1: Fresh-eyes review
  Have someone who has never seen the product read your docs.
  Watch where they get stuck. Fix those spots.

Technique 2: The "why" after every "what"
  After every instruction, add one sentence explaining why.

Technique 3: Prerequisite checklist
  Before writing, list everything the reader must know.
  Put this list at the top. Link to resources for each item.

Technique 4: Read your own docs cold
  Wait 2 weeks after writing, then read your docs as if you
  have never seen the product. Mark every unclear spot.

Technique 5: Track search queries
  What are users searching for? If they search for terms that
  seem "obvious" to you, your docs lack context.
```

### Techniques for Understanding Your Audience

You cannot rely on intuition alone. Use these techniques to discover who your audience is and what they need.

**Analytics:**

```text
What to track:
- Page views and unique visitors per page
- Search queries (what terms, how many results returned)
- Time on page and scroll depth
- Bounce rate (entered and left without action)
- Exit pages (where do readers give up?)

Actionable patterns:
- High bounce rate + low time → title does not match content
- Low scroll depth → content too long or not engaging
- Repeated search for same term → missing content or poor labeling
- High traffic + low satisfaction → needs rewrite
```

**User research methods:**

```text
Surveys:
  Embedded: "Was this page helpful?" after each page
  Periodic: Quarterly documentation satisfaction survey
  Targeted: Survey users who filed support tickets

Interviews:
  Recruit users from different segments (new, power, integrator)
  Ask: "What is the hardest thing to find in our docs?"

Card sorting:
  Give users cards with topic names, ask them to organize.
  Validates IA matches their mental model.

Tree testing:
  Show users the navigation tree without content.
  Ask: "Where would you click to find X?"
```

**Support tickets as a source:**

```text
Mine support tickets for documentation gaps:
- Questions that come up repeatedly → create a how-to guide
- Misunderstood concepts → clarify terminology or add prerequisites
- Confusing error messages → improve error docs or the error itself
- Common workarounds → document the proper solution

Process:
1. Tag tickets by topic (authentication, deployment, etc.)
2. Tag tickets by root cause (missing, unclear, wrong)
3. Monthly review of top documentation-related tickets
4. Prioritize fixes by ticket volume
5. After fix, measure ticket reduction
```

**Community signals:**

```text
- Stack Overflow: "How do I X with Y?" → missing how-to guides
- GitHub Issues: Documentation bugs → fix gaps
- Discord/Slack: Questions in support channels → missing explanations
- Blog comments: Reader confusion → clarify sections

For each source, collect:
- The exact question asked
- How many people upvoted or echoed it
- Whether an acceptable answer exists in the docs
```

**Analytics-to-action table:**

```text
| Signal                          | Likely Cause              | Action                        |
|---------------------------------|---------------------------|-------------------------------|
| High search for term, 0 results | Missing content           | Write page covering the term  |
| High bounce, low time           | Title mismatches content   | Rewrite to match title        |
| Low scroll, high exit mid-page  | Content too long          | Split into multiple pages     |
| High support tickets on topic   | Unclear documentation     | Audit and rewrite topic       |
| Low traffic to important page   | Poor discoverability      | Add links from related pages  |
```

### Writing for Mixed Audiences

Most documentation serves multiple audiences. The challenge is serving everyone without frustrating anyone.

**Progressive disclosure:**

Show the common case first, then reveal deeper detail for readers who need it.

```text
Level 1: Quick answer (for everyone)
  "The API key goes in the Authorization header."

Level 2: Standard explanation (for most readers)
  "Include the API key in the Authorization header using Bearer:
   Authorization: Bearer <your-api-key>"

Level 3: Detailed context (for advanced readers)
  "The Bearer scheme sends a token as a credential. The server
   validates the token against your account permissions."
```

**Implementation patterns:**

```text
Pattern 1: Summary-first with expandable detail

## Creating a User
POST /api/v2/users with email and name.
<details>Full request body schema, validation, response example.</details>

Pattern 2: Audience callouts

Run the following command:
```bash
deploy --environment production
```

> **For beginners:** The --environment flag tells the deploy tool
> which environment to target.

> **For experts:** You can also use --env prod as shorthand.

Pattern 3: Section-level audience labels

### Quick Setup (New Users)
Two options: environment variables or config file.

### Advanced Configuration (Power Users)
Override defaults, custom middleware, logging, performance tuning.
```

**Layered content approach:**

```text
Layer 1 (core):  Essential info. Read this to accomplish the task.
                 2 minutes. Happy path only.

Layer 2 (detail): Deeper explanation. Read this to understand why.
                  5 minutes. Edge cases and rationale.

Layer 3 (expert): Advanced options. Read this to optimize.
                  10 minutes. Internals and configuration.

Each layer is clearly separated. Readers self-select their depth.
```

### Accessibility in Documentation

Accessibility is not optional. Documentation must work for all developers, regardless of language, ability, or context.

**Non-native English readers:**

```text
Over 60% of developers are non-native English speakers.

Rules for non-native readers:
- Short sentences (under 20 words where possible)
- One idea per sentence
- Define jargon and acronyms on first use
- Avoid idioms ("piece of cake", "hit the ground running")
- Avoid phrasal verbs ("set up", "tear down")
- Use "and", "or", "then" instead of semicolons

Bad: "Simply spin up a container and hook it into your stack."
Good: "Create a container and connect it to your application."
```

**Screen reader accessibility:**

```text
- Headings: Use proper hierarchy (H1, H2, H3), never skip levels
- Links: Descriptive text, never "click here" or "read more"
- Images: Alt text on every image describing the content
- Code blocks: Use language identifiers for syntax announcement
- Tables: Use proper table headers
- Color: Do not convey information with color alone
- Contrast: Meet WCAG AA ratio (4.5:1 for normal text)
```

**Cognitive load reduction:**

```text
1. Consistent structure — every page has the same sections in the same order
2. Chunking — break long procedures into numbered steps (3-7 per section)
3. Minimal choices — pick the most common approach, mention alternatives in callouts
4. Visual hierarchy — use headings, whitespace, and lists for scannability
5. Working memory support — prerequisites at top, verification after each step
```

**Accessibility checklist:** Headings use correct hierarchy; all images have alt text; links have descriptive text; code blocks specify language; color is not the only information channel; text meets WCAG AA contrast; table headers are marked; no idioms; acronyms defined on first use; sentences under 25 words.

### Persona Templates for Documentation Planning

Use these templates to create documentation personas for your project.

**Persona template:**

```text
## Persona: [Name]

### Demographics
Role:         [e.g., Backend Developer, DevOps Engineer]
Experience:   [Junior / Mid / Senior]
Product use:  [Daily / Weekly / Once]
Environment:  [Enterprise / Startup / Side project]

### Goals
- [Primary goal when using the docs]
- [Secondary goal]

### Pain points
- [What frustrates this persona about current docs]
- [What makes them abandon the docs]

### Documentation preferences
Preferred format:  [Tutorial / Reference / How-to / Video]
Preferred depth:   [Minimal / Moderate / Deep]
Reading style:     [Scan / Read fully / Search]

### Typical session
Time available: [5 min / 30 min]
Context:        [Coding / Evaluating]
Starting point: [Search / Navigation / External link]

### Content they need
Required:
- [Must-have document type or information]
Nice to have:
- [Helpful but not critical]
Not needed:
- [What would waste their time]

### How we serve this persona
Current state: [docs for this persona?]
Gap:           [what is missing?]
Priority:      [High / Medium / Low]
```

**Creating your persona set:**

```text
Steps to create documentation personas:

1. Gather data
   - Support tickets (who files them and about what?)
   - Analytics (who visits which pages?)
   - User interviews (who is your actual audience?)
   - Sales/Customer success (who buys and who churns?)

2. Identify patterns
   - Group users by role, goal, and behavior
   - Look for 4-6 distinct clusters
   - Name each cluster

3. Write persona cards
   - One page per persona
   - Include goals, pain points, preferences
   - Use real quotes from interviews or support tickets

4. Validate personas
   - Share with support, sales, and product teams
   - Refine based on feedback

5. Use personas in planning
   - For every new doc: "Which persona is this for?"
   - For prioritization: "Which persona is underserved?"

6. Refresh personas annually
```

**Persona-based content audit template:**

```text
| Doc                     | Newcomer | Regular | Power | Integrator | Evaluator | Maintainer |
|-------------------------|----------|---------|-------|------------|-----------|------------|
| README                  | Primary  | -       | -     | -          | Primary   | -          |
| Getting started guide   | Primary  | -       | -     | -          | Secondary | -          |
| API reference           | -        | Secondary| Primary | Primary | -        | -          |
| How-to: Authentication  | Secondary| Primary | -     | Primary   | -         | -          |
| Configuration reference | -        | Secondary| Primary | Primary | -        | -          |
| Migration guide         | -        | Primary | Primary| Secondary | -        | -          |
| Contribution guide      | -        | -       | -     | -          | -         | Primary   |

Gap analysis: Personas with no "Primary" coverage need prioritization.
```

## Study Cases

### Case 1: Persona-Driven Rewrite of a CLI Tool's Docs

A CLI tool for database migrations had one alphabetical reference page and no getting-started guide.

```text
Persona analysis: 60% junior (needed tutorials), 30% mid (how-to),
10% senior (command reference).

Changes:
1. Added a getting-started guide for newcomers
2. Reorganized commands by task, not alphabetically
3. Added beginner callouts to complex commands
4. Separate reference section for power users

Results:
- Support tickets about "how do I do X?" dropped by 65%
- First-time completion rate improved from 30% to 82%
```

### Case 2: Analytics Reveal an Underserved Audience

An API platform noticed 40% of search queries were variations of "rate limit". No dedicated page existed.

```text
Signal: 1,200 searches/month for "rate limit", 70% clicked nothing.
Rate limit info was buried in the authentication page under "Throttling Policy".

Fix:
1. Created a dedicated rate-limiting page with limits, handling, best practices
2. Added "rate limiting" synonym to search configuration
3. Added link from authentication page to the new page

Results:
- Rate-limit support tickets: 45/month → 8/month
- Searchers finding what they need: 30% → 85%
```

### Case 3: Empathy Gap in Integration Docs

An API integration guide assumed readers knew the request body format, how to handle the response, and how to use the Authorization header.

```text
Rewrite: Added explicit curl examples with request and response,
step-by-step authentication flow, and verification points.

Results:
- Integration success rate: 35% → 92%
- Average integration time: 4 hours → 45 minutes
```

## Examples

### Example 1: Progressive Disclosure in a Reference Page

```text
# API Key Authentication

## Quick Setup
Add your API key to the Authorization header:
```
Authorization: Bearer sk_live_abc123
```

---

<details>
<summary>Detailed explanation (for new users)</summary>
API keys identify your application. Each request must include the key
in the Authorization header using the Bearer scheme. If the key is
invalid or expired, the server returns 401 Unauthorized.
</details>

<details>
<summary>Advanced configuration (for power users)</summary>
Create up to 10 API keys for different environments:
- Production: sk_live_xxx (read-write)
- Staging: sk_staging_xxx (read-write on staging only)
- Read-only: sk_ro_xxx (GET requests only)
</details>
```

### Example 2: Journey-Based Navigation

```text
# Documentation Home

## New to our platform?
[Getting Started Guide](getting-started) — 10 minutes

## Implementing a feature?
[How-to Guides](how-to) — task-oriented solutions
[API Reference](api) — endpoint specifications

## Debugging an issue?
[Error Codes](errors) — find and resolve errors
[Troubleshooting](troubleshooting) — common problems

## Upgrading?
[Migration Guide](migration) — breaking changes and upgrade steps
[Changelog](changelog) — what changed

## Contributing?
[Contribution Guide](contributing) — how to submit changes
```

### Example 3: Persona Card

```text
## Persona: Maria, API Integrator

Role:         Senior Backend Developer
Experience:   8 years, enterprise, strict security requirements
Product use:  Weekly — integrates APIs for data pipeline

Goals:
- Build reliable integration with minimal maintenance
- Understand all error states before production

Pain points:
- Missing error codes in documentation
- No machine-readable spec for code generation

Prefers:
- OpenAPI spec over HTML reference
- Code examples in Python and Go
- Exact limits and rate thresholds
```

## Glossary

| Term | Definition |
|------|------------|
| Persona | A fictional character representing a segment of documentation readers |
| Empathy gap | The difference between what the writer assumes and what the reader knows |
| Progressive disclosure | Revealing information in layers, from simple to detailed |
| Developer journey | A pattern of developer activity with specific documentation needs |
| Curse of knowledge | The inability to imagine what it is like not to know something |
| Cognitive load | The mental effort required to understand and use documentation |
| Fresh-eyes review | Having someone unfamiliar review documentation for clarity |
| Card sorting | A research method where users organize topics to validate IA |
| Tree testing | A research method where users navigate a content tree without content |
| WCAG | Web Content Accessibility Guidelines — standards for web accessibility |
| Chunking | Breaking information into small, manageable units |
| Layered content | Documentation structured in increasing levels of detail |
| Audience callout | A formatted note targeting a specific reader group |
| Evaluator | A reader comparing a product against alternatives before adoption |
| System integrator | A reader connecting a product into a larger technical ecosystem |
| Maintainer | A reader responsible for sustaining or contributing to a project |
| Documentation gap | Missing content that a persona or journey requires |

## Quick References

- [Google's UX Persona Guide](https://www.youtube.com/watch?v=u_YXqBfgwF8) — creating and using personas in product design
- [Nielsen Norman Group: Personas](https://www.nngroup.com/articles/persona/) — research-based persona methodology
- [WCAG 2.1 Guidelines](https://www.w3.org/TR/WCAG21/) — Web Content Accessibility Guidelines
- [Write the Docs: Audience](https://www.writethedocs.org/guide/writing/audience/) — community guidance on documentation audiences
- [Plain Language for Developers](https://www.plainlanguage.gov/) — US government plain language guidelines

## Next Steps

- [Tutorials & Getting Started Guides](tutorials.md) — applying audience understanding to tutorial design
- [API Reference Documentation](api-docs.md) — writing reference docs that serve power users and integrators
