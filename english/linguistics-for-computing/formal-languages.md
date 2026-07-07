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
- [Pumping Lemma in Depth](#pumping-lemma-in-depth)
- [Parser Implementation Examples](#parser-implementation-examples)
- [Learning Tips](#learning-tips)
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

**Detailed examples for each level:**

**Type 3 (Regular) examples:**

```
L_3a = {w ∈ {0,1}* | w ends with "01"}
L_3b = {w ∈ {a,b}* | w has an even number of a's}
L_3c = {0^n 1^m | n, m ≥ 0}  — any number of 0s followed by any number of 1s
```

These can each be recognized by a DFA with a small number of states.

**Type 2 (Context-Free) examples:**

```
L_2a = {a^n b^n | n ≥ 0}          — balanced parentheses analog
L_2b = {ww^R | w ∈ {a,b}*}        — palindromes
L_2c = {a^n b^m c^m | n, m ≥ 0}   — two separate counting contexts
L_2d = {x | x is a valid arithmetic expression}
```

These require a stack to match or count.

**Type 1 (Context-Sensitive) examples:**

```
L_1a = {a^n b^n c^n | n ≥ 0}      — three-way counting
L_1b = {ww | w ∈ {a,b}*}          — copy language
L_1c = {a^n b^m c^n d^m | n, m ≥ 0} — crossed dependencies
```

These cannot be recognized by a PDA because they require tracking multiple independent counts simultaneously. A linear-bounded automaton (LBA) has enough tape to handle the dependency.

**Type 0 (Recursively Enumerable) examples:**

```
L_0a = {x | x is a valid C program that terminates}
L_0b = {x | x is a string encoding a true theorem of arithmetic}
```

These are undecidable — no algorithm can recognize all strings in the language.

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

### Pumping Lemma in Depth

The pumping lemma is a tool for proving that a language is not regular (or not context-free). It exploits the fact that finite automata have limited memory — they must repeat states for sufficiently long strings.

**Regular Pumping Lemma:** If $L$ is regular, there exists $p$ (pumping length) such that for any $s \in L$ with $|s| \ge p$, $s$ can be split as $s = xyz$ where:

1. $|y| > 0$ (the pumpable part is non-empty)
2. $|xy| \le p$ (the pumpable part is within the first $p$ symbols)
3. $xy^iz \in L$ for all $i \ge 0$ (pumping up or down stays in the language)

**Proof that $L = \{a^n b^n \mid n \ge 0\}$ is not regular:**

Assume $L$ is regular with pumping length $p$. Consider $s = a^p b^p$. The pumping lemma says $s = xyz$ with $|xy| \le p$ and $|y| > 0$. Since $|xy| \le p$, both $x$ and $y$ consist entirely of $a$'s. Pumping $y$ down ($i = 0$) gives $a^{p-|y|} b^p$, which has fewer $a$'s than $b$'s — not in $L$. Contradiction. Therefore $L$ is not regular.

**Context-Free Pumping Lemma:** If $L$ is context-free, there exists $p$ such that any $s \in L$ with $|s| \ge p$ can be split as $s = uvwxy$ where:

1. $|vx| > 0$ (at least one of $v, x$ is non-empty)
2. $|vwx| \le p$ (the pumpable part is bounded)
3. $uv^iwx^iy \in L$ for all $i \ge 0$

**Proof that $L = \{a^n b^n c^n \mid n \ge 0\}$ is not context-free:**

Assume $L$ is context-free with pumping length $p$. Consider $s = a^p b^p c^p$. The lemma says $s = uvwxy$ with $|vwx| \le p$. Since $p$ symbols cannot span all three letter groups, $vwx$ misses at least one letter type. Pumping changes the count of two letter types but not the third, breaking the balance. Contradiction. Therefore $L$ is not context-free.

**Why pumping matters in practice:** Pumping lemmas tell you when a regex or CFG cannot solve a problem. If you try to parse nested structures (HTML, balanced parentheses) with a regex, the pumping lemma proves you will fail for sufficiently large inputs.

```python
import re

# Attempt to match balanced parentheses with regex — fundamentally flawed
pattern = re.compile(r'^(\(+\)+)$')  # matches "((()))" but also "((()"
# This regex matches a^n b^n only up to backtracking limits, not in general
# The pumping lemma proves no regex can match all a^n b^n
```

### Parser Implementation Examples

#### Recursive Descent Parser for Arithmetic

```python
class Token:
    def __init__(self, type, value):
        self.type = type
        self.value = value

class Lexer:
    def __init__(self, text):
        self.text = text
        self.pos = 0
        self.tokens = []
        self._tokenize()

    def _tokenize(self):
        while self.pos < len(self.text):
            ch = self.text[self.pos]
            if ch.isdigit():
                num = ''
                while self.pos < len(self.text) and self.text[self.pos].isdigit():
                    num += self.text[self.pos]
                    self.pos += 1
                self.tokens.append(Token('NUMBER', int(num)))
            elif ch in '+-*/()':
                self.tokens.append(Token(ch, ch))
                self.pos += 1
            else:
                self.pos += 1  # skip whitespace

class Parser:
    def __init__(self, lexer):
        self.tokens = lexer.tokens
        self.pos = 0

    def peek(self):
        return self.tokens[self.pos] if self.pos < len(self.tokens) else None

    def consume(self, expected=None):
        token = self.peek()
        if expected and token.type != expected:
            raise SyntaxError(f"Expected {expected}, got {token.type}")
        self.pos += 1
        return token

    # Grammar:
    # expr   -> term (('+' | '-') term)*
    # term   -> factor (('*' | '/') factor)*
    # factor -> NUMBER | '(' expr ')'

    def parse_expr(self):
        result = self.parse_term()
        while self.peek() and self.peek().type in ('+', '-'):
            op = self.consume().type
            right = self.parse_term()
            if op == '+':
                result = ('add', result, right)
            else:
                result = ('sub', result, right)
        return result

    def parse_term(self):
        result = self.parse_factor()
        while self.peek() and self.peek().type in ('*', '/'):
            op = self.consume().type
            right = self.parse_factor()
            if op == '*':
                result = ('mul', result, right)
            else:
                result = ('div', result, right)
        return result

    def parse_factor(self):
        token = self.peek()
        if token.type == 'NUMBER':
            self.consume('NUMBER')
            return ('num', token.value)
        elif token.type == '(':
            self.consume('(')
            expr = self.parse_expr()
            self.consume(')')
            return expr
        else:
            raise SyntaxError(f"Unexpected token: {token.type}")

    def parse(self):
        ast = self.parse_expr()
        if self.peek():
            raise SyntaxError("Unexpected tokens after expression")
        return ast

# Example
lexer = Lexer("3 + 4 * 5")
parser = Parser(lexer)
ast = parser.parse()
print(ast)  # ('add', ('num', 3), ('mul', ('num', 4), ('num', 5)))
```

#### Parsing with a Stack (Shunting Yard)

The shunting yard algorithm converts infix expressions to postfix (Reverse Polish Notation) using a stack:

```python
def shunting_yard(tokens):
    precedence = {'+': 1, '-': 1, '*': 2, '/': 2}
    output = []
    operators = []

    for token in tokens:
        if token.type == 'NUMBER':
            output.append(token.value)
        elif token.type in '+-*/':
            while (operators and operators[-1] != '(' and
                   precedence.get(operators[-1], 0) >= precedence[token.type]):
                output.append(operators.pop())
            operators.append(token.type)
        elif token.type == '(':
            operators.append('(')
        elif token.type == ')':
            while operators and operators[-1] != '(':
                output.append(operators.pop())
            operators.pop()  # discard '('

    while operators:
        output.append(operators.pop())

    return output

# Evaluate: 3 + 4 * 5
tokens = [Token('NUMBER', 3), Token('+', '+'), Token('NUMBER', 4),
          Token('*', '*'), Token('NUMBER', 5)]
rpn = shunting_yard(tokens)
print(rpn)  # [3, 4, 5, '*', '+']
```

#### Building a Parse Tree with Grammar Rules

The recursive descent parser above builds an AST directly. For more complex parsing tasks, parser generators like ANTLR and Bison take a grammar specification and generate the parser code automatically:

```antlr
// ANTLR4 grammar for arithmetic
grammar Arithmetic;

expr: term (('+'|'-') term)* ;
term: factor (('*'|'/') factor)* ;
factor: NUMBER | '(' expr ')' ;

NUMBER: [0-9]+ ;
WS: [ \t\r\n]+ -> skip ;
```

### Learning Tips

**Build small parsers manually:** Implement recursive descent parsers for tiny languages (arithmetic, JSON subset, CSV). This builds deep understanding of how grammars map to code.

**Prove non-regularity with pumping:** For any language you suspect is not regular, write out the pumping lemma proof. The exercise forces precise thinking about a language's structure.

**Draw automata:** Sketching DFAs, NFAs, and PDAs on paper clarifies the mechanics of state transitions and stack operations. Compare DFA and PDA for the same language.

**Experiment with parser generators:** Write a grammar in ANTLR or Bison and see how ambiguous grammars produce parse conflicts. The error messages from parser generators teach grammar engineering.

**Study the Chomsky hierarchy by example:** For each level, write down 3-5 concrete languages and determine their type. Verify by trying to design the corresponding automaton.

**Use online tools:** Regex101 (interactive regex debugging), Grammarator (CFG visualization), and JFLAP (automata simulation) provide immediate feedback.

**Practice grammar transformations:** Take a grammar and convert it to CNF, eliminate left recursion, and left-factor it. These mechanical transformations build fluency with grammar representations.

## Glossary

| Term | Definition |
|------|------------|
| Alphabet | Finite set of symbols from which strings are formed |
| Ambiguous grammar | Grammar producing more than one parse tree for some string |
| AST | Abstract Syntax Tree — simplified tree representation of a parse |
| BNF | Backus-Naur Form — notation for context-free grammars |
| CFG | Context-Free Grammar — rules have single non-terminal on left |
| Chomsky hierarchy | Classification of grammars into four levels of generative power |
| Context-sensitive grammar | Grammar where rules depend on surrounding symbols |
| DFA | Deterministic Finite Automaton — exactly one transition per symbol per state |
| EBNF | Extended BNF — adds repetition, optionality, grouping |
| Empty string | String of length zero, denoted epsilon |
| FIRST set | Set of terminals that can begin strings derived from a non-terminal |
| FOLLOW set | Set of terminals that can appear after a non-terminal |
| FSA | Finite State Automaton — machine with finite states |
| Grammar | Finite set of production rules for generating language strings |
| Greibach Normal Form | CFG where productions are A -> a gamma |
| Kleene star | Operation producing zero or more repetitions |
| LALR | Look-Ahead LR — practical subset of LR parsing |
| LBA | Linear-Bounded Automaton — Turing machine with bounded tape |
| Left recursion | Grammar rule where a non-terminal derives itself as first symbol |
| Left factoring | Transforming a grammar to remove common prefixes in rules |
| Lexer | Program converting source code to tokens using regular languages |
| LL parser | Top-down parser producing leftmost derivation |
| LR parser | Bottom-up parser producing rightmost derivation |
| NFA | Nondeterministic Finite Automaton — multiple transitions possible |
| Non-terminal | Grammar symbol that can be expanded |
| Parse tree | Tree representation of a derivation |
| PDA | Pushdown Automaton — FSA with stack, equivalent to CFGs |
| Pumping lemma | Property used to prove languages are not regular/context-free |
| Recursive descent | Top-down parsing where each non-terminal becomes a function |
| Recursively enumerable | Type 0 languages recognized by a Turing machine |
| Regular expression | Pattern notation equivalent to regular languages |
| Regular language | Language recognizable by a finite automaton |
| Shunting yard | Algorithm for converting infix to postfix notation using a stack |
| String | Finite sequence of symbols from an alphabet |
| Terminal | Grammar symbol appearing in the generated language |
| Leftmost derivation | Derivation where the leftmost non-terminal is expanded first |
| Rightmost derivation | Derivation where the rightmost non-terminal is expanded first |
| Chomsky Normal Form | CFG where productions are A -> BC or A -> a |
| Catastrophic backtracking | Exponential time explosion in regex engines due to nested quantifiers |
| LL(*) | Parser class used by ANTLR, combining LL(k) with arbitrary lookahead |
| PEG | Parsing Expression Grammar — ordered choice deterministic grammar |

## Quick References

- [Hopcroft, Motwani, Ullman: Introduction to Automata Theory](https://www.pearson.com/en-us/subject-catalog/p/introduction-to-automata-theory-languages-and-computation/P200000003299)
- [Chomsky: Three Models for the Description of Language (1956)](https://doi.org/10.1080/00437956.1956.11698009)
- [Aho, Sethi, Ullman: Compilers: Principles, Techniques, and Tools](https://www.pearson.com/en-us/subject-catalog/p/compilers-principles-techniques-and-tools/P200000003267)
- [BNF and EBNF: What are they and how do they work?](https://tomassetti.me/ebnf/)
- [Regular Expression Matching Can Be Simple and Fast (Russ Cox)](https://swtch.com/~rsc/regexp/regexp1.html)
- [ANTLR Documentation](https://www.antlr.org/)
- [RE2: A linear-time regex engine](https://github.com/google/re2)
- [JFLAP: Automata Simulator](https://www.jflap.org/) — interactive DFA, NFA, PDA, and Turing machine simulation
- [Regex101](https://regex101.com/) — interactive regex debugger with explanation
- [Shunting Yard Algorithm (Wikipedia)](https://en.wikipedia.org/wiki/Shunting_yard_algorithm) — infix to postfix conversion
- [Recursive Descent Parsing Tutorial](https://www.engr.mun.ca/~theo/Misc/exp_parsing.htm) — practical guide to building parsers
- [Grammator: CFG Analyzer](https://www.jflap.org/) — CFG derivation and parse tree visualization

## Next Steps

- [Parsing](parsing.md) — algorithms for syntactic analysis in natural and programming languages
- [Semantics](semantics.md) — meaning in language and code, from lexical semantics to type systems
