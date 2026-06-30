# Regex Engine Internals

## Description

How regex engines actually work under the hood — the difference between NFA and DFA, how backtracking works, why some patterns are catastrophically slow, and what modern engines do about it. Understanding internals is crucial for writing predictable, performant patterns and diagnosing slowdowns.

## Prerequisites

- [Lookahead & Lookbehind Assertions](lookahead-and-lookbehind.md) — zero-width assertions and how they affect engine behavior

## Table of Contents

- [Finite Automata: The Theoretical Foundation](#finite-automata-the-theoretical-foundation)
- [DFA Engines](#dfa-engines)
- [NFA Engines](#nfa-engines)
- [Traditional NFA vs POSIX NFA](#traditional-nfa-vs-posix-nfa)
- [The Backtracking Algorithm](#the-backtracking-algorithm)
- [Greedy, Lazy, and Possessive Quantifiers](#greedy-lazy-and-possessive-quantifiers)
- [Backtracking in Groups and Alternation](#backtracking-in-groups-and-alternation)
- [Catastrophic Backtracking](#catastrophic-backtracking)
- [ReDoS: Regular Expression Denial of Service](#redos-regular-expression-denial-of-service)
- [Non-Backtracking Engines: RE2 and Go](#non-backtracking-engines-re2-and-go)
- [Optimization Techniques in Modern Engines](#optimization-techniques-in-modern-engines)
- [Debugging Regex Performance](#debugging-regex-performance)
- [Study Cases](#study-cases)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Finite Automata: The Theoretical Foundation

Every regex pattern corresponds to a finite automaton — a mathematical model of computation with a finite number of states and transitions between them. The theory originates from Stephen Kleene's 1956 work on regular languages and was formalized by Ken Thompson for the QED editor in 1968.

A finite automaton processes a string character by character. At each step, it reads one character and follows a transition to a new state. If it reaches an accepting state after consuming the entire input, the string matches.

Two types of finite automata are used in regex engines:

| Property | NFA | DFA |
|----------|-----|-----|
| States | Can be in multiple states at once | Single state at any time |
| Transitions | Multiple possible transitions for same input | Exactly one transition per input per state |
| Epsilon transitions | Allowed (move without consuming input) | Not allowed |
| Backtracking | Required | Not required |
| Time complexity | O(2^n) worst case | O(n) guaranteed |

Understanding these two models is the key to understanding regex performance.

### DFA Engines

A Deterministic Finite Automaton has exactly one transition from each state for each input character. This means the engine can process the input in a single left-to-right pass, maintaining only the current state.

**How DFA matching works:**

Consider the pattern `ab|ac`. A DFA for this pattern looks like:

```
state 0 --a--> state 1
state 1 --b--> state 2 (accept)
state 1 --c--> state 3 (accept)
```

For input "ac", the DFA goes: state 0 -> (read 'a') -> state 1 -> (read 'c') -> state 3 (accept). No backtracking, no bookkeeping.

**Properties:**

- Runs in linear time O(n) where n is the input length
- Every character is examined exactly once
- No backtracking means no exponential blowup
- Memory usage can be large (exponential number of states in worst case)
- Limited feature set: cannot support backreferences, lookahead, lookbehind, or capturing groups

**Engines that use DFA or DFA-like approaches:**

- `awk` (some implementations)
- `egrep` (GNU grep -E)
- `lex` / `flex` (lexer generators compile to DFA)
- `RE2` (uses DFA when possible, falls back to NFA with no backtracking)

**What DFA cannot do:**

Because DFA must decide the next state deterministically, it cannot support features that require "remembering" a value and checking it later. A DFA cannot implement:

- Backreferences: `(a|b)\1` requires remembering which alternative matched
- Lookahead/lookbehind: `foo(?=bar)` requires checking ahead without consuming
- Capturing groups: the engine must record the position of submatch
- Possessive quantifiers: these are a backtracking control feature

For applications that only need basic pattern matching (no backreferences), a DFA engine is ideal. Tools like `grep -E`, `flex`, and `RE2` prove that most useful patterns can be matched in linear time.

### NFA Engines

A Nondeterministic Finite Automaton can have multiple transitions for the same input character and can move spontaneously via epsilon transitions. When an NFA reaches a choice point, it must explore multiple paths.

**How NFA matching works:**

Take the pattern `a(b|b)c`. The NFA has:

```
state 0 --a--> state 1
state 1 --epsilon--> state 2, state 3
state 2 --b--> state 4
state 3 --b--> state 5
state 4 --c--> state 6 (accept)
state 5 --c--> state 6 (accept)
```

When the NFA reaches state 1 after reading 'a', it has two epsilon transitions to states 2 and 3. Both paths must be explored.

Two strategies exist for exploring these paths:

1. **Simultaneous exploration** — track all possible states at once (BFS-like). This gives O(m * n) time where m is the number of states, but cannot support backreferences.
2. **Backtracking** — explore one path fully, then backtrack to try alternatives (DFS-like). This supports the full feature set but can be exponential.

Most programming language regex engines use the backtracking strategy because it naturally supports capturing groups, backreferences, and lookarounds.

**Engines using NFA with backtracking:**

- PCRE (PHP, R, Delphi, many others)
- Python `re` module
- JavaScript `RegExp`
- Java `java.util.regex`
- .NET `Regex`
- Ruby `Regexp`
- Perl (the original reference implementation)
- Rust `regex` crate (uses NFA with backtracking only for backreferences, otherwise uses DFA)

### Traditional NFA vs POSIX NFA

Two flavors of NFA engines differ in how they handle alternation:

**Traditional NFA (leftmost-first):**

The engine tries alternatives in the order they appear in the pattern. This is the default in Perl, PCRE, Python, JavaScript, Java, .NET, and Ruby.

```
Pattern: a|aa
Input: "aa"
Result: "a" (first alternative wins)
```

The engine matches 'a' at position 0 and stops — it never tries the second alternative because the first one already succeeded. This is the "leftmost-first" rule.

**POSIX NFA (leftmost-longest):**

The engine must find the longest match among all alternatives that starts at the earliest position. This is required by POSIX standards and used in `awk`, `grep -E` (with POSIX mode), and some Tcl versions.

```
Pattern: a|aa
Input: "aa"
Result: "aa" (the longest match wins)
```

The engine tries both alternatives at each position and keeps the longest match. The POSIX specification mandates that the match be the leftmost and longest possible.

**Practical impact:**

Traditional NFA gives the programmer control over which alternative matches through ordering. POSIX NFA is slower because it must explore all alternatives to find the longest match. Most real-world engines use the traditional approach.

### The Backtracking Algorithm

The backtracking algorithm used by traditional NFA engines is essentially a depth-first search with backtracking. Here is how it works at a high level:

```
function match(pattern, input, position):
    for each alternative at current position:
        attempt to match the rest
        if success: return the match
    return failure
```

When the engine encounters a quantifier like `a*` against "aaa", it must decide how many 'a' characters to consume. The decision process:

1. The engine tries to consume as many as possible (greedy default) or as few as possible (lazy mode)
2. It remembers the choice point in case it needs to backtrack
3. If the rest of the pattern fails, it returns to the choice point and tries the next option

**State bookkeeping:**

The engine maintains a stack of choice points. Each choice point records:

- The current position in the input
- The current position in the pattern
- Which alternative was tried
- Any captured group values

When a match attempt fails, the engine pops the most recent choice point and tries the next alternative. When all alternatives are exhausted at a given position, the engine advances the input start position by one and tries again.

Consider matching `a*b` against "aaac":

```
Start: input=0, pattern pos=0
  a* matches 'a' at 0 → choice point 1
  a* matches 'a' at 1 → choice point 2
  a* matches 'a' at 2 → choice point 3
  a* matches nothing (at position 3) → choice point 4
  Try b at position 3 → 'c' ≠ 'b' → FAIL
  Backtrack to choice point 3: a* matches 'aaa', try b at 3 → FAIL
  Backtrack to choice point 2: a* matches 'aa', try b at 2 → FAIL
  Backtrack to choice point 1: a* matches 'a', try b at 1 → FAIL
  Backtrack to start: a* matches nothing, try b at 0 → FAIL
No match found
```

The engine tried 5 different ways to match `a*` before concluding failure.

### Greedy, Lazy, and Possessive Quantifiers

Quantifiers are greedy by default — they match as much as possible and give back only when forced.

**Greedy `*`, `+`, `?`, `{n,m}`:**

```
Pattern: <.*>
Input: "<div>text</div>"
Match: "<div>text</div>" (full string)
```

The engine:

1. `.*` matches "<div>text</div>" (the entire input)
2. The pattern requires `>` after `.*`. It fails at end of input.
3. The engine backtracks: `.*` gives up one character at a time
4. When `.*` holds "<div>text</di", the next char is 'v' — not '>'. Backtrack.
5. Eventually `.*` holds "<div>text</div" and the next char is '>' — match!

The engine tries as many characters as possible for `.*`, then shortens it character by character until the rest of the pattern can match.

**Lazy `*?`, `+?`, `??`, `{n,m}?`:**

```
Pattern: <.*?>
Input: "<div>text</div>"
Match: "<div>" (shortest match)
```

The engine:

1. `.*?` matches nothing (lazy — as few as possible)
2. Try `>` — the next char is 'd'. Not '>'. Backtrack.
3. `.*?` expands to "<"
4. Try `>` — the next char is 'i'. Not '>'. Backtrack.
5. ... continues expanding until `.*?` holds "<div" and `>` matches.

The engine starts with the minimum and expands only when the rest of the pattern fails. This is the mirror of greedy matching.

**Visual comparison:**

| Input | Pattern | Greedy result | Lazy result |
|-------|---------|---------------|-------------|
| `"a1b2c"` | `\w+\d` | `"a1b2"` | `"a1"` |
| `"ab"` | `a??b` | `"ab"` | `"ab"` |

**Possessive `*+`, `++`, `?+`, `{n,m}+`:**

Possessive quantifiers match as much as possible and never give back — they do not create a choice point. If the rest of the pattern fails, the engine does not backtrack into the possessive quantifier.

```
Pattern: <.*+>
Input: "<div>text</div>"
```

The engine:

1. `.*+` matches "<div>text</div>" (the entire input)
2. The pattern requires `>`. It fails at end of input.
3. The engine does NOT backtrack into `.*+`. The match fails.

```
Pattern: <[^>]*+>
Input: "<div>"
```

The engine:

1. `[^>]*+` matches "<div" (all non-'>' characters)
2. `>` matches '>'. Success!

Possessive quantifiers are an optimization and a safety tool. They prevent backtracking, which eliminates the risk of catastrophic backtracking when the quantifier cannot yield a different result anyway.

**Atomic groups `(?>...)`:**

Atomic groups provide the same effect as possessive quantifiers but for arbitrary subpatterns. Once the engine exits an atomic group, all choice points inside the group are discarded.

```
Pattern: (?>a*+)b
Input: "aaa"
```

`(?>a*+)` matches "aaa". Then `b` fails. The engine does not backtrack into the atomic group. Match fails.

Atomic groups are supported in PCRE, Python (via `regex` module, not `re`), Java, .NET, and Ruby (Oniguruma). They are not supported in JavaScript or Python's `re` module.

### Backtracking in Groups and Alternation

**Capturing groups:**

Each capturing group records the text matched by its subpattern. When the engine backtracks into a group, the captured value is updated or restored.

```
Pattern: (a|ab)(c|bc)
Input: "abc"
```

The engine:

1. Try first branch: `(a|ab)` matches 'a' via first alternative. Capture group 1 = "a"
2. `(c|bc)` tries 'c' at position 1 → input is 'b'. Fail.
3. `(c|bc)` tries 'bc' at position 1 → input is 'bc'. Fail.
4. Backtrack into group 1: `(a|ab)` tries second alternative 'ab'. Capture group 1 = "ab"
5. `(c|bc)` tries 'c' at position 2 → 'c' matches. Capture group 2 = "c"
6. Match: group 1 = "ab", group 2 = "c"

The engine tried both branches of alternation inside group 1 because the first branch led to a dead end.

**Alternation backtracking:**

```
Pattern: (cat|car)
Input: "cart"
```

The engine:

1. `cat` matches "cat" at position 0. End of pattern. Match is "cat".
2. It never tries "car" because `cat` already matched.

If the pattern were `(car|cat)`, "car" would match instead since it appears first.

```
Pattern: (cat|car)go
Input: "cargo"
```

The engine:

1. `cat` matches "cat". Pattern continues: `go` at position 3 → input is "go". Match!
2. `car` never tried.

But with:

```
Pattern: (cat|car)t
Input: "cart"
```

The engine:

1. `cat` matches "cat". Pattern continues: `t` at position 3 → input is "r". Fail.
2. Backtrack. `car` matches "car". Pattern continues: `t` at position 3 → input is "t". Match!
3. The engine had to try both alternatives.

**Nested backtracking:**

When quantifiers and alternation are nested, the number of choice points multiplies.

```
Pattern: (a+|aa+)(b+|bb+)
Input: "aaab"
```

The engine tries (a+="aaa", b+="b"), then (a+="aaa", bb+="ab"? fails), then (a+="aa", aa+="a", b+="b"), and many more combinations. Each alternation and quantifier creates branching.

### Catastrophic Backtracking

Catastrophic backtracking occurs when the number of possible paths through the pattern grows exponentially with input length. The engine tries millions or billions of combinations, often taking minutes or hours for strings of moderate length (20-30 characters).

**The classic example:**

```
Pattern: (a|aa|aaa)*b
Input: "aaaaaaaaaaaaac" (13 a's, then c)
```

The engine must match `(a|aa|aaa)*` against the 13 a's, then match `b` against 'c'. Since 'c' does not match `b`, the engine backtracks and tries every possible way to partition the a's into groups of 1, 2, and 3.

How many ways are there? This is equivalent to the number of ways to write n as a sum of 1, 2, and 3, which grows roughly like O(1.84^n). For 13 a's, that is hundreds of attempts. For 30 a's, it is over 5 million. For 50 a's, over 10 trillion.

**Why it happens:**

Catastrophic backtracking requires:

1. A quantifier that can match overlapping substrings of the same input
2. Nested quantifiers or quantifiers applied to groups containing alternatives
3. A pattern that almost matches but fails near the end

The canonical form is:

```
(x+)+y
(x|y)*z
(x*x*)*y
```

Each of these has O(2^n) complexity because the quantifiers create an exponential number of ways to partition the input.

**Common patterns that cause catastrophic backtracking:**

| Pattern | Issue | Input |
|---------|-------|-------|
| `(a+)+b` | Nested quantifiers | "aaaa...ac" |
| `(a|aa)*b` | Alternation inside quantifier | "aaaa...ac" |
| `(.*)*` | Dot-star inside quantifier | "anything that doesn't match the rest" |
| `(x+x+)+y` | Multiple overlapping quantifiers | "xxxx...xz" |
| `(\d+\s*)+$` | Nested quantifiers in validation | Long string of digits with trailing whitespace that doesn't match |

**Detailed walkthrough of `(a+)+b` against "aaaaac":**

Inner `a+` consumes all 5 a's. `b` fails. The engine reduces inner `a+` to 4 a's, outer `(a+)+` tries another iteration consuming the last 'a'. `b` fails. Backtrack again: first iteration holds 4 a's, second holds nothing. `b` fails. Remove second iteration, reduce first to 3 a's, second iteration consumes 2 a's. `b` fails. And so on until every partition is exhausted.

The number of partitions: 16 ways for 5 a's, 512 for 10 a's, 524,288 for 20 a's, over 536 million for 30 a's.

**How to detect catastrophic backtracking:**

- The regex appears to "hang" or "freeze" on certain inputs
- CPU usage spikes to 100% for a single regex match
- Matching a slightly longer input takes exponentially longer
- Adding a single character increases match time by a factor of 2-3

### ReDoS: Regular Expression Denial of Service

ReDoS is a security vulnerability where an attacker crafts a string that triggers catastrophic backtracking in a regex, causing the application to consume excessive CPU time. This can freeze a server thread, enabling denial-of-service attacks with minimal bandwidth.

**How ReDoS works:**

The attacker identifies a vulnerable regex — one that has exponential worst-case complexity. They then send input strings that almost match the pattern but fail at the last character. The engine spends enormous time exploring all possible match paths before concluding failure.

**Real-world (CVEs):**

| CVE | Product | Pattern | Impact |
|-----|---------|---------|--------|
| CVE-2019-11358 | jQuery `$.html()` | `<[a-z][^>]*\s+h[^>]*\n` | XSS filter bypass + ReDoS |
| CVE-2015-9235 | Node.js `is-my-json-valid` | `(a|aa)+` | ReDoS in JSON validation |
| CVE-2017-16099 | `moment.js` | `/[\s\S]*/` inside certain patterns | Application freeze |
| CVE-2022-24761 | `relative-import` npm package | Nested quantifiers | ReDoS via crafted import string |
| CVE-2013-6414 | Ruby on Rails | `(a+)+b` in token validation | Auth token bypass via ReDoS |

**Cloudflare incident (2019):**

In July 2019, Cloudflare's WAF (Web Application Firewall) had a regex that caused a global outage. The pattern was designed to detect SQL injection but contained nested quantifiers:

```
(?:(?:\"|'|\]|\[|\\|\/| )*?...
```

Under certain inputs, the pattern's backtracking caused Cloudflare edge servers to consume 100% CPU, taking down a significant portion of the internet for about 30 minutes.

**Identifying evil regex patterns:**

A regex is likely vulnerable to ReDoS if:

- It contains nested quantifiers: `(x+)+`, `(x*x*)*`, `(x+)+$`
- It has alternation inside quantified groups: `(a|b)*`, `(x|xx)*`
- It uses `.*` or `.+` before a required pattern that might not match
- The match time grows polynomially (O(n^2)) or exponentially (O(2^n)) with input length

Characteristic | Severity | Example
---|---|---
Exponential backtracking | Critical | `(a+|aa+|aaa+)*b`, `(x+x+)+y`
Polynomial backtracking | High | `.*b.*b.*b.*b` (O(n^k) with k fixed patterns)
Quadratic backtracking | Medium | `(\s*,\s*)*` with alternating spaces and commas

### Non-Backtracking Engines: RE2 and Go

Some engines avoid catastrophic backtracking entirely by using non-backtracking algorithms. The most important are RE2 and Go's `regexp` package.

**RE2 (Google, C++):**

RE2 was created by Russ Cox (Google) to guarantee linear-time matching. It uses:

- DFA simulation for patterns without backreferences
- NFA with submatch tracking via automaton intersection
- No backtracking at all

RE2 guarantees O(n) time for any pattern that does not use backreferences. This comes at the cost of features:

```
RE2 limitations:
- No backreferences: (a|b)\1
- No lookahead/lookbehind: foo(?=bar)
- No possessive quantifiers: a++
- No atomic groups: (?>a+)
- No recursion: (?R)
- No balancing groups (.NET)
```

For the vast majority of real-world patterns (email validation, URL parsing, log parsing, input filtering), RE2 is sufficient and provides a security guarantee against ReDoS.

**Go's `regexp` package:**

Go uses the same algorithms as RE2 (the Go `regexp` package was written by Russ Cox). It guarantees linear time for all patterns without backreferences. When a pattern contains a backreference, Go falls back to backtracking and loses the time guarantee.

```go
package main

import "regexp"

func main() {
    // Guaranteed linear time
    safe := regexp.MustCompile(`(a|aa|aaa)*b`)

    // Falls back to backtracking — no guarantee
    risky := regexp.MustCompile(`(a|b)\1`)
}
```

**Rust's `regex` crate:**

Rust's regex crate takes a hybrid approach:

1. If the pattern has no backreferences, it compiles to a DFA (linear time)
2. If backreferences are present, it uses backtracking only for that portion
3. The DFA path guarantees O(m*n) time and exits quickly even for malicious input

```rust
use regex::Regex;

fn main() {
    // This is safe — DFA path
    let re = Regex::new(r"(a|aa|aaa)*b").unwrap();
    assert!(!re.is_match("aaaaaaaaaaaaac"));

    // Backreferences trigger backtracking — be careful
    let re2 = Regex::new(r"(a|b)\1").unwrap();
    assert!(re2.is_match("aa"));
}
```

**When to use non-backtracking engines:**

- User-supplied regex patterns (search functionality, log analysis)
- Regex applied to user-supplied input
- High-throughput systems where latency must be bounded
- Security-critical code paths (input validation, WAF)

For internal code with fixed patterns that you have tested, backtracking engines are usually fine. The key is understanding the risk and testing accordingly.

### Optimization Techniques in Modern Engines

Modern backtracking engines employ many optimizations to reduce or avoid worst-case behavior.

**Pre-checking the input:**

Before starting the full NFA simulation, the engine checks whether the input could possibly match. Common pre-checks:

- Minimum length check: if input is shorter than the minimum pattern length, reject immediately
- First character optimization: find the first literal character or character class and scan for it
- Required substring: extract literal substrings from the pattern and require them to exist

Perl's regex engine, for example, extracts the longest literal string from the pattern and uses a fast Boyer-Moore-like search to find candidate positions before starting the NFA.

**Boyer-Moore-like optimizations:**

The Boyer-Moore string search algorithm uses bad-character and good-suffix heuristics to skip characters. Many engines apply similar ideas to find literal prefixes or suffixes of patterns.

```
Pattern: "abc\w+xyz"
The engine extracts "abc" as a literal prefix and scans for it using fast substring search.
Only when "abc" is found does the NFA start matching.
```

**Caching and memoization:**

Some engines cache the results of subpattern matches to avoid recomputing them. This is particularly effective for nested quantifiers where the same subpattern is tried at the same position multiple times.

PCRE has a "recursion cache" for recursive patterns. Python's `re` module does not cache backtracking results, which is one reason it can be slower than PCRE for certain patterns.

**Thompson NFA simulation:**

The original technique used by Thompson for the QED editor (and the basis for RE2) is to simulate all NFA states simultaneously. Instead of backtracking, the engine maintains a set of current states and transitions all of them on each character.

```
States at position 0: {0}
Read 'a': states become {1, 2} (epsilon closure)
Read 'b': states become {3, 5}
Read 'c': states become {6} (accepting)
```

This simulation runs in O(m * n) time where m is the number of states and n is the input length. The set of states is bounded by the number of states in the NFA, so worst-case time is linear in the input length.

**Substring extraction:**

Engines like PCRE and Perl identify fixed substrings within the pattern and use them as checkpoints. If a required substring does not exist in the input, the engine rejects immediately without any backtracking.

**Anchor optimization:**

When a pattern starts with `^`, the engine only tries the match at position 0. Similarly, `$` allows the engine to fail early when the input is longer than the pattern can handle.

**Character class optimization:**

Character classes like `\d`, `\w`, and `[a-z]` are compiled into efficient bitmaps or ranges for fast lookup, rather than checking each character individually with a loop.

### Debugging Regex Performance

When a regex is slow, you need to see what the engine is doing. Several tools provide visual debugging.

**regex101.com debugger:**

The most accessible tool. Features:

- Step-by-step regex debugger
- Shows how many steps each match takes
- Displays backtracking visually
- Lists quantifier iterations
- Shows captured groups at each step

The debugger shows the explosion of steps. For "aaaaac" (5 a's), you might see 30+ steps. For 11 a's, hundreds of steps.

**RegexBuddy (commercial):**

RegexBuddy provides a detailed debugger with:

- Color-coded backtracking visualization
- Statistical breakdown of match attempts
- Simulation speed control
- Regex tree view showing the compiled structure

**PCRE's `--debug` and `-D` flags:**

When compiling PCRE, you can enable debugging output to see the compiled pattern and match tracing.

```bash
# With pcretest (PCRE test tool)
pcretest -d
# Enter pattern and input, see step-by-step matching

# With PCRE2
pcretest -p
```

**Python tracing:**

Python's `re` module does not have a built-in debugger, but you can use the `re.DEBUG` flag to see the compiled pattern structure:

```python
import re

# See the compiled pattern
re.compile(r'(a|aa|aaa)*b', re.DEBUG)
```

Output shows the bytecode-like instructions of the compiled pattern. It does not show runtime backtracking but helps understand how the pattern is structured.

**Measuring match attempts:**

You can estimate backtracking by measuring match time for increasing input lengths. If time doubles with each additional character, you have exponential backtracking.

```python
import re
import time

pattern = re.compile(r'(a|aa|aaa)*b')

for length in range(10, 30):
    test_string = 'a' * length + 'c'
    start = time.perf_counter()
    pattern.match(test_string)
    elapsed = time.perf_counter() - start
    print(f"n={length}: {elapsed*1000:.2f}ms")
```

A linear pattern's time stays flat. An exponential pattern's time doubles with each increment.

## Study Cases

### Case 1: Email validation regex that hangs

**Problem:** A developer writes an email validator:

```python
import re

email_pattern = r'^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$'
```

This regex works but is slow when validating malicious input like:

```
"aaaa...aaaa@b.c"  (1000 a's before @)
```

The `[a-zA-Z0-9._%+-]+` matches all the a's. The `@` must follow. If the input has no `@`, the engine backtracks through all 1000 characters one by one, making 1000+ attempts.

**Analysis:** The `+` quantifier is greedy. Every time `@` fails, the engine gives up one character from `+` and tries again. The time grows linearly with input length (O(n) backtracking), not exponentially, but it is still wasteful.

**Solution:** Use possessive quantifier or atomic group if supported:

```python
import regex  # not re

email_pattern = r'^[a-zA-Z0-9._%+-]++@[a-zA-Z0-9.-]++\.[a-zA-Z]{2,}$'
```

Or restructure to avoid backtracking entirely by using `[^@]+` for the local part:

```python
import re

email_pattern = r'^[^@]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$'
```

The `[^@]+` cannot match `@`, so it stops exactly at the `@` without backtracking.

### Case 2: Catastrophic path in a log parser

**Problem:** A log processing tool uses this pattern to extract key-value pairs:

```
Pattern: (\w+\s*)+=(.+?)
Input: "key1 = value1 key2 = value2 ... key100 = value100"
```

The `(\w+\s*)+` part has nested quantifiers. If the input has a malformed line like:

```
"key1 = value1 key2 = value2 ... key100 = "
```

The final `value100` is missing. The engine tries to match `(.+?)` lazily but there is nothing to match. Then it backtracks into `(\w+\s*)+`, trying thousands of ways to partition the preceding words. With 100 keys, the backtracking explodes.

**Solution:** Remove the outer quantifier. The `+` on the group is unnecessary because each key already matches with `\w+\s*`:

```python
# Before
pattern = re.compile(r'(\w+\s*)+=(.+?)')

# After — one key-value per match
pattern = re.compile(r'(\w+)\s*=\s*(.+)')
```

Use `re.findall` to extract multiple pairs instead of relying on nested quantifiers.

### Case 3: Choosing RE2 for user-supplied regex

**Problem:** A log search tool allows users to enter custom patterns. Users occasionally enter patterns that freeze the search for minutes.

**Root cause:** Patterns like `(a|aa|aaa)*b` against crafted input cause exponential backtracking in Python's `re` module.

**Solution:** Use `re2` (Python binding for RE2) for user-supplied patterns, or use Go's `regexp` package if rewriting in Go.

```python
try:
    import re2 as re_module
except ImportError:
    import re as re_module

def search_logs(pattern, log_content):
    try:
        # re2 guarantees no catastrophic backtracking
        compiled = re_module.compile(pattern)
        return compiled.findall(log_content)
    except re_module.error:
        return []
```

**Tradeoff:** RE2 does not support backreferences or lookahead. For a search tool, most users do not need these features. For those who do, fall back to a timeout-wrapped backtracking engine.

## Glossary

| Term | Definition |
|------|------------|
| NFA | Nondeterministic Finite Automaton — a state machine that can be in multiple states at once, allowing multiple possible transitions for the same input |
| DFA | Deterministic Finite Automaton — a state machine with exactly one transition per input per state, guaranteeing linear-time matching |
| Backtracking | Algorithmic technique where the engine explores one match path, then returns to try alternatives when the current path fails |
| Greedy quantifier | A quantifier (`*`, `+`, `?`, `{n,m}`) that matches as much as possible, then gives back characters on backtracking |
| Lazy quantifier | A quantifier (`*?`, `+?`, `??`, `{n,m}?`) that matches as little as possible, then expands on backtracking |
| Possessive quantifier | A quantifier (`*+`, `++`, `?+`, `{n,m}+`) that matches as much as possible and never backtracks |
| Atomic group | A group `(?>...)` whose internal choice points are discarded upon exit, preventing backtracking into the group |
| Epsilon transition | A transition in an NFA that consumes no input character; used to model choice points |
| Accepting state | A state in a finite automaton that indicates a successful match |
| Catastrophic backtracking | Exponential-time blowup caused by nested quantifiers and overlapping alternatives, forcing the engine to explore an exponential number of partitions |
| ReDoS | Regular Expression Denial of Service — a security attack that exploits catastrophic backtracking to consume server CPU |
| Choice point | A saved position in the input and pattern that the engine can return to when backtracking |
| Leftmost-first | Traditional NFA strategy: try alternatives in pattern order and stop at the first match |
| Leftmost-longest | POSIX NFA strategy: find the longest match among all alternatives at the leftmost position |
| Thompson NFA | NFA simulation that tracks all possible states simultaneously, guaranteeing O(m*n) time |
| State explosion | The exponential growth of DFA states when converting from an NFA |
| Boyer-Moore | A string search algorithm that skips characters using bad-character and good-suffix heuristics |
| Pre-check | An optimization that verifies basic conditions (minimum length, required substring) before full matching |
| Timeout | A defense mechanism that aborts regex matching when it exceeds a time limit |

## Quick References

- [Russ Cox — Regular Expression Matching Can Be Simple And Fast](https://swtch.com/~rsc/regexp/regexp1.html) — the definitive article on NFA vs DFA and Thompson NFA simulation
- [Russ Cox — Implementing Regular Expressions](https://swtch.com/~rsc/regexp/regexp3.html) — submatch tracking with automata
- [Regular-Expressions.info — Engine Internals](https://www.regular-expressions.info/engine.html) — comprehensive explanation of NFA backtracking
- [Wikipedia — ReDoS](https://en.wikipedia.org/wiki/ReDoS) — ReDoS vulnerability overview with examples
- [Cloudflare — Details of the Cloudflare outage on July 2, 2019](https://blog.cloudflare.com/details-of-the-cloudflare-outage-on-july-2-2019/) — postmortem of a ReDoS-caused global outage
- [OWASP — Regular Expression Denial of Service](https://owasp.org/www-community/attacks/Regular_expression_Denial_of_Service) — attack taxonomy and prevention
- [RE2 GitHub Repository](https://github.com/google/re2) — Google's linear-time regex library

## Next Steps

- [Regex Performance & Security](regex-performance-and-security.md) — practical optimization, testing, and defensive regex programming
