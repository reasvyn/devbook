# Regex in Programming Languages

## Description

A practical comparison of how regular expressions are implemented and used across Python, JavaScript, Go, Rust, Java, PCRE-based languages, and command-line tools. Each language exposes different APIs, supports different features, and uses different underlying engines — porting a pattern without understanding these differences causes subtle bugs.

## Prerequisites

- [Regex Performance & Security](regex-performance-and-security.md) — engine types, catastrophic backtracking, and safe usage patterns

## Table of Contents

- [Python: The `re` Module](#python-the-re-module)
- [JavaScript: RegExp and String Methods](#javascript-regexp-and-string-methods)
- [Go: The `regexp` Package](#go-the-regexp-package)
- [Rust: The `regex` Crate](#rust-the-regex-crate)
- [Java: `java.util.regex`](#java-javautilregex)
- [PCRE: Perl Compatible Regular Expressions](#pcre-perl-compatible-regular-expressions)
- [Command-Line Tools: grep, sed, awk](#command-line-tools-grep-sed-awk)
- [Feature Comparison Table](#feature-comparison-table)
- [Porting Patterns Between Languages](#porting-patterns-between-languages)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Python: The `re` Module

Python's `re` module uses a traditional NFA backtracking engine similar to PCRE. It supports the full range of features including lookahead, lookbehind, backreferences, and atomic groups.

#### Core Functions

The module provides several top-level functions that operate on a pattern string and a target string. Compilation is cached internally, so calling these directly does not recompile on every call for simple patterns.

```python
import re

text = "Contact: alice@example.com or bob@test.org"

# re.search() — find the first match anywhere in the string
match = re.search(r'(\w+)@(\w+\.\w+)', text)
if match:
    print(match.group(0))   # alice@example.com
    print(match.group(1))   # alice
    print(match.group(2))   # example.com

# re.match() — only matches at the START of the string
m = re.match(r'\w+@\w+\.\w+', text)
print(m)  # None — text starts with "Contact: "

# re.fullmatch() — requires the ENTIRE string to match
m = re.fullmatch(r'.*@.*', text)
print(bool(m))  # True — entire string contains @

# re.findall() — returns all matches as a list of strings or tuples
emails = re.findall(r'(\w+)@(\w+\.\w+)', text)
print(emails)  # [('alice', 'example.com'), ('bob', 'test.org')]

# re.finditer() — iterator over match objects (memory efficient for large texts)
for match in re.finditer(r'(\w+)@(\w+\.\w+)', text):
    print(f"User: {match.group(1)}, Domain: {match.group(2)}")

# re.sub() — substitute matches with a replacement string or function
censored = re.sub(r'\w+@\w+\.\w+', '[REDACTED]', text)
print(censored)  # "Contact: [REDACTED] or [REDACTED]"

# re.split() — split string at pattern boundaries
parts = re.split(r'[,.\s]+', "apple, banana.orange  grape")
print(parts)  # ['apple', 'banana', 'orange', 'grape']
```

#### The `match` vs `search` Distinction

A common pitfall for developers moving from other languages: `re.match()` only checks the beginning of the string. If you want behavior equivalent to JavaScript's `str.match(/pattern/)` or Perl's `$str =~ /pattern/`, use `re.search()`.

```python
text = "hello world"
print(re.match(r'world', text))   # None — world is not at the start
print(re.search(r'world', text))  # <re.Match object>
```

#### Flags

Flags modify how the engine interprets the pattern. They can be passed as a bitwise-OR'd argument or embedded in the pattern string.

```python
text = "Hello\nWorld"

re.search(r'hello', text, re.IGNORECASE)               # matches "Hello"
re.findall(r'^\w+', text, re.MULTILINE)                # ['Hello', 'World']
re.search(r'Hello.World', text, re.DOTALL)             # dot matches newlines
re.search(r'\w+', 'café', re.ASCII)                    # matches 'caf' only

# re.VERBOSE — ignore whitespace and allow comments
re.search(r'\b\d{3}[-.\s]?\d{3}[-.\s]?\d{4}\b',
          "Call 555-123-4567 today", re.VERBOSE)

# Embedded flags in the pattern
re.search(r'(?i)hello', 'HELLO')
re.search(r'(?im)^hello', "HELLO\nworld", re.MULTILINE)
```

#### Named Groups and Backreferences

Python uses the `(?P<name>...)` syntax for named capture groups, which is unique among languages. Most others use `(?<name>...)` without the `P`.

```python
text = "2024-12-25"
pattern = r'(?P<year>\d{4})-(?P<month>\d{2})-(?P<day>\d{2})'
match = re.search(pattern, text)
print(match.group('year'))   # 2024
print(match.group('month'))  # 12

# Named backreference: (?P=name)
duplicate = re.search(r'(?P<word>\b\w+\b)\s+(?P=word)', "hello hello world")
print(duplicate.group())  # hello hello

# In replacement strings, use \g<name> or \g<number>
print(re.sub(r'(?P<year>\d{4})', r'\g<year>', "2024"))  # 2024
```

#### Atomic Groups and Possessive Quantifiers

Python supports atomic groups `(?>...)` and possessive quantifiers `++`, `*+`, `?+`, `{n,m}+`. These prevent backtracking into the group, which can prevent catastrophic backtracking.

```python
# Atomic group — once matched, the engine never backtracks into the group
re.search(r'(?>\d+)\w', '123a')   # fails — \d+ consumes all digits,
                                   # atomic prevents giving any back,
                                   # \w can't match 'a' without digits

# Equivalent without atomic group would match because \d+ gives back '3'
re.search(r'\d+\w', '123a')       # matches '123a' — \d+ gives back '3' to \w
```

#### Compilation and Debugging

`re.compile()` precompiles a pattern and returns a `Pattern` object. While top-level functions cache patterns internally, explicit compilation is useful for hot loops and attaching flags permanently. The `re.DEBUG` flag prints the engine's compiled bytecode (`LITERAL`, `MIN_REPEAT`, `ASSERT`, etc.) — invaluable for optimization.

```python
pattern = re.compile(r'\b\w+@\w+\.\w+\b', re.IGNORECASE)
pattern.search(text)
pattern.findall(text)

import re
re.compile(r'(?P<year>\d{4})-(?P<month>\d{2})', re.DEBUG)
```

#### Limits

Python's `re` module is vulnerable to catastrophic backtracking on nested quantifiers. Use the `regex` third-party library (not covered here) for advanced features like overlapping matches and recursive patterns. Python's default recursion limit also applies to regex — deeply nested patterns may raise `RecursionError`.

### JavaScript: RegExp and String Methods

JavaScript's regex support has evolved significantly. ES3 provided basic features; ES2018 added named groups, lookbehind, and dotall flag. The engine is a traditional NFA backtracking engine.

#### Literal Syntax and the RegExp Constructor

```javascript
// Literal syntax — preferred when pattern is known at authoring time
const re1 = /\d{3}-\d{4}/g;

// Constructor — needed for dynamic patterns from user input
const pattern = "\\d{3}-\\d{4}";
const flags = "g";
const re2 = new RegExp(pattern, flags);
```

Note the double backslash when using the constructor — the string is interpreted as a string literal first, so backslashes must be escaped.

#### RegExp Methods

```javascript
const re = /(\w+)@(\w+\.\w+)/;
const text = "Email: alice@example.com";

// .exec() — returns match object or null, advances lastIndex when global
let match = re.exec(text);
console.log(match[0]);       // "alice@example.com"
console.log(match[1]);       // "alice"
console.log(match.index);    // 7 — position in string

// .test() — returns boolean (faster than exec when you only need yes/no)
console.log(re.test(text));  // true
```

#### String Methods

```javascript
const text = "alice@example.com and bob@test.org";

// .match() — returns array or null (non-global: details like exec)
//             returns array of matches (global: strings only, no groups)
console.log(text.match(/\w+@\w+\.\w+/g));
// ["alice@example.com", "bob@test.org"]

console.log(text.match(/(\w+)@(\w+\.\w+)/));
// ["alice@example.com", "alice", "example.com", index: 0, ...]

// .matchAll() — ES2020, returns iterator with full match objects for global
for (const m of text.matchAll(/(\w+)@(\w+\.\w+)/g)) {
    console.log(m[1], m[2]);  // "alice" "example.com", then "bob" "test.org"
}

// .search() — returns index of first match or -1
console.log(text.search(/@/));  // 5

// .replace() — string replacement
console.log(text.replace(/\w+@\w+\.\w+/g, '[REDACTED]'));
// "[REDACTED] and [REDACTED]"

// .replace() with callback for dynamic replacement
const censored = text.replace(/(\w+)@(\w+\.\w+)/g, (match, user, domain) => {
    return `${user[0]}***@${domain}`;
});
console.log(censored);  // "a***@example.com and b***@test.org"

// .split() — split string at pattern
console.log("a,b,c".split(/,/));  // ["a", "b", "c"]
```

#### Flags

```javascript
/g  // global — find all matches, not just the first
/i  // ignore case
/m  // multiline — ^ and $ match line boundaries
/s  // dotall — ES2018, dot matches newlines
/u  // unicode — treat pattern and string as Unicode, enable \p{} escapes
/y  // sticky — match only at lastIndex position (no searching ahead)
```

The `y` flag (sticky) forces matching only at `lastIndex`, not by searching ahead. Unlike `g`, it does not advance past non-matching positions — it returns `false` if the pattern does not match at the exact cursor position.

#### Named Groups (ES2018)

JavaScript uses `(?<name>...)` for named groups — the same syntax as most other languages (unlike Python's `(?P<name>...)`).

```javascript
const re = /(?<year>\d{4})-(?<month>\d{2})-(?<day>\d{2})/;
const m = re.exec("2024-12-25");
console.log(m.groups.year);   // 2024
console.log(m.groups.month);  // 12

// Named backreference: \k<name>
const dup = /(?<word>\b\w+\b)\s+\k<word>/;
console.log(dup.test("hello hello"));  // true
```

#### Lookbehind (ES2018)

JavaScript gained lookbehind in ES2018. The syntax matches most other languages: `(?<=...)` for positive and `(?<!...)` for negative lookbehind.

#### Missing Features

JavaScript does not support possessive quantifiers (`++`, `*+`, `?+`), atomic groups (`(?>...)`), or the `\A`/`\Z` anchors. Use lookahead workarounds where possible. The lack of atomic groups means patterns with nested quantifiers are more prone to catastrophic backtracking.

### Go: The `regexp` Package

Go uses Google's RE2 engine, which guarantees linear-time matching by forgoing backtracking entirely. This means certain features are simply not available, but you can safely apply user-supplied patterns without DoS risk.

#### RE2 Limitations

| Missing Feature | Impact |
|----------------|--------|
| Backreferences | Cannot match the same text that appears in a captured group |
| Lookahead/lookbehind | Cannot assert context without consuming characters |
| Atomic groups | Cannot prevent backtracking (not needed — no backtracking) |
| Possessive quantifiers | Not applicable for the same reason |
| Recursive patterns | Not supported |

Golang's regexp is the right choice when safety matters more than expressive power. For features beyond RE2, consider using a C-based PCRE binding.

#### Core Functions

```go
package main

import (
    "fmt"
    "regexp"
)

func main() {
    text := "Contact: alice@example.com or bob@test.org"

    // Compile — returns (*Regexp, error); MustCompile panics on invalid pattern
    re, err := regexp.Compile(`(\w+)@(\w+\.\w+)`)
    if err != nil {
        panic(err)
    }

    // MustCompile for patterns known at compile time
    re2 := regexp.MustCompile(`\w+@\w+\.\w+`)

    // FindString — first match as string
    fmt.Println(re2.FindString(text))  // alice@example.com

    // FindStringSubmatch — full match and capture groups
    match := re.FindStringSubmatch(text)
    fmt.Println(match[0])  // alice@example.com
    fmt.Println(match[1])  // alice

    // FindAllString — all matches
    matches := re2.FindAllString(text, -1)  // -1 means all
    fmt.Println(matches)  // [alice@example.com bob@test.org]

    // ReplaceAllString — substitution
    result := re2.ReplaceAllString(text, "[REDACTED]")
    fmt.Println(result)   // Contact: [REDACTED] or [REDACTED]

    // ReplaceAllStringFunc — callback-based replacement
    result2 := re2.ReplaceAllStringFunc(text, func(s string) string {
        return "[FILTERED]"
    })

    // Split
    parts := regexp.MustCompile(`[,.\s]+`).Split("apple, banana.orange  grape", -1)
    fmt.Println(parts)  // [apple banana orange grape]

    // MatchString — boolean check
    fmt.Println(re2.MatchString(text))  // true

    // FindStringIndex — location of match as [start, end]
    loc := re2.FindStringIndex(text)
    fmt.Println(loc)  // [10, 27]
}
```

#### Named Groups

Go uses the same `(?P<name>...)` syntax as Python. Named groups can be accessed by name in the result map.

```go
re := regexp.MustCompile(`(?P<year>\d{4})-(?P<month>\d{2})-(?P<day>\d{2})`)
match := re.FindStringSubmatch("2024-12-25")
for i, name := range re.SubexpNames() {
    if i != 0 && name != "" {
        fmt.Printf("%s: %s\n", name, match[i])
    }
}
// Output:
// year: 2024
// month: 12
// day: 25
```

#### Performance Guarantee

Go's `regexp` runs in O(n) time regardless of pattern complexity. This makes it the safest choice for validating user-submitted patterns in web applications. The trade-off is that some patterns that would work in Python or JavaScript simply will not compile — Go will reject backreferences at compile time.

### Rust: The `regex` Crate

Rust's `regex` crate, like Go's, is based on RE2 and guarantees linear-time matching. It also uses a hybrid automaton approach (NFAs and DFAs) to achieve excellent performance.

#### Core API

```rust
use regex::Regex;

fn main() {
    let text = "Contact: alice@example.com or bob@test.org";

    // Regex::new returns Result; unwrap for known-valid patterns
    let re = Regex::new(r"(\w+)@(\w+\.\w+)").unwrap();

    // is_match — boolean
    assert!(re.is_match(text));

    // find — first match as Match struct
    if let Some(m) = re.find(text) {
        println!("Found: {} at {:?}", m.as_str(), m.range());
    }

    // captures — with capture groups
    if let Some(caps) = re.captures(text) {
        println!("Full: {}", &caps[0]);   // alice@example.com
        println!("User: {}", &caps[1]);   // alice
        println!("Domain: {}", &caps[2]); // example.com
    }

    // Iterating over all captures
    for caps in re.captures_iter(text) {
        println!("Found: {} as {}", &caps[1], &caps[2]);
    }

    // replace and replace_all
    let result = re.replace_all(text, "[REDACTED]");
    println!("{}", result);

    // With a closure for dynamic replacement
    let result = re.replace_all(text, |caps: &regex::Captures| {
        format!("{}@{}", &caps[1], &caps[2])
    });

    // No split in the standard regex crate — use the regex-split feature or
    // find_iter manually
}
```

#### Named Groups

```rust
let re = Regex::new(r"(?P<year>\d{4})-(?P<month>\d{2})").unwrap();
if let Some(caps) = re.captures("2024-12") {
    println!("Year: {}", caps.name("year").unwrap().as_str());
    println!("Month: {}", caps.name("month").unwrap().as_str());
}
```

#### RegexSet

The `RegexSet` feature allows matching against multiple patterns in a single pass. This is useful for classification tasks.

```rust
use regex::RegexSet;

let set = RegexSet::new(&[
    r"alice",
    r"bob",
    r"\d{4}",
]).unwrap();

let matches = set.matches("alice was born in 1995");
println!("Matched patterns: {:?}", matches.matched_indices().collect::<Vec<_>>());
// [0, 2] — patterns 0 and 2 matched
```

#### Safety and Performance

Rust's regex crate has no `unsafe` code in its matching engine. It uses a lazy DFA approach that combines the speed of DFA (deterministic finite automaton) with the memory efficiency of NFA (nondeterministic finite automaton). The library is designed for zero-copy matching and works well in hot loops.

### Java: `java.util.regex`

Java's regex engine is a traditional NFA backtracking engine similar to PCRE. It supports possessive quantifiers and has robust Unicode support.

#### Pattern and Matcher

```java
import java.util.regex.Pattern;
import java.util.regex.Matcher;

String text = "Contact: alice@example.com or bob@test.org";

// Compile pattern
Pattern pattern = Pattern.compile("(\\w+)@(\\w+\\.\\w+)");
Matcher matcher = pattern.matcher(text);

// find() — similar to re.search()
while (matcher.find()) {
    System.out.println("Full: " + matcher.group(0));
    System.out.println("User: " + matcher.group(1));
    System.out.println("Domain: " + matcher.group(2));
}

// matches() — requires ENTIRE string to match (like re.fullmatch())
boolean entireMatch = Pattern.matches(".*@.*", text);  // true

// lookingAt() — matches at start of string (like re.match())
Matcher m2 = pattern.matcher(text);
boolean startsWith = m2.lookingAt();  // true if text starts with an email
```

#### Flags

```java
Pattern p1 = Pattern.compile("hello", Pattern.CASE_INSENSITIVE);
Pattern p2 = Pattern.compile("^hello", Pattern.MULTILINE);
Pattern p3 = Pattern.compile("hello.*world", Pattern.DOTALL);
Pattern p4 = Pattern.compile(
    "\\b\\d{3}[-.\\s]?\\d{4}\\b",
    Pattern.COMMENTS  // equivalent to verbose
);
```

Java does not use `\A` and `\Z` anchors. Instead, `^` and `$` behave according to the `MULTILINE` flag: without it, `^` matches start of the entire string; with it, `^` matches start of every line.

#### Named Groups

```java
Pattern pattern = Pattern.compile("(?<year>\\d{4})-(?<month>\\d{2})");
Matcher matcher = pattern.matcher("2024-12-25");
if (matcher.find()) {
    System.out.println(matcher.group("year"));   // 2024
    System.out.println(matcher.group("month"));  // 12

    // Named backreference: \k<name>
    Pattern dup = Pattern.compile("(?<word>\\b\\w+\\b)\\s+\\k<word>");
    System.out.println(dup.matcher("hello hello").find());  // true
}
```

#### Possessive Quantifiers

Java supports possessive quantifiers (`*+`, `++`, `?+`, `{n,m}+`), which refuse to give back characters during backtracking. This is a defense against catastrophic backtracking.

```java
// Possessive: never backtracks
Pattern p1 = Pattern.compile("\".*+\"");
// .*+ consumes everything to the end, then " fails — but possessive
// won't give back, so entire match fails even if a closing quote exists
Matcher m1 = p1.matcher("\"hello\"");
System.out.println(m1.find());  // false — possessive consumed the closing quote

// Greedy (default): backtracks
Pattern p2 = Pattern.compile("\".*\"");
Matcher m2 = p2.matcher("\"hello\"");
System.out.println(m2.find());  // true — backtracking gives back the closing quote
```

#### Matcher Methods for Iteration

```java
// reset() — reuse matcher on new text
matcher.reset("alice@new.org");
matcher.find();

// replaceAll, replaceFirst
String result = Pattern.compile("\\w+@\\w+\\.\\w+")
    .matcher(text)
    .replaceAll("[REDACTED]");

// appendReplacement / appendTail for complex replacements
StringBuffer sb = new StringBuffer();
Matcher m = Pattern.compile("(\\w+)@(\\w+\\.\\w+)").matcher(text);
while (m.find()) {
    m.appendReplacement(sb, m.group(1) + "@***");
}
m.appendTail(sb);
System.out.println(sb.toString());
```

### PCRE: Perl Compatible Regular Expressions

PCRE is the reference backtracking NFA engine. It is used by PHP, R, the MySQL `REGEXP` operator, Notepad++, and many other tools. PCRE2 is the current version (PCRE was the original).

#### PHP Functions

```php
<?php
$text = "Contact: alice@example.com or bob@test.org";

// preg_match — returns 1 if match found (unlike preg_match_all)
$pattern = '/(\w+)@(\w+\.\w+)/';
if (preg_match($pattern, $text, $matches)) {
    echo $matches[0];  // alice@example.com
    echo $matches[1];  // alice
}

// preg_match_all — all matches
preg_match_all($pattern, $text, $matches, PREG_SET_ORDER);
foreach ($matches as $m) {
    echo $m[1] . "@" . $m[2] . "\n";
}

// preg_replace
$result = preg_replace($pattern, '[REDACTED]', $text);

// preg_replace with callback
$result = preg_replace_callback($pattern, function($m) {
    return substr($m[1], 0, 1) . "***@" . $m[2];
}, $text);

// preg_split
$parts = preg_split('/[,.\s]+/', "apple, banana.orange  grape");
print_r($parts);
?>
```

#### PCRE-Only Features

PCRE has the richest feature set of any commonly used regex engine.

```php
// Atomic groups: (?>...) — no backtracking into the group
preg_match('/(?>\d+)\w/', '123a');  // fails — atomic

// Possessive quantifiers: *+ ++ ?+ {n,m}+
preg_match('/".*+"/', '"hello"');  // fails — same as Java example

// \K — reset the match start (like a variable-length lookbehind workaround)
preg_match('/foo\Kbar/', 'foobar', $m);
echo $m[0];  // "bar" — "foo" is consumed but not included in the match

// Recursive patterns: (?R) — match the entire pattern recursively
preg_match('/\(([^()]+|(?R))*\)/', '((a)(b))', $m);
// Matches nested parentheses

// Subroutines: (?1) — match the first capture group again
preg_match('/(\w+) and (?1)/', 'hello and world', $m);
// (?1) reuses the subpattern from group 1

// Branch reset: (?|...|...) — reset group numbering in alternatives
preg_match('/(?|(a)|(b))/', 'b', $m);
echo $m[1];  // "b" — group 1 in both alternatives

// Callouts: (?C) — call external code during matching (engine-specific)
```

PHP patterns require delimiters (any non-alphanumeric character). Common choices are `/`, `~` (useful when the pattern contains `/`), and `#`.

### Command-Line Tools: grep, sed, awk

Command-line tools operate line-by-line by default. Patterns do not cross newline boundaries unless explicitly handled.

#### grep

```bash
# Basic regex (BRE) — default
grep 'hello.*world' file.txt

# Extended regex (ERE) with -E
grep -E '[0-9]{3}-[0-9]{4}' file.txt

# PCRE with -P (if available — GNU grep compiled with PCRE)
grep -P '\d{3}-\d{4}' file.txt

# Only matching text, not the whole line
grep -oE '[a-z]+@[a-z]+\.[a-z]+' file.txt

# Count matches per file
grep -c 'error' *.log

# Invert match
grep -v '^#' config.ini  # show non-comment lines

# Recursive search
grep -r 'TODO' src/
```

Note that `grep` returns the matching line, not the matching text. Use `-o` to extract only the match. By default, grep uses basic regular expressions (BRE) where `(`, `)`, `{`, `}`, `+`, `?`, `|` must be escaped to have special meaning. The `-E` flag switches to ERE where these metacharacters work as expected.

#### sed

```bash
# Substitution: s/pattern/replacement/flags
sed 's/foo/bar/' file.txt              # replace first occurrence per line
sed 's/foo/bar/g' file.txt             # replace all occurrences per line
sed 's/^[[:space:]]*//' file.txt       # trim leading whitespace
sed -E 's/([0-9]{3})-([0-9]{4})/\1\2/g' file.txt  # extended regex with -E

# In-place editing (BSD sed requires empty backup extension)
sed -i.bak 's/foo/bar/g' file.txt      # GNU sed
sed -i '' 's/foo/bar/g' file.txt       # BSD sed (macOS)

# Address range before substitution
sed '2,5s/foo/bar/' file.txt           # only lines 2 through 5
sed '/^DEBUG/,/^END/s/foo/bar/' file.txt  # between two markers

# Print only matching lines
sed -n '/error/p' file.txt             # -n suppresses default print, /p prints
```

The `-E` flag enables ERE in sed (GNU sed and modern BSD sed). Older sed implementations use `-r` for ERE.

#### awk

awk has its own regex syntax that is closest to ERE. Patterns are applied line-by-line as a filter.

```bash
# Print lines matching a pattern
awk '/error/ {print}' file.txt

# Using the match() function with groups
awk 'match($0, /([a-z]+)@([a-z]+\.[a-z]+)/, m) {print m[1], m[2]}' file.txt

# Using ~ operator
awk '$1 ~ /^[0-9]+$/ {print $0}' file.txt  # lines where first field is all digits

# sub and gsub for substitution
awk '{gsub(/[0-9]+/, "NUMBER"); print}' file.txt

# Field separator as regex
awk -F '[,.\s]+' '{print $1, $2, $3}' file.txt

# Using regex in conditions
awk '$3 ~ /admin|root/ {count++} END {print count}' /etc/passwd
```

### Feature Comparison Table

| Feature | Python | JavaScript | Go (RE2) | Rust (RE2) | Java | PCRE |
|---------|--------|-----------|----------|------------|------|------|
| Engine type | Backtracking NFA | Backtracking NFA | Automata (linear) | Automata (linear) | Backtracking NFA | Backtracking NFA |
| Greedy/lazy quantifiers | Yes | Yes | Yes | Yes | Yes | Yes |
| Possessive quantifiers | Yes | No | N/A | N/A | Yes | Yes |
| Atomic groups `(?>...)` | Yes | No | No | No | Yes | Yes |
| Backreferences | Yes | Yes | No | No | Yes | Yes |
| Named groups | `(?P<name>)` | `(?<name>)` | `(?P<name>)` | `(?P<name>)` | `(?<name>)` | `(?<name>)` |
| Named backref | `(?P=name)` | `\k<name>` | No | No | `\k<name>` | `\k<name>` |
| Lookahead | Yes | Yes | No | No | Yes | Yes |
| Lookbehind | Yes | Yes (ES2018) | No | No | Yes | Yes |
| Recursive patterns | No | No | No | No | No | Yes |
| Subroutines `(?1)` | No | No | No | No | No | Yes |
| `\K` (reset match) | No | No | No | No | No | Yes |
| `u` (Unicode) flag | Via `(?u)` | `u` flag | Yes (default) | Yes (default) | Yes (default) | Yes (default) |
| DOTALL flag | `re.DOTALL` / `(?s)` | `s` flag | N/A | N/A | `Pattern.DOTALL` / `(?s)` | `s` flag |
| Multiline flag | `re.MULTILINE` / `(?m)` | `m` flag | Via `(?m)` | Via `(?m)` | `Pattern.MULTILINE` / `(?m)` | `m` flag |
| Global matching | `findall`/`finditer` | `g` flag | `FindAll*` methods | `find_iter` | `while(find())` | `preg_match_all` |
| Inline flags | `(?imxs)` | `(?ims)` | `(?ims)` | `(?ims)` | `(?imxs)` | `(?imxs)` |
| Thread safe | Yes | Yes (stateless) | Yes | Yes | Yes | No (global state) |
| ReDoS protection | No | No | Built-in | Built-in | Partial (possessive) | No |
| Minimum version | N/A | ES3 (ES2018 for modern) | Go 1.0 | 1.0 | Java 1.4 | PCRE 1.0 |

### Porting Patterns Between Languages

#### JavaScript to Python

| JS Pattern | Python Equivalent | Notes |
|------------|-------------------|-------|
| `/pattern/g` | `re.findall(r'pattern')` | Global flag means find all |
| `/pattern/i` | `re.search(r'pattern', flags=re.I)` | Case insensitive |
| `str.match(/pattern/)` | `re.search(r'pattern', str)` | `match` in JS searches anywhere; Python's `re.match` does not |
| `str.replace(/pattern/g, repl)` | `re.sub(r'pattern', repl, str)` | |
| `(?<name>...)` | `(?P<name>...)` | Different named group syntax |
| `\k<name>` | `(?P=name)` | Named backreference syntax differs |
| `$&` in replacement | `\g<0>` | Full match in replacement |
| `$1` in replacement | `\1` or `\g<1>` | Group reference in replacement |

#### Python to JavaScript

| Python | JavaScript |
|--------|-----------|
| `re.search(p, s)` | `s.match(/p/)` or `s.search()` but returns index |
| `re.match(p, s)` | `/^p/.test(s)` — anchor at start |
| `re.fullmatch(p, s)` | `/^p$/.test(s)` — anchor both ends |
| `re.findall(p, s)` | `s.match(/p/g)` |
| `re.finditer(p, s)` | `s.matchAll(/p/g)` |
| `re.sub(p, r, s)` | `s.replace(/p/g, r)` |
| `(?P<name>...)` | `(?<name>...)` |
| `(?P=name)` | `\k<name>` |

#### RE2 Patterns to Backtracking Engines

When moving a pattern from Go or Rust to Python or JavaScript, check if you are relying on the absence of backtracking to avoid catastrophic behavior. The same pattern may be slow or crash in a backtracking engine.

Conversely, moving a pattern from Python/JavaScript to Go/Rust may fail at compile time if the pattern uses backreferences, lookahead, or lookbehind. Rewrite these patterns by restructuring the text processing logic (e.g., use two passes, or use `strings.Split` in Go before matching).

#### RE2 Workarounds for Lookahead and Backreferences

RE2 does not support lookahead or backreferences. The typical workaround is to match more broadly and filter programmatically.

```go
// Python: r'\b\w+\b(?=\s+Smith)'  — RE2: no lookahead, so:
re := regexp.MustCompile(`\b\w+\b`)
for _, match := range re.FindAllString(text, -1) {
    // Check context programmatically
}

// Python: (r'(\b\w+\b) \1')       — RE2: no backreferences, so:
words := regexp.MustCompile(`\b\w+\b`).FindAllString(text, -1)
for i := 1; i < len(words); i++ {
    if words[i] == words[i-1] {
        fmt.Printf("Duplicate: %s\n", words[i])
    }
}
```

## Glossary

| Term | Definition |
|------|------------|
| Atomic group | A group `(?>...)` that, once matched, does not allow backtracking into its contents |
| Backreference | A reference to a previously captured group within the same pattern (e.g., `\1`, `\k<name>`) |
| Backtracking | The process by which an NFA engine tries different alternatives when a match fails, going back to previous decision points |
| BRE | Basic Regular Expressions — the default mode in grep where most metacharacters must be escaped |
| DFA | Deterministic Finite Automaton — an engine that processes each input character once, guaranteeing linear time |
| ERE | Extended Regular Expressions — a mode where metacharacters like `+`, `?`, `|` do not need escaping |
| Lazy DFA | Rust's approach combining DFA speed with NFA memory efficiency by building DFA states on demand |
| NFA | Nondeterministic Finite Automaton — an engine that tries alternatives and backtracks on failure |
| Possessive quantifier | A quantifier (`*+`, `++`, `?+`) that does not give back characters during backtracking |
| RE2 | Google's regex library that guarantees linear-time matching by omitting backtracking-dependent features |
| Recursive pattern | A pattern that can match itself recursively, used for nested structures like parentheses |
| Subroutine | A syntax `(?1)` that invokes the subpattern of a capture group numbered 1, without capturing again |
| `\K` | A PCRE escape that resets the start of the match, so everything matched before `\K` is not included in the result |

## Quick References

- [Python `re` module documentation](https://docs.python.org/3/library/re.html) — official reference for all functions, flags, and syntax
- [MDN Regular Expressions guide](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions) — JavaScript regex reference with compatibility tables
- [Go `regexp` package documentation](https://pkg.go.dev/regexp) — official Go regexp reference
- [Rust `regex` crate documentation](https://docs.rs/regex/) — API docs for the regex crate
- [Java `Pattern` class documentation](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/regex/Pattern.html) — Java regex API reference
- [PCRE2 documentation](https://www.pcre.org/current/doc/html/) — official PCRE2 man pages
- [RE2 syntax reference](https://github.com/google/re2/wiki/Syntax) — what RE2 supports and rejects
- [Regular-Expressions.info](https://www.regular-expressions.info/) — comprehensive tutorial and comparison across languages

## Next Steps

- [Real-World Regex Patterns](real-world-regex-patterns.md) — practical patterns for common validation and text-processing tasks
