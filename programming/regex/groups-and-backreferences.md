# Groups, Capture & Backreferences

## Description

Groups let you extract, reuse, and transform portions of a match. Capturing groups store sub-matches for backreferences and replacements; non-capturing and atomic groups control engine behavior. Mastering groups is essential for data extraction, search-and-replace refactoring, and text parsing.

## Prerequisites

- [Regex Syntax Fundamentals](regex-syntax-fundamentals.md) — quantifiers, character classes, anchors, alternation

## Table of Contents

- [What Is a Group?](#what-is-a-group)
- [Capturing Groups](#capturing-groups)
- [Numbered Backreferences](#numbered-backreferences)
- [Named Groups](#named-groups)
- [Named Backreferences](#named-backreferences)
- [Non-Capturing Groups](#non-capturing-groups)
- [Atomic Groups](#atomic-groups)
- [Branch Reset](#branch-reset)
- [Group Reuse in Substitution](#group-reuse-in-substitution)
- [Nested Groups and Capture Numbering](#nested-groups-and-capture-numbering)
- [Capturing vs Non-Capturing Performance](#capturing-vs-non-capturing-performance)
- [Common Mistakes](#common-mistakes)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### What Is a Group?

A group is a portion of a regex pattern enclosed in parentheses `(...)`. Parentheses serve two purposes:

1. **Grouping** — apply quantifiers or alternation to a sub-pattern
2. **Capturing** — store the matched text for later use

```python
import re
# Grouping: quantifier applies to whole group
pattern = r"(ha)+"
print(re.findall(pattern, "hahaha"))   # ['ha']
# Without grouping, quantifier applies only to previous char
pattern = r"ha+"
print(re.findall(pattern, "hahaha"))   # ['ha', 'ha', 'ha']
```

The default group type is a *capturing group*. Every `(` opens capture number `n+1`.

### Capturing Groups

A capturing group `(...)` stores whatever text its sub-pattern matches. The stored text is accessible:

- Inside the pattern via backreference (`\1`, `\2`, etc.)
- In the match result as a numbered submatch
- In replacement strings as `$1`, `\1`, etc.

```python
import re
match = re.search(r"(\w+)@(\w+)\.(\w+)", "user@example.com")
print(match.group(0))    # 'user@example.com'  (full match)
print(match.group(1))    # 'user'
print(match.group(2))    # 'example'
print(match.group(3))    # 'com'
```

JavaScript:

```javascript
const m = /(\w+)@(\w+)\.(\w+)/.exec("user@example.com");
console.log(m[0]);   // 'user@example.com'
console.log(m[1]);   // 'user'
console.log(m[2]);   // 'example'
console.log(m[3]);   // 'com'
```

**Group numbering starts at 1.** Group 0 is always the entire match.

### Numbered Backreferences

A backreference matches the *same text* that a capturing group matched earlier in the pattern — not the same pattern, but the actual captured characters.

Syntax: `\N` where `N` is the group number.

```python
import re
# Match repeated word: \1 repeats whatever (\w+) captured
pattern = r"\b(\w+)\s+\1\b"
print(re.search(pattern, "hello hello"))     # match
print(re.search(pattern, "hello world"))     # no match
```

JavaScript:

```javascript
const re = /\b(\w+)\s+\1\b/;
console.log(re.exec("hello hello"));     // match
console.log(re.exec("hello world"));     // null
```

**HTML tag matching:**

```python
import re
# \1 ensures opening and closing tags match
pattern = r"<(div|p|h1)>(.*?)</\1>"
text = "<div>content</div>"
print(re.search(pattern, text))    # match
text = "<div>content</p>"
print(re.search(pattern, text))    # no match — closing tag differs
```

**Backreference with quantifiers** — the backreference matches the captured text, then the quantifier repeats the backreference:

```python
# Match two or more consecutive same words
pattern = r"\b(\w+)(\s+\1)+\b"
print(re.search(pattern, "yes yes yes"))     # match
```

**Important:** A backreference inside a group refers to the group's most recent capture from a *previous* repetition, not the current one. In repeated groups, only the last capture is stored.

```python
# Group captures last iteration only
match = re.search(r"(\d)+", "123")
print(match.group(1))    # '3', not '1'
```

### Named Groups

Named groups give a human-readable name to a capturing group instead of a number. Syntax varies by flavor:

| Engine | Syntax | Backreference |
|--------|--------|---------------|
| Python | `(?P<name>...)` | `(?P=name)` |
| PCRE2, PHP, .NET, Java | `(?<name>...)` | `\k<name>` |
| .NET alternative | `(?'name'...)` | `\k'name'` |
| JavaScript | `(?<name>...)` | `\k<name>` |

Python:

```python
import re
pattern = r"(?P<year>\d{4})-(?P<month>\d{2})-(?P<day>\d{2})"
match = re.search(pattern, "2025-12-01")
print(match.group("year"))    # '2025'
print(match.group("month"))   # '12'
print(match.group("day"))     # '01'
# Also accessible by number
print(match.group(1))         # '2025'
```

JavaScript (ES2018+):

```javascript
const re = /(?<year>\d{4})-(?<month>\d{2})-(?<day>\d{2})/;
const m = re.exec("2025-12-01");
console.log(m.groups.year);   // '2025'
console.log(m.groups.month);  // '12'
```

**When to use named groups:**

- Complex patterns with many groups — names are self-documenting
- Patterns where groups may be added or reordered — names insulate against renumbering
- Code that needs to be readable by others

### Named Backreferences

Inside the pattern, reference a named group's captured text:

Python:

```python
import re
pattern = r"\b(?P<word>\w+)\s+(?P=word)\b"
print(re.search(pattern, "hello hello"))     # match
# Named groups can also be referenced by number
pattern = r"\b(?P<word>\w+)\s+\1\b"         # also works
```

PCRE2 / JavaScript:

```javascript
const re = /\b(?<word>\w+)\s+\k<word>\b/;
console.log(re.exec("hello hello"));
```

.NET:

```csharp
var re = new Regex(@"\b(?<word>\w+)\s+\k<word>\b");
```

**Mixed references:** Most engines let you reference a named group by both name and number. Avoid mixing styles in the same pattern for clarity.

### Non-Capturing Groups

Non-capturing groups `(?:...)` group a sub-pattern without storing the matched text. They use the same parentheses syntax but with `?:` immediately after the opening `(`.

```python
import re
# Capturing — both groups stored
m = re.search(r"(\d+)(px|em|rem)", "12px")
print(m.groups())          # ('12', 'px')
# Non-capturing — only first group stored
m = re.search(r"(\d+)(?:px|em|rem)", "12px")
print(m.groups())          # ('12',)
```

**Use non-capturing groups when:**

- You need grouping for alternation: `(?:cat|dog)` instead of `(cat|dog)` when capture is unneeded
- Applying quantifiers to a sub-pattern: `(?:https?:\/\/)?`
- Wrapping a sub-pattern without polluting capture numbers

```python
import re
# Alternation without capture
pattern = r"File (?:saved|deleted|updated) successfully"
print(re.search(pattern, "File saved successfully"))     # match
```

**Effect on numbering:** Non-capturing groups are invisible to the numbering system. They do not increment the capture counter.

```python
import re
# Group 1 is (\d+), group 2 is (.+)
pattern = r"(?:[A-Z]+)(\d+)(?:[-_ ])(.+)"
m = re.search(pattern, "AB12_test")
print(m.group(1))    # '12'
print(m.group(2))    # 'test'
```

### Atomic Groups

Atomic groups `(?>...)` disable backtracking into the group once the group has matched. If the overall pattern fails later, the engine will not try different permutations inside an atomic group — it discards the group's internal backtracking states.

```python
import re
# Without atomic group — backtracking succeeds
pattern = r"\d+(?:foo)"
text = "123foo"
print(re.search(pattern, text))    # match
```

The real power of atomic groups comes from catastrophic backtracking prevention:

```python
import re
# Catastrophic backtracking risk
bad_pattern = r"(\d+)+$"
# Atomic group prevents backtracking into the group
good_pattern = r"(?>\d+)+$"
text = "1234567890X"
# bad_pattern would backtrack exponentially on this non-match
# good_pattern fails fast
```

**How atomic groups work:**

1. The engine enters the atomic group and tries the sub-pattern
2. If the sub-pattern matches, the engine commits to that match and forgets all alternative positions inside the group
3. If the rest of the pattern fails, the engine cannot backtrack into the atomic group to try alternatives
4. The only option is to backtrack to before the atomic group itself

```python
import re
# Without atomic
pattern1 = r"(a|ab)(c)"
text = "abc"
print(re.search(pattern1, text))    # match: group1='a', group2='bc'
# With atomic — group2 fails, no backtracking into group1
pattern2 = r"(?>a|ab)(c)"
text = "abc"
print(re.search(pattern2, text))    # no match
```

Explanation: In `pattern2`, the atomic group matches `a` (first alternative). It forgets that `ab` was also a possibility. Then `(c)` fails because after `a` comes `b`. The engine cannot backtrack into the atomic group to try `ab`, so the overall match fails.

**Use cases for atomic groups:**

- Performance optimization in patterns with nested quantifiers
- Preventing unwanted backtracking that changes match semantics
- Implementing possessive-like behavior (atomic group + quantifier is equivalent to possessive quantifier: `(?>x+)` = `x++`)

### Branch Reset

Branch reset `(?|...|...|...)` makes different alternatives share the same capture group numbers. Without it, each alternative in alternation creates separate capture groups.

```python
import re
# Without branch reset — each alternative has its own numbering
pattern = r"(cat)|(dog)"
m = re.search(pattern, "dog")
print(m.lastindex)               # 2
print(m.group(1))                # None
print(m.group(2))                # 'dog'
```

With branch reset:

```python
import re
# With branch reset — group 1 is always the animal
pattern = r"(?|(cat)|(dog))"
m = re.search(pattern, "dog")
print(m.lastindex)               # 1
print(m.group(1))                # 'dog'
m = re.search(pattern, "cat")
print(m.group(1))                # 'cat'
```

Branch reset works when alternatives have the *same number* of capturing groups. Each alternative's groups are renumbered starting from the same base.

```python
import re
# Each alternative has 2 groups
pattern = r"(?|(cat)_(says)|(dog)_(barks))"
m = re.search(pattern, "dog_barks")
print(m.group(1))    # 'dog'
print(m.group(2))    # 'barks'
```

**Flavors supporting branch reset:** PCRE (PHP), Perl. Not supported in Python's `re` module (available in `regex` third-party module), JavaScript, or Java.

### Group Reuse in Substitution

Captured groups are commonly reused in replacement strings. Syntax varies by context:

| Context | Syntax | Example |
|---------|--------|---------|
| Python `re.sub` | `\1`, `\g<1>`, `\g<name>` | `re.sub(r"(\w+) (\w+)", r"\2, \1", text)` |
| JavaScript `replace` | `$1`, `$&`, `$``, `$'` | `text.replace(/(\w+) (\w+)/, '$2, $1')` |
| VSCode / Editors | `$1`, `$2` | Find: `(\w+) (\w+)` Replace: `$2, $1` |
| sed | `\1` | `sed 's/\(foo\) \(bar\)/\2 \1/'` |
| PHP `preg_replace` | `$1`, `$2` or `\1`, `\2` | `preg_replace('/(\w+) (\w+)/', '$2, $1', $text)` |

**Swapping names:**

```python
import re
text = "Doe, John"
result = re.sub(r"(\w+),\s*(\w+)", r"\2 \1", text)
print(result)    # 'John Doe'
```

JavaScript:

```javascript
const text = "Doe, John";
const result = text.replace(/(\w+),\s*(\w+)/, '$2 $1');
console.log(result);   // 'John Doe'
```

**Named groups in substitution:**

```python
import re
pattern = r"(?P<last>\w+),\s*(?P<first>\w+)"
result = re.sub(pattern, r"\g<first> \g<last>", "Doe, John")
print(result)    # 'John Doe'
```

**Reformatting dates:**

```python
import re
text = "2025-12-01"
result = re.sub(r"(\d{4})-(\d{2})-(\d{2})", r"\3/\2/\1", text)
print(result)    # '01/12/2025'
```

**Referring to the full match:**

JavaScript:

```javascript
// $& = full match, $` = before match, $' = after match
console.log("hello world".replace(/world/, "($&)"));   // 'hello (world)'
console.log("hello world".replace(/world/, "($`)")   ); // 'hello (hello )'
console.log("hello world".replace(/world/, "($')")   ); // 'hello ()'
```

In Python, use `re.sub` with a function for full control:

```python
import re
def replacer(m):
    return f"({m.group(0)})"
result = re.sub(r"world", replacer, "hello world")
print(result)    # 'hello (world)'
```

### Nested Groups and Capture Numbering

Capturing groups are numbered by the position of their opening `(` in the pattern, counted from left to right.

```python
import re
# Outer group 1, inner group 2, trailing group 3
pattern = r"((\w+)@(\w+))\.com"
m = re.search(pattern, "user@example.com")
print(m.group(0))    # 'user@example.com'
print(m.group(1))    # 'user@example'
print(m.group(2))    # 'user'
print(m.group(3))    # 'example'
```

**Numbering rule in detail:**

```
Pattern:   (  (  \w+  )  @  (  \w+  )  )  \.  com
Group #:   1  2              3
```

The opening `(` at position 0 is group 1. The next `(` at position 1 inside group 1 is group 2. The `(` after `@` is group 3.

**Nested backreferences** — a backreference can refer to an outer group from inside a nested group:

```python
import re
# Match XML-like tags with same attribute values
pattern = r"(<(\w+)>)(.*?)</\2>"
text = "<div>content</div>"
print(re.search(pattern, text))    # match, \2 refers to group 2
```

**Forward references** — some engines let you reference a group that appears later in the pattern. The backreference matches whatever that group captures (which is empty at the point of forward reference). Forward references are rarely needed — prefer restructuring the pattern.

### Capturing vs Non-Capturing Performance

Non-capturing groups affect performance, not just readability.

**Memory overhead:** The regex engine must store every captured group's text and position. For simple patterns run once on one string, this is negligible. For patterns applied to thousands of strings or inside tight loops, the overhead adds up.

```python
import re, time
text = "A" * 10000 + "B"
cap_pattern = r"(?: (A+) )+ B"
ncap_pattern = r"(?: (?:A+) )+ B"
start = time.perf_counter()
for _ in range(1000):
    re.search(cap_pattern, text)
cap_time = time.perf_counter() - start
start = time.perf_counter()
for _ in range(1000):
    re.search(ncap_pattern, text)
ncap_time = time.perf_counter() - start
print(f"Capturing: {cap_time:.3f}s  Non-capturing: {ncap_time:.3f}s")
```

**Guidelines:**

- Use `(?:...)` whenever you group for alternation or quantifiers but don't need the captured text
- Capture only the groups you actually reference
- For `re.findall`, capturing groups change the return type — if you want the full match, avoid capturing or use `re.finditer`
- In large-scale text processing (log parsing, data pipelines), non-capturing groups reduce memory pressure

**`re.findall` quirk:**

```python
import re
# No groups → returns full matches
print(re.findall(r"\w+@\w+\.\w+", "a@b.com c@d.com"))
# ['a@b.com', 'c@d.com']
# With groups → returns tuples of groups only
print(re.findall(r"(\w+)@(\w+\.\w+)", "a@b.com c@d.com"))
# [('a', 'b.com'), ('c', 'd.com')]
# Mixed → non-capturing doesn't affect the result
print(re.findall(r"(\w+)@(?:\w+\.\w+)", "a@b.com c@d.com"))
# ['a', 'c']
```

### Common Mistakes

**1. Excess capturing groups**

Developers often use `(...)` when `(?:...)` would suffice. This pollutes capture numbering and slows the engine.

```python
# Bad: captures every alternation branch
re.search(r"(cat|dog|bird)", text)
# Good: no capture needed
re.search(r"(?:cat|dog|bird)", text)
```

**2. Assuming backreference matches pattern, not text**

A backreference matches the *exact characters* captured, not the pattern that produced them.

```python
import re
pattern = r"(\d{2})-\1"
print(re.search(pattern, "12-12"))    # match
print(re.search(pattern, "12-99"))    # no match — \1 is "12", not \d{2}
```

**3. Backreference to non-existent group**

Referencing `\3` when only two groups exist causes an error or matches literal "3" depending on the flavor.

```python
import re
# In Python, invalid backreference raises an error
# re.search(r"(\w+) \3", "abc 3")      # error
```

**4. Capture group in `re.split`**

`re.split` with a capturing group includes the captured text in the result:

```python
import re
# Without capture: split only
print(re.split(r"\s+", "a b c"))           # ['a', 'b', 'c']
# With capture: separators included
print(re.split(r"(\s+)", "a b c"))         # ['a', ' ', 'b', ' ', 'c']
```

This is useful for preserving separators, but surprising if unintended.

**5. Repeated group only stores last capture**

```python
import re
m = re.search(r"(\d)+", "123")
print(m.group(1))    # '3', not '1'
```

If you need all digits individually, use `re.findall` or `re.finditer`.

**6. Named group syntax confusion**

Using `(?<name>...)` in Python's `re` module fails — it requires `(?P<name>...)`.

```python
import re
# re.search(r"(?<year>\d{4})", text)   # error
re.search(r"(?P<year>\d{4})", text)
```

**7. Greedy backreference leading to no match**

A greedy group before a backreference can consume text that the backreference needs:

```python
import re
pattern = r"(.+)\s+\1"
print(re.search(pattern, "abc abc abc"))   # matches 'abc abc' (backtracking)
```

**8. Branch reset unsupported**

Using `(?|...)` in Python's `re` module raises an error. Always check flavor support.

## Study Cases

### Case 1: Log Parser Extracting Components

Parse Apache combined log lines to extract IP, timestamp, method, path, status, and user agent.

```python
import re
log_line = r'192.168.1.1 - - [10/Dec/2025:12:34:56 +0000] "GET /api/users HTTP/1.1" 200 1234 "https://example.com" "Mozilla/5.0"'
pattern = r'''
    (?P<ip>\S+)                     # IP address
    \s+-\s+                         # dash
    (?P<user>\S+)                   # remote user
    \s+\[(?P<timestamp>[^\]]+)\]    # timestamp
    \s+"(?:GET|POST|PUT|DELETE)     # HTTP method (non-capturing)
    \s+(?P<path>\S+)               # request path
    \s+\S+"\s+                      # HTTP version
    (?P<status>\d{3})              # status code
    \s+(?P<bytes>\d+|-)            # bytes sent
    \s+"(?P<referer>[^"]*)"        # referer
    \s+"(?P<agent>[^"]*)"          # user agent
'''
m = re.search(pattern, log_line, re.VERBOSE)
if m:
    print(f"IP: {m.group('ip')}, Path: {m.group('path')}, Status: {m.group('status')}")
```

### Case 2: Parsing CSV with Quoted Fields

Parse a CSV line where fields may be quoted and contain commas.

```python
import re
csv_line = 'John,Doe,"123 Main St, Apt 4",NYC,"jdoe@email.com"'
# Non-quoted field or quoted field
parts = re.split(r'("(?:[^"]|"")*")', csv_line)
fields = []
for part in parts:
    if part.startswith('"') and part.endswith('"'):
        fields.append(part[1:-1])
    else:
        fields.extend(part.split(','))
print([f.strip() for f in fields if f.strip()])
```

### Case 3: Finding Repeated Words

Detect consecutive duplicate words (common typo in prose).

```python
import re
text = "This is is a test test of the the duplicated word system."
pattern = r"\b(\w+)\s+\1\b"
for m in re.finditer(pattern, text, re.IGNORECASE):
    print(f"Duplicate '{m.group(1)}' at position {m.start()}-{m.end()}")
# Remove duplicates
result = re.sub(pattern, r"\1", text, flags=re.IGNORECASE)
print(result)   # 'This is a test of the duplicated word system.'
```

### Case 4: Template Variable Substitution

Replace `{{ variable }}` placeholders with values from a dictionary.

```python
import re
template = "Hello {{ name }}, your order #{{ order_id }} is {{ status }}."
data = {"name": "Alice", "order_id": "12345", "status": "shipped"}
def replace_var(m):
    var_name = m.group(1).strip()
    return data.get(var_name, m.group(0))
pattern = r"\{{(.+?)\}}"
result = re.sub(pattern, replace_var, template)
print(result)   # 'Hello Alice, your order #12345 is shipped.'
```

## Examples

### Example 1: Swapping First and Last Name

```python
import re
text = "Doe, John"
result = re.sub(r"(\w+),\s*(\w+)", r"\2 \1", text)
print(result)   # 'John Doe'
```

### Example 2: Normalizing Phone Numbers

```python
import re
phones = ["(123) 456-7890", "123-456-7890", "123.456.7890"]
for phone in phones:
    m = re.search(r"\(?(\d{3})\)?[-. ]?(\d{3})[-. ]?(\d{4})", phone)
    if m:
        normalized = f"({m.group(1)}) {m.group(2)}-{m.group(3)}"
        print(f"{phone:25s} → {normalized}")
```

### Example 3: Matching Nested Parentheses (Limitation)

Standard regex cannot handle arbitrary nesting depth. Groups only help to a limited level:

```python
import re
# Match one level of nesting
pattern = r"\(([^()]*)\)"
text = "function(a, b(c, d), e)"
print(re.findall(pattern, text))   # ['c, d'] — innermost only
# Match up to two levels
pattern = r"\(((?:[^()]*\([^()]*\))*[^()]*)\)"
print(re.findall(pattern, text))   # ['b(c, d)', 'a, b(c, d), e']
```

For true nesting, use a parser — not regex.

### Example 4: Extracting URLs from HTML

```python
import re
html = '<a href="https://example.com">Ex</a> <a href="/relative">Rel</a>'
pattern = r'''href=["'](?P<url>[^"']+)["']'''
for m in re.finditer(pattern, html):
    print(m.group("url"))
```

## Glossary

| Term | Definition |
|------|------------|
| Alternation | The `|` operator that matches one of several alternatives |
| Atomic group | A group `(?>...)` that disables backtracking into its contents once matched |
| Backreference | A reference to a previously captured group that matches the same text, not the same pattern |
| Backtracking | The engine's process of trying alternative paths when a match fails |
| Branch reset | A construct `(?|...|...|...)` that resets capture group numbering across alternatives |
| Capture number | The sequential number assigned to each capturing group based on its opening `(` position |
| Capturing group | Parentheses `(...)` that store the matched subtext for later use |
| Forward reference | A backreference to a group that appears later in the pattern |
| Group | A sub-pattern enclosed in parentheses, used for grouping or capturing |
| Named backreference | A reference to a named group using syntax like `\k<name>` or `(?P=name)` |
| Named group | A capturing group with a human-readable name instead of a number |
| Non-capturing group | A group `(?:...)` that groups without storing the matched text |
| Possessive quantifier | A quantifier like `++` that does not backtrack (equivalent to atomic group) |
| Substitution | The replacement string in a search-and-replace operation, which can reference captures |
| Zero-width assertion | A pattern that matches a position, not characters (lookahead, lookbehind, anchors) |

## Quick References

- [Regular-Expressions.info: Grouping and Backreferences](https://www.regular-expressions.info/brackets.html) — comprehensive reference with flavor comparisons
- [Python re documentation](https://docs.python.org/3/library/re.html) — official Python regex docs
- [MDN: Groups and Backreferences](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_expressions/Groups_and_backreferences) — JavaScript guide
- [Regex101](https://regex101.com/) — interactive tester with group match visualization

## Next Steps

- [Lookahead & Lookbehind Assertions](lookahead-and-lookbehind.md) — zero-width assertions for context matching without consuming characters
- [Regex Engine Internals](regex-engine-internals.md) — how NFA/DFA engines process groups and backtrack
- [Real-World Regex Patterns](real-world-regex-patterns.md) — practical patterns using groups for data extraction
