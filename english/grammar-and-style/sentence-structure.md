# Sentence Structure for Technical Writing

## Description

Clear sentences are the foundation of good technical documentation. Understanding sentence structure — clauses, modifiers, parallelism, and rhythm — helps you write documentation that readers can parse quickly and act on confidently.

## Prerequisites

- [Why Grammar Matters](intro/why-grammar-matters.md) — the importance of grammar in technical contexts

## Table of Contents

- [The Anatomy of a Sentence](#the-anatomy-of-a-sentence)
- [Simple, Compound, and Complex Sentences](#simple-compound-and-complex-sentences)
- [The Technical Writer's Sentence](#the-technical-writers-sentence)
- [Modifiers and Ambiguity](#modifiers-and-ambiguity)
- [Parallelism](#parallelism)
- [Active Voice vs. Passive Voice](#active-voice-vs-passive-voice)
- [Sentence Length and Readability](#sentence-length-and-readability)
- [Transitions and Flow](#transitions-and-flow)
- [Common Sentence Errors in Technical Writing](#common-sentence-errors-in-technical-writing)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### The Anatomy of a Sentence

Every sentence has at minimum a subject and a verb.

```
Subject  Verb        Object
The API  returns     a JSON response.
```

**Subject** — the thing performing the action:
```text
The compiler translates TypeScript into JavaScript.
The user clicks the Submit button.
```

**Verb** — the action or state:
```text
The server starts on port 8080.
The database contains user records.
```

**Object** — the thing receiving the action:
```text
The function parses the request body.
The script writes logs to stdout.
```

**Clauses** — groups of words with a subject and verb:

- **Independent clause** — can stand alone: "The server starts."
- **Dependent clause** — cannot stand alone: "When the server starts"

```text
Independent:        The deployment succeeded.
Dependent clause:   Before deploying,  [you must run tests.]
                    ^^^^^^^^^^^^^^^^^   ^^^^^^^^^^^^^^^^^^^^^
                    cannot stand alone  independent clause
```

### Simple, Compound, and Complex Sentences

**Simple sentence** — one independent clause:

```text
The function returns a promise.
```

**Compound sentence** — two independent clauses joined by a conjunction:

```text
The function parses the input, and it returns the result.
```

**Complex sentence** — one independent clause + one or more dependent clauses:

```text
When the input is invalid, the function throws an error.
```

**Compound-complex sentence** — two independent clauses + dependent clauses:

```text
When the input is invalid, the function throws an error, and the caller must handle it.
```

For technical writing, favor simple and complex sentences. Compound-complex sentences are harder to parse.

### The Technical Writer's Sentence

Technical documentation has a different rhythm than prose writing.

**Guidelines:**

**One idea per sentence.** Avoid packing multiple instructions into one sentence:

```text
// Poor
Click the Settings button, then select the Account tab, and after that update your email address, then click Save.

// Better
Click the Settings button. Select the Account tab. Update your email address. Click Save.
```

**Start with context.** Place the most important information first:

```text
// Weak
You must restart the server to apply the changes.

// Strong
To apply the changes, restart the server.
```

**Subject before verb before object** — the default English word order:

```text
// Poor word order
Into the database the script inserts records.

// Standard
The script inserts records into the database.
```

**Use second person ("you").** Direct address is clearer than passive constructions:

```text
// Passive, indirect
The configuration file should be edited by the user.

// Active, direct
Edit the configuration file.
```

**Avoid nominalization** — turning verbs into nouns:

```text
// Nominalized (weak)
The implementation of the feature requires the modification of the existing codebase.

// Direct (strong)
Implementing the feature requires modifying the existing codebase.
```

### Modifiers and Ambiguity

Misplaced modifiers are a common source of confusion in technical writing.

**Dangling modifiers:**

```text
// Poor — who is configuring?
After configuring the database, the application is ready.

// Better
After configuring the database, you can run the application.
```

**Misplaced modifiers:**

```text
// Poor — is the error valid or the email?
The user received an error for an invalid email address.

// Better
The user received an error indicating the email address is invalid.
```

**Squinting modifiers** — could apply to either side:

```text
// Poor — does it happen quickly, or does it fix quickly?
Configuring the server quickly resolves the issue.

// Better
Quickly configure the server to resolve the issue.
```

**Only and just** — place them immediately before what they modify:

```text
// Poor
The tool only validates the input file.

// Better
The tool validates only the input file.  (not the output)
The tool only validates the input file.  (does not transform it)
```

### Parallelism

Elements in a list or series must use the same grammatical form.

**Lists with parallel structure:**

```text
// Not parallel
The tool can:
- Parse XML files
- Generates reports
- It validates schemas

// Parallel
The tool can:
- Parse XML files
- Generate reports
- Validate schemas
```

**Verbs in parallel:**

```text
// Not parallel
To deploy, you need to build the project, then test the build, and the final step is uploading.

// Parallel
To deploy, build the project, test the build, and upload the artifacts.
```

**Nouns in parallel:**

```text
// Not parallel
The process involves configuration, testing, and deploying the changes.

// Parallel
The process involves configuring, testing, and deploying the changes.
```

**Bullet list consistency:**

```text
// Not parallel (mix of fragments and full sentences)
Advantages:
- Faster deployment
- It reduces errors
- Easy to maintain

// Parallel (all noun phrases)
Advantages:
- Faster deployment
- Reduced errors
- Easier maintenance
```

### Active Voice vs. Passive Voice

**Active voice** — subject performs the action:

```text
The compiler translates the source code.
```

**Passive voice** — subject receives the action:

```text
The source code is translated by the compiler.
```

**When to use active voice (most of the time):**

```text
// Active (clear who does what)
The installation script creates a configuration file.

// Passive (who creates it?)
A configuration file is created.
```

**When passive voice is acceptable:**

```text
// The actor is unknown or irrelevant
The database connection was closed unexpectedly.

// The result is more important than the actor
The application must be restarted after the update.

// Convention in some scientific/formal writing
The sample was analyzed using spectrometry.
```

In technical documentation, aim for 80-90% active voice sentences.

### Sentence Length and Readability

**Fog Index and readability scores** measure how difficult a text is to understand:

- **Average sentence length:** 15-20 words for technical documentation
- **Maximum sentence length:** 30 words
- **Shortest sentence:** 5-10 words for emphasis or instructions

**Breaking long sentences:**

```text
// Original (42 words)
The authentication middleware validates the JWT token included in the Authorization header of every incoming request by checking its signature against the public key stored in the environment configuration and then attaching the decoded payload to the request object for downstream handlers to access.

// Broken (three sentences, 15-20 words each)
The authentication middleware validates the JWT token in the Authorization header. It checks the token's signature against the public key stored in environment configuration. The middleware then attaches the decoded payload to the request object for downstream handlers.
```

**Varying sentence length:**

```text
Strive for rhythm by varying sentence length. A short sentence after a long one
commands attention. Too many long sentences exhaust readers. Too many short
sentences feel choppy. Mix them deliberately.
```

### Transitions and Flow

Transitions guide readers through your document.

**Sequential transitions:**

```text
First, install the dependencies. Next, configure the database. Then, run the migration script. Finally, start the application.
```

**Causal transitions:**

```text
The server failed to start because the port was occupied. Therefore, change the port number in the configuration file. As a result, the server starts correctly on the next attempt.
```

**Contrasting transitions:**

```text
The batch process works for small datasets. However, for large datasets, the streaming API is more efficient.
```

**Transition words for technical writing:**

| Purpose | Transitions |
|---------|-------------|
| Sequence | First, Next, Then, After, Before, Finally |
| Cause/Effect | Because, Therefore, Consequently, As a result |
| Contrast | However, In contrast, On the other hand |
| Addition | Additionally, Moreover, Furthermore, Also |
| Example | For example, For instance, Specifically |
| Summary | In summary, In short, Overall |

### Common Sentence Errors in Technical Writing

**Run-on sentences:**

```text
// Poor
Install the package run the command check the output.

// Better
Install the package. Then, run the command. Finally, check the output.
```

**Comma splices:**

```text
// Poor
The function returns a promise, it resolves with the user data.

// Better
The function returns a promise that resolves with the user data.
```

**Subject-verb agreement:**

```text
// Incorrect
The list of dependencies are in the package.json file.

// Correct
The list of dependencies is in the package.json file.
```

**Pronoun-antecedent agreement:**

```text
// Incorrect
Every developer should check their syntax before committing. (acceptable in modern style)

// Traditional
Every developer should check their syntax before committing. (singular they is now standard)

// Reword to avoid
Developers should check their syntax before committing.
```

**Sentence fragments:**

```text
// Fragment
The tool parses YAML files. And validates the schema.

// Complete
The tool parses YAML files and validates the schema.
```

## Study Cases

### Case 1: Rewriting a Confusing Installation Guide

```text
// Original
The installation of the application requires the downloading of the latest
release from the official repository and then the extraction of the archive
to a directory of your choice and after that you need to run the setup script
which will check the system dependencies and configure the application.

// Revised — active voice, short sentences
Download the latest release from the official repository. Extract the archive
to a directory of your choice. Then, run the setup script. The script checks
system dependencies and configures the application.
```

### Case 2: Clarifying Ambiguous Error Messages

```text
// Original
Error: Invalid input file.

// Revised — context, action
Error: The input file contains invalid JSON at line 42. Check for missing
commas or unquoted property names. Then, correct the file and run the command
again.
```

### Case 3: Converting Passive Procedures to Active Instructions

```text
// Original (passive, nominalized)
A backup of the database should be made before the migration is performed.
After the migration has been completed, the application should be restarted.
The new schema can then be verified by checking the database logs.

// Revised (active, direct)
Back up the database before running the migration. After the migration
completes, restart the application. Then, check the database logs to verify
the new schema.
```

## Examples

### Example 1: One Sentence, One Action

```text
// Poor
Enter your credentials and click Sign In and then select a project.

// Better
Enter your credentials. Click Sign In. Then, select a project.
```

### Example 2: Active Voice Conversion

```text
// Passive
The report is generated by the system every Monday.

// Active
The system generates the report every Monday.
```

### Example 3: Parallel List Fix

```text
// Before
The tutorial covers: setting up the environment, how to write your first test,
and running the test suite.

// After
The tutorial covers: setting up the environment, writing your first test, and
running the test suite.
```

### Example 4: Dangling Modifier Fix

```text
// Before
After deploying to production, the logs should be monitored.

// After
After deploying to production, monitor the logs.
```

### Example 5: Breaking Up a Run-On

```text
// Before
Click the Add button to create a new project then you can enter the project
name and description and select a template then click Create.

// After
Click the Add button to create a new project. Enter the project name and
description. Select a template. Then, click Create.
```

## Glossary

| Term | Definition |
|------|------------|
| Clause | A group of words containing a subject and a verb |
| Independent clause | A clause that forms a complete sentence on its own |
| Dependent clause | A clause that cannot stand alone as a complete sentence |
| Modifier | A word or phrase that describes another element |
| Dangling modifier | A modifier that does not clearly refer to the intended subject |
| Parallelism | Using the same grammatical form for related elements |
| Nominalization | Turning a verb into a noun (e.g., "implement" → "implementation") |
| Active voice | The subject performs the action |
| Passive voice | The subject receives the action |
| Fog Index | A readability metric based on sentence length and word complexity |
| Comma splice | Joining two independent clauses with only a comma |
| Run-on sentence | Two or more independent clauses without proper punctuation |
| Squinting modifier | A modifier that could apply to either the preceding or following element |
| Transition | A word or phrase that connects ideas between sentences |

## Quick References

- [The Elements of Style, Strunk & White](https://www.penguinrandomhouse.com/books/160938/the-elements-of-style-by-william-strunk-jr/) — classic guide to clear writing
- [Plain English Campaign](https://www.plainenglish.co.uk/) — guides for clear communication
- [Hemingway Editor](https://hemingwayapp.com/) — tool for improving readability
- [Microsoft Style Guide: Sentence Structure](https://learn.microsoft.com/en-us/style-guide/punctuation/sentence-structure)

## Next Steps

- [Punctuation for Clarity](punctuation.md) — using punctuation to make technical sentences clearer
- [Active vs. Passive Voice](active-vs-passive.md) — deeper dive into voice choice
- [Conciseness & Eliminating Jargon](conciseness.md) — writing shorter, clearer sentences
