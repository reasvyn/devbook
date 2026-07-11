# Semantics: Meaning in Language & Code

## Description

Semantics is the study of meaning — in natural language, what a sentence conveys; in programming languages, what a program computes. Understanding semantics bridges the gap between syntax (structure) and the actual behavior or interpretation of symbols. For developers, semantic thinking improves API design, type system reasoning, and the ability to write code that says what it means.

## Prerequisites

- [Formal Languages & Grammars](formal-languages.md) — the formal structure of languages
- [Parsing](parsing.md) — syntactic analysis as the foundation for semantic interpretation

## Table of Contents

- [What Is Semantics?](#what-is-semantics)
- [Semantics in Natural vs. Programming Languages](#semantics-in-natural-vs-programming-languages)
- [Lexical Semantics](#lexical-semantics)
- [Semantic Relations Between Words](#semantic-relations-between-words)
- [Compositional Semantics](#compositional-semantics)
- [Semantic Roles](#semantic-roles)
- [Formal Semantics of Programming Languages](#formal-semantics-of-programming-languages)
- [Operational Semantics](#operational-semantics)
- [Denotational Semantics](#denotational-semantics)
- [Axiomatic Semantics](#axiomatic-semantics)
- [Type Systems and Semantic Correctness](#type-systems-and-semantic-correctness)
- [Naming and Meaning in APIs](#naming-and-meaning-in-apis)
- [Domain-Driven Design and Ubiquitous Language](#domain-driven-design-and-ubiquitous-language)
- [Semantic Versioning](#semantic-versioning)
- [Semantic Analysis in Compilers](#semantic-analysis-in-compilers)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### What Is Semantics?

Semantics is the branch of linguistics and logic concerned with meaning. In natural language, semantics asks what words and sentences refer to. In programming languages, semantics asks what programs compute.

**The three layers of language:**

| Layer | Question | Field |
|-------|----------|-------|
| Syntax | What is the structure? | Grammar |
| Semantics | What does it mean? | Meaning |
| Pragmatics | What is the intent/context? | Use |

"Colorless green ideas sleep furiously." — Syntactically perfect, semantically anomalous.

### Semantics in Natural vs. Programming Languages

| Property | Natural language | Programming language |
|----------|----------------|-------------------|
| Ambiguity | Pervasive | Minimal by design |
| Context-dependence | Heavy | Controlled |
| Meaning source | World knowledge + structure | Formal rules |
| Change over time | Evolves constantly | Versioned explicitly |

**The bridge — Compositionality (Frege's principle):** The meaning of a complex expression is determined by the meanings of its parts and the rules used to combine them. Both NL and PL semantics follow this principle.

### Lexical Semantics

**Sense vs. Reference:**

| Concept | Definition | Example |
|---------|------------|---------|
| Sense | Conceptual meaning | "morning star" and "evening star" differ in sense |
| Reference | Actual thing in the world | Both refer to Venus |

**Semantic features:** Words as bundles of binary features:

```
man:   [+HUMAN] [+MALE] [+ADULT]
woman: [+HUMAN] [-MALE] [+ADULT]
```

This mirrors tagged unions and enum variants in programming.

### Semantic Relations Between Words

**Synonymy:** Words with similar meaning (big/large, start/begin). True synonyms are rare.

**Hyponymy:** Type hierarchy (animal -> mammal -> dog -> poodle). The linguistic equivalent of inheritance:

```python
class Animal: pass
class Mammal(Animal): pass
class Dog(Mammal): pass
```

**Hypernymy:** Superordinate category — "animal" is hypernym of "dog".

**Meronymy:** Part-whole — finger is meronym of hand.

**Antonymy:** Complementary (alive/dead), gradable (hot/cold), relational (buy/sell).

**Polysemy vs. Homonymy:** Polysemy — one word, multiple related meanings ("mouse" animal -> device). Homonymy — different words, same form ("bank" financial vs. river).

**WordNet** encodes these relations for English: each word has senses, hypernyms, hyponyms, and meronyms.

### Compositional Semantics

**Lambda calculus for semantics** maps syntactic structure to logical forms:

```
Every dog barks.
Every:  lambda P lambda Q forall x (P(x) -> Q(x))
dog:    lambda x dog(x)
barks:  lambda x barks(x)
Composition: forall x (dog(x) -> barks(x))
```

**Semantic types** mirror Hindley-Milner type systems:

```
dog:     e -> t       (Entity -> Bool)
loves:   e -> e -> t  (curried function)
every:   (e -> t) -> (e -> t) -> t  (higher-order function)
```

### Semantic Roles

Semantic roles describe noun phrase functions relative to the verb:

| Role | Definition | Example |
|------|------------|---------|
| Agent | Performs action | "John opened the door." |
| Patient | Undergoes action | "John opened the door." |
| Instrument | Tool used | "With a key." |
| Source | Origin | "From Paris." |
| Goal | Destination | "To London." |

**Frame semantics (Fillmore):** A frame is a structured set of roles for a situation. FrameNet has over 1,200 frames.

### Formal Semantics of Programming Languages

Three main approaches:

| Approach | Focus | What it specifies |
|----------|-------|-------------------|
| Operational | Step-by-step execution | How the program runs |
| Denotational | Mathematical meaning | What the program computes |
| Axiomatic | Logical properties | What the program guarantees |

### Operational Semantics

Defines program meaning as a sequence of computational steps.

**Structural Operational Semantics (SOS):**

```
<n, env> -> n
<x, env> -> env(x)
<e1 + e2, env> -> n1 + n2   where <e1, env> -> n1, <e2, env> -> n2
```

**Small-step vs. Big-step:**

| Approach | Analogy |
|----------|---------|
| Small-step | Single steps (debugging) |
| Big-step | Whole expression evaluation |

**Operational semantics for a simple imperative language (IMP):**

```
<skip, sigma> -> sigma
<x := a, sigma> -> sigma[x |-> [[a]]sigma]
<if b then c1 else c2, sigma> -> <c1, sigma>  if [[b]]sigma = true
<while b do c, sigma> -> sigma                 if [[b]]sigma = false
<while b do c, sigma> -> <c; while b do c, sigma>  if [[b]]sigma = true
```

### Denotational Semantics

Maps programs to mathematical objects. Compositional by construction.

```
[[n]] = n
[[e1 + e2]] = [[e1]] + [[e2]]
[[lambda x.e]] = lambda v. [[e]][x/v]
[[while b do c]]sigma = fix(lambda f. lambda sigma. if [[b]]sigma then f([[c]]sigma) else sigma)
```

The fixpoint operator handles loops mathematically.

**Domain theory** handles non-termination: bottom (non-termination), every function must be monotone and continuous.

### Axiomatic Semantics

Hoare logic reasons about program correctness with triples:

```
{P} C {Q}
```
If P holds before C and C terminates, Q holds after.

**Rules:**

```
Assignment: {Q[x/a]} x := a {Q}
Sequence:   {P} c1 {R}  {R} c2 {Q} / {P} c1; c2 {Q}
While:      {I and b} c {I} / {I} while b do c {I and not b}
```

**Total vs. Partial correctness:**

| Type | Meaning |
|------|---------|
| Partial | If C terminates, Q holds |
| Total | C terminates and Q holds |

### Type Systems and Semantic Correctness

**Sound type system (progress + preservation):**

```
Progress: A well-typed term is not stuck.
Preservation: If a well-typed term steps, the result is well-typed.
```

**Simply typed lambda calculus:**

```
Types: tau ::= int | bool | tau1 -> tau2
Rules: Gamma |- n : int, Gamma |- e1 + e2 : int
       Gamma, x:tau1 |- e : tau2 => Gamma |- lambda x:tau1.e : tau1->tau2
```

**Subtyping:** `{x: int, y: int} <: {x: int}` (width subtyping). Functions are contravariant in input.

**Semantic errors type systems catch:** Adding number to string, calling non-function, accessing missing field, null dereference.

### Naming and Meaning in APIs

Names carry meaning. Well-named APIs communicate without documentation.

```python
# Poor:
def proc(a, b): return a * b + 3.14

# Better:
def calculate_circle_area(radius): return math.pi * radius ** 2
```

**Semantic map between NL and APIs:**

| NL concept | API equivalent | Example |
|------------|---------------|---------|
| Synonym | Method aliases | size(), length(), count() |
| Hyponym | Inheritance | save_to_file <: persist |
| Antonym | Inverse operations | push() / pop() |
| Meronym | Composition | car.engine.start() |

### Domain-Driven Design and Ubiquitous Language

DDD aligns software semantics with domain semantics:

```python
class Account:
    def deposit(self, amount: Money): ...
    def withdraw(self, amount: Money): ...
```

**Bounded context:** The same word means different things in different contexts ("Customer" in Sales vs. Support vs. Shipping).

**Entity vs. Value Object:** Entity has identity (User with ID). Value Object is defined by attributes (Color with RGB).

### Semantic Versioning

SemVer uses version numbers to communicate change impact:

```
MAJOR.MINOR.PATCH
1.0.0 -> 2.0.0: Breaking API changes
1.0.0 -> 1.1.0: New features, backward-compatible
1.0.0 -> 1.0.1: Bug fixes, backward-compatible
```

### Semantic Analysis in Compilers

After parsing, compilers perform name resolution and type checking:

```python
class NameResolver:
    def resolve(self, name):
        for scope in reversed(self.scopes):
            if name in scope:
                return scope[name]
        raise SemanticError(f"Undefined variable: {name}")
```

**Attribute grammars** add semantic rules to CFG productions:

```
Production              Semantic rule
expr -> expr1 + expr2   expr.type = int if both operands are int else error
```

### Distributional Semantics and Word Embeddings

Distributional semantics is based on the distributional hypothesis: words that appear in similar contexts have similar meanings. This is operationalized through vector representations.

**Count-based methods:** Build a co-occurrence matrix where rows are words, columns are contexts, and cells count how often a word appears in a context. Dimensionality reduction (SVD, PCA) produces dense word vectors:

```python
import numpy as np
from sklearn.decomposition import TruncatedSVD

cooccurrence = np.array([
    [0, 5, 2, 0, 0],  # dog
    [5, 0, 1, 0, 0],  # cat
    [2, 1, 0, 3, 2],  # car
    [0, 0, 3, 0, 4],  # drive
])
svd = TruncatedSVD(n_components=2)
embeddings = svd.fit_transform(cooccurrence)
print("Word embeddings (2D):\n", embeddings)
```

**Prediction-based methods (Word2Vec):** Neural networks predict context from word (skip-gram) or word from context (CBOW). The hidden layer weights become the embedding vectors. These embeddings capture semantic relationships as vector arithmetic:

$$
\text{vec}(\text{king}) - \text{vec}(\text{man}) + \text{vec}(\text{woman}) \approx \text{vec}(\text{queen})
$$

**Contextual embeddings (BERT, ELMo):** Unlike static embeddings, contextual models assign different vectors to the same word in different contexts. "Bank" in "river bank" vs. "savings bank" gets different representations. This resolves polysemy that static embeddings cannot handle.

### Word Sense Disambiguation

Determining which sense of a polysemous word is intended requires semantic context:

**Lesk algorithm:** Compare the dictionary definitions of each sense with the surrounding context words. The sense with the most overlapping words wins.

```python
def lesk_similarity(sense_def, context_words):
    """Score how well a sense definition matches context."""
    def_words = set(sense_def.lower().split())
    context_set = set(w.lower() for w in context_words)
    return len(def_words & context_set)

senses = {
    "bank_1": "financial institution where deposits are held",
    "bank_2": "sloping land beside a river or lake",
}
context = ["I", "need", "to", "deposit", "a", "check"]
best_sense = max(senses, key=lambda s: lesk_similarity(senses[s], context))
print(f"Best sense: {best_sense} -> {senses[best_sense]}")
```

**Supervised WSD:** Train classifiers on sense-annotated corpora like SemCor. Features include surrounding words, part-of-speech tags, and syntactic relations. Modern approaches use contextual embeddings fine-tuned on WSD datasets, achieving over 80% accuracy on standard benchmarks.

### Semantic Parsing

Semantic parsing maps natural language to executable meaning representations:

**SQL generation:** "Show all employees hired after 2020" becomes:

```sql
SELECT * FROM employees WHERE hire_date > '2020-01-01'
```

**Code generation:** "Sort the list of users by name" becomes:

```python
sorted(users, key=lambda u: u.name)
```

Semantic parsing combines syntactic analysis (parsing the sentence structure) with semantic composition (building the meaning representation compositionally). Modern approaches use sequence-to-sequence models with attention and structured decoders that enforce target language syntax.

### Pragmatic Inference in Communication

The distinction between semantics (literal meaning) and pragmatics (intended meaning) is crucial for understanding how humans communicate and how AI systems should interpret instructions:

**Implicature in requirements:** When a stakeholder says "The system should handle large files," semantics gives us the literal meaning. Pragmatics tells us we need to define "large" (100MB? 1GB?), decide what "handle" means (upload? process? store?), and understand that this is a request for clarification, not a specification.

**Presupposition in API names:** The function `delete_user(id)` semantically means "remove the user with this ID from the system." It pragmatically presupposes that a user with this ID exists, that deletion is the right operation (not deactivation), and that the caller has permission. Documenting these presuppositions is the job of API documentation, adding a pragmatic layer to the semantic specification.

## Glossary

| Term | Definition |
|------|------------|
| Agent | Semantic role of the entity performing an action |
| Antonymy | Opposite meaning relationship between words |
| Axiomatic semantics | Program semantics via logical assertions (Hoare logic) |
| Compositionality | Meaning of a whole from meanings of its parts |
| Denotational semantics | Programs mapped to mathematical objects |
| Fixpoint | Value x where f(x) = x; used in loop semantics |
| Frame | Structured set of semantic roles for a situation |
| Hoare logic | Formal system for program correctness |
| Homonymy | Different words sharing the same form |
| Hyponymy | Subtype relationship between words |
| Lexical semantics | Study of word meanings |
| Meronymy | Part-whole relationship |
| Operational semantics | Program meaning as execution steps |
| Polysemy | One word with multiple related meanings |
| Preservation | Well-typed terms remain well-typed after evaluation |
| Progress | Well-typed terms are not stuck |
| SemVer | Version numbers encoding change meaning |
| Type soundness | Well-typed programs don't produce type errors |
| Ubiquitous language | DDD shared vocabulary between developers and domain experts |
| WordNet | Lexical database of English semantic relations |

## Quick References

- [Jurafsky & Martin: Speech and Language Processing (Semantics chapters)](https://web.stanford.edu/~jurafsky/slp3/)
- [Winskel: The Formal Semantics of Programming Languages](https://mitpress.mit.edu/books/formal-semantics-programming-languages)
- [Pierce: Types and Programming Languages](https://www.cis.upenn.edu/~bcpierce/tapl/)
- [Evans: Domain-Driven Design](https://www.domainlanguage.com/ddd/)
- [FrameNet](https://framenet.icsi.berkeley.edu/)
- [WordNet](https://wordnet.princeton.edu/)
- [Hoare: An Axiomatic Basis for Computer Programming (1969)](https://dl.acm.org/doi/10.1145/363235.363259)
- [SemVer Specification](https://semver.org/)

## Next Steps

- [Pragmatics](pragmatics.md) — context, implicature, and intent in language and API design
