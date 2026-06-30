# Basic Grammar Refresher for Developers

## Description

A practical review of English grammar fundamentals for developers who write documentation, code comments, commit messages, and technical communication. Covers parts of speech, sentence structure, clauses, phrases, and subject-verb agreement with examples drawn from software development.

## Prerequisites

None. This is a foundational reference.

## Table of Contents

- [Parts of Speech](#parts-of-speech)
- [Subjects and Predicates](#subjects-and-predicates)
- [Clauses](#clauses)
- [Phrases](#phrases)
- [Subject-Verb Agreement](#subject-verb-agreement)
- [Glossary](#glossary)
- [Next Steps](#next-steps)

## Content / Material

### Parts of Speech

Every word in English belongs to one of eight categories called parts of speech. Understanding these categories helps you write clearer documentation and debug grammatical errors in your own writing.

#### Nouns

A noun names a person, place, thing, or idea. In code, nouns appear everywhere: variable names, class names, function names, and API endpoints.

**Common nouns** name general items:

```text
server, database, function, array, error, request, response
```

**Proper nouns** name specific entities and are capitalized:

```text
TypeScript, PostgreSQL, AWS, Linux, React, GitHub Actions
```

**Abstract nouns** name concepts or ideas:

```text
performance, latency, reliability, abstraction, recursion, concurrency
```

**Concrete nouns** name physical things:

```text
cable, server rack, monitor, keyboard, power supply
```

Variable names are typically nouns or noun phrases. Class names in most languages follow PascalCase and are singular nouns:

```java
public class UserRepository { ... }
public class PaymentGateway { ... }
```

Function names often combine verbs with nouns to describe actions:

```text
fetchUserData, validateInput, parseResponse, createConnection
```

**Countable vs. uncountable nouns** affect article and quantifier choice:

| Countable | Uncountable |
|-----------|-------------|
| three servers | some bandwidth |
| several errors | less memory |
| a database | information |
| many requests | much overhead |

A common mistake: "less errors" should be "fewer errors" because errors are countable.

#### Pronouns

A pronoun replaces a noun to avoid repetition. The noun it replaces is called the antecedent.

**Personal pronouns:**

| Person | Subject | Object | Possessive |
|--------|---------|--------|------------|
| 1st singular | I | me | my/mine |
| 2nd singular | you | you | your/yours |
| 3rd singular | he/she/it | him/her/it | his/her/its |
| 1st plural | we | us | our/ours |
| 2nd plural | you | you | your/yours |
| 3rd plural | they | them | their/theirs |

**Possessive pronouns** show ownership. Note: "its" (possessive) has no apostrophe. "It's" means "it is."

```text
// Correct
The function returns its result as a JSON object.

// Wrong — "it's" means "it is"
The function returns it's result as a JSON object.
```

**Relative pronouns** introduce relative clauses:

```text
who, whom, which, that, whose
```

```text
The developer who wrote this code used a factory pattern.
The library that we migrated to has better type support.
```

**Clear antecedents are critical in technical writing.** An ambiguous pronoun confuses the reader:

```text
// Ambiguous — what does "it" refer to?
Extract the archive and run the installer. It may take a few minutes.

// Clear
Extract the archive and run the installer. The installation may take a few minutes.
```

#### Verbs

A verb describes an action, occurrence, or state of being. Verbs are the engine of every sentence.

**Action verbs** describe specific actions:

```text
compile, deploy, parse, validate, execute, transform, merge, deploy
```

```text
The compiler translates TypeScript into JavaScript.
The deployment pipeline runs the test suite.
```

**Linking verbs** connect the subject to a description:

```text
be (is, am, are, was, were), become, seem, appear, remain
```

```text
The system is stateless.
The database remains available during the migration.
```

**Auxiliary verbs (helping verbs)** combine with main verbs to express tense, mood, or voice:

```text
be, have, do, will, shall, may, can, must, would, should, could, might
```

```text
The service has been deployed.
You should configure the environment variables.
The request must include an API key.
```

**Modal auxiliary verbs** express necessity, possibility, permission, or obligation:

```text
must    — required   ("You must set a timeout.")
should  — recommended ("You should use prepared statements.")
may     — permitted  ("You may override the default port.")
can     — possible   ("You can check the logs.")
will    — certain    ("The server will restart.")
might   — possible   ("The connection might fail.")
```

These are essential in documentation to communicate the level of requirement precisely.

**Imperative mood** uses the base form of the verb for commands and instructions:

```text
// Verb used in imperative mood
Install the dependencies.
Run the migration script.
Click Save.
```

Commit messages follow the imperative mood convention:

```text
Fix memory leak in connection pool
Add rate limiting to authentication endpoint
Remove deprecated API routes
Update dependency versions
```

#### Adjectives

An adjective modifies a noun, providing more detail. In technical writing, precise adjectives prevent ambiguity:

```text
asynchronous operation, synchronous request, nullable field, immutable object, eager loading, lazy evaluation, recursive function, distributed system, monolithic application, stateless protocol
```

**Comparative adjectives** compare two things ("faster," "more efficient"). **Superlative adjectives** compare three or more ("fastest," "most efficient").

```text
The in-memory cache is faster than the disk-based cache.
Redis is one of the most popular caching solutions.
```

**Avoid vague adjectives** in documentation:

```text
// Vague
The system has good performance.

// Specific
The system handles 10,000 requests per second with a 99th percentile latency under 50ms.
```

#### Adverbs

An adverb modifies a verb, an adjective, or another adverb. Many adverbs end in -ly.

```text
The system runs continuously.          (modifies "runs")
The query returns remarkably fast.     (modifies "fast")
The service was partially deployed.    (modifies "deployed")
```

**Adverb placement** affects clarity:

```text
// Ambiguous — does "only" modify "validates" or "input"?
The tool only validates the input file.

// Clear
The tool validates only the input file.     (not the output file)
The tool only validates the input file.     (does not transform it)
```

**Common adverbs in technical writing:** automatically, manually, recursively, synchronously, asynchronously, periodically, incrementally, conditionally, permanently.

Prefer specific measures over vague adverbs:

```text
// Vague
The process runs quickly.

// Specific
The process completes in under 200 milliseconds.
```

#### Prepositions

A preposition shows the relationship between a noun (or pronoun) and another word in the sentence. Prepositions form prepositional phrases.

```text
in, on, at, to, from, by, with, for, about, through, during, without, between, among, under, over, across, after, before, against, within, upon, via
```

**Common technical prepositional phrases:**

```text
access to, based on, associated with, mapped to, derived from, identical to,
compatible with, responsible for, dependent on, independent of, similar to
```

```text
You need access to the production logs.
The recommendation is based on usage patterns.
This error is associated with a missing configuration file.
The deployment is dependent on the CI pipeline passing.
The session token is derived from the user credentials.
```

**Preposition choice errors are common among non-native speakers:**

```text
// Incorrect
Compare with the benchmark results.   (depends on meaning)

// Use "compare to" for likeness, "compare with" for analysis
The new algorithm compares favorably to the old one.
Compare these results with the baseline.

// Incorrect
Based off the analysis, we decided to refactor.

// Correct
Based on the analysis, we decided to refactor.
```

#### Conjunctions

A conjunction connects words, phrases, or clauses.

**Coordinating conjunctions** (for, and, nor, but, or, yet, so — FANBOYS) connect equal elements:

```text
Install Python and configure the environment.
The server is down, but the database is still running.
```

**Subordinating conjunctions** (because, although, while, when, if, unless, since, after, before, until, as, whereas) connect a dependent clause to an independent clause:

```text
Because the API is rate-limited, batch your requests.
When the migration completes, restart the application.
Unless you configure SSL, the connection will not be encrypted.
```

**Correlative conjunctions** work in pairs (either...or, neither...nor, both...and, not only...but also, whether...or):

```text
The endpoint accepts either JSON or XML.
The update affects both the frontend and the backend.
If the input is invalid, return a 400 status code.
```

#### Interjections

Interjections express emotion or reaction: `oh`, `wow`, `oops`, `hey`, `ugh`. They are rarely appropriate in technical writing, documentation, or code comments. Omit them entirely from professional writing.

### Subjects and Predicates

Every complete sentence has two parts: a subject (what the sentence is about) and a predicate (what is said about the subject).

#### Complete Subject vs. Simple Subject

The **complete subject** includes the noun and all its modifiers. The **simple subject** is the core noun. Similarly, the **complete predicate** includes the verb and all modifiers and objects, while the **simple predicate** is the verb or verb phrase.

```text
Complete subject:      The database connection pool     Complete predicate: is exhausted.
Simple subject:        pool                              Simple predicate:   is exhausted.

Complete subject:      Every Node.js application in this repository
Simple subject:        application
Predicate:             uses Express middleware.

// Simple vs complete predicate
Simple predicate:      deploys
Complete predicate:    deploys the application to Kubernetes every night.
```

#### Compound Subjects and Predicates

A **compound subject** has two or more subjects sharing the same verb:

```text
The frontend and the backend are deployed separately.
Logging, monitoring, and alerting are handled by the DevOps team.
```

A **compound predicate** has two or more verbs sharing the same subject:

```text
The script reads the configuration file and applies the changes.
The API validates the request, processes the data, and returns a response.
```

#### Clear Subjects and the Implied "You"

Ambiguous subjects confuse readers. Imperative sentences (commands and instructions) have an implied subject: "you." Use the imperative to make instructions direct:

```text
// Ambiguous — who should configure?
The database should be configured before running the application.

// Clear — implied subject "you"
Configure the database before running the application.

// Standard imperative form
[You] Install the dependencies.
[You] Run npm start.
[You] Open the configuration file.
```

```python
# Build the Docker image and tag it with the commit hash.
```

### Clauses

A clause is a group of words containing a subject and a verb. Clauses are the building blocks of sentences.

#### Independent Clause

An independent clause (main clause) can stand alone as a complete sentence.

```text
The server starts.                                    (one independent clause)
The deployment succeeded, and the service is live.    (two independent clauses)
```

#### Dependent Clause

A dependent clause (subordinate clause) has a subject and a verb but cannot stand alone. It depends on an independent clause for its meaning.

```text
Dependent:      When the server starts         Complete: When the server starts, it loads the config.
Dependent:      Because the database is down   Complete: Because the database is down, the API returns 503.
```

**Noun clauses** act as nouns: "What the function returns depends on the input."

**Adjective clauses** modify nouns: "The library that we replaced had a memory leak."

**Adverb clauses** modify verbs, expressing condition, time, or reason:

```text
If the cache is empty, fetch from the database.         (condition)
After the build completes, deploy the artifacts.         (time)
Because the query was slow, we added an index.           (reason)
```

#### Relative Clauses: Restrictive vs. Non-Restrictive

A relative clause begins with a relative pronoun (who, whom, which, that, whose). The distinction between restrictive and non-restrictive clauses is critical for precise documentation.

**Restrictive (essential)** — narrows down which thing. Use `that` (or `who` for people). No commas.

```text
The method that handles authentication is in a separate module.
The developer who wrote this test is on the platform team.
```

**Non-restrictive (non-essential)** — adds extra information. Use `which`. Set off with commas.

```text
The authentication module, which was rewritten last quarter, now supports OAuth.
```

```text
// Restrictive — only features that are deprecated will be removed
The features that are deprecated will be removed in version 3.

// Non-restrictive — all features will be removed (they happen to be deprecated)
The features, which are deprecated, will be removed in version 3.
```

#### How Clause Structure Affects Documentation Clarity

Place the most important information in the independent clause. Put conditions in dependent clauses:

```text
// Weak — main idea buried in dependent clause
Because the deployment failed when the configuration was invalid, we added validation.

// Strong — main idea in independent clause
We added configuration validation because the deployment failed when the configuration was invalid.
```

Vary clause types to avoid monotony. Mix independent and dependent clauses for better rhythm.

#### Run-On Sentences and Comma Splices

A **run-on sentence** joins independent clauses without proper punctuation. A **comma splice** joins them with only a comma. Both are common in quick communication but should be avoided in documentation.

```text
// Run-on
Install the package run the command check the output.

// Comma splice
The server is down, we are investigating.

// Correct
Install the package. Run the command. Check the output.
The server is down; we are investigating.
```

### Phrases

A phrase is a group of related words without a subject-verb pair. Phrases add detail to sentences.

#### Noun Phrases

A noun phrase consists of a noun and its modifiers:

```text
the configuration file
a high-performance caching layer
the newly deployed microservice
```

Noun phrases are common in variable and function naming:

```text
maxConnectionRetries, defaultErrorMessage, userAuthenticationToken
```

#### Verb Phrases

A verb phrase consists of the main verb and its auxiliary verbs:

```text
has been deployed
should have been tested
will be running
is being processed
```

```text
The deployment has been completed.
The tests should have been run before merging.
```

#### Prepositional Phrases

A prepositional phrase begins with a preposition and ends with a noun or pronoun. It functions as either an adjective or an adverb.

**Adjective function** (modifies a noun):

```text
The variable in the global scope is accessible everywhere.
The code with the security patch must be deployed immediately.
```

**Adverb function** (modifies a verb):

```text
Save the file before running the test.
The application runs on port 3000.
Access the database through the connection pool.
```

#### Participial Phrases

A participial phrase begins with a present participle (-ing) or past participle (-ed, -en, -t) and acts as an adjective.

```text
Running on port 8080, the server is ready for requests.
Exhausted by the migration, the database needs a restart.
Built with TypeScript, the codebase is type-safe.
```

**Dangling modifiers** occur when the participial phrase does not logically refer to the subject of the main clause. This is one of the most common grammatical errors in technical writing.

```text
// Dangling — the database did not run the migration
After running the migration, the database was restarted.

// Correct
After running the migration, restart the database.
```

#### Infinitive and Gerund Phrases

**Infinitive phrases** begin with `to` + verb. They can serve as nouns, adjectives, or adverbs:

```text
To optimize performance is our priority.            (noun — subject)
We have a plan to migrate the database.             (adjective — modifies "plan")
To deploy the application, run the deploy script.   (adverb — explains purpose)
```

**Gerund phrases** begin with a verb ending in -ing and function as nouns. Distinguish them from present participles: a gerund acts as a noun; a participle acts as an adjective or forms a continuous tense.

```text
// Gerund (noun)
Testing is essential before deployment.

// Present participle (adjective)
The testing environment is ready.

// Present participle (continuous verb)
The system is testing the connection.

// More gerund examples
Refactoring the legacy code took three sprints.
The team prefers writing tests before implementation.
```

### Subject-Verb Agreement

A verb must agree with its subject in number. Singular subjects take singular verbs; plural subjects take plural verbs.

#### Basic Rules

```text
// Singular
The application runs on port 3000.
The error message is misleading.

// Plural
The applications run on different ports.
The error messages are misleading.
```

#### Collective Nouns

A collective noun (team, committee, group, family, audience, staff) can be singular or plural depending on whether the group acts as a unit or as individuals.

In technical contexts, collective nouns are most often singular:

```text
The development team is meeting at 2 PM.
The user base has grown by 20%.
```

**Data:** Traditionally the plural of "datum," but in modern technical writing "data" is widely accepted as singular. Be consistent within a document.

```text
// Both acceptable — choose one and stay consistent
The data is stored in S3.
The data are stored in S3.
```

#### Indefinite Pronouns

Indefinite pronouns like everyone, everybody, everything, each, nobody, nothing, anyone, anything, someone, something are singular:

```text
Everyone on the team has access to the repository.
Each of the services runs in its own container.
Nobody in the group knows the root cause.
```

**None** can be singular or plural depending on context:

```text
None of the code is ready for review.        (singular — not one unit)
None of the tests are passing.               (plural — not any of them)
```

**Either** and **neither** are singular:

```text
Either of the approaches is valid.
Neither of the solutions meets the requirements.
```

#### Subjects Separated from Verbs by Prepositional Phrases

The verb must agree with the subject, not the object of a prepositional phrase that separates them:

```text
// The subject is "list," not "dependencies"
The list of dependencies is in the package.json.

// The subject is "version," not "features"
The latest version of the features is in the main branch.

// The subject is "collection," not "APIs"
A collection of REST APIs provides access to the data.
```

To find the true subject, mentally remove the prepositional phrase:

```text
The list [of dependencies] is in the package.json.
The latest version [of the features] is in the main branch.
```

#### Agreement with Compound Subjects

Compound subjects joined by **and** take a plural verb:

```text
The frontend and the backend are deployed separately.
The CEO and the CTO approve all architectural decisions.
```

Exception: if the two nouns form a single concept, use singular:

```text
Docker and containerization is the topic of the workshop.   (one topic)
```

Compound subjects joined by **or** or **nor** — the verb agrees with the subject closest to it:

```text
The application or the database is causing the issue.
The database or the application caches are causing the issue.
Neither the developer nor the reviewers noticed the bug.
Neither the reviewers nor the developer noticed the bug.
```

With **either...or** and **neither...nor**, the same proximity rule applies:

```text
Either the frontend or the backend is responsible for authentication.
Either the frontend or the APIs are responsible for authentication.
```

#### Agreement with There Is / There Are

The verb agrees with the noun that follows "there is" or "there are":

```text
There is a configuration file in the root directory.      (singular)
There are multiple configuration files in the directory.  (plural)
There is a database and a cache server running.           (singular, first noun)
There are a database and a cache server running.          (plural, less common)
```

Use "there is" before singular count nouns and uncountable nouns. Use "there are" before plural count nouns.

```text
There is enough disk space for the deployment.
There are enough replicas to handle the load.
```

#### Special Cases

Some nouns that look plural are treated as singular:

**Fields ending in -ics** (mathematics, physics, economics, statistics, ethics) take singular verbs when referring to the field as a whole, plural when referring to specific applications:

```text
Statistics is essential for data analysis.                    (field)
The statistics for this quarter show improved performance.    (specific numbers)
```

**Series, species, means** can be singular or plural depending on context:

```text
A series of tests is scheduled for this weekend.              (singular)
Multiple species of malware were detected.                     (plural)
```

**Titles of works** take singular verbs even if they contain plural words:

```text
The Twelve-Factor App is a methodology for building SaaS applications.
Clean Code remains a popular reference for developers.
```

**Measured quantities** (time, distance, money, weight) take singular verbs when considered as a unit:

```text
Five hundred milliseconds is the timeout limit.
Ten gigabytes of data is stored on each node.
Fifty dollars is the monthly hosting cost.
```

**Each** and **every** before compound subjects require a singular verb:

```text
Each server and each database is monitored independently.
Every function and every class in this module follows the same pattern.
```

## Glossary

| Term | Definition |
|------|------------|
| Adjective | A word that modifies a noun |
| Adverb | A word that modifies a verb, adjective, or another adverb |
| Antecedent | The noun a pronoun refers to |
| Clause | A group of words containing a subject and a verb |
| Comma splice | Joining two independent clauses with only a comma |
| Common noun | A general noun (server, database, error) |
| Complete predicate | The verb and all its modifiers and objects |
| Complete subject | The noun and all its modifiers |
| Compound predicate | Two or more predicates sharing the same subject |
| Compound subject | Two or more subjects sharing the same verb |
| Conjunction | A word connecting words, phrases, or clauses |
| Dangling modifier | A modifier with no clear subject to modify |
| Dependent clause | A clause that cannot stand alone as a sentence |
| Gerund | A verb form ending in -ing functioning as a noun |
| Imperative mood | Verb form for commands ("Run the tests.") |
| Independent clause | A clause forming a complete sentence on its own |
| Indefinite pronoun | everyone, each, nobody, nothing, anything |
| Infinitive | The base form of a verb preceded by "to" |
| Linking verb | A verb connecting subject to description (is, seem, become) |
| Modal verb | An auxiliary verb expressing necessity or possibility |
| Modifier | A word or phrase that describes another element |
| Non-restrictive clause | A non-essential clause set off by commas, uses "which" |
| Noun | A word naming a person, place, thing, or idea |
| Object | The noun receiving the action of the verb |
| Participial phrase | A participle phrase acting as an adjective |
| Phrase | A group of related words without a subject-verb pair |
| Predicate | What the subject does or is |
| Preposition | A word showing relationship (in, on, at, to, from, with) |
| Prepositional phrase | A preposition followed by a noun |
| Pronoun | A word replacing a noun (he, she, it, they) |
| Proper noun | A specific noun requiring capitalization (TypeScript, AWS) |
| Relative clause | A clause introduced by who, which, that |
| Restrictive clause | An essential clause using "that," no commas |
| Run-on sentence | Independent clauses joined without punctuation |
| Simple predicate | The main verb or verb phrase |
| Simple subject | The core noun or pronoun |
| Subject | The noun performing the action or being described |
| Subject-verb agreement | Subject and verb must match in number |
| Verb | A word describing an action, occurrence, or state |

## Next Steps

- [Verb Tenses in Technical Writing](verb-tenses-in-technical-writing.md) — choosing the right tense for documentation, comments, and commit messages
- [Common Grammar Pitfalls for Developers](common-grammar-pitfalls-for-developers.md) — frequent errors and how to avoid them
- [Sentence Structure for Technical Writing](sentence-structure.md) — crafting clear, readable sentences with proper clause structure
