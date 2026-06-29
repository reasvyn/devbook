# Stakeholder Updates & Status Reports

## Description

Engineers must communicate project status to a variety of audiences: team members, managers, executives, and external stakeholders. Each audience needs different information at different levels of detail. This document covers how to write effective status updates that inform, build trust, and drive decisions.

## Prerequisites

- [Conciseness](../grammar-and-style/conciseness.md) — keeping status updates tight and actionable
- [Email & Slack Communication for Engineers](email-slack.md) — channel selection and async messaging fundamentals

## Table of Contents

- [Understanding Your Audience](#understanding-your-audience)
- [Writing Effective Status Reports](#writing-effective-status-reports)
- [Choosing the Right Level of Detail](#choosing-the-right-level-of-detail)
- [KPIs and Metrics in Updates](#kpis-and-metrics-in-updates)
- [Async Updates vs. Meetings](#async-updates-vs-meetings)
- [Handling Bad News](#handling-bad-news)
- [Requesting Decisions](#requesting-decisions)
- [Weekly vs. Milestone Updates](#weekly-vs-milestone-updates)
- [Email vs. Dashboard Formats](#email-vs-dashboard-formats)
- [Status Report Templates](#status-report-templates)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Understanding Your Audience

Different stakeholders have different concerns. Effective updates tailor the message to the audience.

**Engineering team members** care about:

- Technical progress: what features are implemented, what is in review, what is blocked
- Dependencies: what they need from other teams
- Technical decisions: architecture, design trade-offs, technical debt
- Coordination: who is working on what

The format can be detailed and technical. Use direct language, reference tickets and PRs, and include enough technical context for peers to understand.

```text
This week I completed the API key management service:
- POST /v1/api-keys is implemented and reviewed (PR #421)
- Key hashing uses bcrypt with cost factor 12
- Still need to implement the key rotation endpoint (TICKET-5678)
- Blocked on: database migration script (waiting on DBA team for schema review)
```

**Engineering managers** care about:

- Progress against plan: are we on track for the milestone?
- Risks and blockers: what could derail the project?
- Team health: is anyone overloaded or stuck?
- Resource needs: do we need more people, tools, or time?

Managers need a balance of technical and project-level information. Do not overload them with implementation details, but provide enough context for them to understand risks.

```text
Status: On track (green)
- API key service: 80% complete, on schedule for milestone 2
- Key rotation endpoint will start next week
- Risk: DBA team is backlogged; migration review may take longer than expected
- No changes to timeline expected
```

**Executives and directors** care about:

- Business impact: how does this project affect revenue, customers, or strategy?
- Timeline: when will it ship?
- Major risks: what could prevent shipping?
- Resource asks: do we need budget or headcount?

Executives need a one-paragraph summary. Use business language, not technical jargon. Lead with impact, not implementation.

```text
The API key management project is on track for the July release. It will
enable our external partners to integrate with the platform securely.
One risk: the database team is short-staffed, which could delay the
migration. We are evaluating whether to hire a contractor for this sprint.
```

**External stakeholders** (clients, partners) care about:

- What they receive and when
- How it affects their integration or usage
- What action they need to take
- Contact information for questions

External updates must be polished. Avoid internal jargon, ticket numbers, and technical deep-dives.

```text
We are on track to deliver the API key management feature by July 15. This
feature will allow your team to generate unique API keys for each of your
services. We will send integration documentation by July 1.
```

### Writing Effective Status Reports

A good status report is structured, factual, and actionable.

**The three-part structure:**

1. **Summary** — one sentence on overall status (green / yellow / red)
2. **Progress** — what was accomplished since the last update
3. **Next steps** — what will be done before the next update

```text
Status: GREEN — On track for milestone delivery

Progress this week:
- Completed the API key creation endpoint (PR #421 merged)
- Implemented key hashing with bcrypt
- Started the key rotation endpoint (TICKET-5678)

Next week:
- Complete key rotation endpoint
- Write integration tests
- Begin database migration script

Blockers:
- None currently
```

**Use the traffic light system.** Every update should have a clear status indicator.

- **Green:** On track. No major issues.
- **Yellow:** At risk. There are issues that could delay the timeline. Mitigation in progress.
- **Red:** Off track. The timeline will slip without intervention. Needs decision or resources.

**Be honest about red status.** Do not sugar-coat bad news. A clear red status with a recovery plan is better than a yellow status that hides the problem.

**Update stakeholders proactively.** Do not wait for the weekly status meeting to announce a problem. Send a brief update as soon as you know.

### Choosing the Right Level of Detail

The level of detail depends on the audience, the frequency of updates, and the project phase.

**Daily updates (standups):** Bullet points. What I did, what I will do, blockers. One to three sentences per item.

```text
- Completed the API key creation endpoint
- Will start the key rotation endpoint
- No blockers
```

**Weekly updates:** A few paragraphs or a bullet list. Progress against the weekly plan, metrics, risks, and next week's plan.

**Monthly or milestone updates:** A few paragraphs or a one-page document. Summary of progress against the milestone, key achievements, metrics, risks, and updated timeline.

**Quarterly or annual updates:** A presentation or full report. Strategic context, achievements, metrics, lessons learned, and plans for the next period.

**The inverted pyramid:** Put the most important information first. Each subsequent paragraph adds detail. The reader can stop reading at any point and still get the key message.

```text
[Most important] The API key project is on track for July release.
[Important details] Database migration review is the critical path.
[Additional details] The DBA team is backlogged. We are discussing options.
[Reference] Here is the full project timeline for reference.
```

### KPIs and Metrics in Updates

Metrics make status reports objective. Instead of saying "good progress," show the data.

**Engineering metrics for status reports:**

```text
- Velocity: 25 story points completed (target: 30)
- Sprint progress: 7 of 10 tickets closed
- Cycle time: 3.5 days average for PRs (target: 2 days)
- Test coverage: 85% (target: 80%)
- Bug count: 2 open bugs (baseline: 5)
- Deployment frequency: 3 deploys per week (target: 5)
```

**Project metrics:**

```text
- Milestone progress: 65% complete (target for this week: 60%)
- Budget burn: $45,000 of $100,000 budget used
- Timeline variance: 0 days behind schedule
- Risk count: 3 open risks (1 high, 2 medium)
```

**Business metrics (for execs):**

```text
- Feature adoption: 30% of users have used the new feature (target: 50%)
- Performance improvement: p99 latency reduced from 500ms to 200ms
- Revenue impact: $0 direct revenue; enables partner integrations worth $2M ARR
- Customer satisfaction: support tickets related to this area dropped by 40%
```

**Metrics without context are misleading.** Always compare against a baseline or target.

```text
// Without context
Test coverage: 85%

// With context
Test coverage: 85% (target: 80%, previous: 72%). Up 13 points since the
testing initiative started. Remaining gaps are in error-handling code paths.
```

### Async Updates vs. Meetings

Not every update needs a meeting. Async updates save time and create a written record.

**When to use async updates:**

- Weekly status report to the team or manager
- Progress update on a well-understood project
- Information that does not need discussion
- Updates to a large audience

**When to use a meeting instead:**

- The update triggers a decision that needs discussion
- The project is in a red state and needs team alignment
- The audience has conflicting priorities that need resolution
- The update is sensitive (layoffs, reorgs, performance issues)

**The async-first approach:** Write the update first. Send it async. If it triggers discussion, schedule a meeting. This eliminates the status meeting that could have been an email.

**Live dashboards as async updates:** If your team maintains a live dashboard with project metrics, you can reduce status report overhead. Just include a link to the dashboard and a brief commentary on notable changes.

```text
Status: Green

See the project dashboard for live metrics:
[link to dashboard]

Notable this week:
- Sprint completion rate improved to 85% (was 70%)
- Two new risks added, both medium severity
- One blocker resolved (DBA team approved the schema)

No meeting needed unless you have questions.
```

### Handling Bad News

Delivering bad news is a skill. Done poorly, it erodes trust. Done well, it builds credibility.

**Principles for delivering bad news:**

**Be timely.** The moment you know there is a problem, communicate it. Waiting does not make it better.

**Be direct.** Do not bury bad news in a paragraph of good news. State the problem clearly in the first sentence.

```text
// Burying the bad news
We made good progress this week. The frontend is complete and the integration
tests are passing. However, the database migration is delayed by two weeks
because of a schema conflict we discovered.

// Direct
The database migration is delayed by two weeks because of a schema conflict.
This will push the project timeline from July 15 to July 29. Here is the
revised plan.
```

**Provide context.** Explain why it happened. Did you miss something in the planning phase? Was it an external dependency? Honest context helps stakeholders trust your future estimates.

**Present a recovery plan.** Bad news is easier to hear when paired with a solution. What are you doing to fix it? What is the revised timeline?

```text
We discovered a schema conflict between the new API key table and the
existing user profiles table. The migration script needs redesign.

Recovery plan:
1. Schema redesign: 3 days (in progress)
2. DBA re-review: 2 days
3. Migration execution: 1 day

New timeline: July 29 (two-week delay)
Original timeline: July 15

I will provide daily updates until the migration is unblocked.
```

**Escalate appropriately.** If the delay affects a public commitment or executive priority, make sure the right people hear it from you before they hear it from someone else.

**Accept responsibility.** If the delay was caused by an oversight in your team, own it. Do not blame other teams publicly (handle cross-team issues offline).

### Requesting Decisions

Status updates often need to ask for decisions. A clear decision request increases the chance of getting a timely answer.

**A good decision request includes:**

- **The decision needed:** what exactly needs to be decided?
- **The context:** why does this decision need to be made now?
- **The options:** what are the choices, with pros and cons?
- **Your recommendation:** what do you think the team should do?
- **The deadline:** by when does the decision need to be made?

```text
Decision needed: Should we implement API key rotation as a separate endpoint
or as a parameter on the create endpoint?

Context: We need to finalize the API design this week to meet the milestone.
The frontend team needs the API contract to start their work.

Options:
A. Separate endpoint (POST /v1/api-keys/:id/rotate) — cleaner API, follows
REST conventions, but requires an additional API call.
B. Optional parameter on create (POST /v1/api-keys with ?rotate=true) —
simpler for clients, but mixes creation and rotation semantics.

Recommendation: Option A. REST conventions are well-understood and the
additional API call is negligible.

Deadline: Friday, 14:00 UTC. The frontend team starts integration on Monday.
```

**Decision requests in async channels** should be a separate message, not buried in a status report. Use a clear label.

```text
[DECISION REQUEST] API key rotation endpoint design
Deadline: Friday, 14:00 UTC
```

**Follow up on pending decisions.** If a decision deadline passes without a response, follow up individually with the decision maker.

### Weekly vs. Milestone Updates

Different cadences serve different purposes.

**Weekly updates:**

- Audience: team and immediate manager
- Format: email, Slack, or shared document
- Content: what was done, what will be done, blockers
- Length: 2-5 bullet points
- Tone: informal, direct

**Milestone updates:**

- Audience: manager, stakeholders, execs
- Format: shared document or presentation
- Content: summary of progress against milestone goals, key achievements, metrics, updated timeline, risks
- Length: one page or 5-10 slides
- Tone: formal, business-oriented

**Combining the two:** Send weekly updates to your team and manager. At milestone boundaries, compile the weekly updates into a milestone report for broader distribution.

```text
Milestone 1 Complete: API Key Management

What was delivered:
- API key creation, listing, revocation, and rotation endpoints
- Integration with auth middleware
- Database schema for key storage
- Migration script for existing clients

Metrics:
- 100% of planned features delivered
- 4,200 lines of code
- 95% test coverage
- 0 critical bugs found in staging

Timeline:
- Planned: 4 weeks
- Actual: 4 weeks, 2 days (2-day delay due to schema review)
- Overall project: on track for July 15 release

Risks for Milestone 2:
- Integration with partner onboarding tool may require API changes
- Documentation is not yet started
```

### Email vs. Dashboard Formats

Choose the format based on how the audience will consume the update.

**Email updates:**

- Good for: external stakeholders, executives, formal communication
- Include: summary, key metrics, risks, next steps
- Length: 2-3 paragraphs
- Best practice: use bullet points and bold for key information
- Include a call to action if you need a response

```text
Subject: Project Status: API Key Management — Week 4

Status: GREEN

Progress this week:
- API key rotation endpoint completed (ahead of schedule)
- Integration tests passing
- Documentation started

Risks:
- None currently

Next week:
- Begin partner onboarding integration
- Complete documentation

Questions or concerns? Reply to this email or catch me on Slack.
```

**Dashboard updates:**

- Good for: internal teams, ongoing projects, frequent updates
- Format: shared document, wiki page, or project management tool
- Include: live metrics, links to PRs, links to design docs
- Update frequency: continuously or daily
- Best practice: keep the dashboard updated in real-time, use the email to point to the dashboard

**Choosing between them:**

- If the audience needs to make a decision, use email with a clear ask
- If the audience just needs awareness, use a dashboard with a periodic summary email
- If the audience is external, always use email
- If the audience is internal and distributed, use async dashboards with Slack summaries

## Study Cases

### Case 1: Weekly Email That Prevented a Missed Deadline

A team was building a new authentication service. The tech lead noticed that the database migration was taking longer than expected. Instead of waiting for the weekly status meeting, they sent a brief email to the engineering manager and project manager:

```text
Subject: [Heads up] Auth service migration — possible delay

The database schema review has identified a compatibility issue with our
existing user data. We are working on a solution with the DBA team.

Current estimate: 2 days of additional work
Impact on timeline: likely 2-day slip on milestone 1
I will update you when we have a confirmed resolution timeline.

No action needed from you yet. Just wanted you to be aware.
```

The project manager was able to adjust the milestone timeline before anyone else's work was affected. The team was seen as proactive, not reactive.

### Case 2: Bad News Delivered Well

A feature was two weeks behind schedule because of an underestimation in the planning phase. The engineering lead sent this update:

```text
Subject: [Status update] Search feature — revised timeline

Status: RED — Off track by 2 weeks

What happened:
During implementation, we discovered that the search index needs to support
faceted filtering, which was not in the original design. Supporting faceted
filtering requires changes to both the indexing pipeline and the query API.

We should have caught this during the design review. I take responsibility
for the oversight.

Revised plan:
1. Implement faceted indexing (5 days)
2. Update query API to support filters (3 days)
3. Update frontend search UI (2 days)
4. Buffer for edge cases (2 days)

Original delivery: June 15
Revised delivery: June 29

Options to reduce impact:
- We could ship basic search on June 15 and add faceted filters in a
  follow-up. The basic search covers 80% of use cases. The PM team has
  confirmed this is acceptable.

I recommend shipping basic search on June 15 and adding facets in the next
sprint. This meets the committed deadline and avoids rushing the facet
implementation.

Please confirm by Wednesday if you prefer the phased approach.
```

The stakeholders appreciated the honesty, the clear recovery plan, and the option to reduce impact. They chose the phased approach.

### Case 3: Dashboard Replaced Status Meetings

A team was spending two hours per week in status update meetings. The meetings were 30 minutes, but 25 minutes was people reading their updates aloud while everyone else waited for their turn.

The team moved to an async dashboard with a shared document. Each person updated their section by 10 AM Friday. The manager read the updates and only scheduled a meeting if there were blockers to discuss.

The result: status meetings dropped from weekly to bi-weekly, and then to ad-hoc (only when needed). The team saved approximately 80 person-hours per quarter.

## Examples

### Example 1: Weekly Update to Engineering Manager

```text
Subject: Weekly update — Auth service, June 8-12

Status: Yellow

Completed:
- OAuth integration (PR #456 merged)
- Token refresh endpoint
- Unit tests for auth middleware (90% coverage)

In progress:
- Rate limiting middleware (TICKET-7890) — 50% complete, on schedule
- Integration tests for OAuth flows

Risks:
- The rate limiting library has a known issue with distributed Redis
  backends. I am evaluating alternatives. Decision needed this week
  (see separate DECISION REQUEST message).

Next week:
- Complete rate limiting middleware
- Start performance testing
- Write deployment runbook
```

### Example 2: Executive Update

```text
Subject: Partner API project — milestone 1 complete

The partner API project has completed milestone 1 on schedule. The API key
management system is now ready for internal testing.

Key achievements:
- Partners can generate and rotate API keys independently
- Keys are hashed with industry-standard encryption
- Dashboard for monitoring key usage

Next milestone starts Monday and focuses on partner onboarding tooling.

The project is on track for the July 15 delivery date.

Questions? Reply or set up time to discuss.
```

### Example 3: Decision Request in Status Update

```text
Subject: Decision needed — PostgreSQL 16 upgrade timing

We need to decide when to upgrade the auth database from PostgreSQL 12 to
PostgreSQL 16 (version 12 reaches end-of-life in September).

Options:
A. Upgrade in June (ahead of the partner project launch)
   Pros: Clean separation from the partner launch timeline
   Cons: Requires team focus during an active project

B. Upgrade in August (after partner launch)
   Pros: Team can focus fully on the partner project
   Cons: Two weeks of buffer before EOL

C. Upgrade in September (just before EOL)
   Pros: Maximizes development time
   Cons: Risk of EOL-related security patches not being available

Recommendation: Option A (June). The upgrade is well-understood (we have
done it in staging), and doing it early avoids pressure later.

Deadline: May 25. The DBA team needs two weeks lead time to schedule.
```

## Glossary

| Term | Definition |
|------|------------|
| Async update | A status report sent without requiring real-time discussion |
| Blocker | An issue that prevents progress and needs resolution |
| Green/Yellow/Red | Traffic light system for indicating project health |
| Inverted pyramid | A writing structure that presents the most important information first |
| Milestone | A significant checkpoint in a project schedule |
| Stakeholder | Anyone with an interest in the project's outcome |
| Status report | A periodic summary of project progress, risks, and next steps |
| Traffic light system | A color-coded system for indicating project status |
| Velocity | The amount of work a team completes in a sprint |
| Risk | A potential issue that could negatively affect the project |

## Quick References

- [Atlassian: How to Write a Status Report](https://www.atlassian.com/work-management/project-management/status-report) — guide from Atlassian
- [Google re:Work: Effective Team Communication](https://rework.withgoogle.com/guides/communication/) — research-based communication practices
- [Status Report Templates (Smartsheet)](https://www.smartsheet.com/status-report-templates) — templates for different audiences
- [Amazon's Six-Pager Memo](https://www.inc.com/amazon-jeff-bezos-six-page-memo-replaces-powerpoint.html) — alternative to slide-based updates
- [How to Write a Status Report That Actually Gets Read](https://blog.trello.com/status-report-template) — practical tips for status reports

## Next Steps

- [Meeting Notes & Action Items](meeting-notes.md) — capturing decisions made during status discussions
- [RFCs & Technical Decision Documents](rfcs.md) — writing structured proposals for major decisions
