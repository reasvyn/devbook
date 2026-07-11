# Higher-Order Logic & Type Theory

## Description

Higher-order logic (HOL) extends first-order logic by allowing quantification over predicates, functions, and sets — enabling the direct formalization of induction, transitive closure, and other concepts beyond FOL's reach. Type theory, its computational counterpart, is the foundation of modern functional programming languages and proof assistants. The Curry-Howard correspondence reveals a deep connection: every proof is a program, and every program is a proof.

## Prerequisites

- [First-Order Logic](first-order-logic.md) — syntax, semantics, models, completeness, compactness, limitations
- [Proof Systems](proof-systems.md) — natural deduction, sequent calculus, soundness and completeness

## Table of Contents

- [Beyond First-Order Logic](#beyond-first-order-logic)
- [Simply Typed Lambda Calculus](#simply-typed-lambda-calculus)
- [Curry-Howard Correspondence](#curry-howard-correspondence)
- [Intuitionistic Logic](#intuitionistic-logic)
- [Dependent Types](#dependent-types)
- [Inductive Types and Recursion](#inductive-types-and-recursion)
- [Proof Assistants](#proof-assistants)
- [Applications](#applications)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Beyond First-Order Logic

**First-order logic** quantifies only over individuals (elements of the domain).

**Second-order logic** extends this with quantification over predicates and relations:

$$\forall P \exists x P(x) \quad \text{(every non-empty predicate has an element)}$$
$$\forall R (\forall x \forall y \forall z (R(x, y) \land R(x, z) \to y = z)) \quad \text{(R is a partial function)}$$

**Key differences from FOL:**

| Property | FOL | SOL |
|----------|-----|-----|
| Quantifies over | Individuals | Individuals, predicates, functions |
| Compactness | Yes | No |
| Lowenheim-Skolem | Yes | No |
| Complete axiomatization | Yes (Godel) | No (Godel's incompleteness) |
| Decidable validity | No (semi-decidable) | No (not even semi-decidable) |
| Categorical theories | No | Yes (e.g., arithmetic, real numbers) |

**Higher-order logic** continues this hierarchy: third-order quantifies over predicates of predicates, and so on.

**Why higher-order matters for computing:**

Induction is not expressible in FOL but is expressible in SOL:

$$\forall P (P(0) \land \forall n (P(n) \to P(n+1)) \to \forall n P(n))$$

This quantifies over all properties $P$, which is second-order. Every practical programming language that supports induction or recursion relies on higher-order reasoning.

**Type theory** provides a more constructive and computational alternative to set-theoretic higher-order logic.

### Simply Typed Lambda Calculus

The simply typed lambda calculus ($\lambda^\to$) is the core language of typed functional programming.

**Syntax:**

$$t ::= x \mid c \mid \lambda x : T. t \mid t_1 t_2$$

**Types:**

$$T ::= \text{Bool} \mid \text{Int} \mid T \to T \mid T \times T$$

**Typing rules (natural deduction style):**

$$\frac{x : T \in \Gamma}{\Gamma \vdash x : T} (Var)$$

$$\frac{\Gamma, x : T \vdash e : U}{\Gamma \vdash \lambda x : T. e : T \to U} (\to I)$$

$$\frac{\Gamma \vdash e_1 : T \to U \quad \Gamma \vdash e_2 : T}{\Gamma \vdash e_1 e_2 : U} (\to E)$$

**Example derivations:**

$$\frac{x : \text{Int} \in \{x : \text{Int}\}}{\{x : \text{Int}\} \vdash x : \text{Int}}$$
$$\vdash \lambda x : \text{Int}. x : \text{Int} \to \text{Int}$$

**The identity function:**

$$\lambda x : \alpha. x : \alpha \to \alpha$$

In Haskell:

```haskell
id :: a -> a
id x = x
```

In Rust:

```rust
fn id<T>(x: T) -> T { x }
```

**Evaluation ($\beta$-reduction):**

$$(\lambda x : T. e_1) e_2 \to_\beta e_1[e_2/x]$$

**Normalization:** Every well-typed term in the simply typed lambda calculus reduces to a normal form (no infinite reductions). This corresponds to the fact that all programs in simply typed languages terminate — which is both a feature (no infinite loops) and a limitation (no Turing completeness).

**Curry-Howard for simple types:**

| Programming | Logic |
|-------------|-------|
| Type $T$ | Proposition $T$ |
| Term $t : T$ | Proof of $T$ |
| Function type $T \to U$ | Implication $T \to U$ |
| $\lambda x : T. e$ | $\to$ introduction |
| Application $e_1 e_2$ | $\to$ elimination (modus ponens) |
| Product type $T \times U$ | Conjunction $T \land U$ |
| Sum type $T + U$ | Disjunction $T \lor U$ |
| Unit type | True ($\top$) |
| Empty type | False ($\bot$) |

### Curry-Howard Correspondence

The **Curry-Howard correspondence** (also called proofs-as-programs) states:

> A proof is a program, and the formula it proves is its type.

**Three levels of correspondence:**

| Level | Logic | Type Theory | Programming |
|-------|-------|-------------|-------------|
| **Propositions as Types** | Proposition $\phi$ | Type $\tau$ | Type of a program |
| **Proofs as Terms** | Proof of $\phi$ | Term $t : \tau$ | Program with type $\tau$ |
| **Proof check as Type check** | Proof correctness | Term type-checks | Program type-checks |

**The fundamental mapping:**

```
Proposition    Type        Programming language
──────────────────────────────────────────────
p ∧ q          P × Q       Pair (product type)
p ∨ q          P + Q       Either (sum type)
p → q          P → Q       Function type
⊥              Void        Empty type (uninhabited)
⊤              Unit        Singleton type
∀x P(x)        Πx: A. P(x)   Dependent product (forall)
∃x P(x)        Σx: A. P(x)   Dependent sum (exists)
```

**A concrete example in Haskell:**

```haskell
-- Proposition: A → B → A  (always true in logic)
-- Proof: a function of type a -> b -> a

proof :: a -> b -> a
proof x y = x

-- The term `\x y -> x` is a proof of the proposition a → b → a
```

**In Lean:**

```lean
-- The same theorem in Lean
theorem k_axiom {a b : Prop} (h_a : a) (h_b : b) : a := h_a

-- Using tactics
theorem k_axiom' {a b : Prop} (h_a : a) (h_b : b) : a := by
  exact h_a
```

**The Curry-Howard isomorphism reveals:**

- A **type-checker** is a **proof-checker**: if a program type-checks, it corresponds to a valid proof.
- A **compiler** that erases types is a **proof normalizer**.
- **Type inference** is **proof search** (though with restrictions to make it decidable).

### Intuitionistic Logic

Intuitionistic (or constructive) logic rejects two classical principles:

1. **Law of Excluded Middle (LEM):** $p \lor \neg p$
2. **Double Negation Elimination:** $\neg\neg p \to p$

**Brouwer-Heyting-Kolmogorov (BHK) interpretation:**

A proof of | is
-----------|----
$\phi \land \psi$ | A proof of $\phi$ together with a proof of $\psi$
$\phi \lor \psi$ | A proof of $\phi$ or a proof of $\psi$ (with an indication of which)
$\phi \to \psi$ | A construction that transforms a proof of $\phi$ into a proof of $\psi$
$\neg \phi$ | A construction that transforms a proof of $\phi$ into a proof of $\bot$
$\exists x \phi(x)$ | An object $a$ together with a proof of $\phi(a)$
$\forall x \phi(x)$ | A construction that, given any $a$, produces a proof of $\phi(a)$

**Key difference from classical logic:**
- Classical: $\phi$ is true iff it holds in all models.
- Intuitionistic: $\phi$ is true iff it has a proof.

**Why intuitionistic logic matters for computation:**

The BHK interpretation aligns perfectly with the Curry-Howard correspondence:
- A proof of $\phi \to \psi$ is a **function** from proofs of $\phi$ to proofs of $\psi$.
- A proof of $\phi \lor \psi$ is a pair with a **tag** indicating which disjunct holds.
- A proof of $\exists x \phi(x)$ is a pair of a **witness** and a proof.

This is exactly how sum types, product types, and existential types work in programming languages.

```haskell
-- A proof of A → B → A in intuitionistic logic (also valid classically)
ex1 :: a -> b -> a
ex1 a _ = a

-- Excluded middle is NOT provable in intuitionistic logic
-- em :: Either a (a -> Void)   -- Cannot implement this!
```

**Godel-Gentzen translation:** Every classical theorem can be translated to an intuitionistically valid theorem by inserting $\neg\neg$ in strategic places. This means classical logic can be embedded in intuitionistic logic.

### Dependent Types

Dependent types allow types to depend on values. This elevates type checking to verifying program properties.

**Pi types ($\Pi$-types):** A type family indexed by values:

$$\Pi x : A. B(x)$$

Inhabitants are functions where the return type $B(x)$ can depend on the argument $x$.

```lean
-- In Lean: a function that returns a type depending on the input
def vec_type (n : Nat) : Type :=
  match n with
  | 0 => Unit
  | _+1 => Nat × vec_type (n-1)

-- Π-type: (n : Nat) → vec_type n
-- Each call with a different n returns a different type
```

**Sigma types ($\Sigma$-types):** Existential quantification:

$$\Sigma x : A. B(x)$$

Inhabitants are pairs $(a, b)$ where $a : A$ and $b : B(a)$.

```lean
-- Σ-type: existence of a natural number satisfying a property
def has_sqrt (n : Nat) : Type :=
  Σ (x : Nat), x * x = n

-- A proof that 9 has a square root
theorem nine_has_sqrt : has_sqrt 9 :=
  ⟨3, by native_decide⟩
```

**Dependent types in practice:**

```lean
-- Vector of length n (dependent type)
inductive Vec (α : Type) : Nat → Type where
  | nil  : Vec α 0
  | cons : α → Vec α n → Vec α (n+1)

-- Type-safe append: length is statically known
def append {α : Type} {m n : Nat}
    (xs : Vec α m) (ys : Vec α n) : Vec α (m + n) :=
  match xs with
  | Vec.nil => ys
  | Vec.cons x xs' => Vec.cons x (append xs' ys)
```

In Idris, a similar language:

```idris
-- Idris: Vect is a dependent type
append : Vect n a -> Vect m a -> Vect (n + m) a
append [] ys = ys
append (x :: xs) ys = x :: append xs ys

-- The type checker verifies the length equation
```

**Agda:**

```agda
-- Agda: dependent pattern matching
data Vec (A : Set) : ℕ → Set where
  []  : Vec A 0
  _∷_ : A → Vec A n → Vec A (suc n)

_++_ : Vec A m → Vec A n → Vec A (m + n)
[]       ++ ys = ys
(x ∷ xs) ++ ys = x ∷ (xs ++ ys)
```

**Dependent types vs. refinement types:**

| Type System | Expressiveness | Decidability |
|-------------|---------------|--------------|
| Simple types | Low | Decidable |
| Polymorphic types (System F) | Medium | Undecidable (but restricted forms decidable) |
| Dependent types | High | Undecidable (interactive proving needed) |
| Refinement types (Liquid Haskell) | Medium | Decidable (SMT-based) |

### Inductive Types and Recursion

Inductive types define data types by their constructors, and come with a **principle of induction** that is both a proof principle and a recursion mechanism.

**Natural numbers as an inductive type:**

```lean
inductive Nat : Type where
  | zero : Nat
  | succ : Nat → Nat
```

This definition simultaneously gives:
- The type `Nat`
- Constructors `zero : Nat` and `succ : Nat → Nat`
- The **induction principle**: to prove $P(n)$ for all $n$, prove $P(0)$ and that $P(n) \to P(n+1)$

**Recursive functions:**

```lean
def add (n m : Nat) : Nat :=
  match n with
  | 0 => m
  | succ k => succ (add k m)

-- The termination checker ensures all recursive calls are on structurally smaller arguments
```

**Proving properties by induction:**

```lean
theorem add_zero (n : Nat) : add n 0 = n := by
  induction n with
  | zero => rfl
  | succ n ih =>
    simp [add, ih]

theorem add_comm (n m : Nat) : add n m = add m n := by
  induction n with
  | zero => simp [add]
  | succ n ih =>
    simp [add, ih, add_succ]
```

**W types (well-founded trees):** The general type of inductive types, used to model all inductive constructions.

### Proof Assistants

Proof assistants are interactive systems for developing formal proofs using type theory.

| Assistant | Core Logic | Tactics | Automation |
|-----------|-----------|---------|------------|
| **Coq** | Calculus of Inductive Constructions (CIC) | Ltac, SSReflect | ring, omega, lia, auto |
| **Lean** | Calculus of Constructions + inductive types | `by` blocks, `simp`, `omega` | `native_decide`, `omega` |
| **Agda** | Martin-Lof Type Theory | No separate tactic language | `auto` |
| **Isabelle/HOL** | Higher-Order Logic (set-theoretic) | `auto`, `sledgehammer` | Sledgehammer, SMT |

#### Coq Example

```coq
(* Coq: proof of commutativity of addition *)

Theorem plus_comm : forall n m : nat, n + m = m + n.
Proof.
  intros n m.
  induction n as [| n' IH].
  - (* n = 0 *)
    simpl. rewrite <- plus_n_O. reflexivity.
  - (* n = S n' *)
    simpl. rewrite IH. rewrite plus_n_Sm. reflexivity.
Qed.
```

#### Lean Example

```lean
-- Lean: same theorem
theorem add_comm (n m : Nat) : n + m = m + n := by
  induction n with
  | zero =>
    simp [Nat.add_zero, Nat.zero_add]
  | succ n ih =>
    simp [Nat.add_succ, Nat.succ_add, ih]
```

#### Agda Example

```agda
-- Agda: commutativity
comm : (n m : ℕ) → n + m ≡ m + n
comm zero m = sym (identityʳ m)
comm (suc n) m = begin
  suc (n + m) ≡⟨ cong suc (comm n m) ⟩
  suc (m + n) ≡⟨ sym (cong suc (identityʳ m)) ⟩
  m + suc n ∎
```

**Applications of proof assistants:**
- **CompCert:** verified C compiler in Coq (proven correct compilation)
- **seL4:** verified microkernel in Isabelle/HOL
- **F*:** a verification-oriented programming language with Z3 automation
- **Mathlib:** Lean's mathematical library, formalizing advanced mathematics

### Applications

#### Functional Programming Language Design

Every major functional language incorporates insights from type theory:

**Haskell:** System F (polymorphic lambda calculus) + type classes (a form of parametric polymorphism with ad-hoc overloading):

```haskell
-- System F: type variables
id :: forall a. a -> a
id x = x

-- Type classes (inspired by category theory and dependent types)
class Functor f where
    fmap :: (a -> b) -> f a -> f b
```

**Rust:** ownership types (a form of substructural/linear types):

```rust
// Linear types prevent use-after-move
fn consume(x: String) {
    // x is consumed here
}

fn main() {
    let s = String::from("hello");
    consume(s);
    // println!("{}", s);  // Compile error: s is moved
}
```

**Scala:** path-dependent types (a limited form of dependent types):

```scala
trait Animal {
  type Food
  def eat(food: Food): Unit
}

class Cow extends Animal {
  type Food = Grass
  def eat(grass: Grass): Unit = {}
}
```

#### Verification of Cryptographic Protocols

Dependent types allow encoding protocol invariants at the type level:

```lean
-- Type-level representation of message states
inductive MsgState : Type where
  | Init : MsgState
  | Sent : MsgState
  | Ackd : MsgState

def send (s : MsgState) : MsgState :=
  match s with
  | MsgState.Init => MsgState.Sent
  | _ => s  -- error: cannot send twice

-- The type system prevents sending the same message twice
```

#### Formalizing Mathematics

Proof assistants have formalized major theorems:

- **Feit-Thompson theorem** (finite group theory) — Coq
- **Four color theorem** — Coq
- **Kepler conjecture** (Flyspeck project) — HOL Light
- **Liquid Tensor Experiment** — Lean
- **Perfectoid spaces** — Lean

```lean
-- A small excerpt from mathlib: the irrationality of sqrt(2)
theorem sqrt_two_irrational : Irrational (Real.sqrt 2) := by
  rintro ⟨a, b, hb, h⟩
  apply hb
  have hsq : (a : ℚ) ^ 2 = 2 * (b : ℚ) ^ 2 := by
    -- algebraic manipulation using h
    sorry  -- full proof in mathlib
  -- ... leads to contradiction using parity argument
```

## Glossary

| Term | Definition |
|------|------------|
| Higher-order logic | Logic that allows quantification over predicates and functions |
| Second-order logic | FOL plus quantification over subsets and relations |
| Type theory | Formal system treating types as objects, foundation of programming languages |
| Lambda calculus | Formal system for function abstraction and application |
| Simply typed lambda calculus | Lambda calculus with a simple type system |
| System F | Polymorphic lambda calculus with universal types |
| Dependent type | A type that depends on a value |
| Pi type ($\Pi$) | A type of functions where the return type depends on the argument |
| Sigma type ($\Sigma$) | A type of pairs where the second component's type depends on the first |
| Curry-Howard correspondence | Proofs correspond to programs, propositions to types |
| BHK interpretation | Constructive interpretation of logical connectives in terms of proofs |
| Intuitionistic logic | Logic without excluded middle and double negation elimination |
| LEM | Law of Excluded Middle ($p \lor \neg p$) |
| Inductive type | A type defined by its constructors, with induction principle |
| Inductive family | A family of types indexed by a value |
| Proof assistant | Interactive tool for developing formal proofs |
| Normalization | Every term reduces to a normal form; corresponds to logical consistency |
| Type inhabitation | The problem of finding a term of a given type (corresponds to proof search) |

## Quick References

- [Types and Programming Languages, Pierce](https://www.cis.upenn.edu/~bcpierce/tapl/) — the standard introduction to type theory and programming languages
- [Software Foundations, Pierce et al.](https://softwarefoundations.cis.upenn.edu/) — Coq-based introduction to formal verification
- [Theorem Proving in Lean](https://leanprover.github.io/theorem_proving_in_lean/) — official Lean tutorial
- [Homotopy Type Theory (HoTT book)](https://homotopytypetheory.org/book/) — advanced type theory with univalent foundations
- [Curry-Howard Correspondence (Stanford Encyclopedia)](https://plato.stanford.edu/entries/curry-howard/)
- [Coq Proof Assistant](https://coq.inria.fr/)
- [Lean Theorem Prover](https://leanprover.github.io/)
- [Agda Programming Language](https://wiki.portal.chalmers.se/agda/)

## Next Steps

This document concludes the logic module sequence. For further study, consider:

- [Boolean Algebra & Logic Circuits](boolean-algebra.md) — algebraic theory of logic gates and minimization
- **Category Theory** — the abstract foundation underlying type theory and logic (not yet covered in DevBook)
- [Applications in Computing](applications-in-computing.md) — practical uses of logic in database theory, AI, and verification
