# Logic for Developers

## Description

Logic is the foundation of both mathematics and computation. Every conditional, loop, type check, and database query is an exercise in logic. This document covers the core principles of propositional and predicate logic — and how they map directly to code.

## Prerequisites

- [The Mathematical Mindset](the-mathematical-mindset.md)

## Table of Contents

- [Why Logic Matters in Code](#why-logic-matters-in-code)
- [Propositional Logic](#propositional-logic)
- [Predicate Logic](#predicate-logic)
- [Truth Tables](#truth-tables)
- [Common Logical Fallacies in Code](#common-logical-fallacies-in-code)

## Content / Material

### Why Logic Matters in Code

- Boolean conditions are propositions.
- Database `WHERE` clauses are predicates.
- Type systems are logic systems (Curry-Howard correspondence).
- State machines are logical state transitions.
- API contracts specify logical preconditions and postconditions.

### Propositional Logic

A *proposition* is a statement that is either true or false. Propositions combine with logical operators:

| Operator | Math | Code |
|----------|------|------|
| AND | $A \land B$ | `A && B` |
| OR | $A \lor B$ | `A \|\| B` |
| NOT | $\lnot A$ | `!A` |
| Implication | $A \implies B$ | `if (A) { B }` |
| Equivalence | $A \iff B$ | `A == B` (boolean) |

### Predicate Logic

A *predicate* is a proposition that depends on one or more variables:

```
isEven(x)    → true if x is even
x > y        → a predicate with two variables
∀x ∈ S: P(x) → "for all x in S, P(x) holds"
∃x ∈ S: P(x) → "there exists x in S such that P(x) holds"
```

In code:
```javascript
const allEven = arr.every(x => x % 2 === 0)   // ∀
const hasEven = arr.some(x => x % 2 === 0)     // ∃
```

### Truth Tables

A truth table enumerates all possible truth values of a logical expression:

| A | B | A AND B | A OR B | A → B |
|---|---|---------|--------|-------|
| T | T | T | T | T |
| T | F | F | T | F |
| F | T | F | T | T |
| F | F | F | F | T |

Note: `A → B` (implication) is only false when `A` is true and `B` is false. This is exactly how `if (A) { B }` works.

### Common Logical Fallacies in Code

| Fallacy | Description | Example |
|---------|-------------|---------|
| **Affirming the consequent** | `A → B` and `B` being true does not imply `A` | "User is admin because they can delete" (maybe they have permission another way) |
| **Denying the antecedent** | `A → B` and `A` being false does not imply `B` is false | "Not admin, so can't edit" (but maybe they're a moderator) |
| **Null vs. false** | Treating null and false as the same | `if (!value)` when value could be null, 0, or false |

## Glossary

| Term | Definition |
|------|------------|
| Proposition | A statement that is either true or false |
| Predicate | A proposition that depends on variables |
| Quantifier | ∀ (for all) and ∃ (there exists) |
| Truth table | A table enumerating all possible truth values of an expression |

## Next Steps

- [Set Theory](../beginner/set-theory.md) — sets, unions, intersections, and their code analogues
