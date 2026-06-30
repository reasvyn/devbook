# Common Grammar Pitfalls for Developers

## Description

Developers write every day — documentation, code comments, commit messages, pull requests, and emails. Small grammar mistakes undermine clarity and credibility. This reference covers the most frequent English grammar errors in technical writing, with concrete examples from real developer contexts.

## Prerequisites

- [Basic Grammar Refresher for Developers](basic-grammar-refresher.md) — understanding parts of speech
- [Verb Tenses in Technical Writing](verb-tenses-in-technical-writing.md) — understanding the verb system

## Table of Contents

- [Articles: A, An, The](#articles-a-an-the)
- [Prepositions](#prepositions)
- [Modal Verbs — RFC 2119 and Beyond](#modal-verbs--rfc-2119-and-beyond)
- [Countable vs Uncountable Nouns](#countable-vs-uncountable-nouns)
- [Dangling and Misplaced Modifiers](#dangling-and-misplaced-modifiers)
- [Parallel Structure](#parallel-structure)
- [Common Punctuation Errors](#common-punctuation-errors)
- [Subject-Verb Agreement Pitfalls](#subject-verb-agreement-pitfalls)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Articles: A, An, The

Article errors are the most common grammar mistake among non-native English speakers in technical contexts. English has three article forms: **a**, **an**, **the**, and the **zero article** (no article).

#### A vs An — Sound, Not Spelling

Choose **a** before consonant sounds and **an** before vowel sounds. The spoken sound after the article determines the choice, not the written letter.

```text
// Consonant sounds — use "a"
a user          (/j/ sound)
a one-to-one mapping  (/w/ sound)
a URL           (/j/ sound)
a Python script

// Vowel sounds — use "an"
an API          (/æ/ sound)
an HTTP request (/eɪtʃ/ sound)
an SQL query    (/ɛs/ sound)
an 8-byte block (/eɪ/ sound)
```

Common mistakes:

```text
// Wrong              // Right
a API endpoint        an API endpoint
a HTTP request        an HTTP request
a SQL injection       an SQL injection   (if pronouncing "ess-queue-ell")
```

#### Definite vs Indefinite in Technical Contexts

**Indefinite (a/an)** introduces something new or non-specific:

```text
A database connection pool improves performance.
Use a try-catch block to handle errors.
```

**Definite (the)** refers to something specific, already introduced, or uniquely identifiable:

```text
Create a config file. The config file must be in YAML format.
The MySQL driver is installed.
The first argument is the path.     // ordinals take "the"
```

#### Zero Article (No Article)

Many technical concepts take no article when speaking about them in general. The zero article applies to uncountable nouns, abstract concepts, plural count nouns in general statements, programming languages, and technologies — when speaking generally.

```text
// General (no article)                 // Specific (with "the")
Information is stored in the database.   The information in this table is encrypted.
Documentation should be thorough.        The documentation for this API is incomplete.
Performance matters.                     The performance of the new system is poor.

Docker containers are lightweight.       The Docker containers in this project are misconfigured.
APIs should be versioned.                The APIs we expose must be backward-compatible.

Python is dynamically typed.             The Python interpreter handles this.
Docker simplifies deployment.            The Docker Compose file defines services.
HTTP is stateless.                       The HTTP protocol defines status codes.
```

#### Common Article Errors

| Error | Correction |
|---|---|
| `The data shows that...` | `Data shows that...` (see Countable section) |
| `The access to the API is restricted` | `Access to the API is restricted` |
| `The Node.js is a runtime` | `Node.js is a runtime` |
| `I have a experience with Docker` | `I have experience with Docker` |
| `The life is short` | `Life is short` |

### Prepositions

Prepositions connect nouns and verbs to other parts of a sentence. They are notoriously irregular in English and a major source of errors for non-native speakers.

#### Common Technical Preposition Pairs

```text
// Wrong                    // Right
access of the database      access to the database
based off of the results    based on the results
based from React            based on React
consists with three tiers   consists of three tiers
different than JS           different from JS       (preferred)
```

**compare to** vs **compare with**:

```text
The compiler can be compared to a translator.      // likeness (different things)
Compared with Python, Go has faster execution.     // analysis (similar things)
```

**correspond to** vs **correspond with**:

```text
Each key corresponds to a value in the map.        // match / equivalent
```

#### Prepositions in Error Messages and Commands

Modern style guides prefer omitting redundant prepositions:

```text
// Older                    // Modern
click on the button         click the button
connect together            connect
shrink down                 shrink
off of the list             off the list
```

#### Preposition Stranding

Ending a sentence with a preposition is grammatically acceptable. The old rule against it does not apply to English.

```text
// Natural — stranded
Which server is the request going to?
What error are you looking at?
The table the data is stored in.

// Overly formal — same meaning
To which server is the request going?
At what error are you looking?
```

#### Common Errors by Language Background

**for** vs **since** with time:

```text
I have been coding for three years.         // duration
I have been coding since 2023.              // starting point
```

**in** vs **on** vs **at** with time and place:

```text
// Time
at 3:00 PM          on Monday           in March
on Monday morning   in the morning

// Place
at the terminal     on the server       in the container
```

#### Preposition + Technical Action Reference

```text
install on       Install the package on the server.
deploy to        Deploy the application to production.
connect to       Connect to the database.
log in to        Log in to the dashboard.       (not "login to")
run on           The service runs on port 3000.
write to         Write the output to stdout.
read from        Read the input from stdin.
compare with     Compare the output with the expected result.
```

### Modal Verbs — RFC 2119 and Beyond

Modal verbs express necessity, possibility, permission, and obligation. In technical writing — especially specifications — their precise meaning is critical.

#### RFC 2119 Keywords

RFC 2119 defines precise meanings for modal verbs in specifications. Use these consistently in requirements.

| Keyword | Meaning | Example |
|---|---|---|
| MUST / MUST NOT | Absolute requirement | `The server MUST return 200 on success.` |
| SHOULD / SHOULD NOT | Recommendation | `The client SHOULD retry after timeout.` |
| MAY / OPTIONAL | Truly optional | `The client MAY include an ETag header.` |
| SHALL | Historical (use MUST instead) | Prefer `MUST` over `shall` |

#### Can vs May vs Might

- **can** — ability or possibility
- **may** — permission or possibility (ambiguous)
- **might** — weaker possibility

```text
// Ability
You can override the default config with a local file.

// Permission
You may submit a PR after the tests pass.

// Possibility (strong)
The cache may improve performance by 40%.

// Possibility (weak)
This change might introduce a breaking change.
```

**May** is often ambiguous between permission and possibility. Rephrase when clarity matters:

```text
// Ambiguous
The user may delete the record.

// Clear — permission
The user is allowed to delete the record.

// Clear — possibility
The user might delete the record accidentally.
```

#### Will vs Would

- **will** — definite intention or certainty
- **would** — hypothetical, conditional, or polite

```text
The build will fail if tests do not pass.          // certain
The build would fail if we removed the type check. // conditional
Would you mind rebasing this branch?                // polite
We will deprecate this endpoint in v2.              // intention
```

#### Modal Usage in Documentation

```text
// Vague error message
You cannot use this feature.

// Precise
This feature requires a premium license.

// Inconsistent instructions
You must enter a name. You can also add a description. You might want to save.

// Consistent
Enter a name. Add an optional description. Save the form.

// Specification (Kubernetes API convention)
The request body MUST be valid JSON.
The response SHOULD include a Retry-After header.
The client MAY specify a timeout.
```

### Countable vs Uncountable Nouns

English divides nouns into **countable** (can be counted: `one bug`, `two bugs`) and **uncountable** (cannot be counted: `information`). Uncountable nouns take singular verbs and cannot use **a/an** or plural forms.

#### Individual Terms

**Data** — traditionally plural, now commonly singular in computing. Be consistent within a document:

```text
The data are stored.      // traditional — plural
The data is stored.       // modern — singular (acceptable)
```

**Information, documentation, feedback, knowledge** — always uncountable:

```text
// Wrong                     // Right
many informations            much information / a lot of information
many documentations          much documentation
many feedbacks               a lot of feedback
```

**Code** — uncountable as a concept, countable when referring to lines or status codes:

```text
This code is well written.              // uncountable concept
The codebase has 50,000 lines of code.  // countable — lines
Each status code represents an outcome. // countable — codes
```

#### Less vs Fewer

- **less** — with uncountable nouns (`less memory`, `less bandwidth`, `less latency`)
- **fewer** — with countable plural nouns (`fewer bugs`, `fewer dependencies`, `fewer allocations`)

```text
// Wrong                          // Right
This uses less allocations.       This uses fewer allocations.
The new algorithm uses less CPU cycles.   fewer CPU cycles
```

Exception: use **less than** with measurements (`less than 10 ms`, `less than 100 req/s`).

#### Amount vs Number

- **amount** — with uncountable nouns (`amount of traffic`, `amount of memory`)
- **number** — with countable plural nouns (`number of requests`, `number of users`)

```text
The amount of errors decreased.           // Wrong
The number of errors decreased.           // Right
```

#### Much vs Many

- **much** — with uncountable nouns (`much documentation`, `much overhead`)
- **many** — with countable plural nouns (`many features`, `many endpoints`)

In positive statements, **a lot of** sounds more natural than **much**: `There is a lot of documentation.`

#### Quick Reference Table

| Term | Countable? | Example |
|---|---|---|
| data | both (be consistent) | `The data is/are ready` |
| information | no | `The information is accurate` |
| code | no (concept); yes (lines, status) | `code is clean`, `100 codes` |
| documentation | no | `documentation is thorough` |
| feedback | no | `Feedback is welcome` |
| knowledge | no | `Knowledge of SQL helps` |
| software | no | `software is installed` |
| hardware | no | `hardware is expensive` |
| traffic | no | `traffic is increasing` |
| bandwidth | no | `bandwidth is limited` |
| latency | no | `latency is low` |
| bug | yes | `fewer bugs` |
| feature | yes | `many features` |
| request | yes | `100 requests per second` |

### Dangling and Misplaced Modifiers

A **dangling modifier** modifies something not clearly stated in the sentence. A **misplaced modifier** is positioned so it modifies the wrong word.

#### Dangling Participial Phrases

Participial phrases (starting with -ing or -ed verbs) must logically modify the subject of the main clause. When the subject does not match, the modifier dangles.

```text
// Dangling — who ran the tests?           // Fixed
Running the tests, the bug was found.       Running the tests, we found the bug.
After configuring the server, the app was deployed.  ...the team deployed the app.
Using the wrong port, the connection failed.   ...the client failed to connect.
When deploying, the database must be migrated.  When deploying, migrate the database.
```

Imperative sentences (commands) have an implied **you** subject, so they naturally avoid dangling modifiers:

```text
Before deploying, migrate the database.     // correct — implied "you"
When configuring the server, use port 443.  // correct
```

#### Only, Just, Almost, Nearly — Placement Matters

These modifiers attach to the word immediately following them. Moving them changes the meaning:

```text
Only the server logs errors.          // No one else logs errors
The server only logs errors.          // It does nothing else with errors
The server logs only errors.          // It does not log warnings

// Documentation example
The function only returns the first element.       // (implies it does nothing else — wrong intention)
The function returns only the first element.       // (implies it ignores the rest — correct)

Just run the test.                    // Run it immediately
Run just the test.                    // Run only the test, nothing else
```

#### Squinting Modifiers

A **squinting modifier** could modify either the preceding or following element, creating ambiguity:

```text
// Squinting — what is "quickly" modifying?
Running the tests quickly reveals bugs.

// Clear
Quickly running the tests reveals bugs.             // fast execution
Running the tests quickly reveals bugs.             // fast bug discovery

// Squinting
Developers who deploy frequently cause outages.

// Clear
Developers who deploy frequently cause outages.     // frequent deployers
Developers who deploy cause frequent outages.       // frequent outages
```

### Parallel Structure

**Parallel structure** means using the same grammatical form for related elements in a list, comparison, or pair. Violations are common in bullet lists, instructions, and comparisons.

#### Lists in Documentation

Every item in a bullet or numbered list must share the same grammatical form — all nouns, all imperatives, all full sentences, or all noun phrases.

```text
// Not parallel
To configure the server:
- Setting the port number
- You should enable logging
- A firewall must be configured

// Parallel — all imperatives
To configure the server:
- Set the port number
- Enable logging
- Configure the firewall

// Not parallel
The framework supports:
- Building REST APIs
- You can add middleware
- Websocket support

// Parallel
The framework supports:
- Building REST APIs
- Adding middleware
- Supporting WebSockets
```

#### Either...Or, Neither...Nor, Not Only...But Also

The elements after each part of these correlative conjunctions must be grammatically parallel:

```text
// Not parallel                        // Parallel
either an API key or you need a token  either an API key or a bearer token
not only improves but also it reduces  not only improves but also reduces
neither valid JSON nor is it XML       neither valid JSON nor valid XML
```

#### Code Comments with Parallel Structure

```text
// Not parallel                   // Parallel
// 1. Parses the input            // 1. Parses the input
// 2. Validation of the format    // 2. Validates the format
// 3- Sends a response            // 3. Sends a response
```

**Checklist:** Remove list markers and read items as a continuous sentence. If the grammar pattern shifts, restructure.

### Common Punctuation Errors

#### Comma Splices

A **comma splice** joins two independent clauses with only a comma. Fix with a period, semicolon, or conjunction.

```text
// Comma splice                      // Fixed
The server is down, we are fixing.    The server is down. We are fixing.
Install the dependency, then build.   Install the dependency, and then build.
The API returns JSON, the client parses it.   The API returns JSON; the client parses it.
```

#### Semicolons in Code vs Prose

In prose, semicolons join related independent clauses. In technical writing, prefer periods — shorter sentences improve readability.

```text
// Prose — semicolon
The database uses replication; the primary handles writes.

// Better for docs
The database uses replication. The primary handles writes.

// Code — semicolons terminate statements
const db = new Database();
db.connect();
```

#### Apostrophes in Acronyms and Plurals

Acronym plurals **never** take an apostrophe. The apostrophe is for possession only.

```text
// Plural (no apostrophe)            // Possessive (apostrophe)
APIs are versioned                    The API's documentation is outdated.
URLs must be encoded                  The URL's path is invalid.
CPUs are 64-bit                       The CPU's clock speed is 4 GHz.
multiple SDKs                         // not SDK's
the 1990s                             // not 1990's
```

#### Hyphens in Compound Modifiers

When two or more words work as a single adjective before a noun, hyphenate them. Do not hyphenate after the noun.

```text
// Before noun (hyphenate)            // After noun (no hyphen)
user-friendly interface               The interface is user friendly.
real-time system                      The system operates in real time.
high-performance computing            The computing is high performance.
mission-critical application          The application is mission critical.
third-party library                   The library is third party.

// Technical compound modifiers
end-to-end encryption
command-line interface
object-oriented programming
open-source software
single-threaded execution
peer-to-peer network
machine-learning model
32-bit architecture
```

No hyphen with adverbs ending in **-ly**: `highly available system`, `widely used framework`, `fully qualified domain`.

### Subject-Verb Agreement Pitfalls

A verb must agree in number with its subject. Errors happen when other words come between the subject and the verb.

#### Intervening Phrases

Phrases between the subject and verb do NOT change the number of the subject. The verb agrees with the subject, not the noun in the intervening phrase.

```text
// Correct                              // Wrong
The set of features includes auth.      The set of features include auth.
The array of options is processed.      The array of options are processed.
The list of dependencies needs update.  The list of dependencies need update.
The cluster of servers is load-balanced.
One of the endpoints returns 404.       // "one" is singular
```

#### Collective Nouns with Technical Teams

In American English (standard for most technical documentation), treat collective nouns as singular — the group acting as a unit.

```text
The team has decided to use TypeScript.
The DevOps team maintains the CI/CD pipeline.
The company is migrating to the cloud.
```

#### Indefinite Pronouns

**Each, every, anyone, nobody, someone** are always singular.

```text
Each of the endpoints handles a specific resource.
Every module has its own test suite.
Anyone who submits a PR must sign the CLA.

// Wrong
Each of the endpoints handle a specific resource.
```

#### There Is vs There Are

With compound subjects, the verb agrees with the **first** noun after the verb:

```text
There is a bug and several warnings in the build.   // "bug" is singular — acceptable
There are several warnings and a bug.               // "warnings" is plural

// Better — restructure
The build contains a bug and several warnings.
```

#### Additional Agreement Pitfalls

**Either/Neither** — singular in formal writing:

```text
Either of the solutions works.
Neither approach is correct.
```

**The number vs A number:**

```text
The number of requests is growing.          // singular — "the number" is one thing
A number of requests are failing.           // plural — "a number of" means "many"
```

**Fractions and percentages** — agree with the noun after **of**:

```text
One-third of the code is generated.
One-third of the files are generated.
Fifty percent of the bandwidth is consumed.
```

**Titles of documents, repos, and projects** — always singular:

```text
Docker Compose is a tool for defining multi-container apps.
// Not: Docker Compose are a tool...
```

## Glossary

| Term | Definition |
|------|------------|
| Article | A word (a, an, the) used before a noun to indicate definiteness |
| Zero article | The absence of an article before a noun (e.g., "Python is fast") |
| Definite article | The word "the", for specific or identifiable nouns |
| Indefinite article | The word "a" or "an", for non-specific or new nouns |
| Preposition | A word showing relationship (in, on, at, to, for, with) |
| Preposition stranding | Ending a sentence with a preposition (grammatically acceptable) |
| Modal verb | A verb (can, may, must, should, will) expressing necessity or possibility |
| RFC 2119 | A standard defining precise meanings for requirement keywords |
| Countable noun | A noun that can be counted (bug/bugs, feature/features) |
| Uncountable noun | A noun that cannot be counted (information, code, feedback) |
| Dangling modifier | A modifier that does not clearly refer to any word in the sentence |
| Misplaced modifier | A modifier placed too far from the word it modifies |
| Squinting modifier | A modifier that could apply to either the preceding or following element |
| Parallel structure | Using the same grammatical form for related elements |
| Comma splice | Joining two independent clauses with only a comma |
| Compound modifier | Two or more words working as a single adjective (hyphenated before a noun) |
| Collective noun | A noun (team, group) referring to a collection of individuals |
| Indefinite pronoun | A pronoun (each, every, anyone) referring to a non-specific person or thing |
| Subject-verb agreement | The rule that a verb must match its subject in number |

## Quick References

- [RFC 2119: Key words for use in RFCs](https://www.rfc-editor.org/rfc/rfc2119) — definitive guide to requirement keywords
- [Microsoft Style Guide](https://learn.microsoft.com/en-us/style-guide/welcome/) — grammar and style conventions for technical writing
- [Google Developer Documentation Style Guide](https://developers.google.com/style/) — grammar guidelines from Google
- [Purdue OWL](https://owl.purdue.edu/owl/general_writing/grammar/) — comprehensive grammar reference

## Next Steps

- [Sentence Structure for Technical Writing](sentence-structure.md) — building clear sentences with correct grammar
- [Punctuation for Clarity](punctuation.md) — using punctuation to support grammatical correctness
- [Writing for International Audiences](international-audiences.md) — adapting your writing for global readers
