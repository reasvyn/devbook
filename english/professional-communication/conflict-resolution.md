# Conflict Resolution for Technical Teams

## Description

Technical disagreements are inevitable in engineering teams — over architecture, code style, tooling, and ownership. This document covers structured techniques for resolving technical conflict, separating substantive disagreement from interpersonal friction, and building processes that prevent recurring conflicts.

## Prerequisites

- [Giving & Receiving Technical Feedback](giving-feedback.md) — feedback techniques that prevent and resolve conflicts
- [RFCs & Technical Decision Documents](rfcs.md) — documentation practices that capture decisions and prevent re-litigation

## Table of Contents

- [The Nature of Technical Conflict](#the-nature-of-technical-conflict)
- [Types of Technical Conflict](#types-of-technical-conflict)
- [Separating Technical Disagreement from Personal Conflict](#separating-technical-disagreement-from-personal-conflict)
- [Writing a Conflict Position Statement](#writing-a-conflict-position-statement)
- [The SCRIP Model: Share, Clarify, Resolve, Implement, Prevent](#the-scrib-model-share-clarify-resolve-implement-prevent)
- [Escalation Paths](#escalation-paths)
- [Mediation Techniques](#mediation-techniques)
- [Documenting Decisions After Conflict](#documenting-decisions-after-conflict)
- [Preventing Recurring Conflicts with Process](#preventing-recurring-conflicts-with-process)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### The Nature of Technical Conflict

Technical conflict is often seen as a failure of rationality — if everyone were perfectly logical, disagreements would not exist. In reality, technical decisions involve trade-offs, incomplete information, and subjective priorities. Reasonable engineers can disagree about the same data.

**Why technical conflicts feel intense:**

- Engineers invest ego in their work. A challenge to the code feels like a challenge to competence.
- Technical decisions have real consequences. Choosing the wrong database can cost months of work.
- Engineers are trained to find flaws. A code review is an adversarial process by design.
- Strong opinions are common. Engineers with deep expertise feel certainty about their positions.
- Decisions are often irreversible or expensive to reverse. The stakes raise the emotional temperature.

**The cost of unresolved conflict:**

- Decisions are made by whoever shouts loudest, not by the best argument
- Team members avoid working together
- Technical debt accumulates because no consensus can be reached
- Good engineers leave teams with toxic conflict patterns
- Energy is spent on politics instead of product

**The cost of surface-level "resolution":**
- False consensus where disagreeing parties remain unconvinced
- Silent non-compliance — the losing party does not follow the decision
- Resentment that surfaces in unrelated discussions
- The conflict recurs at the next decision point

### Types of Technical Conflict

Not all technical conflicts are the same. Identifying the type determines the resolution strategy.

**Architecture decisions:**

The most common source of high-stakes conflict. Monolith vs. microservices, SQL vs. NoSQL, synchronous vs. event-driven. These decisions have long-lasting consequences.

```text
Example: "Should we use Kafka or RabbitMQ for our event pipeline?"
- Pro-Kafka: better throughput, replay capability, ecosystem
- Pro-RabbitMQ: simpler, easier to operate, sufficient for current load
- Actual trade-off: Kafka wins at scale; RabbitMQ wins at simplicity
- Resolution depends on: growth projections, team expertise, operational capacity
```

**Code style and conventions:**

Lower stakes but surprisingly emotional. Tabs vs. spaces, naming conventions, file organization.

```text
Example: "Should we use Prettier defaults or customize the configuration?"
- Pro-defaults: zero configuration, widest community acceptance
- Pro-custom: specific preferences for readability
- Resolution: usually a linter config with an auto-formatter
```

**Tooling decisions:**

CI/CD platforms, monitoring tools, IDEs, dependency management. These affect everyone's daily workflow.

```text
Example: "Should we migrate from Jenkins to GitHub Actions?"
- Pro-migration: simpler configuration, tighter GitHub integration
- Pro-Jenkins: extensive existing pipelines, deep customization
- Resolution: trial period for the new tool, then team vote
```

**Ownership and responsibility:**

Who maintains which service? Who is on call for which component? Who reviews PRs for which area?

```text
Example: "Which team owns the shared authentication library?"
- Team A: uses it most, wants to control its evolution
- Team B: depends on it, wants input into breaking changes
- Resolution: shared ownership model with defined change processes
```

**Process conflicts:**

Agile vs. waterfall, sprint length, definition of done, code review requirements.

```text
Example: "Should we require two approvals on every PR or one?"
- Pro-two: fewer bugs, more knowledge sharing
- Pro-one: faster delivery, less bottleneck
- Resolution: depends on team maturity, risk tolerance, project phase
```

**Resource conflicts:**

Who gets the senior engineer's time? Which feature gets the capacity? Which tech debt gets addressed first?

These may not be technical disagreements at all but prioritization disagreements dressed in technical language.

### Separating Technical Disagreement from Personal Conflict

The most dangerous pattern in engineering teams is mistaking a personal conflict for a technical disagreement or vice versa.

**Signs it is a technical disagreement:**
- The discussion focuses on data, trade-offs, and criteria
- Both parties can articulate the other's position accurately
- New information changes positions
- The disagreement stays contained to the specific topic
- Both parties respect each other's expertise outside this topic

**Signs it is or has become personal:**
- The discussion uses loaded language ("obviously," "clearly," "anyone would know")
- Positions are attributed to personality ("you always over-engineer things")
- Past unrelated conflicts are brought up
- The tone shifts from collaborative to combative
- Either party avoids the other outside the specific topic
- The disagreement persists despite clear evidence

**The conversion pattern:**

A technical disagreement becomes personal when someone feels their competence is being attacked. Once that happens, the conversation is no longer about the technical issue.

**Preventing the conversion:**

**Use "I" statements about your reasoning, not "you" statements about their judgment:**

```text
// Personal-triggering
You are over-engineering this.

// Technical
I am worried that this abstraction adds complexity without clear benefit.
```

**Separate the person from the proposal:**

```text
// Conflating
You always choose the most complex solution.

// Separating
This proposal uses event sourcing. My concern is that event sourcing adds
operational complexity. Let's evaluate whether the benefits justify the cost.
```

**When you feel the conversation turning personal:**

```text
I notice we are both getting defensive. Can we step back and restate the
technical trade-offs we are weighing?
```

**If the other person makes it personal:**

```text
I hear that you disagree with my approach. Let's focus on the specific
trade-offs rather than general patterns. What exactly concerns you about
this design?
```

**When to stop and reset:**

If either party feels attacked, stop the discussion. Schedule a follow-up with a neutral facilitator. Do not try to resolve a personal conflict in the same conversation where the technical issue lives.

### Writing a Conflict Position Statement

A conflict position statement is a written document that forces clarity and prevents unproductive debate. Each party writes one before a formal resolution session.

**Structure of a position statement:**

```
1. The question or decision to be made
2. My position
3. My rationale (with evidence or data)
4. The trade-offs I acknowledge
5. What would change my mind
6. A proposed path forward
```

**Template:**

```markdown
## Decision: [What are we deciding?]

## My Position
[One sentence stating my preferred option]

## Rationale
- [Data point or reasoning supporting position]
- [Another data point]
- [Experience or precedent]

## Acknowledged Trade-Offs
- [What I recognize I am sacrificing]
- [Downsides of my position]

## What Would Change My Mind
- [Specific evidence or condition that would make me switch]
- [Scenario or data that would invalidate my reasoning]

## Proposed Path Forward
- [Actionable next step that respects both positions]
```

**Why writing helps:**

- Forces precision — vague arguments become obvious when written
- Reduces repetition — both parties read each other's full position before discussing
- Identifies common ground — they often agree on more than they think
- Documents the decision — the statements form the basis for the decision record

**Example position statement:**

```markdown
## Decision: Should the new billing service use PostgreSQL or DynamoDB?

## My Position
Use PostgreSQL.

## Rationale
1. The billing data is highly relational (invoices, line items, payments,
   refunds, adjustments). Modeling this in DynamoDB requires complex
   denormalization and multiple access patterns.
2. The team has strong PostgreSQL experience and no DynamoDB experience.
3. Current volume (50K invoices/month) is well within PostgreSQL's range.
4. We already operate PostgreSQL in production with established monitoring.

## Acknowledged Trade-Offs
- DynamoDB offers stronger horizontal scaling if volume grows 100x.
- PostgreSQL requires more careful index management at scale.

## What Would Change My Mind
- Evidence that projected volume will exceed 5M invoices/month within 12 months.
- A concrete plan for DynamoDB access pattern modeling that the team feels
  confident implementing.

## Proposed Path Forward
1. Finalize volume projections with product team.
2. If projections are under 5M/month, use PostgreSQL.
3. If projections exceed 5M/month, schedule a spike to prototype DynamoDB
   access patterns.
```

### The SCRIP Model: Share, Clarify, Resolve, Implement, Prevent

SCRIP is a five-stage model for resolving technical conflicts systematically.

**Stage 1: Share — each party states their position without interruption.**

Rules:
- One person speaks at a time
- No interruptions, no cross-talk
- The listener takes notes
- No rebuttal during this stage

```text
Facilitator: "Alex, please share your position on the caching strategy. Maria,
please hold your questions and responses until Alex finishes."
```

**Stage 2: Clarify — each party asks questions to understand the other's position.**

Rules:
- Questions only — no statements, no arguments
- "What do you mean by X?" "How did you arrive at that conclusion?"
- Paraphrase to confirm understanding

```text
Maria: "Alex, you said the Redis cache adds operational complexity. Can you
give me a specific example of what complexity you foresee?"
Alex: "Maria, you mentioned Redis handles 100K ops/sec easily. Is that based
on your experience or benchmarks?"
```

**Stage 3: Resolve — find a path forward.**

Techniques:
- Identify areas of agreement first
- List options, not positions
- Evaluate options against agreed criteria
- Choose the best option or define how the decision will be made

```text
Facilitator: "You both agree that caching is needed. You disagree on Redis
vs. in-memory. Let's agree on criteria: operational cost, performance, and
team familiarity. How does each option score on these?"
```

**Stage 4: Implement — agree on concrete next steps.**

Outputs:
- The decision
- Who is responsible for implementing
- Timeline
- Success criteria
- Review date

```text
Decision: Use Redis with a 5-minute TTL.
Owner: Maria will implement the caching layer.
Timeline: PR by Friday, deploy by Tuesday.
Success: p95 latency under 200ms for dashboard queries.
Review: After two weeks, review latency metrics in the team retro.
```

**Stage 5: Prevent — what process prevents this conflict recurring?**

Questions to ask:
- Should we have documented guidelines for this type of decision?
- Do we need a decision-making framework for future cases?
- Should we schedule a review of this decision in three months?
- What would we do differently next time?

```text
Prevention: Add a "caching strategy" section to the architecture decision
record template. Future caching decisions should reference this record.
```

**When SCRIP is appropriate:**

- Ongoing disagreements that block progress
- Architectural decisions with significant impact
- Conflicts that have become emotional or personal
- Decisions that affect multiple teams

**When SCRIP is too heavy:**

- Minor style disagreements (use a linter)
- One-time decisions with low impact (just pick one)
- Situations where one party clearly has authority to decide (tech lead decision)

### Escalation Paths

Not all conflicts can be resolved between the two parties. A clear escalation path ensures conflicts are resolved before they damage the team.

**Level 1: Direct resolution between parties.**

The two engineers attempt to resolve the disagreement using the techniques in this document. Target: resolved within 3 days.

**Level 2: Tech lead or team lead facilitation.**

If Level 1 fails, the tech lead facilitates a discussion. The tech lead does not impose a decision unless necessary. Target: resolved within 1 week.

**Level 3: Engineering manager decision.**

If the tech lead cannot facilitate resolution, or if the conflict has interpersonal elements, the engineering manager makes the call. The manager considers technical input but may weigh team health, deadlines, and organizational priorities. Target: resolved within 2 weeks.

**Level 4: Architecture review board or senior staff.**

For conflicts with organization-wide implications (company-wide tech stack decisions, security architecture), escalate to a cross-team architecture review board. Target: resolved within 1 month.

**Level 5: People team / HR.**

For conflicts that involve harassment, discrimination, or behavior that violates company policy. This path is separate from technical escalation and should be used when behavior is the issue, not the technical decision.

**When to escalate:**

| Situation | Appropriate level |
|-----------|-----------------|
| Disagreement on microservice boundaries | Level 2 (tech lead) |
| Two engineers who cannot work together | Level 3 (manager) |
| Company-wide database standard | Level 4 (architecture board) |
| Hostile comments in code review | Level 5 (HR) |
| Resourcing conflict between teams | Level 3 (manager) |
| Framework choice for new service | Level 2 (tech lead) |

**The escalation agreement:**

Before a conflict arises, teams should agree on their escalation path and document it. Having the path agreed in advance makes it easier to invoke when needed.

```text
Our team escalation path:
1. Discuss directly for 3 days.
2. If unresolved, involve the tech lead.
3. If the tech lead cannot help, the engineering manager decides.
4. Escalate to architecture board for cross-team decisions.
```

### Mediation Techniques

Mediation is the art of facilitating a conversation between two people in conflict without taking sides. The mediator's job is to ensure both parties are heard and to guide them toward resolution.

**The mediator's role:**

- Remains neutral throughout
- Ensures each person speaks without interruption
- Reframes personal attacks as substantive concerns
- Keeps the discussion focused on the current issue
- Prevents re-litigation of past conflicts
- Guides the conversation toward actionable outcomes

**Reframing techniques:**

When one party makes a personal statement, reframe it as a substantive concern.

```text
// Personal statement
Alex: "Maria always over-engineers everything. This is another example."

// Reframed
Mediator: "Alex, you are concerned that the proposed solution is more complex
than necessary. Can you describe the specific complexity you see in this
design?"
```

```text
// Personal statement
Maria: "Alex is just afraid of change. He always wants to stick with what he
knows."

// Reframed
Mediator: "Maria, you believe the existing stack can handle the requirements
and that migrating adds risk. Can you share the data you are basing that on?"
```

**The "both-and" framing:**

Instead of forcing an either/or choice, explore how both concerns can be addressed.

```text
Alex wants: safety and stability.
Maria wants: speed and innovation.

Both-and framing: "How can we deliver the feature quickly while ensuring we
can roll back if something goes wrong?"
```

**Recognizing common ground:**

Mediators actively identify and highlight agreement.

```text
Mediator: "I want to point out that you both agree microservices introduce
operational complexity. You disagree on whether the benefits outweigh that
cost. Is that accurate?"
```

**Timeboxing:**

Set clear time limits for each stage of the discussion.

```text
"Let's take 15 minutes to share positions. Then 10 minutes for questions.
Then 15 minutes to find a path forward. If we do not reach consensus,
the tech lead will decide by end of week."
```

**The one-sentence summary:**

At the end of the mediation, ask each person to summarize the outcome in one sentence.

```text
Alex: "We agreed to use PostgreSQL with a plan to evaluate DynamoDB if
volume exceeds 5M/month."
Maria: "We will use PostgreSQL now, with benchmarks for DynamoDB ready
before Q3 planning."
```

**When mediation fails:**

If the mediator cannot guide the parties to resolution:
- Escalate to the next level (manager or architecture board)
- The mediator documents both positions and why resolution was not reached
- A decision is imposed by the appropriate authority
- The conflict is revisited at a set future date with new data

### Documenting Decisions After Conflict

The resolution of a conflict is only valuable if it is captured and referenced later. Undocumented decisions get re-litigated.

**The Decision Record format:**

A lightweight document that captures what was decided, why, and who was involved.

```markdown
# Decision Record: [Title]

## Decision
[One sentence stating the decision]

## Date
[Date of decision]

## Participants
- [Names and roles of everyone involved]

## Context
[The problem or question that required a decision]

## Options Considered
- Option A: [brief description]
- Option B: [brief description]
- Option C: [brief description]

## Rationale
[Why the chosen option was selected. Reference data, benchmarks, or team
standards.]

## Trade-offs Accepted
[What was sacrificed. Known downsides of the chosen option.]

## Dissenting Opinion
[If anyone disagreed, summarize their position and why it was not chosen.
This acknowledges their input and prevents future re-litigation.]

## Action Items
- [ ] [Owner] — [Action] — [Deadline]

## Review Date
[When to revisit this decision to evaluate whether it is still correct.]
```

**Why the Dissenting Opinion section matters:**

- It shows the losing party that their position was heard and considered
- It provides context for future engineers who might wonder why a different approach was not chosen
- It reduces the incentive to bring up the same argument in future meetings
- It documents conditions under which the decision should be revisited

**Where to store decision records:**

- A `decisions/` directory in the repository
- An architecture decision log in the team wiki
- Links from the relevant code or documentation back to the decision record

**The 3-month review:**

Schedule a follow-up to evaluate whether the decision was correct. If it was not, correct it early.

```text
Review date: 2025-07-15
Criteria for success: p95 latency under 200ms, zero caching-related incidents
Outcome: [TBD]
```

### Preventing Recurring Conflicts with Process

The best conflict resolution is prevention. Process and documentation remove ambiguity that causes conflict.

**Processes that prevent technical conflict:**

**Architecture decision records (ADRs).** Every significant technical decision gets an ADR. Future discussions reference it rather than re-debating.

**RFC process for significant changes.** Forces trade-off analysis before code is written.

**Coding standards and linters.** Automated style enforcement removes the most common source of low-value conflict.

**Decision-making framework.** Agree how decisions are made at each level:

| Decision type | Method |
|--------------|--------|
| Code style | Linter enforces |
| Minor library choice | Author decides |
| Architecture within a service | Tech lead decides after discussion |
| Cross-service architecture | RFC + team consensus |
| Company-wide standard | Architecture board |

**Ownership model.** Each component has an explicitly designated owner who makes final decisions, with input from stakeholders.

**Regular architecture reviews.** Schedule recurring meetings for upcoming decisions before they become urgent under deadline pressure.

**Blame-free incident reviews.** Focus on what the process missed, not who made the call.

**Team health checks.** Periodically survey about conflict health — comfort disagreeing, resolution timeliness, and recurring tensions.

## Glossary

| Term | Definition |
|------|------------|
| SCRIP model | Share, Clarify, Resolve, Implement, Prevent — a five-stage conflict resolution framework |
| Position statement | A written document stating one's position, rationale, and conditions for changing |
| ADR | Architecture Decision Record — a documented architectural decision |
| Modular monolith | A single deployment unit with clearly separated internal modules |
| Shared ownership | Multiple teams or individuals responsible for different aspects of the same component |
| Reframing | Restating a personal attack as a substantive concern |
| Timeboxing | Setting a fixed time limit for an activity |
| Escalation path | A defined hierarchy for raising unresolved conflicts |
| Decision record | A document capturing what was decided, why, and by whom |
| Both-and framing | Finding a solution that addresses both parties' concerns |
| Lazy consensus | A decision-making model where silence implies consent |
| False consensus | Apparent agreement where one party does not genuinely agree |
| Dissenting opinion | A documented position that disagrees with the chosen decision |
| Facilitation | Guiding a discussion without taking sides |
| Gatekeeping | Using authority to block decisions unnecessarily |

## Quick References

- [Crucial Conversations, Patterson et al.](https://www.crucialconversations.com/) — tools for high-stakes discussions
- [Difficult Conversations, Stone, Patton, Heen](https://www.penguinrandomhouse.com/books/172305/difficult-conversations-by-douglas-stone-bruce-patton-and-sheila-heen/) — framework for handling tough topics
- [Architecture Decision Records](https://adr.github.io/) — documentation standard for architectural decisions
- [Google's Guide to Technical Decision-Making](https://google.github.io/eng-practices/review/decisions/) — making and documenting technical decisions
- [Blameless Postmortems, Etsy](https://codeascraft.com/2012/05/22/blameless-postmortems/) — learning from failures without blame

## Next Steps

- [Writing Effective Code Review Comments](code-reviews.md) — applying conflict resolution in the code review process
- [Stakeholder Updates & Status Reports](stakeholder-updates.md) — communicating decisions and conflicts to stakeholders
