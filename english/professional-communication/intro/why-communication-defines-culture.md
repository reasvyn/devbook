# Why Communication Defines Engineering Culture

## Description

Engineering culture is communication culture. The way a team writes, reviews, and discusses determines how decisions are made, how knowledge is shared, and how reliable the systems they build become. This document explains why written communication is the foundation of effective engineering organizations.

## Prerequisites

- [Why English Matters for Developers](../../intro/why-english-matters.md)
- [Why Grammar Matters for Developers](../../grammar-and-style/intro/why-grammar-matters.md)

## Table of Contents

- [Communication as Infrastructure](#communication-as-infrastructure)
- [The Four Pillars of Engineering Communication](#the-four-pillars-of-engineering-communication)
- [Async-First Culture](#async-first-culture)
- [Written Communication Forms in Engineering](#written-communication-forms-in-engineering)
- [Code Review as Communication](#code-review-as-communication)
- [Incident Communication](#incident-communication)
- [Technical Decision Records](#technical-decision-records)
- [Status Updates & Stakeholder Communication](#status-updates--stakeholder-communication)
- [Meeting Culture & Written Preparation](#meeting-culture--written-preparation)
- [The Communication Maturity Model](#the-communication-maturity-model)
- [Remote & Distributed Communication](#remote--distributed-communication)
- [Communication Anti-Patterns](#communication-anti-patterns)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Communication as Infrastructure

In software engineering, communication is not a soft skill — it is infrastructure. Like code, tests, or deployment pipelines, communication systems determine how effectively an organization operates.

**Communication failure modes:**

| Failure mode | Symptom | Cost |
|---|---|---|
| Information silos | Teams don't know what other teams are doing | Duplicated work, conflicting designs |
| Knowledge hoarding | Key information exists only in one person's head | Bus factor, blocking questions |
| Decision opacity | Nobody knows why decisions were made | Repeated arguments, inconsistent direction |
| Context switching | Engineers constantly interrupted for "quick questions" | 40%+ productivity loss |
| Meeting overload | Every decision requires a synchronous meeting | Slowed progress, schedule fragmentation |

**Communication as leverage:**

Good communication multiplies individual effectiveness:

- One well-written RFC can align ten engineers for a month
- One clear incident postmortem can prevent the same outage across the entire org
- One good README can save hundreds of support questions
- One well-documented API can eliminate dozens of integration calls

### The Four Pillars of Engineering Communication

**1. Precision**

Technical communication must be precise. Vague language causes bugs.

| Vague | Precise |
|---|---|
| "The system slows down sometimes." | "Query latency increases from 50ms to 2000ms between 2-3 PM daily." |
| "We should fix this soon." | "This bug blocks the release planned for next Tuesday. Fix is due Friday." |
| "The API is slow." | "The /search endpoint returns responses in 800-1200ms at p95." |

**2. Transparency**

Engineering communication should default to public. Public discussions:
- Include people who might have relevant context
- Create a searchable record for future reference
- Level the information playing field across the organization
- Reduce the bus factor

**3. Timeliness**

The right information at the wrong time is useless. Communication should happen:
- Before decisions are made (not after)
- When context is fresh (not weeks later)
- When the audience can act on it

**4. Psychological safety**

Engineers must feel safe to communicate problems, mistakes, and disagreements. Without safety, communication degrades to:
- Withholding bad news until it's critical
- Avoiding technical disagreements
- Hiding mistakes instead of learning from them
- Saying "yes" when they mean "no"

### Async-First Culture

Async-first means written communication is the default. Synchronous communication (meetings, calls) is reserved for cases where real-time interaction adds value.

**Why async-first:**

- **Respects deep work.** Engineers need uninterrupted blocks for focused work. Synchronous communication fragments these blocks.
- **Scales to distributed teams.** Time zones make synchronous communication impossible for global teams.
- **Creates a record.** Written communication is searchable, referencable, and reviewable.
- **Enables thoughtful responses.** Written communication allows time for reflection and research.

**The async communication stack:**

| Layer | Tool | Purpose |
|---|---|---|
| Documentation | Confluence, Notion, docs site | Permanent reference |
| RFCs | Google Docs, Markdown, PRs | Proposals and decisions |
| Code review | GitHub/GitLab PRs | Code discussion |
| Async updates | Email, Slack threads | Status updates |
| Chat | Slack, Discord | Quick questions |
| Sync | Video calls, in-person | Brainstorming, conflict resolution |

**Rules for async communication:**

1. Write complete messages. "Can we talk?" is not an async message.
2. Include context. Assume the reader hasn't thought about this topic recently.
3. Use descriptive subject lines. "Question" is useless; "Question about the payment flow retry logic" is useful.
4. Set expectations for response time. "No rush" vs "This blocks the release."
5. Summarize decisions. After a synchronous discussion, write a summary and post it async.

### Written Communication Forms in Engineering

**Design documents / RFCs:**

Before starting significant work, write a proposal:

```
Title: [Short descriptive title]
Status: [Draft | In Review | Accepted | Rejected | Superseded]
Author: [Name]
Date: [Date]

## Problem
What problem are we solving? Why is it important?

## Proposed Solution
What are we going to do? Include diagrams, API sketches, trade-offs.

## Alternatives Considered
What else did we evaluate? Why not those?

## Open Questions
What is not yet decided? What needs more research?

## Timeline
When would this ship? What are the milestones?
```

**Postmortems:**

After an incident, write a postmortem:

```
Title: [Date] [Severity] [Service] — [Brief Title]
Date: [Date]
Author: [Name]
Severity: SEV1 / SEV2 / SEV3

## Summary
What happened? What was the impact?

## Timeline
When did each event occur? (UTC)

## Root Cause
Why did it happen?

## Resolution
How was it fixed?

## Action Items
What will prevent recurrence?
```

**Status updates:**

Keep stakeholders informed:

```
Project: [Project name]
Date: [Date]
Status: [On Track | At Risk | Blocked]

## Completed This Week
- [List of accomplishments]

## Planned for Next Week
- [List of planned work]

## Blockers
- [Anything that needs help from the reader]

## Risks
- [Potential problems on the horizon]
```

### Code Review as Communication

Code review is one of the most frequent written communication activities for developers. Treat it as such.

**Writing good review comments:**

Bad: "This is wrong." (Why? What's the alternative?)

Good: "This approach uses O(n^2) time. For our data set (100k+ items), that could cause latency issues. Consider using a hash map for O(1) lookups instead."

**The code review comment framework:**

1. **Observation:** What you noticed
2. **Rationale:** Why it matters
3. **Suggestion:** What they could do instead

**Review etiquette:**

- Assume good intent. The author made the best decision they could with the information they had.
- Be specific. General feedback ("this could be better") is not actionable.
- Separate nitpicks from blockers. Mark style comments explicitly as "nit:".
- Explain the "why" behind the "what". If you suggest a change, explain your reasoning.
- Respond promptly (within a few hours during work hours).

**PR descriptions as communication:**

A good PR description tells the reviewer:
1. What the change does
2. Why the change is needed
3. How the change was tested
4. What decisions were made during implementation

### Incident Communication

During incidents, communication must be:
- Fast (no time for editing)
- Precise (ambiguity costs time)
- Consistent (multiple people may need to communicate)
- Tracked (for postmortem analysis)

**The incident communication template:**

```
INCIDENT #[ID]
Service: [affected service]
Severity: [SEV1/SEV2/SEV3]
Status: [Detecting | Investigating | Mitigating | Resolved]

Impact: [What is affected and how]
    e.g. "All payment transactions failing. Users cannot complete purchases."

Time: [Current time UTC]

Update: [What is happening now]
    e.g. "Engineering identified the issue: database connection pool exhausted.
    Scaling up connections. ETA 10 minutes."

Next update: [Time]
```

**Rules for incident communication:**

1. Communicate proactively. If you haven't sent an update in 30 minutes, send a "still working" update.
2. Be honest about unknowns. "We don't know the root cause yet" is better than silence.
3. Don't speculate. If you don't know when it will be fixed, say "No ETA yet."
4. One source of truth. Use a single channel (status page, Slack thread) for updates.

### Technical Decision Records

Technical Decision Records (TDRs) capture architectural and design decisions. They are lightweight, version-controlled, and written alongside the code.

**The TDR format:**

```
# [Title]

## Context
What forces are at play? What constraints exist?

## Decision
What did we decide? Use imperative: "We will use PostgreSQL."

## Consequences
What trade-offs does this decision create?

## Status
[Proposed | Accepted | Deprecated | Superseded]
```

**Why TDRs matter:**

- Prevent repeated debates about the same decision
- Provide context for future developers (who may wonder "why did they do this?")
- Create a searchable history of the project's technical evolution
- Enable systematic review of past decisions

### Status Updates & Stakeholder Communication

Writing for stakeholders (managers, executives, cross-team collaborators) requires adapting your communication:

**The stakeholder update structure:**

1. **The headline:** What is the most important thing they need to know?
2. **Progress:** What did we accomplish since the last update?
3. **Plan:** What will we accomplish before the next update?
4. **Risks:** What could derail the plan?
5. **Help needed:** What do we need from them?

**Common mistakes:**

- Too much detail. Stakeholders don't need to know every commit; they need to know progress toward goals.
- Too little context. Don't assume they remember every detail from the last update.
- Hiding problems. Bad news does not get better with time. Communicate risks early.

### Meeting Culture & Written Preparation

The best meetings start with good writing.

**The written meeting agenda:**

Before the meeting, share a written document with:
1. Meeting goal (one sentence)
2. Pre-reading (what everyone should review before the meeting)
3. Agenda items with time allocations
4. Decisions that need to be made

**The decision document:**

For meetings that make decisions, write a decision document beforehand:

```
## Decision to be made: [Single, clear question]

## Context
- Relevant background information

## Options Considered
- Option A: [pros/cons]
- Option B: [pros/cons]

## Recommendation
- What the document author thinks and why
```

**Meeting notes as communication:**

After a meeting, send written notes within 24 hours:
1. What was decided
2. What action items were assigned (who does what by when)
3. What was deferred

### The Communication Maturity Model

Organizations progress through stages of communication maturity:

| Level | Name | Characteristics |
|---|---|---|
| 1 | Ad hoc | Verbal decisions, no records, frequent misunderstandings |
| 2 | Reactive | Written communication after incidents, some documentation |
| 3 | Proactive | RFCs for decisions, postmortems after incidents, documentation maintained |
| 4 | Systematic | Async-first culture, templates for all communication, metrics tracked |
| 5 | Learning | Communication patterns continuously improved, blameless culture, knowledge management |

**How to advance:**

- Level 1 → 2: Start writing decisions down. Adopt a RFC template.
- Level 2 → 3: Make documentation part of the definition of done. Require postmortems.
- Level 3 → 4: Shift to async-first. Invest in documentation tooling.
- Level 4 → 5: Measure communication effectiveness. Iterate on patterns.

### Remote & Distributed Communication

Remote work amplifies communication challenges. Without physical proximity, intentional communication practices become critical.

**Remote communication best practices:**

- Over-communicate by default. What seems obvious to you may not be obvious to your remote teammate.
- Record everything. Meetings, demos, design sessions — record for time zones.
- Write decisions down. A hallway conversation in an office resolves a question; a Slack DM does not.
- Use status indicators. "Focusing" / "Available" / "In meetings" to set expectations.
- Default to public channels. DM only for sensitive or personal topics.

**The remote communication stack:**

| Need | Tool |
|---|---|
| Async discussion | Team chat (Slack, Discord) |
| Long-form writing | Docs (Notion, Google Docs, Markdown) |
| Code discussion | PR comments |
| Sync discussion | Video calls |
| Recorded sync | Loom, recorded meetings |

### Communication Anti-Patterns

**1. The deer in headlights:**

Engineer receives a question via DM, feels pressure, responds "Let me look into it" — and then forgets.

Fix: Respond with a concrete promise: "I'll check and get back to you by 3 PM."

**2. The drive-by comment:**

"This doesn't look right." — no context, no suggestion, no follow-up.

Fix: "This doesn't look right because [reason]. Consider [suggestion]."

**3. The decision by exhaustion:**

After hours of meeting debate, the group agrees to a solution just to end the meeting. The decision was never properly evaluated.

Fix: End the meeting with "Let's write up the options and decide async by [date]."

**4. The reply-all update:**

Every status update goes to 50 people. Everyone ignores them. The one person who needed to know misses the update.

Fix: Distribute status updates to the minimum necessary audience. Use distribution lists for broad announcements.

**5. The missing context:**

"I changed the database schema." — Why? What changed? What needs to migrate?

Fix: "I changed the database schema to support the new reporting feature. The users table now has a 'role' column. Run migrations/042_add_role.sql."

## Study Cases

### Case 1: From Meeting-Culture to Async-First

A startup of 50 engineers ran on meetings. Every decision required a meeting. Engineers spent 30+ hours per week in meetings.

**The change:**
1. Mandated RFCs for any decision affecting multiple teams
2. Required written agendas for any meeting (no agenda = cancel)
3. Introduced async standups via Slack
4. Established "No Meeting Wednesday" for deep work

**Results:**
- Meeting time dropped from 30+ hours to 8 hours per week
- Engineers reported higher satisfaction and productivity
- Decisions were better documented and more easily questioned
- Onboarding time reduced because decisions had written records

### Case 2: Incident Communication Failure

A SEV1 outage at a fintech company was poorly communicated:

- First update: 45 minutes after the incident started (engineering was trying to fix, not communicate)
- Updates were in multiple channels (some said "investigating," some said "resolved")
- Different team members gave conflicting information to stakeholders

**The fix:**
1. Created a dedicated #incidents channel with an automated template
2. Assigned a communications lead for every SEV1/SEV2 (separate from engineering lead)
3. Mandated updates every 30 minutes even if "still investigating"
4. Standardized status: Investigating → Mitigating → Monitoring → Resolved

**Results:**
- Stakeholder trust improved significantly
- Postmortem creation became easier with structured timelines
- Communication load on engineers decreased

### Case 3: The Silent Team

A remote team had excellent engineers but very little written communication. Knowledge was shared verbally in video calls. New hires struggled for months.

**Diagnosis:**
- No onboarding documentation
- Decisions discussed on video calls but not written down
- Code had minimal comments
- PR reviews were superficial ("LGTM")

**The intervention:**
1. Sprint started with a written planning document
2. Every decision required a written summary posted in Slack
3. PR review guidelines required at least one substantive comment
4. Onboarding documented as a living guide in a shared doc

**Results:**
- New hire ramp time reduced from 4 months to 6 weeks
- Fewer repeated questions in Slack
- Team felt more aligned even across time zones

### Case 4: The Postmortem That Changed Everything

After a major database corruption incident, a team wrote a thorough postmortem. The root cause was a subtle race condition in a migration script.

**The postmortem revealed:**
- The migration had been reviewed but the race condition wasn't noticed
- The test environment didn't match production load patterns
- No rollback plan existed for the migration

**Actions taken:**
- Migration review checklist created
- Production-scale load testing added to CI
- All migrations now require a rollback plan
- Postmortem shared with the entire engineering org

The written postmortem prevented similar incidents across 5 other teams that read it and checked their own migration processes.

## Examples

### Example 1: Bad vs. good RFI

Bad:
```
Can we use Redis for caching? I think it would be faster.
```

Good:
```
RFC: Use Redis for API response caching

## Problem
The /users endpoint currently queries the database on every request.
With 500+ requests/second during peak, this creates unnecessary
database load and increases p95 latency to 800ms.

## Proposal
Add Redis as a caching layer:
- Cache responses for 60 seconds (TTL)
- Invalidate cache on user update
- Use Redis cluster for HA

## Tradeoffs
+ Reduces database load by ~80%
+ Reduces p95 latency to 50ms
- Adds Redis as an operational dependency
- Adds ~$200/month in infrastructure cost

## Recommended
Proceed with implementation. Cost is acceptable given the latency
improvement.
```

### Example 2: Bad vs. good incident update

Bad:
```
Things are still broken. Working on it.
```

Good:
```
INCIDENT UPDATE: Database replication lag

Service: user-api
Status: Investigating
Impact: User profiles may show stale data (1-5 minute delay)

Current state: We identified increased replication lag on
the read replica. Primary database is healthy.

Next step: Engineering is investigating the replication
mechanism. Possible cause: large batch job running on primary.

Next update: 14:30 UTC (in 15 minutes)
```

### Example 3: Bad vs. good code review comment

Bad:
```
This is wrong.
```

Good:
```
This query will perform poorly as the users table grows. The WHERE
clause uses `LIKE '%search%'` which cannot use the index, so every
query does a full table scan. Consider using PostgreSQL full-text
search or a dedicated search service.

If full-text search is too complex for now, we could at least use
`LIKE 'search%'` (without leading wildcard) to at least use a b-tree
index on the column.
```

### Example 4: Bad vs. good meeting agenda

Bad:
```
Subject: Database meeting
Agenda: Discuss database stuff
```

Good:
```
Subject: Database migration decision — Tuesday 2 PM

Goal: Decide between blue-green migration and rolling update

Pre-read:
- Comparison doc: link
- Previous migration postmortem: link

Agenda (30 min):
1. Review options (5 min) — read pre-read beforehand
2. Q&A (10 min)
3. Decision (10 min)
4. Assign owner and timeline (5 min)

Output: Decision document with migration plan
```

## Glossary

| Term | Definition |
|---|---|
| Async-first | Defaulting to written, non-real-time communication |
| Bus factor | The number of team members who must be hit by a bus before the project fails |
| Incident | An event that causes disruption to normal service |
| Postmortem | Written analysis of an incident to learn and prevent recurrence |
| RFC | Request for Comments — a structured proposal document |
| SEV1/SEV2/SEV3 | Severity levels for incidents (critical, major, minor) |
| Silos | Groups that do not share information across organizational boundaries |
| Stakeholder | Someone affected by or interested in a project's outcome |
| TDR | Technical Decision Record — documented architectural decision |
| Transparency | Making information visible and accessible to those who need it |

## Quick References

- [Atlassian: Incident Management](https://www.atlassian.com/incident-management) — incident response best practices
- [Google SRE Books](https://sre.google/sre-book/) — postmortem culture and incident communication
- [RFC 7322: RFC Style Guide](https://www.rfc-editor.org/rfc/rfc7322) — formal RFC writing standards
- [How to Write a Postmortem](https://www.pagerduty.com/resources/learn/incident-response/postmortem/) — PagerDuty guide
- [Making an Async-First Culture Work](https://doist.com/blog/async-remote/) — Doist's approach to async communication
- [Stack Overflow: Code Review Etiquette](https://stackoverflow.blog/2021/10/19/code-review-etiquette/) — writing good review comments

## Next Steps

- [Writing Effective Code Review Comments](../code-reviews.md) — mastering PR communication
- [Incident Postmortems & Incident Reports](../incident-reports.md) — structured incident writing
