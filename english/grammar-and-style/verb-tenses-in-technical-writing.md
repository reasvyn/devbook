# Verb Tenses in Technical Writing

## Description

Verb tense tells the reader when an action occurs. Mastering the twelve English tenses and their aspects lets you write API docs, changelogs, commit messages, incident reports, RFCs, and tutorials with precision and consistency — reducing ambiguity and helping readers act on your words confidently.

## Prerequisites

- [Basic Grammar Refresher for Developers](basic-grammar-refresher.md) — understanding parts of speech, especially verbs

## Table of Contents

- [The 12 English Tenses: An Overview](#the-12-english-tenses-an-overview)
- [Simple Present in Technical Writing](#simple-present-in-technical-writing)
- [Simple Past in Technical Writing](#simple-past-in-technical-writing)
- [Simple Future in Technical Writing](#simple-future-in-technical-writing)
- [Present Perfect in Technical Writing](#present-perfect-in-technical-writing)
- [Imperative Mood](#imperative-mood)
- [Progressive and Perfect Aspects](#progressive-and-perfect-aspects)
- [Tense Consistency](#tense-consistency)
- [Special Cases: Conditionals and Modals](#special-cases-conditionals-and-modals)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### The 12 English Tenses: An Overview

A **tense** locates an action in time (past, present, future). An **aspect** describes the nature of that action — whether it is complete, ongoing, or repeated. Together they form twelve combinations. The table below shows each with a neutral technical example.

| Tense | Example in Tech Writing | When to Use |
|-------|------------------------|-------------|
| Simple Present | `validate()` returns a boolean. | Facts, general truths, API docs |
| Present Progressive | The server is restarting. | Ongoing action right now |
| Present Perfect | The team has deprecated this endpoint. | Past action with present relevance |
| Present Perfect Progressive | The build has been running for six hours. | Ongoing action that began in the past |
| Simple Past | The connection timed out. | Completed past event |
| Past Progressive | The daemon was processing requests when the disk filled. | Ongoing action interrupted by another event |
| Past Perfect | The cache had expired before the request arrived. | Action completed before another past event |
| Past Perfect Progressive | The pod had been crashing for hours before the alert fired. | Ongoing past action before another past event |
| Simple Future | The migration will run at midnight. | Future event or prediction |
| Future Progressive | The service will be rolling out updates all week. | Ongoing future action |
| Future Perfect | By Q3, the team will have completed the rewrite. | Action completed before a future time |
| Future Perfect Progressive | By deployment, we will have been testing for two weeks. | Ongoing future action up to a future time |

Not every tense is equally useful in technical writing. Simple present, simple past, simple future, present perfect, and the imperative mood do most of the work. The progressive and perfect progressive forms appear rarely, but they matter when you need to describe sequences or ongoing states.

### Simple Present in Technical Writing

Simple present is the default tense for most developer documentation. It expresses facts, habitual actions, and general truths that do not depend on a specific time.

**API documentation.** Describe what a function, method, or endpoint does using simple present. The action holds for every invocation.

```js
/**
 * Parses a CSV string and returns an array of row objects.
 * Throws a ParseError if the input is malformed.
 */
function parseCsv(input: string): Row[]
```

```text
GET /users/:id

Returns a user object for the given ID. The response includes
the user's name, email, and role. Returns 404 if the user does
not exist.
```

**Error messages.** Error messages use simple present to describe the current state.

```text
File not found.
Connection refused.
Access denied.
Bucket does not exist.
```

These statements are timeless — they describe the situation as it is right now.

**Code comments.** Explain what a block of code does, not what it is doing right this instant.

```python
# Computes the Levenshtein distance between two strings.
# Uses dynamic programming with O(n*m) space.
def levenshtein(a: str, b: str) -> int:
```

**Configuration and specification files.**

```yaml
# server.yaml
# Defines the maximum number of concurrent connections.
# Applies to all environments.
max_connections: 100
```

**General truths.** Documentation for algorithms, protocols, and systems invariants.

```text
TCP guarantees in-order delivery of packets.
The CAP theorem states that a distributed system cannot guarantee
consistency, availability, and partition tolerance simultaneously.
```

**Contrast with present progressive.** Present progressive (also called present continuous) describes an action in progress right now.

```text
Simple present:   The daemon listens on port 8080.
Present progressive: The daemon is listening on port 8080.
```

The first sentence states a permanent fact about the daemon's configuration. The second implies the daemon started listening recently and may stop. In technical writing, simple present is almost always preferred unless you are describing a transient state.

### Simple Past in Technical Writing

Use simple past for actions that began and ended at a known time in the past. This tense anchors events to a specific moment.

**Incident reports and postmortems.** Pinpoint when things happened.

```text
At 14:32 UTC the primary database node crashed.
The load balancer redirected traffic to the replica.
At 14:35 UTC the replica also failed because it was
already at capacity.
```

Each sentence refers to a discrete, completed event. This precision is critical for root-cause analysis.

**Changelogs — past tense entries.** Many changelogs use past tense for fixed, removed, or deprecated items.

```markdown
## [2.4.0] - 2026-03-15

### Fixed
- Fixed a race condition in the connection pool.
- Corrected off-by-one error in pagination logic.

### Removed
- Removed deprecated `v1/auth` endpoint.
- Dropped support for Node.js 14.

### Changed
- Updated the rate-limiting algorithm from token bucket to sliding window.
```

Note that some changelogs use present tense or imperative. The key is consistency within a single document.

**Release notes.** Describe what changed in a specific release.

```text
In this release, we migrated the build system from Webpack to Vite.
We also replaced Mocha with Vitest for unit tests.
```

**Step-by-step tutorials — describing what just happened.** When a tutorial asks the reader to perform an action and then explains the result, use past tense for the explanation.

```text
You ran npm install. The command resolved all dependencies and
created a node_modules directory. It also generated a
package-lock.json file.
```

**Bug descriptions in issue trackers.**

```text
The user reported that the export button did not respond after
the 2.3.0 upgrade. The button rendered correctly but the click
handler returned undefined.
```

### Simple Future in Technical Writing

Simple future describes actions that have not happened yet. In technical writing it appears in planning documents, RFCs, conditional instructions, and deprecation notices.

**RFCs and proposals.** Propose changes that will take effect later.

```text
## Proposal

This change will introduce a new indexing strategy based on
LSM trees. The existing B-tree index will remain available
but will be deprecated in the next major release.

## Migration Path

Existing databases will migrate automatically on first startup.
The migration will run as a background task and will not block
reads.
```

**Roadmaps and planning documents.**

```text
### Q3 2026

The team will ship the GraphQL federation layer. We will also
publish an SDK for Rust. The legacy REST API will enter
maintenance mode.
```

**Conditional documentation.** Describe what happens when the user takes an action.

```text
If you enable compression, the server will gzip responses
larger than 1 KB. The client will decompress them automatically
if it includes the Accept-Encoding header.
```

**Warnings about future behavior.**

```text
Warning: This endpoint will return a 410 Gone status after
June 30, 2026. Migrate to /v3/orders before that date.
```

**Simple future vs. present for scheduled events.** English allows simple present for scheduled future events.

```text
The deployment window opens at 2:00 AM UTC. (scheduled fact)
The deployment will open at 2:00 AM UTC.  (future prediction)
```

Both are correct. Present sounds more definitive; future sounds like a prediction. For documented schedules, present tense is often stronger.

### Present Perfect in Technical Writing

Present perfect connects a past action to the present moment. It is the workhorse of changelogs, deprecation notices, and documentation describing what has changed since the last version.

**Structure.** `has/have + past participle`.

```text
has deprecated
have added
has migrated
```

**Changelogs.** Present perfect tells the reader what is true *now* because of a past change.

```markdown
## [2.5.0] - 2026-04-01

### Added
- Added support for WebSocket connections.
- Introduced middleware chaining for request pipelines.

### Changed
- Updated the minimum Node.js version to 18.
- Migrated the documentation site to Astro.
```

Some projects use simple past ("Added support...") while others use present perfect ("Have added support..."). Either is acceptable, but present perfect emphasizes the current state: support *is now available*.

**Deprecation notices.** The "has been deprecated" pattern is standard.

```text
This method has been deprecated. Use sendBatch() instead.

The /v2/orders endpoint has been deprecated and will be
removed in version 4.0.
```

**Documenting changes since the last version.**

```text
Since version 1.3, the library has dropped support for Python 3.8,
has added async/await helpers, and has rewritten the HTTP client
using httpx.
```

**Release notes.** Present perfect works for listing cumulative changes.

```text
What's New in 3.0

We have redesigned the dashboard. We have added real-time
metrics and have consolidated the settings panel. The
navigation has been simplified to three main sections.
```

### Imperative Mood

The **imperative mood** issues a direct command. The subject (you) is implied. In technical writing, the imperative is the dominant form for instructions, commit messages, and error prompts.

**Commit messages.** The de facto standard for commit messages is imperative present tense.

```text
Fix race condition in connection pool
Add WebSocket support to the API gateway
Remove deprecated v1/auth endpoint
Update README with new setup instructions
Update dependency versions
```

This convention comes from Git itself: "If applied, this commit will *fix* the race condition." The imperative form reads naturally as an instruction to the codebase.

Do not write:

```text
Fixed race condition in connection pool   (past tense — describes what was done)
Fixes race condition in connection pool   (third person — describes what the commit does)
Fixing race condition in connection pool  (progressive — awkward)
```

**Tutorial instructions.** Every step the reader must perform uses the imperative.

```text
1. Clone the repository.
2. Run npm install.
3. Copy .env.example to .env.
4. Start the development server.
```

**Error messages with instructions.**

```text
Error: Port 3000 is already in use.
  → Specify a different port with --port.
  → Or stop the existing process.

Press any key to continue.
```

**README setup steps.**

```markdown
## Installation

Install the package via npm:

```bash
npm install @org/awesome-lib
```

Configure the client with your API key:

```js
import { Client } from '@org/awesome-lib';

const client = new Client({ apiKey: process.env.API_KEY });
```
```

**API documentation — `@param` and `@returns` tags.**

```js
/**
 * @param {string} id - The user ID to look up.
 * @param {Object} [options] - Optional query parameters.
 * @returns {Promise<User>} A promise that resolves to the user object.
 */
```

The descriptions use simple present, but the imperative governs the structure "do this, get that."

### Progressive and Perfect Aspects

Aspect modifies the temporal quality of an action.

**Simple** (no helper): the action is a single fact.

```text
The server processes the request.
```

**Progressive** (be + -ing): the action is ongoing.

```text
The server is processing the request.
```

**Perfect** (have + past participle): the action is complete relative to another time.

```text
The server has processed the request.
```

**Perfect progressive** (have + been + -ing): ongoing action up to a reference time.

```text
The server has been processing requests since midnight.
```

**When progressive is appropriate.** Progressive tenses are rare in technical writing. Use them when the ongoing nature of an action matters for understanding a sequence.

```text
Simple present is better:  The batch job runs every hour.
Progressive is confusing:  The batch job is running every hour. (implies it is running right now)

Use progressive for active monitoring:
  The deployment script is running. Do not interrupt it.

Use progressive in alerts:
  The service is degrading. Response times have exceeded 5 seconds.
```

**When perfect is needed for sequence clarity.** Perfect tenses clarify which action happened first.

```text
Without perfect (confusing):
  The cache expired, so the query hit the database.
  (Did the cache expire before or because of the query?)

With past perfect (clear):
  The cache had expired before the query arrived, so the query
  hit the database.
```

**Avoiding unnecessary complexity.** Progressive and perfect progressive add words without adding meaning in most technical contexts.

```text
Wordy and unclear:   The system has been verifying the checksum
                     and will have been completing the validation
                     by the time the next poll occurs.

Concise and clear:   The system verifies the checksum and
                     completes validation before the next poll.
```

Prefer simple tenses. Reach for progressive or perfect only when the sequence of events is otherwise ambiguous.

### Tense Consistency

Consistency is more important than which tense you choose. Switching tenses without a reason confuses the reader about the timeline.

**Code comments.** Pick one tense and stay in it.

```python
# BAD: Mixes present and future
# Loads the config file. It will parse each section and
# returns a dictionary.

# GOOD: All present
# Loads the config file, parses each section, and returns
# a dictionary.
```

**API documentation.** Use simple present throughout.

```text
BAD:
  This endpoint returns a user object. It will throw an error
  if the ID does not exist. It was designed for internal use
  only.

GOOD:
  This endpoint returns a user object. It throws an error if
  the ID does not exist. It is designed for internal use only.
```

**Tutorials.** Tutorials have a specific pattern: imperative for instruction steps, simple present or simple past for explanation, and future for warnings.

```text
1. Open the terminal.
2. Run docker compose up.

The command starts three containers: a web server, a database,
and a Redis instance. Docker Compose assigns them to the same
network, so the web server can reach the database using the
service name `db`.

If you stop the process with Ctrl+C, the containers will shut
down gracefully.
```

Note the shifts are intentional: imperative for action, present for explanation, future for a conditional outcome. This is acceptable because each shift signals a change in purpose.

**Error messages.** Use simple present consistently.

```text
BAD:
  The file was not found. The system will attempt to create it.
  (mixes past and future)

GOOD:
  File not found. Create it before proceeding.
  (present + imperative — clear and actionable)
```

**RFCs.** Use simple future for proposals and simple present for facts about the current system.

```text
The current system stores metadata in PostgreSQL.
This proposal will replace PostgreSQL with FoundationDB.
The new storage layer will expose the same API, so callers
will not need to change.
```

**Common tense-shifting mistakes.**

- Changelog mixing imperative ("Fix bug") with past tense ("Fixed bug") in the same release.
- Code comments that start in present and drift into future ("Returns the value. It will return null if...").
- Tutorial steps that alternate between "you will" and "you" (imperative). Pick one.

```text
CONSISTENT (imperative throughout):
  1. Install the CLI tool.
  2. Authenticate with your API key.
  3. Run the migration.

INCONSISTENT:
  1. You will install the CLI tool.
  2. Authenticate with your API key.
  3. The migration will now run.
```

### Special Cases: Conditionals and Modals

**Zero conditional — general truths.** If/when + present, present. Use for system invariants and specifications.

```text
If the file exists, it is overwritten.
When the buffer reaches capacity, the worker flushes it to disk.
```

These are always true — no conditionality implied.

**First conditional — real possibility.** If + present, will + base verb. Use for documentation describing user actions.

```text
If you set DEBUG=true, the application logs all HTTP requests.
If the migration fails, the script reverts to the last snapshot.
```

**Second conditional — hypothetical present/future.** If + past, would + base verb. Rare in technical writing, but useful in design documents.

```text
If the system used a lock-free data structure, it would achieve
higher throughput under contention.
```

This implies the current system does *not* use a lock-free structure. The conditional signals a hypothetical alternative.

**Third conditional — unreal past.** If + past perfect, would have + past participle. Useful in postmortems.

```text
If we had tested the migration on a staging environment, we
would have caught the schema conflict before production.
```

The reality: the team did *not* test on staging, and the production incident happened.

**"Will" vs. "going to" in technical plans.** Both express future, but they carry different nuance.

```text
Will:     The patch will ship in the next release.
          (neutral prediction, often a commitment)

Going to: The patch is going to ship in the next release.
          (stronger intention, often based on current evidence)
```

In technical writing, `will` is the safer choice. It is concise and does not imply a prior plan. Use `going to` only when you want to emphasize that current evidence supports the prediction.

```text
Preferred: The next release will include the fix.
Colloquial but acceptable: The next release is going to include the fix.
```

**Modal verbs for advice and necessity.** Modal auxiliaries like `must`, `should`, `may`, `can`, and `might` interact with tense to express obligation, recommendation, or possibility.

```text
must / must not:        The client must include an API key.
                        (absolute requirement)
should / should not:    You should validate input before passing
                        it to the query builder. (recommendation)
may / may not:          The endpoint may return cached data.
                        (permission or possibility)
can / cannot:           The browser can cache this response.
                        (ability)
might / might not:      The service might experience brief
                        downtime during the migration.
                        (weak possibility)
```

Combine modals with perfect infinitives to express past possibilities in postmortems.

```text
The load balancer should have routed traffic to the backup
datacenter, but the health check did not fire.
```

## v4.0 Breaking Changes

- Removed the `/v1/orders` endpoint.
- Changed the response format from XML to JSON.
- Increased the minimum page size from 1 to 10.
```

**Approach B — present perfect.**

```markdown
## v4.0 Breaking Changes

- Removed the `/v1/orders` endpoint.
- Changed the response format from XML to JSON.
- Increased the minimum page size from 1 to 10.
```

Wait, those look identical. The difference is conceptual: past tense frames each item as a historical event, while present perfect frames them as the current state. Most developers read "Removed" as past tense describing the change, not an ongoing state.

**Approach C — imperative.**

```markdown
## v4.0 Breaking Changes

- Remove the `/v1/orders` endpoint. (migration instruction)
- Change the response format from XML to JSON.
- Increase the minimum page size from 1 to 10.
```

Imperative sounds like instructions, not announcements. Use imperative in migration guides, not in release highlights.

**Verdict:** Past tense or present perfect both work for changelogs. Pick one and stay consistent across the changelog.

### Case 2: Writing an incident timeline

An SRE team writes a postmortem after a database outage. The timeline uses simple past for discrete events and past perfect for precedence.

```text
## Timeline

12:00 UTC — A routine schema migration began.
12:03 UTC — The migration acquired an exclusive lock on the
            `orders` table.
12:05 UTC — Queued writes began backing up because the lock
            blocked inserts.
12:12 UTC — The replication lag alarm fired. The standby had
            already fallen behind by 200 MB of WAL.
12:18 UTC — The primary ran out of disk space because the WAL
            had not been archived since the migration started.
12:20 UTC — The on-call engineer received the alert.
12:25 UTC — The engineer canceled the migration, freed disk
            space, and promoted the standby.
```

Note how past perfect ("had already fallen behind", "had not been archived") clarifies which events preceded others. Without it, the timeline would be ambiguous about causality.

### Case 3: Converting a poorly-tense tutorial to consistent style

**Before (inconsistent):**

```text
Step 1: You will create a new project.
Step 2: You installed the dependencies.
Step 3: The app will start on port 3000.
```

**After (consistent imperative + present for explanation):**

```text
Step 1: Create a new project.

  The create command scaffolds a project with the following
  structure:
  ```
  my-app/
  ├── src/
  ├── public/
  └── package.json
  ```

Step 2: Install the dependencies.

  npm install reads the package.json and resolves all
  dependencies. It creates a node_modules directory and a
  lockfile.

Step 3: Start the development server.

  The server starts on port 3000. Open http://localhost:3000
  to see the welcome page.
```

Imperative for steps, simple present for explanations. Clean, predictable, easy to follow.

## 2.0 Retrospective

We shipped version 2.0 on March 1. The team completed the
rewrite from jQuery to React in six months. We reduced the
bundle size by 60% and improved the first-paint time by 40%.
The migration required 340 files changed across 12 modules.
```

**Example 3: Present perfect in a deprecation policy.**

```text
## Deprecation Policy

As of version 3.0, we have deprecated the following features:

- Legacy plugin API (has been replaced by the hook system)
- JSON config files (have been replaced by YAML)
- Node.js 12 support (has reached end of life)

These features will be removed in version 4.0, scheduled for
Q1 2027.
```

**Example 4: Imperative mood in a commit history.**

```text
$ git log --oneline -5

a1b2c3d Fix null pointer in session middleware
e4f5g6h Add rate limiting to the public API
i7j8k9l Remove debug logging from production code
m0n1o2p Update ESLint config to enforce consistent imports
q3r4s5t Refactor auth module into service layer
```

Note that every message is imperative, present tense, and under 72 characters.

**Example 5: Conditionals in an RFC.**

```text
## Trade-offs

If the system used a pull-based model, consumers would control
their own pace, but the producer would need to buffer messages.
If we had chosen RabbitMQ instead of Kafka, we would have gained
better routing but lost log compaction. We chose Kafka because
log compaction is a hard requirement for the audit trail.
```

## Glossary

| Term | Definition |
|------|------------|
| Aspect | A grammatical category that describes the nature of an action (complete, ongoing, or repeated) |
| Future perfect | A tense describing an action that will be completed before a future reference point (will have done) |
| Future perfect progressive | A tense describing an ongoing action that will continue up to a future reference point (will have been doing) |
| Future progressive | A tense describing an action that will be in progress at a future time (will be doing) |
| Imperative mood | A verb form that issues a direct command (Run, Install, Fix) |
| Modal auxiliary verb | A helper verb that expresses necessity, possibility, or permission (must, should, can, may, might) |
| Past perfect | A tense describing an action completed before another past event (had done) |
| Past perfect progressive | A tense describing an ongoing action that continued up to another past event (had been doing) |
| Past progressive | A tense describing an action that was in progress at a past time (was doing) |
| Present perfect | A tense linking a past action to the present moment (has done) |
| Present perfect progressive | A tense describing an action that began in the past and continues to the present (has been doing) |
| Present progressive | A tense describing an action currently in progress (is doing) |
| Simple future | A tense describing an action that has not yet occurred (will do) |
| Simple past | A tense describing a completed action at a known past time (did) |
| Simple present | A tense describing facts, habits, or general truths (does) |
| Tense | A grammatical category that locates an action in time (past, present, future) |

## Quick References

- [Microsoft Style Guide — Tense](https://learn.microsoft.com/en-us/style-guide/grammar/verbs) — voice, mood, and tense guidelines from the Microsoft Writing Style Guide
- [Google Developer Documentation Style Guide — Verbs](https://developers.google.com/style/verbs) — Google's guidance on verb tense and mood in developer documentation
- [Git Commit Message Convention](https://cbea.ms/git-commit/) — imperative mood rationale for commit messages

## Next Steps

- [Common Grammar Pitfalls for Developers](common-grammar-pitfalls-for-developers.md) — frequent mistakes and how to avoid them
- [Sentence Structure for Technical Writing](sentence-structure.md) — building clear, readable sentences
- [Active vs. Passive Voice in Technical Writing](active-vs-passive.md) — choosing the right voice for your audience
