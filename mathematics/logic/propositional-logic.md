# Propositional Logic

## Description

Propositional logic is the simplest branch of logic, dealing with propositions (statements that are true or false) and how they combine through logical connectives. Every programming language condition, database query, and digital circuit is built on propositional logic. Mastering it gives you a formal understanding of boolean expressions, conditional reasoning, and satisfiability — skills that transfer directly to writing correct code and debugging complex conditions.

## Prerequisites

- [Mathematical Thinking](../intro/mathematical-thinking.md) — comfort with abstraction, notation, and formal definitions
- [Set Theory](../discrete-mathematics/set-theory.md) — basic set operations, subsets, and set-builder notation used in truth sets

## Table of Contents

- [Propositions](#propositions)
- [Logical Connectives](#logical-connectives)
- [Truth Tables](#truth-tables)
- [Tautologies and Contradictions](#tautologies-and-contradictions)
- [Logical Equivalence](#logical-equivalence)
- [De Morgan's Laws](#de-morgans-laws)
- [Normal Forms](#normal-forms)
- [Satisfiability](#satisfiability)
- [Applications](#applications)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Propositions

A **proposition** is a declarative sentence that is either true or false (but not both).

```
Proposition: "2 + 2 = 4"          → true
Proposition: "Every prime is odd"  → false
Not a proposition: "x is even"     → depends on x (open sentence)
Not a proposition: "Is this true?" → question, not declarative
```

In programming, propositions correspond to boolean expressions:

```python
# Propositions in Python
x > 0                  # true or false depending on x
len(s) == 0            # true or false
is_authenticated       # a boolean variable is itself a proposition
```

**Atomic propositions** are the simplest kind — they contain no logical connectives. We usually denote them with lowercase letters: $p$, $q$, $r$, $p_1$, $p_2$.

**Compound propositions** are formed by combining atomic propositions with logical connectives.

### Logical Connectives

The five fundamental connectives in classical propositional logic:

| Name | Symbol | Reads as | Arity |
|------|--------|----------|-------|
| Negation | $\neg$ | not | unary |
| Conjunction | $\land$ | and | binary |
| Disjunction | $\lor$ | or | binary |
| Implication | $\to$ (or $\implies$) | implies / if...then | binary |
| Biconditional | $\leftrightarrow$ (or $\iff$) | iff / if and only if | binary |

**Negation ($\neg p$)** — flips truth value:

```python
def neg(p: bool) -> bool:
    return not p
```

**Conjunction ($p \land q$)** — true exactly when both operands are true:

```python
def conj(p: bool, q: bool) -> bool:
    return p and q
```

In short-circuit evaluation (C, Java, Python), `p and q` does not evaluate `q` if `p` is false. This is an operational detail, but the truth table remains the same.

**Disjunction ($p \lor q$)** — true when at least one operand is true (inclusive or):

```python
def disj(p: bool, q: bool) -> bool:
    return p or q
```

**Implication ($p \to q$)** — false only when $p$ is true and $q$ is false. This is the most counter-intuitive connective for beginners.

$$p \to q \equiv \neg p \lor q$$

```python
def implies(p: bool, q: bool) -> bool:
    return not p or q
```

**Biconditional ($p \leftrightarrow q$)** — true when both operands have the same truth value:

$$p \leftrightarrow q \equiv (p \to q) \land (q \to p) \equiv p = q$$

```python
def iff(p: bool, q: bool) -> bool:
    return p == q
```

### Truth Tables

A truth table enumerates every possible assignment of truth values to variables and shows the result.

**Truth table for the five connectives:**

| $p$ | $q$ | $\neg p$ | $p \land q$ | $p \lor q$ | $p \to q$ | $p \leftrightarrow q$ |
|-----|-----|----------|-------------|------------|-----------|----------------------|
| F | F | T | F | F | T | T |
| F | T | T | F | T | T | F |
| T | F | F | F | T | F | F |
| T | T | F | T | T | T | T |

A formula with $n$ variables requires $2^n$ rows.

**Compound truth table for $(p \lor q) \to (\neg p \land r)$:**

| $p$ | $q$ | $r$ | $p \lor q$ | $\neg p$ | $\neg p \land r$ | $(p \lor q) \to (\neg p \land r)$ |
|-----|-----|-----|-----------|----------|-----------------|----------------------------------|
| F | F | F | F | T | F | T |
| F | F | T | F | T | T | F |
| F | T | F | T | T | F | F |
| F | T | T | T | T | T | T |
| T | F | F | T | F | F | F |
| T | F | T | T | F | F | F |
| T | T | F | T | F | F | F |
| T | T | T | T | F | F | F |

### Tautologies and Contradictions

A **tautology** is a formula that evaluates to true for every assignment. A **contradiction** evaluates to false for every assignment.

**Example of a tautology:** $p \lor \neg p$ (law of excluded middle)

| $p$ | $\neg p$ | $p \lor \neg p$ |
|-----|----------|----------------|
| F | T | T |
| T | F | T |

**Example of a contradiction:** $p \land \neg p$

| $p$ | $\neg p$ | $p \land \neg p$ |
|-----|----------|-----------------|
| F | T | F |
| T | F | F |

A formula that is neither a tautology nor a contradiction is a **contingency** — it is true for some assignments and false for others.

**Common tautologies:**

| Name | Formula |
|------|---------|
| Law of Excluded Middle | $p \lor \neg p$ |
| Law of Non-Contradiction | $\neg(p \land \neg p)$ |
| Identity | $p \to p$ |
| Double Negation | $p \leftrightarrow \neg\neg p$ |
| Implication Rewrite | $(p \to q) \leftrightarrow (\neg p \lor q)$ |
| Modus Ponens | $(p \land (p \to q)) \to q$ |
| Modus Tollens | $(\neg q \land (p \to q)) \to \neg p$ |
| Hypothetical Syllogism | $((p \to q) \land (q \to r)) \to (p \to r)$ |

In program verification, tautologies correspond to always-true assertions. A loop invariant that is a tautology is trivially true but also useless — you want invariants that are contingencies (true for valid states, false for invalid ones).

### Logical Equivalence

Two formulas $A$ and $B$ are **logically equivalent** ($A \equiv B$) when $A \leftrightarrow B$ is a tautology — they produce the same truth value for every assignment.

**Fundamental equivalences:**

| Name | Rule |
|------|------|
| Double Negation | $\neg\neg p \equiv p$ |
| Commutativity | $p \land q \equiv q \land p$, $p \lor q \equiv q \lor p$ |
| Associativity | $(p \land q) \land r \equiv p \land (q \land r)$, $(p \lor q) \lor r \equiv p \lor (q \lor r)$ |
| Distributivity | $p \land (q \lor r) \equiv (p \land q) \lor (p \land r)$, $p \lor (q \land r) \equiv (p \lor q) \land (p \lor r)$ |
| Identity | $p \land \top \equiv p$, $p \lor \bot \equiv p$ |
| Domination | $p \lor \top \equiv \top$, $p \land \bot \equiv \bot$ |
| Idempotence | $p \land p \equiv p$, $p \lor p \equiv p$ |
| Absorption | $p \lor (p \land q) \equiv p$, $p \land (p \lor q) \equiv p$ |
| De Morgan | $\neg(p \land q) \equiv \neg p \lor \neg q$, $\neg(p \lor q) \equiv \neg p \land \neg q$ |
| Implication | $p \to q \equiv \neg p \lor q$ |
| Biconditional | $p \leftrightarrow q \equiv (p \to q) \land (q \to p)$ |
| Exportation | $(p \land q) \to r \equiv p \to (q \to r)$ |

These equivalences are the algebraic rules for rewriting boolean expressions in code.

```python
# Logical equivalence example: not (x > 0 and y < 10)
# By De Morgan: not (x > 0) or not (y < 10)
# Equivalently: x <= 0 or y >= 10

def original(x, y):
    return not (x > 0 and y < 10)

def equivalent(x, y):
    return x <= 0 or y >= 10

# Same truth table for all x, y
```

### De Morgan's Laws

De Morgan's laws are the most practically useful equivalences in programming:

$$\neg(p \land q) \equiv \neg p \lor \neg q$$
$$\neg(p \lor q) \equiv \neg p \land \neg q$$

**In natural language:**

- "Not (A and B)" = "Not A or not B"
- "Not (A or B)" = "Not A and not B"

**In code:**

```python
# De Morgan's first law: not (A and B) == not A or not B
# Before:
if not (user.is_active and user.has_permission):
    raise Forbidden

# After (equivalent):
if not user.is_active or not user.has_permission:
    raise Forbidden
```

```python
# De Morgan's second law: not (A or B) == not A and not B
# Before:
if not (x < 0 or x > 100):
    print("In range")

# After (equivalent):
if x >= 0 and x <= 100:
    print("In range")
```

**Generalized De Morgan:**

$$\neg(p_1 \land p_2 \land \dots \land p_n) \equiv \neg p_1 \lor \neg p_2 \lor \dots \lor \neg p_n$$
$$\neg(p_1 \lor p_2 \lor \dots \lor p_n) \equiv \neg p_1 \land \neg p_2 \land \dots \land \neg p_n$$

In hardware, De Morgan's laws are used to convert AND-OR circuits into NAND-NAND or NOR-NOR circuits, since NAND and NOR are universal gates.

### Normal Forms

Every propositional formula can be rewritten into equivalent standard forms.

**Literal:** a variable ($p$) or its negation ($\neg p$).

**Clause:** a disjunction of literals ($p \lor \neg q \lor r$).

**Term:** a conjunction of literals ($p \land \neg q \land r$).

#### Disjunctive Normal Form (DNF)

A formula is in DNF when it is a disjunction of conjunctions of literals:

$$(p \land q) \lor (\neg p \land r) \lor (p \land \neg q \land \neg r)$$

**Building DNF from a truth table:** take every row where the formula is true, form a conjunction that matches that row, and OR them all together.

```python
# A function returns true for exactly (T,T) and (F,T):
# DNF: (p AND q) OR (NOT p AND q)
def dnf_example(p, q):
    return (p and q) or (not p and q)
# Simplifies to: q
```

#### Conjunctive Normal Form (CNF)

A formula is in CNF when it is a conjunction of disjunctions of literals:

$$(p \lor q) \land (\neg p \lor r) \land (p \lor \neg q \lor \neg r)$$

**Building CNF from a truth table:** take every row where the formula is false, form a disjunction that excludes that row (by negating the row's assignment), and AND them all together.

**Why CNF matters:** The SAT problem (satisfiability) is defined as: given a CNF formula, does there exist an assignment making it true? Modern SAT solvers operate exclusively on CNF.

```python
# CNF transformation step for implications
# (p -> q) becomes (NOT p OR q)
# For (p -> q) AND (q -> r), CNF is:
# (NOT p OR q) AND (NOT q OR r)

def cnf_example(p, q, r):
    return (not p or q) and (not q or r)
```

#### Conversion Algorithm (CNF)

1. Eliminate $\leftrightarrow$: replace $p \leftrightarrow q$ with $(p \to q) \land (q \to p)$
2. Eliminate $\to$: replace $p \to q$ with $\neg p \lor q$
3. Push negations inward using De Morgan's laws until they only apply to literals
4. Distribute $\lor$ over $\land$ using distributivity

**Example:** Convert $(p \to q) \land \neg(q \lor r)$ to CNF.

Step 1: $(\neg p \lor q) \land \neg(q \lor r)$ (eliminate $\to$)

Step 2: $(\neg p \lor q) \land (\neg q \land \neg r)$ (De Morgan)

Step 3: $(\neg p \lor q) \land \neg q \land \neg r$ (already in CNF: conjunction of clauses)

### Satisfiability

A formula is **satisfiable** if there exists at least one assignment of truth values to its variables that makes it true. This assignment is called a **satisfying assignment** or **model**.

The **Boolean Satisfiability Problem (SAT)** asks: given a propositional formula, is it satisfiable?

- A tautology is always satisfiable (every assignment works).
- A contradiction is unsatisfiable.
- A contingency is satisfiable (some assignments work).

SAT was the first problem proven to be NP-complete (Cook-Levin theorem, 1971). Despite its theoretical hardness, modern SAT solvers handle formulas with millions of variables using techniques like:

- **Unit propagation:** if a clause has a single literal, set that literal to true.
- **Pure literal elimination:** if a variable appears only positively (or only negatively), set it to true (or false).
- **Conflict-driven clause learning (CDCL):** when a conflict is reached, learn a new clause to avoid exploring the same dead end.
- **DPLL algorithm:** Davis-Putnam-Logemann-Loveland, the basis of most SAT solvers.

```python
# A simple SAT check for a CNF formula using brute force
# Formula: (p OR q) AND (NOT p OR q) AND (p OR NOT q)

def is_satisfiable():
    for p in [False, True]:
        for q in [False, True]:
            clause1 = p or q
            clause2 = (not p) or q
            clause3 = p or (not q)
            if clause1 and clause2 and clause3:
                return (p, q)
    return None

print(is_satisfiable())  # (True, True)
```

### Applications

#### Boolean Search

Search engines and document retrieval systems use propositional logic to express queries:

```
"cat AND dog"        → documents containing both terms
"cat OR dog"         → documents containing either term
"cat AND NOT dog"    → documents with "cat" but not "dog"
"(java OR python) AND (compiler OR interpreter)"
```

In Elasticsearch, Lucene, and SQL full-text search, these boolean operators are direct implementations of logical connectives.

#### SQL WHERE Conditions

Every SQL WHERE clause is a propositional formula evaluated per row:

```sql
SELECT * FROM users
WHERE (age > 18 AND country = 'US')
   OR (age > 21 AND country = 'CA');
```

The database evaluates this formula row by row. Understanding logical equivalence helps you simplify queries without changing results:

```sql
-- Original
WHERE NOT (status = 'active' OR role = 'admin')

-- Equivalent (De Morgan)
WHERE status != 'active' AND role != 'admin'
```

The second form is often faster because the database can use independent indexes on `status` and `role`.

#### Digital Circuits

Logic gates implement propositional connectives directly:

```
AND gate  → p ∧ q
OR gate   → p ∨ q
NOT gate  → ¬p
NAND gate → ¬(p ∧ q)
NOR gate  → ¬(p ∨ q)
XOR gate  → p ⊕ q (exclusive or)
```

**Half adder circuit:**

$$Sum = p \oplus q$$
$$Carry = p \land q$$

```
p ──┬─XOR── Sum
    └─AND── Carry
q ──┘
```

**Full adder** chains half adders to handle carry-in. Modern CPUs use optimized CNF-based representations for circuit verification.

#### Program Assertions and Verification

```python
# Precondition and postcondition as propositional formulas
def divide(a: int, b: int) -> float:
    # Precondition: b != 0
    assert b != 0
    result = a / b
    # Postcondition: result * b == a
    assert abs(result * b - a) < 1e-9
    return result
```

In formal verification tools like Dafny, Frama-C, or why3, preconditions and postconditions are propositional (and first-order) formulas checked by automated solvers.

#### Conditional Logic in Code

Complex conditionals can be simplified using logical equivalences:

```python
# Complex nested condition
if x > 0:
    if y > 0:
        do_something()

# Equivalent flat condition (p AND q)
if x > 0 and y > 0:
    do_something()
```

```python
# Guard clause simplification
# Original:
if not (error is None or retry_count >= 3):
    handle()

# De Morgan:
if error is not None and retry_count < 3:
    handle()
```

## Glossary

| Term | Definition |
|------|------------|
| Proposition | A declarative sentence that is either true or false |
| Atomic proposition | A proposition that contains no logical connectives |
| Compound proposition | A proposition formed by combining atomic propositions with connectives |
| Logical connective | An operator that combines propositions (and, or, not, implies, iff) |
| Truth table | A table enumerating all truth assignments and their results for a formula |
| Tautology | A formula that is true for every assignment of its variables |
| Contradiction | A formula that is false for every assignment of its variables |
| Contingency | A formula that is neither a tautology nor a contradiction |
| Logical equivalence | Two formulas that have the same truth value for all assignments |
| Literal | A variable or its negation |
| Clause | A disjunction of literals |
| Term | A conjunction of literals |
| Conjunctive Normal Form (CNF) | A conjunction of clauses |
| Disjunctive Normal Form (DNF) | A disjunction of terms |
| Satisfiability | Whether there exists an assignment making a formula true |
| SAT | The Boolean Satisfiability Problem |
| Model | A satisfying assignment for a formula |
| DPLL | Davis-Putnam-Logemann-Loveland algorithm for SAT solving |
| CDCL | Conflict-Driven Clause Learning, the basis of modern SAT solvers |

## Quick References

- [Logic in Computer Science, Huth & Ryan](https://www.cambridge.org/highereducation/books/logic-in-computer-science/C791B0C0A8B6A2A1D5CBDAF5B3614A0E) — accessible textbook with programming focus
- [SAT Competition Results](http://satcompetition.org/) — benchmark of modern SAT solvers
- [Z3 Theorem Prover](https://github.com/Z3Prover/z3) — Microsoft's SMT solver
- [MiniSAT](http://minisat.se/) — minimal, efficient SAT solver implementation
- [Boolean Algebra at Wikipedia](https://en.wikipedia.org/wiki/Boolean_algebra) — reference for all laws and identities

## Next Steps

- [Predicate Logic](predicate-logic.md) — adds quantifiers and predicates, enabling richer logical statements
- [Proof Systems](proof-systems.md) — formal rules for deriving truths from axioms
- [Boolean Algebra & Logic Circuits](boolean-algebra.md) — algebraic view of propositional logic, optimization, and circuit design
