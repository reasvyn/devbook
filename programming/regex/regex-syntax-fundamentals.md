# Regex Syntax Fundamentals

## Description

The core building blocks of regex: literal characters, metacharacters, character classes, anchors, quantifiers, alternation, grouping, and escape sequences. Every pattern you will ever write is composed from these elements.

## Prerequisites

- [What Are Regular Expressions?](intro/what-is-regex.md) — basic understanding of what regex is and why it matters

## Table of Contents

- [Literal Characters](#literal-characters)
- [Metacharacters and Escaping](#metacharacters-and-escaping)
- [The Dot Metacharacter](#the-dot-metacharacter)
- [Character Classes](#character-classes)
- [Shorthand Character Classes](#shorthand-character-classes)
- [Anchors](#anchors)
- [Quantifiers](#quantifiers)
- [Greedy vs Lazy Quantifiers](#greedy-vs-lazy-quantifiers)
- [Alternation](#alternation)
- [Grouping with Parentheses](#grouping-with-parentheses)
- [Escape Sequences](#escape-sequences)
- [Building Patterns Step by Step](#building-patterns-step-by-step)
- [Common Beginner Mistakes](#common-beginner-mistakes)

## Content / Material

### Literal Characters

The simplest regex pattern is a literal character: it matches that exact character in the input. The pattern `a` matches "a" wherever it appears. The pattern `hello` matches the exact sequence "hello".

```python
import re

print(re.search(r'hello', 'say hello world'))   # match
print(re.search(r'hello', 'Hello world'))        # None (case-sensitive)
print(re.search(r'hello', 'heLLo'))              # None
print(re.search(r'hello', 'Hello', re.IGNORECASE))  # match

# Literal text matching
text = 'The price is $19.99 for item #42.'
print(re.findall(r'price', text))   # ['price']
print(re.findall(r'item', text))    # ['item']
```

Most characters in a regex pattern are literal. Letters, digits, and many punctuation characters match themselves.

### Metacharacters and Escaping

Twelve characters have special meaning in regex and must be escaped with a backslash to match them literally:

```text
. ^ $ * + ? { } [ ] \ | ( )
```

To match a literal dot, write `\.`. For a literal asterisk, write `\*`. For a literal backslash, write `\\`.

```python
import re

text = 'price: $19.99 (tax: $2.00) [receipt #42]'

print(re.findall(r'\$', text))     # ['$', '$']
print(re.findall(r'\.', text))     # ['.', '.']
print(re.findall(r'\(', text))     # ['(', '(']
print(re.findall(r'\\', 'path\\to\\file'))  # ['\\', '\\']

# Inside character classes, most metacharacters are literal
print(re.findall(r'[$()]', text))  # ['$', '(', ')']
```

### The Dot Metacharacter

The dot `.` matches any single character except a newline (`\n`). It is the most general-purpose wildcard in regex.

```python
import re

print(re.findall(r'c.t', 'cat cut cot cat scat'))   # ['cat', 'cut', 'cot', 'cat']
print(re.findall(r'a.b', 'a\nb'))                    # [] (dot ≠ newline)
print(re.findall(r'a.b', 'a\nb', re.DOTALL))         # ['a\nb'] (DOTALL flag)
```

The dot is often used with quantifiers to match variable-length content:

```python
import re

text = 'He said "hello" and she said "world"'

# Greedy: matches from first quote to last quote
print(re.findall(r'".*"', text))   # ['"hello" and she said "world"']

# Lazy: matches each quoted string individually
print(re.findall(r'".*?"', text))  # ['"hello"', '"world"']
```

Beginners reach for `.*` to match "anything", but this often matches too much (greedy) or fails on multi-line input. Better: use negated character classes.

```python
import re

# .*? works but is fragile
text = 'name: John; age: 30; city: NY'
print(re.findall(r'name: (.*?);', text))  # ['John']

# [^;]+ is explicit and avoids backtracking
print(re.findall(r'name: ([^;]+);', text))  # ['John']

# For multi-line, use [\s\S] to match any character including newlines
html = '<div>\n  <p>Hello</p>\n</div>'
print(re.findall(r'<div>([\s\S]*?)</div>', html))  # ['\n  <p>Hello</p>\n']
```

### Character Classes

A character class matches one character from a defined set. Enclose the set in square brackets `[...]`.

```regex
[abc]       # matches 'a', 'b', or 'c'
[0-9]       # matches any digit
[a-zA-Z]    # matches any letter
[^0-9]      # matches any character NOT a digit (negated class)
```

```python
import re

print(re.findall(r'[aeiou]', 'hello world'))           # ['e', 'o', 'o']
print(re.findall(r'[0-9]', 'order #42: $19.99'))       # ['4', '2', '1', '9', '9', '9']
print(re.findall(r'[^0-9]', 'order #42: $19.99'))      # ['o', 'r', 'd', 'e', 'r', ' ', '#', ':', ' ', '$', '.']
print(re.findall(r'[0-9a-fA-F]', 'Hex: A1B2C3'))       # ['A', '1', 'B', '2', 'C', '3']
print(re.sub(r'[aeiou]', '', 'hello world'))            # 'hll wrld'
print(re.findall(r'[^\s]', 'a b c'))                    # ['a', 'b', 'c']
```

Range expressions use `-` between two characters. [A-z] includes ASCII chars between Z (90) and a (97) — probably not what you want:

```python
import re

print(re.findall(r'[A-z]', 'Hello[World]'))
# ['H', 'e', 'l', 'l', 'o', '[', 'W', 'o', 'r', 'l', 'd', ']']

# Correct: explicit letter ranges
print(re.findall(r'[A-Za-z]', 'Hello[World]'))  # ['H', 'e', 'l', 'l', 'o', 'W', 'o', 'r', 'l', 'd']
```

Mixed ranges and specific characters:

```python
import re

print(re.findall(r"[a-zA-Z0-9_'-]+", "it's a well-known fact"))
# ["it's", 'a', 'well-known', 'fact']
```

### Shorthand Character Classes

Shorthand classes provide convenient abbreviations for common character sets:

| Shorthand | Equivalent | Matches |
|-----------|------------|---------|
| `\d` | `[0-9]` | Digit |
| `\w` | `[a-zA-Z0-9_]` | Word character (letter, digit, underscore) |
| `\s` | `[ \t\n\r\f\v]` | Whitespace |
| `\D` | `[^0-9]` | Non-digit |
| `\W` | `[^a-zA-Z0-9_]` | Non-word character |
| `\S` | `[^ \t\n\r\f\v]` | Non-whitespace |

```python
import re

print(re.findall(r'\d+', 'order #42: $19.99'))       # ['42', '19', '99']
print(re.findall(r'\w+', 'hello, world! #42'))        # ['hello', 'world', '42']
print(re.findall(r'\s', 'a b\tc\nd'))                 # [' ', '\t', '\n']
print(re.findall(r'\D+', 'order #42: $19.99'))        # ['order #', ': $', '.']
print(re.findall(r'\W+', 'hello, world!'))            # [', ', '!']
print(re.findall(r'\S+', 'a b  c'))                   # ['a', 'b', 'c']

# Combining shorthands with literals
idents = ['foo', '_private', '2fast', 'camelCase', 'snake_case']
pattern = r'\b[a-zA-Z_]\w*\b'
print([i for i in idents if re.match(pattern, i)])
# ['foo', '_private', 'camelCase', 'snake_case']

# Extract numbers including decimals
print(re.findall(r'-?\d+(?:\.\d+)?', 'values: 42, 3.14, -7, 100.5'))
# ['42', '3.14', '-7', '100.5']

# Match punctuation and symbols
print(re.findall(r'[^\w\s]', 'hello, world! #42'))  # [',', '!', '#']
```

Shorthand behavior varies across engines. Python 3's `re` module makes `\w` match Unicode word characters by default:

```python
import re

print(re.findall(r'\w+', 'café résumé'))  # ['café', 'résumé']
```

### Anchors

Anchors match positions in the input string, not characters. They are zero-width assertions — they do not consume characters.

| Anchor | Meaning |
|--------|---------|
| `^` | Start of string (or line in multiline mode) |
| `$` | End of string (or line in multiline mode) |
| `\b` | Word boundary — between a word char and a non-word char |
| `\B` | Non-word boundary |

```python
import re

# ^ and $ for start/end anchoring
print(re.findall(r'^hello', 'hello world'))    # ['hello']
print(re.findall(r'^hello', 'say hello'))      # [] (not at start)
print(re.findall(r'world$', 'hello world'))    # ['world']
print(re.findall(r'world$', 'hello worlds'))   # [] (not at end)

# \b word boundary
print(re.findall(r'\bcat\b', 'cat concat scat'))  # ['cat'] (standalone only)
print(re.findall(r'\bcat', 'cat concat'))          # ['cat', 'cat']
print(re.findall(r'cat\b', 'cat scat'))            # ['cat', 'cat']

# \B non-word boundary
print(re.findall(r'\Bcat\B', 'concatenate'))       # ['cat'] (embedded)
```

Using anchors for validation:

```python
import re

# Without anchors: substring match
print(bool(re.search(r'\d{5}', '123456')))   # True (partial match)

# With anchors: full string must match
print(bool(re.match(r'^\d{5}$', '12345')))   # True
print(bool(re.match(r'^\d{5}$', '123456')))  # False
```

Multiline mode changes `^` and `$` to match line boundaries:

```python
import re

text = 'line1\nline2\nline3'

print(re.findall(r'^\w+', text))                       # ['line1']
print(re.findall(r'^\w+', text, re.MULTILINE))         # ['line1', 'line2', 'line3']
print(re.findall(r'\w+$', text, re.MULTILINE))         # ['line1', 'line2', 'line3']
```

### Quantifiers

Quantifiers specify how many times the preceding element must match.

| Quantifier | Meaning |
|------------|---------|
| `*` | Zero or more |
| `+` | One or more |
| `?` | Zero or one (optional) |
| `{n}` | Exactly n times |
| `{n,}` | n or more |
| `{n,m}` | Between n and m (inclusive) |

```python
import re

print(re.findall(r'ca*t', 'ct cat caat caaat'))       # ['ct', 'cat', 'caat', 'caaat']
print(re.findall(r'ca+t', 'ct cat caat'))              # ['cat', 'caat']
print(re.findall(r'colou?r', 'color colour'))          # ['color', 'colour']
print(re.findall(r'\b\d{5}\b', 'zip 12345 123456 1234'))  # ['12345']
print(re.findall(r'\b\d{3,}\b', '1 12 123 1234'))     # ['123', '1234']
print(re.findall(r'\b\d{2,4}\b', '1 12 123 1234 12345'))  # ['12', '123', '1234']
```

Quantifiers are greedy by default: they match as many characters as possible while still allowing the overall pattern to succeed.

```python
import re

text = '<b>bold</b> and <i>italic</i>'
print(re.findall(r'<.*>', text))   # ['<b>bold</b> and <i>italic</i>']  (too greedy)

# Without group: quantifier applies only to preceding char
print(re.findall(r'ab+', 'ab abb abbb'))       # ['ab', 'abb', 'abbb']
# With group: quantifier applies to entire group
print(re.findall(r'(ab)+', 'ab abab ababab'))  # ['ab', 'ab', 'ab']
```

Using quantifiers for common patterns:

```python
import re

# US phone with flexible separators
flex = r'\d{3}[-. ]\d{3}[-. ]\d{4}'
print(re.findall(flex, '555-123-4567 555.123.4567 555 123 4567'))
# ['555-123-4567', '555.123.4567', '555 123 4567']

# Optional area code
area = r'(\(\d{3}\) )?\d{3}-\d{4}'
print(re.findall(area, '(555) 123-4567 987-6543'))  # ['(555) ', '']
```

### Greedy vs Lazy Quantifiers

Appending `?` makes a quantifier lazy (non-greedy), matching as few characters as possible.

| Greedy | Lazy | Meaning |
|--------|------|---------|
| `*` | `*?` | Zero or more (lazy) |
| `+` | `+?` | One or more (lazy) |
| `?` | `??` | Zero or one (lazy — prefers zero) |
| `{n,m}` | `{n,m}?` | Between n and m (lazy) |

```python
import re

html = '<b>text</b> and <i>more</i>'
print(re.findall(r'<.*>', html))     # ['<b>text</b> and <i>more</i>']  (greedy)
print(re.findall(r'<.*?>', html))    # ['<b>', '</b>', '<i>', '</i>']   (lazy)

# Greedy backtracks to find last match
text = 'The sun is bright and the sky is blue'
print(re.search(r'(.*) is', text).group(1))   # 'The sun is bright and the sky'

# Lazy stops at first match
print(re.search(r'(.*?) is', text).group(1))  # 'The sun'

# findall: greedy captures last possible, lazy captures individually
text_q = '"first" and "second" and "third"'
print(re.findall(r'"(.*?)"', text_q))  # ['first', 'second', 'third']
print(re.findall(r'"(.*)"', text_q))   # ['first" and "second" and "third']
```

Lazy quantifiers can hurt performance due to stepwise expansion:

```python
import re, time

text = 'a' * 1000 + 'b'

start = time.time()
re.match(r'a+b', text)      # greedy: matches all 1000, backtracks once
print(f'Greedy: {time.time() - start:.6f}s')

start = time.time()
re.match(r'a+?b', text)     # lazy: starts with 1, expands 1000 times
print(f'Lazy: {time.time() - start:.6f}s')
```

### Alternation

The alternation operator `|` matches one of several alternatives, like a logical OR.

```python
import re

print(re.findall(r'cat|dog|bird', 'I have a cat and a dog and a bird'))
# ['cat', 'dog', 'bird']
```

Alternation is greedy left-to-right. Put longer alternatives first:

```python
import re

# Short alternative wins when placed first
text = 'catfish'
print(re.search(r'cat|catfish', text).group())     # 'cat'

# Longer alternative first: full match
print(re.search(r'catfish|cat', text).group())     # 'catfish'
```

Alternation has the lowest precedence. It splits the entire pattern unless constrained by grouping:

```python
import re

# Without group: matches "cat" OR "dog fish"
print(re.findall(r'cat|dog fish', 'cat dog fish'))  # ['cat', 'dog fish']

# With group: matches "cat fish" OR "dog fish"
print(re.findall(r'(?:cat|dog) fish', 'cat dog fish'))  # ['dog fish']
```

Using alternation for flexible patterns:

```python
import re

# Filter by file extension
files = ['file.txt', 'image.jpg', 'doc.pdf', 'script.py']
pattern = r'\.(txt|jpg|pdf)$'
print([f for f in files if re.search(pattern, f)])
# ['file.txt', 'image.jpg', 'doc.pdf']

# Multiple date formats
dates = ['2024-01-15', '01/15/2024', '2024.01.15']
p = r'\d{4}[-/\.]\d{2}[-/\.]\d{2}|\d{2}[-/\.]\d{2}[-/\.]\d{4}'
print([d for d in dates if re.match(p, d)])
# ['2024-01-15', '01/15/2024', '2024.01.15']
```

### Grouping with Parentheses

Parentheses `(...)` serve two purposes: grouping (applying quantifiers to sequences) and capturing (storing matched text).

Capturing groups store the matched substring:

```python
import re

m = re.search(r'(cat|dog)s?', 'dogs')
print(m.group(0))   # 'dogs'  (full match)
print(m.group(1))   # 'dog'   (captured group)

# Nested groups: numbered by opening paren order
m = re.search(r'((\d{4})-(\d{2}))-(\d{2})', '2024-01-15')
print(m.group(1))   # '2024-01'
print(m.group(2))   # '2024'
print(m.group(3))   # '01'
print(m.group(4))   # '15'
```

Non-capturing groups `(?:...)` group without storing the match:

```python
import re

# Capturing group: findall returns captured groups
print(re.findall(r'(cat|dog)s?', 'cats and dogs'))  # ['cat', 'dog']

# Non-capturing group: findall returns full matches
print(re.findall(r'(?:cat|dog)s?', 'cats and dogs'))  # ['cats', 'dogs']
```

Using groups for extraction:

```python
import re

# Parse key=value pairs
text = 'name=John age=30 city=NY'
print(re.findall(r'(\w+)=(\w+)', text))
# [('name', 'John'), ('age', '30'), ('city', 'NY')]

# Parse structured log line
log = '2024-01-15 10:30:00 ERROR [user42] Connection failed'
pattern = r'(\d{4}-\d{2}-\d{2}) (\d{2}:\d{2}:\d{2}) (\w+) \[(\w+)\] (.+)'
print(re.search(pattern, log).groups())
# ('2024-01-15', '10:30:00', 'ERROR', 'user42', 'Connection failed')
```

### Escape Sequences

Escape sequences provide special character matching and non-printable character support.

Non-printable characters: `\t` (tab), `\n` (newline), `\r` (carriage return), `\f` (form feed), `\0` (null).

```python
import re

tsv = 'name\tage\tcity\nJohn\t30\tNY'
print(re.split(r'\t', tsv)[:3])           # ['name', 'age', 'city']
print(re.split(r'\r?\n', 'line1\r\nline2\nline3'))  # ['line1', 'line2', 'line3']
```

Hexadecimal and Unicode escapes: `\xNN` (ASCII hex), `\uNNNN` (Unicode BMP).

```python
import re

print(re.findall(r'\x24', 'price: $19.99'))   # ['$']  (0x24 = dollar sign)
print(re.findall(r'\u00e9', 'café'))           # ['é']
```

Raw strings in Python prevent the string parser from interpreting backslashes before the regex engine sees them:

```python
import re

# Without raw string: quadruple backslash for literal backslash
print(re.search('\\\\', 'a\\b').group())    # '\'

# With raw string: natural regex escaping
print(re.search(r'\\', 'a\\b').group())     # '\'

# Matching Windows paths
print(re.findall(r'[A-Z]:\\.+', 'C:\\Users\\john'))
# ['C:\\Users\\john']
```

### Building Patterns Step by Step

The most reliable way to write complex regex is to start simple and add layers incrementally.

**Building a US phone number matcher:**

```python
import re

# Step 1: just 10 digits
print(bool(re.match(r'\d{10}', '5551234567')))  # True

# Step 2: allow common separators
p = r'\d{3}[-. ]?\d{3}[-. ]?\d{4}'
print(bool(re.match(p, '555-123-4567')))   # True
print(bool(re.match(p, '555.123.4567')))   # True

# Step 3: support area code parentheses
p = r'(\(\d{3}\)\s?)?\d{3}[-. ]?\d{3}[-. ]?\d{4}'
print(bool(re.match(p, '(555) 123-4567')))  # True

# Step 4: optional country code
p = r'(\+1\s?)?(\(\d{3}\)\s?)?\d{3}[-. ]?\d{3}[-. ]?\d{4}'
tests = ['+1 (555) 123-4567', '(555) 123-4567', '555-123-4567']
for t in tests:
    print(f'{t}: {bool(re.match(p, t))}')
# All True

# Step 5: capture groups for structured output
p = r'^(\+1\s?)?(\(\d{3}\)\s?)?(\d{3})[-. ]?(\d{3})[-. ]?(\d{4})$'
for t in tests:
    m = re.match(p, t)
    if m:
        print(m.groups())
# ('+1 ', '(555) ', '555', '123', '4567')
# (None, '(555) ', '555', '123', '4567')
# (None, None, '555', '123', '4567')
```

**Extracting function signatures from Python code:**

```python
import re

code = '''
def hello():
    pass
def greet(name, greeting="Hello"):
    pass
def complex_func(a, b=1, *args, **kwargs):
    pass
'''

# Step 1: capture function name
print(re.findall(r'def (\w+)\(', code))
# ['hello', 'greet', 'complex_func']

# Step 2: capture name and parameters
p = r'def (\w+)\(([^)]*)\)'
print(re.findall(p, code))
# [('hello', ''), ('greet', 'name, greeting="Hello"'), ('complex_func', 'a, b=1, *args, **kwargs')]

# Step 3: include preceding decorator
code2 = '''
@staticmethod
def utility(a, b):
    pass
def plain():
    pass
'''
p = r'(?:@\w+\n)?def (\w+)\(([^)]*)\)'
print(re.findall(p, code2))
# [('utility', 'a, b'), ('plain', '')]
```

**Parsing a configuration file:**

```python
import re

config = '''
DB_HOST = localhost
DB_PORT = 5432
DEBUG = true
'''

p = r'^(\w+)\s*=\s*(.+)$'
for m in re.finditer(p, config, re.MULTILINE):
    print(f'{m.group(1)} => {m.group(2)}')
# DB_HOST => localhost
# DB_PORT => 5432
# DEBUG => true
```

### Common Beginner Mistakes

**1. Forgetting to escape the dot**

```python
import re

print(re.findall(r'file.txt', 'file.txt file_txt'))  # ['file.txt', 'file_txt']  (too broad)
print(re.findall(r'file\.txt', 'file.txt file_txt'))  # ['file.txt']  (correct)
```

**2. Misusing `^` inside character classes**

```python
import re

print(bool(re.search(r'[^a]', 'aaa')))     # False (negates the class)
print(bool(re.search(r'^[a]+$', 'aaa')))   # True (anchor outside brackets)
```

**3. Overusing `.*` when negated classes work better**

```python
import re

text = 'key1=val1&key2=val2&key3=val3'
print(re.findall(r'key1=(.*)&key2=(.*)&key3=(.*)', text))
# [('val1', 'val2', 'val3')]  -- works but brittle

print(re.findall(r'key1=([^&]*)&key2=([^&]*)&key3=(.*)', text))
# [('val1', 'val2', 'val3')]  -- explicit delimiter

# Generic key-value parser
print(re.findall(r'(\w+)=([^&]+)', text))
# [('key1', 'val1'), ('key2', 'val2'), ('key3', 'val3')]
```

**4. Assuming regex matches whole strings**

```python
import re

print(bool(re.search(r'\d{5}', '123456')))     # True (substring match)
print(bool(re.match(r'^\d{5}$', '12345')))     # True (full match)
print(bool(re.match(r'^\d{5}$', '123456')))    # False
```

**5. Not accounting for case sensitivity**

```python
import re

print(bool(re.search(r'hello', 'Hello World')))                # False
print(bool(re.search(r'hello', 'Hello World', re.IGNORECASE))) # True
```

**6. Alternation order causing unexpected matches**

```python
import re

path = 'users/123/posts/456'
print(re.search(r'users/\d+|users/\d+/posts/\d+', path).group())
# 'users/123'  (shorter alternative matches first)

print(re.search(r'users/\d+/posts/\d+|users/\d+', path).group())
# 'users/123/posts/456'  (most specific first)
```

**7. Forgetting that dot does not match newlines**

```python
import re

text = 'line1\nline2'
print(re.findall(r'line1.line2', text))             # []
print(re.findall(r'line1.line2', text, re.DOTALL))   # ['line1\nline2']
```

## Study Cases

### Study Case 1: Validating and Parsing Email Addresses

```python
import re

email_pattern = re.compile(r'''
    ^
    (?P<local>[a-zA-Z0-9._%+-]+)
    @
    (?P<domain>[a-zA-Z0-9.-]+\.[a-zA-Z]{2,})
    $
''', re.X)

def parse_email(email: str):
    m = email_pattern.match(email)
    return m.groupdict() if m else None

tests = [
    'user@example.com',
    'first.last@sub.domain.org',
    'user+tag@example.co.uk',
    'invalid@com',
    '@example.com',
    'user@.com',
]
for t in tests:
    result = parse_email(t)
    print(f'{t:40s} => {result}')
```

### Study Case 2: Apache Combined Log Parsing

```python
import re

log_pattern = re.compile(r'''
    ^
    (?P<ip>\d+\.\d+\.\d+\.\d+)\s-\s-
    \s\[(?P<timestamp>[^\]]+)\]
    \s"(?P<method>\w+)\s(?P<path>[^\s]+)\sHTTP/\d\.\d"
    \s(?P<status>\d{3})
    \s(?P<size>\d+|-)
    \s"(?P<referer>[^"]*)"
    \s"(?P<ua>[^"]*)"
    $
''', re.X)

line = '192.168.1.1 - - [15/Jan/2024:10:30:15 +0000] "GET /api/users HTTP/1.1" 200 1234 "https://example.com" "Mozilla/5.0"'
m = log_pattern.match(line)
if m:
    for k, v in m.groupdict().items():
        print(f'{k}: {v}')
```

### Study Case 3: JavaScript `var` to `let`/`const` Conversion

```python
import re

js_code = '''
var name = "John";
var count = 42;
var result = computeValue(name, count);
'''

def replace_var(m):
    name, value = m.group(1), m.group(2)
    if re.match(r'^["\']', value) or value in ('true', 'false', 'null') or re.match(r'^\d+\.?\d*$', value):
        return f'const {name} = {value};'
    return f'let {name} = {value};'

result = re.sub(r'\bvar\s+(\w+)\s*=\s*([^;]+);', replace_var, js_code)
print(result)
# const name = "John";
# const count = 42;
# let result = computeValue(name, count);
```

```

## Glossary

| Term | Definition |
|------|------------|
| Alternation | The `\|` operator matching one of several patterns with left-to-right priority |
| Anchor | A zero-width assertion matching a position (`^` start, `$` end, `\b` word boundary) |
| Backtracking | The engine's process of trying alternative quantifier distributions when a match fails |
| Capturing group | Parentheses `(...)` that store the matched substring for later access |
| Character class | Brackets `[...]` defining a set of characters, any one of which matches at that position |
| DOTALL | A regex flag (`re.DOTALL`) making `.` match newline characters |
| Escape sequence | A backslash followed by a character for special meaning or literal matching |
| Greedy quantifier | A quantifier matching the maximum characters possible while allowing overall match |
| Lazy quantifier | A quantifier matching the minimum characters possible, denoted by appended `?` |
| Metacharacter | A character with special regex meaning (`. ^ $ * + ? { } [ ] \ \| ( )`) |
| MULTILINE | A regex flag (`re.MULTILINE`) making `^` and `$` match line boundaries |
| Negated character class | `[^...]` matching any character NOT in the specified set |
| Non-capturing group | `(?:...)` that groups without storing the matched text |
| Quantifier | A metacharacter specifying repetition (`*` zero+, `+` one+, `?` zero/one, `{n,m}` range) |
| Raw string | A Python string `r'...'` preventing backslash interpretation before the regex engine |
| Shorthand class | A backslash-letter escape for common sets (`\d` digit, `\w` word char, `\s` whitespace) |
| Word boundary | The position `\b` between a word char and a non-word char, or at string edges |
| Zero-width assertion | A regex construct matching a position without consuming input characters |

## Next Steps

- [Groups, Capture & Backreferences](groups-and-backreferences.md) — extracting and reusing matched text
- [Lookahead & Lookbehind Assertions](lookahead-and-lookbehind.md) — context-sensitive matching without consuming characters
- [Regex Engine Internals](regex-engine-internals.md) — how the engine processes patterns behind the scenes
