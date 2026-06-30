# Documentation Maintenance & Lifecycle

## Description

Documentation rots. Code changes, APIs evolve, screenshots go stale, and links break. Without a structured maintenance lifecycle, your documentation gradually becomes inaccurate and erodes user trust. This document covers how documentation ages, how to manage it through creation, review, deprecation, and archiving, and how to measure and prevent documentation debt.

## Prerequisites

- [Docs as Code & CI/CD for Documentation](docs-as-code.md) — maintenance automation depends on docs-as-code infrastructure

## Table of Contents

- [Documentation Rot](#documentation-rot)
- [The Documentation Lifecycle](#the-documentation-lifecycle)
- [Documentation Review Cycles](#documentation-review-cycles)
- [Ownership Models](#ownership-models)
- [Versioning Strategies](#versioning-strategies)
- [Deprecation](#deprecation)
- [When to Update vs Rewrite](#when-to-update-vs-rewrite)
- [Aging Detection](#aging-detection)
- [Documentation Debt](#documentation-debt)
- [Metrics](#metrics)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Documentation Rot

Documentation rot is the gradual deterioration of documentation quality over time. Even when nobody changes anything, the product, ecosystem, and reader expectations change around it.

**Types of rot:**

```text
Content rot:
  Code examples stop working, screenshots no longer match the UI,
  version numbers are outdated, config uses deprecated syntax

Structural rot:
  Links break, navigation no longer matches the product,
  terms change but docs do not

Context rot:
  Best practices evolve, new features make old workflows obsolete,
  industry terminology shifts

Invisible rot:
  Docs still "work" but are suboptimal. Users struggle more than
  they should but do not report it because they assume it is their fault
```

**Why documentation rots:**

```text
1. Product changes without doc updates — feature ships, docs stay behind
2. Ownership gaps — "I thought the other team owned that page"
3. No review cadence — docs written once, never revisited
4. Incentive misalignment — teams measured on shipping features, not docs
5. Broken link accumulation — external sites change, old blog posts disappear
```

**The cost of rot:**

```text
| Cost Type          | Impact                                      |
|--------------------|---------------------------------------------|
| Support tickets    | Users ask questions the docs should answer  |
| User churn         | Frustrated users switch to competitors      |
| Onboarding time    | New developers take longer to get productive|
| Integration bugs   | Consumers code against inaccurate docs      |
| Team productivity  | Internal team wastes time on outdated docs  |
| Brand perception   | Stale docs signal a neglected product       |
```

### The Documentation Lifecycle

Documentation passes through six stages with specific requirements per stage.

**Stage 1: Create**

```text
Activity: Research, outline, write, add examples and code
Output:   First draft
Checklist:
- [ ] Content follows the mandatory format
- [ ] Code examples are tested
- [ ] Links to prerequisites exist
- [ ] File is in the correct directory
- [ ] Parent index.md is updated
```

**Stage 2: Review**

```text
Activity: Technical review (accuracy), editorial review (style),
         peer review (clarity), user review (usability)
Output:  Approved, publishable documentation
Checklist:
- [ ] Technical accuracy verified by SME
- [ ] Code examples tested on clean environment
- [ ] Style guide compliance checked
- [ ] Links validated (internal and external)
- [ ] Review comments addressed
```

**Stage 3: Publish**

```text
Activity: Build docs site, deploy to production, announce
Output:   Published, discoverable documentation
Checklist:
- [ ] Documentation builds without errors
- [ ] URL structure follows conventions
- [ ] Search index updated
- [ ] Navigation includes link to new page
- [ ] Related pages updated with cross-links
```

**Stage 4: Maintain**

```text
Activity: Monitor analytics, address feedback, fix issues,
         update for product changes, refresh periodically
Output:   Continuously accurate documentation
Checklist:
- [ ] Feedback loop is active (inline ratings, comments)
- [ ] Regular freshness review is scheduled
- [ ] Code examples re-tested after release cycles
- [ ] Broken link alerts are monitored
```

**Stage 5: Deprecate**

```text
Activity: Add deprecation notice, add migration path,
         remove from navigation, redirect old URLs
Output:   Deprecated documentation with clear guidance
Checklist:
- [ ] Deprecation banner added (visible, dated)
- [ ] Migration or replacement guide linked
- [ ] Timeline for removal documented
- [ ] Navigation hides or demotes deprecated docs
```

**Stage 6: Archive**

```text
Activity: Remove from active docs, preserve in archive,
         update all cross-references, redirect URLs
Output:   Archived documentation (accessible but not promoted)
Checklist:
- [ ] Page removed from primary navigation
- [ ] Search results demote archived docs
- [ ] All incoming links updated or redirected
- [ ] Archived status is clearly visible
```

**Lifecycle state machine:**

```text
                          ┌─────────────┐
                          │   Create    │
                          └──────┬──────┘
                                 │
                                 ▼
                          ┌─────────────┐
            ┌─────────────│   Review    │────────────┐
            │             └──────┬──────┘            │
            │                    │                    │
            │ Failed             ▼  Approved          │
            │             ┌──────────────┐           │
            └─────────────│   Publish    │           │
                          └──────┬───────┘           │
                                 │                    │
                                 ▼                    │
                          ┌──────────────┐           │
            ┌─────────────│  Maintain    │           │
            │             └──────┬───────┘           │
            │               │    │                    │
            │ Needs update │    │ Deprecated         │
            │               ▼    ▼                    │
            │          ┌──────────────────┐          │
            └──────────│   Deprecate      │          │
                       └───────┬──────────┘          │
                               │                     │
                               ▼                     │
                        ┌──────────────┐             │
                        │   Archive    │             │
                        └──────────────┘             │
                                                      │
                                (Cycle back to Create)┘
```

### Documentation Review Cycles

Different types of reviews happen at different frequencies.

**Review cadences:**

```text
Triggered reviews:
  Per PR:       Automated checks + peer review
  Per release:  Full audit of changed features' docs
  Per deprecation: Review all docs referencing the feature

Scheduled reviews:
  Monthly:      High-traffic pages (top 20%)
  Quarterly:    Medium-traffic pages and critical docs
  Biannually:   Full documentation inventory
  Annually:     Strategy review, persona refresh, IA audit
```

**What to check in each review:**

```text
Monthly (high-traffic pages):
- Code examples still work
- Screenshots match the UI
- Links are valid
- User feedback reviewed

Quarterly (medium-traffic, critical docs):
- All monthly items plus terminology currency
- Deprecation notices up to date
- Version references accurate
- Prerequisites and next steps still relevant

Biannual (full inventory):
- All quarterly items plus owner verification
- No orphaned documents
- Navigation structure still valid
- Content aligns with product direction
```

**Review roles:**

```text
| Role                 | What They Check                        | Frequency              |
|----------------------|----------------------------------------|------------------------|
| Subject matter expert| Technical accuracy, code examples      | Per PR, per release    |
| Technical writer     | Style, clarity, completeness           | Per PR                 |
| Peer reviewer        | Organization, readability, consistency | Per PR                 |
| Product manager      | Feature correctness, roadmap alignment | Per release            |
| User researcher      | Usability, comprehension               | Quarterly              |
```

**Automated vs human review:** Automated every PR: spell check, link check, style check, markdown lint, code validation, readability. Human review: accuracy, completeness, logic, tone, example realism, comprehension.

### Ownership Models

Clear ownership is essential for maintenance. Without it, nothing gets updated.

**Model 1: Direct Responsible Individual (DRI)**

```text
One person owns each page. Named in metadata.
Good for: Small teams, stable features.
Bad for: Large teams (bottleneck), rapidly changing features.
```

**Model 2: Shared Ownership by Team**

```text
Each engineering team owns their feature's docs.
Rotating "docs steward" handles reviews each sprint.
Good for: Feature teams, rapidly changing features.
Bad for: Cross-cutting docs, teams that deprioritize docs.
```

**Model 3: Community Contribution**

```text
Anyone can submit PRs. Maintainers review and merge.
Good for: Open-source projects, large contributor communities.
Bad for: Proprietary products, rapid breaking changes.
```

**Model 4: Dedicated Documentation Team**

```text
Professional writers create and maintain content. SMEs review for accuracy.
Good for: 500+ pages, enterprise products, consistent brand voice.
Bad for: Small startups, highly technical niche tools.
```

**Ownership comparison:**

```text
| Criteria            | DRI       | Team      | Community | Docs Team    |
|---------------------|-----------|-----------|-----------|--------------|
| Quality control     | High      | Medium    | Low       | Very high    |
| Freshness           | Medium    | High      | High      | Low          |
| Consistency         | Medium    | Low       | Low       | Very high    |
| Cost                | Low       | Medium    | Very low  | High         |
| Scalability         | Poor      | Good      | Excellent | Good         |
| Best for            | Stable    | Feature   | Open      | Enterprise   |
|                     | docs      | docs      | source    | docs         |
```

**Choosing a model:** Most organizations use a hybrid. Core product docs use DRI or docs team. Feature-specific docs use team ownership. Open-source projects use community + maintainers.

### Versioning Strategies

**Doc Per Version:** Each version gets its own tree (`/docs/v1.0/`). Perfect accuracy, but high maintenance. Best for products with 2-3 active versions.

**Single Source of Truth with Version Markers:** One tree with inline annotations (`{% if version >= "2.0" %}`). Single file to maintain, but complex conditionals. Best for minor version differences.

**Selective Versioning (Latest + LTS):** Maintain latest and one LTS version. Older versions archived. Balances accuracy with cost. Best for SaaS and fast-moving products.

**Version selector best practices:**

```text
- Label versions: "latest", "LTS", "archived"
- Show banner on non-latest versions: "You are viewing outdated docs"
- Link to migration guide from older versions
- Default to the latest version
```

**Version policy table:**

```text
| Version | Status     | Release Date | End of Docs  | Notes             |
|---------|------------|--------------|--------------|-------------------|
| v3.0    | Active     | 2026-01-15   | TBD          | Current release   |
| v2.5    | LTS        | 2025-06-01   | 2027-06-01   | Security fixes    |
| v2.0    | Maintenance| 2024-09-01   | 2026-03-01   | Critical fixes    |
| v1.0    | Archived   | 2023-03-01   | 2025-09-01   | No longer supported|
```

### Deprecation

Deprecation is the process of marking features as obsolete and guiding users to the replacement.

**When to deprecate documentation:**

```text
Triggers:
- Feature is replaced by a new version
- Feature is removed from the product
- Technology or approach is no longer best practice
- Product line is discontinued

Do NOT deprecate because of low usage — if it still works, it needs docs.
```

**Deprecation timeline:**

```text
T - 6 months:  Add deprecation banner, announce in changelog, link to replacement
T - 3 months:  Update banner with removal date, remove from navigation
T (removal):   Replace with redirect, update cross-references, archive page
T + 1 year:    Remove archived docs from search, redirect remaining links
```

**Deprecation banner:**

```markdown
> **Deprecated:** This page documents a deprecated feature.
> It will be removed on [date].
> See [Migration Guide](migration-guide.md) for the replacement.
```

**Migration guide requirements:**

```text
Every deprecation needs a migration guide covering:
1. What changed and why
2. Timeline (deprecation date, removal date)
3. Step-by-step migration steps
4. Before/after code examples
5. Common issues during migration
6. Rollout plan (staging first, canary deploy)
```

**Deprecation metadata:**

```yaml
---
title: "Legacy Authentication"
deprecated: true
deprecated_version: "3.0"
removal_date: "2027-01-15"
replacement: "authentication-guide.md"
migration_guide: "migration-v2-to-v3.md"
---
```

### When to Update vs Rewrite

Knowing whether to update or rewrite saves time and maintains quality.

**Signals for update:**

```text
- Content is mostly accurate with minor issues
- A few code examples are outdated
- Screenshots need refreshing
- Version numbers or dates are wrong
- Links need fixing
- A new section should be added to an otherwise good document
Effort: 15 minutes to 2 hours
```

**Signals for rewrite:**

```text
- Product behavior has fundamentally changed
- More than 40% of content is outdated or wrong
- Document has been patched 5+ times and is incoherent
- Audience has changed (junior → senior)
- User satisfaction is consistently below 3.0/5.0
- Support tickets repeatedly reference this doc as the cause
Effort: 4 hours to several days
```

**Update vs rewrite matrix:**

```text
| Condition                              | Update | Rewrite |
|----------------------------------------|--------|---------|
| Less than 20% of content is outdated   | ✓      |         |
| More than 40% of content is outdated   |        | ✓       |
| Structure still works                  | ✓      |         |
| Doc patched 5+ times                   |        | ✓       |
| Single feature changed                 | ✓      |         |
| Product fundamentally changed          |        | ✓       |
| User satisfaction > 3.5                | ✓      |         |
| User satisfaction < 2.5                |        | ✓       |
```

**Rewrite process:**

```text
1. Audit existing doc — what is useful, outdated, missing?
2. Plan new structure — write outline based on current reader needs
3. Write from scratch — do not edit the old version
4. Review — technical, editorial, user
5. Replace — publish new version, set up redirects, update cross-refs
6. Measure — track satisfaction, support tickets, search CTR before and after
```

### Aging Detection

You cannot fix what you do not measure. Aging detection automates the discovery of stale documentation.

**Automated freshness checks:**

```bash
#!/bin/bash
THRESHOLD=180  # 6 months in days
for file in docs/**/*.md; do
  last_modified=$(git log -1 --format="%ct" "$file" 2>/dev/null)
  [ -z "$last_modified" ] && continue
  age=$(( ($(date +%s) - last_modified) / 86400 ))
  if [ "$age" -gt "$THRESHOLD" ]; then
    echo "STALE: $file (last updated $age days ago)"
  fi
done
```

**Freshness metadata:**

```yaml
---
title: "Installation Guide"
last_reviewed: "2026-03-15"
review_interval: "180"
owner: "alice@example.com"
review_status: "current"  # current, due, overdue, stale
---
```

**Stale content flags:**

```text
| Flag                  | Rule                     | Action                  |
|-----------------------|--------------------------|-------------------------|
| Last reviewed > 6 mo  | Due for review           | Notify owner            |
| Last reviewed > 1 yr  | Stale                    | Schedule rewrite audit  |
| Last reviewed > 2 yr  | Severely stale           | Evaluate for archive    |
| Broken links > 3      | Link rot                 | Fix links, review doc   |
| User rating < 3.0     | Unhelpful                | Triage for update       |
| Support tickets > 5   | Causing friction         | Prioritize rewrite      |
| No views in 90 days   | Not useful               | Consider archiving      |
```

**Broken link detection schedule:**

```text
- Every PR: internal links only (fast)
- Weekly: external links (scheduled CI job)
- Pre-release: full audit of all links
```

**Content decay scoring:**

```text
| Dimension            | Weight | 0 pts (< 6 mo) | 1 pt (6-12 mo) | 2 pts (> 12 mo) |
|----------------------|--------|-----------------|-----------------|------------------|
| Last updated         | 3x     |                 |                 |                  |
| Last reviewed        | 2x     |                 |                 |                  |
| Broken links         | 3x     | 0 links         | 1-3 links       | 4+ links         |
| User rating          | 2x     | 4.0+            | 3.0-3.9         | < 3.0            |
| Support tickets/mo   | 2x     | 0-1             | 2-5             | 6+               |

Score interpretation: 0-4 healthy, 5-9 monitor, 10-15 attention, 16+ critical
```

### Documentation Debt

Documentation debt is the accumulated cost of deferred documentation work.

**Types of documentation debt:**

```text
Coverage debt:  Features shipped without docs, public APIs missing from reference
Accuracy debt:  Outdated examples, incorrect instructions, deprecated features still documented
Freshness debt: Docs not reviewed in over a year, no review schedule
Structural debt: Orphaned pages, duplicate content, broken navigation
Quality debt:   Style guide violations, missing glossary definitions, poor accessibility
```

**Tracking documentation debt:**

```text
Debt register format:

| ID  | Document       | Type       | Severity | Owner    | Status       |
|-----|----------------|------------|----------|----------|--------------|
| D-1 | auth-guide.md  | Accuracy   | Critical | A. Kumar | In progress  |
| D-2 | api-ref.md     | Coverage   | High     | J. Chen  | Triaged      |
| D-3 | deploy.md      | Freshness  | Medium   | L. Perez | Backlog      |
```

**Prioritizing debt:**

```text
Priority = Impact × Frequency × Confidence

Impact (1-5): 5 = data loss/security, 4 = blocked task, 3 = confusion,
              2 = minor, 1 = cosmetic
Frequency (1-5): 5 = all users, 4 = most users, 3 = some, 2 = few, 1 = very few
Confidence (1-5): 5 = confirmed by data, 3 = suspected, 1 = guess

Thresholds: 60-125 critical, 30-59 high, 10-29 medium, 1-9 low
```

**Budgeting for debt:**

```text
Recommended allocation: 20% of documentation time for debt reduction.
1 sprint per quarter dedicated to doc debt.

Prevention strategies:
- Documentation in definition of done (feature is not complete without docs)
- Automated freshness reminders via CI
- Documentation champions in each engineering team
- Regular debt sprints each quarter
```

### Metrics

What gets measured gets maintained.

**Documentation health score:**

```text
Composite = (
  Coverage × 0.25 +
  Freshness × 0.25 +
  Accuracy × 0.20 +
  Satisfaction × 0.15 +
  Accessibility × 0.15
) × 100

Health score: 90-100 excellent, 75-89 good, 50-74 needs improvement, below 50 critical
```

**Key metrics:**

```text
Coverage:  Documented features / Total public features × 100
           Target: 95%+ for public APIs, 80%+ for all features

Freshness: Docs reviewed within interval / Total docs × 100
           Target: average freshness age under 120 days

Accuracy:  Pages with no known issues / Total pages × 100
           Target: over 90%

Satisfaction: Yes votes / (Yes + No votes) × 100
              Target: over 80%
              NPS target: above +30
```

**Documentation dashboard:**

```text
Documentation Health — Q2 2026

Overall:
  Health Score:  76/100 (Good)        ▲ +5 from Q1
  Total docs:    156
  Coverage:      92%                  ▲ +2%
  Freshness:     72%                  ▲ +7%
  Accuracy:      88%                  ▼ -1%
  Satisfaction:  83%                  ▲ +3%

Debt:
  Critical:      8
  High:          15
  Medium:        22
  Low:           14
  Effort:        320 hours

Trend: Q1: 71 → Q2: 76 → Target: 85
```

**Metrics review cadence:**

```text
Weekly (automated):  Broken link count, new doc-related support tickets
Monthly (team):      Top 10 most-viewed pages freshness, lowest-rated pages
Quarterly (team):    Full health score, coverage gap analysis, debt prioritization
Annually (product):  Strategy review, persona refresh, full content audit
```

## Study Cases

### Case 1: Documentation Debt Sprint at a SaaS Company

A platform had 500 docs pages. 40% had not been updated in 2+ years.

```text
Problem: 200 outdated pages, 150 unowned, 120 support tickets/month, 3.1/5 rating.

Quarter 1: Inventory and triage — 45 critical, 80 high-priority pages identified
Quarter 2: Fix critical — 33 rewritten, 12 archived, ownership assigned
Quarter 3: Fix high-priority — 60 pages updated, review cadence established
Quarter 4: Prevent — ownership to teams, freshness dashboard built

Results (12 months): Tickets 120→45/month, satisfaction 3.1→4.2,
freshness 30%→78%, debt 200→45 pages.
```

### Case 2: API Version Deprecation and Migration

An API platform deprecated v1. They had 200 pages documenting v1 endpoints.

```text
Plan:
Month 1: Deprecation banners on all 200 v1 pages
Month 2-3: Migration guide written, before/after examples
Month 4-5: Navigation restructured, v1 under "Legacy", redirects set up
Month 6: v1 archived, removed from search

Results: v1 traffic 60%→5%, migration support tickets: 15 (expected 100+).
```

### Case 3: Ownership Model Transition

A startup grew from 3 to 30 engineers. DRI model no longer scaled.

```text
Problem: 3 months behind on updates, 40% of pages unowned, 25% freshness.

Transition: Docs divided by feature area, each team owned their docs,
tech writer embedded for training, rotating docs steward role.

Results: Freshness 25%→80%, unowned pages 40%→2%, update latency 3 mo→2 weeks.
```

## Examples

### Example 1: Freshness Metadata in Front Matter

```yaml
---
title: "Authentication Guide"
owner: "alice@example.com"
owner_team: "auth-engineering"
last_reviewed: "2026-03-15"
review_interval_days: 180
review_status: "current"
page_views_month: 2400
user_rating: 4.3
known_issues: 2
---
```

### Example 2: Deprecation Banner

```markdown
> **Deprecated:** This page documents the v2 authentication system.
> v2 auth is deprecated as of v3.0 (2026-01-15) and will be removed on
> 2027-01-15. See [v3 Auth Guide](auth-guide-v3.md) and
> [Migration Guide](migration-v2-to-v3.md).
```

### Example 3: Documentation Debt Register

```text
# Documentation Debt Register

## Critical (this sprint)
| ID  | Document          | Issue                        | Owner    | Effort |
|-----|-------------------|------------------------------|----------|--------|
| D-8 | deploy-guide.md   | CLI flags changed in v3.2    | L. Perez | 4h     |
| D-12| error-codes.md    | Missing 8 new error codes    | A. Kumar | 6h     |
| D-15| quickstart.md     | Code example broken in v3.2  | J. Chen  | 2h     |

## High (within 2 sprints)
| ID  | Document          | Issue                        | Owner    | Effort |
|-----|-------------------|------------------------------|----------|--------|
| D-3 | api-ref.md        | 3 endpoints missing docs     | M. Torres| 12h    |
| D-7 | auth-guide.md     | Screenshots outdated         | L. Perez | 3h     |
```

### Example 4: Automated Freshness Reminder

```markdown
> **Freshness Review Due**
>
> This document was last reviewed on 2025-12-01 (210 days ago).
> Maximum review interval is 180 days.
>
> Please review for: code example accuracy, screenshot validity,
> link health, terminology correctness.
>
> After review, update the `last_reviewed` date in front matter.
```

### Example 5: Lifecycle State in Index

```text
| Document              | Lifecycle Stage | Owner      | Last Updated     |
|-----------------------|-----------------|------------|------------------|
| Getting started       | Maintain        | Docs team  | 2026-06-01       |
| Authentication guide  | Maintain        | Auth eng   | 2026-05-15       |
| V2 API reference      | Deprecating     | API eng    | 2026-04-01       |
| V1 API reference      | Archived        | -          | 2024-12-01       |
```

## Glossary

| Term | Definition |
|------|------------|
| Documentation rot | The gradual deterioration of documentation quality over time |
| Documentation lifecycle | The six stages: create, review, publish, maintain, deprecate, archive |
| Documentation debt | The accumulated cost of deferred documentation work |
| DRI | Direct Responsible Individual — a single person who owns a document |
| Freshness | A metric measuring how recently documentation was reviewed or updated |
| Stale content | Documentation not reviewed within its review interval |
| Deprecation | The process of marking a feature as obsolete and guiding users to the replacement |
| Migration guide | Documentation that helps users transition from an old feature to its replacement |
| Coverage gap | A feature or API endpoint that lacks documentation |
| Content decay | The process by which documentation becomes less accurate over time |
| Review interval | The maximum time between documentation reviews for a page |
| Version marker | Inline annotations indicating which product version content applies to |
| Selective versioning | Maintaining documentation for only the latest and LTS versions |
| Doc per version | Maintaining separate documentation trees for each product version |
| Ownership model | The system for assigning responsibility for documentation maintenance |
| Docs steward | A rotating role within a team responsible for documentation health |
| Definition of done | A checklist including documentation before a feature ships |
| Documentation champion | A team member who manages documentation quality |
| Doc health score | A composite metric measuring overall documentation quality |
| Decay score | A score indicating how outdated a document is |
| Freshness check | An automated process identifying documents needing review |
| Archive | The final lifecycle stage: preserved but no longer promoted |
| Documentation NPS | A metric measuring user satisfaction with documentation |

## Quick References

- [Write the Docs: Documentation Debt](https://www.writethedocs.org/guide/doc-debt/) — community resources on managing documentation debt
- [Google Developer Docs: Versioning](https://developers.google.com/style/versioning) — versioning documentation for APIs and products
- [lychee Link Checker](https://lychee.cli.rs/) — fast link checker for CI integration
- [Semantic Versioning](https://semver.org/) — version numbering conventions
- [Keep a Changelog](https://keepachangelog.com/) — standards for changelogs and release notes

## Next Steps

- [Style Guides & Consistency](style-guides.md) — establishing style rules that make maintenance easier
- [Testing & Reviewing Documentation](documentation-testing.md) — automated and manual testing to prevent documentation rot
