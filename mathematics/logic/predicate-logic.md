# Predicate Logic

## Description

Predicate logic (also called first-order logic) extends propositional logic with predicates, variables, and quantifiers, enabling statements about "all objects" and "some objects." It is the language of database queries (SQL), formal specifications (TLA+, Z), type systems, and automated reasoning. Understanding predicate logic is essential for anyone writing correct software that deals with collections, invariants, or formal contracts.

## Prerequisites

- [Propositional Logic](propositional-logic.md) — propositions, connectives, truth tables, logical equivalence

## Table of Contents

- [From Propositions to Predicates](#from-propositions-to-predicates)
- [Quantifiers](#quantifiers)
- [Free and Bound Variables](#free-and-bound-variables)
- [Scope](#scope)
- [Nested Quantifiers](#nested-quantifiers)
- [Quantifier Negation](#quantifier-negation)
- [Equality](#equality)
- [Functions and Relations](#functions-and-relations)
- [Applications](#applications)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### From Propositions to Predicates

A **predicate** is a statement that contains variables and becomes a proposition when the variables are given specific values.

$$P(x): x > 0$$

$P(x)$ is not a proposition — it has no truth value until $x$ is fixed. But $P(5)$ is true, and $P(-2)$ is false.

**Predicates as functions:**

```python
def P(x):
    return x > 0     # returns a boolean

print(P(5))    # True
print(P(-2))   # False
```

**A predicate with multiple arguments:**

$$Loves(x, y): x \text{ loves } y$$

```python
def loves(x, y):
    return x in loves_relation.get(y, set())
```

In programming, predicates appear everywhere:
- `is_empty(xs)` — unary predicate
- `x < y` — binary predicate (less-than relation)
- `is_between(x, a, b)` — ternary predicate

The **domain** (or universe of discourse) is the set of values that variables can take. In programming, the domain might be `int`, `string`, `User`, or any type.

### Quantifiers

Quantifiers turn open predicates into propositions by specifying how many objects satisfy the predicate.

#### Universal Quantifier ($\forall$)

$\forall x P(x)$ means "$P(x)$ is true for every $x$ in the domain."

$$\forall x (x \ge 0 \to \sqrt{x} \text{ is real})$$

In programming:

```python
def all_positive(xs):
    """∀x ∈ xs: x > 0"""
    return all(x > 0 for x in xs)
```

```python
# In list comprehensions:
all(x > 0 for x in numbers)     # ∀x ∈ numbers: x > 0
```

#### Existential Quantifier ($\exists$)

$\exists x P(x)$ means "there exists at least one $x$ in the domain such that $P(x)$ is true."

$$\exists x (x > 0 \land x^2 = 4)$$

```python
def has_negative(xs):
    """∃x ∈ xs: x < 0"""
    return any(x < 0 for x in xs)
```

#### Finite Domain Quantification

When the domain is finite $\{a_1, a_2, \dots, a_n\}$:

$$\forall x P(x) \equiv P(a_1) \land P(a_2) \land \dots \land P(a_n)$$
$$\exists x P(x) \equiv P(a_1) \lor P(a_2) \lor \dots \lor P(a_n)$$

This reveals why $\forall$ is a generalized conjunction and $\exists$ is a generalized disjunction.

```python
# Universal quantification over a finite set
def forall(domain, predicate):
    return all(predicate(x) for x in domain)

# Existential quantification over a finite set
def exists(domain, predicate):
    return any(predicate(x) for x in domain)
```

### Free and Bound Variables

A variable is **bound** when it falls under the scope of a quantifier. A variable is **free** otherwise.

$$\underbrace{\forall x}_{x \text{ bound}} P(x, \underbrace{y}_{y \text{ free}})$$

- In $\forall x (x > y)$, $x$ is bound, $y$ is free.
- In $(\forall x P(x)) \land Q(x)$, the $x$ in $P(x)$ is bound, the $x$ in $Q(x)$ is free (different variables with the same name — confusing and should be avoided).
- In $\forall x \exists y R(x, y)$, both $x$ and $y$ are bound.

A formula with no free variables is a **sentence** (or closed formula). Only sentences have definite truth values.

```python
def has_free_vars(formula, bound_vars):
    """Check if formula has free variables (conceptual)."""
    # In practice, you'd parse the AST
    pass

# In programming, free variables appear in closures:
def make_predicate(threshold):
    # threshold is free in the inner function
    def predicate(x):
        return x > threshold    # threshold is free, x is bound
    return predicate
```

**Renaming bound variables:** Bound variables can be renamed consistently without changing meaning:

$$\forall x P(x) \equiv \forall y P(y)$$

But renaming must avoid variable capture:

# Correct:
$$\forall x (x > y) \equiv \forall z (z > y)$$

# Incorrect (captures $y$):
$$\forall x (x > y) \not\equiv \forall y (y > y)$$

### Scope

The **scope** of a quantifier is the part of the formula where the quantified variable is bound.

$$\forall x \underbrace{(P(x) \to Q(x))}_{\text{scope of }\forall x}$$

Parentheses matter for scope:

$$\forall x P(x) \to Q(x)$$

Here, the scope of $\forall x$ is only $P(x)$. The $x$ in $Q(x)$ is free. This is almost always a mistake.

**Proper scoping:**

```python
# In Python comprehensions, scope is clear:
result = [x * 2 for x in items]   # x bound to the comprehension
# x is not accessible outside

# Lambda calculus scoping:
f = lambda x: x + 1     # x is bound by lambda
# f(5) applies the function, x is scoped to lambda body
```

### Nested Quantifiers

When quantifiers are nested, the order matters.

#### Same Quantifier Order

$\forall x \forall y P(x, y)$ and $\forall y \forall x P(x, y)$ are equivalent. Same for $\exists x \exists y$ and $\exists y \exists x$.

#### Mixed Quantifiers

$\forall x \exists y P(x, y)$ means: "For every $x$, there exists some $y$ (possibly depending on $x$) such that $P(x, y)$ holds."

$\exists y \forall x P(x, y)$ means: "There exists a single $y$ such that for every $x$, $P(x, y)$ holds."

These are **not equivalent**.

$$\forall x \exists y (y > x) \quad \text{(true in natural numbers)}$$

$$\exists y \forall x (y > x) \quad \text{(false in natural numbers)}$$

**Intuition:** $\forall x \exists y$ is like a function from $x$ to $y$; $\exists y \forall x$ requires a constant $y$ that works for all $x$.

```python
# ∀x ∈ xs: ∃y ∈ ys: y > x
def all_have_greater(xs, ys):
    return all(any(y > x for y in ys) for x in xs)

# ∃y ∈ ys: ∀x ∈ xs: y > x
def has_greater_than_all(xs, ys):
    return any(all(y > x for x in xs) for y in ys)
```

**Common Nested Patterns:**

| Formula | Meaning | Example |
|---------|---------|---------|
| $\forall x \forall y P(x,y)$ | P holds for all pairs | Everyone knows everyone |
| $\exists x \exists y P(x,y)$ | P holds for some pair | Someone knows someone |
| $\forall x \exists y P(x,y)$ | Each x has a matching y | Everyone has a mother |
| $\exists y \forall x P(x,y)$ | One y matches all x | There's a universal mother |
| $\exists x \forall y P(x,y)$ | One x matches all y | Someone is known by everyone |
| $\forall y \exists x P(x,y)$ | Each y has a matching x | Everyone has an acquaintance |

### Quantifier Negation

The negation rules for quantifiers mirror De Morgan's laws:

$$\neg \forall x P(x) \equiv \exists x \neg P(x)$$
$$\neg \exists x P(x) \equiv \forall x \neg P(x)$$

**In natural language:**

- "Not everyone is here" = "Someone is not here"
- "No one is here" = "Everyone is not here"

**In programming:**

```python
# Negating a universal assertion
# not all(x > 0 for x in xs)  ==  any(x <= 0 for x in xs)

def not_all_positive(xs):
    return not all(x > 0 for x in xs)

def some_not_positive(xs):
    return any(x <= 0 for x in xs)
# Equivalent!
```

**Pushing negation through nested quantifiers:**

$$\neg \forall x \exists y P(x, y) \equiv \exists x \neg \exists y P(x, y) \equiv \exists x \forall y \neg P(x, y)$$

Pattern: each quantifier flips ($\forall \leftrightarrow \exists$) and the negation moves to the predicate.

```python
# A real-world example: negating a specification
# Spec: "For every request, there exists a response"
# ¬(∀request ∃response replied(request, response))
# = ∃request ¬∃response replied(request, response)
# = ∃request ∀response ¬replied(request, response)
# = "There is a request with no response"

def spec_violated(requests, responses):
    """Check if spec 'every request has a response' is violated."""
    return any(
        all(not replied(req, resp) for resp in responses)
        for req in requests
    )
```

### Equality

Equality ($=$) is a special binary predicate with fixed semantics:

$$x = y \quad \text{means } x \text{ and } y \text{ are the same object}$$

**Properties of equality:**

| Property | Formula |
|----------|---------|
| Reflexivity | $\forall x (x = x)$ |
| Symmetry | $\forall x \forall y (x = y \to y = x)$ |
| Transitivity | $\forall x \forall y \forall z ((x = y \land y = z) \to x = z)$ |
| Substitution | $\forall x \forall y (x = y \to (P(x) \to P(y)))$ |

**In programming, equality is fundamental:**

```python
# Structural equality vs identity
a = [1, 2, 3]
b = [1, 2, 3]

a == b    # True: structural equality (∀ elements are equal)
a is b    # False: not the same object
```

In formal verification and databases, equality allows expressing uniqueness constraints:

$$\forall x \forall y (\text{Employee}(x) \land \text{Employee}(y) \land x \neq y \to \text{ssn}(x) \neq \text{ssn}(y))$$

### Functions and Relations

In predicate logic, we distinguish:

- **Constants:** specific objects ($0, \text{null}, \text{True}$)
- **Functions:** map objects to objects ($\text{succ}(x), \text{parentOf}(x), \text{length}(s)$)
- **Predicates (Relations):** map objects to truth values ($x < y, \text{isPrime}(x)$)

**Signature (vocabulary):** A tuple $(C, F, R, arity)$ specifying the constants, function symbols, relation symbols, and their arities.

```
Signature for arithmetic:
Constants:  {0, 1}
Functions: {+ / 2, * / 2, succ / 1}
Relations: {< / 2, = / 2, isPrime / 1}
```

**Terms are built from constants, variables, and functions:**

$$\text{succ}(0) + \text{succ}(\text{succ}(0))$$

**Atomic formulas are predicates applied to terms:**

$$x + y < z \times 2$$
$$x = \text{succ}(y)$$

**Formulas** combine atomic formulas with connectives and quantifiers.

```python
# Functions and predicates in code
def age_of(person):
    # function symbol
    pass

def is_senior(person):
    # predicate symbol
    return age_of(person) >= 65

def all_employees_have_ids(employees, ids):
    """∀e ∈ employees: ∃i ∈ ids: id_of(e) = i"""
    return all(any(id_of(e) == i for i in ids) for e in employees)
```

### Applications

#### Database Queries (SQL)

SQL is essentially a syntactic variant of predicate logic:

```sql
-- Predicate logic: {x | Employee(x) ∧ Department(x, 'Engineering')}
SELECT * FROM Employee e
WHERE e.department = 'Engineering';

-- Existential: {x | Employee(x) ∧ ∃p (Project(p) ∧ works_on(x, p))}
SELECT * FROM Employee e
WHERE EXISTS (
    SELECT 1 FROM Project p
    WHERE p.manager_id = e.id
);

-- Universal via NOT EXISTS (∀x P(x) = ¬∃x ¬P(x)):
-- Employees who work on ALL projects:
SELECT * FROM Employee e
WHERE NOT EXISTS (
    SELECT 1 FROM Project p
    WHERE NOT EXISTS (
        SELECT 1 FROM WorksOn w
        WHERE w.emp_id = e.id AND w.project_id = p.id
    )
);
```

**Relational algebra** is grounded in predicate logic:
- Selection ($\sigma$) corresponds to predicate filtering
- Projection corresponds to existential quantification over dropped columns
- Join corresponds to conjunction of predicates from two tables

#### Formal Specifications (TLA+, Z)

Formal specification languages use predicate logic to describe system behavior:

```tla+
(* TLA+ specification of a simple counter *)
VARIABLES counter
Init == counter = 0
Next == counter' = counter + 1
Spec == Init ∧ □[Next]_counter
```

The TLA+ model checker (TLC) translates specifications into propositional/quantified formulas and uses SAT-based techniques to explore state spaces.

```z
-- Z notation: a birthday book
BirthdayBook
known: P NAME
birthday: NAME → DATE

∀ n: known • birthday(n) ∈ DATE
```

#### Type Systems

Type judgments in programming languages use quantifiers:

$$\frac{\Gamma \vdash e_1 : \text{Int} \quad \Gamma \vdash e_2 : \text{Int}}{\Gamma \vdash e_1 + e_2 : \text{Int}}$$

$$\frac{\Gamma[x \mapsto T] \vdash e : U}{\Gamma \vdash \lambda x : T. e : T \to U}$$

Polymorphism uses $\forall$:

$$\text{id} : \forall \alpha. \alpha \to \alpha$$

This reads as: "for all types $\alpha$, id has type $\alpha \to \alpha$."

```python
# In Python, type variables express universal quantification
from typing import TypeVar

T = TypeVar('T')

def identity(x: T) -> T:
    return x
# Type: ∀T. T → T
```

#### Formal Verification

Predicate logic is the language of preconditions, postconditions, and invariants:

```python
def binary_search(arr: list[int], target: int) -> int:
    """Find target in sorted arr.
    
    Precondition:
        ∀i, j (0 ≤ i < j < len(arr) → arr[i] ≤ arr[j])  # sorted
        
    Postcondition:
        result = -1 → ∀i (0 ≤ i < len(arr) → arr[i] ≠ target)
        result ≥ 0 → arr[result] = target
    """
    ...
```

Tools like Dafny, Frama-C, and Why3 translate these annotations to verification conditions in predicate logic and discharge them with SMT solvers.

## Glossary

| Term | Definition |
|------|------------|
| Predicate | A statement containing variables that becomes a proposition when variables are assigned |
| Domain | The set of objects over which variables range |
| Universal quantifier ($\forall$) | Means "for all" — the predicate holds for every element of the domain |
| Existential quantifier ($\exists$) | Means "there exists" — the predicate holds for at least one element |
| Bound variable | A variable under the scope of a quantifier |
| Free variable | A variable not bound by any quantifier |
| Sentence | A formula with no free variables |
| Scope | The syntactic region where a quantifier binds its variable |
| Signature | The set of constant, function, and relation symbols used in a language |
| Term | An expression built from constants, variables, and function symbols |
| Atomic formula | A predicate applied to terms (including equality) |
| Substitution | Replacing a variable with a term | $P(t/x)$ |
| Quantifier negation | The rule $\neg \forall \equiv \exists \neg$ and $\neg \exists \equiv \forall \neg$ |
| Capture | When a variable accidentally becomes bound after substitution |

## Quick References

- [First-Order Logic (Stanford Encyclopedia)](https://plato.stanford.edu/entries/logic-firstorder/) — comprehensive reference
- [TLA+ Homepage](https://lamport.azurewebsites.net/tla/tla.html) — Leslie Lamport's formal specification language
- [Z Notation](https://en.wikipedia.org/wiki/Z_notation) — formal specification language based on ZF set theory and FOL
- [Logic in Computer Science, Huth & Ryan](https://www.cambridge.org/highereducation/books/logic-in-computer-science/C791B0C0A8B6A2A1D5CBDAF5B3614A0E) — chapters on predicate logic and verification

## Next Steps

- [Proof Systems](proof-systems.md) — formal deduction rules for reasoning with quantifiers
- [First-Order Logic](first-order-logic.md) — deeper semantics, models, compactness, and completeness
