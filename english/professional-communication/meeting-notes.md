# Meeting Notes & Action Items

## Description

Meeting notes capture decisions, action items, and important discussions so that participants and absentees can stay aligned. This document covers how to take effective notes before, during, and after meetings, with templates for common engineering meeting types.

## Prerequisites

- [Conciseness](../grammar-and-style/conciseness.md) — taking brief but complete meeting notes

## Table of Contents

- [Why Meeting Notes Matter](#why-meeting-notes-matter)
- [Before the Meeting: Preparation](#before-the-meeting-preparation)
- [During the Meeting: Real-Time Note Taking](#during-the-meeting-real-time-note-taking)
- [Capturing Decisions vs. Discussions](#capturing-decisions-vs-discussions)
- [Action Items with Owners and Deadlines](#action-items-with-owners-and-deadlines)
- [Distributing and Following Up](#distributing-and-following-up)
- [Templates for Different Meeting Types](#templates-for-different-meeting-types)
- [Tools for Collaborative Notes](#tools-for-collaborative-notes)
- [Common Note-Taking Pitfalls](#common-note-taking-pitfalls)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Why Meeting Notes Matter

Meeting notes serve several critical functions in an engineering organization.

**Memory aid:** The human brain forgets within hours. Notes preserve decisions, context, and details that participants will not remember next week.

**Absentee inclusion:** Not everyone can attend every meeting. Notes keep the whole team informed.

**Decision record:** When someone asks "why did we choose this approach?" six months later, the meeting notes provide the answer.

**Accountability:** Action items with named owners and deadlines create accountability. Without notes, action items are promises that disappear into the ether.

**Meeting efficiency:** Knowing that someone is taking notes encourages the group to stay focused and reach conclusions.

**Legal and compliance:** In regulated industries, meeting notes may be required for audits or compliance.

### Before the Meeting: Preparation

Good meeting notes start before the meeting begins.

**Review the agenda.** If the meeting has an agenda, understand the topics and the goals. What decisions need to be made? What information needs to be shared?

**Set up the document.** Create the notes document in advance. Use a template that matches the meeting type. Share the document with participants before the meeting.

**Prepare the action item tracker.** Create a table with columns for action, owner, deadline, and status. This saves time during the meeting.

**Check past action items.** Review action items from the previous meeting. Did everyone complete their tasks? If not, those items may need to be carried forward.

**Choose a note-taking tool.** Decide whether to use a shared document (Google Docs, Notion, wiki) or a dedicated tool. The key requirement is that notes are accessible to all participants and searchable later.

**Assign a note taker.** For recurring meetings, rotate the note-taker role. For ad-hoc meetings, the organizer takes notes by default.

### During the Meeting: Real-Time Note Taking

Taking notes during a live meeting is a skill that improves with practice.

**Write down decisions immediately.** When the group makes a decision, write it down right away. Confirm the wording with the group.

```text
Decision: We will migrate the auth database to PostgreSQL 16 during the
June maintenance window (June 15, 14:00-16:00 UTC). Rollback plan: restore
from backup. Confirmed by: Alice, Bob, Charlie.
```

**Capture action items as they are assigned.** Do not wait until the end of the meeting. Assign the owner and deadline on the spot.

```text
Action: Alice will draft the database migration script.
Owner: Alice
Deadline: June 10
```

**Note disagreements and open questions.** If the group does not reach a decision, document the disagreement and the plan for resolution.

```text
Open question: Should we use blue-green deployment or rolling update for the
migration? Alice and Bob have different opinions. Resolution plan: Alice will
write a one-page comparison by June 8 for discussion at the next meeting.
```

**Use shorthand and expand later.** Speed is important during the meeting. Use abbreviations and short phrases. Expand unclear items immediately after the meeting.

```text
// During meeting
DB mig delay -> schema conflict -> 2wk slip
Alice: new timeline by Fri

// After meeting (expanded)
The database migration is delayed by two weeks due to a schema conflict
between the new API key table and the existing user profiles table.
Alice will provide a revised timeline by Friday.
```

**Do not transcribe everything.** Capture decisions, action items, and key discussion points. Do not try to write down every sentence. If the discussion is important enough to transcribe, record the meeting with consent.

**Keep the meeting on track.** If the discussion goes off-topic, note the tangent and suggest parking it.

```text
[Parking lot] Discussion about CI pipeline performance. We will schedule a
separate meeting for this topic.
```

**Ask for clarification when needed.** If you are unsure what was decided, ask. "Let me confirm what I wrote: the decision is to use Option A, correct?"

### Capturing Decisions vs. Discussions

The most common mistake in meeting notes is recording the discussion instead of the decision.

**Discussion** is the conversation — the arguments, the back-and-forth, the exploration of alternatives. It is transient.

**Decision** is the outcome — what the group agreed to do. It is permanent.

**What to capture:**

- Decisions (required)
- Action items (required)
- Key discussion points that explain why a decision was made (useful)
- Alternatives that were explicitly rejected and why (useful)
- The full conversation transcript (rarely useful)

**A good decision record includes:**

- What was decided
- Who made the decision
- What alternatives were considered
- Why this option was chosen
- Any conditions or caveats

```text
Decision: Adopt Option A (blue-green deployment) for the database migration.

Decided by: Group consensus (Alice, Bob, Charlie)

Alternatives considered:
- Option B (rolling update): rejected because schema changes are not backward
  compatible and rolling update requires backward compatibility
- Option C (offline migration): rejected because the service cannot have
  extended downtime

Why Option A: Blue-green deployment allows us to test the new schema before
switching traffic. It requires double the infrastructure for the migration
window, but the SRE team confirmed the capacity is available.

Conditions: The old environment must be kept for 48 hours after migration
before cleanup.
```

**The 5-minute rule for decisions:** If the group discusses a topic for 5 minutes without reaching a decision, the facilitator should either assign a decision deadline or escalate to a decision maker. Note this in the meeting notes.

### Action Items with Owners and Deadlines

Action items are the most important output of any meeting. Without them, the meeting produced no tangible outcome.

**A good action item has:**

- A specific action (what needs to be done)
- An owner (who will do it)
- A deadline (by when)
- A clear definition of done (how to know it is complete)

```text
// Vague action item
Improve test coverage.

// Specific action item
Owner: Alice
Action: Write unit tests for the auth middleware error paths
Deadline: June 10
Definition of done: All error paths in authMiddleware.go have tests
with >90% coverage.
```

**Action item states:**

- Open: not yet started
- In progress: actively being worked on
- Blocked: cannot proceed because of a dependency
- Done: completed and verified
- Cancelled: no longer needed

**Tracking action items across meetings:**

- Start each meeting with a review of open action items
- Carry forward incomplete items with a note on why they were not done
- Close items only when the action is verified, not just assigned

**Follow-up cadence:**

- Daily standup: quick check on action items
- Weekly team meeting: review open action items
- Monthly: audit overdue action items

**Escalating overdue items:**

- If an action item is past due, the owner should communicate why
- If the owner cannot complete the item, reassign it
- If the item is no longer relevant, cancel it

### Distributing and Following Up

Notes are only useful if they are read.

**Distribute notes promptly.** Send meeting notes within 2 hours of the meeting ending. Fresh notes are more accurate and more likely to be read.

**Send to all participants and relevant absentees.** If someone was invited but could not attend, they are the most important audience for the notes.

**Use a consistent format.** Subject line prefix like "Meeting Notes:" makes notes easy to find in email archives.

```text
Subject: Meeting Notes: Sprint Planning (June 5, 2026)
```

**Summarize at the top.** Write a one-paragraph summary of the key decisions and action items. Busy readers can read the summary and skip the details.

```text
Summary:
- Decided: adopt blue-green deployment for database migration
- Action: Alice will draft migration script by June 10
- Action: Bob will schedule migration window with SRE by June 8
- No decisions made on CI pipeline improvements (deferred to next meeting)
```

**Link to relevant documents.** If the meeting discussed a design doc, RFC, or PR, include the link in the notes.

**Archive notes in a searchable location.** Use a shared drive, wiki, or project management tool where notes can be found later.

**Follow up on action items.** Send a reminder 2 days before the deadline. If items are overdue, escalate.

### Templates for Different Meeting Types

**Template 1: Daily Standup**

```markdown
# Standup Notes — YYYY-MM-DD

## Participants
[List of attendees]

## What everyone worked on

### Alice
- Yesterday: Completed API key creation endpoint
- Today: Start key rotation endpoint
- Blockers: None

### Bob
- Yesterday: Reviewed PR #421
- Today: Begin database migration script
- Blockers: Waiting for DBA team schema review

## Action Items
| Action | Owner | Deadline | Status |
|--------|-------|----------|--------|
| Follow up on DBA schema review | Bob | EOD | In progress |
```

**Template 2: Sprint Planning**

```markdown
# Sprint Planning — Sprint 12 (June 5-18)

## Participants
[List of attendees]

## Sprint Goal
[One sentence describing the sprint goal]

## Committed Tickets
| Ticket | Description | Owner | Points |
|--------|-------------|-------|--------|
| TICKET-1234 | API key rotation endpoint | Alice | 5 |
| TICKET-1235 | Database migration script | Bob | 8 |
| TICKET-1236 | Integration tests | Charlie | 3 |

## Capacity
- Total team capacity: 40 points
- Committed: 36 points
- Buffer: 4 points for bugs and unplanned work

## Action Items
| Action | Owner | Deadline |
|--------|-------|----------|
| Schedule migration window with SRE | Bob | June 7 |
| Update sprint board with tickets | Alice | Immediately |
```

**Template 3: Retrospective**

```markdown
# Sprint Retrospective — Sprint 11 (May 22 - June 4)

## Participants
[List of attendees]

## What Went Well
- PR review turnaround improved from 2 days to 12 hours
- Fewer critical bugs in production (1 vs. 4 in previous sprint)
- Team communication improved with daily standups

## What Could Be Improved
- Sprint planning estimates were off by 20% on average
- Integration tests were added late in the sprint
- Documentation was not updated for three tickets

## Action Items
| Action | Owner | Deadline |
|--------|-------|----------|
| Add estimation refinement session before planning | Alice | Next sprint |
| Write integration tests before feature implementation | Team | Starting next sprint |
| Include documentation in definition of done | Bob | Immediately |
```

**Template 4: One-on-One**

```markdown
# 1:1 — Alice (Manager) & Bob (Engineer) — June 5, 2026

## Previous Action Items
| Action | Owner | Status |
|--------|-------|--------|
| Complete database migration training | Bob | Done |
| Set up career development plan | Alice | In progress |

## Topics Discussed
- Career goals: Bob wants to move toward systems architecture
- Current project: Bob is enjoying the database migration work
- Concerns: Bob feels the sprint load is too high (8 points vs. 5 average)
- Feedback: Alice noted Bob's design reviews are thorough and helpful

## Action Items
| Action | Owner | Deadline |
|--------|-------|----------|
| Find a systems architecture mentor for Bob | Alice | June 12 |
| Reduce Bob's sprint load to 5 points next sprint | Alice | Next planning |
| Bob to shadow the SRE team's architecture review | Bob | June 15 |
```

**Template 5: Design Review**

```markdown
# Design Review — API Key Management Service

Date: June 5, 2026
Author: Alice
Reviewers: Bob, Charlie, Diana

## Design Overview
[Brief description of the design]

## Feedback Summary
| Reviewer | Feedback | Status |
|----------|----------|--------|
| Bob | Consider using bcrypt instead of SHA-256 for key hashing | Accepted |
| Charlie | The migration plan should include a rollback step | Accepted |
| Diana | Rate limiting should be per-key, not per-service | Deferred for discussion |

## Decisions
- Key hashing: bcrypt with cost factor 12
- Migration rollback: added to the migration plan
- Rate limiting: to be discussed in a follow-up meeting

## Action Items
| Action | Owner | Deadline |
|--------|-------|----------|
| Update design document with bcrypt decision | Alice | June 6 |
| Schedule follow-up for rate limiting discussion | Alice | June 8 |
| Approve final design after changes | Bob | June 10 |
```

### Tools for Collaborative Notes

**Google Docs:** Most common for collaborative note-taking. Supports real-time editing, comments, and action item suggestions.

Best for: ad-hoc meetings, design reviews, team-wide notes.

**Notion:** Structured documents with databases for tracking action items. Good for recurring meetings with consistent templates.

Best for: sprint planning, retros, 1:1s.

**Confluence / wiki:** Persistent storage for decisions and historical records. Less suitable for real-time collaborative editing.

Best for: design decisions, architectural records, meeting archives.

**Slack canvas:** Lightweight document within Slack. Good for quick notes but limited formatting and search capabilities.

Best for: standup notes, quick sync meetings.

**Dedicated meeting notes tools:** Fellow, Coda, Hugo, and others offer templates and action item tracking.

**Choosing a tool:**

- If the whole company uses one tool, use that tool
- If notes need to be found years later, use a searchable wiki
- If real-time collaboration is critical, use Google Docs
- If action item tracking is the priority, use Notion or a dedicated tool

### Common Note-Taking Pitfalls

**Writing too much:** Transcribing the entire conversation makes notes hard to read. Focus on decisions, action items, and key points.

**Writing too little:** A single line saying "discussed project status" is useless. What was the status? What was decided?

**Not capturing the decision:** The group discusses a topic for 15 minutes but the notes say "discussed." Did they decide anything? What was the outcome?

**Vague action items:** "Alice will look into the database issue." What does "look into" mean? By when? How will we know it is done?

**Delayed distribution:** Notes sent three days after the meeting are stale. People have already started working based on their (possibly incorrect) memory.

**Not following up:** Notes with action items that are never reviewed. Open action items should be reviewed at the start of each meeting.

**No owner for action items:** An action item without an owner is not an action item. It is a wish.

**Inconsistent format:** Every meeting uses a different structure. Participants cannot find information because they do not know where to look.

**No context for absentees:** Notes that assume the reader was in the meeting. Missing contextual information for anyone who was not present.

**Over-polishing:** Spending 30 minutes formatting notes after a 30-minute meeting. Notes should be functional, not beautiful.

## Study Cases

### Case 1: From Useless Notes to Valuable Records

A team had a weekly sync meeting. The notes were always one line: "Discussed project status." No decisions, no action items. The next week, the same topics came up again because nothing was resolved.

The team adopted the template approach. They created a shared document with sections for decisions, action items, and open questions. The facilitator brought the document to the meeting and filled it out in real-time.

After three weeks, the team noticed:
- Decisions were actually followed through
- Action items were completed before the next meeting
- Absentees could catch up without the team repeating everything

The notes for the same meeting now looked like this:

```text
Weekly Sync — June 5, 2026

Decisions:
- Database migration: use blue-green deployment (agreed)
- Sprint goal: complete API key service (agreed)

Action Items:
- Alice: draft migration script (June 10)
- Bob: schedule SRE window (June 8)

Open Questions:
- CI pipeline performance: deferred to next meeting (June 12)
```

### Case 2: Action Item Tracking Across Sprints

A team had action items from retros that were forgotten by the next sprint. The retro notes were long documents that no one read after the meeting.

The team created a living action item tracker in their project management tool. Every retro, they reviewed the tracker, closed completed items, and added new ones.

Key changes:
- Action items had owners, deadlines, and ticket numbers
- Each retro started with 5 minutes to review the tracker
- Overdue items were escalated to the engineering manager
- Items that were stale for two sprints were closed or reprioritized

After three months, the team's retro action item completion rate went from 30% to 85%.

### Case 3: Collaborative Notes for a Distributed Team

A distributed team had team members in four time zones. The standup meeting was held in Slack, but responses were inconsistent and the daily update was not archived.

The team created a shared Google Doc with a template. Each person filled out their update before the daily standup window. The document was structured with sections for each person and a section for action items.

The result:
- Updates were available for the whole team regardless of time zone
- The document served as an archive that could be searched
- The standup window was reduced from 30 minutes to 15 minutes because people read the doc first and only discussed blockers

## Examples

### Example 1: Standup Notes

```markdown
# Standup — June 10, 2026

## Alice
- Yesterday: Migrated auth database schema in staging. Found a permission issue with the migration script.
- Today: Fix the permission issue and run migration in staging again.
- Blockers: None.

## Bob
- Yesterday: Reviewed Alice's migration script. Approved pending permission fix.
- Today: Start writing the deployment runbook.
- Blockers: None.

## Charlie (OOO)
- Out until June 14. No updates.

## Action Items
None today.
```

### Example 2: Retro Notes

```markdown
# Sprint 12 Retro — June 18, 2026

Participants: Alice, Bob, Charlie, Diana (facilitator)

## What Went Well
- Completed 90% of committed points (up from 75% last sprint)
- Only one production bug (down from four)
- Code review turnaround averaged 6 hours (target: 24 hours)

## What Could Be Improved
- Integration tests were still added late in the sprint
- Documentation updates were skipped for three tickets
- Sprint planning overestimated capacity (40 points committed, 36 completed)

## Action Items
| Action | Owner | Deadline | Ticket |
|--------|-------|----------|--------|
| Make integration tests a prereq for feature PRs | Team | Next sprint | TICKET-1300 |
| Update definition of done to include doc updates | Alice | June 20 | TICKET-1301 |
| Plan 36 points instead of 40 for Sprint 13 | Bob | Next planning | TICKET-1302 |
```

### Example 3: Design Review Notes

```markdown
# Design Review — API Key Rotation

Date: June 12, 2026
Author: Alice
Reviewers: Bob, Charlie

## Discussion
- Bob raised a concern about key rotation causing race conditions if two
  clients rotate simultaneously. Alice proposed using a version field.
- Charlie asked about key recovery if a rotation fails mid-way. Alice will
  add a recovery procedure.

## Decisions
- Use version field to handle concurrent rotation (accepted)
- Key recovery procedure will be added to the design (accepted)
- Use bcrypt cost factor 12 (confirmed)

## Action Items
| Action | Owner | Deadline |
|--------|-------|----------|
| Add version field to key schema | Alice | June 14 |
| Add key recovery procedure section | Alice | June 14 |
| Approve final design | Bob, Charlie | June 16 |
```

## Glossary

| Term | Definition |
|------|------------|
| Action item | A task with an owner and deadline that results from a meeting |
| Agenda | A list of topics to be discussed in a meeting |
| Decision record | A written note of what was decided and why |
| Definition of done | Criteria that must be met for a task to be considered complete |
| Facilitator | The person responsible for keeping the meeting focused and productive |
| Note taker | The person responsible for capturing decisions and action items |
| Parking lot | A place to capture off-topic discussions for later resolution |
| Retrospective | A meeting to reflect on a completed sprint and identify improvements |
| Standup | A brief daily meeting to coordinate team work |
| Transcribe | To write down every word spoken during a meeting |

## Quick References

- [How to Take Meeting Notes (Harvard Business Review)](https://hbr.org/2018/11/how-to-take-better-meeting-notes) — research-backed note-taking advice
- [Meeting Notes Templates (Atlassian)](https://www.atlassian.com/team-playbook/plays/meeting-notes) — templates for various meeting types
- [How to Run Effective Meetings (Basecamp)](https://basecamp.com/guides/how-we-run-meetings) — meeting fundamentals
- [The Art of the Retro (Agile Alliance)](https://www.agilealliance.org/glossary/retrospective/) — guide to running effective retrospectives
- [Google Docs Meeting Notes Template](https://docs.google.com/document/u/0/) — collaborative note-taking setup

## Next Steps

- [Stakeholder Updates & Status Reports](stakeholder-updates.md) — communicating meeting outcomes to broader audiences
- [RFCs & Technical Decision Documents](rfcs.md) — documenting design decisions in a structured format
