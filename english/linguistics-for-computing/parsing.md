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
- [Pratt Parsing](#pratt-parsing)
- [Parser Combinators](#parser-combinators)
- [LR Parsing and LALR](#lr-parsing-and-lalr)
- [GLR Parsing](#glr-parsing)
- [Earley Parsing](#earley-parsing)
- [Parsing Expression Grammars](#parsing-expression-grammars)
- [Error Recovery in Parsers](#error-recovery-in-parsers)
- [Parser Generators and Their Tradeoffs](#parser-generators-and-their-tradeoffs)
- [Parsing Natural Language](#parsing-natural-language)
- [Part-of-Speech Tagging](#part-of-speech-tagging)
- [Dependency Parsing](#dependency-parsing)
- [Constituency Parsing](#constituency-parsing)
- [Ambiguity in Natural vs. Programming Languages](#ambiguity-in-natural-vs-programming-languages)
- [Practical Parsing Tools](#practical-parsing-tools)
- [Parsing in Compilers and Interpreters](#parsing-in-compilers-and-interpreters)
- [Learning Tips](#learning-tips)
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

**Limitations:** Left-recursive grammars cause infinite loops. The grammar must be refactored to eliminate left recursion before a recursive descent parser can handle it. Backtracking can also be expensive if the grammar has many alternatives at a single position.

**Backtracking variant:** A recursive descent parser with backtracking tries each alternative and rolls back on failure. This is simpler to write but can have exponential worst-case time complexity. A packrat parser (PEG-based) uses memoization to cache results at each position, reducing this to linear time.

### Pratt Parsing

Pratt parsing (also called precedence climbing or top-down operator precedence) assigns numeric binding powers to tokens to handle operator precedence without a separate grammar layer.

**How it works:**

Each token type has a left binding power (lbp) and optionally a prefix or infix handler. Null denotations (nud) handle prefix positions. Left denotations (led) handle infix positions.

```python
class PrattParser:
    def __init__(self, tokens):
        self.tokens = tokens
        self.pos = 0
        self.prefix = {}
        self.infix = {}
        self.lbp = {}

    def register_prefix(self, token_type, handler):
        self.prefix[token_type] = handler

    def register_infix(self, token_type, handler, bp):
        self.infix[token_type] = handler
        self.lbp[token_type] = bp

    def parse(self, rbp=0):
        token = self.peek()
        self.advance()
        left = self.prefix[token[0]](self, token)
        while self.pos < len(self.tokens) and rbp < self.lbp.get(self.peek()[0], 0):
            token = self.advance()
            left = self.infix[token[0]](self, left, token)
        return left
```

**Binding powers for arithmetic:**

| Operator | Binding power | Associativity |
|----------|--------------|---------------|
| + - | 10 | Left |
| * / | 20 | Left |
| ^ | 30 | Right |
| unary - | 40 | Right |

```python
# Register handlers
parser.register_prefix("NUMBER", lambda p, t: ("num", t[1]))
parser.register_prefix("MINUS", lambda p, t: ("neg", p.parse(40)))
parser.register_infix("PLUS", lambda p, l, t: ("add", l, p.parse(10)), 10)
parser.register_infix("TIMES", lambda p, l, t: ("mul", l, p.parse(20)), 20)
```

**Why use Pratt parsing:**

- No separate grammar file or parser generator needed
- Handles associativity and precedence in minimal code
- Easy to extend with new operators
- Used by rust-analyzer, esprima, jq, and many production parsers
- Linear time complexity for typical expression grammars

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

**Advantages of combinators:**

- Compositional — small parsers combine naturally
- Embedded in the host language — full access to types, control flow, and tooling
- No code generation step
- Good for DSLs and small-to-medium grammars

**Disadvantages:**

- Error messages are often poor compared to generated parsers
- Performance can be unpredictable with backtracking
- Grammar is not portable — locked to the host language

### LR Parsing and LALR

LR parsers use shift-reduce with a stack:

| Action | Description |
|--------|-------------|
| Shift | Push next input token onto stack |
| Reduce | Pop right-hand side, push left-hand non-terminal |

**LALR(1):** Merges LR(1) states with the same core but different lookaheads. Used by YACC and Bison. Fewer grammars accepted than full LR(1) but smaller tables.

**LR parse table construction:**

The parser generator builds ACTION and GOTO tables from the grammar. The ACTION table tells the parser whether to shift or reduce given the current state and lookahead token. The GOTO table tells the parser which state to transition to after a reduce.

```
Example ACTION table snippet for state 5:

LOOKAHEAD:  +    *    (    )   num   $
State 5:    s6   r2   r2   r2        r2
```

LR parsing is deterministic and linear time. The tables can be large (thousands of states for realistic languages), which is why LALR merging is attractive — it produces much smaller tables at the cost of accepting fewer grammars.

**LR vs. LL tradeoffs:**

| Aspect | LL | LR |
|--------|----|----|
| Grammar class | LL(k) subset | LR(k) superset |
| Table size | Small | Large (LALR reduces) |
| Error detection | Early | Late (may shift many tokens before detecting error) |
| Hand-written feasible | Yes | No (table-driven) |
| Error recovery | Easier to customize | Harder |

### GLR Parsing

Generalized LR (GLR) parsing extends LR to handle ambiguous and non-deterministic grammars by exploring multiple parse paths simultaneously.

**How GLR works:**

When the LR table has a shift/reduce or reduce/reduce conflict, GLR forks the parser state. It maintains a graph-structured stack (GSS) that represents all possible parser states at each point in the input. When a token is consumed, the parser advances all active states. States that reach the end with a valid parse produce a result.

```python
# Conceptual GLR operation on ambiguous grammar
def glr_parse(tokens, table):
    # Active states: set of (stack, position) pairs
    active = {initial_state()}
    results = []

    for token in tokens:
        next_active = set()
        for stack, pos in active:
            actions = table.action(stack.top(), token.type)
            for action in actions:       # May be multiple!
                if action.is_shift():
                    next_active.add(shift(stack, token, action.next_state))
                elif action.is_reduce():
                    next_active.add(reduce(stack, action.production))
        active = next_active

    # Accept any active state that can reduce to start symbol
    for stack, pos in active:
        if can_accept(stack):
            results.append(build_tree(stack))
    return results
```

**Use cases for GLR:**

- Parsing natural language with PCFGs (ambiguous by nature)
- Parsing programming languages with complex syntax (C++, Ruby)
- Parsing languages with context-sensitive features that are easier to express ambiguously

**Performance:**

GLR can be O(n³) in the worst case for highly ambiguous grammars, but for typical programming language grammars it runs in near-linear time. Tree-sitter uses a GLR-based approach with optimizations for incremental re-parsing.

**Tree-sitter's GLR variant:**

Tree-sitter adds error tolerance and incremental parsing. It maintains a syntax tree that survives partial edits, re-parsing only the affected regions. This makes it ideal for IDEs and code editors where the code is often syntactically incomplete.

### Earley Parsing

Earley parsing handles all context-free grammars, including ambiguous, left-recursive, and right-recursive grammars, in O(n³) worst-case time (O(n²) for unambiguous grammars, O(n) for LR grammars).

**The Earley algorithm maintains a set of "Earley items" for each position in the input:**

An Earley item is a production rule with a dot marking how much of the rule has been matched, plus a pointer to the position where the rule started. The algorithm processes items through three operations:

**Prediction:** If the dot precedes a non-terminal, add all productions of that non-terminal with the dot at the beginning.

**Scanning:** If the dot precedes a terminal matching the current input token, advance the dot past the terminal.

**Completion:** If the dot is at the end of a production, look for items in the chart at the rule's start position that have this non-terminal after the dot, and advance those items.

```python
def earley_parse(grammar, tokens):
    chart = [set() for _ in range(len(tokens) + 1)]
    # Start with all productions of the start symbol
    for prod in grammar.productions[grammar.start]:
        chart[0].add(Item(prod, 0, 0))

    for i in range(len(tokens) + 1):
        # Process items in current column until no more changes
        changed = True
        while changed:
            changed = False
            for item in list(chart[i]):
                if not item.complete():
                    symbol = item.next_symbol()
                    if symbol in grammar.nonterminals:
                        # Predict: add all productions of symbol
                        for prod in grammar.productions[symbol]:
                            new = Item(prod, 0, i)
                            if new not in chart[i]:
                                chart[i].add(new)
                                changed = True
                    elif i < len(tokens) and symbol == tokens[i]:
                        # Scan: advance dot past terminal
                        new = item.advance()
                        chart[i + 1].add(new)
                        changed = True
                else:
                    # Complete: advance items that predicted this
                    for pred in chart[item.start]:
                        if not pred.complete() and pred.next_symbol() == item.rule.lhs:
                            new = pred.advance()
                            if new not in chart[i]:
                                chart[i].add(new)
                                changed = True

    # Accept if any complete start-symbol item is at the end
    return any(item.rule.lhs == grammar.start and item.start == 0
               for item in chart[len(tokens)])
```

**Advantages of Earley:**

- Handles ANY context-free grammar — no grammar refactoring required
- Works well with ambiguous grammars (returns all parses)
- Natural language parsing often uses Earley with PCFGs

**Disadvantages:**

- O(n³) worst-case makes it slower than LR or LL for unambiguous grammars
- Memory usage is higher due to the chart
- Not suitable for production compilers processing large files

### Parsing Expression Grammars

PEGs use prioritized choice (`/`) instead of unordered alternation (`|`).

```
CFG: A -> ab | ac    (nondeterministic)
PEG: A <- ab / ac    (ordered: try ab first)
```

PEGs eliminate ambiguity by ordering. Used by PEG.js, packrat parsing with memoization.

**Key difference:** PEG ordered choice means the grammar designer decides precedence, not the parser generator.

**PEG vs. CFG — practical implications:**

- PEGs are always unambiguous by construction
- PEGs cannot express languages that require unbounded lookahead with alternatives
- PEGs do not have left recursion; it must be eliminated or converted to repetition
- Every PEG defines exactly one language, but the ordered choice means some CFG languages are not expressible as PEGs

**Packrat parsing:**

PEGs are typically implemented with packrat parsing, which uses memoization to cache the result of each rule at each position. This guarantees linear time at the cost of O(n × r) memory, where r is the number of rules.

### Error Recovery in Parsers

When a parser encounters invalid input, it must report the error and attempt to continue. Error recovery strategies fall into several categories:

**Panic mode:** The parser discards tokens until it finds a synchronizing token (typically `;`, `}`, `end`, or a keyword). The parser then resumes from that point.

```python
def panic_sync(parser, sync_tokens):
    while parser.pos < len(parser.tokens):
        token = parser.peek()
        if token[0] in sync_tokens:
            return  # Resume parsing
        parser.advance()
    # End of input reached — cannot recover
```

Panic mode is simple and never hangs, but it can skip significant regions of valid code, producing cascading errors.

**Insertion/deletion recovery:** The parser simulates inserting or deleting a token to make the input valid. If inserting a semicolon would allow the parse to continue, the parser reports the missing token and proceeds as if it were present.

**Error productions:** The grammar explicitly includes productions for common mistakes:

```
statement -> error ";"         # Catch erroneous statements
expression -> error            # Catch erroneous expressions
```

The `error` pseudo-terminal matches any sequence of tokens until a synchronizing token. This is used in YACC/Bison and gives good error messages for common mistakes.

**Error recovery in practice:**

| Parser | Strategy | Quality of recovery |
|--------|----------|-------------------|
| GCC (hand-written) | Hand-coded recovery per construct | Very good, specific messages |
| YACC/Bison | Error productions + panic mode | Moderate |
| Tree-sitter | GLR with error nodes | Excellent, survives any input |
| Elastic tab stop (Roslyn) | Region-based resynchronization | Very good |

**Error-tolerant parsing (Tree-sitter approach):**

Rather than crashing or skipping, Tree-sitter produces a concrete syntax tree that includes ERROR nodes marking the invalid regions. The editor can highlight these regions and still provide correct syntax highlighting and code completion for surrounding valid code.

### Parser Generators and Their Tradeoffs

Choosing a parser generator depends on the grammar class, performance requirements, and error handling needs.

**Comparison of generator features:**

| Generator | Grammar class | Output language | Error handling | Learning curve |
|-----------|--------------|----------------|----------------|----------------|
| YACC/Bison | LALR(1) | C, C++ | Error productions | Steep |
| ANTLR | LL(*) | Java, JS, Python, C# | Automatic recovery | Moderate |
| Tree-sitter | GLR | C (bindings for many) | Error-tolerant | Moderate |
| PEG.js/PEG | PEG | JavaScript | Limited | Easy |
| Nom | Combinator (Rust) | Rust | Custom strategies | Moderate |
| Parsec/Parboiled | Combinator | Haskell, Scala, Java | Custom | Easy-moderate |

**When to use each:**

- **YACC/Bison:** Production compilers, when performance and small binaries matter
- **ANTLR:** DSLs, code generation, multi-language targets
- **Tree-sitter:** IDE integration, syntax highlighting, incremental parsing
- **PEG.js:** Small DSLs in web applications, prototyping
- **Nom:** Embedded parsers in Rust, network protocol parsing, binary format parsing

**Performance characteristics:**

| Generator | Throughput | Memory | Startup time |
|-----------|-----------|--------|-------------|
| Hand-written recursive descent | Highest | Lowest | N/A (compiled) |
| YACC/Bison | High | Low | Instant |
| ANTLR | Medium-high | Medium | Fast |
| Tree-sitter | Medium (incremental) | Medium-high | Fast |
| Combinator library | Medium | Low-medium | N/A (compiled) |

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

**Statistical vs. rule-based parsing:**

Early NLP parsers used hand-written grammar rules and lexicons. Modern parsers use statistical models trained on treebanks (manually parsed corpora). The shift to statistical parsing began in the early 1990s and accelerated with the rise of machine learning. Today, transformer-based models (BERT, RoBERTa) fine-tuned on parsing tasks achieve the highest accuracy, but they require substantial computational resources.

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

**Why POS tagging matters for parsing:**

POS tags disambiguate word roles before syntactic analysis begins. "Run" can be a noun (a morning run) or a verb (they run fast). Knowing the POS tag narrows the parse options. Most dependency and constituency parsers use POS tags as input features.

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

**Transition-based parsing algorithms:**

The most common transition-based system is the arc-standard algorithm, using three actions:

| Action | Description |
|--------|-------------|
| SHIFT | Move word from buffer to stack |
| LEFT-ARC(l) | Add labeled dependency from top to second-top, remove second-top |
| RIGHT-ARC(l) | Add labeled dependency from second-top to top, remove top |

The parser runs in O(n) time, making it suitable for real-time applications.

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

**Neural constituency parsing:**

Modern constituency parsers use span-based representations and self-attention. The parser scores all possible spans in a sentence and selects the highest-scoring tree structure. These models achieve F1 scores above 96% on standard benchmarks like the Penn Treebank.

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

**Why programming languages avoid ambiguity:**

Ambiguity in code means the compiler and the programmer might disagree on what the code does. Programming language designers go to great lengths to ensure that every valid program has exactly one parse tree. This is why LR and LL grammars are preferred for programming languages — they guarantee determinism.

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

**Incremental parsing in IDEs:**

Modern IDEs cannot re-parse the entire file on every keystroke — that would be too slow. Instead, they use incremental parsers that re-parse only the changed region and its surrounding context. Tree-sitter and Roslyn (C#) both support incremental parsing. The parser maintains the previous syntax tree, applies edits as invalidation regions, and re-parses only those regions.

## Learning Tips

- **Start with recursive descent.** It is the easiest parsing algorithm to implement by hand and teaches the core concepts of grammar structure and lookahead. Implement a simple expression parser as a first project.
- **Use grammar visualization tools.** Tools like Grammophone or railroad diagram generators help visualize grammar structure and are excellent for debugging ambiguous grammar rules.
- **Build a calculator first.** Writing a parser for arithmetic expressions with precedence (using either recursive descent or Pratt parsing) gives immediate, testable feedback. Calculator parsers are the "hello world" of parsing.
- **Test with invalid inputs.** Good parsers fail gracefully. Always test your parser with empty input, unexpected characters, partial expressions, and deeply nested structures to verify error recovery.
- **Study real grammars.** Read the grammar files of real languages — JSON, SQL, or a small subset of your favorite language. Understanding how real-world languages are defined helps when designing your own DSLs.
- **Learn one parser generator deeply.** Master ANTLR or Tree-sitter, then branch out. The concepts transfer between tools once you understand grammar classes and table construction.
- **Avoid over-engineering.** For many tasks, a simple hand-written parser is sufficient. Reach for parser generators only when the grammar grows beyond 10-15 rules or when you need cross-language support.
- **Profile before optimizing.** Parsing is rarely the bottleneck in applications. Before micro-optimizing your parser, measure its actual contribution to overall performance.

## Glossary

| Term | Definition |
|------|------------|
| AST | Abstract Syntax Tree — simplified tree representation of code |
| Bottom-up parsing | Builds tree from leaves to root (reduce tokens to productions) |
| Chart (Earley) | Table of Earley items at each input position during Earley parsing |
| Constituency | Phrase structure analysis grouping words into nested constituents |
| CYK algorithm | DP algorithm for parsing CFGs in Chomsky Normal Form |
| Dependency | Binary grammatical relation between head and dependent |
| Earley item | A production rule with a dot position and start pointer in Earley parsing |
| Error production | Grammar rule including the `error` pseudo-terminal for recovery |
| GLR | Generalized LR — extends LR with parallel state exploration for ambiguous grammars |
| GSS | Graph-Structured Stack — data structure used in GLR parsing to represent multiple states |
| Incremental parsing | Re-parsing only the changed region of a file, not the entire input |
| LALR | Look-Ahead LR — pragmatic subset of LR(1) with merged states |
| LAS | Labeled Attachment Score — percentage of words with correct head and dependency label |
| Lexer | Program converting character sequences to tokens |
| LL parser | Top-down parser, left-to-right, leftmost derivation |
| LR parser | Bottom-up parser, left-to-right, rightmost derivation |
| Maximal munch | Lexer strategy: take the longest matching token pattern |
| Packrat parsing | PEG parsing with memoization for linear time performance |
| Parser combinator | Higher-order function composing simple parsers into complex ones |
| PCFG | Probabilistic CFG with rule probabilities |
| PEG | Parsing Expression Grammar — ordered choice, no ambiguity |
| POS tagging | Part-of-speech tagging — labeling words with grammatical categories |
| Pratt parsing | Top-down operator precedence using numeric binding powers |
| Recursive descent | Top-down parsing: each non-terminal becomes a function |
| Shift-reduce | LR parsing actions — shift pushes, reduce pops |
| Token | Classified lexical unit (NUMBER, IDENTIFIER, KEYWORD) |
| Top-down parsing | Builds tree from root to leaves (expand non-terminals) |
| Transition-based parsing | Left-to-right dependency parsing with a stack and buffer |
| Tree-sitter | Incremental, error-tolerant parsing library using GLR |
| UAS | Unlabeled Attachment Score — percentage of words with correct head |

## Quick References

- [Aho, Sethi, Ullman: Compilers: Principles, Techniques, and Tools](https://www.pearson.com/en-us/subject-catalog/p/compilers-principles-techniques-and-tools/P200000003267)
- [ANTLR Mega Tutorial](https://tomassetti.me/antlr-mega-tutorial/)
- [Tree-sitter Documentation](https://tree-sitter.github.io/tree-sitter/)
- [Jurafsky & Martin: Speech and Language Processing](https://web.stanford.edu/~jurafsky/slp3/)
- [spaCy Parser](https://spacy.io/api/parser)
- [Universal Dependencies](https://universaldependencies.org/)
- [Crafting Interpreters (Robert Nystrom)](https://craftinginterpreters.com/)
- [Pratt Parsing: Top Down Operator Precedence](https://tdop.github.io/) — original paper by Vaughan Pratt
- [Earley Parsing Explained](https://loup-vaillant.fr/tutorials/earley-parsing/) — accessible tutorial with examples
- [Parsing Techniques — A Practical Guide](https://dickgrune.com/Books/PTAPG_2nd_Edition/) — comprehensive reference on parsing algorithms
- [Let's Build a Parser (YouTube series)](https://www.youtube.com/playlist?list=PLP29wDx6QmW5yfO1LQAoXH7T0x3uJnP6X) — practical parser construction tutorial
- [Ohm: A New PEG System](https://ohmlang.github.io/) — modular PEG-based parser generator

## Next Steps

- [Semantics](semantics.md) — meaning in language and code, from lexical semantics to type systems
- [Language Models](language-models.md) — language models and linguistic theory, n-grams to transformers
