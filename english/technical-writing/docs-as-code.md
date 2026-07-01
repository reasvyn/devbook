# Docs as Code & CI/CD for Documentation

## Description

Docs as Code is the practice of applying software engineering tools and workflows — version control, code review, CI/CD pipelines, automated testing — to documentation. This document covers the philosophy, tooling, and practical implementation of treating documentation as a first-class deliverable in the development process.

## Prerequisites

- [READMEs](readmes.md) — understanding README structure in a version-controlled environment
- [Style Guides](../grammar-and-style/style-guides.md) — automated style checking depends on defined style rules

## Table of Contents

- [Docs as Code Philosophy](#docs-as-code-philosophy)
- [Version Control for Documentation](#version-control-for-documentation)
- [Git Workflows for Docs](#git-workflows-for-docs)
- [Branching Strategies for Documentation](#branching-strategies-for-documentation)
- [Writing in Markdown vs. AsciiDoc](#writing-in-markdown-vs-asciidoc)
- [Static Site Generators](#static-site-generators)
- [CI/CD Pipelines for Docs](#cicd-pipelines-for-docs)
- [Automated Checks](#automated-checks)
- [Spelling and Grammar Checks](#spelling-and-grammar-checks)
- [Link Validation](#link-validation)
- [Style Guide Enforcement](#style-guide-enforcement)
- [Documentation Review Workflows](#documentation-review-workflows)
- [Automated API Doc Generation](#automated-api-doc-generation)
- [Hosting and Deployment](#hosting-and-deployment)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Docs as Code Philosophy

Docs as Code is built on a simple principle: documentation is subject to the same quality standards as code.

**The core tenets:**

```text
1. Documentation lives in the same repository as code (or a closely linked one)
2. Documentation is written in plain text formats (Markdown, AsciiDoc)
3. Documentation goes through the same review process as code changes
4. Documentation is tested automatically (spelling, links, style, build)
5. Documentation is built and deployed automatically
6. Documentation ownership is shared among the team
```

**What Docs as Code enables:**

```text
Version history: Every change to documentation is tracked in Git. Revert any change.
Code review: Documentation changes go through pull requests with reviewer feedback.
Continuous deployment: Docs are built and deployed automatically on merge.
Testing: Automated checks catch broken links, spelling errors, and style violations.
Collaboration: Multiple authors can work on documentation using branches.
```

**Common objections:**

```text
Objection: "Writers are not developers. Git is too complex."
Response: Tools like GitHub's web editor, Netlify CMS, and Forestry provide
GUI-based editing that commits to Git in the background.

Objection: "We do not have time to review docs through PRs."
Response: Fixing a doc bug in a PR costs minutes. Fixing it in production
costs hours of triage and re-deployment.
```

### Version Control for Documentation

**What to version control:**

```text
Everything plain text: .md, .yml, .toml, .json, templates, images (PNG, SVG)
What not to version: Generated HTML/PDF, build artifacts, large binaries (>1MB)
```

**Repository structure options:**

```text
Same repo as code (small/medium projects):
/repo
  /src
  /docs
  mkdocs.yml

Separate docs repo (large projects, dedicated team):
/docs-repo
  /docs
  mkdocs.yml

Monorepo:
/repo
  /packages/pkg-a/docs
  /packages/pkg-b/docs
  /docs (cross-project)
```

### Git Workflows for Docs

**Feature branch workflow:**

```text
1. git checkout -b docs/add-guide
2. Write documentation
3. git commit -m "docs: add authentication guide"
4. git push origin docs/add-guide
5. Create pull request
6. Review and merge
```

**Commit conventions:**

```text
docs: add authentication guide
docs(api): fix parameter type for createUser
docs: fix broken link in getting-started
```

### Branching Strategies for Documentation

**Versioned software releases:**

```text
main — latest stable
v2.x — v2 series (if supported)
v1.x — v1 legacy (critical fixes only)

When a new major version is released: create a branch from main
for the current version, then update main for the next version.
```

### Writing in Markdown vs. AsciiDoc

**Markdown (most common):**

```text
Pros: Universal, GitHub renders natively, vast tool ecosystem, easy to learn
Cons: Limited syntax, inconsistent extensions, verbose tables
Best for: Open-source projects, GitHub-hosted docs, teams with many contributors
```

**AsciiDoc (more powerful):**

```text
Pros: Rich syntax (admonitions, cross-refs, includes), consistent, PDF output
Cons: Less widely known, smaller tool ecosystem, steeper learning curve
Best for: Large doc projects, multi-format output (HTML + PDF), content reuse
```

**Which to choose:**

```text
Markdown if most contributors are developers and GitHub rendering is primary.
AsciiDoc if you need PDF output, complex cross-referencing, or heavy reuse.
```

### Static Site Generators

Static site generators (SSGs) build documentation from markup files into deployable HTML sites.

**MkDocs:**

```text
Best for: Project documentation, smaller sites
Language: Python
Theme: Material for MkDocs (most popular)
Config: mkdocs.yml with nav, theme, and plugins
Built-in: dev server with live reload, search
```

**Docusaurus:**

```text
Best for: Large documentation sites, product docs
Language: JavaScript/React
Features: Versioned docs, blog, custom pages, Algolia search, i18n, MDX
```

**Hugo:**

```text
Best for: Performance, multilingual sites
Language: Go
Features: Fastest build times, built-in i18n, powerful templating
```

**Choosing an SSG:**

```text
MkDocs — Project docs, low learning curve (Python)
Docusaurus — Product docs, versioning, React (JavaScript)
Hugo — Performance, multilingual (Go)
Sphinx — Python projects (Python)
```

### CI/CD Pipelines for Docs

**Basic CI pipeline:**

```yaml
name: Docs CI
on:
  pull_request:
    paths: ["docs/**", "mkdocs.yml"]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Build docs
        run: pip install mkdocs && mkdocs build
      - name: Check links
        run: pip install linkchecker && linkchecker site/
      - name: Lint prose
        uses: errata-ai/vale-action@v2
```

**Deployment pipeline:**

```yaml
name: Deploy Docs
on:
  push:
    branches: [main]
    paths: ["docs/**", "mkdocs.yml"]
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: pip install mkdocs && mkdocs build
      - uses: peaceiris/actions-gh-pages@v3
        with:
          publish_dir: ./site
```

**PR preview deployments:**

```text
Preview documentation changes in pull requests before merging.
Tools: Netlify Deploy Previews, Vercel Preview, Read the Docs PR Builder.
CI builds and deploys the preview, then posts the URL in the PR.
```

### Automated Checks

**Categories of checks:**

```text
Build checks: Does the documentation build without errors?
Content checks: Spelling, grammar, style, broken links, alt text
Consistency: Terminology, heading capitalization, code block language tags
Freshness: Documents not reviewed in 6+ months, deprecated feature references
```

### Spelling and Grammar Checks

**CSpell:**

```json
{
  "words": ["Diataxis", "monorepo"],
  "ignorePaths": ["node_modules"]
}
```

**Vale with write-good:**

```ini
# .vale.ini
StylesPath = .vale/styles
MinAlertLevel = suggestion
[*.md]
BasedOnStyles = write-good
```

### Link Validation

**lychee (fast link checker in Rust):**

```bash
lychee docs/ --exclude "https://twitter.com"
```

**Link maintenance strategy:**

```text
- Check internal links on every PR
- Check external links weekly via scheduled CI
- Use a redirect service for external URLs
- Archive documents with too many dead links
```

### Style Guide Enforcement

**Vale configuration:**

```ini
# .vale.ini
StylesPath = .vale/styles
[*.md]
BasedOnStyles = Google, ProjectStyle
ProjectStyle.Terms = YES
```

**Custom Vale rule example:**

```yaml
# .vale/styles/ProjectStyle/Terms.yml
extends: substitution
message: "Use '%s' instead of '%s'"
level: error
swap:
  "log in": "sign in"
  "execute": "run"
  "leverage": "use"
```

**Other style checking tools:**

```text
write-good: Catches weasel words and complex phrases (Node.js)
alex: Catches insensitive language (gender bias, ableism)
markdownlint: Lints Markdown formatting and syntax
proselint: Catches redundancy, jargon, cliches
```

### Documentation Review Workflows

**Review types:**

```text
Technical review — Subject matter expert verifies accuracy
Editorial review — Writer/editor checks style and grammar
Peer review — Another writer checks clarity and completeness
User review — Target audience member tests the guide
```

**PR-based review workflow:**

```text
1. Author creates branch and writes documentation
2. CI runs automated checks (spelling, links, build)
3. Author assigns reviewers (SME + writer)
4. Reviewers leave comments on the PR
5. Author addresses feedback
6. PR is approved and merged
7. Docs are automatically deployed
```

**Review checklist template:**

```markdown
- [ ] Technical accuracy: Facts are correct, examples work
- [ ] Clarity: The content is easy to understand
- [ ] Completeness: No missing steps or information
- [ ] Style guide: Follows project conventions
- [ ] Links: All links work and are relevant
- [ ] Code: Examples are correct and copy-pasteable
```

### Automated API Doc Generation

**Code annotation tools:**

```text
JSDoc (JavaScript):
/**
 * Creates a new user account.
 * @param {string} email - User email address
 * @returns {Promise<User>} The created user
 */

Sphinx (Python):
def create_user(email: str, password: str) -> User:
    """Creates a new user account."""
```

**OpenAPI-based generation:**

```text
Write an OpenAPI spec and generate reference documentation with:
- Swagger UI: Interactive docs from OpenAPI YAML
- Redoc: Static HTML docs from OpenAPI
- Widdershins: OpenAPI to Markdown conversion

CI pipeline: Update spec → CI validates → Generate HTML → Deploy
```

### Hosting and Deployment

**GitHub Pages:**

```text
Free for public repos. Integrated with GitHub Actions. Custom domain. HTTPS.
```

**Netlify:**

```text
Free tier. Deploy previews. Global CDN. Form handling.
```

**Read the Docs:**

```text
Free for open source. Built-in versioning. Search. PDF output.
```

**Vercel:**

```text
Zero-config. Preview deployments. Edge network.
```

**Choosing a host:**

```text
GitHub Pages — Simplest for projects on GitHub
Netlify — Best preview deployments, feature-rich free tier
Read the Docs — Best versioned docs, search, PDF output
Vercel — Best for Docusaurus or Next.js docs
```

## Summary

{Describe what this documentation change does}

## Checklist

- [ ] I verified all code examples work
- [ ] I checked all links are valid
- [ ] The document follows the style guide
- [ ] Vale reports no errors or warnings
- [ ] The documentation builds without errors
```

## Glossary

| Term | Definition |
|------|------------|
| Docs as Code | Treating documentation with the same tools and workflows as software code |
| CI/CD | Continuous Integration and Continuous Deployment — automated building, testing, and releasing |
| Static site generator | A tool that builds HTML pages from markup files and templates |
| MkDocs | A Python-based static site generator for project documentation |
| Docusaurus | A React-based static site generator for documentation sites |
| Vale | An open-source prose linter for enforcing style guide rules |
| lychee | A fast link checker for finding broken links |
| OpenAPI | A specification for describing REST APIs |
| PR preview | A temporary deployment of documentation from a pull request |
| Linting | Automated checking of content against defined rules |
| Markdown | A lightweight markup language for plain text formatting |
| AsciiDoc | A more feature-rich markup language than Markdown |
| SSG | Static Site Generator |
| Front matter | Metadata at the top of a markup file (title, date, tags) |

## Quick References

- [Docs as Code (Anne Gentle)](https://www.docsasocode.com/) — the definitive book on the subject
- [MkDocs](https://www.mkdocs.org/) — project documentation generator
- [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/) — popular MkDocs theme
- [Docusaurus](https://docusaurus.io/) — documentation site framework
- [Hugo](https://gohugo.io/) — static site generator
- [Vale](https://vale.sh/) — prose linter
- [lychee](https://lychee.cli.rs/) — link checker
- [Read the Docs](https://readthedocs.org/) — documentation hosting platform
- [GitHub Pages](https://pages.github.com/) — free static site hosting
- [Netlify](https://www.netlify.com/) — hosting with deploy previews

## Next Steps

- [Diataxis Framework](diataxis.md) — structuring documentation within a docs-as-code workflow
- [API Docs](api-docs.md) — creating API reference documentation that integrates into CI/CD pipelines
