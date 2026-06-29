# Incident Postmortems & Incident Reports

## Description

Incident reports document what went wrong, why it happened, and what the team will do to prevent it from happening again. This document covers how to write clear, blameless postmortems that drive real improvements in system reliability and team processes.

## Prerequisites

- [Sentence Structure](../grammar-and-style/sentence-structure.md) — writing clear, factual sentences under pressure

## Table of Contents

- [What Is an Incident Report?](#what-is-an-incident-report)
- [The Postmortem Culture: Blameless vs. Blameful](#the-postmortem-culture-blameless-vs-blameful)
- [Structure of a Good Postmortem](#structure-of-a-good-postmortem)
- [Writing Clearly Under Pressure](#writing-clearly-under-pressure)
- [Distinguishing Facts from Assumptions](#distinguishing-facts-from-assumptions)
- [Action Items and Follow-Up Tracking](#action-items-and-follow-up-tracking)
- [SRE Best Practices for Incident Reporting](#sre-best-practices-for-incident-reporting)
- [Examples of Good and Bad Postmortems](#examples-of-good-and-bad-postmortems)
- [Incident Report Templates](#incident-report-templates)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### What Is an Incident Report?

An incident report is a written record of a service disruption, security breach, or operational anomaly. It documents the timeline, root cause, impact, and corrective actions. Unlike a bug report, which focuses on a specific software defect, an incident report captures the full operational context.

**Purpose of incident reports:**

- **Learning:** Understand what happened and why, so the team can prevent recurrence
- **Accountability:** Track how incidents are resolved and what improvements follow
- **Communication:** Inform stakeholders about the incident and its impact
- **Compliance:** Meet regulatory requirements for documenting security or availability issues
- **Process improvement:** Identify gaps in monitoring, alerting, runbooks, and team processes

**When to write an incident report:**

- Service outage exceeding the team's SLA threshold
- Security breach or data exposure
- Performance degradation affecting multiple users
- Data loss or corruption
- Failed deployment that required rollback
- Any incident that triggered the incident response process

### The Postmortem Culture: Blameless vs. Blameful

The culture around incident reporting determines whether teams learn from failures or hide them.

**Blameless culture:** Assumes that people did their best with the information and tools available. Errors are caused by systemic factors, not individual negligence.

```text
// Blameless framing
The engineer who deployed the change was following the standard deployment
process. The process did not include a canary step, so the bad change reached
all users simultaneously.
```

**Blameful culture:** Assumes that incidents are caused by individual mistakes. Punishes the person who made the error.

```text
// Blameful framing
Engineer X deployed without testing. Engineer Y approved the PR without
reviewing it properly. They caused the outage.
```

**Why blameless is better:**

- People report incidents honestly if they know they will not be punished
- Root causes are found in processes, tools, and culture, not in individuals
- Firing or punishing the person who caused an incident does not fix the system
- Blame culture encourages hiding mistakes, which makes incidents harder to prevent

**Blameless does not mean consequence-free.** If someone acted maliciously or recklessly, there are separate processes for that. Postmortems are for systemic learning.

### Structure of a Good Postmortem

A complete postmortem has several sections. Adapt the structure to your team's needs, but keep the core elements.

**Executive summary:** A one-paragraph overview for busy stakeholders.

```text
On June 15, 2026, from 14:32 to 15:17 UTC, the payments API was unavailable
due to a database connection pool exhaustion. The root cause was a missing
connection release in the refund processing pipeline. Approximately 3,400
requests failed. The issue was resolved by restarting the service and
deploying a fix. Action items include adding connection pool monitoring and
implementing a circuit breaker.
```

**Timeline:** A minute-by-minute account of what happened. Include timestamps, actions, and who took them.

```text
14:32 — Alert triggered: payments API error rate > 5%
14:33 — Engineer A on-call acknowledges the alert
14:35 — Engineer A checks Grafana; sees connection pool at 100%
14:38 — Engineer A investigates recent deployments; identifies PR #421 merged at 14:00
14:42 — Engineer A rolls back PR #421
14:45 — Deploy complete; connection pool still at 100%
14:48 — Engineer B joins the call; suggests restarting the service
14:52 — Service restart initiated
14:55 — Connection pool recovers; error rate drops to 0%
15:00 — Health checks pass
15:17 — Monitoring confirms full recovery; incident declared resolved
```

**Impact:** Quantify the damage. Include metrics and business impact.

```text
- Duration: 45 minutes
- Failed requests: 3,400
- Affected users: ~12,000 (payments timed out but were retried)
- Revenue impact: $0 (failed transactions were retried successfully after recovery)
- SLA breach: Yes (99.9% availability target missed for the quarter)
```

**Root cause analysis:** Dig into the underlying causes, not just the proximate trigger. Use the Five Whys technique.

```text
Why did the connection pool exhaust?
  Because the refund handler did not release connections to the database.

Why did the refund handler not release connections?
  Because the error path in the try-catch block did not call connection.close().

Why did the error path not close the connection?
  Because the developer was not aware that the error path needed manual cleanup.

Why was the developer not aware?
  Because the codebase uses a custom database wrapper and the connection lifecycle
  was not documented.

Why was the connection lifecycle not documented?
  Because the team did not have a documentation standard for internal libraries.
```

**Detection.** How was the incident discovered? Was it detected by monitoring, reported by a user, or found during routine checks?

**Response.** Who responded and how? What worked well in the response process? What could be improved?

```
Response strengths:
- On-call engineer acknowledged the alert within 1 minute
- Rollback was initiated within 10 minutes of the alert

Response weaknesses:
- The monitoring dashboard did not show connection pool usage by default
- The runbook for database connection issues was outdated
```

**Action items:** Concrete tasks with owners and deadlines.

```text
| Action | Owner | Deadline | Type |
|--------|-------|----------|------|
| Add connection.close() to all error paths in the refund handler | Engineer A | June 18 | Fix |
| Add Grafana alert for connection pool usage > 80% | Engineer B | June 20 | Monitoring |
| Document connection lifecycle in the database wrapper README | Engineer A | June 25 | Documentation |
| Update the runbook for database connection pool issues | Engineer C | June 25 | Process |
| Conduct a team training session on connection management | Engineer B | July 1 | Training |
```

### Writing Clearly Under Pressure

During an incident, engineers write under extreme stress. The goal is to produce clear, accurate communication despite the pressure.

**Tips for writing during an incident:**

**Use present tense for active status.** "The API is returning 503 errors" is clearer than "The API was returning 503 errors."

**Separate facts from observations.** "The error rate is 5%" is a fact. "The database seems overloaded" is an observation. Label them clearly.

**Include timestamps.** Every status update should include a UTC timestamp. This makes the timeline reconstructable later.

```text
14:35 UTC — Error rate is 5% and increasing. Investigating.
14:38 UTC — Identified the culprit deployment (PR #421 at 14:00). Initiating rollback.
14:42 UTC — Rollback in progress. Estimated completion in 3 minutes.
```

**Avoid speculation.** If you do not know the root cause, say so. Guesses in incident chat channels become "facts" in the postmortem if not corrected.

```text
// Speculation presented as fact
The database is overloaded. We need more replicas.

// Honest assessment
The error logs suggest database connection failures. I am investigating whether
this is a connection pool issue or a database-side problem.
```

**Use the discipline of "status updates."** Every 15 minutes, post a brief update to the incident channel. Even if nothing has changed, say so.

```text
15:00 UTC — Still investigating. No new findings. Next update at 15:15.
```

### Distinguishing Facts from Assumptions

The postmortem must distinguish between what is known and what is inferred. Mixing them leads to incorrect root cause analysis and wasted effort.

**Facts** are verifiable and timestamped:

```text
- The alert fired at 14:32 UTC
- CPU usage reached 95% at 14:35 UTC
- The rollback completed at 14:45 UTC
- Error rate returned to baseline at 15:00 UTC
```

**Assumptions** are hypotheses that need verification:

```text
- Assumption: The connection pool exhausted because of the missing close()
  (verified: code review confirmed the missing call)
- Assumption: No other processes were competing for database connections
  (verified: no concurrent deploys were running)
```

**Best practices:**

- Label assumptions clearly in the postmortem draft
- Verify each assumption before finalizing the document
- If an assumption cannot be verified, note it as unconfirmed
- Do not base action items on unconfirmed assumptions

**Common sources of misattribution:**

- Survivorship bias: The survivors of the incident (the parts that did not fail) are not analyzed
- Confirmation bias: The team sees evidence that supports their favorite theory
- Hindsight bias: Everything seems obvious in retrospect
- Attribution bias: The team blames individuals for systemic issues

### Action Items and Follow-Up Tracking

An incident report without action items is just a story. Every postmortem must produce concrete tasks that reduce the likelihood or impact of similar incidents.

**Good action items are SMART:**

- **Specific:** "Add a Grafana alert for connection pool usage" not "Improve monitoring"
- **Measurable:** "Error rate alert fires within 1 minute of crossing 5% threshold"
- **Assignable:** One owner per action item
- **Realistic:** Doable within the team's capacity
- **Time-bound:** Has a clear deadline

**Action item categories:**

| Category | Example |
|----------|---------|
| Fix | Add missing connection.close() call |
| Monitoring | Add alert for connection pool usage |
| Process | Update deployment checklist to include canary step |
| Documentation | Document the connection lifecycle |
| Training | Team training on database connection patterns |
| Test | Add integration test for error path connection handling |

**Tracking:**

- Add action items to the team's project management tool
- Link each action item to the postmortem document
- Review open action items in weekly team meetings
- Close an action item only when the change is deployed and verified

### SRE Best Practices for Incident Reporting

Site Reliability Engineering brings specific practices to incident reporting.

**Error budgets:** Use incident reports to track how much of the error budget was consumed. If the team's SLO is 99.9% uptime, a 45-minute outage consumes a significant portion of the quarterly budget.

**Blameless postmortems:** This is a core SRE practice. Postmortems focus on systemic causes, not individual errors.

**Postmortem frequency:** SRE teams write postmortems for all incidents that consume error budget, not just major outages.

**Incident classification:** Use severity levels consistently.

```text
SEV1: Complete service outage affecting all users
SEV2: Partial outage or degradation affecting a subset of users
SEV3: Minor issue with workaround available
SEV4: Non-urgent issue found during routine operations
```

**Metrics-driven postmortems:** Every postmortem should reference monitoring data, not just anecdotal evidence.

**Postmortem review:** Schedule a meeting to review the postmortem with the team. Do not just write it and file it away.

### Examples of Good and Bad Postmortems

**Bad postmortem:**

```text
Postmortem: Payment Service Outage

What happened: The payments API went down.

Why: Someone forgot to close a database connection.

Who's at fault: John from the payments team. He wrote the refund handler
without proper cleanup.

Action items:
- John should test his code properly next time
- More code review

Lessons learned: Be more careful.
```

**Why this is bad:** It blames an individual, offers vague action items, provides no timelines or metrics, and does not address systemic causes.

**Good postmortem:**

```text
Postmortem: Payments API Outage — June 15, 2026

Executive summary: [one paragraph as shown above]

Timeline:
  14:32 — Alert: payments API error rate > 5%
  14:33 — Engineer A on-call acknowledges
  ...

Root cause: Missing connection.close() in the refund handler's error path,
causing database connection pool exhaustion.

Contributing factors:
  1. The database wrapper's connection lifecycle was undocumented
  2. The error path was not covered by tests
  3. Connection pool monitoring was not configured
  4. The deployment did not include a canary phase

Impact:
  - Duration: 45 minutes
  - Failed requests: 3,400
  - SLA breach: Yes
  - Revenue impact: $0 (retried successfully)

Action items:
  [table of action items with owners and deadlines]

Lessons learned:
  1. The custom database wrapper needs documentation on connection lifecycle
  2. Monitoring should cover connection pool health
  3. Error paths are as important as happy paths in testing
  4. Canary deployments would have caught this before full rollout
```

### Incident Report Templates

**Template 1: Full Postmortem**

```markdown
# Postmortem: [Incident Title]

Date: YYYY-MM-DD
Severity: SEV[1-4]
Duration: [start] - [end] UTC

## Executive Summary

[One paragraph overview]

## Timeline

| Time (UTC) | Event |
|------------|-------|
| HH:MM | Event description |

## Impact

- Duration:
- Failed requests:
- Affected users:
- Revenue impact:
- SLA breach:

## Root Cause

[Analysis using Five Whys or similar technique]

## Contributing Factors

1. Factor one
2. Factor two

## Detection

How was this incident discovered?

## Response

What worked well? What could be improved?

## Action Items

| Action | Owner | Deadline | Type |
|--------|-------|----------|------|

## Lessons Learned

1. Lesson one
2. Lesson two
```

**Template 2: Incident Summary (for stakeholders)**

```markdown
## Incident Summary

Date: YYYY-MM-DD
Duration: XX minutes
Service: [Service name]
Impact: [Brief description of user impact]

### What Happened

[2-3 paragraph summary]

### Root Cause

[One paragraph]

### Current Status

[Resolved / Monitoring / In progress]

### Next Steps

- [Action item 1]
- [Action item 2]
```

## Study Cases

### Case 1: Writing a Postmortem for a Deployment Failure

A team deployed a configuration change that accidentally increased the log verbosity to DEBUG across all services, filling the disk on all application servers.

The timeline documented when the deploy started, when the first disk-full alert fired, when the team identified the cause (log volume spike correlated with the deploy time), when the rollback started, and when disks recovered.

The root cause was that the configuration validation pipeline did not check log level settings. The fix added a validation rule that rejects changes to log levels without explicit approval.

Action items included adding the validation rule, creating a Grafana dashboard for disk usage per service, and updating the deployment checklist to require a canary environment.

### Case 2: Distinguishing Facts from Assumptions After a Security Incident

A security incident involved an exposed AWS S3 bucket containing customer data.

Fact: The bucket policy allowed public read access. Fact: The bucket was discovered by a security scanning service. Fact: The bucket contained 50,000 customer records.

Assumption: The bucket was left open by Engineer X during a migration. (Unverified: no evidence tying Engineer X to the bucket configuration change.)

Assumption: The data was accessed by unauthorized parties. (Partially verified: the scanning service's logs showed read requests, but it is unclear if anyone else accessed the data.)

The postmortem clearly labeled which findings were facts and which were assumptions. Action items focused on preventing future exposure, not blaming individuals.

### Case 3: A Postmortem That Drove Organizational Change

A database migration caused 30 minutes of read-only mode on a critical service. The postmortem revealed that the migration tool did not support online schema changes.

Instead of a quick fix, the team used the postmortem to drive a six-month initiative to adopt online migration tooling across the organization. Each quarterly review checked progress.

The action item was not just "fix the migration script" but "evaluate and adopt zero-downtime migration tools for all production databases."

## Examples

### Example 1: Good Timeline Entry

```text
14:42 UTC — PR #421 identified as the culprit. The deploy at 14:00 introduced
a refund handler with a missing connection.close() in the error path. Rollback
initiated. ETA: 3 minutes.
```

### Example 2: Good Action Item

```text
| Action | Owner | Deadline | Type |
|--------|-------|----------|------|
| Add a runbook entry for "Connection Pool Exhaustion" with steps to check pool usage, identify the culprit connection, and restart the service if needed | Engineer B | June 20 | Documentation |
```

### Example 3: Good Impact Description

```text
Impact:
- Total duration: 45 minutes (14:32 - 15:17 UTC)
- Requests failed due to timeout: 3,412 (12.4% of total requests in that window)
- Requests retried successfully: 3,401 (11 requests failed permanently due to client-side timeout)
- Users affected: approximately 12,000 (those with in-flight payment requests)
- Revenue impact: $0 — all successful retries captured the intended payments
- SLO consumption: consumed 12% of the quarterly error budget for payments API
```

## Glossary

| Term | Definition |
|------|------------|
| Blameless postmortem | A postmortem that focuses on systemic causes rather than individual errors |
| Error budget | The maximum acceptable downtime or failure rate within an SLO |
| Five Whys | A root cause analysis technique that asks "why" five times to drill into underlying causes |
| Incident | An unplanned event that causes service disruption or degradation |
| Postmortem | A written analysis of an incident, its causes, and corrective actions |
| Root cause | The underlying condition that led to the incident |
| Runbook | A documented set of procedures for operating and troubleshooting a system |
| SEV | Severity level of an incident (SEV1-4) |
| SLO | Service Level Objective — a target for service reliability |
| Timeline | A chronological record of events during an incident |

## Quick References

- [Google SRE: Postmortem Culture](https://sre.google/sre-book/postmortem-culture/) — SRE book chapter on blameless postmortems
- [Atlassian: How to Write a Great Postmortem](https://www.atlassian.com/incident-management/postmortem) — practical guide from Atlassian
- [PagerDuty: Incident Response Guide](https://response.pagerduty.com/) — comprehensive incident management guide
- [Etsy's Debriefing Facilitation Guide](https://extfiles.etsy.com/DebriefingFacilitationGuide.pdf) — guide for facilitating postmortem meetings
- [Microsoft: How to Write a Postmortem](https://docs.microsoft.com/en-us/azure/architecture/guide/resiliency/postmortem)

## Next Steps

- [Writing Effective Code Review Comments](code-reviews.md) — applying structured writing to technical feedback
- [Stakeholder Updates & Status Reports](stakeholder-updates.md) — communicating incident impact to non-technical audiences
