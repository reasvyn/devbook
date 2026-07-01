# Parsing: Natural & Programming Languages

## Description

Parsing is the process of analyzing a sequence of symbols to determine its grammatical structure. For programming languages, parsing turns source code into executable structures. For natural language, parsing uncovers the syntactic relationships that convey meaning. Understanding both traditions helps developers build better tools, write clearer code, and work effectively with language models.

## Prerequisites

- [Formal Languages & Grammars](formal-languages.md) — alphabets, strings, grammars, Chomsky hierarchy, regular and context-free languages

## Table of Contents

- [What Is Parsing?](#what-is-parsing)
- [Tokenization and Lexing](#tokenization-and-lexing)
- [Top-Down vs. Bottom-Up Parsing](#top-down-vs-bottom-up-parsing)
- [Recursive Descent Parsers](#recursive-descent-parsers)
- [Parser Combinators](#parser-combinators)
- [LR Parsing and LALR](#lr-parsing-and-lalr)
- [Parsing Expression Grammars](#parsing-expression-grammars)
- [Parsing Natural Language](#parsing-natural-language)
- [Part-of-Speech Tagging](#part-of-speech-tagging)
- [Dependency Parsing](#dependency-parsing)
- [Constituency Parsing](#constituency-parsing)
- [Ambiguity in Natural vs. Programming Languages](#ambiguity-in-natural-vs-programming-languages)
- [Practical Parsing Tools](#practical-parsing-tools)
- [Parsing in Compilers and Interpreters](#parsing-in-compilers-and-interpreters)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### What Is Parsing?

Parsing takes a linear sequence of symbols and recovers the hierarchical structure assigned by a grammar.

**Two levels:** Lexical analysis (characters to tokens, regular languages) and syntactic analysis (tokens to parse tree/AST, context-free languages).

**Why parsing matters:**

| Domain | Why parsing is needed |
|--------|----------------------|
| Compilers | Analyze source code to produce machine code |
| Linters | Analyze code for style and correctness |
| IDEs | Autocomplete, refactoring, navigation |
| Natural language | Translation, summarization, QA |
| Data formats | Parse JSON, XML, YAML, protobuf |

**The universal challenge:**

```
Time flies like an arrow; fruit flies like a banana.
```

A parser must disambiguate "fruit flies" (noun phrase) vs. "time flies" (noun + verb).

### Tokenization and Lexing

Lexical analysis converts a character stream into tokens.

**Token types:**

```text
Input:  if (x < 10) { return x + 1; }

Tokens: IF "if", LPAREN "(", IDENT "x", LT "<", NUMBER "10",
        RPAREN ")", LBRACE "{", RETURN "return", IDENT "x",
        PLUS "+", NUMBER "1", SEMI ";", RBRACE "}"
```

**Maximal munch:** When multiple patterns match, the lexer takes the longest match (so "==" becomes EQ, not two ASSIGN tokens).

**Lexer states:** Some languages need context-sensitive lexing — Python's INDENT/DEDENT tokens, C's typedef tracking, HTML's mode switching.

### Top-Down vs. Bottom-Up Parsing

| Property | Top-down | Bottom-up |
|----------|----------|-----------|
| Direction | Root to leaves | Leaves to root |
| Algorithm | Predictive (LL) | Shift-reduce (LR) |
| Grammar restriction | No left recursion | Any CFG |
| Implementation | Simpler, hand-written | Table-driven |
| Grammar class | LL(k) | LR(k) (superset of LL) |

**Top-down derivation:** `expr => expr + term => term + term => factor + term => 3 + term => ...`

**Bottom-up reduction:** `3 (factor) => term => expr`, `4 * 5 => term`, `3 + term => expr`

### Recursive Descent Parsers

Each non-terminal in the grammar becomes a function. The most common approach for hand-written parsers.

**Grammar:**

```
expr    -> term (("+" | "-") term)*
term    -> factor (("*" | "/") factor)*
factor  -> NUMBER | "(" expr ")"
```

**Implementation:**

```python
class Parser:
    def __init__(self, tokens):
        self.tokens = tokens
        self.pos = 0

    def parse_expr(self):
        left = self.parse_term()
        while self.peek()[0] in ("PLUS", "MINUS"):
            op = self.consume()[1]
            right = self.parse_term()
            left = ("binop", op, left, right)
        return left

    def parse_term(self):
        left = self.parse_factor()
        while self.peek()[0] in ("TIMES", "DIVIDE"):
            op = self.consume()[1]
            right = self.parse_factor()
            left = ("binop", op, left, right)
        return left

    def parse_factor(self):
        if self.peek()[0] == "NUMBER":
            return ("num", self.consume("NUMBER")[1])
        elif self.peek()[0] == "LPAREN":
            self.consume("LPAREN")
            result = self.parse_expr()
            self.consume("RPAREN")
            return result
```

**Pratt parsing (precedence climbing):** An alternative using numeric precedence values, used by `rust-analyzer` and `esprima`.

### Parser Combinators

Higher-order functions that compose smaller parsers into larger ones. Code mirrors the grammar closely.

```python
def token(pattern):
    compiled = re.compile(pattern)
    def parse(input_str):
        m = compiled.match(input_str)
        if m:
            return [(m.group(0), input_str[m.end():])]
        return []
    return parse

def seq(*parsers):
    """Run parsers in sequence."""
    ...

def alt(*parsers):
    """Try each parser, return first success."""
    ...
```

Popular in functional programming: Parsec (Haskell), Nom (Rust), PyParsing (Python).

### LR Parsing and LALR

LR parsers use shift-reduce with a stack:

| Action | Description |
|--------|-------------|
| Shift | Push next input token onto stack |
| Reduce | Pop right-hand side, push left-hand non-terminal |

**LALR(1):** Merges LR(1) states with the same core but different lookaheads. Used by YACC and Bison. Fewer grammars accepted than full LR(1) but smaller tables.

### Parsing Expression Grammars

PEGs use prioritized choice (`/`) instead of unordered alternation (`|`).

```
CFG: A -> ab | ac    (nondeterministic)
PEG: A <- ab / ac    (ordered: try ab first)
```

PEGs eliminate ambiguity by ordering. Used by PEG.js, packrat parsing with memoization.

**Key difference:** PEG ordered choice means the grammar designer decides precedence, not the parser generator.

### Parsing Natural Language

**Key differences from programming languages:**

| Aspect | Programming | Natural |
|--------|-------------|---------|
| Design | Deliberate | Evolved |
| Ambiguity | Minimal | Pervasive |
| Coverage | Complete grammar | Open vocabulary |
| Resolution | Deterministic | Probabilistic |

**The NLP parsing pipeline:**

```
Raw text -> Tokenization -> POS tagging -> Lemmatization -> Parsing -> Semantics
```

**Ambiguity challenges:**

```
"I saw the man with the telescope."
  -> (saw (the man) (with the telescope))  — I used telescope
  -> (saw (the man with the telescope))    — man had telescope

"Flying planes can be dangerous."
  -> (Flying (planes)) can be dangerous.   — planes that fly
  -> (Flying planes) can be dangerous.     — the act of flying
```

### Part-of-Speech Tagging

POS tagging labels each word with a grammatical category.

**Penn Treebank tags:**

```text
Input:  "The/DT quick/JJ brown/JJ fox/NN jumps/VBZ over/IN the/DT lazy/JJ dog/NN"
```

**Approaches:**

| Approach | Accuracy | Example |
|----------|----------|---------|
| Baseline (most common tag) | ~90% | Every word -> NN |
| HMM (Viterbi) | ~95% | Stanford Tagger |
| CRF | ~96.5% | ClearNLP |
| BiLSTM-CRF | ~97.5% | Flair, spaCy |
| Transformer | ~98% | BERT fine-tuned |

### Dependency Parsing

Represents sentence structure as binary relations between words.

**Dependency tree for "The quick brown fox jumps":**

```
jumps (ROOT)
  |--- nsubj -> fox
  |            |--- det -> The
  |            |--- amod -> quick, brown
  |--- prep -> over
               |--- pobj -> dog
                            |--- det -> the
                            |--- amod -> lazy
```

**Transition-based parsing** processes left to right with a stack and buffer. **Graph-based parsing** finds the maximum spanning tree.

**Evaluation:** UAS (Unlabeled Attachment Score, ~95%), LAS (Labeled Attachment Score, ~93%).

### Constituency Parsing

Produces phrase structure trees showing how words group into phrases.

```
           S
         /   \
       NP     VP
      / \    / \
    Det  N  V   NP
    The dog chased the cat
```

**Probabilistic CFGs (PCFGs)** assign probabilities to rules. The CYK algorithm finds the most probable parse using dynamic programming.

**Constituency vs. Dependency:**

| Property | Constituency | Dependency |
|----------|-------------|-----------|
| Structure | Nested phrases | Binary relations |
| Root | S (sentence) | ROOT (predicate) |
| Words | At leaves | At all nodes |
| Applications | Language modeling | Relation extraction |

### Ambiguity in Natural vs. Programming Languages

**Natural language ambiguity:**

| Type | Example |
|------|---------|
| Lexical | "bank" (financial vs. river) |
| Syntactic | "Visiting relatives can be boring." |
| Semantic | "The chicken is ready to eat." |
| Scope | "Every man loves a woman." |

**Programming language ambiguity (minimal by design):**

Dangling-else in C: `if (a) if (b) x = 1; else x = 2;` — else binds to nearest if.

C typedef: `x * y;` is ambiguous (declaration vs. expression) — resolved by the "lexer hack" feeding type info back from parser.

### Practical Parsing Tools

| Tool | Grammar class | Input | Use case |
|------|---------------|-------|----------|
| YACC/Bison | LALR(1) | BNF | Production compilers |
| ANTLR | LL(*) | EBNF | DSLs, language workbenches |
| Tree-sitter | GLR | Grammar DSL | IDEs, editors (error-tolerant, incremental) |
| PEG.js | PEG | PEG | Small DSLs in JavaScript |
| Nom | Combinator | Rust | Embedded parsers |

### Parsing in Compilers and Interpreters

```
Source -> Lexer -> Parser -> AST -> Semantic Analysis -> Code Gen
```

**Parser's role:**

- **Lexer:** Defines token patterns (regular languages)
- **Parser:** Builds AST from tokens
- **Semantic analysis:** Uses AST for type checking and symbol resolution
- **Code gen:** Walks AST to emit instructions/bytecode

## Glossary

| Term | Definition |
|------|------------|
| AST | Abstract Syntax Tree — simplified tree representation of code |
| Bottom-up parsing | Builds tree from leaves to root (reduce tokens to productions) |
| Constituency | Phrase structure analysis grouping words into nested constituents |
| CYK algorithm | DP algorithm for parsing CFGs in Chomsky Normal Form |
| Dependency | Binary grammatical relation between head and dependent |
| LALR | Look-Ahead LR — pragmatic subset of LR(1) |
| Lexer | Program converting character sequences to tokens |
| LL parser | Top-down parser, left-to-right, leftmost derivation |
| LR parser | Bottom-up parser, left-to-right, rightmost derivation |
| Parser combinator | Higher-order function composing simple parsers into complex ones |
| PCFG | Probabilistic CFG with rule probabilities |
| PEG | Parsing Expression Grammar — ordered choice, no ambiguity |
| POS tagging | Part-of-speech tagging — labeling words with grammatical categories |
| Recursive descent | Top-down parsing: each non-terminal becomes a function |
| Shift-reduce | LR parsing actions — shift pushes, reduce pops |
| Token | Classified lexical unit (NUMBER, IDENTIFIER, KEYWORD) |
| Top-down parsing | Builds tree from root to leaves (expand non-terminals) |
| Tree-sitter | Incremental, error-tolerant parsing library |

## Quick References

- [Aho, Sethi, Ullman: Compilers: Principles, Techniques, and Tools](https://www.pearson.com/en-us/subject-catalog/p/compilers-principles-techniques-and-tools/P200000003267)
- [ANTLR Mega Tutorial](https://tomassetti.me/antlr-mega-tutorial/)
- [Tree-sitter Documentation](https://tree-sitter.github.io/tree-sitter/)
- [Jurafsky & Martin: Speech and Language Processing](https://web.stanford.edu/~jurafsky/slp3/)
- [spaCy Parser](https://spacy.io/api/parser)
- [Universal Dependencies](https://universaldependencies.org/)
- [Crafting Interpreters (Robert Nystrom)](https://craftinginterpreters.com/)

## Next Steps

- [Semantics](semantics.md) — meaning in language and code, from lexical semantics to type systems
- [Language Models](language-models.md) — language models and linguistic theory, n-grams to transformers
