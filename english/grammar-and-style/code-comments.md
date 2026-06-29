# Grammar for Code Comments & Commit Messages

## Description

Code comments and commit messages are a form of technical communication read by humans. Applying grammar rules to them improves code readability, maintainability, and team collaboration. This document covers sentence structure in comments, docstring conventions, commit message standards, and code review etiquette.

## Prerequisites

- [Sentence Structure for Technical Writing](sentence-structure.md) — clauses, modifiers, and parallelism for clear comments
- [Conciseness & Eliminating Jargon](conciseness.md) — keeping comments and messages short

## Table of Contents

- [Complete Sentences vs. Fragments](#complete-sentences-vs-fragments)
- [Grammar Rules for Comments](#grammar-rules-for-comments)
- [Docstring Conventions](#docstring-conventions)
- [Comment Style per Language](#comment-style-per-language)
- [Commit Message Standards](#commit-message-standards)
- [Conventional Commits](#conventional-commits)
- [Code Review Comment Grammar](#code-review-comment-grammar)
- [TODO and FIXME Conventions](#todo-and-fixme-conventions)
- [Documentation in Code vs. External Docs](#documentation-in-code-vs-external-docs)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Complete Sentences vs. Fragments

Comments are part of the codebase and must follow standard English grammar. Different contexts demand different degrees of grammatical completeness.

**Use complete sentences for:**

- Docstrings and public API documentation
- Block comments explaining complex logic
- Descriptions of non-obvious behavior

```python
# Complete sentence
# The cache layer invalidates entries every 300 seconds.
```

**Use fragments for:**

- Inline annotations that label what a line does
- Variable or parameter descriptions in short form
- TODO markers

```python
# Fragment — acceptable inline annotation
count += 1  # increment visitor count
```

**Mixing fragments and complete sentences inconsistently** creates confusion:

```python
# Inconsistent — fragments and full sentences mixed arbitrarily
# Process the input file.
# validating each row
# Then writes output.

# Consistent — all complete sentences
# Process the input file. Validate each row. Write the output.
```

### Inline Comments

Inline comments appear on the same line as code. Start with a lowercase letter (treat as a continuation of the code line), omit the period for fragments, and align comments for readability when they appear in blocks.

```python
results  = query(database)     # execute the search query
count    = len(results)        # count matching records
progress = count / total * 100 # calculate completion percentage
```

When inline comments become too long, move them to a line above:

```python
# Send the payload and wait for acknowledgment before proceeding.
client.send(payload)
```

### Block Comments

Block comments explain intent, algorithm choices, or non-obvious behavior. Use full sentences with correct punctuation.

```c
/*
 * This function implements the QuickSelect algorithm to find the
 * k-th smallest element in an unsorted array. It uses Hoare's
 * partition scheme for better performance on duplicate values.
 *
 * The algorithm runs in O(n) average time and O(n^2) worst case.
 */
int quick_select(int arr[], int n, int k) {
    ...
}
```

```python
# When reading configuration from environment variables, the loader
# first checks for a .env file in the project root. If no .env file
# exists, it falls back to the system environment.
# Values are parsed as strings by default. The loader will coerce
# boolean-like values to actual booleans unless STRICT_MODE is set.
```

### Docstring Conventions

Docstrings are formal documentation embedded in code. They follow language-specific conventions.

**Python — PEP 257:**

```python
def calculate_interest(principal: float, rate: float, time: int) -> float:
    """Calculate compound interest over a given time period.

    Uses the formula A = P(1 + r)^t where P is the principal amount,
    r is the annual interest rate, and t is the number of years.

    Args:
        principal: The initial investment amount.
        rate: The annual interest rate as a decimal (e.g., 0.05 for 5%).
        time: The number of years the money is invested.

    Returns:
        The total amount after compounding, including principal.

    Raises:
        ValueError: If principal or rate is negative.
    """
    ...
```

**JSDoc:**

```javascript
/**
 * Fetches paginated results from the API endpoint.
 * Handles rate limiting by retrying with exponential backoff when
 * the server returns a 429 status code.
 *
 * @param {string} endpoint - The API path.
 * @param {object} [options] - Optional query parameters.
 * @returns {Promise<object[]>} A promise resolving to merged results.
 */
async function fetchAllPages(endpoint, options = {}) {
    ...
}
```

**GoDoc:**

```go
// ParseConfig reads and parses a YAML configuration file.
//
// It reads the file at the given path, expands environment variable
// references, and unmarshals the result into the provided struct.
// Returns an error if the file cannot be read or contains invalid YAML.
func ParseConfig(path string, cfg interface{}) error {
    ...
}
```

**Rust doc comments:**

```rust
/// Parses command-line arguments into a Config struct.
///
/// Supports flags for input file (`-i`), output format (`-f`), and
/// verbose mode (`-v`). Unknown flags cause the function to print
/// usage information and exit the process.
///
/// # Panics
/// Panics if required arguments are missing and no default is set.
pub fn parse_args() -> Config {
    ...
}
```

### Comment Style per Language

```
// C, C++, Java, JS/TS:  // single-line  /* block */  /** doc */
# Python, Ruby, Shell:   # single-line  (no native multi-line)
// Go:                   // only (block = sequential // lines)
```

Use the comment syntax idiomatic to your language. Mixing styles within the same codebase creates inconsistency.

### Grammar Rules for Comments

**Complete sentences should start with a capital letter:**

```python
# incorrect
# the cache expires after 5 minutes

# correct
# The cache expires after 5 minutes.
```

**Fragments should start lowercase:**

```python
# incorrect
# Increments counter

# correct
# increment counter
```

**Use consistent verb tense:**

```python
# Inconsistent — switches between present and imperative
# Open the file, reads the first line, then close it.

# Consistent — all present tense
# Opens the file, reads the first line, and closes it.

# Consistent — all imperative (instructions)
# Open the file, read the first line, and close it.
```

**Use articles (a, an, the) properly:**

```python
# Missing articles
# Query returns list of users from database.

# With articles
# The query returns a list of users from the database.
```

**Subject-verb agreement in comments:**

```python
# Incorrect
# A set of configuration values are loaded.

# Correct
# A set of configuration values is loaded.
```

**Pronoun reference must be clear:**

```python
# Unclear — "it" could refer to the parser or the schema
# The parser reads the schema. It must be valid.

# Clear
# The parser reads the schema. The schema must be valid.
```

**Avoid ambiguous modifiers:**

```python
# Ambiguous — does "only" apply to the function or the validation?
# This function only validates user input.

# Clear
# This function validates only user input.
```

### Commit Message Standards

A commit message has three parts: subject line (50 chars max), body (72 chars wrapped), and optional footer.

**Subject line rules:**

- Use the imperative mood ("Add" not "Added" or "Adding")
- Capitalize the first word only
- Do not end with a period
- Keep under 50 characters
- Be specific ("Add validation for email input fields" not "fix stuff")

Apply the "This commit will..." test:

```
This commit will: Add validation          → correct
This commit will: Added validation        → incorrect
This commit will: Fix null pointer        → correct
This commit will: Fixed null pointer      → incorrect
```

**Body grammar:**

Use full sentences with correct punctuation. Explain the motivation and trade-offs, not just the mechanics. Use blank lines to separate paragraphs.

```
Add retry logic for database connections

The connection pool occasionally drops connections during high load.
This commit adds exponential backoff retry that retries failed
connections up to three times before propagating the error.
```

**Footer conventions:**

```
Fixes #1234
BREAKING CHANGE: The `createUser` function now requires an email parameter.
```

### Conventional Commits

The Conventional Commits specification adds a structured prefix to the subject line.

```
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
```

**Types:**

```
feat:     new feature              fix:      bug fix
docs:     documentation            style:    formatting (no logic change)
refactor: code restructuring       perf:     performance improvement
test:     adding/fixing tests      chore:    maintenance
ci:       CI configuration
```

**Breaking changes** go in the footer:

```
feat(api): change endpoint response format

BREAKING CHANGE: The /users endpoint now returns paginated results.
```

**Scopes** indicate the affected module. Use nouns or noun phrases:

```
feat(auth): add OAuth2 token refresh
fix(parser): handle empty input
```

### Code Review Comment Grammar

Code review comments must be precise and respectful. Use complete sentences for suggestions. Use questions or conditional language to soften directives.

```
// Too direct
Add error handling.

// Better — invites discussion
What do you think about adding error handling here? If the
database call fails, this function will throw an unhandled
exception.
```

**Common review patterns:**

```
Nit: There is a missing space after the colon.
Suggestion: Extract the validation logic into a separate function.
Issue: If the config file is missing, this line throws an error.
```

Use bullet points for multiple observations. When addressing feedback, be clear and actionable: "Fixed. Increased the timeout from 30s to 60s."

### TODO and FIXME Conventions

TODO and FIXME markers must include enough context for the next developer.

**Format:**

```python
# TODO: <action> — <reason or context> (<tracking reference>)
# FIXME: <problem> — <expected fix> (<tracking reference>)
```

**Rules:**

- Always include a reason or tracking reference
- Use the imperative mood for the action
- Capitalize the first word and do not use a period
- Include an owner name or issue number in parentheses

```python
# Poor — no context
# TODO: fix this

# Good
# TODO: refactor this function — violates the single
#       responsibility principle (JIRA-789)

# FIXME: query returns incorrect results when the date range
#        spans DST boundary — fix: use UTC timestamps (GH-901)
```

**HACK and XXX markers** indicate suboptimal solutions that should be revisited:

```python
# HACK: This import forces module signal handler registration
#       before app startup. Remove after migration.
# XXX: Investigate replacing this third-party dependency with
#      the standard library.
```

### Documentation in Code vs. External Docs

**Put in code comments:** design rationale, non-obvious behavior, algorithm explanations, limitations, security considerations.

```python
# The token must be validated on every request, not cached,
# because the authentication service may revoke tokens at
# any time. Caching would create a window of vulnerability.
```

**Put in external documentation:** installation, API references, tutorials, architecture overviews, contribution guidelines.

```python
# Good in code: explains design decision
# Uses the Builder pattern to construct complex query objects.
# The builder ensures that required parameters are set.

# Bad in code: belongs in README
# To install, run pip install mypackage. Then import it...
```

**Docstring vs. external docs:**

Docstrings (extracted by Sphinx, JSDoc, godoc) describe what a function does, its parameters, return values, and exceptions. External documentation explains how functions compose together and provides conceptual background.

```python
def create_user(name: str, email: str, age: int) -> User:
    """Create a new user in the database.

    Args:
        name: The user's display name.
        email: The user's email address.
        age: The user's age in years.

    Returns:
        A User object with the generated ID.

    Raises:
        DuplicateEmailError: If the email already exists.
    """
```

## Study Cases

### Case 1: Refactoring a Legacy Comment Block

```python
# Original — inconsistent grammar
# this function does something with the data. It checks if the data is
# valid. If it is valid, then process it. If not valid, then skip. Also
# logging everything. the function returns the number of records processed
# and also maybe an error.

# Revised — consistent grammar, clear structure
# Validate the incoming data and process valid records.
# For each record, this function checks that required fields are present
# and that types match the expected schema. Valid records are processed
# and counted. Invalid records are skipped and logged.
#
# Returns:
#   A tuple of (processed_count, error_list).
```

### Case 2: Converting a Bad Commit History

```
Before:
did stuff, fixed bug, WIP, updated something, added new feature, fix

After:
feat: add user profile page with avatar upload
fix: resolve race condition in session manager
refactor: extract pagination logic into utility module
docs: update README with deployment instructions
```

### Case 3: Improving a Code Review Comment

```text
Original: u should handle the error here.

Improved: Consider handling the error here. If the database connection
fails, this function returns a 500 error without logging the cause.
Wrap this call in a try-except block and log the exception.
```

## Examples

### Example 1: Inline Comment Consistency

```python
# Inconsistent
x = get_config()   # loads configuration
y = validate(x)    # Validating the config
z = process(y)     # Process

# Consistent (fragments)
x = get_config()   # load configuration
y = validate(x)    # validate configuration
z = process(y)     # process configuration
```

### Example 2: Commit Message Body

```text
feat: add Redis caching for database queries

Querying the database on every request caused high latency under
load. This commit adds a Redis cache layer that stores query
results for 60 seconds. Cache entries are invalidated when the
underlying data changes.
```

### Example 3: JSDoc with Full Sentences

```javascript
/**
 * Validates and sanitizes user input before storage.
 * Applies a whitelist of allowed HTML tags and strips others.
 *
 * @param {string} input - The raw user input to sanitize.
 * @returns {string} The sanitized, safe output string.
 */
function sanitizeInput(input) { ... }
```

### Example 4: FIXME With Full Context

```python
# FIXME: Race condition in cache invalidation — two concurrent
#        requests can both check the cache, get a miss, and then
#        both recompute the same expensive query. The fix is to
#        implement a mutex around the check-and-compute block.
```

### Example 5: Commenting a Complex Algorithm

```python
# Implements binary search on a sorted array.
#
# At each step, the algorithm compares the target value to the
# middle element of the search interval. If the target matches,
# the index is returned. If the target is smaller, the search
# continues in the left half. If larger, it continues in the
# right half.
#
# This implementation handles duplicate values by returning the
# index of the first occurrence, using a left-biased comparison.
#
# Time complexity: O(log n)
# Space complexity: O(1)
def binary_search(arr: list, target: int) -> int:
    left, right = 0, len(arr) - 1
    result = -1
    while left <= right:
        mid = (left + right) // 2
        if arr[mid] == target:
            result = mid
            right = mid - 1  # continue searching left for first occurrence
        elif arr[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    return result
```

## Glossary

| Term | Definition |
|------|------------|
| Block comment | A comment that spans multiple lines, usually explaining a section of code |
| Body (commit) | The part of a commit message after the subject line, providing context |
| Conventional Commits | A specification for adding structured prefixes to commit messages |
| Docstring | Documentation embedded in source code, extractable by tools like Sphinx or JSDoc |
| Footer (commit) | The final section of a commit message, containing references or metadata |
| FIXME | A marker indicating a known bug that needs fixing |
| Fragment | An incomplete sentence, acceptable in certain comment contexts |
| HACK | A marker indicating a suboptimal solution that should be revisited |
| Imperative mood | A verb form that expresses a command or request (e.g., "Add", "Fix") |
| Inline comment | A comment placed on the same line as code |
| JSDoc | The documentation standard for JavaScript and TypeScript |
| PEP 257 | Python's docstring convention specification |
| Scope | A conventional commits element indicating the affected module |
| Subject line | The first line of a commit message, summarizing the change |
| TODO | A marker indicating a task or improvement to be completed later |
| XXX | A marker indicating something problematic or suspicious |

## Quick References

- [Conventional Commits Specification](https://www.conventionalcommits.org/) — the official specification
- [PEP 257 — Docstring Conventions](https://peps.python.org/pep-0257/) — Python docstring standards
- [JSDoc Reference](https://jsdoc.app/) — JSDoc tag reference
- [GoDoc Documentation](https://go.dev/doc/comment) — Go comment conventions
- [chris.beams.io — How to Write a Git Commit Message](https://chris.beams.io/posts/git-commit/) — the definitive guide
- [Google Code Review Guidelines](https://google.github.io/eng-practices/review/) — code review best practices at Google
- [RuboCop Comment Documentation](https://rubocop.org/) — Ruby comment conventions
- [Rust RFC 1574 — Doc Comments](https://github.com/rust-lang/rfcs/blob/master/text/1574-more-api-documentation-conventions.md) — Rust doc conventions
- [Microsoft C# Comment Conventions](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/language-specification/documentation-comments)

## Next Steps

- [Technical Style Guides: Microsoft, Google, Apple](style-guides.md) — how style guides govern comment and commit conventions
- [Punctuation for Clarity](punctuation.md) — proper punctuation in comments and documentation
