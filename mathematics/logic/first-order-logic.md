# First-Order Logic

## Description

First-order logic (FOL) extends propositional logic with predicates, quantifiers, functions, and equality to form the most widely studied logical system. It is the foundation of database theory, formal verification, axiomatic set theory, and knowledge representation. This document covers the syntax, semantics, and metatheory of FOL — structures, models, completeness, compactness, and the limitations that motivate higher-order logics.

## Prerequisites

- [Predicate Logic](predicate-logic.md) — predicates, quantifiers, free/bound variables, nested quantifiers
- [Proof Systems](proof-systems.md) — natural deduction, sequent calculus, resolution, soundness and completeness concepts

## Table of Contents

- [Syntax of First-Order Logic](#syntax-of-first-order-logic)
- [Semantics: Structures and Models](#semantics-structures-and-models)
- [Satisfiability and Validity](#satisfiability-and-validity)
- [Godel's Completeness Theorem](#godels-completeness-theorem)
- [Compactness Theorem](#compactness-theorem)
- [Lowenheim-Skolem Theorem](#lowenheim-skolem-theorem)
- [Limitations of First-Order Logic](#limitations-of-first-order-logic)
- [Applications](#applications)
- [FOL in Programming: SQL, Prolog, and Type Systems](#fol-in-programming-sql-prolog-and-type-systems)
- [Common Pitfalls in FOL](#common-pitfalls-in-fol)
- [Learning Tips](#learning-tips)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Syntax of First-Order Logic

A **first-order language** is defined by a **signature** (or vocabulary):

$$\Sigma = (\mathcal{C}, \mathcal{F}, \mathcal{R})$$

Where:
- $\mathcal{C}$ is a set of **constant symbols** ($c, d, c_1, c_2$)
- $\mathcal{F}$ is a set of **function symbols**, each with an arity $n \ge 1$ ($f, g, +, \times$)
- $\mathcal{R}$ is a set of **relation symbols** (predicates), each with an arity $n \ge 1$ ($R, P, <, =$)
- The equality symbol $=$ is typically included by default

**Terms** are defined recursively:

1. Every variable ($x, y, z, x_1$) is a term.
2. Every constant symbol is a term.
3. If $f$ is an $n$-ary function symbol and $t_1, \dots, t_n$ are terms, then $f(t_1, \dots, t_n)$ is a term.

**Formulas** are defined recursively:

1. If $t_1, t_2$ are terms, $t_1 = t_2$ is a formula (atomic).
2. If $R$ is an $n$-ary relation symbol and $t_1, \dots, t_n$ are terms, $R(t_1, \dots, t_n)$ is a formula (atomic).
3. If $\phi$ is a formula, $\neg \phi$ is a formula.
4. If $\phi, \psi$ are formulas, $(\phi \land \psi), (\phi \lor \psi), (\phi \to \psi), (\phi \leftrightarrow \psi)$ are formulas.
5. If $\phi$ is a formula and $x$ is a variable, $\forall x \phi$ and $\exists x \phi$ are formulas.

**Example signature for arithmetic:**

$$\mathcal{C} = \{0, 1\}$$
$$\mathcal{F} = \{+/2, \times/2, \text{succ}/1\}$$
$$\mathcal{R} = \{</2, =/2\}$$

**Example formulas:**

$$\forall x \forall y (x + y = y + x) \quad \text{(commutativity of addition)}$$
$$\forall x (\neg (x = 0) \to \exists y (x = \text{succ}(y))) \quad \text{(every non-zero has a predecessor)}$$
$$\neg \exists x (x \times 0 = 1) \quad \text{(no multiplicative inverse of zero)}$$

**Substitution:** $\phi[t/x]$ means "replace all free occurrences of $x$ in $\phi$ with term $t$."

**Capture-avoiding substitution:** If $t$ contains a variable $y$ that would become bound after substitution, we must rename the bound variable first.

$$\forall y (x < y) [y/x] \quad \text{must become} \quad \forall z (y < z)$$

### Semantics: Structures and Models

A **structure** $\mathcal{A}$ (or interpretation) for a signature $\Sigma$ consists of:

1. A nonempty **domain** $|\mathcal{A}|$ (also written $A$).
2. For each constant $c \in \mathcal{C}$, an element $c^\mathcal{A} \in |\mathcal{A}|$.
3. For each $n$-ary function symbol $f \in \mathcal{F}$, a function $f^\mathcal{A}: |\mathcal{A}|^n \to |\mathcal{A}|$.
4. For each $n$-ary relation symbol $R \in \mathcal{R}$, a relation $R^\mathcal{A} \subseteq |\mathcal{A}|^n$.

**Example:** The standard structure of arithmetic $\mathcal{N}$:

$$|\mathcal{N}| = \mathbb{N} = \{0, 1, 2, \dots\}$$
$$0^\mathcal{N} = 0$$
$$+^\mathcal{N} = \text{standard addition on } \mathbb{N}$$
$$\times^\mathcal{N} = \text{standard multiplication on } \mathbb{N}$$
$$<^\mathcal{N} = \{(a, b) \in \mathbb{N}^2 \mid a < b\}$$

A **variable assignment** $s: \text{Vars} \to |\mathcal{A}|$ maps variables to elements of the domain.

**Interpretation of terms:** Given assignment $s$:

$$[x]^{\mathcal{A}, s} = s(x)$$
$$[c]^{\mathcal{A}, s} = c^\mathcal{A}$$
$$[f(t_1, \dots, t_n)]^{\mathcal{A}, s} = f^\mathcal{A}([t_1]^{\mathcal{A}, s}, \dots, [t_n]^{\mathcal{A}, s})$$

**Satisfaction ($\mathcal{A} \models \phi[s]$):** defined by induction on formulas:

| Formula | $\mathcal{A} \models \phi[s]$ iff |
|---------|-------------------------------|
| $t_1 = t_2$ | $[t_1]^{\mathcal{A},s} = [t_2]^{\mathcal{A},s}$ |
| $R(t_1, \dots, t_n)$ | $([t_1]^{\mathcal{A},s}, \dots, [t_n]^{\mathcal{A},s}) \in R^\mathcal{A}$ |
| $\neg \phi$ | $\mathcal{A} \not\models \phi[s]$ |
| $\phi \land \psi$ | $\mathcal{A} \models \phi[s]$ and $\mathcal{A} \models \psi[s]$ |
| $\phi \lor \psi$ | $\mathcal{A} \models \phi[s]$ or $\mathcal{A} \models \psi[s]$ |
| $\phi \to \psi$ | $\mathcal{A} \not\models \phi[s]$ or $\mathcal{A} \models \psi[s]$ |
| $\forall x \phi$ | for every $a \in |\mathcal{A}|$, $\mathcal{A} \models \phi[s(x/a)]$ |
| $\exists x \phi$ | there exists $a \in |\mathcal{A}|$ such that $\mathcal{A} \models \phi[s(x/a)]$ |

Where $s(x/a)$ is the assignment that maps $x$ to $a$ and agrees with $s$ on all other variables.

A **model** of a formula $\phi$ (or a set of formulas $\Gamma$) is a structure $\mathcal{A}$ such that $\mathcal{A} \models \phi[s]$ for all assignments $s$ (if $\phi$ is a sentence, the assignment doesn't matter).

### Satisfiability and Validity

A **sentence** $\phi$ is:

| Property | Definition | Notation |
|----------|------------|----------|
| **Satisfiable** | There exists some structure $\mathcal{A}$ such that $\mathcal{A} \models \phi$ | |
| **Valid** | For every structure $\mathcal{A}$, $\mathcal{A} \models \phi$ | $\models \phi$ |
| **Falsifiable** | There exists some structure $\mathcal{A}$ such that $\mathcal{A} \not\models \phi$ | |

**Relationships:**

- $\phi$ is valid iff $\neg \phi$ is unsatisfiable.
- $\phi$ is satisfiable iff $\neg \phi$ is falsifiable.
- If $\phi$ is valid, it is satisfiable (but not conversely).

**Entailment:** $\Gamma \models \phi$ means every model of $\Gamma$ is also a model of $\phi$.

**Example of validity:**

$$\models \forall x (x = x) \quad \text{(reflexivity of equality)}$$
$$\models \forall x \forall y ((x = y) \to (P(x) \to P(y))) \quad \text{(substitution)}$$
$$\models (\forall x P(x)) \to P(t) \quad \text{(universal instantiation)}$$
$$\models P(t) \to \exists x P(x) \quad \text{(existential generalization)}$$

**Example of satisfiability (not validity):**

$$\exists x \exists y (x \neq y)$$

Satisfiable (true in domains with at least two elements) but not valid (false in singleton domains).

**Decidability:**

- Validity in propositional logic is decidable (truth tables).
- Validity in first-order logic is **undecidable** (Church, 1936). There is no algorithm that, given any FOL formula, correctly determines whether it is valid.
- Validity in FOL is **semi-decidable**: if a formula is valid, a proof will eventually be found by exhaustive search. If it is not valid, the search may never terminate.

### Godel's Completeness Theorem

**Theorem (Godel, 1929):** For any set of first-order sentences $\Gamma$ and sentence $\phi$:

$$\Gamma \vdash \phi \quad \text{if and only if} \quad \Gamma \models \phi$$

In words: provability in a sound and complete proof system coincides with logical entailment.

**Two directions:**

| Direction | Name | Meaning |
|-----------|------|---------|
| $\Gamma \vdash \phi \implies \Gamma \models \phi$ | Soundness | Every provable statement is true in all models |
| $\Gamma \models \phi \implies \Gamma \vdash \phi$ | Completeness | Every true logical consequence is provable |

**Why completeness is surprising:** It says that the purely syntactic notion of proof (manipulating symbols according to rules) captures the semantic notion of truth (holding in all structures) perfectly.

**Proof sketch:**

1. If $\Gamma$ is consistent (no contradiction is provable), then $\Gamma$ has a model.
2. Construct the model from the syntactic elements themselves: the domain is the set of terms, and relations are defined by what is provable.
3. Extend $\Gamma$ to a maximally consistent set $\Gamma^*$ (Henkin construction).
4. Add witnessing constants to handle existential quantifiers.
5. Show that the term model satisfies exactly the formulas in $\Gamma^*$.

**Corollary (Compactness):** $\Gamma$ has a model iff every finite subset of $\Gamma$ has a model.

**Practical significance:** The completeness theorem guarantees that proof search procedures (resolution, tableaux, sequent calculus) are complete for first-order logic. Any valid formula has a finite proof.

### Compactness Theorem

**Theorem (Compactness):** A set $\Gamma$ of first-order sentences has a model iff every finite subset $\Gamma_0 \subseteq \Gamma$ has a model.

**Equivalently:** If $\Gamma \models \phi$, then there exists a finite $\Gamma_0 \subseteq \Gamma$ such that $\Gamma_0 \models \phi$.

**Consequences:**

**1. Non-standard models of arithmetic:** Let $\Gamma$ be the set of all sentences true in the standard model of arithmetic $(\mathbb{N}, +, \times, 0, 1)$. Add a new constant $c$ and axioms $\{c > 0, c > 1, c > 2, \dots\}$. Every finite subset is satisfiable (interpret $c$ as a sufficiently large natural number). By compactness, the whole set is satisfiable — producing a **non-standard model** of arithmetic with infinite elements.

**2. Graph coloring:** A graph is $k$-colorable iff every finite subgraph is $k$-colorable. (Express the coloring condition as a set of first-order sentences and apply compactness.)

**3. Algebraic closure:** A field has an algebraically closed extension iff every finite subset of the extension axioms is consistent.

**4. Upward Lowenheim-Skolem:** If a set of sentences has an infinite model, it has models of arbitrarily large cardinality.

### Lowenheim-Skolem Theorem

**Downward Lowenheim-Skolem:** If a countable set of first-order sentences has a model, it has a model whose domain is at most countable.

**Upward Lowenheim-Skolem:** If a set of first-order sentences has an infinite model, it has models of every infinite cardinality.

**The Skolem Paradox:** Zermelo-Fraenkel set theory (ZFC) proves the existence of uncountable sets. But by the Downward Lowenheim-Skolem theorem, ZFC has a countable model (if it has any model at all). How can a countable model satisfy "there exists an uncountable set"?

Resolution: In a countable model of ZFC, what the model considers "uncountable" is different from what is actually uncountable. The model's "powerset of $\mathbb{N}$" is, from the outside, countable — but the model lacks a bijection between it and $\mathbb{N}$.

**Relevance to computing:** These theorems show that first-order logic cannot uniquely characterize infinite structures. Any attempt to axiomatize the natural numbers, real numbers, or any infinite structure will have unintended models.

### Limitations of First-Order Logic

#### Inexpressibility

Certain important concepts cannot be expressed in first-order logic:

| Concept | Why Inexpressible |
|---------|------------------|
| "There are finitely many $x$ such that $P(x)$" | Compactness prevents defining finiteness |
| "The domain has exactly $n$ elements" | Cannot define exact finite cardinality uniformly |
| "The relation $R$ is the transitive closure of $E$" | Transitive closure is not first-order definable |
| "The graph is connected" | Requires transitive closure of edge relation |
| "Every non-empty subset has a least element" (well-ordering) | Requires quantifying over subsets (second-order) |
| Induction principle: "If $P(0)$ and $\forall n (P(n) \to P(n+1))$ then $\forall n P(n)$" | Requires quantifying over all properties $P$ |

**Transitive closure example:** In first-order logic, you can express that $R$ is transitive ($\forall x \forall y \forall z (R(x,y) \land R(y,z) \to R(x,z))$). But you cannot express that $R$ is the smallest transitive relation containing some base relation $E$. This is the **reachability problem** in graphs — fundamental to databases and program analysis.

#### Expressive Power Hierarchy

$$\text{Propositional} \subset \text{FOL} \subset \text{Second-Order Logic} \subset \text{Higher-Order Logic}$$

FOL can express more than propositional logic (quantification) but less than second-order logic (quantification over relations).

#### Fragments of FOL

Some decidable fragments exist:

| Fragment | Restrictions | Decidability |
|----------|-------------|--------------|
| Monadic FOL | Only unary predicates, no functions | Decidable (Loweinheim) |
| Two-variable FOL | Only two variable symbols | Decidable |
| Guarded fragment | All quantifiers are "guarded" by atomic formulas | Decidable |
| FO(+, x) on N | FOL with $+$ and $\times$ on natural numbers | Undecidable (Godel's Incompleteness) |
| Presburger arithmetic | FOL with only $+$ on $\mathbb{N}$ | Decidable |
| FO with equality | Full FOL | Undecidable |

### Applications

#### Formal Methods and Verification

First-order logic is the language of formal verification:

```python
# In a tool like Dafny or Why3
def binary_search(arr: list[int], key: int) -> int:
    # Precondition: sorted array
    # ∀i,j (0 ≤ i < j < len(arr) → arr[i] ≤ arr[j])
    #
    # Postcondition:
    # (result = -1 → ∀i (0 ≤ i < len(arr) → arr[i] ≠ key))
    # (result ≠ -1 → arr[result] = key ∧ 0 ≤ result < len(arr))
    pass
```

The verification conditions generated by such tools are first-order formulas checked by SMT solvers.

#### Knowledge Representation

Description logics (the logical foundation of OWL and semantic web) are decidable fragments of FOL:

$$\text{Parent} \equiv \exists \text{hasChild.Top}$$
$$\text{Grandparent} \equiv \exists \text{hasChild.Parent}$$
$$\text{Doctor} \equiv \text{Person} \sqcap \exists \text{hasDegree.MD}$$

These correspond to FOL formulas:
$$\forall x (\text{Doctor}(x) \leftrightarrow \text{Person}(x) \land \exists y (\text{hasDegree}(x, y) \land \text{MD}(y)))$$

#### Database Theory

**Relational algebra** and **relational calculus** are directly based on FOL:

$$\{x \mid \exists y (\text{Employee}(y) \land \text{Department}(y, 'CS') \land \text{Salary}(y, x))\}$$

This is the **domain relational calculus**. SQL is a syntactic variant of the **tuple relational calculus**, which is a restricted form of FOL.

```sql
-- FOL: {e.name | Employee(e) ∧ ∀p (Project(p) ∧ ManagedBy(p, e.id))}
SELECT e.name FROM Employee e
WHERE NOT EXISTS (
    SELECT 1 FROM Project p
    WHERE p.manager_id != e.id
)
```

#### Model Checking

Although model checking of finite-state systems typically uses temporal logics, the underlying state exploration can be expressed in FOL with a fixed-point operator (FO + LFP, which expresses the modal mu-calculus).

### FOL in Programming: SQL, Prolog, and Type Systems

#### SQL and Relational Calculus

SQL queries are syntactic sugar for domain relational calculus, a restricted fragment of FOL. Every SQL query corresponds to a FOL formula with:

- `SELECT columns` → free variables being projected
- `FROM tables` → domain of quantification
- `WHERE conditions` → quantifier-free formula (for simple predicates)
- `NOT EXISTS` → universal quantification or negated existential

```sql
-- Find employees who manage all projects in department CS
-- FOL: {e.name | Employee(e) ∧ ∀p (Project(p) ∧ Dept(p, 'CS') → Manages(e, p))}
SELECT e.name
FROM Employee e
WHERE NOT EXISTS (
    SELECT 1 FROM Project p
    WHERE p.dept = 'CS'
    AND p.manager_id != e.id
)
```

SQL's limited quantification (only `EXISTS` and `NOT EXISTS` correspond to $\exists$ and $\neg \exists$) means some FOL queries cannot be expressed directly. Recursive CTEs (Common Table Expressions) extend SQL beyond FOL into fixed-point logic.

#### Prolog and Horn Clauses

Prolog is a logic programming language based on a subset of FOL called Horn clauses. A Horn clause has at most one positive literal:

```
% FOL: ∀x (Parent(x, y) ∧ Male(x) → Father(x, y))
% Prolog: father(X, Y) :- parent(X, Y), male(X).

% FOL: ∀x (∃y Parent(y, x) → Child(x))
% Prolog: child(X) :- parent(_, X).

% Facts
parent(john, mary).
parent(mary, alice).
male(john).

% Queries
?- father(john, mary).   % true
?- child(alice).          % true
?- parent(X, Y).          % enumerates all parent relationships
```

Prolog uses resolution with SLDNF (Selective Linear Definite clause with Negation as Failure) to answer queries. The absence of a proof is interpreted as falsity (closed-world assumption), which differs from standard FOL semantics (open-world assumption).

#### Type Systems and Curry-Howard Correspondence

The Curry-Howard correspondence maps logical systems to type systems:

| Logic | Programming |
|-------|-------------|
| Proposition | Type |
| Implication ($\to$) | Function type ($\to$) |
| Conjunction ($\land$) | Product type (pair, tuple) |
| Disjunction ($\lor$) | Sum type (Either, tagged union) |
| Universal quantifier ($\forall$) | Polymorphic type (generics) |
| Existential quantifier ($\exists$) | Abstract data type (existential) |
| Proof | Program (term) |
| Proof normalization | Program evaluation |

First-order logic corresponds to a type system with dependent types (like Idris, Agda, Coq). The `forall` quantifier in Haskell (`forall a. a -> a`) is second-order, not first-order — it quantifies over types, which are like predicates in logic.

```haskell
-- Universal quantification in Haskell (second-order)
id :: forall a. a -> a
id x = x

-- This corresponds to: ∀a (a → a) in second-order logic
```

**Dependent types (FOL in types):**

```idris
-- Idris: FOL-style quantification over values
append : (n : Nat) -> (m : Nat) -> Vect n a -> Vect m a -> Vect (n + m) a

-- The type says: for all n, m, and any vector of length n and m,
-- append produces a vector of length n + m
-- This is: ∀n∀m∀a (Vect(n, a) → Vect(m, a) → Vect(n+m, a))
```

### Common Pitfalls in FOL

**Pitfall 1: Confusing free and bound variables.**

```text
// Wrong: P(x) → ∀x Q(x)  — x is both free and bound
// Right: P(x) → ∀y Q(y)  — renamed to avoid confusion
```

Always rename bound variables before substituting to avoid capture.

**Pitfall 2: Reordering quantifiers incorrectly.**

$$\forall x \exists y \text{Loves}(x, y) \quad \text{(everyone loves someone)}$$
$$\exists y \forall x \text{Loves}(x, y) \quad \text{(someone is loved by everyone)}$$

These are not equivalent. Swapping $\forall$ and $\exists$ changes the meaning. The first says each person loves at least one person (possibly different people). The second says there is a universal beloved.

**Pitfall 3: Using $\to$ when $\land$ is needed with $\exists$.**

Translating "Some cats are black":

```text
// Wrong: ∃x (Cat(x) → Black(x))
// This is true in any domain where at least one thing is not a cat!
// Right: ∃x (Cat(x) ∧ Black(x))
```

The implication $\to$ is true whenever the antecedent is false, so $\exists x (Cat(x) \to Black(x))$ is satisfied by any non-cat element.

**Pitfall 4: Using $\land$ when $\to$ is needed with $\forall$.**

Translating "All cats are black":

```text
// Wrong: ∀x (Cat(x) ∧ Black(x))
// This says everything is a black cat — far too strong.
// Right: ∀x (Cat(x) → Black(x))
```

**Pitfall 5: Negating quantifiers incorrectly.**

$$\neg \forall x P(x) \quad \text{is equivalent to} \quad \exists x \neg P(x)$$
$$\neg \exists x P(x) \quad \text{is equivalent to} \quad \forall x \neg P(x)$$

This is the **duality of quantifiers**. Push negations inward over quantifiers by flipping $\forall \leftrightarrow \exists$.

**Pitfall 6: Assuming the domain is non-empty.**

FOL structures must have a non-empty domain. But in reasoning about software, you may need to express properties about empty collections. Be careful translating from programming to logic — an empty list in code does not correspond to an empty domain.

**Pitfall 7: Equality vs. equivalence.**

The equality symbol $=$ in FOL refers to identity — two terms refer to the same object. This is different from logical equivalence ($\leftrightarrow$), bi-implication, or observational equivalence in programming. In databases, $=$ for `NULL` values behaves differently from FOL equality.

### Learning Tips

**Translate natural language to FOL and back:** Take English sentences, translate them to FOL, and then translate the FOL back to English to verify the meaning is preserved. This builds the muscle memory for quantifier placement.

**Practice with finite models:** When checking validity or satisfiability, start with small finite domains (1, 2, 3 elements). Enumerate the possibilities to build intuition before reasoning about infinite domains.

**Remember the duality laws:** $\neg \forall x \phi \iff \exists x \neg \phi$ and $\neg \exists x \phi \iff \forall x \neg \phi$. These are essential for negation and contrapositive reasoning.

**Draw Venn-style diagrams for quantifiers:** Visualize $\forall x (P(x) \to Q(x))$ as "the P circle is inside the Q circle" and $\exists x (P(x) \land Q(x))$ as "the circles overlap".

**Use SMT solvers for verification:** The Z3 solver (by Microsoft) can check satisfiability of FOL formulas in decidable fragments. Experimenting with Z3 gives immediate feedback on your understanding.

```python
from z3 import *

x, y = Ints('x y')
s = Solver()
s.add(ForAll([x, y], x + y == y + x))  # commutativity
print(s.check())  # sat — addition is commutative
```

**Separate syntax from semantics:** Confusing the two is the most common conceptual error. Syntax is about symbols and rules for forming formulas. Semantics is about meaning in structures. The same formula can be true in one structure and false in another.

## Glossary

| Term | Definition |
|------|------------|
| Signature | The set of constant, function, and relation symbols for a first-order language |
| Term | An expression built from variables, constants, and function symbols |
| Atomic formula | A formula of the form $t_1 = t_2$ or $R(t_1, \dots, t_n)$ |
| Structure | A domain with interpretations of all symbols in the signature |
| Model | A structure that satisfies a given formula or theory |
| Variable assignment | A mapping from variables to domain elements |
| Satisfaction | The relation $\mathcal{A} \models \phi[s]$ meaning $\phi$ holds in $\mathcal{A}$ under $s$ |
| Validity | A formula that holds in every structure |
| Satisfiability | A formula that holds in some structure |
| Entailment | $\Gamma \models \phi$: every model of $\Gamma$ is also a model of $\phi$ |
| Completeness | Every valid formula is provable (Godel, 1929) |
| Soundness | Every provable formula is valid (truth-preserving rules) |
| Compactness | A set of formulas has a model iff every finite subset has a model |
| Lowenheim-Skolem | A theory with an infinite model has models of every infinite size |
| Non-standard model | A model not isomorphic to the intended structure |
| Categoricity | A theory with exactly one model up to isomorphism (rare in FOL) |
| Conservative extension | Adding definitions to a theory without changing its theorems |
| Presburger arithmetic | Decidable FOL fragment with $+$ but no $\times$ on $\mathbb{N}$ |
| Monadic FOL | FOL with only unary predicates (no $n$-ary relations for $n > 1$) |
| Horn clause | A clause with at most one positive literal, used in Prolog |
| Closed-world assumption | Everything not provably true is false (Prolog semantics) |
| Open-world assumption | Truth is independent of provability (standard FOL semantics) |
| Curry-Howard correspondence | The isomorphism between proofs and programs |
| Dependent type | A type that depends on a value, analogous to FOL quantification |
| Quantifier duality | $\neg \forall x \phi \iff \exists x \neg \phi$ and vice versa |
| Capture-avoiding substitution | Renaming bound variables to prevent accidental capture during substitution |
| Henkin construction | Building a model from syntactic elements by extending to a maximally consistent set |
| SMT solver | Satisfiability Modulo Theories — automated solver for quantifier-free FOL with theories |
| Resolution | A proof system for FOL based on refutation and unification |
| Skolemization | Replacing existential quantifiers with fresh function symbols |
| Herbrand model | A model where the domain is the set of ground terms |

## Quick References

- [A Concise Introduction to Mathematical Logic, Rautenberg](https://link.springer.com/book/10.1007/978-1-4419-1221-3) — rigorous but accessible FOL metatheory
- [Logic in Computer Science, Huth & Ryan](https://www.cambridge.org/highereducation/books/logic-in-computer-science/C791B0C0A8B6A2A1D5CBDAF5B3614A0E) — FOL semantics and model checking
- [Stanford Encyclopedia: First-Order Logic](https://plato.stanford.edu/entries/logic-firstorder/) — comprehensive philosophical and mathematical reference
- [Model Theory, Hodges](https://www.cambridge.org/core/books/model-theory/93FD6B5B5EF7A0C1A6E0B8E3F4A0C3E1) — the standard reference for model theory
- [Z3 SMT Solver](https://github.com/Z3Prover/z3) — supports quantifier reasoning in FOL
- [Learn Prolog Now!](https://www.learnprolognow.org/) — free online Prolog course
- [Curry-Howard Correspondence (Wikipedia)](https://en.wikipedia.org/wiki/Curry%E2%80%93Howard_correspondence) — detailed explanation of proofs-as-programs
- [SQL and Relational Calculus](https://en.wikipedia.org/wiki/Relational_calculus) — connection between SQL and FOL
- [The Art of Prolog, Sterling & Shapiro](https://mitpress.mit.edu/9780262691635/the-art-of-prolog/) — advanced Prolog programming techniques
- [Types and Programming Languages, Pierce](https://www.cis.upenn.edu/~bcpierce/tapl/) — type systems and their logical foundations
- [First-Order Logic in SMT (Z3 Tutorial)](https://microsoft.github.io/z3guide/docs/logic/Quantifiers/) — using Z3 for quantifier reasoning

## Next Steps

- [Modal & Temporal Logic](modal-and-temporal-logic.md) — adding necessity, possibility, and time to logical systems
