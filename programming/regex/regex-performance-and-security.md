# Regex Performance & Security

## Description

How to write regex that is fast, predictable, and safe against denial-of-service attacks. Covers measuring performance, avoiding common pitfalls, optimizing patterns, preventing ReDoS, and building a testing strategy for production regex.

## Prerequisites

- [Regex Engine Internals](regex-engine-internals.md) — how backtracking works, NFA vs DFA, and catastrophic backtracking

## Table of Contents

- [Measuring Regex Performance](#measuring-regex-performance)
- [Common Performance Pitfalls](#common-performance-pitfalls)
- [Optimization Techniques](#optimization-techniques)
- [ReDoS: Regular Expression Denial of Service](#redos-regular-expression-denial-of-service)
- [Defensive Regex Programming](#defensive-regex-programming)
- [Writing Regex Tests](#writing-regex-tests)
- [Study Cases](#study-cases)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Measuring Regex Performance

Before optimizing a regex, you need to measure. Performance metrics that matter:

- **Steps per match** — how many operations the engine performs
- **Backtracking count** — how many times the engine re-tries alternatives
- **Match time** — wall-clock time for a given input
- **Scaling behavior** — how match time grows with input length

**Using regex101.com:**

The regex101 debugger shows the number of steps a match takes. Compare these two patterns matching against "aaaaaaaaaaaaac":

| Pattern | Steps | Time |
|---------|-------|------|
| `^[a-z]+c$` | 15 | Instant |
| `(a|aa|aaa)*c` | 34 | Instant |
| `(a|aa|aaa)*b` | 1,287 | Slow |
| `(a+)+b` | 1,024 | Slow |

The last two patterns show high step counts because the engine tries many combinations before concluding failure. The step count grows exponentially with input length.

**Microbenchmarking in Python:**

```python
import re
import time

def benchmark(pattern, inputs, iterations=1000):
    compiled = re.compile(pattern)
    for label, test_input in inputs:
        start = time.perf_counter()
        for _ in range(iterations):
            compiled.match(test_input)
        elapsed = time.perf_counter() - start
        print(f"{label}: {elapsed/iterations*1e6:.2f}µs per match")

patterns = [
    (r'^[a-z]+c$', "Safe pattern"),
    (r'(a|aa|aaa)*b', "Catastrophic pattern"),
]

inputs = [
    ("Short match", "aaaac"),
    ("Short fail", "aaaab"),
    ("Long fail", "a" * 30 + "b"),
]

for pattern, label in patterns:
    print(f"\n{label}: {pattern}")
    for input_label, test_input in inputs:
        compiled = re.compile(pattern)
        start = time.perf_counter()
        compiled.match(test_input)
        elapsed = time.perf_counter() - start
        print(f"  {input_label}: {elapsed*1e6:.2f}µs")
```

The safe pattern shows constant time regardless of input length. The catastrophic pattern shows time increasing by a factor of ~2 with each additional character.

**Interpreting benchmarks:**

- If time stays flat as input grows: the pattern is linear or better
- If time grows linearly with input: O(n) — acceptable
- If time grows quadratically: O(n²) — might be a problem at scale
- If time doubles with each character: O(2ⁿ) — catastrophic, must fix

| Scaling | 10 chars | 20 chars | 30 chars | Verdict |
|---------|----------|----------|----------|---------|
| O(n) | 1µs | 2µs | 3µs | Safe |
| O(n²) | 1µs | 4µs | 9µs | Watch |
| O(2ⁿ) | 1µs | 1ms | 1s | Critical |

### Common Performance Pitfalls

**Nested quantifiers:**

The most common source of catastrophic backtracking. A quantifier applied to a group that itself contains a quantifier.

```
Bad:  (a+)+b
Bad:  (x*x*)*y
Bad:  (\d+\s*)+$
Good: a+b
Good: \d+\s*$
```

The pattern `(a+)+b` has an outer `+` around an inner `+`. For input "aaaaac", the engine must partition the a's into groups, leading to O(2ⁿ) time. The fix is almost always to remove the outer quantifier — it is redundant.

**Alternation with common prefix:**

```
Bad:  (abcd|abce|abcf)
Good: abc(d|e|f)
Bad:  (intro|intranet|internal)
Good: int(r(o|ranet)|ernal)
```

When alternatives share a prefix, the engine tries each alternative separately, re-matching the common prefix every time. Factor out the common prefix so the engine matches it once.

The performance difference is dramatic for long inputs:

```python
import re

# Bad: 5 alternatives, each starting with "abcdefghij"
bad = re.compile(r'(abcdefghij_xxxx|abcdefghij_yyyy|abcdefghij_zzzz|abcdefghij_wwww|abcdefghij_vvvv)')

# Good: common prefix factored out
good = re.compile(r'abcdefghij_(xxxx|yyyy|zzzz|wwww|vvvv)')

test = "abcdefghij_zzzz"

# Both match, but 'bad' re-matches the prefix 5 times in worst case
```

**Unnecessary backtracking from greedy quantifiers:**

```
Pattern: <html>.*
Input:  "<html>content</html>"
```

The `.*` matches everything to the end, then backtracks. If the pattern only needs "<html>", use a non-greedy or more specific quantifier:

```
Good: <html>.*?</html>   (lazy)
Good: <html>[^<]*        (specific)
```

Greedy quantifiers cause backtracking proportional to how much they over-consume. The fix is to be as specific as possible about what the quantifier should match.

**Using dot `.` when a character class suffices:**

```
Bad:  .+@.+
Good: [^\s@]+@[^\s@]+
```

Dot matches everything (except newline in most engines). This means the engine has more backtracking possibilities. A specific character class limits the search space and fails faster on invalid input.

**Not anchoring the pattern:**

```
Bad:  \d{5}
Good: ^\d{5}$
```

Without anchors, the engine tries the match at every position in the input. For a 1000-character string, `\d{5}` is attempted up to 996 times before failing. With `^` and `$`, it is tried once.

**Unnecessary capture groups:**

```
Bad:  (\d+)-(\d+)-(\d+)
Good: \d+-\d+-\d+
```

If you do not need the captured values, use non-capturing groups `(?:...)` or no groups at all. Capturing groups add bookkeeping overhead.

### Optimization Techniques

**Unrolling the loop:**

For repeated patterns, unrolling eliminates backtracking. The classic example is matching quoted strings:

```
Naive:  ".*?"
Unrolled: "[^"]*"
```

The naive pattern uses lazy `.*?` to match characters inside quotes. Each expansion of `.*?` is a backtrack step. The unrolled version uses `[^"]*` which stops exactly at the closing quote without backtracking.

Compare steps for `".*?"` vs `"[^"]*"` against a 100-character string:

| Pattern | Steps | Explanation |
|---------|-------|-------------|
| `".*?"` | ~200 | Each character may require backtrack expansion |
| `"[^"]*"` | ~102 | One step per character, then done |

For multi-character delimiters, match everything that is not the delimiter, then match the delimiter. A C-style comment unrolled: `/\*[^*]*\*+(?:[^/*][^*]*\*+)*/`.

**Atomic groups to prevent backtracking:**

Atomic groups `(?>...)` discard internal choice points once the group exits. `(?>(a|aa|aaa))*b` prevents the engine from backtracking into the alternation — if it matched "aaa", that choice is final. Supported in PCRE, Java, .NET, Ruby, and Python's `regex` module.

**Specific character classes over dot:** `(.+)` against `<div>text</div>` matches "div>text</div" then backtracks. `([^>]+)` stops at the first `>` — no backtracking.

**Anchoring patterns:** `^` and `$` prevent the engine from trying every position. `re.match` anchors to start in Python, but `re.search` and most other languages do not. Always anchor when validating whole strings.

**Fail-fast ordering:** Order alternatives by likelihood. `user|admin|guest|administrator` stops at the first match. In traditional NFA, match frequency matters; in POSIX NFA, match length matters.

**Using non-capturing groups:** `(?:...)` saves the engine from recording submatch positions. If you do not need captured values, prefer non-capturing groups or no groups at all.

### ReDoS: Regular Expression Denial of Service

ReDoS is an attack where a crafted input triggers catastrophic backtracking, consuming excessive CPU and potentially freezing the application.

**How ReDoS works:**

1. The developer writes a regex with nested quantifiers or overlapping alternatives
2. An attacker sends input that almost matches but fails at the last character
3. The regex engine explores exponential combinations before reporting failure
4. The server thread is blocked for seconds or minutes
5. With many concurrent requests, the server becomes unresponsive

**Evil regex characteristics:**

```
Characteristic               Example                Complexity
─────────────────────────────────────────────────────────────
Nested quantifiers           (a+)+$                 O(2ⁿ)
Overlapping alternatives     (a|aa|aaa)+$           O(2ⁿ)
Quantified overlapping groups (x+x+)+y              O(2ⁿ)
Long literal prefix + .*     prefix.*suffix         O(n²) if suffix is absent
Alternation inside *         (\s*,\s*)*             O(n²)
```

**CVE examples:**

| CVE | Library | Pattern | Impact |
|-----|---------|---------|--------|
| CVE-2019-11358 | jQuery | `<[a-z][^>]*\s+h[^>]*\n` | 1 line of HTML could freeze the page |
| CVE-2018-20163 | `trim` npm | `(a+)+b` | 50KB of input = 30s CPU |
| CVE-2017-16099 | `moment.js` | `/[\s\S]*/` patterns | Date parsing freeze |
| CVE-2007-4785 | PHP PCRE | `(a+)+c` | PHP crash with 100+ character input |
| CVE-2022-36067 | `vm2` sandbox | Nested quantifiers | Sandbox escape via ReDoS |

**Cloudflare outage (July 2019):** Cloudflare's WAF had a SQL-injection-detection regex with nested quantifiers. A crafted HTTP request triggered exponential backtracking across all edge servers, taking down millions of websites for ~30 minutes.

**Practical attack:** An email validator `[a-zA-Z0-9._%+-]+@...` against 10,000 a's without `@` causes 10,000 backtrack steps. Not exponential, but with slightly more complex patterns it becomes catastrophic. Always test with a scaling benchmark:

```python
import re, time

def test_scaling(pattern, max_len=30):
    for n in range(1, max_len + 1):
        start = time.perf_counter()
        re.match(pattern, 'a' * n + 'b')
        elapsed = time.perf_counter() - start
        if elapsed > 1:
            print(f"⚠ ReDoS at n={n}: {elapsed:.2f}s")
            break

test_scaling(r'(a|aa|aaa)+b')  # Exponential — doubles per char
```

### Defensive Regex Programming

**Timeout limits:**

Regex engines with built-in timeout support: .NET `Regex(pattern, opts, TimeSpan)`, Python `regex` module (`timeout` parameter), and Java via `ExecutorService` wrappers. Python's built-in `re` module lacks timeouts — use `signal.SIGALRM` or RE2 instead. Go and Rust guarantee linear time, so timeouts are unnecessary.

**Input length limits:**

Before running a regex, check the input length. This is the simplest defense against ReDoS.

```python
import re

MAX_INPUT_LENGTH = 1024

def safe_search(pattern, text):
    if len(text) > MAX_INPUT_LENGTH:
        raise ValueError(f"Input too long: {len(text)} > {MAX_INPUT_LENGTH}")
    return re.search(pattern, text)
```

For patterns with exponential complexity, even 30-50 characters can cause problems. Set limits conservatively.

**Using non-backtracking engines:**

| Engine | Language | Guarantee | Backrefs |
|--------|----------|-----------|----------|
| RE2 | C++, Python, Go | O(n) | No |
| Go `regexp` | Go | O(n) (w/o backrefs) | Falls back |
| Rust `regex` | Rust | O(n) (w/o backrefs) | Falls back |

```python
import re2
re2.compile(r'(a|aa|aaa)*b').match("a" * 100)  # Always fast
```

**Regex linting tools:**

Static analysis detects vulnerable patterns before production. `regexploit` (Python) scans for exponential/polynomial complexity. `rxxr2` (Ruby) does the same. regex101.com's built-in linter highlights nested quantifiers and overlapping alternatives in red.

**Static analysis rules:**

- Flag `+` or `*` inside a group that also has `+` or `*`
- Flag alternation with shared prefix longer than 3 characters
- Flag `.*` followed by a required substring that may be absent
- Flag unanchored patterns that could be anchored

### Writing Regex Tests

**Test categories:**

Every regex in production should have tests covering:

| Category | Purpose | Example inputs |
|----------|---------|----------------|
| Valid matches | Verify correct matches | "user@example.com" |
| Valid non-matches | Verify correct rejection | "not-an-email" |
| Edge cases | Boundaries and special values | "", "@", "a@b" |
| Performance | Linear scaling | Long valid strings |
| ReDoS | No catastrophic backtracking | "aaaa...ab" |

**Test structure:**

Tests should cover valid matches, invalid inputs, edge cases (empty string, unicode, boundary lengths), and performance scaling. Example in Python with `unittest`:

```python
import re, time, unittest

class TestEmailRegex(unittest.TestCase):
    PATTERN = r'^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$'

    def setUp(self):
        self.re = re.compile(self.PATTERN)

    def test_basic(self):
        self.assertIsNotNone(self.re.match("user@example.com"))

    def test_invalid(self):
        self.assertIsNone(self.re.match("not-an-email"))

    def test_empty(self):
        self.assertIsNone(self.re.match(""))

    def test_performance_linear(self):
        for length in [100, 200, 400, 800]:
            start = time.perf_counter()
            self.re.match("a" * length + "@b.c")
            elapsed = time.perf_counter() - start
            self.assertLess(elapsed, length * 0.001)  # 1µs per char max
```

**Performance regression tests** verify that time does not grow exponentially with input length. Run against incrementally longer inputs and assert linear scaling.

**Testing in CI:**

Add performance tests to your CI pipeline. Run each regex against incrementally longer inputs and fail the build if time grows faster than linear.

Integration with CI: add `regexploit --check patterns.txt` and performance regression tests to your pipeline.

## Glossary

| Term | Definition |
|------|------------|
| Steps per match | The number of individual operations a regex engine performs during a match attempt |
| Backtracking count | The number of times the engine re-tries alternatives after a match failure |
| Nested quantifiers | A quantifier applied to a group that also contains a quantifier (e.g., `(a+)+`), the leading cause of catastrophic backtracking |
| Common prefix | A shared starting substring in alternation branches (e.g., `abcd` and `abce` share `abc`) |
| Anchoring | Using `^` and `$` to constrain where a match can start and end |
| Unrolling the loop | A technique that replaces a lazy quantifier with a specific character class to eliminate backtracking |
| Atomic group | A group `(?>...)` whose internal choice points are discarded on exit, preventing backtracking |
| Possessive quantifier | A quantifier (`*+`, `++`, `?+`) that matches greedily and never backtracks |
| ReDoS | Regular Expression Denial of Service — an attack exploiting catastrophic backtracking to consume excessive CPU |
| Evil regex | A regex pattern with exponential or polynomial worst-case complexity, vulnerable to ReDoS |
| Evil input | Input crafted specifically to trigger a regex's worst-case behavior |
| Timeout | A mechanism that aborts regex matching when a time limit is exceeded |
| Fail-fast ordering | Placing the most likely alternatives first in alternation to minimize backtracking |
| Fuzz testing | Automated testing with random or semi-random inputs to discover unexpected behavior |
| Performance regression | A change that causes a previously fast regex to become slow |
| Boyer-Moore | A fast string search algorithm using bad-character and good-suffix heuristics to skip characters |
| Static analysis | Automated analysis of code or patterns to detect potential vulnerabilities without executing them |
| Exponential time | Time complexity O(cⁿ) where each additional input character doubles or triples the execution time |
| Polynomial time | Time complexity O(nᵏ) where execution time grows as a fixed power of input length |
| Linear time | Time complexity O(n) where execution time grows proportionally to input length |

## Quick References

- [regex101.com](https://regex101.com) — online regex tester with debugger, performance stats, and linting
- [regexploit](https://github.com/doyensec/regexploit) — Python tool to detect ReDoS-vulnerable patterns
- [rxxr2](https://github.com/reiase/rxxr2) — Ruby static analyzer for regex complexity
- [OWASP ReDoS Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Regular_Expression_Denial_of_Service_Cheat_Sheet.html) — prevention guidelines
- [RE2 GitHub](https://github.com/google/re2) — Google's linear-time regex library
- [Go regexp documentation](https://pkg.go.dev/regexp) — Go's built-in linear-time regex engine
- [Rust regex crate](https://docs.rs/regex/) — Rust's hybrid DFA/backtracking regex engine
- [Cloudflare outage postmortem](https://blog.cloudflare.com/details-of-the-cloudflare-outage-on-july-2-2019/) — ReDoS in the wild
- [Regular-Expressions.info — Performance](https://www.regular-expressions.info/catastrophic.html) — catastrophic backtracking explained
- [Microsoft — .NET Regex Timeouts](https://docs.microsoft.com/en-us/dotnet/standard/base-types/best-practices-regex) — timeout best practices in .NET

## Next Steps

- [Regex in Programming Languages](regex-in-programming-languages.md) — language-specific regex APIs, differences, and best practices for Perl, Python, JavaScript, Java, Go, Rust, and C#
