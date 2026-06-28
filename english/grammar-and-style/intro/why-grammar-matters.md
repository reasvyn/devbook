# Why Grammar Matters for Developers

## Description

Grammar is not pedantry — it is precision. For developers, grammatical errors in code comments, documentation, and communication introduce ambiguity, reduce credibility, and create maintenance burden. This document explains why grammar matters in every context a developer writes.

## Prerequisites

- [Why English Matters for Developers](../../intro/why-english-matters.md)

## Table of Contents

- [Grammar as a Tool for Clarity](#grammar-as-a-tool-for-clarity)
- [The Cost of Ambiguity](#the-cost-of-ambiguity)
- [Grammar in Code Comments](#grammar-in-code-comments)
- [Grammar in Documentation](#grammar-in-documentation)
- [Grammar in Commit Messages & PRs](#grammar-in-commit-messages--prs)
- [Grammar in Technical Communication](#grammar-in-technical-communication)
- [Common Grammatical Errors in Technical Writing](#common-grammatical-errors-in-technical-writing)
- [Punctuation That Changes Meaning](#punctuation-that-changes-meaning)
- [Sentence Structure for Technical Prose](#sentence-structure-for-technical-prose)
- [Voice, Mood & Modality](#voice-mood--modality)
- [Style Guides & Grammar Rules](#style-guides--grammar-rules)
- [Grammar Checkers & Tooling](#grammar-checkers--tooling)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Grammar as a Tool for Clarity

Grammar is the system of rules that governs how words combine into meaningful sentences. It is not arbitrary — it evolved to reduce ambiguity and increase communicative efficiency.

**Why developers specifically need good grammar:**

- **Code is read more than written.** A grammatical error in a comment is read by every future developer who touches that code.
- **Documentation is a contract.** Ambiguous grammar creates legal-grade disputes over what a feature actually does.
- **Global teams.** Your readers and collaborators may not be native English speakers. Correct grammar is easier for them to parse.
- **AI training data.** If you write documentation that gets used as training data for LLMs, grammatical errors propagate to millions of users.

**Grammar vs. style:**

| Aspect | Grammar | Style |
|---|---|---|
| Correctness | Right or wrong | Consistent or inconsistent |
| Rules | Established by usage | Established by convention |
| Examples | Subject-verb agreement | Oxford comma yes/no |
| Enforcement | Linters catch some | Style guides define |

### The Cost of Ambiguity

Ambiguity in technical writing has real costs:

**Development cost:** A developer misreads an ambiguous comment and makes the wrong change. Hours lost debugging.

**Support cost:** A customer misunderstands ambiguous documentation and opens a support ticket. Minutes per ticket × thousands of customers.

**Security cost:** Ambiguous security documentation leads to misconfiguration. Misconfiguration leads to breaches.

**Onboarding cost:** New team members struggle with unclear internal documentation. Weeks of lost productivity.

**Examples of costly ambiguity:**

"Users without admin access cannot modify settings and save changes."

This could mean:
1. Cannot modify settings, and also cannot save changes (two prohibitions)
2. Cannot modify settings in a way that saves changes (one compound prohibition)

Which is it? The grammar fails to distinguish.

Correct: "Users without admin access cannot modify settings or save changes."

### Grammar in Code Comments

Code comments are mini-technical documents. They should follow the same grammatical standards as any other technical writing.

**Good comment grammar:**

```python
# Calculate the hash of the payload using SHA-256.
# This hash is used to verify integrity during transit.
```

**Bad comment grammar:**

```python
# calculating the hash for the payload using sha256
# used to verify integrity while its in transit
```

Problems: missing capitalization, inconsistent tense ("calculating" vs "used"), vague pronoun reference ("its" — the payload? the hash?), wrong word ("while" instead of "during" or "in").

**Common comment grammar issues:**

| Issue | Bad | Good |
|---|---|---|
| Sentence fragments | "adding user to database" | "Adds the user to the database." |
| Tense inconsistency | "This function loads data and then return it." | "This function loads data and then returns it." |
| Missing articles | "Function returns error if file not found." | "The function returns an error if the file is not found." |
| Pronoun ambiguity | "Update the config and restart it." | "Update the config and restart the server." |
| Wrong preposition | "Compare with the previous value." | "Compare to the previous value" (or "with" depending on context) |

**The grammar of docstrings:**

Docstrings are formal documentation embedded in code. They demand even higher grammatical standards:

```python
def calculate_mean(values: list[float]) -> float:
    """Calculate the arithmetic mean of a list of numbers.

    Args:
        values: A list of numeric values. Must contain at least one element.

    Returns:
        The arithmetic mean as a float.

    Raises:
        ValueError: If the input list is empty.
    """
```

Note: consistent imperative mood ("Calculate"), consistent sentences ("Must contain..."), consistent phrasing for each parameter.

### Grammar in Documentation

Documentation has the highest grammar standards because it represents the product to the world.

**Grammatical consistency in documentation:**

- **Use consistent terminology.** Don't call it a "button" in one place and a "control" in another.
- **Use consistent grammatical person.** "You" (second person) is standard. Avoid "the user" (third person) or "we" (first person) for the same audience.
- **Use consistent mood.** Imperative for instructions ("Click Save"), indicative for explanations ("The system saves automatically").
- **Parallel structure.** Lists should use consistent grammatical form:

Bad:
```markdown
The installer:
- Copies files to the target directory
- Will register the service
- Configuration file creation
```

Good:
```markdown
The installer:
- Copies files to the target directory
- Registers the service
- Creates the configuration file
```

**The serial comma (Oxford comma):**

Whether to use the Oxford comma is a style choice, but be consistent:

With Oxford comma: "Install Python, Node.js, and Docker."
Without: "Install Python, Node.js and Docker."

The Oxford comma can prevent ambiguity:
"To the developers, the PM and the designer" — the PM is a developer?
"To the developers, the PM, and the designer" — three distinct groups.

**Articles (a, an, the):**

Non-native speakers often struggle with articles. Documentation should use articles correctly:

Bad: "Error occurred during deployment. Check log for details."
Good: "An error occurred during deployment. Check the log for details."

### Grammar in Commit Messages & PRs

Commit messages are permanent records of what changed and why. They are read by reviewers, future developers, and automated tools.

**The 50/72 rule:**

- First line: 50 characters max, imperative mood, capitalized
- Body: wrapped at 72 characters, explains what and why (not how)

```
Add rate limiting to the payment endpoint

Rate limiting prevents abuse of the payment endpoint by limiting
requests to 10 per second per API key. This protects against
card testing attacks and accidental overcharging.
```

**Imperative mood:**

Git convention uses the imperative mood ("Add", "Fix", "Update", "Remove"). This matches the grammatical form of "If applied, this commit will..."

**Pull request descriptions:**

PR descriptions tell the story of the change. Good grammar helps reviewers understand:

Bad:
```
so i changed the thing with the database cause it was slow
also fixed some stuff from the last pr which was kinda broken
```

Good:
```
Optimize database queries for the user listing endpoint

Replaced N+1 queries with a single JOIN query, reducing page load
time from 3s to 200ms for organizations with 500+ users.

Also fixes a regression from PR #123 where deleted users were still
returned in the listing response.
```

### Grammar in Technical Communication

**Email grammar:**

- Subject line: brief, descriptive, grammatical
- Body: complete sentences, paragraphs, clear ask
- Sign-off: professional, consistent

Bad: "need help with deployment pls" (no subject context, no greeting, no sign-off)

Good: "Deployment failure on staging — database connection timeout" (clear subject, problem stated upfront)

**Incident communication:**

During incidents, grammar precision is critical for accurate communication:

```
INCIDENT: Database connection failures on production
IMPACT: All write operations failing since 14:32 UTC
STATUS: Engineering team is investigating. No ETA yet.
NEXT UPDATE: In 15 minutes or when new information is available.
```

Note: Consistent tense (present continuous for ongoing status), clear structure, no ambiguity.

### Common Grammatical Errors in Technical Writing

**Subject-verb agreement:**

- "The list of features is long." (subject: list, not features)
- "The data are stored." (vs "The data is stored" — both acceptable, be consistent)
- "Each of the tests passes." (subject: each, not tests)

**Dangling modifiers:**

"Button the user clicks the Save button." — Who or what is "Butted"?

"A database connection is required before running the query." — Who runs the query?

Correct: "The application requires a database connection before running the query."

**Misplaced modifiers:**

Bad: "The function returns the result with error handling." — Does the function have error handling, or does the result have it?

Good: "With error handling, the function returns the result."

**Pronoun-antecedent agreement:**

Bad: "Every developer should bring their laptop." (singular "developer" + plural "their")

Good: "All developers should bring their laptops." (plural + plural)

Or: "Every developer should bring a laptop." (avoid the issue)

**Tense consistency:**

Bad: "The system validates the input and then returned the result."

Good: "The system validates the input and then returns the result."

**Preposition usage:**

Common errors:
- "Different than" → "Different from" (preferred)
- "Based off" → "Based on"
- "Compare to" ≠ "Compare with" (compare to = liken; compare with = contrast)
- "Regardless if" → "Regardless of whether"

### Punctuation That Changes Meaning

**The comma that saves lives:**

"Let's eat, Grandma!" vs "Let's eat Grandma!"

**Comma splice:**

Bad: "The server is down, we are investigating."
Good: "The server is down. We are investigating."
Or: "The server is down, and we are investigating."

**Colon vs. semicolon:**

- Colon: introduces or explains ("The problem is simple: we need more memory.")
- Semicolon: joins related independent clauses ("The API is stable; the UI is not.")

**Hyphen vs. dash:**

- Hyphen: connects compound words ("user-friendly interface")
- En dash (–): ranges (Python 3.7–3.11)
- Em dash (—): interruption or emphasis ("The system failed—without warning—and the data was lost.)

**Quotation marks:**

- Use straight quotes in code, curly quotes in prose
- Periods and commas go inside quotation marks (American style)
- Colons and semicolons go outside

**Apostrophes:**

- "The variable's value" (possessive)
- "The variable value" (attributive noun, possessive not needed)
- "Its value" (possessive pronoun, NO apostrophe)
- "It's value" — WRONG ("it's" = "it is")

### Sentence Structure for Technical Prose

**The standard sentence:**

Subject + Verb + Object: "The system [subject] returns [verb] an error [object]."

**Keep sentences short:**

Guideline: 15–20 words for technical prose. Longer sentences can be split.

Bad: "The system will automatically retry the connection if the initial attempt fails due to a transient network error up to three times before giving up and logging a failure message to the application log."

Good: "The system retries the connection automatically if the initial attempt fails. It retries up to three times. After all retries fail, it logs an error."

**Vary sentence structure:**

Monotonous: "The system validates the input. The system formats the data. The system sends the response."

Better: "The system validates the input, formats the data, and sends the response."

**Transitional phrases:**

- "However," "Therefore," "In addition," "For example," "In contrast"
- These guide the reader through the logical flow
- Use sparingly — not every sentence needs a transition

### Voice, Mood & Modality

**Active vs. passive voice:**

| Active | Passive |
|---|---|
| "The system writes the log." | "The log is written by the system." |
| "Click Save to confirm." | "Save should be clicked to confirm." |

Active voice is preferred in technical writing:
- Shorter
- Clearer about who does what
- More direct and authoritative

Use passive voice when:
- The actor is unknown ("The file was corrupted.")
- The actor is obvious ("The user is redirected.")
- The action matters more than the actor ("The payment is processed every hour.")

**Mood:**

| Mood | Example | Use case |
|---|---|---|
| Imperative | "Install the package." | Instructions |
| Indicative | "The package installs dependencies." | Explanations |
| Conditional | "If the installation fails, check the log." | Troubleshooting |

**Modality (certainty and obligation):**

| Modal | Meaning | Example |
|---|---|---|
| must | Required | "You must set the API key." |
| should | Recommended | "You should use a virtual environment." |
| may | Optional | "You may configure the port." |
| can | Possible | "You can run the tests locally." |
| will | Future certainty | "The system will reboot." |
| might | Possibility | "The connection might time out." |

Be precise about modality in documentation. "Must" and "should" have different legal and practical implications.

### Style Guides & Grammar Rules

Style guides define the grammatical and stylistic conventions for a documentation ecosystem.

**Google Developer Documentation Style Guide:**

- Use second person ("you")
- Use active voice
- Use sentence case for headings
- Use the serial comma
- Write for scannability

**Microsoft Style Guide:**

- Use "you" and "your"
- Use present tense
- Use active voice
- Avoid jargon, slang, idioms
- Write short sentences

**Apple Style Guide:**

- Use "you" to address the reader
- Avoid "the user" — it distances the reader
- Use sentence case for UI text
- Use the term "tap" for iOS, "click" for macOS

**Common style guide rules across all three:**

| Rule | Google | Microsoft | Apple |
|---|---|---|---|
| Second person | Yes | Yes | Yes |
| Active voice | Yes | Yes | Yes |
| Serial comma | Yes | No (British) | Yes |
| Contractions | Yes | Yes | Yes |
| "e.g." / "i.e." | Spell out | Spell out | Use Latin |
| Numbers | Spell 0-9, digits 10+ | Spell 0-9, digits 10+ | Spell 0-9, digits 10+ |

### Grammar Checkers & Tooling

**Prose linters for technical writing:**

| Tool | What it checks |
|---|---|
| Vale | Customizable style checks, supports Google/Microsoft/Apple styles |
| write-good | Passive voice, hedge words, redundant phrases |
| Alex | Insensitive language |
| proselint | Broader prose issues |
| language-tool | Grammar and style errors |

**Configuring Vale for technical documentation:**

`.vale.ini`:
```
StylesPath = .vale/styles
MinAlertLevel = suggestion

[*.md]
BasedOnStyles = Google, write-good
Google.Spelling = NO  # don't flag code terms
```

**Checking grammar in CI:**

```yaml
# GitHub Actions workflow for grammar checking
name: Lint docs
on: [pull_request]
jobs:
  vale:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: errata-ai/vale-action@v2
```

**Grammarly and similar tools:**

- Good for general grammar checking
- Not designed for technical writing specifically
- May flag technical terms as errors
- May suggest style changes that conflict with technical writing conventions

## Study Cases

### Case 1: The Missing Comma

A deployment script read:

```python
# Remove files that are not needed for production
# and archive the rest
```

A developer misread and removed all files because "the rest" seemed to refer to what was already described as not needed.

Correct:
```python
# Remove files that are not needed for production.
# Archive the remaining files.
```

Adding the period and restructuring removed the ambiguity.

### Case 2: Ambiguous Pronoun in an Incident Response Runbook

A runbook step read:

"Restart the database and the cache server. Make sure it is responding."

During an incident, an engineer restarted the database, checked that it was responding — but the cache was not. The "it" was ambiguous. The runbook was written during normal hours and the ambiguity was never noticed.

Fix: "Make sure both services are responding."

### Case 3: Documentation Mistranslation

An API documentation page said:

"The rate limit resets every hour."

A developer interpreted this as: the counter resets to zero every hour on the hour.

The actual behavior: The rate limit resets one hour after the first request.

The grammatical fix: "The rate limit resets one hour after the first request in the window."

### Case 4: The En Dash Disaster

A migration guide said:

"This feature supports Python 3.5-3.8."

Read by a developer as: Python 3.5 to 3.8.

But the writer used a hyphen instead of an en dash: "Python 3.5–3.8" (en dash) means range. "Python 3.5-3.8" (hyphen) could mean version 3.5 minus 3.8 — nonsensical but visually confusing with version numbers.

The corrected version with explicit wording: "This feature supports Python 3.5 through 3.8."

## Examples

### Example 1: Sentence structure transformations

Too long:
```
The error occurs when the database connection pool is exhausted because all available connections are being used by long-running queries that were initiated by the reporting module during peak hours.
```

Split:
```
The error occurs when the database connection pool is exhausted. This happens when the reporting module initiates long-running queries during peak hours, consuming all available connections.
```

### Example 2: Active vs. passive transformations

Passive:
"The configuration file should be modified by the administrator before the service is started."

Active:
"Modify the configuration file before starting the service."

### Example 3: Parallel structure

Unparallel:
```
To use the API, you must:
1. Obtaining an API key
2. Configure the client
3. Send requests
```

Parallel:
```
To use the API, you must:
1. Obtain an API key
2. Configure the client
3. Send requests
```

### Example 4: Before and after documentation editing

Before:
```
To configure the proxy you need to edit the config file. The file is located at /etc/app/config.ini by default. You can set the proxy_host and proxy_port settings. If you don't set them then the system will try to connect directly which might not work depending on your network configuration.
```

After:
```
1. Open the configuration file:
   /etc/app/config.ini

2. Set the proxy settings:
   ```ini
   proxy_host = proxy.example.com
   proxy_port = 8080
   ```

3. Save and restart the application.

If you skip this configuration, the application connects directly.
This may fail if your network requires a proxy.
```

## Glossary

| Term | Definition |
|---|---|
| Active voice | Subject performs the action ("The system logs the error.") |
| Ambiguity | A statement with more than one possible interpretation |
| Article | A determiner (a, an, the) that specifies noun definiteness |
| Clause | A group of words containing a subject and a verb |
| Comma splice | Joining two independent clauses with only a comma |
| Dangling modifier | A modifier without a clear subject to modify |
| En dash | A dash (–) used for ranges, longer than a hyphen |
| Em dash | A dash (—) used for emphasis or interruption |
| Hedge word | A word reducing certainty (basically, essentially, virtually) |
| Imperative mood | Verb form for commands and instructions |
| Indicative mood | Verb form for statements of fact |
| Modality | Linguistic expression of necessity, possibility, or permission |
| Oxford comma | The final comma before "and" in a list |
| Parallel structure | Using the same grammatical form for items in a list |
| Passive voice | Subject receives the action ("The error was logged.") |
| Pronoun-antecedent agreement | Pronoun matches its referent in number and gender |
| Serial comma | Another term for the Oxford comma |
| Style guide | Documented conventions for writing and formatting |
| Subject-verb agreement | Subject and verb match in number (singular/plural) |

## Quick References

- [Google Developer Documentation Style Guide](https://developers.google.com/style) — grammar standards for technical docs
- [Microsoft Style Guide](https://learn.microsoft.com/en-us/style-guide/) — comprehensive technical writing style reference
- [Grammarly Handbook](https://www.grammarly.com/handbook/) — general grammar reference
- [Vale: Prose Linter](https://vale.sh/) — automated style and grammar checking
- [The Elements of Style (Strunk & White)](https://www.gutenberg.org/ebooks/30609) — classic grammar reference
- [Purdue OWL](https://owl.purdue.edu/) — comprehensive writing resources

## Next Steps

- [Sentence Structure for Technical Writing](../sentence-structure.md) (planned) — crafting clear, readable sentences
- [Punctuation for Clarity](../punctuation-for-clarity.md) (planned) — mastering technical punctuation
