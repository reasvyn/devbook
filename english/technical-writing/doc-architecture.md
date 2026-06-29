# Documentation Architecture & Information Design

## Description

Good documentation starts with good architecture. Information design principles help you organize content so readers can find what they need quickly. This document covers structuring documentation projects, designing navigation, modeling content, and managing versioned documentation at scale.

## Prerequisites

- [Style Guides](../grammar-and-style/style-guides.md) — architectural decisions affect how style guides are applied across projects

## Table of Contents

- [Information Architecture Principles for Docs](#information-architecture-principles-for-docs)
- [Organizing Documentation: The Diataxis Model](#organizing-documentation-the-diataxis-model)
- [Navigation Design](#navigation-design)
- [Search and Discoverability](#search-and-discoverability)
- [Content Modeling](#content-modeling)
- [Cross-Referencing and Linking Strategy](#cross-referencing-and-linking-strategy)
- [Versioned Documentation](#versioned-documentation)
- [Site Maps for Documentation](#site-maps-for-documentation)
- [Structuring Large Documentation Projects](#structuring-large-documentation-projects)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Information Architecture Principles for Docs

Information architecture (IA) is the practice of organizing content so that users can navigate it intuitively. For documentation, IA determines whether a reader finds the answer in seconds or gives up in frustration.

**Core IA principles applied to documentation:**

```text
Organizational principle: Group related content together. Tutorials belong with
tutorials, reference with reference. Mixing types confuses readers.

Labeling principle: Use clear, consistent labels for navigation and headings.
"Getting Started" is better than "Quickstart Guide for Beginners".

Search principle: Design for search first, browse second. Many readers go
straight to the search box. Your IA must support both paths.

Growth principle: Design the IA to accommodate new content without requiring
restructuring. A flat hierarchy (2-3 levels) is easier to extend than a deep one.

Context principle: Show readers where they are within the documentation.
Breadcrumbs, section highlights, and "you are here" indicators reduce disorientation.
```

**The four questions readers ask:**

```text
1. What is this?        — Overview, introduction, landing page
2. How do I start?      — Getting started guide, installation, quickstart
3. How do I do X?       — How-to guides, tutorials, recipes
4. What does this do?   — Reference, API docs, configuration reference
```

Every piece of content should answer one of these questions. Content that answers none of them should not exist.

**Common IA patterns for documentation:**

```text
Flat structure (small projects):
/ — Home
  /getting-started
  /guides
  /reference
  /faq

Hierarchical structure (medium projects):
/ — Home
  /getting-started
    /installation
    /quickstart
  /guides
    /authentication
    /deployment
    /scaling
  /reference
    /api
    /configuration
    /cli

Matrix structure (large projects with multiple dimensions):
  Organize by: audience (admin, developer, end-user)
  Then by: task (setup, configure, troubleshoot)
  Then by: content type (tutorial, guide, reference)
```

**Avoid these IA anti-patterns:**

```text
The dumping ground: A single "miscellaneous" or "other" section where
unrelated content accumulates. Everything belongs somewhere specific.

The copycat: Copying another project's IA without considering whether
it matches your content. What works for Stripe may not work for your tool.

The org chart: Mirroring your company's internal team structure instead
of the user's mental model. Users do not care which team owns what.

The deep tunnel: Navigation with 5+ levels of hierarchy. Readers lose
orientation. Keep it to 3 levels or fewer.
```

### Organizing Documentation: The Diataxis Model

Diataxis is a documentation framework by Daniele Procida that organizes content into four modes: tutorials, how-to guides, explanation, and reference.

**The four quadrants:**

```text
                   Study                    |                 Work
                   Goal: Learning           |            Goal: Completion
--------------------------------------------|-----------------------------------
Tutorials:                                  | How-to guides:
  Learning-oriented                         |  Task-oriented
  Step-by-step lessons                      |  Practical solutions to problems
  "Teach a beginner to do something"        |  "Show an experienced user how to X"
--------------------------------------------|-----------------------------------
Explanation:                                | Reference:
  Understanding-oriented                    |  Information-oriented
  Background, concepts, discussion          |  Facts, parameters, commands
  "Why does this work?"                     |  "What is the syntax?"
```

**Applying Diataxis to documentation IA:**

```text
Tutorials section:
- Getting started lessons
- End-to-end walkthroughs
- Learning paths

How-to guides section:
- Authentication setup
- Deployment procedures
- Troubleshooting recipes

Explanation section:
- Architecture overview
- Design decisions
- Key concepts

Reference section:
- API documentation
- Configuration file reference
- CLI command reference
- Changelog
```

A well-structured documentation project uses all four modes. Missing modes mean gaps in what readers can accomplish.

### Navigation Design

Navigation is the bridge between your IA and the reader. Good navigation makes your IA invisible — readers find what they need without thinking about the structure.

**Navigation components:**

```text
Primary navigation (main sidebar):
- Top-level sections that reflect the IA
- Collapsible subsections for deep content
- "Getting Started" should always be first
- "Reference" should be last

Secondary navigation (within-page):
- Table of contents (anchors to headings)
- Previous/next page links
- Breadcrumb trail showing current position

Utility navigation (global):
- Search bar (prominently placed)
- Version selector
- Language selector
- Theme toggle (light/dark)
```

**Sidebar design principles:**

```text
// Good sidebar structure:
Getting Started          ← always first
  Installation
  Quickstart
Guides                   ← task-oriented
  Authentication
  Deployment
  Monitoring
Concepts                 ← understanding-oriented
  Architecture
  Security Model
Reference                ← always last
  API Reference
  CLI Reference
  Configuration

// Bad sidebar — mixed modes:
Getting Started
  Installation
  API Reference          ← reference does not belong here
  Architecture           ← concept does not belong here
  Quickstart
FAQ                      ← better integrated into relevant sections
```

**Breadcrumb patterns:**

```text
Standard: Home > Guides > Authentication > API Keys
Hide home: Guides > Authentication > API Keys
Short: Authentication > API Keys (when parent is obvious)
```

**Mobile navigation considerations:**

```text
- Hide sidebar by default; show with hamburger menu
- Keep top-level section names short (1-2 words)
- Provide search as the primary mobile navigation
- Test on actual mobile devices, not just responsive previews
```

### Search and Discoverability

Search is often the primary way readers navigate documentation. Invest in making it work well.

**Search requirements for documentation:**

```text
Full-text search: Index all page content, not just titles and headings

Faceted search: Allow filtering by content type (tutorial, reference, guide),
version, or product area

Fuzzy matching: Handle typos and partial matches ("authetnicate" should
find "authentication")

Synonym expansion: Map common synonyms ("deploy" = "release" = "publish")

Section-level results: Return the relevant section, not just the page title

Keyboard navigation: Arrow keys to move through results, Enter to select
```

**Improving search rankings within your docs:**

```text
1. Write descriptive page titles that match what users search for
   "How to authenticate API requests" (good)
   "Authentication" (too short, ambiguous)
   "API Auth Overview" (uses internal jargon)

2. Include synonyms in the first paragraph
   "This guide covers authentication, login, and signing in to the API."

3. Use H2 headings for major topics within a page
   Search engines and internal search index heading text

4. Avoid publishing multiple pages with very similar content
   They compete for the same search queries and dilute results
```

**Search result presentation:**

```text
Each result should show:
- Page title (linked)
- Breadcrumb path (context)
- Snippet of matching content (with search terms highlighted)
- Content type badge (Tutorial, Guide, Reference)
- Last updated date
```

**When search is not enough:**

```text
- Provide a "popular topics" section on the homepage
- Include "related articles" at the bottom of every page
- Offer an AI chat interface for complex questions
- Maintain a "can't find what you're looking for?" feedback link
```

### Content Modeling

Content modeling is the practice of defining the structure and relationships of your documentation content.

**Content types and their attributes:**

```text
Tutorial:
  title, description, difficulty (beginner/intermediate/advanced),
  estimated time, prerequisites list, steps (ordered list),
  verification steps, next steps

How-to guide:
  title, description, prerequisites list, task steps, expected outcome,
  troubleshooting notes, related guides

Explanation:
  title, description, body content, diagrams, related concepts

Reference:
  title, description, items (tables or definitions), examples,
  version information, deprecation notices
```

**Content relationships:**

```text
Parent-child: A guide belongs to a section. The section belongs to a module.

Sequential: Tutorials are ordered. Tutorial 2 depends on Tutorial 1.

Related: Cross-references between content types. A how-to guide links
to the relevant reference page and explanation.

Versioned: A page exists in multiple versions. The v2 API reference
is a versioned copy of the v1 reference with changes applied.
```

**Metadata for content management:**

```text
Front-matter example (YAML):
---
title: "Authenticating API Requests"
description: "How to obtain and use API tokens"
contentType: how-to-guide
difficulty: intermediate
estimatedTime: 15 minutes
prerequisites:
  - "Installation guide"
products: ["api", "sdk"]
versions: ["v2", "v3"]
authors: ["jane.doe"]
lastReviewed: "2025-06-01"
---
```

### Cross-Referencing and Linking Strategy

Links connect documentation into a web of knowledge. A linking strategy ensures readers can navigate between related content without getting lost.

**Types of links in documentation:**

```text
Hierarchical links:
- Parent section links (breadcrumbs, "back to X")
- Child section links (table of contents, section landing pages)
- Sibling links (previous/next page)

Associative links:
- Prerequisites (what to read first)
- Next steps (what to read after)
- Related topics (alternative approaches)
- See also (deeper dives)

Contextual links:
- Inline hyperlinks within paragraphs
- Tooltip references for glossary terms
- Footnote citations
```

**Link placement patterns:**

```text
Prerequisites: At the top of the page, before the main content.
"Before starting this guide, make sure you have read:
- [Installation guide](installation)
- [API overview](api-overview)"

Related topics: At the bottom or in a sidebar.
"Related:
- [API rate limiting](rate-limiting)
- [Authentication best practices](auth-best-practices)"

Inline references: Within the text where relevant.
"The server returns a 429 status code when rate-limited.
See [rate limiting](rate-limiting) for details."
```

**Link quality guidelines:**

```text
Do:
- Use descriptive link text ("see authentication setup" not "click here")
- Link to the most specific relevant page, not a section index
- Verify links work before publishing
- Use relative paths for same-site links

Don't:
- Use "click here" or "read more" as link text
- Link to external sites without warning the reader
- Create dead ends — every page should have onward navigation
- Over-link — too many inline links distract from the content
```

### Versioned Documentation

When your product evolves, your documentation must evolve with it. Versioned documentation maintains copies of docs for each supported product version.

**Versioning strategies:**

```text
Single version (latest only):
- Simple to maintain
- Readers see only current content
- No confusion about which version to use
- Breaks when breaking changes occur

Multiple versions (all supported):
- Each major version has its own docs
- Readers can switch between versions
- Maintenance overhead increases with each version
- Requires clear version labeling

Selective versioning (latest + LTS):
- Maintain docs for latest and long-term support versions
- Older versions archive with a notice
- Balances maintenance cost with reader needs
```

**Version selector implementation:**

```text
Version selector in the navigation bar:

[v2.0 (latest) v]
  v2.0 (latest)
  v1.5 (LTS)
  v1.0 (legacy — end of life)

When a reader selects a version, all pages in that version are shown.
The URL structure accommodates versioning:

/docs/v2.0/guides/authentication
/docs/v1.5/guides/authentication
/docs/v1.0/guides/authentication
```

**Versioning rules:**

```text
1. Each version is a complete copy of the documentation tree
2. Content changes are made to all active versions individually
3. Deprecation notices are added to old versions before they are archived
4. A banner warns readers when they are viewing an outdated version
5. Search results indicate which version each result belongs to
```

### Site Maps for Documentation

A site map is a high-level diagram of your documentation structure. It serves as a planning tool and a navigation aid.

**Site map format:**

```text
Project Name Docs
├── Getting Started              ← tutorial mode
│   ├── Installation
│   ├── Quickstart (5 min)
│   └── Your First Project
├── Guides                       ← how-to mode
│   ├── Authentication
│   │   ├── API Keys
│   │   └── OAuth 2.0
│   ├── Deployment
│   │   ├── Web Servers
│   │   └── Docker
│   └── Monitoring
├── Concepts                     ← explanation mode
│   ├── Architecture Overview
│   ├── Security Model
│   └── Data Model
├── Reference                    ← reference mode
│   ├── API Reference
│   │   ├── Users
│   │   ├── Orders
│   │   └── Products
│   ├── CLI Reference
│   └── Configuration
└── Resources
    ├── Changelog
    ├── FAQ
    └── Support
```

**Creating a site map:**

```text
1. List all content you need
2. Group content into the four Diataxis modes
3. Order groups by reader journey (start first, reference last)
4. Define hierarchy (maximum 3 levels)
5. Label each group with clear, reader-oriented names
6. Review the map with real users
7. Iterate based on feedback and analytics
```

**Tools for creating site maps:**

```text
- Spreadsheets: Good for planning and tracking
- Miro / FigJam: Collaborative whiteboarding for IA sessions
- Sphinx / MkDocs: Navigation config files (YAML/TOML)
- Tree diagrams: Simple text-based trees for version control
- Card sorting tools: Validate IA with real users
```

### Structuring Large Documentation Projects

Large documentation projects present unique challenges. Here are strategies for managing them at scale.

**Modular documentation architecture:**

```text
Divide content into independent modules that can be maintained separately:

/core          — Core concepts shared across all products
/product-a     — Product-specific documentation
/product-b     — Another product's documentation
/shared        — Reusable content snippets, templates
/contributing  — How to contribute to the docs
```

**Content reuse strategy:**

```text
Use includes or snippets for content that appears in multiple places:

Example — shared authentication snippet:
AUTH_TOKEN_EXAMPLE.md:
  "Include the API key in the Authorization header:
   Authorization: Bearer <your-api-key>"

Include it in:
- /product-a/guides/auth.md
- /product-b/guides/auth.md
- /getting-started/quickstart.md
```

**Documentation ownership:**

```text
For large projects, assign ownership:

| Section          | Owner          | Review cycle |
|------------------|----------------|--------------|
| Getting Started  | Developer Relations | Monthly |
| API Reference    | API Team       | Per release  |
| CLI Reference    | CLI Team       | Per release  |
| Concepts         | DevRel + Tech Writers | Quarterly |
| Tutorials        | Developer Relations | Bi-weekly |
```

**Governance for large documentation:**

```text
- Documentation Steering Committee (meets monthly)
  - Reviews IA changes
  - Approves new sections
  - Resolves ownership disputes

- Style and standards board
  - Maintains the style guide
  - Reviews tooling decisions
  - Approves templates

- Content calendar
  - Tracks planned content
  - Maps to product release schedule
  - Assigns writers and reviewers

- Analytics review (monthly)
  - Identifies underperforming content
  - Finds search queries with no results
  - Tracks reader drop-off points
```

## Study Cases

### Case 1: Restructuring a Disorganized Documentation Site

A SaaS platform had 200+ pages organized by which team created them, not by what readers needed. Readers complained about not finding basic information.

```text
Before (team-oriented IA):
/engineering/api
/engineering/deployment
/support/faq
/product/changelog
/marketing/overview

After (task-oriented IA using Diataxis):
/getting-started/quickstart
/getting-started/installation
/guides/authentication
/guides/deployment
/reference/api
/reference/cli
/concepts/architecture
/resources/changelog

Results:
- Support tickets about "can't find X" dropped by 45%
- Average time to first API call decreased from 30 min to 8 min
- Page views per session increased by 60%
```

### Case 2: Content Modeling for a CLI Tool

```text
Problem: A CLI tool's documentation was a single 50-page reference document.
Readers had to scroll through irrelevant commands to find what they needed.

Solution: Model content into four modes:

Tutorials (new users):
- Getting started with tool
- Your first project

How-to guides (task-oriented):
- Managing configurations
- Deploying to production
- Troubleshooting connection errors

Explanation (concepts):
- How the tool structures projects
- Security model overview

Reference (complete):
- Command reference (one page per command)
- Configuration file reference
- Exit codes reference

Result: Search traffic increased 3x. Reader satisfaction score improved from
3.2/5 to 4.5/5.
```

### Case 3: Versioned Documentation for an API

An API with three active major versions needed versioned documentation that did not confuse readers.

```text
Solution:
- URL structure: /docs/v2/reference/endpoints
- Each version had its own complete documentation tree
- Version selector in the header showed: v3 (latest), v2 (current), v1 (legacy)
- v1 pages had a banner: "This documents an older version. See migration guide."
- Search results showed version badges
- Content was forked at each major release; minor releases used conditional text

Result:
- Confusion about which version to use dropped by 80%
- Migration from v1 to v2 accelerated as readers could compare side-by-side
```

## Examples

### Example 1: Sidebar Navigation Configuration

```yaml
# MkDocs navigation config
nav:
  - Home: index.md
  - Getting Started:
    - Installation: getting-started/installation.md
    - Quickstart: getting-started/quickstart.md
  - Guides:
    - Authentication: guides/authentication.md
    - Deployment: guides/deployment.md
    - Monitoring: guides/monitoring.md
  - Concepts:
    - Architecture: concepts/architecture.md
    - Security: concepts/security.md
  - Reference:
    - API: reference/api.md
    - CLI: reference/cli.md
    - Configuration: reference/config.md
```

### Example 2: Breadcrumb Implementation

```html
<nav aria-label="Breadcrumb">
  <ol>
    <li><a href="/">Home</a></li>
    <li><a href="/guides">Guides</a></li>
    <li><a href="/guides/authentication">Authentication</a></li>
    <li aria-current="page">API Keys</li>
  </ol>
</nav>
```

### Example 3: Content Type Badges

```html
<span class="badge badge-tutorial">Tutorial</span>
<span class="badge badge-guide">How-to Guide</span>
<span class="badge badge-reference">Reference</span>
<span class="badge badge-concept">Explanation</span>
```

### Example 4: Version Banner

```html
<div class="version-banner version-deprecated">
  You are viewing documentation for an outdated version (v1.0).
  <a href="/docs/v2.0">Switch to the latest version</a> or
  <a href="/docs/migration-v1-to-v2">view the migration guide</a>.
</div>
```

### Example 5: Content Model Front Matter

```yaml
---
title: "Deploying to Production"
contentType: how-to-guide
difficulty: advanced
estimatedTime: 30 minutes
prerequisites:
  - "Getting started guide"
  - "Authentication setup"
products: ["cloud", "self-hosted"]
versions: ["v2", "v3"]
---
```

## Glossary

| Term | Definition |
|------|------------|
| Information architecture | The practice of organizing content for intuitive navigation |
| Diataxis | A documentation framework with four modes: tutorials, how-to guides, explanation, reference |
| Content model | A definition of content types, attributes, and relationships |
| Breadcrumb | A navigational element showing the reader's current position in the hierarchy |
| Site map | A hierarchical diagram of all pages in a documentation project |
| Faceted search | Search that allows filtering results by attributes (type, version, product) |
| Versioned documentation | Maintaining separate copies of documentation for each product version |
| Content reuse | Using the same content in multiple locations through includes or snippets |
| Navigation IA | The structure and labeling of navigational elements in documentation |
| Card sorting | A user research method for validating information architecture |
| Front matter | Metadata at the top of a document (title, date, tags) |
| Governance | The processes for managing documentation decisions and ownership |
| LTS | Long-term support — a version maintained for an extended period |

## Quick References

- [Diataxis Documentation Framework](https://diataxis.fr/) — the original framework by Daniele Procida
- [Information Architecture for the Web (Peter Morville)](https://www.oreilly.com/library/view/information-architecture-for/9781491913527/) — foundational IA text
- [MkDocs](https://www.mkdocs.org/) — static site generator for project documentation
- [Docusaurus](https://docusaurus.io/) — documentation site framework with versioning
- [Read the Docs](https://readthedocs.org/) — documentation hosting with version support
- [Card Sorting](https://www.nngroup.com/articles/card-sorting-definition/) — Nielsen Norman Group guide

## Next Steps

- [Diataxis Framework](diataxis.md) — deeper dive into applying the four documentation modes
- [API Docs](api-docs.md) — structuring API reference documentation
