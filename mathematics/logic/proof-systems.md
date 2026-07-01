# Proof Systems

## Description

A proof system provides mechanical rules for deriving truths from assumptions. Understanding proof systems is essential for automated theorem proving, program verification, and logic programming. This document covers natural deduction, sequent calculus, Hilbert systems, and resolution — the core proof frameworks that power SAT solvers, SMT solvers, and the Prolog language.

## Prerequisites

- [Propositional Logic](propositional-logic.md) — connectives, truth tables, logical equivalence, normal forms
- [Predicate Logic](predicate-logic.md) — quantifiers, predicates, free and bound variables

## Table of Contents

- [What is a Proof System?](#what-is-a-proof-system)
- [Natural Deduction](#natural-deduction)
- [Sequent Calculus](#sequent-calculus)
- [Hilbert Systems](#hilbert-systems)
- [Resolution Principle](#resolution-principle)
- [Soundness and Completeness](#soundness-and-completeness)
- [Automated Theorem Proving](#automated-theorem-proving)
- [Applications](#applications)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### What is a Proof System?

A **proof system** (or deductive system) consists of:

1. A set of **axioms** — formulas taken as true without proof
2. A set of **inference rules** — rules for deriving new formulas from existing ones

A **proof** (or derivation) of a formula $\phi$ from a set of premises $\Gamma$ is a finite sequence of formulas where each formula is either:
- An axiom
- A premise from $\Gamma$
- Obtained from earlier formulas by applying an inference rule

We write $\Gamma \vdash \phi$ to mean "$\phi$ is provable from $\Gamma$."

**Three classic proof systems:**

| System | Style | Practical Use |
|--------|-------|--------------|
| Natural Deduction | Introduction/elimination rules for each connective | Teaching, interactive theorem proving (Coq, Lean) |
| Sequent Calculus | Symmetric rules manipulating sequents | Proof search, cut elimination |
| Hilbert System | Few axioms + modus ponens | Meta-theory, proving properties about logic |
| Resolution | Single rule: resolve clauses | Automated theorem proving, SAT solvers, Prolog |

### Natural Deduction

Natural deduction, developed by Gentzen (1934), mirrors how humans reason. For each logical connective, there are **introduction rules** (how to prove a formula with that connective) and **elimination rules** (what follows from a formula with that connective).

**Proof notation:** $\dfrac{\text{premises}}{\text{conclusion}}$

#### Conjunction ($\land$)

**Introduction:** To prove $A \land B$, prove $A$ and prove $B$.

$$\frac{A \quad B}{A \land B} (\land I)$$

**Elimination:** From $A \land B$, you can infer $A$ (or $B$).

$$\frac{A \land B}{A} (\land E_1) \quad \frac{A \land B}{B} (\land E_2)$$

#### Disjunction ($\lor$)

**Introduction:** To prove $A \lor B$, prove $A$ (or prove $B$).

$$\frac{A}{A \lor B} (\lor I_1) \quad \frac{B}{A \lor B} (\lor I_2)$$

**Elimination (proof by cases):** If $A \lor B$ and $A \vdash C$ and $B \vdash C$, then $C$.

$$\frac{A \lor B \quad [A] \cdots C \quad [B] \cdots C}{C} (\lor E)$$

The brackets $[A]$ indicate an assumption that is discharged.

#### Implication ($\to$)

**Introduction (deduction theorem):** If from $A$ we can prove $B$, then $A \to B$.

$$\frac{[A] \cdots B}{A \to B} (\to I)$$

**Elimination (modus ponens):** From $A \to B$ and $A$, infer $B$.

$$\frac{A \to B \quad A}{B} (\to E)$$

#### Negation ($\neg$)

**Introduction:** If from $A$ we derive a contradiction ($\bot$), conclude $\neg A$.

$$\frac{[A] \cdots \bot}{\neg A} (\neg I)$$

**Elimination:** From $A$ and $\neg A$, derive a contradiction.

$$\frac{A \quad \neg A}{\bot} (\neg E)$$

#### Reductio ad Absurdum (RAA)

$$\frac{[\neg A] \cdots \bot}{A} (RAA)$$

Note: RAA is equivalent to double negation elimination ($\neg\neg A \to A$), which is not accepted in intuitionistic logic.

#### Example Proof: $p \land q \vdash q \land p$

$$
\frac{
  \dfrac{p \land q}{q} (\land E_2) \quad
  \dfrac{p \land q}{p} (\land E_1)
}{q \land p} (\land I)
$$

#### Example Proof: $\vdash (p \to (q \to r)) \to ((p \to q) \to (p \to r))$

$$
\frac{
  [p \to (q \to r)]^{(1)} \quad [p]^{(3)}
  {\dfrac{q \to r}{}} (\to E)
  \quad
  [p \to q]^{(2)} \quad [p]^{(3)}
  {\dfrac{q}{}} (\to E)
  {\dfrac{r}{}} (\to E)
  {\dfrac{p \to r}{(p \to q) \to (p \to r)}} (\to I)^{(3)}
}{ \dfrac{(p \to (q \to r)) \to ((p \to q) \to (p \to r))}{} (\to I)^{(1)} }
$$

This is the axiom of the Hilbert system expressed as a natural deduction proof.

### Sequent Calculus

A **sequent** is written as $\Gamma \vdash \Delta$, where $\Gamma$ and $\Delta$ are finite sets (or multisets) of formulas. Intuitively: "if all formulas in $\Gamma$ hold, then at least one formula in $\Delta$ holds."

- $\Gamma$ is the **antecedent** (assumptions)
- $\Delta$ is the **succedent** (conclusions)

A single-formula succedent $\Gamma \vdash A$ means "$\Gamma$ proves $A$."

**Identity rule:**

$$\frac{}{A \vdash A} (Id)$$

**Structural rules:**

$$\frac{\Gamma \vdash \Delta}{\Gamma, A \vdash \Delta} (WL) \quad
\frac{\Gamma \vdash \Delta}{\Gamma \vdash A, \Delta} (WR)$$

**Cut rule (eliminates intermediate lemmas):**

$$\frac{\Gamma \vdash A, \Delta \quad \Gamma, A \vdash \Delta}{\Gamma \vdash \Delta} (Cut)$$

The **cut elimination theorem** (Gentzen's Hauptsatz) shows that any proof using Cut can be transformed into one without Cut. This is the theoretical basis for proof search algorithms.

**Logical rules (a sample):**

$$\frac{\Gamma, A \vdash \Delta}{\Gamma \vdash \neg A, \Delta} (\neg R) \quad
\frac{\Gamma \vdash A, \Delta}{\Gamma, \neg A \vdash \Delta} (\neg L)$$

$$\frac{\Gamma \vdash A, \Delta \quad \Gamma \vdash B, \Delta}{\Gamma \vdash A \land B, \Delta} (\land R) \quad
\frac{\Gamma, A, B \vdash \Delta}{\Gamma, A \land B \vdash \Delta} (\land L)$$

$$\frac{\Gamma \vdash A, B, \Delta}{\Gamma \vdash A \lor B, \Delta} (\lor R) \quad
\frac{\Gamma, A \vdash \Delta \quad \Gamma, B \vdash \Delta}{\Gamma, A \lor B \vdash \Delta} (\lor L)$$

$$\frac{\Gamma, A \vdash B, \Delta}{\Gamma \vdash A \to B, \Delta} (\to R) \quad
\frac{\Gamma \vdash A, \Delta \quad \Gamma, B \vdash \Delta}{\Gamma, A \to B \vdash \Delta} (\to L)$$

**Quantifier rules:**

$$\frac{\Gamma \vdash A[y/x], \Delta}{\Gamma \vdash \forall x A, \Delta} (\forall R) \quad
y \text{ not free in } \Gamma, \Delta$$

$$\frac{\Gamma, A[t/x] \vdash \Delta}{\Gamma, \forall x A \vdash \Delta} (\forall L)$$

$$\frac{\Gamma \vdash A[t/x], \Delta}{\Gamma \vdash \exists x A, \Delta} (\exists R) \quad
\frac{\Gamma, A[y/x] \vdash \Delta}{\Gamma, \exists x A \vdash \Delta} (\exists L)$$
$$y \text{ not free in } \Gamma, \Delta, \exists x A$$

The eigenvariable condition (y not free) prevents unsound reasoning.

### Hilbert Systems

Hilbert systems use few axioms and a single inference rule (modus ponens), making them suitable for meta-theoretical analysis.

**Propositional axioms:**

| Ax | Formula |
|----|---------|
| K | $p \to (q \to p)$ |
| S | $(p \to (q \to r)) \to ((p \to q) \to (p \to r))$ |
| DN | $\neg\neg p \to p$ (double negation elimination) |

**Inference rule:**

$$\frac{A \quad A \to B}{B} (MP)$$

**A proof of $p \to p$ in Hilbert system:**

1. $(p \to ((p \to p) \to p)) \to ((p \to (p \to p)) \to (p \to p))$ (Instance of S)
2. $p \to ((p \to p) \to p)$ (Instance of K)
3. $(p \to (p \to p)) \to (p \to p)$ (MP on 1, 2)
4. $p \to (p \to p)$ (Instance of K)
5. $p \to p$ (MP on 3, 4)

Even this trivial theorem requires 5 steps. Hilbert systems are impractical for human use but valuable for studying the foundations of logic.

### Resolution Principle

Resolution is the computational workhorse of automated theorem proving. It operates on **clauses** (disjunctions of literals) in CNF.

**Resolution rule (propositional):**

$$\frac{C_1 \lor l \quad C_2 \lor \neg l}{C_1 \lor C_2}$$

Where $C_1, C_2$ are clauses and $l$ is a literal. The conclusion is the **resolvent**.

**Example:**

$$\frac{p \lor q \quad \neg p \lor r}{q \lor r}$$

**Resolution for predicate logic** requires unification — finding a substitution that makes two literals identical:

$$\frac{P(x) \lor Q(x) \quad \neg P(f(y)) \lor R(y)}{Q(f(y)) \lor R(y)}$$

Here, $x$ is unified with $f(y)$ via substitution $\{x \mapsto f(y)\}$.

**Resolution algorithm:**
1. Convert the premises and negation of the conclusion to CNF.
2. Repeatedly apply the resolution rule.
3. If the empty clause ($\bot$) is derived, the original formula is valid.

```python
# A simple propositional resolution prover
def resolve(clause1, clause2):
    """Resolve two clauses, return list of resolvents."""
    resolvents = []
    for l1 in clause1:
        for l2 in clause2:
            if l1 == negate(l2):
                resolvent = (clause1 - {l1}) | (clause2 - {l2})
                resolvents.append(resolvent)
    return resolvents

def resolution_prover(clauses, goal):
    """Check if goal follows from clauses using refutation."""
    # Add negated goal
    clauses = clauses | {negate(goal)}
    new = set()
    while True:
        for c1 in clauses:
            for c2 in clauses:
                resolvents = resolve(c1, c2)
                for r in resolvents:
                    if r == set():   # empty clause
                        return True
                    new.add(r)
        if new.issubset(clauses):
            return False
        clauses |= new
```

#### SLD Resolution

**SLD resolution** (Selective Linear Definite) is the basis of Prolog:

- **Goal:** a clause with all negative literals $:- a_1, a_2, \dots, a_n$.
- **Program clause:** a definite clause $h :- b_1, b_2, \dots, b_m$.
- **SLD resolution:** resolve the first literal of the goal with the head of a program clause.

```prolog
% Prolog program
ancestor(X, Y) :- parent(X, Y).
ancestor(X, Y) :- parent(X, Z), ancestor(Z, Y).

% Query
?- ancestor(alice, bob).

% SLD resolution steps:
% :- ancestor(alice, bob).
% resolve with rule 1: :- parent(alice, bob).
% if parent(alice, bob) is a fact: success.
```

### Soundness and Completeness

**Soundness:** If $\Gamma \vdash \phi$ (provable), then $\Gamma \models \phi$ (logically follows).

$$\text{Provable} \implies \text{True}$$

Soundness ensures the proof system does not produce false conclusions. Every rule must preserve truth.

**Completeness:** If $\Gamma \models \phi$, then $\Gamma \vdash \phi$.

$$\text{True} \implies \text{Provable}$$

Completeness ensures that every valid logical consequence can be proved.

**Theorem (Godel, 1929):** First-order logic is sound and complete with respect to the standard deductive systems (natural deduction, sequent calculus, Hilbert systems).

**Implications:**
- For propositional logic: truth tables and proof systems coincide exactly.
- For first-order logic: a formula is valid iff it is provable.
- For propositional logic, provability is decidable (SAT solvers).
- For first-order logic, provability is semi-decidable (if a formula is valid, a proof can be found in finite time; if not, the search may never terminate).

**Completeness of resolution:** Propositional resolution is refutation-complete — if a set of clauses is unsatisfiable, the empty clause can be derived. First-order resolution is refutation-complete only when combined with the **lifting lemma** and factoring.

### Automated Theorem Proving

#### SAT Solvers

Modern SAT solvers implement the **CDCL** (Conflict-Driven Clause Learning) algorithm, which is resolution augmented with:
- **Boolean constraint propagation (BCP):** unit propagation as a core loop.
- **Conflict analysis:** when a conflict occurs, learn a new clause (a resolvent) to prevent revisiting that conflict.
- **Non-chronological backtracking:** jump back to the decision level where the conflict was caused.

```python
# Pseudocode for DPLL (predecessor of CDCL)
def dpll(clauses, assignment):
    if all_clauses_satisfied(clauses, assignment):
        return assignment
    if any_clause_conflict(clauses, assignment):
        return None
    
    # Unit propagation
    assignment = unit_propagate(clauses, assignment)
    if any_clause_conflict(clauses, assignment):
        return None
    
    # Choose unassigned variable
    literal = choose_literal(clauses, assignment)
    # Try True first
    result = dpll(clauses, assignment | {literal: True})
    if result is not None:
        return result
    # Then try False
    return dpll(clauses, assignment | {literal: False})
```

#### SMT Solvers

Satisfiability Modulo Theories (SMT) solvers (Z3, CVC5) extend SAT with theory solvers for arithmetic, arrays, bitvectors, and uninterpreted functions. They combine:
1. A SAT solver for the boolean skeleton
2. Theory solvers that check consistency of the theory-specific literals

```python
# Using Z3 (Python API)
from z3 import *

x = Int('x')
y = Int('y')
s = Solver()
s.add(x > 0)
s.add(y > x)
s.add(y < 0)
print(s.check())  # unsat (cannot have x > 0, y > x, y < 0)
```

#### Prolog as Theorem Prover

Prolog is essentially a resolution theorem prover for Horn clauses:

```prolog
% Prolog uses SLD resolution (linear resolution with selection function)
% A Prolog program is a set of Horn clauses

% append(Xs, Ys, Zs) holds if Zs = Xs ++ Ys
append([], Ys, Ys).
append([X|Xs], Ys, [X|Zs]) :- append(Xs, Ys, Zs).

% Query: ?- append([1,2], [3,4], Z).
% Z = [1,2,3,4]

% Query: ?- append(X, Y, [1,2,3]).
% Generates all splits of [1,2,3]
```

### Applications

#### Program Verification

Proof systems are used to verify software correctness:

```python
# In Dafny, a verified programming language:
function Factorial(n: nat): nat
    requires n >= 0
    ensures Factorial(n) >= 1
{
    if n == 0 then 1 else n * Factorial(n - 1)
}

method ComputeFactorial(n: nat) returns (result: nat)
    ensures result == Factorial(n)
{
    result := 1;
    var i := 0;
    while i < n
        invariant result == Factorial(i)
        invariant i <= n
    {
        i := i + 1;
        result := result * i;
    }
}
```

The Dafny verifier translates the program and annotations into verification conditions and passes them to an SMT solver (currently Z3). The loop invariant is a formula that must be provable by induction.

#### Interactive Theorem Proving (Coq, Lean)

Proof assistants implement natural deduction or type-theoretic proof systems:

```coq
(* Coq: a proof of p -> p *)
Theorem identity : forall (p : Prop), p -> p.
Proof.
  intros p H.
  exact H.
Qed.

(* The proof term: fun (p : Prop) (H : p) => H *)
```

In Lean:

```lean
theorem identity {p : Prop} (h : p) : p := h

-- Or with explicit proof
theorem identity' {p : Prop} (h : p) : p := by
  exact h
```

Both tools type-check the proof term against the proposition, ensuring correctness.

#### Hardware Verification

Digital circuits are verified using SAT and SMT solvers:

```verilog
// Check equivalence of a*b and commutative multiplication
module check_commutativity(a, b);
  input [3:0] a, b;
  assert property (a * b == b * a);
endmodule
```

The equivalence checking tool converts both circuits to CNF and uses a SAT solver to check if $a \times b \neq b \times a$ is satisfiable.

## Glossary

| Term | Definition |
|------|------------|
| Proof system | A set of axioms and inference rules for deriving formulas |
| Inference rule | A rule that derives a conclusion from premises |
| Axiom | A formula accepted as true without proof |
| Derivation | A sequence of formulas where each follows from earlier ones via rules |
| Sequents | An object of the form $\Gamma \vdash \Delta$ |
| Natural deduction | A proof system with introduction/elimination rules for each connective |
| Introduction rule | A rule that shows how to prove a formula with a given connective |
| Elimination rule | A rule that shows what follows from a formula with a given connective |
| Modus ponens | The rule: from $A \to B$ and $A$, infer $B$ |
| Cut rule | If $\Gamma \vdash A$ and $\Gamma, A \vdash B$ then $\Gamma \vdash B$ |
| Cut elimination | Removing Cut from proofs; every provable sequent has a cut-free proof |
| Resolution | A proof rule: from $C_1 \lor l$ and $C_2 \lor \neg l$, infer $C_1 \lor C_2$ |
| Unification | Finding a substitution that makes two terms identical |
| SLD resolution | Linear resolution for Horn clauses, used in Prolog |
| Soundness | If $\vdash \phi$ then $\models \phi$; provable implies true |
| Completeness | If $\models \phi$ then $\vdash \phi$; true implies provable |
| CDCL | Conflict-Driven Clause Learning, the algorithm behind modern SAT solvers |
| Horn clause | A clause with at most one positive literal |
| Empty clause | The clause $\bot$, representing a contradiction |
| Refutation | A proof by contradiction: derive $\bot$ from the negated goal |

## Quick References

- [Natural Deduction](https://en.wikipedia.org/wiki/Natural_deduction) — Gentzen's original formulation and modern notation
- [Sequent Calculus](https://en.wikipedia.org/wiki/Sequent_calculus) — comprehensive reference with rules for all connectives
- [Handbook of Practical Logic and Automated Reasoning, Harrison](https://www.cambridge.org/us/academic/subjects/computer-science/programming-languages-and-applied-logic/handbook-practical-logic-and-automated-reasoning) — extensive coverage of proof systems and automation
- [Z3 Prover](https://github.com/Z3Prover/z3) — Microsoft's SMT solver
- [Lean Theorem Prover](https://leanprover.github.io/) — interactive theorem prover with natural deduction
- [SWI-Prolog](https://www.swi-prolog.org/) — Prolog implementation with SLD resolution

## Next Steps

- [First-Order Logic](first-order-logic.md) — semantics of FOL, models, compactness, completeness theorem
