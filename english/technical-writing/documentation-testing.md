# Testing & Reviewing Documentation

## Description

Documentation needs testing just like code. Broken links, outdated examples, inconsistent terminology, and unclear instructions erode trust and waste users' time. This document covers how to test documentation for accuracy, usability, and consistency — using automated tools, review workflows, and user research.

## Prerequisites

- [Docs as Code & CI/CD for Documentation](docs-as-code.md) — automated documentation testing depends on docs-as-code infrastructure
- [Style Guides & Consistency](style-guides.md) — style guide enforcement tools are a primary method of automated doc testing

## Table of Contents

- [Why Documentation Needs Testing](#why-documentation-needs-testing)
- [Types of Documentation Testing](#types-of-documentation-testing)
- [Automated Checks](#automated-checks)
- [Link Checking](#link-checking)
- [Spell Checking](#spell-checking)
- [Style Guide Linting with Vale](#style-guide-linting-with-vale)
- [Code Example Testing](#code-example-testing)
- [Documentation Review Workflows](#documentation-review-workflows)
- [Testing with Actual Users](#testing-with-actual-users)
- [Measuring Documentation Effectiveness](#measuring-documentation-effectiveness)
- [Documentation Debt](#documentation-debt)
- [Continuous Improvement Processes](#continuous-improvement-processes)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Why Documentation Needs Testing

Documentation rots. Code changes, APIs evolve, UI elements move, and new features ship. Without testing, documentation gradually becomes inaccurate, incomplete, or misleading.

**Consequences of untested documentation:**

```text
- Outdated screenshots and code examples that do not match the current UI
- Broken links that lead to 404 pages
- Inconsistent terminology that confuses readers
- Instructions that no longer work with the latest version
- Missing edge cases that users encounter in practice
```

**Cost of fixing doc bugs:** Found during review = minutes. Reported by user = hours. Causing support spike = days.

### Types of Documentation Testing

**Accuracy testing.** Does the documentation match the current behavior of the product?

```text
Checks: Code examples produce expected output, API paths are correct,
screenshots match current UI, configuration examples use valid syntax.
```

**Usability testing.** Can users find, understand, and act on the information?

```text
Checks: Can users complete a task using only the documentation? Do they
understand the terminology? Where do they get stuck?
```

**Accessibility testing.** Can all users access the documentation content?

```text
Checks: Images have alt text, color contrast meets WCAG AA, links have
descriptive text, heading hierarchy is logical, code blocks have language IDs.
```

**Consistency testing.** Does the documentation follow the same style and conventions?

```text
Checks: Terminology is consistent, tone matches style guide, headings use
same capitalization, code formatting follows the same rules.
```

**Completeness testing.** Are there gaps in documentation coverage?

```text
Checks: Every public API is documented, every config option is explained,
every error code has a description, every concept has an example.
```

### Automated Checks

Automated checks catch the most common documentation defects before they reach users.

| Check | Tools | What It Catches |
|-------|-------|-----------------|
| Link checking | lychee, html-proofer | Broken internal and external links |
| Spell checking | cspell, codespell | Typos, misspelled technical terms |
| Style linting | Vale, proselint, write-good | Style violations, weasel words |
| Markdown linting | markdownlint, remark-lint | Formatting issues, broken syntax |
| Code validation | shellcheck, eslint | Syntax errors in code examples |
| Accessibility | axe, pa11y | Accessibility violations |
| Readability | Hemingway, readability-cli | Complex sentences, high reading level |

**Setting up automated checks in CI:**

```yaml
name: Documentation Checks
on: [pull_request]
jobs:
  lint:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Spell check
        run: npx cspell "docs/**/*.md"
      - name: Markdown lint
        run: npx markdownlint "docs/**/*.md"
      - name: Link check
        uses: lycheeverse/lychee-action@v1
        with:
          args: docs/**/*.md
      - name: Style check
        uses: errata-ai/vale-action@v2
```

**Running checks locally:**

```text
cspell docs/getting-started.md         # Spell check
markdownlint docs/getting-started.md   # Markdown lint
lychee docs/getting-started.md         # Link check
vale docs/getting-started.md           # Style check
shellcheck docs/examples/*.sh          # Shell example validation
```

### Link Checking

Broken links erode credibility. Link checkers verify every href resolves.

**Common link issues:**

```text
Broken internal links:   Files moved or renamed
Broken external links:   External sites changed or removed
Anchor link typos:       Heading ID mismatch
Redirect chains:         Multiple redirects before resolving
HTTP vs. HTTPS:          Link to HTTP when HTTPS is available
```

**Link checker configuration:**

```toml
# lychee.toml
exclude = ['https://localhost/*', 'https://example.com/*']
timeout = 30
max_redirects = 5
```

**Anchor link validation:**

```text
Headings generate anchors automatically:
  "## Getting Started" → #getting-started
  "## API Reference"   → #api-reference

Check that every #reference in a link exists in the target document.
```

**Link maintenance schedule:** PR hook for all links, monthly full audit for external links, quarterly redirect chain review.

### Spell Checking

Spell checkers catch typos that human reviewers miss.

**Configuring a spell checker:**

```json
{
  "words": ["monorepo", "configurable", "changelog", "backend"],
  "ignorePaths": ["node_modules", "dist"],
  "language": "en-US"
}
```

**Technical dictionary management:**

```text
Dictionary entries come from:
- Project-specific terms: product name, feature names
- Domain terminology: Docker, Kubernetes, TypeScript
- Acronyms: API, SDK, CLI, GUI, REST, JSON
- Common misspellings: occurrence, dependent, separate

Workflow: Add new terms on first occurrence, review dictionary quarterly.
```

**Handling false positives:**

```text
- Code snippets contain identifiers and keywords → ignore code blocks
- Command output includes system names → use token ignore patterns
- Configuration files list non-standard terms → add to dictionary
```

### Style Guide Linting with Vale

Vale is an open-source prose linter that checks writing against configurable style guides.

**Vale configuration:**

```ini
# .vale.ini
StylesPath = .vale/styles
MinAlertLevel = warning

[*.md]
BasedOnStyles = Microsoft, DevBook

[*.md]
TokenIgnores = [`[^`]+`]
```

**Creating custom style rules:**

```yaml
# .vale/styles/DevBook/Wordiness.yml
extends: substitution
message: "Consider using '%s' instead of '%s'."
level: warning
swap:
  in order to: to
  utilize: use
  leverage: use
  due to the fact that: because
```

**Common Vale rules to implement:**

```text
- Hedging: flag "basically", "generally", "usually"
- Passive voice: flag passive constructions
- Word complexity: flag words above reading grade level
- Terms: flag prohibited terms from style guide
- Capitalization: enforce sentence case for headings
- Acronyms: flag undefined acronyms on first use
```

### Code Example Testing

Code examples must be tested. Untested code is the most common source of doc bugs.

**Embedded code testing approaches:**

```text
Approach 1: Extract and run.
  Extract code blocks from docs, run them in CI, verify output.
  Tools: doctest (Python), mddoctest

Approach 2: Include from tested files.
  Write examples as separate files tested by the test suite.
  Include in documentation using reference syntax.

Approach 3: Inline with test assertions.
  Write examples with embedded assertions executed during build.
  Tools: Rust doc-tests, Python doctest.
```

**Example: Python doctest:**

```python
def add(a, b):
    """
    Returns the sum of two numbers.

    >>> add(2, 3)
    5
    >>> add(-1, 1)
    0
    """
    return a + b
```

**Code example review checklist:**

```text
- [ ] Does the example run without errors?
- [ ] Does the example produce the described output?
- [ ] Does it use the latest API/CLI syntax?
- [ ] Are placeholders clearly marked (YOUR_API_KEY)?
- [ ] Can the example be copied and pasted?
- [ ] Is it complete enough to be useful?
- [ ] Does it focus on one concept?
```

### Documentation Review Workflows

Documentation reviews follow the same patterns as code reviews with different focus areas.

**Review types:**

```text
Self-review:    Author reviews before submitting
Peer review:    Another writer reviews for clarity and style
Technical review: Developer reviews for technical accuracy
Editorial review: Editor reviews for grammar and consistency
User review:    Target user reviews for usability
```

**Documentation review checklist:**

```text
Accuracy:
- [ ] Facts are correct
- [ ] Code examples produce expected output
- [ ] Screenshots match current UI
- [ ] Version numbers are current

Clarity:
- [ ] Purpose is clear from the title
- [ ] Prerequisites are listed and linked
- [ ] Instructions are in logical order
- [ ] Each step has a single action

Style:
- [ ] Voice and tone match the style guide
- [ ] Terminology matches the project glossary
- [ ] Headings use consistent capitalization
- [ ] Links use descriptive text

Completeness:
- [ ] No missing steps in procedures
- [ ] Edge cases are mentioned
- [ ] Error conditions are documented
- [ ] Next steps link to related documents
```

**Review velocity:** Typo = automated only. Minor (1 paragraph) = 1 reviewer, 1 hour. Standard (new doc) = 2 reviewers, 24 hours. Major (restructure) = team review, 1 week.

### Testing with Actual Users

Automated checks catch syntax issues. User testing catches comprehension problems.

**Task-based testing:**

```text
Give users a task and documentation. Observe whether they succeed.

Example:
  Task: "Create a new project using the CLI and deploy it."
  Success: User completes within 10 minutes using only the docs.
  Measure: Completion rate, time on task, errors made.
```

**First-click testing:**

```text
Show users a documentation page. Ask: "Where would you click to find X?"
Goal: Validate information architecture and navigation.
```

**Five-second testing:**

```text
Show a page for 5 seconds, remove it. Ask: "What was the page about?"
Goal: Validate that the purpose is immediately clear.
```

**Feedback forms:**

```text
Inline: "Was this page helpful?" [Yes] [No]
  Follow-up: "What was missing?" (free text)

Quarterly survey: Documentation satisfaction score
```

**Recruitment for testing:** In-app banners, pre-recruited panels, support ticket invitees, community contributors.

### Measuring Documentation Effectiveness

Quantitative metrics help teams track improvement.

**Key metrics:**

```text
Usage:
- Page views and unique visitors
- Search queries and click-through rates
- Time on page and scroll depth

Task:
- Task completion rate (from user tests or analytics)
- Time to first success (search → find → complete)
- Support ticket reduction after doc update

Quality:
- Documentation debt score
- Style guide compliance percentage
- Broken link count
- Outdated page count
- User satisfaction score
```

**Documentation NPS:**

```text
"On a scale of 0-10, how likely are you to recommend our docs
to a colleague?"

Promoters (9-10): Excellent
Passives (7-8):  Adequate
Detractors (0-6): Needs improvement

Score = % Promoters - % Detractors
Target: NPS greater than 30.
```

**Metrics dashboard example:**

```text
Documentation Health — Q2 2026

Usage:
  Page views:     245,000 (+12% vs Q1)
  Search rate:    68% of sessions
  Top search:     "authentication setup" (1,200 queries)

Quality:
  Style compliance: 94% (target: 90%)
  Broken links:     3 (target: 0)
  User satisfaction: 4.2/5.0

Support:
  Tickets mentioning docs: 42 (target: under 50)
```

### Documentation Debt

Documentation debt is the accumulated gap between what is documented and what should be documented.

**Sources of documentation debt:**

```text
- Features shipped without documentation
- Deprecated features still documented
- Docs not updated after UI or API changes
- Known edge cases not covered
- Outdated screenshots and code examples
- Missing error message explanations
- Orphaned documents (no incoming links)
```

**Tracking documentation debt:**

```text
Inventory each document with:
  - Last updated date
  - Last reviewed date
  - Owner
  - Page views
  - User satisfaction score
  - Known issues count

Flag documents if:
  - Last updated > 6 months
  - User satisfaction < 3.0
  - Known issues > 5
  - Orphaned (no internal links)
```

**Documentation debt sprint:**

```text
1. Inventory: List all documents with review dates
2. Triage: Score each document (Impact × Decay)
3. Fix: Assign top-priority items
4. Verify: Check fixes in review
5. Prevent: Add automated checks

Debt scoring:
  Impact (1-5): How many users does this document affect?
  Decay (1-5):  How out of date is it?
  Priority = Impact × Decay
  20-25: Critical — fix this sprint
  10-19: High — fix next sprint
  5-9:   Medium — schedule within 2 sprints
  1-4:   Low — backlog
```

**Preventing documentation debt:**

```text
- Doc is a release gate: no feature ships without docs
- Automated freshness checks every 6 months
- Documentation in definition of done
- Assign ownership per team
- Quarterly debt reviews
```

### Continuous Improvement Processes

Documentation quality requires ongoing processes, not one-time efforts.

**Review cadence:**

```text
Per commit:     Automated checks (spelling, links, style)
Per PR:         Human review (peer or editorial)
Per release:    Full doc audit for changed features
Per quarter:    User testing and satisfaction survey
Per year:       Full inventory and strategy review
```

**Improvement cycle (Plan-Do-Check-Act):**

```text
Plan:   Identify gaps from user feedback and metrics
Do:     Write or update docs to fill gaps
Check:  Verify improvements through testing
Act:    Integrate changes into regular workflow
```

**Documentation postmortems:**

```text
When a documentation failure causes user impact:
1. What happened?
2. Why did it happen?
3. What should we do differently?
4. How will we prevent recurrence?
```

**Documentation champions program:**

```text
Assign champions in each engineering team:
- Responsibilities: review docs, identify gaps, update after releases
- Authority: approve changes, request improvements
- Training: best practices, style guide, tools
```

**Team documentation health review (quarterly, 30 min):**

```text
1. Review metrics dashboard (5 min)
2. Review user feedback highlights (5 min)
3. Review debt backlog (5 min)
4. Assign improvement items (5 min)
5. Celebrate wins (5 min)
6. Set next quarter goals (5 min)
```

## Documentation Review

### Technical Accuracy
- [ ] Code examples are correct
- [ ] API endpoints match implementation
- [ ] Screenshots reflect current UI

### Clarity
- [ ] Prerequisites are clear
- [ ] Steps are in logical order
- [ ] Concepts explained before use

### Style
- [ ] Follows style guide
- [ ] Terminology consistent
- [ ] Links have descriptive text

### Completeness
- [ ] All sections present
- [ ] Edge cases covered
- [ ] Next steps linked
```

## Glossary

| Term | Definition |
|------|------------|
| Documentation debt | The accumulated gap between what is documented and what should be documented |
| Accuracy testing | Verifying docs match the current product behavior |
| Usability testing | Testing whether users can find, understand, and act on docs |
| Accessibility testing | Verifying docs are usable by people with disabilities |
| Consistency testing | Verifying docs follow the same style and conventions |
| Vale | An open-source prose linter for enforcing style guide rules |
| lychee | A fast link checker for verifying URLs in documentation |
| cspell | A spell checker with support for custom dictionaries |
| markdownlint | A linter for Markdown formatting and syntax |
| Documentation NPS | A metric measuring user satisfaction with documentation |
| Freshness check | Automated check for documents not updated recently |
| Doctest | Executable code examples embedded in documentation |
| WCAG | Web Content Accessibility Guidelines |
| Task completion rate | Percentage of users who can successfully complete a task |
| Documentation champion | A team member responsible for documentation quality |
| Definition of done | A checklist before a feature is considered finished |
| Documentation postmortem | A retrospective analysis of a documentation failure |
| Orphaned document | A document with no internal links pointing to it |
| Decay score | A measure of how outdated a document is |
| Impact score | A measure of how many users a document affects |

## Quick References

- [Vale](https://vale.sh/) — open-source prose linter with style guide support
- [lychee](https://lychee.cli.rs/) — fast link checker for documentation
- [cspell](https://cspell.org/) — spell checker for code and documentation
- [markdownlint](https://github.com/DavidAnson/markdownlint) — linter for Markdown files
- [readability-cli](https://github.com/nicktomlin/readability-cli) — command-line readability checker
- [WCAG 2.1 Guidelines](https://www.w3.org/TR/WCAG21/) — Web Content Accessibility Guidelines
- [Pa11y](https://pa11y.org/) — automated accessibility testing tool
- [Google Developer Docs: Reviewing](https://developers.google.com/style/review) — Google's documentation review guidance
- [Microsoft Style Guide: Errors](https://learn.microsoft.com/en-us/style-guide/) — Microsoft's style guide
- [Documentation Debt: A Practical Guide](https://www.writethedocs.org/guide/doc-debt/) — Write the Docs guide to documentation debt

## Next Steps

- [Diataxis Framework in Practice](diataxis.md) — using Diataxis to identify documentation coverage gaps
- [Documentation Architecture & Information Design](doc-architecture.md) — designing information architecture that is easier to test and maintain
