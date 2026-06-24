# Set Theory

## Description

Set theory is the foundation of all mathematics. A set is a collection of distinct elements. Sets appear everywhere in computing: database queries are set operations, type systems are based on set membership, and data structures are sets with added structure.

## Prerequisites

- [What Is Discrete Mathematics](../intro/what-is-discrete-math.md)

## Table of Contents

- [Definition and Notation](#definition-and-notation)
- [Basic Operations](#basic-operations)
- [Venn Diagrams](#venn-diagrams)
- [Sets in Code](#sets-in-code)

## Content / Material

### Definition and Notation

A set is a collection of distinct objects:

$$ S = \\{1, 2, 3\\}, \quad T = \\{a, b, c\\} $$

- **Element**: $1 \in S$ (1 is an element of S)
- **Not element**: $4 \notin S$
- **Empty set**: $\emptyset = \\{\\}$
- **Cardinality**: $|S| = 3$ (number of elements)

### Basic Operations

| Operation | Notation | Result |
|-----------|----------|--------|
| Union | $A \cup B$ | All elements in A or B |
| Intersection | $A \cap B$ | Elements in both A and B |
| Difference | $A \setminus B$ | Elements in A but not B |
| Subset | $A \subseteq B$ | All elements of A are in B |

```python
A = {1, 2, 3}
B = {3, 4, 5}

print(A | B)   # union:       {1, 2, 3, 4, 5}
print(A & B)   # intersection: {3}
print(A - B)   # difference:  {1, 2}
print(A <= B)  # subset:      False
```

### Venn Diagrams

Venn diagrams visually represent set operations:

```
┌─────────┐
│  A   │  │
│  ┌───┼──┼──┐
│  │   │  │  │
│  └───┼──┼──┘
│      │  │
└──────┴──┘
   A ∪ B = shaded area
```

### Sets in Code

```sql
-- SQL: UNION, INTERSECT, EXCEPT are set operations
SELECT name FROM employees
UNION
SELECT name FROM contractors;
```

```javascript
// JavaScript Set
const a = new Set([1, 2, 3]);
const b = new Set([3, 4, 5]);
// No built-in union/intersection — implement manually
const union = new Set([...a, ...b]);
```

Set operations in databases and programming languages directly mirror mathematical set theory.

## Glossary

| Term | Definition |
|------|------------|
| Set | A collection of distinct elements |
| Union | All elements in either set |
| Intersection | Elements present in both sets |
| Cardinality | The number of elements in a set |
| Empty set | The set with no elements |

## Next Steps

- [Graph Theory](graph-theory.md) — nodes, edges, paths, and trees
