# Email & Slack Communication for Engineers

## Description

Engineers communicate across many channels: email, chat, video calls, and face-to-face conversations. Choosing the right channel and crafting clear messages saves time, reduces misunderstandings, and builds better working relationships. This document covers effective async communication practices for engineering teams.

## Prerequisites

- [International Audiences](../grammar-and-style/international-audiences.md) — writing for readers with different language backgrounds and cultural norms

## Table of Contents

- [Choosing the Right Channel](#choosing-the-right-channel)
- [Writing Clear Async Messages](#writing-clear-async-messages)
- [Threading and Context](#threading-and-context)
- [Mentions and Notifications](#mentions-and-notifications)
- [Writing Questions That Get Answers](#writing-questions-that-get-answers)
- [Time Zones and Working Hours](#time-zones-and-working-hours)
- [Tone in Text](#tone-in-text)
- [Avoiding Miscommunication](#avoiding-miscommunication)
- [Email Etiquette](#email-etiquette)
- [Slack Etiquette](#slack-etiquette)
- [Channel-Specific Patterns](#channel-specific-patterns)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Choosing the Right Channel

Every communication channel has trade-offs. The best channel depends on urgency, complexity, audience size, and the need for persistence.

**Email** is best for:

- Formal communication with external parties
- Detailed instructions or documentation that recipients need to reference later
- Communication that requires a paper trail
- Broadcasting information to a large or unknown audience
- Messages that cross time zones with no expectation of immediate reply

Example: Sending a proposal to a client, documenting a policy change, announcing a release to the organization.

**Slack or chat** is best for:

- Quick questions and answers
- Informal team coordination
- Sharing links and quick updates
- Time-sensitive but not critical notifications
- Building team culture and social connections

Example: Asking a teammate about a function signature, sharing a dashboard link, coordinating a lunch order.

**Face-to-face or video call** is best for:

- Complex discussions with many trade-offs
- Sensitive feedback or difficult conversations
- Brainstorming and creative problem solving
- Building rapport with new team members
- Resolving disagreements after async discussion failed

Example: Architecture decision with strong opinions, performance review feedback, incident retrospective.

**Written documentation** (wiki, RFCs, runbooks) is best for:

- Information that multiple people will need to reference
- Decisions that need a permanent record
- Processes that must be followed exactly
- Knowledge that outlasts any individual team member

**The 5-minute rule:** If a Slack thread has been going back and forth for more than 5 minutes without resolution, switch to a video call. Async back-and-forth is inefficient for complex topics.

### Writing Clear Async Messages

Async messages are read hours or days after they are written. The author is not available to clarify. Therefore, every message must be self-contained and unambiguous.

**Lead with the request.** Put the most important information first. Busy readers scan the first line to decide whether to read the rest.

```text
// Weak lead
Hey, I was working on the user service and noticed something about the way
we handle timeouts. I've been looking at the configuration and I think maybe
we should change it. What do you think?

// Strong lead
[Request] Can we change the user service timeout from 30s to 10s? Here's why:
```

**Use descriptive subject lines.** In email, the subject line is the first thing the recipient sees. In Slack, use the first sentence as the subject.

```text
// Vague subject
Quick question

// Descriptive subject
Question about the user service timeout configuration
```

**One topic per message.** Mixing multiple topics in a single message guarantees that at least one topic will be overlooked.

```text
// Mixed topics
Can you review the PR for the payment fix? Also, the deployment script has
a bug, and we need to update the README for the new API.

// Separate messages
Topic 1: Payment fix PR #421 is ready for review.
Topic 2: Found a bug in the deployment script — details in thread.
Topic 3: The README for the new API needs updating.
```

**Provide context.** Do not assume the recipient knows what you are referring to. Link to relevant tickets, PRs, or documents.

```text
// No context
Can you look at this when you get a chance?

// With context
Can you review PR #421? It fixes the payment timeout issue described in
TICKET-1234. The fix uses a retry mechanism that I'd like a second opinion on.
```

**Use formatting for clarity.** Bold for key terms. Bullet lists for multiple items. Code blocks for commands or code.

```text
Steps to reproduce:
1. Deploy the `fix/timeout` branch to staging
2. Send a payment request with an invalid card number
3. Observe: the request times out after 30s instead of returning a validation error

Expected: validation error within 2s
Actual: timeout after 30s
```

### Threading and Context

Threading keeps conversations organized and makes them referenceable later.

**When to start a new thread:**

- The topic is different from the current conversation
- You want to ask a question without disrupting an ongoing discussion
- You need to share a resource or link
- You want to continue a conversation started in a different channel

**When to reply in thread:**

- You are answering a question asked in a channel
- Your reply is specific to one message and does not apply to the whole channel
- You are adding context to a previous message
- The conversation is starting to branch into subtopics

**Cross-channel context:** When discussing a topic that spans multiple channels, include a link to the related conversation.

```text
Continuing the discussion from #deploy-pipeline (link), I ran the test again
with the updated configuration.
```

**Context window:** Assume the recipient has not read the last 50 messages. Summarize the relevant background in your message.

```text
// No context (assumes recipient has been following the thread)
I think we should go with option B.

// With context
[Context] We were discussing database migration strategies. Option A uses
blue-green deployment. Option B uses rolling update. I think Option B is
better because our schema changes are backward compatible.
```

### Mentions and Notifications

Mentions are powerful. Use them deliberately, not carelessly.

**When to @mention someone:**

- You need their specific input or approval
- You are assigning a task to them
- You are responding to a question they asked
- They need to take action

**When not to @mention:**

- You are sharing information that they may find useful (use a normal message)
- You are posting in a channel they already monitor
- You want to get attention but the message is not urgent

**Channel-wide mentions:**

- @channel: notifies everyone in the channel. Use only for urgent announcements.
- @here: notifies only online members. Use sparingly.
- @everyone: notifies everyone in the workspace. Use only for critical organization-wide messages.

**Notification etiquette:**

- Do not @mention someone and then immediately follow up with "did you see my message?"
- If someone does not respond within a reasonable time, send a polite follow-up, not a repeat @mention
- Respect do-not-disturb hours and status indicators
- If you are mentioned in multiple threads, batch your responses instead of replying instantly to each one

### Writing Questions That Get Answers

The quality of the answer you receive is directly proportional to the quality of the question you ask.

**A good question includes:**

- **What you are trying to do:** the goal, not just the specific blocker
- **What you have tried:** shows you have done your homework
- **What happened:** actual error messages, logs, or screenshots
- **What you expected:** so the reader can identify the gap

```text
// Bad question
Anyone know why the build is failing?

// Good question
The CI build for PR #421 is failing on the "integration-tests" step with:
  Error: Connection refused on localhost:5432

I've confirmed that PostgreSQL is running locally, and I can connect via
psql. The test config points to `localhost:5432`. Any ideas what might
cause this?

Things I've tried:
- Restarting PostgreSQL
- Checking the test config file
- Running the tests without the Docker network

Environment: macOS, PostgreSQL 16, Node 20
```

**Ask in the right channel.** A database question goes in #database or #backend, not #general. A question in the right channel gets answered faster.

**Be patient.** Not everyone is online at the same time. Wait at least 24 hours before escalating a question.

**Close the loop.** When you solve your problem, post the solution in the thread. This helps others with the same question.

```text
[Solved] The issue was that PostgreSQL was listening on a Unix socket, not
TCP. I added `host=localhost` to the connection string and it worked.
Thanks for the pointers.
```

### Time Zones and Working Hours

In distributed teams, time zone awareness is essential for effective communication.

**Use UTC for timestamps.** Always include the time zone when mentioning times. If you say "the deploy is at 3 PM," specify which time zone.

```text
Deploy window: Tuesday, 14:00-15:00 UTC
(09:00-10:00 Eastern, 15:00-16:00 CET, 19:30-20:30 IST)
```

**Do not expect immediate answers.** If you send a message to someone who is in a different time zone, they will see it when they start their day. Plan accordingly.

**Use status messages.** Set your Slack status to indicate your working hours, when you are away, and when you are in focus time.

**Schedule messages.** Use Slack's scheduled send feature to deliver messages during the recipient's working hours.

**Async-first culture:** Default to async communication. Do not expect synchronous responses. If something is urgent, use the agreed-upon escalation channel.

**Respect boundaries:** Do not send non-urgent messages outside of working hours. If you work unusual hours, use scheduled sends.

### Tone in Text

Text lacks vocal tone, facial expression, and body language. Readers infer tone from word choice, punctuation, and formatting.

**Positive tone indicators:**

- "Please" and "thank you" are never wasted
- "I think" softens assertions
- "What do you think?" invites collaboration
- "Great work on" acknowledges effort

**Negative tone indicators (avoid):**

- "Obviously" implies the reader should already know
- "Actually" can sound confrontational
- "As I said before" implies frustration
- ALL CAPS reads as shouting
- Excessive exclamation marks feel unprofessional in some contexts

```text
// Could read as harsh
You need to fix this before the deploy.

// Softer while still clear
This needs to be fixed before the deploy. Can you take a look?
```

**Use emoji thoughtfully.** An emoji can soften a message or add context, but emoji culture varies by team and individual. When in doubt, use words instead.

**Be explicit about tone when needed.** If you are worried a message might be misinterpreted, add a tone marker.

```text
[Not urgent] Can you review this PR when you have a moment?
[Just sharing] Here is the latest benchmark results for reference.
[Curious, not critical] Why did you choose this approach?
```

### Avoiding Miscommunication

Most misunderstandings in text communication come from assumptions the writer did not realize they were making.

**Check for ambiguous pronouns.** "It" and "they" can refer to multiple things.

```text
// Ambiguous
The API returns an error. It needs to be fixed.
(Does "it" mean the API or the error?)

// Clear
The API returns an error. The error message needs to be fixed.
```

**State assumptions explicitly.** If your message relies on an assumption, state it.

```text
Assuming the database migration has been applied, we can deploy the new
service. Has the migration been run in staging?
```

**Confirm understanding.** For important messages, ask the recipient to confirm they understood.

```text
Does that make sense? Let me know if you'd like me to clarify anything.
Just to confirm: you will take the lead on the database migration, right?
```

**Overcommunicate on critical items.** For deployment instructions, security-sensitive changes, or client-facing communication, include more detail than you think is necessary.

### Email Etiquette

**Subject lines:** Be specific. Include project names, ticket numbers, and clear summaries.

```text
// Poor subject
Update

// Good subject
[TICKET-1234] Payment Service: Proposed architecture for Q3 refactor
```

**Reply-all discipline:** Use reply-all only when everyone on the thread needs the information. Most internal email should be reply-to-sender.

**CC and BCC:** CC people who need to know but do not need to act. BCC is rarely appropriate in internal communication. If someone needs to be on the thread, put them in the To or CC field openly.

**Forwarding:** When forwarding an email thread, summarize the context for the new recipient. Do not just forward a 50-message thread with no explanation.

```text
Forwarding the thread about the database migration timeline. The key decision
was to push the migration to next sprint. Please review the timeline in the
last message.
```

**Signatures:** Keep your email signature concise. Name, title, company, and one link. Avoid images, quotes, and disclaimers longer than the email body.

**Response time:** Within 24 hours for internal email. If you need more time to respond, send a brief acknowledgment.

```text
Received, thanks. I'll review this and get back to you by Wednesday.
```

### Slack Etiquette

**Channel conventions:**

- Use the most specific channel for your message
- Do not cross-post the same message to multiple channels
- Use threads for extended discussions
- Use announcements for messages that need everyone's attention

**Status conventions:**

- Set your status when you are in a meeting, in focus time, or out of office
- Update your status before you step away
- Respect others' status indicators

**Reactions:**

- Use reactions to acknowledge messages without spamming the channel with replies
- A thumbs-up reaction says "I saw this" or "I agree"
- Do not use reactions to disagree (use words instead)

**Files and links:**

- Paste files directly into Slack instead of sharing from cloud storage
- When sharing links, include a summary of what the link contains
- Do not share sensitive information (passwords, API keys, PII) in Slack

**Working hours:**

- Respect your team's working hours
- Use scheduled sends for messages outside of business hours
- Do not expect responses outside of the sender's working hours

**Thread discipline:**

- Reply in thread to keep channels readable
- Do not post a new message in a channel when there is already an active thread
- When a thread resolves, post a summary in the channel

```text
[Summary] The database migration discussion concluded: we will go with
Option B (rolling update) with a rollback plan. Action items assigned to
@alice and @bob. Full discussion in thread.
```

### Channel-Specific Patterns

**Standup channel (#standup):**

- Post your update at the same time each day
- Follow the established format (what you did, what you will do, blockers)
- Use threads only for follow-up questions, not for standup answers

**Incident channel (#incidents):**

- Every message must have a timestamp (UTC)
- Only the incident commander should post status updates
- Other participants use the incident thread or war room
- After the incident, archive the channel

**Project channel (#project-name):**

- Pin important links and documents
- Use threads for decision discussions
- Post weekly summaries of progress
- Announce milestones and blockers to the whole channel

**Ask channel (#help or #questions):**

- One question per thread
- Mark solved questions with a checkmark reaction
- If you know the answer, reply even if someone else is already helping
- Link to documentation or past threads when relevant

## Glossary

| Term | Definition |
|------|------------|
| Async communication | Communication that does not require participants to be online simultaneously |
| Cross-posting | Posting the same message to multiple channels |
| Do-not-disturb | A Slack setting that suppresses notifications during specified hours |
| @here | Mentions all currently active members in a channel |
| @channel | Mentions all members of a channel regardless of status |
| @everyone | Mentions all members of a workspace |
| Scheduled send | Sending a message at a specified future time |
| Slack thread | A conversation attached to a specific message in a channel |
| Status | Slack indicator showing availability (online, away, in a meeting, etc.) |
| Tone marker | A prefix or note that clarifies the intended tone of a message |

## Quick References

- [Slack Etiquette Guide](https://slack.com/blog/collaboration/etiquette-tips-in-slack) — official guide from Slack
- [Email Etiquette: The Do's and Don'ts](https://www.grammarly.com/blog/email-etiquette/) — common email best practices
- [Remote Work Etiquette (GitLab)](https://about.gitlab.com/company/culture/all-remote/remote-communication/) — GitLab's guide to async communication
- [No Hello](https://nohello.net/) — why you should not just say "hello" in chat
- [How to Ask Good Questions on Slack](https://www.honeycomb.io/blog/how-to-ask-good-questions-on-slack) — advice for getting helpful answers

## Next Steps

- [Writing Effective Code Review Comments](code-reviews.md) — applying async communication skills to code review
- [Stakeholder Updates & Status Reports](stakeholder-updates.md) — communicating project status to different audiences
