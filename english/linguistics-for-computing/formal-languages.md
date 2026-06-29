# Formal Languages & Grammars

## Description

Formal language theory is the mathematical foundation for describing both programming language syntax and natural language structure. Understanding alphabets, strings, grammars, and the Chomsky hierarchy gives developers the conceptual tools to design parsers, interpreters, compilers, and domain-specific languages with precision.

## Prerequisites

- [What Is Computational Linguistics?](../intro/what-is-computational-linguistics.md) — scientific study of language using computational methods

## Table of Contents

- [Alphabets, Strings, and Languages](#alphabets-strings-and-languages)
- [Grammars as Generative Systems](#grammars-as-generative-systems)
- [The Chomsky Hierarchy](#the-chomsky-hierarchy)
- [Regular Languages and Finite Automata](#regular-languages-and-finite-automata)
- [Regular Expressions and Practical Patterns](#regular-expressions-and-practical-patterns)
- [Context-Free Grammars](#context-free-grammars)
- [Parse Trees and Derivation](#parse-trees-and-derivation)
- [Ambiguity in Grammars](#ambiguity-in-grammars)
- [BNF and EBNF Notation](#bnf-and-ebnf-notation)
- [Grammar Engineering](#grammar-engineering)
- [Practical Uses: Parsers, Compilers, and DSLs](#practical-uses-parsers-compilers-and-dsls)
- [Limits of Formal Grammars](#limits-of-formal-grammars)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Alphabets, Strings, and Languages

An **alphabet** is a finite, non-empty set of symbols. Every formal language is built on an alphabet.

```
Sigma_1 = {0, 1}
Sigma_2 = {a, b, c, ..., z}
```

A **string** is a finite sequence of symbols drawn from an alphabet. The **empty string** is denoted by epsilon (E). A **language** is a set of strings over an alphabet.

```
L1 = {E, a, aa, aaa}                     (finite)
L2 = {a, aa, aaa, aaaa, ...}             (infinite)
L3 = {a^n b^n | n >= 0}                  (infinite)
L4 = {w | w is a valid C program}         (infinite)
```

**Operations on strings:** concatenation, Kleene star, reversal. A language can be viewed as a function f: Sigma* -> {0,1} that decides membership.

### Grammars as Generative Systems

A **grammar** is a finite set of rules that defines (generates) a language. Formally, a grammar G = (N, Sigma, P, S).

| Component | Name | Example |
|-----------|------|---------|
| N | Non-terminals | {S, NP, VP, N, V} |
| Sigma | Terminals | {cat, dog, chased} |
| P | Productions | {S -> NP VP} |
| S | Start symbol | S |

**Derivation:** Starting from S, repeatedly apply productions until only terminals remain.

```
S -> NP VP -> cat VP -> cat chased NP -> cat chased dog
```

### The Chomsky Hierarchy

Noam Chomsky (1956) classified grammars into four types by production rule form.

| Type | Name | Rule Form | Automaton | Example |
|------|------|-----------|-----------|---------|
| 3 | Regular | A -> aB or A -> a | Finite automaton | a*b* |
| 2 | Context-free | A -> gamma | Pushdown automaton | a^n b^n |
| 1 | Context-sensitive | alpha A beta -> alpha gamma beta | Linear-bounded automaton | a^n b^n c^n |
| 0 | Recursively enumerable | alpha -> beta | Turing machine | Any computable language |

**Programming relevance:**

| Language type | Programming use |
|---------------|-----------------|
| Regular | Lexical analysis, token patterns, regex |
| Context-free | Syntax of most programming languages |
| Context-sensitive | Semantic analysis, type checking |
| Recursively enumerable | Full computation (not used in language design) |

### Regular Languages and Finite Automata

A **regular language** can be recognized by a finite state automaton (FSA). A **Deterministic Finite Automaton (DFA)** is a 5-tuple (Q, Sigma, delta, q0, F).

**DFA for strings ending with "01":**

```
State diagram:
     0       1
   q0 ---> q1
    ^       |
    |  1    |
    +-- q2 <-+
     0,1

Accept: q2
```

An **NFA** may have multiple transitions for the same symbol. Every NFA has an equivalent DFA (possibly exponential states).

**Pumping Lemma:** If L is regular, there exists a pumping length p where any s in L with |s| >= p can be "pumped." Used to prove languages like a^n b^n are not regular.

**Limitations of regular languages:** A finite automaton cannot count arbitrarily. L = {a^n b^n | n >= 0} is not regular.

### Regular Expressions and Practical Patterns

Every regex pattern corresponds to a finite automaton. Core operations: concatenation (ab), alternation (a|b), Kleene star (a*).

**Practical regex pitfalls:**

| Pitfall | Pattern | Problem |
|---------|---------|---------|
| Catastrophic backtracking | `(a\|aa\|aaa)*b` | Exponential time on long "a" strings |
| Overmatching | `<.*>` matches full tag span | Use non-greedy `<.*?>` |
| HTML parsing | `<.*>.*?</.*>` | HTML is not regular; use a parser |

**Backreferences** go beyond regular languages — `(cat)\1` matches "catcat", which is not a regular language (it's the copy language ww).

### Context-Free Grammars

A **CFG** has productions where the left side is a single non-terminal: A -> gamma. "Context-free" means the rule applies regardless of surrounding context.

**Example CFG for arithmetic:**

```
E -> E + T | E - T | T
T -> T * F | T / F | F
F -> (E) | id | num
```

**Pushdown Automaton (PDA):** CFGs correspond to automata with a stack, enabling matching of nested structures.

```
PDA actions for "(()())":
Read (, push (    stack: [(]
Read (, push (    stack: [(, (]
Read ), pop (     stack: [(]
Read (, push (    stack: [(, (]
Read ), pop (     stack: [(]
Read ), pop (     stack: []
Accept
```

**Normal forms:** Chomsky Normal Form (CNF): A -> BC or A -> a. Greibach Normal Form (GNF): A -> a gamma.

### Parse Trees and Derivation

A **parse tree** shows the hierarchical structure of a derivation. An **Abstract Syntax Tree (AST)** removes punctuation and non-essential nodes.

**Parse tree for 3 + 4 * 5:**

```
    E
  / | \
 E  +  T
 |    /|\
 T   T * F
 |   |   |
 F   F   5
 |   |
 3   4
```

**AST:**

```
  (+)
 /  \
3    (*)
    /  \
   4    5
```

**Concrete vs. Abstract Syntax:**

| Concrete syntax | Abstract syntax |
|-----------------|-----------------|
| Includes keywords, delimiters, punctuation | Only structural elements |
| One-to-one with grammar rules | Many-to-one with grammar |

### Ambiguity in Grammars

A grammar is ambiguous if some string has more than one parse tree.

**Dangling-else ambiguity:**

```
stmt -> if expr then stmt | if expr then stmt else stmt
```

For `if a then if b then c else d`, two interpretations exist. **Resolved by:** precedence, associativity, disambiguating rules ("else matches nearest if"), or grammar rewrite.

**Operator precedence in grammar:**

```
expr   -> expr + term | term
term   -> term * factor | factor
factor -> NUMBER | ( expr )
```

This makes `*` bind tighter than `+`.

### BNF and EBNF Notation

**Backus-Naur Form (BNF):**

```
<expr> ::= <term> | <expr> "+" <term>
<term> ::= <factor> | <term> "*" <factor>
```

**Extended BNF (EBNF):** Adds operators for repetition `{...}`, optional `[...]`, grouping `(...)`.

```
EBNF for JSON:
value  = string | number | object | array | "true" | "false" | "null"
object = "{" [member {"," member}] "}"
array  = "[" [value {"," value}] "]"
```

### Grammar Engineering

**Left recursion elimination** for top-down parsers:

```
// Left-recursive
E -> E + T | T
// Transformed
E -> T E'
E' -> + T E' | E
```

**Left factoring** when rules share a common prefix:

```
// Unfactored
stmt -> if expr then stmt else stmt | if expr then stmt
// Factored
stmt -> if expr then stmt else_part
else_part -> else stmt | E
```

**FIRST and FOLLOW sets** guide LL parser decisions:

```
FIRST(X) = set of terminals that can begin strings derived from X
FOLLOW(A) = set of terminals that can follow A in derivation
```

### Practical Uses: Parsers, Compilers, and DSLs

**Lexical analysis with regular languages:** Token patterns for identifiers `[a-zA-Z_][a-zA-Z0-9_]*`, numbers `[0-9]+(\.[0-9]+)?`, keywords.

**Syntax analysis with CFGs:** Parser takes tokens and builds AST using grammar productions.

**Parser generators:**

| Tool | Grammar class | Output |
|------|---------------|--------|
| YACC/Bison | LALR(1) | C parser |
| ANTLR | LL(*) | Multi-language parser |
| PEG.js | PEG | JavaScript parser |

**Domain-Specific Languages (DSLs):** Regex (regular), SQL (CFG), JSON (CFG), Markdown (CFG).

### Limits of Formal Grammars

Even CFGs cannot express everything:

| Feature | Why it's not CF |
|---------|-----------------|
| Variable declarations (scope) | Requires counting across contexts (a^n b^n c^n) |
| Type consistency | Cross-reference checking |
| Same identifier twice (ww) | ww is not context-free |

Greibach's Theorem ensures every CFG can be rewritten to GNF, enabling linear parsing for restricted subclasses.

## Study Cases

### Case 1: Regex Performance and ReDoS Prevention

A web application accepts user regex patterns. Pattern `(a|aa|aaa)*b` against long "a" strings causes catastrophic backtracking.

**Solution:** Use a DFA-based engine (RE2):

```python
import re2
pattern = re2.compile(r"(a|aa|aaa)*b")
pattern.match("a" * 100 + "b")  # Returns immediately
```

### Case 2: BNF for a Configuration Language

Grammar for a config format with sections and key-value pairs:

```
config -> {section}
section -> "[" IDENTIFIER "]" {entry}
entry -> IDENTIFIER "=" value
value -> STRING | NUMBER | BOOLEAN | list
list -> "[" [value {"," value}] "]"
```

## Examples

### Example 1: DFA for Binary Strings with "101"

```
States: q0, q1, q2 (got "10"), q3 (found "101")
Transitions:
q0 --0--> q0    q0 --1--> q1
q1 --0--> q2    q1 --1--> q1
q2 --0--> q0    q2 --1--> q3
q3 --0--> q3    q3 --1--> q3
Accept: q3
```

Test: "1101" -> q0 -> q1 -> q1 -> q2 -> q3 (accept)

### Example 2: CFG Derivation of "the cat sat on the mat"

```
S -> NP VP -> Det N VP -> "the" N VP
  -> "the" "cat" VP -> "the" "cat" V NP
  -> "the" "cat" "sat" NP -> "the" "cat" "sat" Det N
  -> "the" "cat" "sat" "the" N -> "the" "cat" "sat" "the" "mat"
```

### Example 3: Removing Left Recursion

```
E -> T E'
E' -> + T E' | E
T -> F T'
T' -> * F T' | E
F -> (E) | id
```

### Example 4: LL(1) Parsing Table Fragment

```
    | id        | +        | *        | (         | )    | $
E   | E -> T E' |          |          | E -> T E' |      |
E'  |           | E'->+TE' |          |           | E'->E| E'->E
T   | T -> F T' |          |          | T -> F T' |      |
T'  |           | T'->E    | T'->*FT' |           | T'->E| T'->E
F   | F -> id   |          |          | F -> (E)  |      |
```

### Example 5: Regex to NFA via Thompson Construction

Pattern `(a|b)*abb`: Build NFA bottom-up with epsilon transitions for union and Kleene star, concatenate for the suffix "abb".

## Glossary

| Term | Definition |
|------|------------|
| Alphabet | Finite set of symbols from which strings are formed |
| Ambiguous grammar | Grammar producing more than one parse tree for some string |
| AST | Abstract Syntax Tree — simplified tree representation of a parse |
| BNF | Backus-Naur Form — notation for context-free grammars |
| CFG | Context-Free Grammar — rules have single non-terminal on left |
| Chomsky hierarchy | Classification of grammars into four levels of generative power |
| DFA | Deterministic Finite Automaton — exactly one transition per symbol per state |
| EBNF | Extended BNF — adds repetition, optionality, grouping |
| Empty string | String of length zero, denoted epsilon |
| FIRST set | Set of terminals that can begin strings derived from a non-terminal |
| FOLLOW set | Set of terminals that can appear after a non-terminal |
| FSA | Finite State Automaton — machine with finite states |
| Grammar | Finite set of production rules for generating language strings |
| Kleene star | Operation producing zero or more repetitions |
| LALR | Look-Ahead LR — practical subset of LR parsing |
| Left recursion | Grammar rule where a non-terminal derives itself as first symbol |
| Lexer | Program converting source code to tokens using regular languages |
| LL parser | Top-down parser producing leftmost derivation |
| LR parser | Bottom-up parser producing rightmost derivation |
| NFA | Nondeterministic Finite Automaton — multiple transitions possible |
| Non-terminal | Grammar symbol that can be expanded |
| Parse tree | Tree representation of a derivation |
| PDA | Pushdown Automaton — FSA with stack, equivalent to CFGs |
| Pumping lemma | Property used to prove languages are not regular/context-free |
| Recursive descent | Top-down parsing where each non-terminal becomes a function |
| Regular expression | Pattern notation equivalent to regular languages |
| Regular language | Language recognizable by a finite automaton |
| String | Finite sequence of symbols from an alphabet |
| Terminal | Grammar symbol appearing in the generated language |

## Quick References

- [Hopcroft, Motwani, Ullman: Introduction to Automata Theory](https://www.pearson.com/en-us/subject-catalog/p/introduction-to-automata-theory-languages-and-computation/P200000003299)
- [Chomsky: Three Models for the Description of Language (1956)](https://doi.org/10.1080/00437956.1956.11698009)
- [Aho, Sethi, Ullman: Compilers: Principles, Techniques, and Tools](https://www.pearson.com/en-us/subject-catalog/p/compilers-principles-techniques-and-tools/P200000003267)
- [BNF and EBNF: What are they and how do they work?](https://tomassetti.me/ebnf/)
- [Regular Expression Matching Can Be Simple and Fast (Russ Cox)](https://swtch.com/~rsc/regexp/regexp1.html)
- [ANTLR Documentation](https://www.antlr.org/)
- [RE2: A linear-time regex engine](https://github.com/google/re2)

## Next Steps

- [Parsing](parsing.md) — algorithms for syntactic analysis in natural and programming languages
- [Semantics](semantics.md) — meaning in language and code, from lexical semantics to type systems
