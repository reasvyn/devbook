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
- [Converting Passive to Active](#converting-passive-to-active)
- [The "By Zombies" Test](#the-by-zombies-test)
- [Passive Voice in Specifications](#passive-voice-in-specifications)
- [Passive Voice in Incident Reports](#passive-voice-in-incident-reports)
- [Voice Consistency Across Documents](#voice-consistency-across-documents)
- [Study Cases](#study-cases)
- [Examples](#examples)
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

## Study Cases

### Case 1: Rewriting a Passive-Heavy Procedure

```text
// Before
A backup of the database should be made. The migration script must be run.
After the migration has been completed, the cache should be cleared.
The application can then be restarted by the user.

// After — active throughout
Back up the database. Run the migration script. After the migration
completes, clear the cache. Then, restart the application.
```

### Case 2: Incident Postmortem Excerpt

```text
// Before — passive obscures root cause
The certificate was not renewed before the expiration date.
As a result, the service was interrupted for 45 minutes.

// After — active clarifies accountability
The operations team did not renew the certificate before the expiration
date. As a result, the service was unavailable for 45 minutes.
```

### Case 3: API Documentation

```text
// Before — passive and indirect
A user object is returned when the ID is provided. The user data can then
be used to fetch the user's posts.

// After — active and direct
The function returns a user object when you provide a user ID. Use the
user data to fetch the user's posts.
```

## Examples

### Example 1: Simple Conversion

```text
Passive: The package is installed by running npm install.
Active:  Run npm install to install the package.
```

### Example 2: Removing Nominalization

```text
Passive, nominalized: The implementation of the new feature was completed.
Active:               The team implemented the new feature.
```

### Example 3: Instruction Clarity

```text
Passive: The form should be submitted after all fields are filled.
Active:  Submit the form after filling all fields.
```

### Example 4: Error Messages

```text
Passive: An error was encountered while processing your request.
Active:  The server encountered an error while processing your request.
```

### Example 5: Keeping Passive (Intentional)

```text
The application was designed to handle 10,000 concurrent users.
(Passive is fine — the actor is irrelevant, the design is the focus.)
```

## Glossary

| Term | Definition |
|------|------------|
| Active voice | The subject performs the action of the verb |
| Passive voice | The subject receives the action of the verb |
| Past participle | The verb form used in passive constructions (e.g., "validated," "created") |
| Actor | The entity performing the action in a sentence |
| Subject | The grammatical entity that the sentence is about |
| Agent | Another term for the actor in a passive sentence |
| Nominalization | Turning a verb into a noun (e.g., "implement" → "implementation") |
| "By zombies" test | Adding "by zombies" to detect passive voice |
| Imperative mood | Command form: "Install the package" |
| RFC 2119 | Standard for requirement keywords: MUST, SHOULD, MAY |
| Depersonalize | Removing the human actor from the description |

## Quick References

- [Microsoft Style Guide: Verbs](https://learn.microsoft.com/en-us/style-guide/grammar/verbs) — active voice guidance
- [Google Style Guide: Active Voice](https://developers.google.com/style/voice) — Google's recommendations
- [Plain English: Active vs. Passive](https://www.plainenglish.co.uk/active-and-passive-voice.html)
- [RFC 2119](https://datatracker.ietf.org/doc/html/rfc2119) — key words for requirement levels

## Next Steps

- [Conciseness & Eliminating Jargon](conciseness.md) — cutting unnecessary words
- [Technical Style Guides](style-guides.md) — applying voice rules across projects
- [Punctuation for Clarity](punctuation.md) — supporting clear active voice with correct punctuation
