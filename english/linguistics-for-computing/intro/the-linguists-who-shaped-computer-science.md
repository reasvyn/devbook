# The Linguists Who Shaped Computer Science

## Description

The fields of linguistics and computer science have been intertwined since the mid-20th century. Linguists studying the structure of human language developed formalisms that became the foundation for programming languages, compilers, natural language processing, and artificial intelligence. This document profiles the key linguists whose theoretical work directly influenced computing — from Chomsky's grammars to Lakoff's cognitive semantics.

## Prerequisites

- [What Is Computational Linguistics?](what-is-computational-linguistics.md)

## Table of Contents

- [The Linguistic Turn in Computing](#the-linguistic-turn-in-computing)
- [Noam Chomsky](#noam-chomsky)
- [Ferdinand de Saussure](#ferdinand-de-saussure)
- [Richard Montague](#richard-montague)
- [John Searle](#john-searle)
- [George Lakoff](#george-lakoff)
- [Deborah Tannen](#deborah-tannen)
- [Other Key Figures](#other-key-figures)
- [Dialogue Between Linguistics and CS](#dialogue-between-linguistics-and-cs)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### The Linguistic Turn in Computing

In the 1950s, two parallel projects emerged: computer scientists building the first programming languages and linguists building formal models of natural language. They discovered shared problems:

**Shared concerns:** Both fields deal with syntax (how elements combine), semantics (what structures mean), hierarchy (nesting), recursion (infinite generation from finite means), and context (environment affects interpretation). Programming languages borrowed terminology — syntax, semantics, grammar, parsing — directly from linguistics.

**The timeline of influence:** Saussure (1916) → structural analysis. Chomsky (1956–57) → formal grammars, compiler design. Montague (1960s) → formal semantics, lambda calculus. Searle (1969) → speech acts, chatbots. Lakoff (1980s) → cognitive semantics, AI frames. Tannen (1980s) → discourse analysis, dialog systems.

### Noam Chomsky: The Architect of Formal Grammars

Noam Chomsky's work in the 1950s and 1960s transformed both linguistics and computer science. His most influential contribution for computing is the Chomsky hierarchy of formal grammars.

**The Chomsky hierarchy:** Type 3 (regular → finite automaton → regex, lexing), Type 2 (context-free → pushdown automaton → BNF, parsing), Type 1 (context-sensitive → linear-bounded automaton → type checking), Type 0 (recursively enumerable → Turing machine → any computation). Every compiler phase maps to a level of this hierarchy.

**Chomsky's specific contributions:** Generative grammar (finite rules generating infinite sentences — the foundation of formal language theory). Transformational grammar (deep structure vs. surface structure — mirrors AST vs. concrete syntax). Universal Grammar (common structural basis across languages — parallels the search for universal intermediate representations). Recursion (fundamental property of both human and programming languages).

**Criticism and nuance:** Chomsky's influence on NLP has been contested. The statistical revolution of the 1990s explicitly rejected rule-based approaches. Neural models achieve high performance without explicit grammar rules. However, the hierarchy remains fundamental to understanding what different computational models can and cannot do.

### Ferdinand de Saussure: Structuralism and the Sign

Ferdinand de Saussure (1857–1913) is the father of modern linguistics. His work was published posthumously as the "Course in General Linguistics" (1916), compiled from student lecture notes.

**Key concepts:**

**1. The linguistic sign.**

Saussure defined a sign as the union of:
- **Signifier (signifiant):** The sound pattern or written form of a word.
- **Signified (signifié):** The concept or mental representation.

The relationship between signifier and signified is arbitrary. There is no inherent reason why "tree" refers to the concept of a tree. This arbitrariness is a foundational principle.

**Computing connection:**

In programming, the relationship between identifier names and their meanings is similarly arbitrary. A variable named `x` could hold anything. The mapping between names and values is established by convention and context — by the programming environment, not by any intrinsic connection.

```
Signifier: "x"  (the identifier in source code)
Signified: the memory location, the value, the type
Arbitrary: any name could refer to any value
```

**2. Langue and parole.**

- **Langue:** The system of language (the rules, the grammar, the conventions).
- **Parole:** Individual speech acts (actual utterances by actual speakers).

This distinction maps directly to computing:

```
Computer science equivalent:
  Langue  = Programming language specification (syntax, semantics, type system)
  Parole  = Actual programs written in that language
```

**3. Syntagmatic and paradigmatic relations.**

- **Syntagmatic:** Horizontal relations between elements in a sequence (word order in a sentence; tokens in a program).
- **Paradigmatic:** Vertical relations between elements that could substitute for each other (nouns that could fill the same slot; types that could be assigned to a variable).

```
Sentence: "The [cat/dog/mouse] chased the [rat/squirrel]"
  Syntagmatic: the sequence: Det N V Det N
  Paradigmatic: cat, dog, mouse are in paradigmatic relation (same slot)
```

**Computing connection:**

```
int x = 42;
// Syntagmatic: the sequence of tokens: type identifier = value ;
// Paradigmatic: the set of types (int, float, double, char) that could fill the type slot
```

**Saussure's influence on computing:**

- **Structural analysis.** The idea that elements are defined by their relationships to each other (not by intrinsic properties) underlies relational databases, network models, and object-oriented inheritance.
- **Binary oppositions.** Saussure emphasized that meaning arises from differences (hot vs. cold, on vs. off). This binary thinking maps directly to Boolean logic and binary computing.
- **Synchrony vs. diachrony.** Studying language at a point in time (synchrony) vs. studying its evolution (diachrony). This mirrors the distinction between static analysis (analyzing a program at a point in time) and version control (tracking changes over time).

### Richard Montague: Formal Semantics Meets Computation

Richard Montague (1930–1971) was a mathematician, logician, and philosopher who showed that natural language semantics could be formalized using the same tools as mathematics and computer science.

**The Montague grammar program:**

Montague's central thesis: "There is no important theoretical difference between natural languages and the artificial languages of logicians."

This was a radical claim. Before Montague, natural language was considered too messy for formal treatment — too ambiguous, too context-dependent, too irregular.

**Montague's method:**

1. Define a formal syntax for a fragment of natural language.
2. Define a formal semantics (usually in terms of lambda calculus and type theory).
3. Provide a compositional mapping from syntax to semantics (every syntactic rule has a corresponding semantic rule).

**The principle of compositionality (Frege's principle):**

> The meaning of a complex expression is determined by the meanings of its parts and the rules used to combine them.

This principle is fundamental to both Montague grammar and programming language semantics:

```
// Programming language equivalent:
// The meaning of "x + y" is determined by:
//   - The meaning of "x" (its value)
//   - The meaning of "y" (its value)
//   - The rule for "+" (addition in the type domain)
```

**Montague's lambda calculus:**

Montague used the lambda calculus — the same formalism that Alonzo Church developed for computability and that forms the core of functional programming languages — to represent meanings:

```
// Natural language
"Every dog barks."

// Montague-style meaning representation
λP.∀x(dog(x) → bark(x))
```

**Computing connections:**

| Montague concept | CS application |
|---|---|
| Compositional semantics | Denotational semantics of programming languages |
| Lambda calculus representation | Functional programming (Haskell, ML, Scheme) |
| Type-theoretic semantics | Type systems, proof assistants (Coq, Agda) |
| Formal pragmatics | Dialog systems, context-aware NLP |
| Intensional logic | Modal logic, temporal logic, verification |

**Legacy:**

Montague grammar influenced the design of functional programming languages, type-theoretic formalisms, and semantic parsing in NLP. Modern approaches to semantic parsing — converting natural language to executable code — build directly on Montague's insight that natural language can be mapped onto formal meaning representations.

### John Searle: Speech Acts and the Chinese Room

John Searle (born 1932) is a philosopher of language whose work on speech acts and his Chinese Room argument have profound implications for computing and AI.

**Speech act theory:**

Building on J.L. Austin's work, Searle categorized the things we do with language:

| Speech act type | Example | Illocutionary point |
|---|---|---|
| Assertive | "The server is down." | Commit to the truth of a proposition |
| Directive | "Restart the server." | Get the hearer to do something |
| Commissive | "I will fix it by 5 PM." | Commit to a future action |
| Expressive | "Sorry about the outage." | Express psychological state |
| Declarative | "You are now on call." | Change reality through utterance |

**Computing connection:**

Speech act theory underlies:
- **Chatbot and dialog system design.** Understanding what a user is doing with their utterance (requesting, complaining, asserting) is essential for appropriate response generation.
- **Command languages and CLIs.** "Restart the server" is a directive speech act. The CLI interprets it as an imperative command.
- **Protocol design.** Network protocol messages are speech acts: SYN (assertive: "I want to connect"), ACK (assertive: "I acknowledge"), FIN (declarative: "I am done sending").
- **Collaborative editing.** Comments on code review are speech acts: "This approach is O(n^2)" (assertive), "Please use a hash map instead" (directive).

**The Chinese Room argument:**

Searle's most famous thought experiment (1980) challenges the claim that a computer running a program can understand language:

**The setup:**
1. Searle is alone in a room with a rulebook written in English.
2. People outside slide Chinese characters under the door.
3. Searle uses the rulebook to match input characters to output characters.
4. To outsiders, it appears Searle understands Chinese.
5. Searle does not understand Chinese — he is just following rules.

**The conclusion:** Syntax (symbol manipulation) is not sufficient for semantics (understanding). A computer program, like Searle in the room, manipulates symbols according to rules without understanding them.

**Implications:**

- **Strong AI vs. Weak AI.** The Chinese Room challenges "strong AI" — the claim that a sufficiently sophisticated program would actually *understand* language. It does not challenge "weak AI" — the claim that programs can behave as if they understand.
- **The symbol grounding problem.** How do symbols connect to the real world? This is an active research area: how do we build AI systems that connect language to perception and action?
- **LLM debates.** The Chinese Room is regularly invoked in debates about whether large language models "understand" language or are just sophisticated pattern matchers.

**Counterarguments:**
- The Systems Reply: Searle is part of a larger system that *does* understand Chinese (the room + rulebook + Searle = a Chinese-understanding system).
- The Robot Reply: Give the room sensory input and robotic arms, and the symbols become grounded in perception and action.
- The Brain Simulator Reply: If the rulebook simulates the exact brain state of a Chinese speaker, then the system understands Chinese.

The Chinese Room remains one of the most debated thought experiments in AI, precisely because it forces a clear distinction between behaving intelligently and genuinely understanding.

### George Lakoff: Cognitive Linguistics and Metaphor

George Lakoff (born 1941) revolutionized linguistics by arguing that language is fundamentally tied to human cognition, embodiment, and metaphor.

**Key ideas:**

**1. Cognitive linguistics.**

Language is not a formal system independent of the mind. It is shaped by how humans think, perceive, and experience the world. This contrasts with Chomsky's view of language as an autonomous cognitive module.

**2. Conceptual metaphor theory (with Mark Johnson, 1980).**

Metaphor is not just decorative language. It is a fundamental cognitive mechanism: we understand abstract concepts through concrete, embodied experience.

**Common conceptual metaphors:**

| Metaphor | Example in language | Grounding |
|---|---|---|
| ARGUMENT IS WAR | "Your claims are indefensible." | Physical conflict |
| TIME IS MONEY | "You're wasting my time." | Valuable resource |
| KNOWING IS SEEING | "I see what you mean." | Visual perception |
| STATES ARE LOCATIONS | "I'm in a difficult situation." | Spatial experience |
| CAUSES ARE FORCES | "The pandemic pushed us to remote work." | Physical force |

**Computing connection:**

User interface design is saturated with conceptual metaphors:
- **DESKTOP** (files, folders, trash can)
- **WINDOWS** (looking into a workspace)
- **CLOUD** (distant, intangible storage)
- **STREAM** (water flowing, continuous data)
- **MEMORY** (human memory metaphor for computer storage)
- **VIRUS** (biological infection for malicious software)

Understanding metaphor explains why some interfaces are intuitive (they leverage existing embodied metaphors) and why some are confusing (they use metaphors that do not match the user's mental model).

**3. Frames and framing.**

Lakoff showed that language activates cognitive frames — structured mental models that shape how we interpret information.

```
Frame: "WAR"
Elements: enemies, allies, battles, weapons, strategy, victory, defeat
Language: "The company launched a marketing campaign against its competitors."

Frame: "SPORTS"
Elements: teams, players, competition, rules, score, win, lose
Language: "Our product is winning in the marketplace."
```

**Computing connection:**

Frame semantics is directly implemented in:
- **FrameNet.** A lexical database of frames and their semantic roles, used in NLP for semantic role labeling.
- **Object-oriented programming.** Frames map neatly to classes (frame = class, slots = attributes, role fillers = instances).
- **Knowledge representation.** AI knowledge bases (Cyc, ConceptNet) use frame-like structures to represent world knowledge.

**Lakoff's influence on computing:**

- **Natural language understanding.** Frame semantics provides structured meaning representations that NLP systems use to extract who did what to whom.
- **Information retrieval.** Understanding the frame a query activates helps search engines return relevant results.
- **Explainable AI.** Lakoff's work explains why users form mental models of AI systems — and why those models are often metaphor-based and sometimes misleading.

### Deborah Tannen: Discourse Analysis and Conversation

Deborah Tannen (born 1945) is a linguist who studies how language works in real conversations, with a focus on discourse analysis, conversational style, and cross-cultural communication.

**Key concepts:**

**1. Conversational style.**

Tannen's research (especially "You Just Don't Understand," 1990) shows that conversational habits differ systematically across cultures, genders, and communities. What counts as "polite interruption" in one group is "rude interruption" in another.

**Computing connection:**

Conversational style differences directly affect:
- **Chatbot design.** What conversational norms should a chatbot follow? Direct and efficient? Warm and personal? The answer depends on the user community.
- **Communication tools.** Slack channels vs. email vs. meetings — different tools favor different conversational styles. Asynchronous tools favor one style; real-time chat favors another.
- **Code review.** Code review comments are conversational acts. Tannen's work explains why the same comment can be seen as helpful by one developer and hostile by another — differences in conversational style.

**2. Discourse analysis.**

Tannen studies how language organizes itself beyond the sentence level: how conversations begin and end, how topics shift, how speakers signal turn-taking, how rapport is built.

**Key discourse concepts:**

| Concept | Definition | Computing connection |
|---|---|---|
| Cohesion | Grammatical and lexical connections between sentences | Text generation, coherence modeling |
| Coherence | Logical and thematic unity | Document structure, narrative generation |
| Turn-taking | How speakers manage conversational transitions | Dialog system design, chatbot pauses |
| Framing | How participants define what is happening | Context-aware systems, personalization |
| Involvement | How speakers signal engagement | Sentiment analysis, engagement metrics |

**3. Cross-cultural communication.**

Tannen documented systematic differences in communication style across cultural groups:

| Dimension | Direct style | Indirect style |
|---|---|---|
| Asking | "Please do X." | "It might be good to do X." |
| Disagreeing | "I disagree because..." | "That's an interesting perspective..." |
| Giving feedback | "This is wrong." | "Have you considered...?" |
| Interrupting | Shows engagement | Shows disrespect |

**Computing connection:**

- **Global team communication.** Engineering teams span cultures. Tannen's work explains why a direct German engineer and an indirect Japanese engineer may misunderstand each other in Slack.
- **Localization.** Software interfaces must adapt to cultural communication norms. A "Confirm delete?" dialog that is appropriately direct in one culture may feel aggressive in another.
- **Sentiment analysis.** Directness vs. indirectness is confounded with sentiment. A polite but negative review ("This could be improved") is easily misclassified as neutral.

### Other Key Figures

**J.L. Austin (1911–1960):** Founder of speech act theory. His constative/performative distinction is the foundation for understanding commands, declarations, and promises in programming and protocol design.

**Ludwig Wittgenstein (1889–1951):** Argued meaning is use — determined by language games, not reference. This underpins distributional semantics and context-aware computing.

**Roman Jakobson (1896–1982):** His communication functions model includes the phatic function (channel checking: "ACK") and metalingual function (communication about code: type annotations, grammars) — central to network protocols and programming.

### Dialogue Between Linguistics and CS

The relationship between linguistics and computer science has evolved through three phases:

**Phase 1: Formal influence (1950s–1970s).** Linguistics provided formalisms (grammars, hierarchies, structural analysis) for programming language design and compiler construction.

**Phase 2: Statistical rebellion (1980s–2010s).** NLP rejected rule-based linguistics for statistical methods. Hand-crafted grammars were replaced by models trained on data.

**Phase 3: Neural synthesis (2010s–present).** Neural models achieve remarkable performance without explicit rules, but linguistics re-emerges as a tool for understanding what models learn — probing studies, compositional semantics, discourse-aware modeling, metaphor detection.

**The current state:** Linguistics provides test cases for evaluating language models, explanations for failures, annotation schemes for training data, and theoretical frameworks. Computer science provides large-scale data for testing theories, computational models revealing formal properties, and applications that benefit from linguistic insight.

## Glossary

| Term | Definition |
|---|---|
| Chomsky hierarchy | Classification of formal grammars by generative power (regular, context-free, context-sensitive, recursively enumerable) |
| Compositionality | Principle that meaning of a complex expression is built from meanings of its parts |
| Conceptual metaphor | Understanding one conceptual domain in terms of another (ARGUMENT IS WAR) |
| Context-free grammar | Grammar where rules apply regardless of context (most programming languages) |
| Discourse analysis | Study of language beyond the sentence level |
| Frame semantics | Theory that word meaning is understood relative to a structured background of experience |
| Generative grammar | Finite set of rules generating an infinite set of sentences |
| Illocutionary act | What a speaker does in uttering a sentence (request, promise, assert) |
| Langue / parole | Saussure's distinction: language system vs. individual speech acts |
| Montague grammar | Formal approach treating natural language with the same tools as logical languages |
| Performative utterance | An utterance that performs an action through its saying |
| Semantics | Study of meaning in language |
| Sign (signifier / signified) | Saussure's unit of language: form + concept |
| Speech act | An utterance considered as an action (request, assertion, promise) |
| Structuralism | Approach analyzing elements by their relationships within a system |
| Syntax | Study of sentence structure in language |
| Chinese Room | Searle's thought experiment challenging whether programs can achieve understanding |
| Dependency grammar | Grammar model where words depend on each other through directed relations |
| Lambda calculus | Formal system for function abstraction and application |
| Parser generator | Tool that generates a parser from a formal grammar description |

### Other Key Figures (Continued)

**Alonzo Church (1903–1995).** Though primarily a mathematician and logician, Church's lambda calculus is the bridge between formal linguistics and computing. Developed in the 1930s, the lambda calculus is a formal system for expressing computation through function abstraction and application. It became the theoretical foundation for functional programming languages (Scheme, ML, Haskell) and for Richard Montague's approach to natural language semantics. Church's thesis — that any effectively computable function can be expressed in the lambda calculus — established the theoretical limits of computation alongside Turing's work.

**Evelyn Berezin (1925–2018).** While not a linguist by training, Berezin designed the first computer grammar for translating natural language. In 1958, at the University of Cambridge, she developed a system that compiled grammatical rules into a running parser — a direct application of Chomsky's context-free grammars to computing. Her work predated modern parser generators like YACC by over a decade and demonstrated that formal grammars could be executed on a computer to analyze sentence structure.

**Lucien Tesnière (1893–1954).** Developed dependency grammar (published posthumously in 1959), which models sentence structure as a tree of directed dependencies between individual words rather than as nested constituency phrases. This approach proved more suitable for languages with free word order and became the foundation for modern dependency parsing in NLP. Most current syntactic analysis tools, including spaCy and Stanford CoreNLP, use dependency formalisms derived from Tesnière's work.

**Yehoshua Bar-Hillel (1915–1975).** A pioneer of computational linguistics and machine translation. Bar-Hillel identified the "translation ambiguity problem" in the 1950s: sentences like "The box is in the pen" cannot be resolved without real-world knowledge. His work demonstrated the limits of purely syntactic approaches and anticipated the need for semantic and pragmatic knowledge in language understanding — a challenge that remains central to NLP today.

### Examples of Linguistic Concepts in Code

**Context-free grammar in a parser definition:**

```python
import re
from typing import Any

# A simple expression grammar
# E → T | E + T | E - T
# T → F | T * F | T / F
# F → NUMBER | ( E )

class Parser:
    def __init__(self, tokens):
        self.tokens = tokens
        self.pos = 0

    def parse(self):
        return self.parse_E()

    def parse_E(self):
        left = self.parse_T()
        while self.pos < len(self.tokens) and self.peek() in ('+', '-'):
            op = self.consume()
            right = self.parse_T()
            left = ('binop', op, left, right)
        return left

    def parse_T(self):
        left = self.parse_F()
        while self.pos < len(self.tokens) and self.peek() in ('*', '/'):
            op = self.consume()
            right = self.parse_F()
            left = ('binop', op, left, right)
        return left

    def parse_F(self):
        if self.peek().isdigit():
            return ('num', int(self.consume()))
        if self.peek() == '(':
            self.consume()
            expr = self.parse_E()
            self.consume()  # ')'
            return expr
        raise SyntaxError("Unexpected token")

    def peek(self): return self.tokens[self.pos]
    def consume(self):
        tok = self.tokens[self.pos]
        self.pos += 1
        return tok
```

This parser implements a context-free grammar (Level 2 of the Chomsky hierarchy) — the same formalism used for most programming language parsers. The recursive structure of `parse_E` calling `parse_T` calling `parse_F` mirrors the grammar rules. The same pattern appears in YACC, ANTLR, and recursive descent parsers across every programming language compiler.

**Speech act classification in a chatbot:**

```python
def classify_speech_act(utterance: str) -> str:
    """Classify an utterance by its illocutionary force."""
    imperative_patterns = [r"^(please )?(restart|start|stop|run|deploy)"]
    question_patterns = [r"^(what|why|how|when|where|who|can|could|would)"]
    assertive_patterns = [r"^(the|it|there|i think|i see)"]
    expressive_patterns = [r"^(sorry|thanks|great|oops|nice)"]

    import re
    for pattern in imperative_patterns:
        if re.search(pattern, utterance.lower()):
            return "directive"
    for pattern in question_patterns:
        if re.search(pattern, utterance.lower()):
            return "question"
    for pattern in assertive_patterns:
        if re.search(pattern, utterance.lower()):
            return "assertive"
    for pattern in expressive_patterns:
        if re.search(pattern, utterance.lower()):
            return "expressive"
    return "assertive"  # default
```

This pattern classifier applies Searle's speech act taxonomy to real-time chat interactions. A chatbot that recognizes whether a user is commanding, asking, or stating can respond more appropriately — acknowledging a directive, answering a question, or validating an assertion.

## Quick References

- [Chomsky: Syntactic Structures (1957)](https://en.wikipedia.org/wiki/Syntactic_Structures)
- [Saussure: Course in General Linguistics (1916)](https://en.wikipedia.org/wiki/Course_in_General_Linguistics)
- [Montague: The Proper Treatment of Quantification (1973)](https://en.wikipedia.org/wiki/Montague_grammar)
- [Searle: Minds, Brains, and Programs (1980)](https://en.wikipedia.org/wiki/Chinese_room)
- [Lakoff & Johnson: Metaphors We Live By (1980)](https://en.wikipedia.org/wiki/Metaphors_We_Live_By)
- [Tannen: Conversational Style (1984)](https://en.wikipedia.org/wiki/Deborah_Tannen)
- [FrameNet](https://framenet.icsi.berkeley.edu/) — lexical database implementing frame semantics

## Next Steps

- [Formal Languages & Grammars](../formal-languages.md) — the Chomsky hierarchy in depth
- [Semantics: From Words to Meaning](../semantics.md) — meaning representation in NLP
- [Pragmatics & Discourse Analysis](../pragmatics.md) — language in context
