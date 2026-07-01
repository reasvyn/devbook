# Changelogs & Release Notes

## Description

Changelogs and release notes communicate what changed between versions of a product. They serve users evaluating upgrades, developers debugging regressions, and teams coordinating releases. This document covers the conventions, structure, and tooling for writing changelogs that are clear, complete, and useful.

## Prerequisites

- [Style Guides](../grammar-and-style/style-guides.md) — understanding style conventions before writing changelogs that follow consistent terminology and formatting

## Table of Contents

- [Why Changelogs Matter](#why-changelogs-matter)
- [The Keep a Changelog Convention](#the-keep-a-changelog-convention)
- [Semantic Versioning and Changelogs](#semantic-versioning-and-changelogs)
- [Organizing by Type](#organizing-by-type)
- [Writing for Different Audiences](#writing-for-different-audiences)
- [Linking to Issues and Pull Requests](#linking-to-issues-and-pull-requests)
- [Automated Changelog Generation](#automated-changelog-generation)
- [Release Announcements](#release-announcements)
- [Changelog Maintenance Practices](#changelog-maintenance-practices)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Why Changelogs Matter

Changelogs serve multiple audiences with different needs:

**For users.** Changelogs help users decide whether and when to upgrade. A user evaluating a new version wants to know: What new features will I get? What bugs are fixed? Will any breaking changes affect my workflow?

**For developers.** Changelogs serve as a historical record of changes. When debugging a regression, developers scan changelogs to identify which change introduced the issue. When planning a release, developers review the changelog to ensure all changes are accounted for.

**For the organization.** Changelogs demonstrate transparency and professionalism. A well-maintained changelog signals that the team is organized and respects its users' time.

**Changelogs vs. release notes.** Changelogs are detailed, chronological records of all changes. Release notes are curated summaries for what the user needs to know. Many projects maintain both.

### The Keep a Changelog Convention

Keep a Changelog is the most widely adopted convention for changelog formatting. It defines a standard structure:

```text
# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/),
and this project adheres to [Semantic Versioning](https://semver.org/).

## [1.2.0] - 2026-06-29

### Added

- New export to PDF feature with page layout options
- Rate limiting on the public API endpoint

### Changed

- Upgraded authentication library to v3.0
- Increased default timeout from 30s to 60s

### Fixed

- Crash when loading malformed JSON configuration files
- Memory leak in the event stream processor

## [1.1.0] - 2026-05-15

### Added

- Dark mode support

### Deprecated

- The legacy API v1 endpoint will be removed in v2.0

## [1.0.1] - 2026-04-01

### Security

- Fixed XSS vulnerability in user profile rendering
```

**Rules of Keep a Changelog:**

```text
1. One file per project (CHANGELOG.md), not per version.
2. Newest version at the top (reverse chronological order).
3. Each version has a release date in YYYY-MM-DD format.
4. Changes are grouped by type (Added, Changed, Deprecated, etc.).
5. Versions link to their diff on the source control platform.
6. Each entry is a single line or paragraph.
7. The unreleased section at the top contains changes not yet released.
```

**Why Keep a Changelog works:**

```text
- Consistent structure reduces cognitive load for readers
- Reverse chronological order puts the most relevant information first
- Categorized entries enable scanning by type of change
- Date-stamped versions help correlate changes with releases
- Standard format works with automated tooling
```

### Semantic Versioning and Changelogs

Semantic Versioning (SemVer) defines how version numbers change based on the type of changes made.

**MAJOR.MINOR.PATCH pattern:**

```text
MAJOR: Breaking changes (incompatible API changes)
MINOR: New features (backward compatible)
PATCH: Bug fixes (backward compatible)
```

**Mapping changelog entries to SemVer:**

```text
MAJOR (breaking):
- Removed sections
- Changed sections that alter existing behavior incompatibly
- Deprecated entries that preview upcoming removals

MINOR (new features):
- Added sections with new functionality
- Changed sections that add backward-compatible enhancements

PATCH (fixes):
- Fixed sections
- Security sections
- Minor Changed entries that do not alter behavior
```

**Unreleased section:**

```text
## [Unreleased]

### Added
- New admin dashboard (planned for v2.0)

### Fixed
- Login redirect loop on expired sessions

The [Unreleased] section accumulates changes between releases.
When the release is cut, [Unreleased] becomes [2.0.0] with the release date,
and a new [Unreleased] section starts empty.
```

**Pre-release versions:**

```text
## [2.0.0-beta.1] - 2026-06-01

### Added
- New graphQL API (beta)

### Changed
- Database schema migration to v3 (preview)

Pre-release versions are included in the changelog but clearly labeled.
Users of stable releases can ignore pre-release entries.
```

### Organizing by Type

The Keep a Changelog convention defines six standard change types. Each type has a specific meaning:

**Added — new features and capabilities:**

```text
### Added
- Real-time collaboration mode for shared documents
- New `--format` flag to specify output format
- Support for WebAuthn hardware security keys
- Audit logging for all admin actions

Rules:
  - New functionality that did not exist before
  - New parameters, options, or configuration settings
  - New documentation pages or sections
```

**Changed — modifications to existing functionality:**

```text
### Changed
- Default session timeout increased from 30 to 60 minutes
- Minimum password length changed from 6 to 8 characters
- Dashboard layout redesigned for accessibility compliance
- Dependency: upgraded Express from 4.18 to 4.19

Rules:
  - Backward-compatible modifications to existing features
  - Performance improvements, refactoring, UI changes
  - Dependency upgrades (unless they fix security issues)
```

**Deprecated — features scheduled for removal:**

```text
### Deprecated
- Legacy API v1 endpoint — use v2 instead
- The `--old-format` flag will be removed in v3.0
- Support for Node 16 will end in Q3 2026

Rules:
  - Features still available but slated for removal
  - Users should migrate to alternatives
  - Include the planned removal version when known
```

**Removed — features that have been dropped:**

```text
### Removed
- Support for Internet Explorer 11
- The deprecated `/api/v1/users` endpoint
- PHP 7.x compatibility layer

Rules:
  - Features that were previously deprecated and are now gone
  - Breaking changes (MAJOR version bump required)
  - Include migration path reference when possible
```

**Fixed — bug fixes and corrections:**

```text
### Fixed
- Crash when opening files with Unicode characters in filenames
- Memory leak in WebSocket connection pool
- Incorrect timezone handling in scheduled reports
- Typo in the installation guide (thanks to @contributor)

Rules:
  - Bug fixes for incorrect behavior
  - Security vulnerabilities (see Security section)
  - Documentation corrections
```

**Security — vulnerability fixes and security improvements:**

```text
### Security
- Fixed SQL injection vulnerability in search endpoint (CVE-2026-1234)
- Updated TLS certificate validation to reject SHA-1 certificates
- Patched XSS vulnerability in comment rendering (CVE-2026-5678)

Rules:
  - Security fixes should include CVE numbers when available
  - List severity level if known (Critical, High, Medium, Low)
  - Encourage users to upgrade promptly
```

**Additional types used by some projects:**

```text
### Performance
- Improved query cache hit rate by 40%
- Reduced bundle size by 25% through tree shaking

### Documentation
- Added API reference for the new webhook system
- Updated getting-started guide for v2 migration

### Infrastructure
- Migrated CI/CD from Jenkins to GitHub Actions
- Added staging environment for load testing
```

### Writing for Different Audiences

**Developer changelogs** focus on technical details — what changed in the code, migration instructions, API signatures.

```text
Developer changelog entry:
### Changed
- `createClient({ timeout })` now accepts timeout in milliseconds
  (previously seconds). Update your configuration:
  ```js
  // Before
  createClient({ timeout: 30 })
  // After
  createClient({ timeout: 30000 })
  ```
```

**End-user release notes** focus on user-visible changes — new features, fixed problems, workflow improvements.

```text
End-user release notes:
## Version 3.0 — What's New

New export feature: You can now export your documents as PDF files with
custom page layouts. Go to File > Export to PDF to try it.

Faster performance: Reports load 40% faster thanks to query optimizations.

Bug fixes:
- Fixed an issue where some Unicode characters caused the app to crash
- Fixed a timezone display error in scheduled reports
```

**When to use each:**

```text
Open-source library:    Developer changelog (CHANGELOG.md)
SaaS product update:    End-user release notes (blog post, in-app notification)
Enterprise software:    Both — changelog for admins, release notes for end users
Mobile app:             App store release notes (end-user focused)
Internal tool:          Developer changelog (teams need migration details)
```

**Combined approach:**

```text
## [2.0.0] - 2026-06-29

For end users:
  New dashboard with customizable widgets. Reports now export to PDF.

For developers:
### Added
- Custom widget API with React component support
- PDF rendering service with template system

### Changed
- BREAKING: Dashboard plugin API v1 is replaced by v2. Migrate plugins.
- Default cache TTL changed from 300s to 600s.

### Removed
- Legacy dashboard v1 (deprecated since v1.8)

Upgrade guide: https://example.com/docs/upgrade-v2
```

### Linking to Issues and Pull Requests

Changelog entries should include references to related issues, pull requests, or commit hashes.

**Format conventions:**

```text
### Fixed
- Crash when loading malformed JSON config files
  ([#892](https://github.com/org/repo/issues/892))
- Memory leak in WebSocket pool
  ([PR #915](https://github.com/org/repo/pull/915))
```

**When to link:**

```text
- Every bug fix should link to the issue it closes
- Every feature should link to the feature request or epic
- Breaking changes should link to migration documentation
- Security fixes should link to the security advisory
- Community contributions should credit the contributor
```

**Contributor attribution:**

```text
### Added
- Dark mode support (thanks to @janesmith in [#723])

### Fixed
- Timezone handling in calendar view ([#812] by @bobwilson)
```

**Dependency references:**

```text
### Changed
- Upgraded `lodash` from 4.17.20 to 4.17.21
  ([CVE-2021-23337](https://nvd.nist.gov/vuln/detail/CVE-2021-23337))
```

### Automated Changelog Generation

Manual changelog maintenance is error-prone and easily forgotten. Automated tools generate changelogs from commit history.

**Conventional Commits.** A commit message convention that maps to changelog types:

```text
feat: add PDF export with page layout options
  → Appears in "Added" section

fix: prevent crash on malformed JSON config
  → Appears in "Fixed" section

feat!: redesign dashboard plugin API (breaking change)
  → Appears in "Changed" with BREAKING label

docs: update getting-started guide for v2
  → Appears in "Documentation" section (if enabled)

chore: upgrade lodash to 4.17.21
  → Appears in "Changed" section
```

**Conventional Commits format:**

```text
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]

Types: feat, fix, docs, style, refactor, perf, test, chore, ci
Breaking change: add ! after type/scope, or add BREAKING CHANGE footer
```

**git-cliff.** A changelog generator configured with a template:

```toml
# cliff.toml
[changelog]
header = "# Changelog\n"
body = """
{% for group, commits in commits | group_by(attribute="group") %}
### {{ group | upper_first }}
{% for commit in commits %}
- {{ commit.message | upper_first }}
{% endfor %}
{% endfor %}
"""
```

**Standard Version / Release Please.** Automates version bumps and changelog updates from Conventional Commits. Pipeline: analyze commits, determine version, update CHANGELOG.md, create tag and release.

**CI workflow for changelog generation:**

```yaml
name: Release
on:
  push:
    branches: [main]
jobs:
  release:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Generate changelog
        run: git-cliff -o CHANGELOG.md
      - name: Create release
        uses: softprops/action-gh-release@v2
        with:
          body_path: CHANGELOG.md
```

**Limitations of automated changelogs:** Entries are only as good as commit messages. Automated entries lack context and user-friendly explanations. Use automation for the first draft, then curate manually.

### Release Announcements

Release announcements complement changelogs by providing context, highlights, and guidance.

**Release blog post structure:** Summary paragraph, top 3 highlights, upgrade considerations, full changelog link, what is coming next, call to action.

**In-app release notes:**

```text
What's New in v3.0
- Real-time collaboration — edit documents with your team simultaneously
- PDF export with custom layouts
- 40% faster report loading
- Bug fixes and performance improvements

[See full changelog] [Dismiss]
```

**Release tagging conventions:**

```text
Git tag:    v3.0.0 — matches the version in CHANGELOG.md
GitHub release: Title = version, Body = changelog entry for that version
npm publish: --tag next for pre-releases
Docker tag: latest, 3.0.0, 3.0, 3
```

### Changelog Maintenance Practices

**Update frequently, not in batch.** Add changelog entries as changes are merged, not at release time. This prevents forgotten entries and reduces release-day stress.

**Guidelines:**

```text
- Add the changelog entry in the same PR as the change
- Keep the [Unreleased] section always up to date
- Review the changelog before each release cut
- Remove entries that are internal-only or trivial
```

**Changelog in code review:**

```text
Review check:
- [ ] Does the PR require a changelog entry?
- [ ] Is the entry in the correct type section?
- [ ] Is the entry clear and user-focused?
- [ ] Does the entry link to the issue/PR?
- [ ] Is the entry correctly placed under [Unreleased]?
```

**What not to include:**

```text
- Internal refactoring with no user impact
- Code style changes (formatting, linting)
- Dependency upgrades with no behavioral change
- Development tooling changes
- Documentation typos (unless user-facing)

Rule of thumb: If a user would care about it, include it. If not, omit it.
```

**Changelog diff linking:**

```text
## [1.2.0] - 2026-06-29
## [1.1.0] - 2026-05-15

Link each version to its diff:
[1.2.0]: https://github.com/org/repo/compare/v1.1.0...v1.2.0
[1.1.0]: https://github.com/org/repo/compare/v1.0.0...v1.1.0
```

## [1.0.0] - 2026-06-29

### Added
- User authentication with OAuth 2.0
- Dashboard with customizable widgets
- Report export to CSV and PDF

### Fixed
- Memory leak in data stream processing
- Calendar date rendering off by one day

### Security
- Patched XSS vulnerability in comment widget (CVE-2026-1234)

[1.0.0]: https://github.com/org/repo/releases/tag/v1.0.0
```

### Case 2: Automated Changelog with Manual Curation

```text
Before (automated only, no curation):
## [2.0.0] - 2026-06-29
- feat: add new dashboard
- fix: update deps
- chore: bump version
- refactor: auth module

After (automated + curated):
## [2.0.0] - 2026-06-29

### Added
- New customizable dashboard with drag-and-drop widgets

### Changed
- Authentication module rewritten for better security and performance
- Minimum Node.js version raised to 18.x

### Fixed
- Dashboard crash when loading large datasets
```

### Case 3: Breaking Change Communication

```text
## [3.0.0] - 2026-06-29

### Changed
- BREAKING: API rate limit format changed from hourly to per-minute.
  Update your rate limit headers: `X-RateLimit-Hour` → `X-RateLimit-Minute`.
  Migration guide: https://example.com/docs/rate-limiting-v3

### Added
- Rate limit headers now include remaining seconds in the window

### Removed
- Legacy API v1 (deprecated since v2.0)
```

### Case 4: End-User Release Notes

```text
Title: Better Collaboration, Faster Reports

We are excited to announce version 3.0 of our platform. This release focuses
on making it easier to work with your team and get insights faster.

New in this release:
- Real-time collaboration: Edit documents with your team simultaneously.
  See who is editing what and changes appear instantly.
- PDF export: Export any report as a PDF with custom page layouts.
- Performance: Reports load 40% faster thanks to query optimizations.
- Bug fixes: Fixed crash with Unicode filenames, timezone display issues.

Upgrading is automatic — all users will be on v3.0 by July 1.

[View full changelog](https://example.com/changelog)
```

### Case 5: Unreleased Section Management

```text
## [Unreleased]

### Added
- Webhook-based event notifications (planned for v2.1)

### Fixed
- Search indexing fails for documents over 100 pages

---

## [2.0.0] - 2026-06-29
...
```

## [1.3.0] - 2026-06-29

### Added
- Dark mode toggle in user preferences

### Fixed
- Screen flicker when switching between dark and light mode
- Missing padding on mobile navigation menu

[1.3.0]: https://github.com/org/repo/compare/v1.2.0...v1.3.0
```

### Example 2: Security Fix Entry

```markdown
### Security
- Fixed path traversal vulnerability in file upload handler
  (CVE-2026-7890, severity: High)
- Updated TLS certificate validation (CVE-2026-7891)
```

### Example 3: Deprecation Notice

```markdown
### Deprecated
- The `--legacy-mode` flag is deprecated and will be removed in v4.0.
  Use the default mode instead.
- Python 3.8 support will end in v3.5 (planned Q4 2026).
```

### Example 4: Multi-Project Monorepo Changelog

```markdown
## [2.0.0] - 2026-06-29

### @org/core
- Added WebSocket-based real-time sync
- Removed deprecated `sync()` method (use `syncRealtime()`)

### @org/cli
- Changed default output format from JSON to YAML
- Fixed crash on empty input files
```

### Example 5: Community Contributions

```markdown
### Fixed
- Image compression quality issue for JPEG files
  (thanks to @alexchen in [#456])
- Missing ARIA labels on navigation elements
  (reported by @sarahm in [#459], fixed in [#462])
```

### Example 6: Pre-Release Versions

```markdown
## [3.0.0-beta.1] - 2026-06-01
### Added
- Plugin system with hot-reloading (beta)

### Known Issues
- Plugin hot-reload does not work on Windows
- Memory usage increases 10-15% with 10+ plugins active
```

### Example 7: Empty Unreleased Section

```markdown
## [Unreleased]

### Added

### Changed

### Fixed
```

### Example 8: Changelog with Upgrade Guides

```markdown
## [2.0.0] - 2026-06-29

This release contains breaking changes. See the
[migration guide](https://example.com/docs/v2-migration) for details.

### Changed
- BREAKING: Configuration file format changed from INI to YAML.
  Run `migrate-config` to convert your existing config automatically.
```

### Example 9: Conventional Commit to Changelog Mapping

```text
Commit: feat(auth): add biometric login support
Changelog: ### Added - Biometric login support

Commit: fix(api)!: change response format for /users endpoint
Changelog: ### Changed - BREAKING: Response format for /users endpoint changed

Commit: fix: handle null values in report generator
Changelog: ### Fixed - Handle null values in report generator
```

### Example 10: Release Tag Convention

```text
Git tags:   v1.0.0  v1.1.0  v2.0.0  v2.0.1
npm tags:   latest  next    beta    dev
Docker:     latest  2.0.0   2.0     2
```

## Glossary

| Term | Definition |
|------|------------|
| Changelog | A curated, chronological record of notable changes for each version of a project |
| Release notes | A user-facing summary of changes in a release, often curated for a broader audience |
| Keep a Changelog | A convention for formatting changelogs with standard sections and reverse-chronological order |
| Semantic Versioning | A versioning scheme using MAJOR.MINOR.PATCH to communicate change types |
| MAJOR version | Incremented for incompatible API changes (breaking changes) |
| MINOR version | Incremented for backward-compatible new features |
| PATCH version | Incremented for backward-compatible bug fixes |
| Conventional Commits | A commit message convention that maps commit types to changelog sections |
| git-cliff | A highly configurable changelog generator that parses conventional commits |
| Unreleased section | The section at the top of the changelog for changes not yet released |
| Breaking change | A change that breaks backward compatibility, requiring a major version bump |
| Deprecation | Marking a feature as scheduled for removal while keeping it functional |
| CVE | Common Vulnerabilities and Exposures — a standard identifier for security vulnerabilities |
| Pre-release | A version released for testing before the stable release (alpha, beta, rc) |
| Diff link | A URL comparing two versions on a source control platform |
| Release tag | A git tag marking a specific commit as a release |
| Monorepo | A single repository containing multiple packages, each with its own changelog |

## Quick References

- [Keep a Changelog](https://keepachangelog.com/) — the standard convention for changelog formatting
- [Semantic Versioning](https://semver.org/) — the versioning specification
- [Conventional Commits](https://www.conventionalcommits.org/) — commit message convention for automated changelogs
- [git-cliff](https://git-cliff.github.io/) — configurable changelog generator
- [Release Please](https://github.com/googleapis/release-please) — automated release tool from Google
- [Standard Version](https://github.com/conventional-changelog/standard-version) — automated versioning and changelog tool
- [Sentinel Changelog](https://docs.sentry.io/changelog/) — example of a well-maintained developer changelog
- [VS Code Release Notes](https://code.visualstudio.com/updates) — example of user-focused release notes
- [Keep a Changelog: Examples](https://keepachangelog.com/en/1.1.0/#examples) — real-world changelog examples

## Next Steps

- [Docs as Code & CI/CD for Documentation](docs-as-code.md) — integrating changelog generation into the CI pipeline
- [API Reference Documentation](api-docs.md) — synchronizing API documentation with changelog entries for API changes
