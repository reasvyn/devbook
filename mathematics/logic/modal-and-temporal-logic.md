# Modal & Temporal Logics

## Description

Modal logic extends propositional logic with operators for necessity and possibility, enabling reasoning about what must be true versus what could be true. Temporal logic adds operators for time — always, eventually, next, and until. Together, they are the primary specification languages for verifying concurrent, reactive, and distributed systems. Model checking, the most widely used formal verification technique in industry, is built on temporal logic.

## Prerequisites

- [Propositional Logic](propositional-logic.md) — connectives, truth tables, logical equivalence
- Basic understanding of state machines and transition systems (helpful but not required)

## Table of Contents

- [Modal Logic: Necessity and Possibility](#modal-logic-necessity-and-possibility)
- [Kripke Semantics](#kripke-semantics)
- [Modal Axiom Systems](#modal-axiom-systems)
- [Temporal Operators](#temporal-operators)
- [Linear Temporal Logic (LTL)](#linear-temporal-logic-ltl)
- [Computation Tree Logic (CTL)](#computation-tree-logic-ctl)
- [Model Checking](#model-checking)
- [Applications](#applications)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Modal Logic: Necessity and Possibility

Modal logic adds two operators to propositional logic:

| Operator | Meaning | Symbol |
|----------|---------|--------|
| Necessity | "It is necessary that" | $\Box$ (box) |
| Possibility | "It is possible that" | $\Diamond$ (diamond) |

$$\Box \phi \quad \text{means } \phi \text{ is necessarily true}$$
$$\Diamond \phi \quad \text{means } \phi \text{ is possibly true}$$

**Duality:** The operators are interdefinable:

$$\Diamond \phi \equiv \neg \Box \neg \phi$$
$$\Box \phi \equiv \neg \Diamond \neg \phi$$

**Interpretation:** "It is possible that $\phi$" means "it is not necessary that $\neg \phi$."

**Epistemic interpretation (knowledge):**
- $K_i \phi$: agent $i$ knows that $\phi$
- This is a variant of modal logic for multi-agent systems

**Deontic interpretation (obligation):**
- $O \phi$: it is obligatory that $\phi$
- $P \phi$: it is permitted that $\phi$

**In program verification,** the operators are often given a computational interpretation:

- $\Box \phi$: $\phi$ holds in all reachable states
- $\Diamond \phi$: $\phi$ holds in some reachable state

### Kripke Semantics

A **Kripke frame** is a pair $(W, R)$ where:
- $W$ is a non-empty set of **possible worlds** (or states)
- $R \subseteq W \times W$ is an **accessibility relation**

A **Kripke model** $(W, R, V)$ adds a **valuation** $V: \text{Atoms} \to \mathcal{P}(W)$ that assigns to each atomic proposition the set of worlds where it is true.

**Satisfaction ($M, w \models \phi$):**

| Formula | $M, w \models \phi$ iff |
|---------|------------------------|
| $p$ | $w \in V(p)$ |
| $\neg \phi$ | $M, w \not\models \phi$ |
| $\phi \land \psi$ | $M, w \models \phi$ and $M, w \models \psi$ |
| $\phi \lor \psi$ | $M, w \models \phi$ or $M, w \models \psi$ |
| $\Box \phi$ | For every $v$ such that $wRv$, $M, v \models \phi$ |
| $\Diamond \phi$ | There exists $v$ such that $wRv$ and $M, v \models \phi$ |

$\Box \phi$ is true at world $w$ iff $\phi$ is true in **all** worlds accessible from $w$.
$\Diamond \phi$ is true at world $w$ iff $\phi$ is true in **some** world accessible from $w$.

**Visualizing a Kripke model:**

```
Worlds: w1, w2, w3, w4
Accessibility: w1 → w2, w1 → w3, w3 → w4

V(p) = {w2, w4}
V(q) = {w3}

At w1: box p is false (p is not true in w3)
At w1: diamond q is true (q is true in w3)
At w3: box p is... w3→w4, p holds at w4, so box p is true at w3
```

**Frame properties and corresponding axioms:**

The accessibility relation $R$ can have various properties that correspond to different modal axioms:

| Property | Definition | Axiom | Name |
|----------|------------|-------|------|
| Reflexive | $\forall w: wRw$ | $\Box \phi \to \phi$ | T |
| Symmetric | $\forall u,v: uRv \to vRu$ | $\phi \to \Box \Diamond \phi$ | B |
| Transitive | $\forall u,v,w: uRv \land vRw \to uRw$ | $\Box \phi \to \Box \Box \phi$ | 4 |
| Euclidean | $\forall u,v,w: uRv \land uRw \to vRw$ | $\Diamond \phi \to \Box \Diamond \phi$ | 5 |
| Serial | $\forall w: \exists v: wRv$ | $\Box \phi \to \Diamond \phi$ | D |

### Modal Axiom Systems

Different combinations of axioms define different modal logics:

| System | Axioms | Accessibility | Interpretation |
|--------|--------|--------------|---------------|
| **K** | All tautologies + K + MP + Nec | None required | Minimal modal logic |
| **T** | K + T | Reflexive | Knowledge: if known, true |
| **S4** | T + 4 | Reflexive + Transitive | Knowledge + positive introspection |
| **S5** | S4 + 5 | Reflexive + Transitive + Euclidean (equivalence relation) | Knowledge + both introspections |
| **D** | K + D | Serial | Deontic: obligation implies permission |
| **KT** | K + T | Reflexive | Alethic: necessary implies actual |

**Axiom K:** $\Box (\phi \to \psi) \to (\Box \phi \to \Box \psi)$

This is the fundamental axiom of all normal modal logics. It says: "If necessarily $\phi \to \psi$, then if necessarily $\phi$, then necessarily $\psi$."

**Necessitation rule (Nec):** If $\vdash \phi$, then $\vdash \Box \phi$.

Note: Necessitation does NOT say $\phi \to \Box \phi$. It says that if $\phi$ is a theorem (provable from nothing), then $\Box \phi$ is also a theorem.

**Important derivations:**

In S4 (and stronger), $\Box$ is **idempotent**:

$$\Box \phi \equiv \Box \Box \phi$$

In S5, both $\Box$ and $\Diamond$ collapse to simple patterns:

$$\Diamond \Box \phi \equiv \Box \phi$$
$$\Box \Diamond \phi \equiv \Diamond \phi$$

**In epistemic logic:**
- $K \phi \to \phi$ (knowledge implies truth) — axiom T
- $K \phi \to K K \phi$ (positive introspection) — axiom 4
- $\neg K \phi \to K \neg K \phi$ (negative introspection) — axiom 5

S5 corresponds to the **ideal knowledge** scenario where agents have perfect reasoning and full introspection.

**In deontic logic:**
- $O \phi$ (obligation) replaces $\Box \phi$
- $P \phi$ (permission) replaces $\Diamond \phi$
- Axiom D: $O \phi \to P \phi$ (if obligated then permitted)
- Note: Axiom T ($O \phi \to \phi$) does NOT hold — obligations can be violated

### Temporal Operators

Temporal logic adds operators for reasoning about time. There are two main views of time:

**Linear time:** At each moment, there is exactly one possible future.
**Branching time:** At each moment, there may be multiple possible futures.

#### Linear Temporal Operators

| Operator | Name | Meaning |
|----------|------|---------|
| $X \phi$ or $\circ \phi$ | Next | $\phi$ holds in the next state |
| $G \phi$ or $\Box \phi$ | Globally (Always) | $\phi$ holds in every future state |
| $F \phi$ or $\Diamond \phi$ | Eventually (Finally) | $\phi$ holds in some future state |
| $\phi U \psi$ | Until | $\phi$ holds until $\psi$ holds (and $\psi$ eventually holds) |
| $\phi W \psi$ | Weak until (Unless) | $\phi$ holds until $\psi$ holds (or $\psi$ never holds) |

**Dualities:**
$$F \phi \equiv \neg G \neg \phi \quad \text{(eventually = not always not)}$$
$$G \phi \equiv \neg F \neg \phi \quad \text{(always = not eventually not)}$$
$$X \phi \equiv \neg X \neg \phi \quad \text{(next is self-dual)}$$

**Definitions:**
$$F \phi \equiv \top U \phi$$
$$G \phi \equiv \neg F \neg \phi$$

### Linear Temporal Logic (LTL)

LTL formulas are evaluated over infinite sequences of states (paths).

**Syntax:**

$$\phi ::= p \mid \neg \phi \mid \phi \land \phi \mid \phi \lor \phi \mid \phi \to \phi \mid X \phi \mid G \phi \mid F \phi \mid \phi U \phi$$

**Semantics (over an infinite path $\pi = s_0, s_1, s_2, \dots$):**

| Formula | $\pi \models \phi$ iff |
|---------|----------------------|
| $p$ | $p$ holds in $s_0$ |
| $\neg \phi$ | $\pi \not\models \phi$ |
| $X \phi$ | $\pi_1 \models \phi$ (the suffix starting at $s_1$) |
| $G \phi$ | For all $i \ge 0$, $\pi_i \models \phi$ |
| $F \phi$ | There exists $i \ge 0$ such that $\pi_i \models \phi$ |
| $\phi U \psi$ | There exists $i \ge 0$ such that $\pi_i \models \psi$ and for all $0 \le j < i$, $\pi_j \models \phi$ |

**Common specification patterns:**

```
Property            LTL Formula
─────────────────────────────────────────────────
Safety (bad never)  G(not bad)
Liveness (good)     F(good)
Response            G(request → F response)
Precedence          (¬a) U b
Starvation-free     GF(enabled) → GF(executed)
Mutual exclusion    G(¬(in_cs1 ∧ in_cs2))
```

**Expressiveness:** LTL cannot express:
- "There exists a path where $\phi$ holds" (requires branching time)
- "Along every path, eventually $\phi$ holds" (this IS LTL: $G F \phi$ means "always eventually along this path")

### Computation Tree Logic (CTL)

CTL is a branching-time logic. Path quantifiers are paired with temporal operators:

| Path Quantifier | Meaning |
|-----------------|---------|
| $A$ | Along All paths |
| $E$ | There Exists a path |

**CTL syntax requires every temporal operator to be immediately preceded by a path quantifier:**

$$\phi ::= p \mid \neg \phi \mid \phi \land \phi \mid \phi \lor \phi \mid \phi \to \phi \mid AX \phi \mid EX \phi \mid AG \phi \mid EG \phi \mid AF \phi \mid EF \phi \mid A(\phi U \psi) \mid E(\phi U \psi)$$

**Semantics:**

| Formula | $M, s \models \phi$ iff |
|---------|------------------------|
| $AX \phi$ | For all successors $s'$ of $s$, $\phi$ holds in $s'$ |
| $EX \phi$ | There exists a successor $s'$ of $s$ such that $\phi$ holds in $s'$ |
| $AG \phi$ | $\phi$ holds in all states along all paths from $s$ |
| $EG \phi$ | There exists a path from $s$ where $\phi$ holds globally |
| $AF \phi$ | Along every path from $s$, $\phi$ eventually holds |
| $EF \phi$ | There exists a path from $s$ where $\phi$ eventually holds |
| $A(\phi U \psi)$ | Along every path from $s$, $\phi$ holds until $\psi$ holds |
| $E(\phi U \psi)$ | There exists a path from $s$ where $\phi$ holds until $\psi$ holds |

**CTL equivalences:**

$$AG \phi \equiv \neg EF \neg \phi$$
$$EG \phi \equiv \neg AF \neg \phi$$
$$AF \phi \equiv \neg EG \neg \phi$$
$$EF \phi \equiv \neg AG \neg \phi$$

**CTL fixed-point characterizations:**

$$AG \phi = \phi \land AX AG \phi \quad \text{(greatest fixed point)}$$
$$AF \phi = \phi \lor AX AF \phi \quad \text{(least fixed point)}$$
$$EG \phi = \phi \land EX EG \phi \quad \text{(greatest fixed point)}$$
$$EF \phi = \phi \lor EX EF \phi \quad \text{(least fixed point)}$$

These equations are used in symbolic model checking algorithms.

#### CTL vs. LTL: Expressiveness

LTL and CTL have incomparable expressiveness:

| Property | Expressible in |
|----------|---------------|
| $G F p$ (infinitely often p) | LTL, not CTL |
| $AG EF p$ (from every state, it is possible to reach p) | CTL, not LTL |
| $GF p \to GF q$ (if p infinitely often then q infinitely often) | LTL, not CTL |
| $AG (p \to AF q)$ (every p leads to q) | CTL and LTL |

**CTL star ($\text{CTL}^*$)** unifies both: temporal operators can be freely combined with path quantifiers.

### Model Checking

**Model checking** is the algorithmic verification of a temporal logic formula against a finite-state system:

$$\text{Given: } M \text{ (a finite Kripke model), } \phi \text{ (a temporal logic formula)}$$
$$\text{Decide: } M \models \phi \quad \text{(does every path starting from initial states satisfy } \phi \text{?)}$$

**Model checking algorithms:**

| Logic | Algorithm | Complexity |
|-------|-----------|------------|
| LTL | Tableau / Buchi automaton intersection | $O(2^{|\phi|} \times |M|)$ |
| CTL | Fixed-point computation over state graph | $O(|M| \times |\phi|)$ |
| CTL* | Combination of LTL + CTL | $O(2^{|\phi|} \times |M|)$ |

**Explicit-state model checking (SPIN):** explores the state graph explicitly.

**Symbolic model checking (NuSMV):** uses Binary Decision Diagrams (BDDs) to represent sets of states and transitions compactly.

```python
# Pseudo-code for CTL model checking: EF p
def check_EF(model, p):
    # EF p = p ∨ EX EF p (least fixed point)
    states = set()
    while True:
        # {s | p in s or there exists s' in states with s→s'}
        new_states = p_states | predecessors(states)
        if new_states == states:
            return states
        states = new_states
```

**State explosion problem:** The number of states grows exponentially with the number of variables. Mitigations include:
- Symbolic representation (BDDs)
- Abstraction and refinement (CEGAR)
- Bounded model checking (SAT-based, up to a finite depth)
- Partial order reduction (for concurrent systems)

### Applications

#### Distributed Systems Verification

Temporal logic is used to specify and verify distributed protocols:

**Mutual exclusion (mutex) properties:**
- Safety: $G \neg (in_{cs1} \land in_{cs2})$ — never both in critical section
- Liveness: $G (want_{cs1} \to F in_{cs1})$ — every request is eventually granted
- Starvation freedom: $GF want_{cs1} \to GF in_{cs1}$ — processes that keep trying eventually succeed

**Byzantine fault tolerance protocols** are specified in TLA+, which combines temporal logic with set theory and actions.

```tla+
(* TLA+ specification of Dijkstra's mutual exclusion *)
VARIABLES pc, flag, turn
Mutex ==
    ∧ pc = "outside"
    ∧ pc' = "waiting"
    ∧ flag' = [flag EXCEPT ![self] = TRUE]
    ∧ turn' = choose i ∈ Proc: TRUE

CS ==
    ∧ pc = "critical"
    ∧ pc' = "outside"
    ∧ flag' = [flag EXCEPT ![self] = FALSE]

(* Specification *)
Spec == Init ∧ □[Next]_vars
```

#### Safety and Liveness Properties

Properties are classified into:

- **Safety:** "nothing bad happens" ($G \neg \text{bad}$). Checkable on finite prefixes.
- **Liveness:** "something good eventually happens" ($F \text{good}$). Requires infinite paths.
- **Fairness:** "if something is infinitely often possible, it should happen" ($GF \text{enabled} \to GF \text{taken}$).

**Alpern-Schneider theorem:** Every property is the intersection of a safety property and a liveness property.

#### Hardware Verification

Temporal logic model checking is routinely used in hardware design:

```verilog
// Verilog assertion: after reset, signal ack must eventually go high
assert property (posedge clk)
    (reset ##0 !ack) |=> eventually ack;

// LTL: G(reset → F ack)
```

Companies like Intel, AMD, IBM, and ARM use model checking to verify cache coherence protocols, bus arbiter logic, and pipeline control.

#### Program Analysis and Bug Finding

Temporal logic specifications can be checked on program traces:

```python
# A lock-safety specification: between lock() and unlock(), 
# the lock is held exactly once
# LTL: G(lock_called → X((¬lock_called) U unlock_called))
```

- **Java PathFinder (JPF)** — model checker for Java bytecode
- **CBMC** — bounded model checker for C programs
- **Spin** — explicit-state model checker for distributed algorithms

## Study Cases

### Case 1: Verifying a Traffic Light Controller

A traffic light cycles through Green → Yellow → Red → Green.

States: {Green, Yellow, Red}

Properties to verify:
1. Safety: Never Green and Red simultaneously on crossing roads
   $AG \neg (Green_{EW} \land Green_{NS})$

2. Liveness: Every road eventually gets green
   $AG (AF Green_{EW})$ and $AG (AF Green_{NS})$

3. Response: Yellow always precedes Red
   $AG (Red \to A(\text{Yellow} U \text{Green}))$

A model checker would confirm these properties or produce a counterexample trace.

### Case 2: Mutual Exclusion Protocol (Peterson's Algorithm)

```python
# Peterson's algorithm for two processes
flag = [False, False]
turn = 0

def process(i):
    j = 1 - i
    while True:
        # Entry protocol
        flag[i] = True
        turn = j
        while flag[j] and turn == j:
            pass  # busy wait
        
        # Critical section
        in_cs[i] = True
        # ... work ...
        in_cs[i] = False
        
        # Exit protocol
        flag[i] = False
        
        # Non-critical section
        # ... work ...
```

LTL properties:
```
mutex:  G ¬(in_cs[0] ∧ in_cs[1])           # safety
liveness:  G (want[i] → F in_cs[i])         # liveness (under fair scheduling)
```

### Case 3: CTL Model Checking a Simple Protocol

A simple server handles requests: states are {Idle, Processing, Done}.

Initial state: Idle

Transitions:
- Idle → Processing (on request)
- Processing → Done (on completion)
- Processing → Processing (retry)
- Done → Idle (reset)

CTL specifications:
- $AG (Request \to AF Done)$ — every request leads to eventual completion
- $AG (Processing \to AX Processing \lor AX Done)$ — from processing, next state is processing or done
- $EF Done$ — it is possible to reach a Done state

## Examples

### Example 1: LTL Mutual Exclusion

Given two processes with critical sections $cs_1$, $cs_2$:

```
Mutual exclusion:  G(¬(cs_1 ∧ cs_2))
No strict sequencing:  ¬G(cs_1 → X(¬cs_2))   (cs_2 can also enter after cs_1)
Individual progress:  G(attempt_1 → F cs_1)   (under fairness)
```

### Example 2: Modal Logic Puzzle

"Necessarily, if it is raining then the ground is wet."
$$\Box (\text{rain} \to \text{wet})$$

"It is not raining."
$$\neg \text{rain}$$

Question: Does it follow that the ground is necessarily not wet?
$$\Box \neg \text{wet}$$

No. $\Box(p \to q)$ and $\neg p$ does not imply $\Box \neg q$. In the actual world, the ground may be wet for other reasons. In non-actual worlds, it could be raining.

### Example 3: Symbolic CTL Model Checking

The formula $AG \phi$ can be computed as the greatest fixed point:

```
X := set of all states
repeat
    X' := {s | φ in s AND for all successors s' of s, s' in X}
until X = X'
return X
```

Starting with all states, each iteration removes states that either lack $\phi$ or have a successor outside $X$.

### Example 4: From Natural Language to Temporal Logic

| English | LTL |
|---------|-----|
| "The system never enters an error state." | $G \neg \text{error}$ |
| "Every request is followed by an acknowledgment." | $G (\text{request} \to F \text{ack})$ |
| "After reset, the signal must go high within 3 cycles." | $G (\text{reset} \to X(\neg \text{high}) \land XX(\neg \text{high}) \land XXX \text{high})$ |
| "Between lock and unlock, no other process enters the critical section." | $G (\text{lock} \to (\neg \text{other}_cs) U \text{unlock})$ |

### Example 5: S5 Reasoning

In S5, $\Diamond \Box \phi \equiv \Box \phi$. This follows from the accessibility relation being an equivalence relation (reflexive + symmetric + transitive).

If $\Diamond \Box \phi$ is true, there is a world $w'$ accessible from the actual world where $\Box \phi$ holds. But since accessibility is symmetric and transitive, $\phi$ must be true in all accessible worlds, hence $\Box \phi$ holds in the actual world.

## Glossary

| Term | Definition |
|------|------------|
| Modal logic | Logic extended with necessity ($\Box$) and possibility ($\Diamond$) operators |
| Kripke frame | A pair $(W, R)$ of worlds and an accessibility relation |
| Kripke model | A frame plus a valuation of atomic propositions at each world |
| Accessibility relation | $R \subseteq W \times W$ determining which worlds are considered from a given world |
| World | A state or possible situation in a Kripke model |
| Axiom K | $\Box(\phi \to \psi) \to (\Box\phi \to \Box\psi)$ |
| Axiom T | $\Box\phi \to \phi$ (reflexivity) |
| Axiom 4 | $\Box\phi \to \Box\Box\phi$ (transitivity) |
| Axiom 5 | $\Diamond\phi \to \Box\Diamond\phi$ (Euclidean) |
| Epistemic logic | Modal logic for knowledge and belief |
| Deontic logic | Modal logic for obligation and permission |
| Temporal logic | Logic for reasoning about time |
| LTL | Linear Temporal Logic — formulas evaluated over single paths |
| CTL | Computation Tree Logic — formulas evaluated over branching time |
| Model checking | Algorithmic verification that a system satisfies a temporal formula |
| Safety property | Something bad never happens ($G \neg \text{bad}$) |
| Liveness property | Something good eventually happens ($F \text{good}$) |
| Fairness | An infinite path where certain events occur infinitely often |
| State explosion | Exponential growth of state space in model checking |
| BDD | Binary Decision Diagram, a compact representation of boolean functions |
| CEGAR | Counterexample-Guided Abstraction Refinement |

## Quick References

- [Model Checking, Clarke, Grumberg & Peled](https://mitpress.mit.edu/books/model-checking-second-edition) — the standard reference on model checking
- [Principles of Model Checking, Baier & Katoen](https://mitpress.mit.edu/books/principles-model-checking) — comprehensive textbook
- [Spin Model Checker](https://spinroot.com/) — explicit-state model checker for LTL
- [NuSMV](https://nusmv.fbk.eu/) — symbolic model checker for CTL and LTL
- [TLA+ Tools](https://lamport.azurewebsites.net/tla/tla.html) — specification language with temporal logic
- [Stanford Encyclopedia: Modal Logic](https://plato.stanford.edu/entries/logic-modal/) — comprehensive philosophical reference

## Next Steps

- [Higher-Order Logic & Type Theory](higher-order-logic.md) — going beyond first-order and modal logic with types and computation
