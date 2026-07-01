# Punctuation for Clarity

## Description

Punctuation guides readers through your technical documentation. Correct punctuation prevents ambiguity, clarifies relationships between ideas, and makes code examples, lists, and instructions easier to follow.

## Prerequisites

- [Sentence Structure](sentence-structure.md) — understanding clauses and sentence types

## Table of Contents

- [Punctuation as a Signpost](#punctuation-as-a-signpost)
- [Periods: Ending and Abbreviating](#periods-ending-and-abbreviating)
- [Commas: The Most Versatile Mark](#commas-the-most-versatile-mark)
- [Semicolons: Connecting Related Clauses](#semicolons-connecting-related-clauses)
- [Colons: Introducing and Explaining](#colons-introducing-and-explaining)
- [Dashes: Emphasis and Interruptions](#dashes-emphasis-and-interruptions)
- [Parentheses: Asides and References](#parentheses-asides-and-references)
- [Quotation Marks: Terms and Titles](#quotation-marks-terms-and-titles)
- [Apostrophes: Possession and Contractions](#apostrophes-possession-and-contractions)
- [Hyphens: Compound Terms](#hyphens-compound-terms)
- [Brackets: Editorial Insertions](#brackets-editorial-insertions)
- [Punctuation in Code Documentation](#punctuation-in-code-documentation)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Punctuation as a Signpost

Punctuation marks are not decorative. Each one signals a specific relationship between parts of a sentence.

```
Period    .   Full stop — end of thought
Comma     ,   Brief pause — separates items or clauses
Semicolon ;   Medium pause — connects related independent clauses
Colon     :   Forward look — introduces or explains
Dash      —   Strong interruption — emphasis or aside
Parenthesis ()  Whisper — supplementary information
```

In technical writing, clarity is the goal. Punctuation rules exist to serve clarity, not the other way around.

### Periods: Ending and Abbreviating

**Ending sentences:**

```text
Every instruction ends with a period.
Complete the setup by running the installer.
```

**In list items:**

```text
// Full sentences — periods at end
1. Install the dependencies.
2. Configure the database.
3. Run the migration.

// Fragments — no periods
Advantages:
- Faster deployment
- Reduced errors
- Easier maintenance
```

**Abbreviations:**

```text
e.g. (exempli gratia — for example)
i.e. (id est — that is)
etc. (et cetera — and so on)

// In modern technical writing, spell out "for example" and "that is"
Use plain text, for example, Markdown or reStructuredText.
```

**Periods with parentheses:**

```text
// Period inside if the parenthetical is a full sentence
The installation script creates a configuration file. (See the appendix for details.)

// Period outside if the parenthetical is a fragment
The installation script creates a configuration file (see the appendix).
```

### Commas: The Most Versatile Mark

**Oxford comma (serial comma) — always use in technical writing:**

```text
// Without Oxford comma — ambiguous
Supported formats: JSON, YAML and TOML.

// With Oxford comma — clear
Supported formats: JSON, YAML, and TOML.
```

**Comma before a coordinating conjunction (FANBOYS: For, And, Nor, But, Or, Yet, So):**

```text
The server parses the request, and the controller returns a response.
Install the dependencies, then run the build script.
```

**Comma after introductory elements:**

```text
After deploying to production, monitor the logs.
For large datasets, use the streaming API.
If the connection fails, retry with exponential backoff.
```

**Commas with nonrestrictive clauses:**

```text
// Nonrestrictive — extra information, needs commas
The main module, which handles authentication, must be updated.

// Restrictive — essential information, no commas
The module that handles authentication must be updated.
```

**Commas with coordinate adjectives:**

```text
// Coordinate (both modify "documentation" equally)
a clear, concise style guide

// Cumulative (first modifies the whole phrase)
a popular open source library
```

**Common comma errors in technical writing:**

```text
// Comma splice — joining independent clauses with only a comma
The server starts, it listens on port 8080.  // INCORRECT
The server starts, and it listens on port 8080.  // CORRECT
The server starts. It listens on port 8080.  // CORRECT
The server starts; it listens on port 8080.  // CORRECT

// Missing comma in a list of three or more
Supports JSON XML and YAML.  // INCORRECT
Supports JSON, XML, and YAML.  // CORRECT

// Missing comma after introductory phrase
By default the application runs on port 3000.  // INCORRECT
By default, the application runs on port 3000.  // CORRECT
```

### Semicolons: Connecting Related Clauses

Use a semicolon to join two related independent clauses without a conjunction:

```text
The synchronous API is simple; the streaming API is more powerful.
```

**Semicolons in complex lists:**

When list items themselves contain commas, use semicolons as separators:

```text
Supported configurations: a local SQLite database for development; a PostgreSQL
database for staging; and a managed RDS instance for production.
```

**Common semicolon mistakes:**

```text
// Not a complete clause after the semicolon
The tool supports three formats; JSON, YAML, and TOML.  // INCORRECT — use colon

// Overuse — semicolons are rare in casual technical writing
The function parses input; it validates the schema; it returns the result.  // AWKWARD
The function parses input, validates the schema, and returns the result.  // BETTER
```

### Colons: Introducing and Explaining

Use a colon to introduce a list, explanation, or example:

```text
The application requires three environment variables: DATABASE_URL, API_KEY, and PORT.

The deployment failed for one reason: the disk was full.

There are two ways to handle this: retry with backoff, or fall back to a cached response.
```

**Colon capitalization:**

```text
// If what follows is a complete sentence, capitalize it
The key principle: Always validate user input.

// If what follows is a list or fragment, do not capitalize
The tool supports: CSV, JSON, and Parquet.
```

**Colon vs. semicolon:**

```text
// Colon — introduces or explains
The solution is simple: restart the server.

// Semicolon — connects two equal ideas
The server is running; the database is connected.
```

### Dashes: Emphasis and Interruptions

**Em dash (—) — strong emphasis or abrupt break:**

```text
// Emphasis
There is one rule every developer should follow — validate all input.

// Interruption
The configuration file — located in the project root — specifies the database connection.

// For an abrupt shift
The function succeeded — or so we thought.
```

**En dash (–) — ranges and connections:**

```text
// Number ranges
See pages 42–58.
Version 2.0–3.0

// Connections
Client–server architecture
React–Redux integration
```

**Hyphen (-) — compound words and prefixes:**

```text
well-known port
high-level design
real-time data
object-oriented programming
```

**Em dash vs. colon:**

```text
// Colon — formal introduction
The fix is one command: npm update.

// Em dash — dramatic emphasis
The fix is one command — npm update.
```

**Spacing with dashes:**

```text
// No spaces around em dash
The result was unexpected—the server crashed.

// Some style guides prefer spaces
The result was unexpected — the server crashed.

// Pick one style and be consistent
```

### Parentheses: Asides and References

Use parentheses for supplementary information:

```text
The function resolves with the user data (see getUser in the API reference).

The migration script (located in the scripts/ directory) creates the initial schema.
```

**Parentheses vs. dashes vs. commas:**

```text
// Commas — least emphasis
The script, which runs during deployment, checks the database schema.

// Parentheses — deemphasize the interruption
The script (located in the scripts directory) checks the database schema.

// Dashes — emphasize the interruption
The script — usually run during deployment — checks the database schema.
```

**Nesting parentheses — avoid in technical writing:**

```text
// Poor — nested parentheses are confusing
The function (which requires authentication (JWT or OAuth)) returns the user data.

// Better — rewrite
The function, which requires authentication (JWT or OAuth), returns the user data.
```

### Quotation Marks: Terms and Titles

**Quoting terms:**

```text
The term "idempotent" means the operation produces the same result regardless of
how many times it is applied.
```

**Titles of short works:**

```text
Read the chapter "Deployment Strategies" in the documentation.
```

**Scare quotes — use sparingly:**

```text
The "agile" process was anything but agile.
```

**Punctuation with quotation marks:**

```text
// American style — period inside
The message says "Connection refused."

// British style — period outside
The message says "Connection refused".
```

For technical writing, use logical placement: punctuation inside if it belongs to the quoted material, outside if it belongs to the sentence.

### Apostrophes: Possession and Contractions

**Possessives:**

```text
// Singular possessor
The developer's machine    (one developer)
The project's deadline     (one project)

// Plural possessor
The developers' machines   (multiple developers)
The projects' deadlines    (multiple projects)
```

**Its vs. It's:**

```text
Its — possessive
The application and its dependencies.

It's — contraction of "it is"
It's important to validate input.

// Apostrophe errors are among the most visible mistakes
// The tool checks its configuration on startup. (possessive — no apostrophe)
// It's a tool that checks its configuration. (contraction + possessive)
```

**Contractions in technical writing:**

Contractions are acceptable in tutorials, guides, and informal documentation. Avoid in formal API reference documentation.

```text
// Acceptable in guides
You'll need to configure the database before starting the application.

// Avoid in reference docs
The function does not modify the original array.
```

### Hyphens: Compound Terms

**Compound adjectives before a noun:**

```text
well-documented code
high-performance computing
real-time data processing
up-to-date documentation
```

**Compound adjectives after a noun — no hyphen:**

```text
The code is well documented.
The documentation is up to date.
```

**Prefixes — hyphenate to avoid ambiguity:**

```text
re-cover (cover again) vs. recover (regain)
re-sign (sign again) vs. resign (quit)
multi-platform
self-contained
```

**Numbers and hyphens:**

```text
twenty-one
64-bit architecture
3-factor authentication
first-class function
```

**Suspended hyphens:**

```text
Supports both 32- and 64-bit architectures.
```

### Brackets: Editorial Insertions

Use square brackets for editorial clarifications in quoted material:

```text
"The function [parseInput] returns the parsed result."
```

**In code documentation:**

```text
/**
 * Updates the user profile.
 * @param {string} userId - The user's unique identifier.
 */
function updateProfile(userId) { ... }
```

**Angle brackets in placeholder text:**

```text
Replace <your-api-key> with your actual API key.
```

### Punctuation in Code Documentation

**Comments and docstrings:**

```text
// Inline comment — lowercase, period if it's a complete sentence
// validate the input before processing
return validateInput(data);

// Complete sentence comment
// This function validates the input before processing.
return validateInput(data);
```

**JSDoc/TSDoc punctuation:**

```text
/**
 * Fetches a user by ID.                  // Complete sentence, period
 * @param id - The user's unique ID.      // Description, period
 * @returns The user object or null.      // Description, period
 */
```

**Error messages:**

```text
// Good — clear, with period
throw new Error("Connection refused: port 8080 is already in use.");

// Avoid technical jargon in user-facing messages
throw new Error("E_CONNREFUSED");  // unhelpful
```

## Glossary

| Term | Definition |
|------|------------|
| Oxford comma | A comma before the final item in a list of three or more |
| Comma splice | Joining two independent clauses with only a comma |
| Independent clause | A group of words that can stand alone as a sentence |
| Coordinating conjunction | A word that joins equal elements: for, and, nor, but, or, yet, so |
| Nonrestrictive clause | A clause that adds extra information and can be removed |
| Restrictive clause | A clause essential to the meaning of the sentence |
| Em dash | A long dash used for emphasis or interruption (—) |
| En dash | A medium dash used for ranges and connections (–) |
| Hyphen | A short dash used in compound words and modifiers (-) |
| Compound adjective | Two or more words that together modify a noun |
| Serial comma | Another term for the Oxford comma |
| Scare quotes | Quotation marks used to cast doubt on a term |

## Quick References

- [Microsoft Style Guide: Punctuation](https://learn.microsoft.com/en-us/style-guide/punctuation/) — official guidance
- [Google Developer Documentation Style Guide: Punctuation](https://developers.google.com/style/punctuation)
- [The Chicago Manual of Style: Punctuation](https://www.chicagomanualofstyle.org/book/ed17/part2/ch06/toc.html)
- [MDN: Writing Style Guide](https://developer.mozilla.org/en-US/docs/MDN/Writing_guidelines/Writing_style_guide)

## Next Steps

- [Active vs. Passive Voice](active-vs-passive.md) — choosing the right voice for clarity
- [Technical Style Guides](style-guides.md) — applying consistent punctuation rules
- [Sentence Structure](sentence-structure.md) — the foundation for punctuation
