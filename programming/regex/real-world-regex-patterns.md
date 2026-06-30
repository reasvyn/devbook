# Real-World Regex Patterns

## Description

A practical reference of battle-tested regex patterns for common validation, extraction, and text-processing tasks. Each pattern includes an analysis of what it matches, its edge cases, performance characteristics, and why simpler alternatives may be safer or more correct.

## Prerequisites

- [Regex in Programming Languages](regex-in-programming-languages.md) — API differences and feature support across Python, JavaScript, Go, Rust, Java, and PCRE

## Table of Contents

- [Email Validation](#email-validation)
- [URL Matching](#url-matching)
- [IP Addresses](#ip-addresses)
- [Date and Time Patterns](#date-and-time-patterns)
- [Phone Numbers](#phone-numbers)
- [Password Validation](#password-validation)
- [Code-Related Patterns](#code-related-patterns)
- [Log Parsing Patterns](#log-parsing-patterns)
- [Text Processing Patterns](#text-processing-patterns)
- [Performance and Safety Notes](#performance-and-safety-notes)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Email Validation

Email validation with regex is a famously contentious topic. An RFC 5322 compliant regex exists but is hundreds of characters long and still cannot verify deliverability.

#### Simple Structural Check

```python
import re

simple = r'^[^@\s]+@[^@\s]+\.[^@\s]+$'
print(re.match(simple, "alice@example.com"))   # match
print(re.match(simple, "a@b.c"))               # match (valid structure)
print(re.match(simple, "@example.com"))        # None — no local part
print(re.match(simple, "alice@"))              # None — no domain
print(re.match(simple, "alice @example.com"))  # None — space in local part
```

This pattern checks for the bare minimum: at least one character before `@`, at least one character after `@`, a dot in the domain, and no spaces. It rejects structurally invalid inputs but accepts technically valid addresses like `a@b.c`.

#### More Comprehensive (but still imperfect)

```python
comprehensive = r'^(?![.])[A-Za-z0-9!#$%&\'*+/=?^_`{|}~.-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,}$'
```

This adds:
- Negative lookahead `(?![.])` — local part cannot start with a dot
- Restricted character set for the local part (RFC 5321/5322 subset)
- Domain must end with a TLD of at least 2 letters

Still misses (intentionally):
- Quoted local parts like `"john.doe"@example.com`
- Comments like `(comment)@example.com`
- Internationalized email addresses (RFC 6531)
- IP address literals in brackets: `user@[192.168.1.1]`

```python
tests = [
    "user@example.com",           # valid
    "user.name+tag@example.com",  # valid (plus addressing)
    "user@sub.example.com",       # valid
    ".user@example.com",          # rejected — starts with dot
    "user@example",               # rejected — no TLD dot
    "user@.example.com",          # rejected — dot after @
    "a@b.c",                      # accepted — technically valid
]
```

#### Best Practice

Validate structure with a simple regex, then verify deliverability by sending a confirmation email. Never reject an email address because your regex is too strict — you will lose users.

### URL Matching

#### HTTP/HTTPS URLs

```python
import re

url_pattern = r'https?://[^\s/$.?#][^\s]*'
text = "Visit https://example.com/path?q=1#section or http://test.org"

urls = re.findall(url_pattern, text)
print(urls)  # ['https://example.com/path?q=1#section', 'http://test.org']
```

This pattern matches URLs starting with `http://` or `https://`. The first character class `[^\s/$.?#]` prevents matching empty schemes. The second `[^\s]*` captures everything up to whitespace.

Limitations:
- Does not validate the domain structure (e.g., `https://invalid` matches)
- Includes trailing punctuation (a period after a URL at end of sentence gets captured)
- Does not match URLs without the scheme (e.g., `example.com/path`)

#### Extracting URL Components

```python
url_components = r'^(https?)://([^\s/:?#]+)(?::(\d+))?(/[^\s?#]*)?(?:\?([^\s#]*))?(?:#([^\s]*))?$'
url = "https://api.example.com:443/users?page=1#top"

match = re.match(url_components, url)
if match:
    print(f"Protocol: {match.group(1)}")    # https
    print(f"Host:     {match.group(2)}")    # api.example.com
    print(f"Port:     {match.group(3)}")    # 443
    print(f"Path:     {match.group(4)}")    # /users
    print(f"Query:    {match.group(5)}")    # page=1
    print(f"Fragment: {match.group(6)}")    # top
```

Each component is optional after the host. The colon for the port, the `?` for the query string, and the `#` for the fragment are included in the optional groups so they only appear when the component exists.

#### Handling Query Parameters

Extracting key-value pairs from a query string requires splitting on `&` and `=`:

```python
query = "page=1&limit=10&sort=name"
params = dict(re.findall(r'(\w+)=(\w+)', query))
print(params)  # {'page': '1', 'limit': '10', 'sort': 'name'}
```

But URLs may contain encoded characters (`%20` for space), multiple values for the same key (`key=a&key=b`), and empty values (`key=`). A production parser should use `urllib.parse.parse_qs` in Python or equivalent in other languages instead of raw regex.

### IP Addresses

#### IPv4

```python
ipv4 = r'^(?:(?:25[0-5]|2[0-4]\d|[01]?\d\d?)\.){3}(?:25[0-5]|2[0-4]\d|[01]?\d\d?)$'

tests = [
    "192.168.1.1",     # valid
    "255.255.255.255", # valid — broadcast address
    "0.0.0.0",         # valid
    "256.1.2.3",       # invalid — octet > 255
    "192.168.1",       # invalid — only 3 octets
    "192.168.1.01",    # accepted by regex, but ambiguous (octal? decimal?)
    "192.168.1.0",     # valid
]

for t in tests:
    print(f"{t}: {bool(re.match(ipv4, t))}")
```

The pattern ensures each octet is 0-255 using three alternatives:
- `25[0-5]` — matches 250-255
- `2[0-4]\d` — matches 200-249
- `[01]?\d\d?` — matches 0-199 (with optional leading 0 or 1)

The group followed by `{3}` matches the first three octets with trailing dots, and the final group matches the fourth octet without a dot.

Edge case: `192.168.1.01` is matched, but leading zeros can be ambiguous — some systems interpret `01` as octal (value 1), others as decimal (value 1, same result for single-digit). For strict validation, add a negative lookahead for leading zeros: `(?!0\d)`.

#### IPv6 (Simplified)

Full IPv6 validation is extremely complex due to the many compression rules. A practical subset:

```python
# Matches most common IPv6 representations
ipv6 = r'^(?:(?:[0-9a-fA-F]{1,4}:){7}[0-9a-fA-F]{1,4})$'

# Looser: allows :: compression once
ipv6_loose = r'^(?:(?:[0-9a-fA-F]{1,4}:){1,7}:|(?:[0-9a-fA-F]{1,4}:){1,6}:[0-9a-fA-F]{1,4}|(?:[0-9a-fA-F]{1,4}:){1,5}(?::[0-9a-fA-F]{1,4}){1,2}|(?:[0-9a-fA-F]{1,4}:){1,4}(?::[0-9a-fA-F]{1,4}){1,3}|(?:[0-9a-fA-F]{1,4}:){1,3}(?::[0-9a-fA-F]{1,4}){1,4}|(?:[0-9a-fA-F]{1,4}:){1,2}(?::[0-9a-fA-F]{1,4}){1,5}|[0-9a-fA-F]{1,4}:(?::[0-9a-fA-F]{1,4}){1,6}|:(?::[0-9a-fA-F]{1,4}){1,7}|::)$'
```

The full pattern is long because IPv6 allows omitting segments with `::`, and that compression can appear at the start, middle, or end of the address. In practice, prefer using the platform's IP address parser rather than a regex for IPv6.

### Date and Time Patterns

#### ISO 8601 Date

```python
iso_date = r'^\d{4}-\d{2}-\d{2}$'
print(re.match(iso_date, "2024-12-25"))  # match
print(re.match(iso_date, "2024-13-01"))  # matches but invalid month!
```

Basic validation: 4 digits, hyphen, 2 digits, hyphen, 2 digits. This does not validate that the month is 01-12 or the day is valid for the given month. For correctness:

```python
iso_date_strict = r'^(?:(?:19|20)\d{2})-(?:0[1-9]|1[0-2])-(?:0[1-9]|[12]\d|3[01])$'

tests = [
    "2024-12-25",  # valid
    "2024-02-30",  # accepted by regex but Feb 30 does not exist!
    "2024-13-01",  # rejected — month 13
    "2024-00-01",  # rejected — month 00
    "0999-12-31",  # rejected — year 0999 does not start with 19/20
]
```

The strict pattern restricts years to 1900-2099 and months to 01-12. Day validation (01-31) still accepts Feb 30. Full date validation requires a calendar-aware parser, not a regex.

#### Multiple Date Formats

```python
date_formats = r'\d{2}[/-]\d{2}[/-]\d{4}'
text = "Events: 12/25/2024, 01-01-2025, and 31/12/2024"
print(re.findall(date_formats, text))
# ['12/25/2024', '01-01-2025', '31/12/2024']
```

This captures dates with `/` or `-` separators. It does not distinguish between MM/DD/YYYY and DD/MM/YYYY — both `12/25/2024` and `31/12/2024` match. When extracting dates from unstructured text, you need context to resolve the ambiguity.

#### Time

```python
time_pattern = r'^(?:[01]\d|2[0-3]):[0-5]\d(?::[0-5]\d)?$'

tests = [
    "14:30",       # valid
    "09:05:30",    # valid — with seconds
    "24:00",       # rejected — 24 is not a valid hour
    "14:60",       # rejected — 60 is not a valid minute
    "2:30",        # rejected — missing leading zero
]
```

The pattern uses `[01]\d|2[0-3]` for hours (00-23) and `[0-5]\d` for minutes/seconds (00-59). Seconds are optional via `(?::[0-5]\d)?`.

### Phone Numbers

#### International (E.164)

```python
e164 = r'^\+?[1-9]\d{1,14}$'

tests = [
    "+14155552671",  # valid US number
    "+442071234567", # valid UK number
    "14155552671",   # valid — plus sign is optional
    "+123456789012345",  # valid — 15 digits max
    "+01234567890",  # rejected — first digit after + must be 1-9
    "+",             # rejected — no digits
]
```

E.164 format allows up to 15 digits total, with a country code starting at 1-9. This pattern is intentionally simple and does not validate area codes, exchange codes, or local number formats. For display formatting, additional parsing is needed.

#### North American (NANP)

```python
nanp = r'^\+1\(?\d{3}\)?\s?\d{3}[-.\s]?\d{4}$'

tests = [
    "+14155552671",        # valid
    "+1(415)555-2671",     # valid — with parentheses
    "+1 415 555 2671",     # valid — spaces
    "+1-415-555-2671",     # valid — dashes
    "+1415555267",         # rejected — too few digits
    "+141555526712",       # rejected — too many digits
]
```

This pattern matches the +1 country code followed by an area code (3 digits, optionally in parentheses), a prefix (3 digits), and a line number (4 digits), with various separator styles. It does not validate that the area code or exchange code is valid (e.g., 911 as an area code would match but may be invalid).

### Password Validation

```python
password = r'^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[^a-zA-Z\d]).{8,}$'

tests = [
    "Abcd1234!",    # valid
    "password",     # rejected — no uppercase, digit, or special
    "PASSWORD1!",   # rejected — no lowercase
    "Ab1!",         # rejected — too short (4 < 8)
    "Abcd1234",     # rejected — no special character
    "Abcdefgh1!",   # valid
]
```

The pattern uses four lookaheads to enforce:
- `(?=.*[a-z])` — at least one lowercase letter
- `(?=.*[A-Z])` — at least one uppercase letter
- `(?=.*\d)` — at least one digit
- `(?=.*[^a-zA-Z\d])` — at least one special character (anything not alphanumeric)
- `.{8,}` — at least 8 characters

Important caveats:
- The lookaheads are checked independently, so `.*` can backtrack and cause poor performance on near-misses
- The special character class `[^a-zA-Z\d]` matches spaces and non-ASCII characters too — tighten to `[!@#$%^&*()_+\-=\[\]{};':"\\|,.<>\/?]` for strict sets
- Password policies should not silently reject valid passwords — provide clear error messages
- Length limits should be generous (at least 64+ characters) to avoid frustrating password manager users

For better performance on long passwords, anchor each lookahead to the start:

```python
password_optimized = r'^(?=[^a-z]*[a-z])(?=[^A-Z]*[A-Z])(?=\D*\d)(?=[a-zA-Z\d]*[^a-zA-Z\d]).{8,}$'
```

Each negated character class `[^a-z]*` before `[a-z]` reduces backtracking compared to `.*`, which can match and then give back characters.

### Code-Related Patterns

#### Hex Color Values

```python
hex_color = r'^#([0-9a-fA-F]{3}|[0-9a-fA-F]{6})$'

tests = [
    "#fff",          # valid — shorthand
    "#FFFFFF",       # valid — uppercase
    "#a1b2c3",       # valid
    "#1234",         # rejected — 4 digits
    "#ggg",          # rejected — invalid hex char
    "fff",           # rejected — no #
    "#abc12345",     # rejected — 8 digits
]
```

This pattern matches `#` followed by exactly 3 or 6 hex digits. For CSS, the 8-digit hex (RGBA) is also valid:

```python
css_hex = r'#[0-9a-fA-F]{3,8}\b'
text = "Colors: #fff, #a1b2c3, #12345678, and #12345"
print(re.findall(css_hex, text, re.IGNORECASE))
# ['#fff', '#a1b2c3', '#12345678']  — note: #12345 is 5 digits, not valid
```

The word boundary `\b` prevents matching hex digits that are part of a longer word (e.g., `#abcdef123` would only match `#abcdef`). For strict CSS validation, limit to 3, 4, 6, or 8 digits.

#### HTML Tags

```python
html_tag = r'<([a-z]+)([^>]+)?>(.*?)<\/\1>'

text = "<p>Hello</p><div class='main'><span>nested</span></div>"

# Warning: this does NOT handle nested tags correctly
for match in re.finditer(html_tag, text, re.IGNORECASE | re.DOTALL):
    print(f"Tag: {match.group(1)}, Content: {match.group(3)[:20]}")
```

This pattern captures:
- Group 1: the tag name
- Group 2: attributes (optional)
- Group 3: the inner content (lazy `.*?`)

The `<\/\1>` backreference ensures the closing tag matches the opening tag name. The `re.DOTALL` flag allows `.*?` to match across newlines.

This fails on nested tags of the same type. For example, `<div><div></div></div>` — the first `</div>` closes the first opening `<div>`, not the innermost one. Regular expressions are not powerful enough to parse nested HTML. Use an HTML parser for production extraction.

#### Comment Extraction

```python
# Single-line comments
single_line = r'//.*'

text = "x = 1; // initialize x\n/* multi\nline */"
print(re.findall(single_line, text))  # ['// initialize x']

# Multi-line comments (does NOT handle nesting)
multi_line = r'/\*[\s\S]*?\*/'
print(re.findall(multi_line, text, re.MULTILINE))
# ['/* multi\nline */']
```

The multi-line pattern uses `[\s\S]*?` instead of `.*?` with `re.DOTALL` — the `[\s\S]` trick matches any character including newlines without requiring the DOTALL flag. The lazy quantifier `*?` prevents the pattern from combining multiple adjacent comments into one match.

Edge case: Patterns like `/* /* nested */ */` are incorrectly matched — the first `*/` ends the match, leaving ` */` unmatched. True nested comment parsing requires a context-free grammar.

#### UUID

```python
uuid = r'^[0-9a-f]{8}-[0-9a-f]{4}-[1-5][0-9a-f]{3}-[89ab][0-9a-f]{3}-[0-9a-f]{12}$'

tests = [
    "550e8400-e29b-41d4-a716-446655440000",  # valid UUID v4
    "00000000-0000-0000-0000-000000000000",  # valid nil UUID
    "550e8400-e29b-41d4-a716-44665544000Z",  # rejected — Z is not hex
    "550e8400-e29b-41d4-a716-4466554400000",  # rejected — too long
]
```

This pattern validates the standard 8-4-4-4-12 hex format. The version digit (the first digit of the third group) is constrained to `[1-5]` for UUID versions 1 through 5. The variant digit (the first digit of the fourth group) is constrained to `[89ab]` for RFC 4122 variant.

#### Base64

```python
base64 = r'^(?:[A-Za-z0-9+/]{4})*(?:[A-Za-z0-9+/]{2}==|[A-Za-z0-9+/]{3}=)?$'

tests = [
    "SGVsbG8=",                  # valid — "Hello"
    "SGVsbG8gV29ybGQ=",         # valid — "Hello World"
    "SGVsbG8gV29ybGQh",         # valid — "Hello World!" (no padding)
    "SGVsbG8=",                  # valid
    "!!!",                       # rejected — invalid characters
    "SGVsbG8",                   # rejected — invalid length (mod 4 != 0)
]
```

Base64 encoding groups 3 bytes into 4 characters. The pattern matches:
- Groups of 4 valid Base64 characters (any number)
- An optional final group of 2 characters with `==` padding, or 3 characters with `=` padding

The pattern does not match unpadded Base64 (where the final group has no padding). For URL-safe Base64 (RFC 4648), replace `+/` with `-_`.

#### Slug

```python
slug = r'^[a-z0-9]+(?:-[a-z0-9]+)*$'

tests = [
    "hello-world",       # valid
    "my-post-2024",      # valid — with numbers
    "Hello-World",       # rejected — uppercase
    "hello--world",      # rejected — double hyphen
    "-hello-world",      # rejected — leading hyphen
    "hello-world-",      # rejected — trailing hyphen
    "hi",                # valid — single word
]
```

The pattern ensures the slug starts and ends with an alphanumeric character, uses only lowercase letters, digits, and hyphens, and never has consecutive hyphens.

### Log Parsing Patterns

#### Apache/Nginx Common Log Format

```python
common_log = r'^(\S+)\s+(\S+)\s+(\S+)\s+\[([^\]]+)\]\s+"[^"]*"\s+(\d{3})\s+(\d+|-)\s*"?([^"]*)"?\s*(\S+)?\s*(\S+)?'

line = '192.168.1.1 - frank [10/Oct/2024:13:55:36 +0000] "GET /index.html HTTP/1.1" 200 2326 "https://example.com/" "Mozilla/5.0"'

match = re.match(common_log, line)
if match:
    print(f"IP:        {match.group(1)}")      # 192.168.1.1
    print(f"Identity:  {match.group(2)}")      # -
    print(f"User:      {match.group(3)}")      # frank
    print(f"Time:      {match.group(4)}")      # 10/Oct/2024:13:55:36 +0000
    print(f"Status:    {match.group(5)}")      # 200
    print(f"Bytes:     {match.group(6)}")      # 2326
    print(f"Referrer:  {match.group(7)}")      # https://example.com/
    print(f"UA:        {match.group(8)}")      # Mozilla/5.0
```

The common log format has these fields separated by whitespace:
- IP address (`\S+` — non-whitespace)
- Identity (`\S+` — typically `-`)
- User ID (`\S+` — typically `-` or a username)
- Timestamp in brackets (`\[([^\]]+)\]`)
- Request in quotes (matched but not captured here)
- Status code (3 digits)
- Bytes sent (digits or `-`)
- Referrer (in quotes, optional)
- User agent (in quotes, optional)

The pattern uses `\S+` for token-based fields and `[^\]]+` for bracketed fields. The final fields are optional — some log formats omit them, and the combined format adds fields beyond common.

#### Syslog Timestamps

```python
syslog_time = r'^([A-Z][a-z]{2}\s+\d{1,2}\s+\d{2}:\d{2}:\d{2})'

line = "Oct 10 13:55:36 myhost sshd[1234]: Failed password for root"
match = re.match(syslog_time, line)
if match:
    print(match.group(1))  # Oct 10 13:55:36
```

Syslog timestamps omit the year (inferred from the file's modification time) and use abbreviated month names. For ISO conversion, parse the month name and current year.

#### Stack Trace File Locations

```python
stack_file = r'File\s+"([^"]+)",\s+line\s+(\d+)'

trace = """Traceback (most recent call last):
  File "/app/src/main.py", line 42, in <module>
    result = process(data)
  File "/app/src/utils.py", line 15, in process
    raise ValueError("invalid input")
"""
for match in re.finditer(stack_file, trace):
    print(f"{match.group(1)}:{match.group(2)}")
    # /app/src/main.py:42
    # /app/src/utils.py:15
```

This extracts the file path (in quotes) and line number from Python-style stack traces. Adjust the pattern for other languages: Java uses `at com.example.MyClass.method(MyClass.java:42)`, JavaScript uses `at method (/app/file.js:42:10)`.

### Text Processing Patterns

#### Whitespace Normalization

```python
text = "Hello     world\n\n\n   this\t\tis   spaced."
normalized = re.sub(r'\s+', ' ', text).strip()
print(normalized)  # "Hello world this is spaced."
```

Replace any run of whitespace characters (spaces, tabs, newlines) with a single space. The `strip()` removes leading and trailing whitespace. This is a common preprocessing step for NLP pipelines and text indexing.

#### Sentence Splitting

```python
text = "Dr. Smith went to Washington. He met with Ms. Jones! Was that correct? Yes."
sentences = re.split(r'(?<=[.!?])\s+', text)
print(sentences)
# ['Dr. Smith went to Washington.', 'He met with Ms. Jones!', 'Was that correct?', 'Yes.']
```

The pattern uses positive lookbehind `(?<=[.!?])` to split after sentence-ending punctuation, keeping the punctuation attached to the sentence. The `\s+` consumes the whitespace between sentences.

This is imperfect — it does not handle:
- Abbreviations like "Dr.", "Ms.", "Prof.", "etc."
- Decimal numbers like "3.14"
- Ellipsis "..."
- Quoted sentences like `He said, "No." Then left.`

For production, use a dedicated sentence tokenizer (e.g., NLTK, spaCy) rather than a regex.

#### Word Counting

```python
text = "Hello, world! This is a test."
words = re.findall(r'\b\w+\b', text)
print(len(words))        # 6
print(words)             # ['Hello', 'world', 'This', 'is', 'a', 'test']
```

The `\b` word boundary anchor ensures each match is a complete word. `\w+` matches alphanumeric characters and underscores. This simple approach:
- Counts "can't" as two words ("can" and "t")
- Includes underscores as part of words (good for code, bad for prose)
- Does not handle hyphenated words ("well-known" becomes two words)
- Does not handle Unicode words outside ASCII

#### Redacting Sensitive Data

```python
text = """
Credit card: 4111-1111-1111-1111
SSN: 123-45-6789
Email: john.doe@example.com
"""

# Credit card numbers (major card providers)
# Luhn algorithm validation requires code, not regex alone
card_pattern = r'\b(?:\d{4}[-\s]?){3}\d{4}\b'
redacted = re.sub(card_pattern, '[CARD REDACTED]', text)

# SSNs
ssn_pattern = r'\b\d{3}-\d{2}-\d{4}\b'
redacted = re.sub(ssn_pattern, '[SSN REDACTED]', redacted)

# Emails
email_pattern = r'\b[\w.+-]+@[\w.-]+\.\w{2,}\b'
redacted = re.sub(email_pattern, '[EMAIL REDACTED]', redacted)

print(redacted)
# Credit card: [CARD REDACTED]
# SSN: [SSN REDACTED]
# Email: [EMAIL REDACTED]
```

Data redaction regexes must be carefully scoped to avoid false positives. The card pattern matches 16-digit numbers with optional separators, but also matches non-card numbers like "1234 5678 9012 3456" that fail the Luhn checksum. For production redaction, always combine pattern matching with algorithmic validation (Luhn for cards, area code rules for SSNs).

### Performance and Safety Notes

Every pattern in this document should be evaluated for performance impact in production:

| Pattern | Backtracking Risk | Notes |
|---------|------------------|-------|
| Email (simple) | Low | No nested quantifiers |
| Email (comprehensive) | Medium | Lookahead plus char class repeats |
| URL | Medium | Trailing `[^\s]*` can match long strings |
| IPv4 | Low | Fixed-length alternatives |
| IPv6 | Low | Fixed structure |
| Date ISO | Low | Fixed length |
| Time | Low | Fixed length |
| Phone E.164 | Low | Limited to 15 digits |
| Phone NANP | Low | Fixed structure |
| Password | High | Multiple lookaheads with `.*` on long strings |
| Hex color | Low | Fixed length |
| HTML tag | Medium | `.*?` on long content can cause issues |
| Comment | Medium | `[\s\S]*?` backtracking on mismatched delimiters |
| UUID | Low | Fixed length, no quantifier ambiguity |
| Base64 | Low | Fixed structure per block |
| Slug | Low | Simple alternation |
| Log line | Medium | Multiple optional groups |
| Card redaction | Low | Fixed structure |

General rules for safe regex:
1. Prefer fixed-length patterns (`\d{4}`) over variable-length repeats (`\d+`)
2. Avoid nested quantifiers like `(.*)*`
3. Anchor patterns with `^` and `$` when validating complete strings
4. Use possessive quantifiers or atomic groups when available and appropriate
5. Test patterns against adversarial inputs (1000+ character strings, strings that nearly match)
6. Use RE2-based engines (Go, Rust) when accepting user-supplied patterns

## Glossary

| Term | Definition |
|------|------------|
| E.164 | International telephone numbering plan with up to 15 digits, prefixed by `+` and country code |
| False positive | A string that matches a pattern but is not a valid instance of the target |
| Luhn algorithm | A checksum formula used to validate credit card numbers, identity numbers, and IMEIs |
| NANP | North American Numbering Plan — telephone numbering for US, Canada, and Caribbean |
| Negated character class | `[^...]` — matches any character NOT listed in the class |
| Positive lookahead | `(?=...)` — asserts the enclosed pattern follows the current position without consuming it |
| RFC 4122 | The standard defining UUID (Universally Unique Identifier) format and generation methods |
| RFC 5322 | The standard defining Internet message format, including email address syntax |
| Slug | A URL-safe identifier derived from a title, typically lowercase with hyphens |
| TLD | Top-Level Domain — the last segment of a domain name (e.g., `.com`, `.org`, `.io`) |
| UUID | Universally Unique Identifier — a 128-bit label typically represented as 32 hex digits in 8-4-4-4-12 groups |

## Quick References

- [RFC 5322 Email Validation](https://www.rfc-editor.org/rfc/rfc5322) — official email address format specification
- [E.164 Phone Number Format](https://www.itu.int/rec/T-REC-E.164) — international public telecommunication numbering
- [RFC 4122 UUID Format](https://www.rfc-editor.org/rfc/rfc4122) — UUID specification
- [ISO 8601 Date and Time Format](https://www.iso.org/iso-8601-date-and-time-format.html) — international date and time notation standard
- [Common Log Format (Apache)](https://httpd.apache.org/docs/current/logs.html) — Apache HTTP server log format documentation
- [Regex101](https://regex101.com/) — online regex tester with engine selection and debugger
- [Regexr](https://regexr.com/) — interactive regex learning and testing tool
- [NLTK Sentence Tokenizer](https://www.nltk.org/api/nltk.tokenize.html) — proper sentence segmentation beyond regex

## Next Steps

This is the final document in the Regular Expressions module. Continue to other programming topics or apply these patterns in your projects.
