# Pragmatics & API Design

## Description

Pragmatics is the study of how context shapes meaning in communication. For software developers, pragmatic principles explain why some APIs feel intuitive while others cause confusion. Understanding implicature, politeness, and context helps you design interfaces that communicate intent clearly and reduce user error.

## Prerequisites

- [Semantics](semantics.md) — meaning in language and code, the foundation for understanding pragmatics

## Table of Contents

- [What Is Pragmatics?](#what-is-pragmatics)
- [Pragmatics vs. Semantics](#pragmatics-vs-semantics)
- [Implicature and Inference](#implicature-and-inference)
- [Grice's Maxims](#grice-maxims)
- [Applying Grice to API Design](#applying-grice-to-api-design)
- [Politeness Theory](#politeness-theory)
- [Politeness in UX and API Design](#politeness-in-ux-and-api-design)
- [Context and Conventions](#context-and-conventions)
- [API Discoverability](#api-discoverability)
- [Affordances in Interfaces](#affordances-in-interfaces)
- [Readability as a Pragmatic Concern](#readability-as-a-pragmatic-concern)
- [Principle of Least Astonishment](#principle-of-least-astonishment)
- [Presupposition and Definiteness](#presupposition-and-definiteness)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### What Is Pragmatics?

Pragmatics studies how context contributes to meaning. It answers: why does the same sentence mean different things in different situations?

| Layer | Example |
|-------|---------|
| Syntax | "Can you open the door?" — question structure |
| Semantics | Question about physical ability |
| Pragmatics | Request to open the door |

**The pragmatic gap:** "It's cold in here." Literal: temperature statement. Pragmatic: request to close the window.

**Why pragmatics matters for developers:**

| Activity | Pragmatic concern |
|----------|------------------|
| API design | Does function name match behavior? |
| Error messages | Does the message help recovery? |
| Code comments | Does it explain why, not just what? |
| Documentation | Does it address the reader's context? |

### Pragmatics vs. Semantics

Semantics: what the words mean in isolation. Pragmatics: what the speaker means by using those words in this context.

| Dimension | Semantics | Pragmatics |
|-----------|-----------|------------|
| Focus | Meaning of words | Meaning in context |
| Truth-conditional | True or false | Appropriateness |
| Context | Fixed meaning | Varies with context |
| Inferences | Entailments | Implicatures (cancellable) |

"Could you pass the salt?" at dinner = request. In a test kitchen = physical ability question. In teaching = example of indirect speech act.

### Implicature and Inference

**Conversational implicature:** Something implied but not explicitly stated.

```
A: "Are you coming to the meeting?"
B: "I have a deadline tomorrow."
Implicature: B is not coming.
```

**Properties of implicatures:** Cancellable ("I have a deadline, but I'll come anyway"), reinforceable, non-detachable, calculable.

**Types:**

| Type | Description | Example |
|------|-------------|---------|
| Generalized | No special context needed | "She walked into a house." (not her own) |
| Particularized | Requires context | "It's 5:00." (in a meeting: we should leave) |
| Scalar | Based on value scale | "Some tests passed." (not all) |

**Scalar implicature in programming:** "Some fields are invalid" implies not all fields are invalid. Be precise.

### Grice's Maxims

**Cooperative Principle:** Make your contribution as required, at the right stage, for the accepted purpose.

| Maxim | Rule | API violation |
|-------|------|---------------|
| **Quantity** | Be as informative as needed | "The function returns data." (too vague) |
| **Quality** | Be truthful | "This is thread-safe." (when it isn't) |
| **Relation** | Be relevant | File error about network |
| **Manner** | Be clear, avoid ambiguity | `HmacSHA256` without explanation |

#### Maxim of Quantity

```
// Too little:
def process(data): pass

// Too much:
def process(data, verbose=False, dry_run=False, skip_validation=False,
            use_cache=True, retry_on_failure=False, timeout=30): pass

// Just right:
def process_transaction(transaction: Transaction) -> Receipt: pass
```

#### Maxim of Quality

```python
# Misleading:
result = save_user(user)  # Returns boolean, not user

# Truthful:
saved_user = save_user(user)  # Returns the saved user
```

#### Maxim of Relation

```python
# Irrelevant:
import os
os.get_user_input()  # Why in os?

# Relevant:
import readline
readline.get_user_input()
```

#### Maxim of Manner

```python
# Obscure:
def cvt(t): return t * 9 / 5 + 32

# Clear:
def celsius_to_fahrenheit(celsius: float) -> float:
    return celsius * 9 / 5 + 32
```

### Applying Grice to API Design

| Maxim | API question |
|-------|-------------|
| Quantity | Does this function name reveal everything needed? |
| Quality | Does the function do what its name says? |
| Relation | Is this where users would look for it? |
| Manner | Is naming clear, consistent, and unambiguous? |

### Politeness Theory

Brown and Levinson's theory of **face** (public self-image):

**Face-threatening acts (FTAs):**

| Act | Threatens | Example |
|-----|-----------|---------|
| Direct request | Negative face (autonomy) | "Close the window." |
| Criticism | Positive face (esteem) | "Your code is buggy." |

**Politeness strategies:**

| Strategy | Example | Face cost |
|----------|---------|-----------|
| Bald on record | "Close the window." | Highest |
| Positive politeness | "Let's close the window." | Medium |
| Negative politeness | "Could you close the window?" | Lower |
| Off record (hint) | "It's cold in here." | Lowest |

### Politeness in UX and API Design

```
// Impolite:
Error 0x7F: Invalid input.

// Polite:
The date format is invalid. Expected: YYYY-MM-DD. You entered: "04-13-2025".
```

**Code review politeness:**

```
// Impolite:
"This is wrong. Rewrite it."

// Polite:
"This loop has O(n^2) complexity. Consider using a hash map for O(1) lookups."
```

**API politeness:**

| Principle | Impolite | Polite |
|-----------|----------|--------|
| Don't assume | Silent TypeError | Validates with clear message |
| Explain why | Generic "Error" | Specific exception with context |
| Give control | `auto_save()` no params | `save(path: Optional[str] = None)` |
| Be consistent | Inconsistent conventions | Consistent patterns throughout |

### Context and Conventions

**Common ground:** Shared knowledge assumed in communication.

```
Dev 1: "The build failed."
Dev 2: "Again? I'll check the CI config."

Common ground: CI = Continuous Integration, specific CI system,
build process details, CI config meaning.
```

**Conventions as shared context:**

```python
# Python — with statement for resource management
with open("file.txt", "r") as f:
    data = f.read()
```

```go
// Go — error as return value
file, err := os.Open("file.txt")
if err != nil { return err }
```

```rust
// Rust — Result type
let file = File::open("file.txt")?;
```

### API Discoverability

Discoverability is the ease of finding the right API for a task.

**Principles:**

```python
# Group related functionality:
data_processor.process()
data_processor.analyze()
data_processor.format()

# Signature reveals intent:
def calculate_discount(price: float, coupon_code: str = None,
                       is_vip: bool = False) -> float: pass

# Consistency enables prediction:
user = get_user(id)
order = get_order(id)  # Pattern enables: invoice = get_invoice(id)
```

### Affordances in Interfaces

**Affordances** suggest how something should be used. **Signifiers** communicate where the affordance is.

```python
# Button affordance (no params, call and get result):
current_time = time.now()

# Builder affordance (chainable):
query = db.select("name").from_table("users").where("age > 18")

# Type annotations as signifiers:
def divide(dividend: float, divisor: float) -> float: pass

# Anti-affordance (looks useful, isn't):
def save_user(user): return True  # Surprise! Returns bool
```

### Readability as a Pragmatic Concern

Readable code communicates intent effectively.

```python
# Unreadable — what is the intent?
def f(x):
    r = 0
    for i in range(1, int(x**0.5) + 1):
        if x % i == 0:
            r += 1
            if i != x // i:
                r += 1
    return r

# Readable — intent is clear
def count_divisors(n: int) -> int:
    """Count the positive divisors of n."""
    count = 0
    for i in range(1, int(sqrt(n)) + 1):
        if n % i == 0:
            count += 1
            if i != n // i:
                count += 1
    return count
```

**Pragmatic signals in readable code:** Function names (what intent?), variable names (what does this represent?), type hints (what kinds of values?), docstrings (why does this exist?).

### Principle of Least Astonishment

A system should behave as most users expect.

**POLA violations:**

```python
# Astonishing: name says one thing, does another
delete_user(id)  # Actually deactivates

# Astonishing: side effects in getter
def get_user(id):
    create_user(id)  # Side effect!
    return database.find(id)

# Astonishing: inconsistent parameter order
func drawLine(x1 int, x2 int, y1 int, y2 int)  # Swapped x/y pairing
```

**POLA checklist:** Does naming match behavior? Are parameters in natural order? Is return type predictable? Are side effects absent?

### Presupposition and Definiteness

**Presuppositions:** Background assumptions for an utterance to make sense.

```python
def get_user(id):
    return database.query("SELECT * FROM users WHERE id = ?", id)

# Presupposes: database connected, users table exists, user with this id exists
```

**Violating presuppositions:**

```python
# Presupposition violation:
get_user(999)  # Returns None or raises? Unknown.

# Better — explicit:
def get_user(id: int) -> Optional[User]:
    """Return user or None if not found."""
    ...
```

**Definiteness:** "The" marks identifiable (given). "A" marks new.

```python
create_user(data)  # "a" user (new)
delete_user(id)    # "the" user (specific)
update_user(id, data)  # "the" user (specific)
```

## Study Cases

### Case 1: Redesigning API with Grice's Maxims

Original:

```python
class Logger:
    def log(self, msg, lev): pass
```

Problems: `lev` obscures valid values. Name `log` is generic — file, stdout, network? Manner violation with abbreviation.

Redesigned:

```python
import logging
logger = logging.getLogger(__name__)
logger.info("Server started on port %d", port)
logger.error("Connection failed: %s", error)
```

Quantity: `info`, `error` describe level. Quality: function does what name says. Relation: methods on Logger. Manner: clear and conventional.

### Case 2: Error Message Redesign

Original: `ERROR: Invalid input` (blaming, no guidance, face-threatening).

Redesigned: `The email address "notanemail" does not appear valid. Expected format: user@example.com.`

Face concerns addressed: Positive face — explains objectively. Negative face — gives options. Cooperative principle — provides format and action.

### Case 3: Context Collapse in Public API

Internal: `def get_users(filters, q, sort, fields): """Get users."""`

Problems: Unclear formats, undocumented return type, assumes internal knowledge.

Public:

```python
def list_users(
    page: int = 1,
    per_page: int = 20,
    role: Optional[UserRole] = None,
    search: Optional[str] = None,
) -> PaginatedResponse[User]:
    """List users with pagination and filtering."""
```

## Examples

### Example 1: Presupposition Failure

```python
API_KEY = os.environ["API_KEY"]
# KeyError: 'API_KEY' — opaque

# Better:
API_KEY = os.environ.get("API_KEY")
if API_KEY is None:
    raise RuntimeError("API_KEY not set. Create .env file with: API_KEY=your_key")
```

### Example 2: Scalar Implicature

```python
# Original — triggers unwanted implicature
errors.append("Some fields are required.")
# Implicit: some (but not all) fields

# Better — no unwanted implicature
errors.append("The 'name' field is required.")
```

### Example 3: POLA Violation with Side Effects

```python
class Cache:
    def get(self, key):
        if key not in self._store:
            self._store[key] = self._load_from_db(key)  # Astonishing side effect
        return self._store[key]

# Unastonishing:
class Cache:
    def get(self, key):
        return self._store.get(key)
    def load(self, key):
        value = self._load_from_db(key)
        self._store[key] = value
        return value
```

### Example 4: Politeness in Error Messages

```
Impolite:
Error!

Polite:
Could not find config at '/etc/app/config.yaml'.
This usually happens when:
  1. The app has not been configured (run 'app init')
  2. The file was moved or deleted
  3. Permissions are incorrect
```

### Example 5: Pragmatic Analysis of Disagreement

```
Dev A: "We should use MongoDB for this."
Dev B: "The data is highly relational."

Pragmatic analysis:
  B's utterance implies: "MongoDB is not suitable."
  This is conversational implicature (not stated, but inferred).
  Violates Manner (indirect) but preserves positive face.
```

## Glossary

| Term | Definition |
|------|------------|
| Affordance | Property suggesting how something should be used |
| Common ground | Shared knowledge between participants |
| Cooperative principle | Participants cooperate to communicate effectively |
| Face | Public self-image in social interaction |
| Grice's maxims | Quantity, Quality, Relation, Manner |
| Implicature | Implied meaning beyond literal words |
| Inference | Deriving implied meaning from context |
| Maxim of Manner | Be clear, brief, avoid obscurity |
| Maxim of Quality | Be truthful |
| Maxim of Quantity | Provide right amount of information |
| Maxim of Relation | Be relevant |
| Negative face | Desire for autonomy |
| POLA | Principle of Least Astonishment |
| Politeness theory | Brown and Levinson's face management theory |
| Positive face | Desire for approval |
| Pragmatics | How context affects meaning |
| Presupposition | Background assumption for meaning |
| Scalar implicature | Implicature on a value scale (all, some, none) |
| Signifier | Cue for using an affordance |

## Quick References

- [Grice: Logic and Conversation (1975)](https://doi.org/10.1163/9789004368811_003)
- [Brown & Levinson: Politeness: Some Universals in Language Usage](https://www.cambridge.org/us/academic/subjects/language-and-linguistics/sociolinguistics/politeness-some-universals-language-usage)
- [Norman: The Design of Everyday Things](https://www.jnd.org/books/design-of-everyday-things-revised/)
- [Bloch: Effective Java](https://www.oreilly.com/library/view/effective-java-3rd/9780134686097/)
- [Martin: Clean Code](https://www.oreilly.com/library/view/clean-code/9780136083238/)
- [Levinson: Pragmatics](https://www.cambridge.org/us/academic/subjects/language-and-linguistics/semantics-and-pragmatics/pragmatics)
- [Postel's Law (Robustness Principle)](https://en.wikipedia.org/wiki/Robustness_Principle)

## Next Steps

- [API Reference Documentation](../technical-writing/api-docs.md) — documenting APIs for clarity and discoverability
