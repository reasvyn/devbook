# Active vs. Passive Voice in Technical Writing

## Description

Voice determines who performs the action in your sentences. Active voice makes technical writing direct and clear; passive voice obscures responsibility. Knowing when to use each is essential for effective documentation.

## Prerequisites

- [Sentence Structure](sentence-structure.md) — subjects, verbs, objects

## Table of Contents

- [What Is Voice?](#what-is-voice)
- [Identifying Active and Passive Voice](#identifying-active-and-passive-voice)
- [Why Active Voice Dominates Technical Writing](#why-active-voice-dominates-technical-writing)
- [When Passive Voice Is Acceptable](#when-passive-voice-is-acceptable)
- [Agent Identification](#agent-identification)
- [Converting Passive to Active](#converting-passive-to-active)
- [The "By Zombies" Test](#the-by-zombies-test)
- [Tools for Detecting Passive Voice](#tools-for-detecting-passive-voice)
- [Passive Voice in Specifications](#passive-voice-in-specifications)
- [Passive Voice in Incident Reports](#passive-voice-in-incident-reports)
- [Voice Consistency Across Documents](#voice-consistency-across-documents)
- [Learning Tips](#learning-tips)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### What Is Voice?

Voice describes the relationship between the subject and the verb in a sentence.

**Active voice** — the subject performs the action:

```text
Subject     Verb          Object
The script  validates    the input.
```

**Passive voice** — the subject receives the action:

```text
Subject     Verb                 Actor
The input   is validated by      the script.
```

The passive voice always uses a form of "to be" (is, are, was, were, been) plus a past participle. The actor may be omitted entirely.

### Identifying Active and Passive Voice

**Ask three questions:**

1. Who or what is performing the action?
2. Who or what is receiving the action?
3. Is the performing entity the grammatical subject?

```text
// Active — subject (compiler) performs the action
The compiler translates TypeScript into JavaScript.
Subject = compiler, action = translates

// Passive — subject (TypeScript) receives the action
TypeScript is translated into JavaScript by the compiler.
Subject = TypeScript, action = is translated
```

**The "be + past participle" pattern:**

Look for any form of "to be" followed by a past participle:

```text
is defined        was created       are generated
were executed     been deployed     being configured
```

**Passive without an actor — the most common form in weak writing:**

```text
The configuration file was modified.
By whom? The sentence does not say.
```

**Active equivalent:**

```text
An administrator modified the configuration file.
```

**Agent vs. subject distinction:**

In passive voice, the grammatical subject is NOT the agent. The agent (the entity performing the action) either appears in a "by" phrase or is omitted entirely. Recognizing this distinction is the foundation of voice analysis.

### Why Active Voice Dominates Technical Writing

**Clarity — active voice makes clear who does what:**

```text
// Passive — who should do this?
The server should be restarted after the update.

// Active — who should restart?
Restart the server after the update.
```

**Conciseness — active voice is shorter:**

```text
// Passive (10 words)
The database migration is performed by the deployment script.

// Active (7 words)
The deployment script performs the database migration.
```

**Directness — active voice commands feel authoritative:**

```text
// Passive — indirect suggestion
The configuration file should be edited before running the application.

// Active — direct instruction
Edit the configuration file before running the application.
```

**Accountability — active voice assigns responsibility:**

```text
// Passive — who approved this?
The deployment was approved.

// Active — who approved?
The lead engineer approved the deployment.
```

**Readability scores and voice:**

Readability formulas like the Flesch-Kincaid Grade Level penalize passive voice. A sentence with multiple passive constructions typically scores 2-4 grade levels higher (harder to read) than its active equivalent. For technical documentation targeting a 9th-10th grade reading level, minimizing passive voice is essential.

### When Passive Voice Is Acceptable

**The actor is unknown or irrelevant:**

```text
The database connection was closed unexpectedly.
Who closed it? Unknown. The result is what matters.

The logs were deleted during the cleanup.
The cleanup process is the focus, not who ran it.
```

**The result is more important than the actor:**

```text
The application must be restarted after the update.
The action (restart) matters. The actor (the user) is implied.

The migration was completed successfully.
The outcome matters more than who ran it.
```

**Conventional in some formal contexts:**

```text
// Scientific/medical writing convention
The sample was analyzed using mass spectrometry.

// Legal/regulatory convention
The data shall be retained for a period of seven years.
```

**To avoid blaming the reader:**

```text
// Direct active — might feel accusatory
You entered an invalid password.

// Passive — less direct
An invalid password was entered.
```

**To emphasize the recipient over the actor:**

```text
Every request is logged by the server.
Requests are the important concept, not the server.

The new feature was requested by 200 users.
The feature is the subject, the users are secondary.
```

**In automated system messages:**

```text
File saved successfully.
(User is implied; the system could say "The system saved the file" but that adds noise)

Connection lost. Reconnecting...
(Agent is the network/system, irrelevant to the user)
```

**When the actor is obvious and repetitive:**

```text
// Passive is acceptable here — "we" is obvious
The package was published to the registry.
The build was triggered automatically.
Tests were executed against the staging environment.
```

In a CI/CD log, repeatedly saying "the system published the package," "the system triggered the build," "the system executed tests" would be tedious. Once the context is established, passive voice is more readable.

### Agent Identification

Identifying the agent (the actual performer of the action) is the critical first step in evaluating whether passive voice is appropriate.

**Explicit agent — appears in a "by" phrase:**

```text
Passive: The API was deprecated by the development team.
Agent:   the development team

Active:  The development team deprecated the API.
```

**Implicit agent — omitted but recoverable from context:**

```text
Passive: The configuration must be validated before deployment.
Agent:   You (the reader / operator) — implied by context

Active:  Validate the configuration before deployment.
```

**Unknown agent — cannot be determined:**

```text
Passive: The file was corrupted during the transfer.
Agent:   Unknown — network issue, disk error, or software bug
Active:  Not possible without speculating
```

This is the strongest case for keeping passive voice. If the agent is truly unknown, active voice would require fabricating an inaccurate subject.

**Agent identification exercise:**

```text
Sentence 1: The release was cancelled.
Agent: Unknown or deliberately omitted.
Assessment: Passive is appropriate IF the reason is explained next.

Sentence 2: The release was cancelled by the product manager.
Agent: the product manager.
Assessment: Passive is weak — rewrite to active.
→ The product manager cancelled the release.

Sentence 3: The deployment script creates the directory structure.
Agent: the deployment script.
Assessment: Already active — correct as written.
```

### Converting Passive to Active

**Step 1: Find the actor.** Ask "who or what does this?"

```text
Passive: The error is thrown when validation fails.
Actor:   The validation function (implicit)
Active:  The validation function throws an error.
```

**Step 2: Make the actor the subject:**

```text
Passive: The configuration is loaded by the application at startup.
Active:  The application loads the configuration at startup.
```

**Step 3: Replace "be" verbs with action verbs:**

```text
Passive:   The system is configured using environment variables.
Active:    You configure the system using environment variables.

Passive:   The connection is established when the user logs in.
Active:    The application establishes the connection when the user logs in.
```

**Step 4: When there is no actor, supply one:**

```text
Passive:   The port must be changed.
Who?:     The user
Active:   Change the port.

Passive:   The dependencies are installed.
Who?:     The installer
Active:   The installer installs the dependencies.
```

**Common passive constructions and their active equivalents:**

| Passive | Active |
|---------|--------|
| It is recommended that... | We recommend... |
| The file should be saved... | Save the file... |
| The connection was established... | The client established the connection... |
| The feature is used by... | Users use the feature to... |
| It can be observed that... | Observe that... |
| The configuration is applied... | Apply the configuration... |
| The function is called... | Call the function... |
| It was determined that... | We determined that... |

**Bulk conversion patterns:**

Certain passive patterns recur in technical documentation and have standard active rewrites:

```text
It is possible to X → You can X
X can be used to Y → Use X to Y
X is required to Y → You must X to Y
X has been designed to Y → X does Y
X needs to be Y'd → Y X
```

### The "By Zombies" Test

A simple test to detect passive voice: add "by zombies" after the verb. If the sentence still makes grammatical sense, it is passive.

```text
The logs were deleted. → The logs were deleted by zombies. ✓ (passive)

The script deletes the logs. → The script deletes the logs by zombies. ✗ (active)
```

**More examples:**

```text
The server is running. → The server is running by zombies. ✗ (not passive — "is running" is active, "running" is a present participle, not a past participle)

The server was restarted. → The server was restarted by zombies. ✓ (passive)

The deployment succeeded. → The deployment succeeded by zombies. ✗ (active — past tense, no "be" verb)
```

The test works because passive voice always uses a past participle, and "by zombies" fills the optional "by [actor]" slot.

**Why it works:**

Every passive verb phrase has the structure `[form of "be"] + [past participle]`. The past participle can always accept a following "by [noun]" phrase. If the verb after the "be" word is a present participle (ending in -ing) or an adjective, the "by zombies" insertion will produce ungrammatical nonsense.

### Tools for Detecting Passive Voice

**Automated writing assistants:**

| Tool | Detection method | Integration |
|------|-----------------|-------------|
| Grammarly | Rule-based NLP | Browser extension, desktop app, API |
| Hemingway Editor | Regex-based heuristic | Web editor, desktop app |
| ProWritingAid | Grammar rule engine | Browser extension, Word plugin |
| Vale (open source) | YAML-based rule files | CLI, CI pipeline, editor plugins |
| write-good (npm) | Regex patterns | CLI, Node.js library |
| textlint | Plugin architecture | Node.js, npm |
| LanguageTool | Java-based rule engine | API, browser extension, CLI |

**Configuring Vale for passive voice detection:**

```yaml
# .vale.ini
StylesPath = .vale/styles
MinAlertLevel = suggestion

[*.md]
BasedOnStyles = Vale

# Active voice rule in Vale style
extends: existence
message: "Avoid using passive voice: '%s'"
ignorecase: true
level: warning
raw:
  - "(\\b(?:am|are|was|were|been|being|be|is)\\s+(?:\\w+ed|\\w+en|\\w+n)\\b)"
tokens:
  - '(?:am|are|was|were|been|being|be)\s+\w+ed'
```

**CI/CD integration:**

Passive voice checks can be automated in CI pipelines:

```yaml
# .github/workflows/lint-docs.yml
name: Lint documentation
on: [pull_request]
jobs:
  vale:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: errata-ai/vale-action@v2
        with:
          files: docs/
```

This ensures that every PR with documentation violations is flagged before merging.

**Limitations of automated detection:**

No tool is 100% accurate. False positives include:
- Past participles used as adjectives ("The file is encrypted." — describes state, not action)
- Passive voice in technical terminology ("is comprised of," "is associated with")
- Intentional passive for emphasis or depersonalization

Always review flagged instances manually — the tool suggests, the writer decides.

### Passive Voice in Specifications

In formal specifications (RFCs, standards documents), passive voice is traditional but increasingly discouraged:

```text
// Traditional passive spec
The connection shall be established before any data is transmitted.

// Modern active spec
The client MUST establish the connection before transmitting any data.
```

**RFC 2119 keywords (MUST, SHOULD, MAY) with active voice:**

```text
// Passive — less clear
The configuration file should be validated before it is applied.

// Active — clear who does what
The system MUST validate the configuration file before applying it.
```

**Specification examples across domains:**

```text
// API specification — passive (traditional)
A GET request shall be sent to the /users endpoint, and a JSON response will be returned.

// API specification — active (preferred)
The client sends a GET request to /users. The server returns a JSON response.

// Security policy — passive (traditional)
Access must be granted only to authenticated users.

// Security policy — active (preferred)
The system grants access only to authenticated users.

// Error handling — passive (traditional)
An error shall be returned if the parameter is invalid.

// Error handling — active (preferred)
The endpoint returns an error if the parameter is invalid.
```

### Passive Voice in Incident Reports

Incident postmortems often use passive voice to depersonalize failures:

```text
The configuration was changed without going through the review process.
(Passive — avoids blaming the individual)

The database migration was executed with incorrect parameters.
(Passive — focuses on the action, not the person)
```

However, overusing passive voice in incident reports can hide root causes:

```text
// Passive — who approved the change?
The change was approved and deployed.

// Active — clearer accountability
The lead engineer approved the change, and the DevOps team deployed it.
```

**Best practice for incident reports:**

Use active voice when the actor is known and relevant. Use passive voice when the lesson is about process, not people.

**Structured postmortem format:**

```
Summary: The production database became unresponsive for 12 minutes.

Timeline:
- 14:02: An engineer deployed a schema migration.        (active — who did it)
- 14:05: The migration acquired a table-level lock.        (active — what it did)
- 14:07: Queries began queuing behind the lock.           (active — what happened)
- 14:12: The on-call engineer was alerted.                (passive — alert source irrelevant)
- 14:15: The migration was rolled back.                   (passive — focus on action)
- 14:17: The database recovered.                          (active — clear cause)

Root cause: The migration script was executed without a lock-timeout threshold.
             (passive — process lesson, not individual blame)
```

### Voice Consistency Across Documents

**Choose a voice strategy per document type:**

| Document Type | Voice Strategy |
|---------------|----------------|
| Tutorials | Active, second person ("You create a project...") |
| API Reference | Active, third person ("The function returns...") |
| README | Active, imperative ("Install the package...") |
| Specifications | Active, with RFC 2119 keywords |
| Incident Reports | Mix — active for facts, passive for process |
| Error Messages | Active ("File not found") or passive ("Access denied") |

**Mixed voice in the same paragraph creates confusion:**

```text
// Inconsistent — switches between passive and active
The configuration file was edited. Then, you restart the server.
After that, the new settings are applied.

// Consistent — all active
Edit the configuration file. Then, restart the server.
The application applies the new settings.

// Consistent — all passive (if deliberately chosen)
The configuration file was edited. The server was restarted.
The new settings were applied.
```

**Voice and tone across document types:**

| Document | Preferred voice | Example |
|----------|----------------|---------|
| Quickstart | Active, imperative | "Run `npm install` to set up dependencies." |
| Conceptual guide | Active, declarative | "The event loop processes callbacks in FIFO order." |
| Troubleshooting | Active, second person | "Check the logs for error codes." |
| Changelog | Active, past tense | "Fixed a memory leak in the connection pool." |
| FAQ | Active, second person | "Set the API key in your environment variables." |

## Learning Tips

- **Keep a "passive score" for your writing.** After finishing a document, count the passive constructions per 100 words. Aim for fewer than 3 per 100 words in technical documentation. Tracking this over time builds awareness.
- **Practice the conversion drill.** Take a paragraph of passive-heavy text (old academic papers, government documents) and rewrite every sentence in active voice. The act forces you to identify agents and recognize patterns.
- **Use a linter from day one.** Configure Vale or write-good in your editor so you see passive voice highlighted as you type. Immediate feedback rewires your writing habits faster than post-hoc editing.
- **Learn the exception list.** Memorize the specific cases where passive is acceptable (unknown actor, process focus, depersonalization). When you write a passive sentence, pause and verify it falls into one of these categories.
- **Read your writing aloud.** Passive voice often sounds stilted when spoken. Reading aloud exposes awkward constructions that silent reading misses. If a sentence feels unnatural to speak, it is likely passive or overly complex.
- **Review error messages and logs.** These are the most visible documentation to users. Ensure error messages use active voice ("The build failed because the test suite returned errors") rather than passive ("An error was encountered during the build").
- **Create a team style guide.** Document your team's voice conventions explicitly. Include before/after examples. This reduces debate during code review and creates a shared standard.
- **Audit old documentation quarterly.** Choose one section each quarter and rewrite it for voice consistency. Over time, the entire documentation set converges to a consistent active-voice standard without requiring a full rewrite.

## Glossary

| Term | Definition |
|------|------------|
| Active voice | The subject performs the action of the verb |
| Passive voice | The subject receives the action of the verb |
| Past participle | The verb form used in passive constructions (e.g., "validated," "created") |
| Actor | The entity performing the action in a sentence |
| Agent | Another term for the actor in a passive sentence |
| Subject | The grammatical entity that the sentence is about |
| Object | The entity receiving the action in an active sentence |
| Nominalization | Turning a verb into a noun (e.g., "implement" → "implementation") |
| "By zombies" test | Adding "by zombies" to detect passive voice |
| Imperative mood | Command form: "Install the package" |
| RFC 2119 | Standard for requirement keywords: MUST, SHOULD, MAY |
| Depersonalize | Removing the human actor from the description |
| Flesch-Kincaid | Readability formula that penalizes passive constructions |
| Vale | Open-source linter with configurable passive voice rules |
| Hemingway Editor | Writing app that highlights passive voice in real-time |
| Implicit agent | Actor omitted from the sentence but recoverable from context |
| Explicit agent | Actor stated explicitly, usually in a "by" phrase |
| False positive | Passive voice flagged by automation that is actually correct usage |
| Transition verb | A verb that takes a direct object and can be made passive |
| Intransitive verb | A verb that cannot be made passive (e.g., "arrive," "sleep") |
| Present participle | Verb form ending in -ing, not used in passive constructions |
| Linking verb | A verb (e.g., "is," "seems") that connects subject to complement, not passive |
| Readability score | Numeric measure of text complexity, affected by voice choices |
| Voice consistency | Using the same voice strategy throughout a document or section |

## Quick References

- [Microsoft Style Guide: Verbs](https://learn.microsoft.com/en-us/style-guide/grammar/verbs) — active voice guidance
- [Google Style Guide: Active Voice](https://developers.google.com/style/voice) — Google's recommendations
- [Plain English: Active vs. Passive](https://www.plainenglish.co.uk/active-and-passive-voice.html)
- [RFC 2119](https://datatracker.ietf.org/doc/html/rfc2119) — key words for requirement levels
- [Vale: Configuration Guide](https://vale.sh/docs/topics/config/) — set up passive voice linting
- [Hemingway Editor](https://hemingwayapp.com/) — online passive voice detector
- [write-good: npm package](https://github.com/btford/write-good) — Node.js tool for catching passive voice
- [LanguageTool: Style Rules](https://languagetool.org/) — open-source grammar checker
- [Grammarly: Passive Voice](https://www.grammarly.com/blog/passive-voice/) — detection and correction
- [ProWritingAid: Style Report](https://prowritingaid.com/) — comprehensive style analysis
- [Flesch-Kincaid Grade Level Calculator](https://www.webfx.com/tools/read-able/) — measure readability

## Next Steps

- [Conciseness & Eliminating Jargon](conciseness.md) — cutting unnecessary words
- [Technical Style Guides](style-guides.md) — applying voice rules across projects
- [Punctuation for Clarity](punctuation.md) — supporting clear active voice with correct punctuation
