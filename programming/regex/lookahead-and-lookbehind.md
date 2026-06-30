# Lookahead & Lookbehind Assertions

## Description

Lookahead and lookbehind — collectively called lookaround — are zero-width assertions that check for patterns before or after the current position without consuming characters. They enable context-sensitive matching that would otherwise require complicated capture-group workarounds.

## Prerequisites

- [Groups, Capture & Backreferences](groups-and-backreferences.md) — group syntax, non-capturing groups, backtracking basics

## Table of Contents

- [What Zero-Width Means](#what-zero-width-means)
- [Positive Lookahead](#positive-lookahead)
- [Negative Lookahead](#negative-lookahead)
- [Positive Lookbehind](#positive-lookbehind)
- [Negative Lookbehind](#negative-lookbehind)
- [Fixed-Width vs Variable-Width Lookbehind](#fixed-width-vs-variable-width-lookbehind)
- [Chaining Assertions](#chaining-assertions)
- [Lookaround Inside Groups](#lookaround-inside-groups)
- [Performance Impact of Assertions](#performance-impact-of-assertions)
- [Simulating Lookbehind with \K](#simulating-lookbehind-with-k)
- [Common Pitfalls](#common-pitfalls)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### What Zero-Width Means

Most regex tokens consume characters — they advance the engine's position in the string. A zero-width assertion matches a *position* between characters without consuming any input. The engine's position does not advance.

```python
import re
# \b is a zero-width assertion — matches a word boundary position
# It does not consume any characters
pattern = r"\bcat\b"
print(re.search(pattern, "cat"))         # match
print(re.search(pattern, "scat"))        # no match — no boundary before 'c'
```

Lookaround assertions follow the same principle. They inspect the text ahead or behind the current position and return true or false. If true, the engine continues from the same position.

```
String:   a  b  c  d  e
Position: 0  1  2  3  4  5

Pattern:   b  (?=c)  c  d
Engine at pos 1: b matches, pos → 2
Engine at pos 2: lookahead checks if next is c  →  yes, pos stays at 2
Engine at pos 2: c matches, pos → 3
Engine at pos 3: d matches, pos → 4  →  full match "bcd"
```

The lookahead confirmed the presence of `c` without consuming it, then the pattern consumed it normally.

### Positive Lookahead

Positive lookahead `(?=...)` asserts that the text immediately following the current position matches the sub-pattern. The sub-pattern is not consumed.

```python
import re
# Match a number only if followed by "px"
pattern = r"\d+(?=px)"
print(re.search(pattern, "12px"))      # matches '12'
print(re.search(pattern, "12em"))      # no match
```

The matched text is only `"12"` — the `"px"` is not included in the match.

```python
import re
# Extract all numbers that are followed by a unit
text = "margin: 12px; padding: 8px; width: 100%;"
matches = re.findall(r"\d+(?=px)", text)
print(matches)   # ['12', '8']
# '100' is not included — it's followed by '%', not 'px'
```

**Practical use: matching fields in structured data**

```python
import re
log = "time=12:34, status=error, code=500"
m = re.search(r"status=(?P<val>\w+)", log)
if m:
    print(m.group("val"))    # 'error'
```

### Negative Lookahead

Negative lookahead `(?!...)` asserts that the text immediately following the current position does *not* match the sub-pattern.

```python
import re
# Match numbers not followed by "px"
pattern = r"\d+(?!px)"
print(re.search(pattern, "12em"))      # matches '12'
print(re.search(pattern, "12px"))      # no match
print(re.search(pattern, "123"))       # matches '12' (greedy stops at 12? Actually 123)
```

Wait — negative lookahead with greedy quantifiers backtracks unexpectedly:

```python
import re
# \d+ greedily matches "123", then (?!px) checks — "px" follows, assertion fails
# Engine backtracks: \d+ gives up "3", now at "12"
# (?!px) checks "3px" — next char is "3", not "p", assertion succeeds
pattern = r"\d+(?!px)"
m = re.search(pattern, "123px")
print(m.group())     # '12' — unexpected!
```

Use `\d+\b(?!px)` or an atomic group to prevent this backtracking.

**Practical use: excluding unwanted patterns**

```python
import re
# Match words that are NOT followed by "ing"
text = "running, walked, swimming, talks"
pattern = r"\b\w+(?!ing)\b"
print(re.findall(pattern, text))
# ['walked', 'talks'] — 'running' and 'swimming' excluded
```

### Positive Lookbehind

Positive lookbehind `(?<=...)` asserts that the text immediately preceding the current position matches the sub-pattern.

```python
import re
# Match a number only if preceded by "$"
pattern = r"(?<=\$)\d+"
print(re.search(pattern, "price $12"))   # matches '12'
print(re.search(pattern, "price 12"))    # no match
```

The matched text is only `"12"` — the `$` is not included.

```python
import re
# Extract all numbers after "error:"
log = "info: starting\nerror: file not found\ninfo: done"
pattern = r"(?<=error:)\s.*"   # Actually this matches whitespace then rest
# Better: capture the message after "error:"
pattern = r"(?<=error:)\s*(.+)"
for m in re.finditer(pattern, log):
    print(m.group(1))    # 'file not found'
```

**Practical use: extracting values after labels**

```python
import re
text = "Name: Alice | Age: 30 | City: NYC"
# Extract values without including the labels
names = re.findall(r"(?<=Name: )\w+", text)
ages = re.findall(r"(?<=Age: )\d+", text)
cities = re.findall(r"(?<=City: )\w+", text)
print(names, ages, cities)   # ['Alice'] ['30'] ['NYC']
```

### Negative Lookbehind

Negative lookbehind `(?<!...)` asserts that the text immediately preceding the current position does *not* match the sub-pattern.

```python
import re
# Match numbers not preceded by "$"
pattern = r"(?<!\$)\d+"
print(re.search(pattern, "price 12"))   # matches '12'
print(re.search(pattern, "price $12"))  # matches '2' — backtracking!
```

Again, backtracking can produce surprising results. `\d+` starts matching from `$12`:
- At position 7 (before `12`), `(?<!\$)` checks if preceding char is `$`. It is `$`, so assertion fails at position 7.
- Engine advances to position 8 (before `2`), `(?<!\$)` checks if preceding char is `$`. Preceding char is `1`, not `$`, assertion passes. `\d+` matches `2`. Match: `"2"`.

```python
import re
pattern = r"(?<!\$)\d+"
m = re.search(pattern, "price $12")
print(m.group())     # '2'
```

To avoid this, use a word boundary: `(?<!\$)\b\d+\b` or anchor the pattern appropriately.

**Practical use: exclude escaped characters**

```python
import re
# Match # that is NOT preceded by \ (i.e., not escaped)
text = "Use \#hash or #tag"
pattern = r"(?<!\\)#\w+"
print(re.findall(pattern, text))   # ['#tag']
# '#hash' is preceded by \ so it's excluded
```

### Fixed-Width vs Variable-Width Lookbehind

Different regex engines impose different restrictions on lookbehind patterns:

| Engine | Lookbehind restriction |
|--------|----------------------|
| Python `re` | Fixed-width only: literal chars, `\d`, `.` (one char), alternation of fixed-width branches |
| Python `regex` | Variable-width allowed |
| JavaScript (ES2018+) | Variable-width allowed (as of V8 6.2) |
| PCRE2 | Variable-width allowed |
| .NET | Variable-width allowed (goes backwards through the pattern) |
| Java | Finite-width (no `*` or `+` without `?`, but bounded quantifiers like `{1,5}` work) |
| Go `regexp` | No lookbehind at all |
| POSIX / GNU sed | No lookbehind |

**Fixed-width lookbehind means every branch must have the same, known length.**

```python
import re
# Python re: fixed-width only
re.search(r"(?<=\d)ab", "1ab")        # OK — \d is exactly 1 char
re.search(r"(?<=ab)cd", "abcd")       # OK — literals are fixed
re.search(r"(?<=\d{3})ab", "123ab")   # OK in Python 3.x? Actually no
```

In Python's `re` module:

```python
import re
# Works: literal of fixed length
print(re.search(r"(?<=abc)def", "abcdef"))      # match

# Works: character class of fixed length
print(re.search(r"(?<=\d)ab", "1ab"))            # match

# Works: alternation where each branch is same length
print(re.search(r"(?<=ab|cd)ef", "abef"))        # match

# Fails: variable length
# re.search(r"(?<=\d+)ab", "12ab")               # error: lookbehind requires fixed-width pattern

# Fails: alternation with different branch lengths
# re.search(r"(?<=ab|cd)ef", "abef")  # Actually this works in Python if lengths same
# re.search(r"(?<=ab|cde)ef", "abef")            # error: length differs
```

JavaScript (ES2018+) allows variable-width:

```javascript
// JavaScript: variable-width lookbehind allowed
const re = /(?<=\d+)\w+/;
console.log("12abc".match(re));    // ['abc']
```

**Workaround for fixed-width engines:** If you need variable-width lookbehind in Python `re`, restructure the pattern:

```python
import re
# Instead of: (?<=\d+)\w+
# Capture the digits and use a non-capturing group
m = re.search(r"(\d+)(\w+)", "12abc")
if m:
    print(m.group(2))    # 'abc'
```

Or use a capture group and ignore the first part:

```python
import re
# Instead of (?<=prefix_)word, just capture prefix_ and discard
pattern = r"prefix_(\w+)"
m = re.search(pattern, "prefix_word")
if m:
    print(m.group(1))    # 'word'
```

### Chaining Assertions

Multiple assertions can be chained at the same position. Each assertion independently inspects the text, and all must pass. This is the foundation of password validation patterns.

```python
import re
# Password must contain:
#   (?=.*[A-Z])  at least one uppercase
#   (?=.*[a-z])  at least one lowercase
#   (?=.*\d)     at least one digit
#   (?=.*[!@#$%]) at least one special char
#   .{8,}        at least 8 characters total
pattern = r"^(?=.*[A-Z])(?=.*[a-z])(?=.*\d)(?=.*[!@#$%]).{8,}$"

tests = ["Weak", "Strong1!", "NoSpecial1", "short1A!"]
for t in tests:
    m = re.search(pattern, t)
    print(f"{t:15s} {'✓' if m else '✗'}")
```

Each lookahead starts from the same position (after `^`). They scan independently. This is much cleaner than trying to write a single pattern that covers all conditions.

**Chaining lookahead and lookbehind:**

```python
import re
# Match a word that is both preceded by "start:" and followed by ";"
text = "start:hello; middle:world;"
pattern = r"(?<=start:)(\w+)(?=;)"
print(re.search(pattern, text).group())   # 'hello'
```

**Assertions at word boundaries:**

```python
import re
# Match "cat" only as a whole word and only if preceded by "black "
text = "The black cat sat"
pattern = r"(?<=\bblack\s)\bcat\b"
print(re.search(pattern, text).group())   # 'cat'
```

**Multiple lookaheads for multi-condition matching:**

```python
import re
# Match lines that contain both "ERROR" and "timeout"
log = """
INFO: request started
ERROR: connection timeout
WARN: retrying request
"""
pattern = r"^(?=.*ERROR)(?=.*timeout).*$"
for m in re.finditer(pattern, log, re.MULTILINE):
    print(m.group())   # 'ERROR: connection timeout'
```

### Lookaround Inside Groups

Lookaround assertions can be placed inside capturing and non-capturing groups. The assertions themselves never capture — they are zero-width — but the group can still capture the surrounding context.

**Capturing with lookaround boundaries:**

```python
import re
# Capture quoted text without including the quotes
text = 'She said "hello world" and left'
pattern = r'"([^"]*)"'
m = re.search(pattern, text)
print(m.group(1))    # 'hello world'

# Same effect with lookbehind and lookahead
pattern2 = r'(?<=")[^"]+(?=")'
m2 = re.search(pattern2, text)
print(m2.group())    # 'hello world'
```

**Lookahead inside a group to restrict what the group can match:**

```python
import re
# Match a word that is at least 5 chars AND contains a digit
pattern = r"\b(?=\w{5,})(?=\w*\d)\w+\b"
text = "cat abcd5 short ok12345"
print(re.findall(pattern, text))   # ['abcd5', 'ok12345']
```

Breakdown:
- `\b` — word boundary
- `(?=\w{5,})` — lookahead: word must be at least 5 chars
- `(?=\w*\d)` — lookahead: word must contain at least one digit
- `\w+` — consume the word
- `\b` — word boundary

**Lookbehind inside a group to conditionally match:**

```python
import re
# Match decimal numbers that are NOT preceded by a currency symbol
text = "price $12.50, total 34.99, cost €9.99"
pattern = r"(?<![$€])\b\d+\.\d{2}\b"
print(re.findall(pattern, text))   # ['34.99']
```

### Performance Impact of Assertions

Lookaround assertions add overhead because the engine must evaluate the assertion at every position before the main pattern can continue.

**Lookahead performance considerations:**

- Positive lookahead is generally fast — the engine tries the sub-pattern and if it matches, continues
- Negative lookahead can trigger more backtracking, especially with greedy quantifiers
- Chained lookaheads each scan from the current position independently

```python
import re, time

text = "A" * 10000 + "B"

# Pattern with 5 chained lookaheads — scans text 5 times
multi_lookahead = r"^(?=.*A)(?=.*B)(?=.*C)(?=.*D)(?=.*E).*$"
# Simple pattern — scans text once
simple = r"^.*ABCDE.*$"

start = time.perf_counter()
for _ in range(1000):
    re.search(multi_lookahead, text)
print(f"Multi lookahead: {time.perf_counter() - start:.3f}s")

start = time.perf_counter()
for _ in range(1000):
    re.search(simple, text)
print(f"Simple: {time.perf_counter() - start:.3f}s")
```

**Lookbehind performance considerations:**

- Lookbehind requires the engine to go backward, which can be less efficient
- Fixed-width lookbehind is optimized in most engines
- Variable-width lookbehind in engines that support it may need to try multiple starting positions

**Guidelines:**

- Avoid putting complex sub-patterns inside lookaround when a capture-group approach works
- For single-pass scanning, chained lookaheads do multiple linear scans over the same region
- Negative lookahead with `.*` can cause the same catastrophic backtracking risks as normal patterns
- If performance matters, benchmark with realistic data

```python
import re
# This pattern has catastrophic backtracking risk inside lookahead
# (?=.*[A-Z]) — the .* will backtrack extensively on long strings
# Better: (?=[^A-Z]*[A-Z])  — more constrained
```

**Optimized password validation:**

```python
import re
# Naive — .* causes extensive backtracking on non-match
naive = r"^(?=.*[A-Z])(?=.*[a-z])(?=.*\d).{8,}$"

# Optimized — specific negated classes limit backtracking
optimized = r"^(?=[^A-Z]*[A-Z])(?=[^a-z]*[a-z])(?=[^\d]*\d).{8,}$"

# The [^A-Z]* can only match non-uppercase — no backtracking ambiguity
```

### Simulating Lookbehind with \K

The `\K` escape (PCRE, Perl) resets the beginning of the match to the current position. Everything matched before `\K` is discarded from the final match, effectively simulating lookbehind.

```python
# PCRE / Perl
# Pattern: \$\K\d+
# Input:  "price $12"
# Match:  "12"  — the $ is matched but \K discards it
```

`\K` is not available in Python's `re` module, JavaScript, or Java. In Python, use capturing groups instead.

```python
# Instead of \K in Python:
import re
# PCRE: \$\K\d+
# Python: use capture
m = re.search(r"\$(\d+)", "price $12")
if m:
    print(m.group(1))   # '12'
```

**Advantages of `\K` over lookbehind:**

- Works in engines that don't support lookbehind (older PCRE, Perl)
- No fixed-width restriction
- Can match text before the reset point (useful for complex prefix matching)

**Disadvantages:**

- Not widely supported across regex flavors
- Can be confusing to readers who expect lookbehind
- The prefix is still consumed by the engine — it just isn't included in the match result

### Common Pitfalls

**1. Variable-width lookbehind not supported**

In Python's `re`, this fails:

```python
import re
# re.search(r"(?<=\d{2,4})\w+", text)   # error
```

**2. Lookahead with greedy quantifier inside causing unexpected results**

```python
import re
# Wants: match numbers not followed by "px"
pattern = r"\d+(?!px)"
print(re.search(pattern, "123px").group())   # '12' — backtracking gave unexpected match
```

Fix: add a word boundary or use an atomic group:

```python
import re
pattern = r"\d+\b(?!px)"
print(re.search(pattern, "123px"))    # no match — correct
```

**3. Assertion ordering matters**

```python
import re
# Password: at least 8 chars, at least one digit
# Wrong order — the .{8,} consumes everything, leaving nothing for (?=.*\d)
pattern = r"^.{8,}(?=.*\d)"
# Correct — assertions before consumption
pattern = r"^(?=.*\d).{8,}"
```

**4. Lookbehind at start of string**

```python
import re
# (?<=\s) at start of string — there is no preceding character
pattern = r"(?<=\s)\w+"
print(re.search(pattern, " hello"))    # match
print(re.search(pattern, "hello"))     # no match — no preceding whitespace
```

This is expected: lookbehind fails when the required preceding context does not exist.

**5. Forgetting assertions are zero-width**

```python
import re
# Trying to consume with lookahead
pattern = r"(?=\d{3})"   # only checks — doesn't consume
text = "123"
m = re.search(pattern, text)
print(m.group())     # '' — empty string, because lookahead doesn't consume!
```

Always add consuming tokens after the lookahead to capture the desired text.

**6. Lookbehind with alternation of different lengths**

```python
import re
# Python re: all branches must be same length
# re.search(r"(?<=cat|elephant)\s", text)  # error — lengths differ
```

If you need alternation with different lengths, use the `regex` module or restructure.

**7. Nested lookaround confusion**

Nested lookaround is syntactically valid but hard to read:

```python
import re
# Nested: match "a" only if followed by "b" that is not followed by "c"
pattern = r"a(?=b(?!c))"
print(re.search(pattern, "abd"))    # match
print(re.search(pattern, "abc"))    # no match
```

## Study Cases

### Case 1: Password Strength Validator

Build a password validator with clear error messages.

```python
import re

def validate_password(password):
    checks = {
        "min_length": len(password) >= 8,
        "uppercase": bool(re.search(r"[A-Z]", password)),
        "lowercase": bool(re.search(r"[a-z]", password)),
        "digit": bool(re.search(r"\d", password)),
        "special": bool(re.search(r"[!@#$%^&*(),.?\":{}|<>]", password)),
        "no_space": not re.search(r"\s", password),
    }
    failed = [k for k, v in checks.items() if not v]
    if not failed:
        return True, "Password is strong"
    messages = {
        "min_length": "At least 8 characters",
        "uppercase": "At least one uppercase letter",
        "lowercase": "At least one lowercase letter",
        "digit": "At least one digit",
        "special": "At least one special character",
        "no_space": "No spaces allowed",
    }
    return False, [messages[f] for f in failed]

# Pattern-based single-regex approach for quick check:
strong = r"^(?=[^A-Z]*[A-Z])(?=[^a-z]*[a-z])(?=[^\d]*\d)(?=[^!@#$%^&*]*[!@#$%^&*]).{8,}$"
tests = ["weak", "Strong1!", "NoSpecial1", "short1A!", "Valid1@Pass"]
for t in tests:
    ok, msg = validate_password(t)
    status = "✓" if ok else f"✗ ({', '.join(msg)})"
    print(f"{t:15s} {status}")
    print(f"  Regex match: {bool(re.match(strong, t))}")
```

### Case 2: Extracting Key-Value Pairs from Logs

Parse structured log lines with position-dependent fields.

```python
import re

log_lines = [
    "ERROR 2025-12-01 12:34:56 request_id=abc123 user=alice timeout after 30s",
    "INFO 2025-12-01 12:35:00 request_id=def456 user=bob completed in 120ms",
    "WARN 2025-12-01 12:35:10 request_id=ghi789 disk usage at 85%",
]

# Extract request_id and message type for each line
for line in log_lines:
    severity = re.search(r"^(ERROR|INFO|WARN)", line).group(1)
    req_id = re.search(r"(?<=request_id=)\w+", line).group()
    # Extract message after the structured fields
    msg = re.search(r"(?<=\d{2}:\d{2}:\d{2}\s).*", line).group()
    # Or: extract only the message part after user/request_id
    if severity == "ERROR":
        err_msg = re.search(r"(?<=user=\w+\s).*", line)
        if err_msg:
            print(f"[{severity}] {req_id}: {err_msg.group()}")
```

### Case 3: URL Router Pattern Matching

Simulate a web URL router that matches paths with conditions.

```python
import re

routes = [
    (r"^/api/users/(?P<user_id>\d+)$", "get_user"),
    (r"^/api/users$", "list_users"),
    (r"^/api/items\?(?=.*page=\d+)(?=.*limit=\d+)", "list_items_paginated"),
    (r"^/(?!(?:api|static|admin)/)\w+$", "public_page"),
]

def route(path):
    for pattern, name in routes:
        m = re.search(pattern, path)
        if m:
            return name, m.groupdict()
    return None, {}

paths = ["/api/users/42", "/api/users", "/api/items?page=1&limit=10",
         "/about", "/api/admin", "/admin/users"]
for path in paths:
    name, params = route(path)
    print(f"{path:30s} → {name} {params}")
```

### Case 4: Sanitizing Markdown URLs

Extract URLs from markdown without including the Markdown syntax.

```python
import re

md = """
Here is a [link to example](https://example.com) and
[another](https://example.org/page) with more text.
Inline images: ![alt](https://example.com/img.png)
"""

# Extract markdown link URLs using lookbehind and lookahead
# Pattern: match URL between ]( and )
urls = re.findall(r"(?<=\]\()([^)]+)(?=\))", md)
print("Link URLs:", urls)
# ['https://example.com', 'https://example.org/page', 'https://example.com/img.png']

# Extract only the URL text (not image alt text)
link_pattern = r"(?<=\]\()(https?://[^)]+)(?=\))"
link_urls = re.findall(link_pattern, md)
print("Link URLs only:", link_urls)
```

## Examples

### Example 1: Number Format Validation

```python
import re
# Match numbers that have at least 3 digits and no leading zeros
pattern = r"(?<!\d)(?:[1-9]\d{2,}|0)(?!\d)"
tests = ["0", "1", "12", "123", "0123", "1234", "001"]
for t in tests:
    m = re.search(pattern, t)
    print(f"{t:8s} → {m.group() if m else 'no match'}")
```

### Example 2: Matching Quoted Strings Without Quotes

```python
import re
text = "'single' \"double\" 'mixed\"  \"nested\\\"quote\""
# Match single-quoted strings (content without quotes)
single = re.findall(r"(?<=')[^']*(?=')", text)
print("Single-quoted:", single)   # ['single']
# Match double-quoted strings
double = re.findall(r'(?<=")[^"]*(?=")', text)
print("Double-quoted:", double)   # ['double', 'nested\\"quote']
```

### Example 3: Negative Lookbehind for Escaped Characters

```python
import re
# Split on commas that are NOT escaped
text = r"a,b\,c,d"
pattern = r"(?<!\\)\,"
parts = re.split(pattern, text)
print(parts)   # ['a', 'b\\,c', 'd'] — the \, is preserved
```

### Example 4: Currency Extraction

```python
import re
text = "Prices: $12.50, €9.99, £24.95, JPY 3000, $5"
# Match amounts in dollars only
dollars = re.findall(r"(?<=\$)\d+(?:\.\d{2})?", text)
print("Dollars:", dollars)   # ['12.50', '5']
# Match any currency amount (capture the symbol too)
all_cur = re.findall(r"([$€£])(\d+(?:\.\d{2})?)", text)
print("All currencies:", all_cur)
# [('$', '12.50'), ('€', '9.99'), ('£', '24.95')]
```

### Example 5: Validation of Email-like Patterns

```python
import re
# Local part must not start/end with dot, no consecutive dots
email_pattern = r"""
    ^
    (?=[a-zA-Z0-9.!#$%&'*+/=?^_`{|}~-]{1,64}@)   # local part length
    [a-zA-Z0-9!#$%&'*+/=?^_`{|}~-]               # first char (no dot)
    (?:[a-zA-Z0-9.!#$%&'*+/=?^_`{|}~-]*          # rest of local
    (?<![.]))?                                     # no trailing dot
    @
    (?=[a-zA-Z0-9.-]{1,255})                      # domain constraints
    [a-zA-Z0-9]
    (?:[a-zA-Z0-9-]*[a-zA-Z0-9])?
    (?:\. [a-zA-Z0-9](?:[a-zA-Z0-9-]*[a-zA-Z0-9])?)*
    \.[a-zA-Z]{2,}
    $
"""
# Simplified for readability — real email validation is complex
```

## Glossary

| Term | Definition |
|------|------------|
| Assertion | A regex construct that tests a condition at a position without consuming characters |
| Chained assertion | Multiple lookaheads/lookbehinds applied at the same position, all must pass |
| Consume | To advance the regex engine's position in the string by matching characters |
| Fixed-width lookbehind | A lookbehind pattern whose length is known at compile time (e.g., `\d{3}`, `abc`) |
| Negative lookahead | `(?!...)` — assertion that the following text does NOT match the pattern |
| Negative lookbehind | `(?<!...)` — assertion that the preceding text does NOT match the pattern |
| Positive lookahead | `(?=...)` — assertion that the following text matches the pattern |
| Positive lookbehind | `(?<=...)` — assertion that the preceding text matches the pattern |
| Variable-width lookbehind | A lookbehind pattern with variable length (e.g., `\d+`, `.*?`), not supported by all engines |
| Zero-width assertion | An assertion that matches a position, not characters; the engine position does not advance |
| `\K` | A PCRE/Perl escape that resets the match start, simulating lookbehind |

## Quick References

- [Regular-Expressions.info: Lookahead and Lookbehind](https://www.regular-expressions.info/lookaround.html) — comprehensive lookaround guide
- [Python re documentation](https://docs.python.org/3/library/re.html) — Python's fixed-width lookbehind rules
- [MDN: Lookahead and Lookbehind](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_expressions/Assertions) — JavaScript assertions guide
- [Regex101](https://regex101.com/) — interactive tester with assertion visualization
- [PCRE2 Specification](https://www.pcre.org/current/doc/html/pcre2pattern.html) — variable-width lookbehind in PCRE2

## Next Steps

- [Regex Engine Internals](regex-engine-internals.md) — how NFA/DFA engines handle assertions internally
- [Real-World Regex Patterns](real-world-regex-patterns.md) — practical patterns combining groups, lookaround, and anchors
- [Regex Performance & Security](regex-performance-and-security.md) — catastrophic backtracking and ReDoS prevention
