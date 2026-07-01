# Giving & Receiving Technical Feedback

## Description

Technical feedback is the primary mechanism for improving code quality, architecture decisions, and team processes. This document covers structured models for delivering feedback, separating technical critique from personal criticism, receiving feedback gracefully, and navigating disagreements in engineering contexts.

## Prerequisites

- [Writing Effective Code Review Comments](code-reviews.md) — applying feedback techniques in the code review context
- [Conciseness](../grammar-and-style/conciseness.md) — keeping feedback focused and actionable

## Table of Contents

- [Why Technical Feedback Is Different](#why-technical-feedback-is-different)
- [The SBI Model: Situation, Behavior, Impact](#the-sbi-model-situation-behavior-impact)
- [Separating Technical Feedback from Personal Criticism](#separating-technical-feedback-from-personal-criticism)
- [Framing Suggestions as Questions](#framing-suggestions-as-questions)
- [The Feedback Sandwich Controversy](#the-feedback-sandwich-controversy)
- [Receiving Feedback Gracefully](#receiving-feedback-gracefully)
- [Handling Disagreement](#handling-disagreement)
- [Asynchronous vs. Synchronous Feedback](#asynchronous-vs-synchronous-feedback)
- [Escalation and Mediation](#escalation-and-mediation)
- [Building a Feedback Culture](#building-a-feedback-culture)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Why Technical Feedback Is Different

Technical feedback operates in a unique space. Unlike general interpersonal feedback, technical feedback must balance objective correctness with subjective preference. A line of code can be demonstrably wrong, or it can be a valid stylistic choice. The challenge is distinguishing between the two.

**Technical feedback typically involves:**

- Code correctness and logic errors
- Architecture and design decisions
- Performance and scalability considerations
- Security implications
- Style and maintainability preferences
- Process and workflow suggestions

Each type requires a different approach. A logic error demands direct correction. A style preference invites discussion. Mixing up these modes causes friction.

**The cost of poor feedback:**

- Defensive reactions that block learning
- Silent resentment that erodes team cohesion
- Bad decisions being accepted because no one wanted to argue
- Good engineers leaving teams with unhealthy feedback cultures

**The cost of no feedback:**

- Mistakes compound over time
- Junior engineers do not grow
- Knowledge is not shared
- Team standards degrade

Finding the middle path — direct enough to be useful, kind enough to be heard — is the goal.

### The SBI Model: Situation, Behavior, Impact

The SBI model is a structured framework for delivering feedback that reduces defensiveness and increases clarity. It separates feedback into three components: the context, the observable action, and the consequence.

**Situation — where and when did the behavior occur?**

Identify the specific context. This anchors the feedback in a concrete event rather than a general pattern.

```text
During yesterday's sprint retro...
In the PR for the payment service refactor...
In the incident review for the database migration...
```

**Behavior — what exactly did the person do?**

Describe the observable action without interpretation or judgment. Use neutral language. Stick to facts.

```text
You proposed switching from PostgreSQL to DynamoDB...
You removed the integration tests before merging...
You interrupted the architect three times during the design review...
```

**Impact — what was the result of that behavior?**

Explain the effect on the team, the project, or the outcome. This is where the feedback gets its weight.

```text
...which caused the team to spend two days migrating back when we discovered the access pattern did not fit.
...which meant the build pipeline did not catch the regression that reached production.
...which made it difficult for the architect to present their full analysis.
```

**Putting it together:**

```text
SITUATION: In yesterday's architecture review for the event-ingestion pipeline,
BEHAVIOR: you dismissed the Kafka proposal without discussing trade-offs,
IMPACT: which made the team hesitant to propose alternatives and led to a less optimal decision.
```

**Why SBI works:**

- Situation forces specificity (reduces vague generalizations)
- Behavior stays observable (reduces defensiveness)
- Impact connects to real consequences (motivates change)
- The three-part structure prevents rambling

**Common SBI mistakes:**

- Skipping situation: "You always interrupt people." This is a general accusation, not feedback.
- Adding interpretation to behavior: "You were rude during the retro." Rude is an interpretation, not an observable behavior. "You said three times that the proposal was 'obviously wrong'" is observable.
- Assuming impact: "You made everyone feel bad." State what happened, not how you imagine others felt.

### Separating Technical Feedback from Personal Criticism

The most common failure in technical feedback is conflating the work with the person. When a developer hears "this code is wrong," their brain may process it as "I am wrong." This is the fundamental attribution error at work.

**Techniques to separate code from person:**

**Use passive observation for the code, active language for the person's autonomy:**

```text
// Poor — conflates person and code
You wrote this incorrectly.

// Better — separates person from code
This function call passes arguments in the wrong order. The expected order is (lat, lon).
```

**Refer to the code as a shared artifact, not the individual's property:**

```text
// Poor
Your code has a bug here.

// Better
There is an off-by-one error on line 34. The loop condition should be `<=` instead of `<`.
```

**Avoid absolute statements about the person's capability:**

```text
// Poor
You clearly did not test this.

// Better
A null input breaks this path. We might need a test for that edge case.
```

**Highlight the decision, not the decision-maker:**

```text
// Poor
You were wrong to choose a singleton here.

// Better
Using a singleton here introduces global state that makes testing harder. Could we use dependency injection instead?
```

**When you are on the receiving end:**

- Recognize that feedback about your code is feedback about the code
- Ask clarifying questions to stay in problem-solving mode
- Do not treat "I disagree with your approach" as "I disagree with you"

### Framing Suggestions as Questions

Questions are the gentlest form of feedback because they leave the recipient in control. Instead of telling someone what to do, you invite them to arrive at the conclusion themselves.

**Genuine questions** — you are actually curious:

```text
I saw you used an array instead of a set for the duplicate check. Was there a specific performance consideration?
```

**Suggestive questions** — you have a preferred direction but want to open discussion:

```text
Could we move this validation to the service layer instead of the controller? It would make the controller easier to test.
```

**Leading questions disguised as feedback** — these feel manipulative and should be avoided:

```text
// Poor — rhetorical and accusatory
Did you even consider the performance implications of this?

// Better — direct and helpful
This nested loop has O(n*m) complexity. If the dataset grows, we might see latency. Could we use a hashmap instead?
```

**When to use questions vs. direct statements:**

| Situation | Use |
|-----------|-----|
| Style preference or optional improvement | Question |
| Minor correctness issue | Question or suggestion |
| Security vulnerability | Direct statement |
| Logic error that will cause failure | Direct statement |
| Teaching opportunity | Question (let them discover) |
| Production blocker | Direct statement |

**The "I wonder" technique:**

Starting with "I wonder" signals curiosity rather than judgment.

```text
I wonder if the caching layer is working as intended here. The TTL seems short for this data.
```

**The "What do you think?" pivot:**

After stating your concern, hand control back to the recipient.

```text
The error handling in this path rethrows a generic exception that loses the original stack trace. What do you think about wrapping it with more context instead?
```

### The Feedback Sandwich Controversy

The feedback sandwich — wrapping criticism between two pieces of praise — is one of the most debated techniques in professional communication.

**The sandwich structure:**

```text
Positive opening:  "Great work on the API design."
Criticism:         "The error handling could be more robust."
Positive closing:  "Overall, the PR is well structured."
```

**Why proponents like it:**

- Softens the blow of criticism
- Ensures you acknowledge what went well
- Prevents the recipient from feeling purely attacked

**Why critics argue against it:**

- Feels manipulative once recognized
- The praise becomes devalued — recipients learn to wait for the "but"
- Indirect feedback is confusing: was the praise genuine or just setup?
- High-performers may miss the criticism entirely if the praise dominates

**What the research says:**

- The sandwich does not improve receptiveness to criticism
- It can reduce trust if the recipient perceives the praise as insincere
- Direct feedback, delivered respectfully, is more effective than structured praise-criticism-praise

**Alternatives to the sandwich:**

**Direct but kind:**
```text
The authentication flow is well implemented. I have a concern about the error handling on line 42 — it swallows exceptions that should propagate. Could we address that?
```

**Contextual feedback:**
```text
I know this feature shipped under a tight deadline. The functionality works correctly. For the next iteration, I think we should add retry logic to the external API calls.
```

**Separate channels:**
```text
// Praise publicly
In standup: "Alex did a great job on the migration script. It handled 50,000 records without issues."

// Criticism privately
In a DM: "The migration script works well, but I noticed it does not log failed records. Can we add that before we use it in production?"
```

**The compromise:**
Use the sandwich only when you need to deliver corrective feedback to someone who is new to the team or anxious about performance. After they have established trust, switch to direct feedback.

### Receiving Feedback Gracefully

Receiving feedback is a skill that requires active effort. Your first reaction to criticism is often emotional. Managing that reaction determines whether you grow from the feedback or reject it.

**The Acknowledge-Reflect-Act framework:**

**Acknowledge** — receive the feedback without defending:

```text
Thank you for pointing that out. I see your concern about the null handling.
I understand why the nested callbacks make this hard to follow.
I hear you saying that the API contract is unclear.
```

**Reflect** — process the feedback before responding:

- Is the feedback about a fact (the code is wrong) or an opinion (the style is bad)?
- Does the feedback point to a real problem?
- Is there data or evidence that supports or contradicts the feedback?

**Ask clarifying questions:**
```text
Can you show me the specific case where this breaks?
What alternative approach would you suggest?
Is this a blocker or a nice-to-have?
Are there any constraints I should know about?
```

**Act** — decide what to do:

- Accept the feedback and make the change
- Accept the feedback but defer the change to a follow-up
- Reject the feedback with reasoned justification
- Propose a compromise

**Defensive responses to avoid:**

| Response | Why it fails |
|----------|-------------|
| "That's not a bug, it's a feature." | Dismisses the feedbacker's concern |
| "The user will never do that." | Assumes you know all edge cases |
| "This is how we always did it." | Prevents improvement |
| "You just don't like my style." | Conflates substantive feedback with preference |
| Silence / ignoring | Misses the opportunity to learn |

**Productive responses:**
```text
You're right, I missed that edge case. I'll add handling for it.
Let me think about that and get back to you after I check the data.
I see your point about consistency. I'll align with the existing pattern.
I disagree because of X, but I'm open to discussing it further.
```

**When feedback feels unfair:**

- Take a beat before responding. A 30-minute delay prevents regret.
- Separate intent from impact. The feedbacker may not have intended to hurt.
- Check your interpretation with a trusted colleague.
- If the delivery was inappropriate, address the delivery separately from the content.

### Handling Disagreement

Disagreement in technical feedback is inevitable and healthy. The goal is not to avoid disagreement but to resolve it constructively.

**Types of technical disagreement:**

- **Fact-based:** "This sort is O(n^2), not O(n log n)." Resolvable with evidence.
- **Principle-based:** "We should favor composition over inheritance." Resolvable with team standards.
- **Experience-based:** "In my last team, microservices caused more problems than they solved." Resolvable with context and data.
- **Taste-based:** "I prefer early returns over guard clauses." Often not worth resolving — agree to disagree.

**Steps for resolving disagreement:**

**State your position clearly:**
```text
I believe we should use feature flags for this rollout because it allows gradual exposure and quick rollback.
```

**State the opposing position accurately:**
```text
You believe feature flags add unnecessary complexity and that a simple toggle in the config is sufficient.
```

**Surface underlying criteria:**
```text
My priority is safety: I want to minimize blast radius if something goes wrong.
Your priority is simplicity: you want to minimize code and cognitive overhead.
```

**Look for a third option:**
```text
What if we use feature flags for the first week, then replace them with config toggles once the feature stabilizes?
```

**Escalate when needed:**
- If the disagreement affects team standards, bring it to the team
- If it affects architecture, involve the tech lead or architect
- If it is blocking progress, set a deadline for resolution

**Know when to yield:**

Ask yourself: "Is this hill worth dying on?" If the issue is minor and the other person feels strongly, let it go. Save your capital for issues that matter.

### Asynchronous vs. Synchronous Feedback

The communication channel affects how feedback is perceived.

**Asynchronous feedback (code review comments, email, Slack):**

| Advantage | Disadvantage |
|-----------|-------------|
| Gives time to compose thoughts | Loses tone and nuance |
| Creates a written record | Can feel cold or impersonal |
| Allows the recipient to process privately | Delayed response can feel like ignoring |
| Scales to many reviewers | Back-and-forth can drag on |

**Best practices for async feedback:**
- Write complete thoughts — do not send one sentence at a time
- Assume good intent — text has no tone
- Use clear subject lines or thread titles
- Set expectations for response time
- Avoid "just" and "only" — they minimize the recipient's work

**Synchronous feedback (video call, in-person, voice chat):**

| Advantage | Disadvantage |
|-----------|-------------|
| Tone and body language convey intent | Requires scheduling |
| Immediate clarification of misunderstandings | Can pressure the recipient into agreeing |
| Builds rapport and trust | No written record (unless recorded) |
| Faster resolution of complex issues | Emotional responses are harder to manage |

**Best practices for sync feedback:**
- State the purpose upfront: "I want to discuss the error-handling approach in your PR."
- Leave space for the recipient to respond
- Watch for defensive body language and adjust
- End with a summary of agreed actions

**Choosing the right mode:**

| Situation | Recommended mode |
|-----------|-----------------|
| Minor style nit | Async (code review comment) |
| Logic error or bug | Async (clear, documented) |
| Architecture disagreement | Sync (too complex for text) |
| Performance feedback | Async (can include data) |
| Sensitive feedback | Sync (tone matters most) |
| Repeated pattern of issues | Sync (needs discussion) |
| Praise | Either, but public praise is powerful |

### Escalation and Mediation

Some feedback situations escalate beyond what two people can resolve. Having a process for this protects both parties and the team.

**When to escalate:**

- The disagreement is blocking progress with no resolution in sight
- The feedback was delivered in a way that damaged the working relationship
- The issue has team-wide or organizational implications
- One party feels unsafe or unable to raise their perspective

**Escalation paths:**

**Direct manager:**
The first escalation point. Bring both parties together with their manager to facilitate a resolution.

**Tech lead or architect:**
For technical disagreements about architecture or design. The tech lead's decision is final unless overridden by broader consensus.

**Engineering manager:**
For process disagreements, team norms, or repeated patterns. The manager can set team standards that resolve the disagreement.

**HR or People team:**
For interpersonal conflicts, harassment, or behavior that violates company policy. This is a last resort after internal resolution attempts fail.

**Mediation techniques:**

**Neutral framing:**
```text
We have a disagreement about whether to use microservices or a monolith for the new billing service. Let me summarize both positions before we discuss.
```

**Time-bounded discussion:**
```text
We have 30 minutes to discuss this. If we cannot reach consensus, the tech lead will make the final call.
```

**Option generation:**
```text
Instead of arguing for our preferred positions, let's generate three options that we can both evaluate objectively.
```

**Written position statements:**
Before a mediation session, ask each party to write a one-paragraph summary of their position and the data supporting it. This clarifies thinking and prevents repeating the same arguments.

**Documenting the outcome:**
```text
Decision: We will use a monolith for the initial rollout.
Rationale: Team velocity is more important than scalability at this stage.
Condition: If API call volume exceeds 1,000 requests per second, we will revisit the architecture in a spike.
```

## Glossary

| Term | Definition |
|------|------------|
| SBI model | Situation, Behavior, Impact — a structured feedback framework |
| Feedback sandwich | Wrapping criticism between two pieces of praise |
| Fundamental attribution error | Attributing others' mistakes to character and your own to circumstances |
| Observable behavior | Something that can be seen or heard, without interpretation |
| Leading question | A question that suggests the desired answer |
| Acknowledge-Reflect-Act | A framework for receiving feedback productively |
| Defensive response | An automatic reaction that rejects or minimizes feedback |
| Escalation | Raising a disagreement to a higher authority for resolution |
| Mediation | Facilitated discussion to resolve a disagreement between parties |
| Position statement | A written summary of one's stance on a disagreement |
| N+1 query | A performance anti-pattern where queries execute per item in a loop |
| Write-through cache | A cache that writes data to both cache and backing store simultaneously |
| Timebox | A fixed time limit for an activity or discussion |
| Blast radius | The scope of damage when a system fails |

## Quick References

- [SBI Feedback Model — Center for Creative Leadership](https://www.ccl.org/articles/leading-effectively-articles/situation-behavior-impact-the-feedback-tool/) — detailed guide to the SBI framework
- [Radical Candor, Kim Scott](https://www.radicalcandor.com/) — the balance between caring personally and challenging directly
- [Nonviolent Communication, Marshall Rosenberg](https://www.cnvc.org/) — framework for compassionate communication
- [The Feedback Fallacy, HBR](https://hbr.org/2019/03/the-feedback-fallacy) — research challenging common feedback assumptions
- [Crucial Conversations, Patterson et al.](https://www.crucialconversations.com/) — tools for handling high-stakes discussions

## Next Steps

- [Conflict Resolution](conflict-resolution.md) — structured approaches for resolving technical disagreements
- [Meeting Notes & Action Items](meeting-notes.md) — documenting feedback and decisions in meetings
