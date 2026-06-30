# Levels of Linguistic Analysis: A Roadmap

## Description

Language can be studied at multiple interacting levels — from individual speech sounds to whole texts. This roadmap maps the hierarchy of linguistic analysis (phonetics, phonology, morphology, syntax, semantics, pragmatics, discourse) and shows how each level maps to concrete computing problems: speech recognition, tokenization, parsing, search, chatbots, documentation, and code analysis.

## Prerequisites

- [Formal Languages & Grammars](formal-languages.md) — alphabets, strings, the Chomsky hierarchy, and the formal foundations of language description

## Table of Contents

- [The Hierarchy of Linguistic Analysis](#the-hierarchy-of-linguistic-analysis)
- [Phonetics & Phonology](#phonetics--phonology)
- [Morphology](#morphology)
- [Syntax](#syntax)
- [Semantics](#semantics)
- [Pragmatics](#pragmatics)
- [Discourse](#discourse)
- [Interactions between Levels](#interactions-between-levels)
- [Levels of Analysis in Programming Languages](#levels-of-analysis-in-programming-languages)
- [Relevance to Computing Subfields](#relevance-to-computing-subfields)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### The Hierarchy of Linguistic Analysis

Linguistics analyzes language at distinct but interdependent levels. Each level abstracts over the one below it, adding structure and meaning.

```
Discourse     — multi-sentence structure, cohesion, coherence
Pragmatics    — speaker intent, context, implicature
Semantics     — literal meaning, truth conditions, reference
Syntax        — sentence structure, constituency, dependency
Morphology    — word structure, prefixes, suffixes, roots
Phonology     — abstract sound systems, phonemes
Phonetics     — physical production and perception of speech sounds
```

**Two-way traffic:** Higher levels constrain lower levels (syntax determines which morphological forms are allowed), and lower levels feed into higher levels (phonological form determines how a word is pronounced in context).

| Computing field | Primary levels |
|-----------------|----------------|
| Speech recognition | Phonetics, phonology, language model |
| Tokenization | Morphology |
| Parsing | Syntax |
| Search | Morphology, semantics |
| Chatbots | Syntax, semantics, pragmatics, discourse |
| Machine translation | All levels |
| Code analysis | Syntax, semantics |
| Documentation | Syntax, pragmatics, discourse |

### Phonetics & Phonology

**Phonetics** studies the physical reality of speech: how sounds are produced (articulatory), transmitted (acoustic), and perceived (auditory). **Phonology** studies how sounds function as abstract units within a given language — the system of **phonemes** and the rules that govern their distribution.

#### Phonetics

The International Phonetic Alphabet (IPA) provides symbols for every distinguishable speech sound:

```
IPA for English consonants:
p    as in "pat"     [pʰæt]   b    as in "bat"     [bæt]
t    as in "tap"     [tʰæp]   d    as in "dap"     [dæp]
k    as in "cat"     [kʰæt]   g    as in "gap"     [gæp]
f    as in "fat"     [fæt]    v    as in "vat"     [væt]
s    as in "sat"     [sæt]    z    as in "zap"     [zæp]
```

Three dimensions describe consonants: **place** of articulation (bilabial, alveolar, velar, glottal), **manner** of articulation (stop, fricative, affricate, nasal, approximant), and **voicing** (voiced, voiceless). Vowels are described by tongue height, backness, and lip rounding.

**Computing relevance:** Speech recognition must map acoustic signals to phonemes; text-to-speech must map text to phonemes and then to acoustic parameters; pronunciation dictionaries map words to phoneme sequences.

```python
pronunciations = {
    "cat": ["k", "æ", "t"],
    "bat": ["b", "æ", "t"],
}

# Levenshtein distance at phoneme level for pronunciation similarity
def phoneme_edit_distance(p1, p2):
    m, n = len(p1), len(p2)
    dp = [[0] * (n + 1) for _ in range(m + 1)]
    for i in range(m + 1):
        dp[i][0] = i
    for j in range(n + 1):
        dp[0][j] = j
    for i in range(1, m + 1):
        for j in range(1, n + 1):
            cost = 0 if p1[i-1] == p2[j-1] else 1
            dp[i][j] = min(dp[i-1][j] + 1, dp[i][j-1] + 1, dp[i-1][j-1] + cost)
    return dp[m][n]

print(phoneme_edit_distance(["k", "æ", "t"], ["b", "æ", "t"]))  # 1
```

#### Phonology

Phonology asks which sound differences distinguish meaning. In English, [p] and [b] are distinct phonemes — "pat" and "bat" differ. The aspirated [pʰ] in "pat" versus unaspirated [p] in "spat" are **allophones** of the same phoneme.

| Concept | Definition | Example |
|---------|------------|---------|
| Phoneme | Smallest sound unit distinguishing meaning | /p/ vs /b/ in "pat"/"bat" |
| Allophone | Predictable variant of a phoneme | [pʰ] in "pat" vs [p] in "spat" |
| Minimal pair | Two words differing by one phoneme | "cat" / "bat" |
| Phonotactics | Rules for permissible sound sequences | English allows "splash" but not "pslah" |

**Computing relevance:** Acoustic models in speech recognition map audio to phoneme probabilities; grapheme-to-phoneme conversion handles irregular English spellings ("enough" -> [ɪˈnʌf]); prosody prediction requires phonological phrase boundaries.

```
WFST for English plural allomorphy:
cat + PL  -> /s/   (voiceless: cats)
dog + PL  -> /z/   (voiced: dogs)
kiss + PL -> /ɪz/  (sibilant: kisses)
```

### Morphology

Morphology studies the internal structure of words. A **morpheme** is the smallest unit that carries meaning or grammatical function. **Free morphemes** stand alone ("cat", "run"); **bound morphemes** must attach ("un-", "-ed"). **Affixes** include prefixes ("un-do"), suffixes ("walk-ing"), infixes ("fan-bloody-tastic"), and circumfixes (German "ge-sag-t").

**Inflectional vs derivational morphology:**

```
Inflectional paradigm of "walk":
  walk (present 1sg/2sg/pl), walks (3sg), walked (past), walking (participle)

Derivational family of "create":
  create (verb) -> creation (noun), creative (adj), creatively (adv), creator (noun)
```

| Property | Inflectional | Derivational |
|----------|--------------|--------------|
| Changes core meaning | No | Yes |
| Changes POS | Rarely | Often |
| Productive | Fully | Semi-productive |

**Morphological typology:**

| Type | Description | Example |
|------|-------------|---------|
| Isolating | ~1 morpheme per word | Mandarin Chinese |
| Agglutinative | Many morphemes, clear boundaries | Turkish, Finnish |
| Fusional | Morphemes fuse multiple meanings | Russian, Arabic |
| Polysynthetic | One word = entire sentence | Inuktitut |

**Computing relevance:** Subword tokenization (BPE, WordPiece, SentencePiece) learns morphological patterns. Stemming normalizes forms for search. Lemmatization returns dictionary forms. Spell checkers use morphological generators.

```python
def pluralize(noun):
    if noun.endswith(("s", "x", "z", "ch", "sh")):
        return noun + "es"
    elif noun.endswith("y") and noun[-2] not in "aeiou":
        return noun[:-1] + "ies"
    else:
        return noun + "s"

for w in ["cat", "box", "baby", "church"]:
    print(f"{w:10} -> {pluralize(w)}")
```

### Syntax

Syntax studies how words combine into phrases and sentences. **Constituency** groups words into nested phrases. **Dependency** captures binary head-dependent relations.

```
Constituency tree for "The cat chased the mouse":
S
├── NP: Det("The") N("cat")
└── VP: V("chased") NP(Det("the") N("mouse"))

Dependency tree:
        chased
       /   |   \
    cat         mouse
    /           /
  The         the
```

**Phrase structure rules:**

```
S      -> NP VP
NP     -> Det N | N | NP PP
VP     -> V NP | V | VP PP
PP     -> P NP
```

**Ambiguity:** The same string can have multiple syntactic structures. "I saw the man with a telescope" has two parses — the telescope is either the instrument of seeing or an attribute of the man.

**Computing relevance:** Parsing algorithms (CKY, Earley, transition-based dependency parsing) assign syntactic structure. Grammar checkers detect agreement errors and sentence fragments. Code formatters traverse ASTs for consistent output. Machine translation uses syntactic transfer between source and target languages.

```python
# Simple constituency check
grammar = {
    "S": [["NP", "VP"]],
    "NP": [["Det", "N"], ["N"]],
    "VP": [["V", "NP"], ["V"]],
}

def parse(symbol, tokens, pos=0):
    if pos >= len(tokens):
        return None, pos
    for rule in grammar.get(symbol, []):
        new_pos = pos
        matched = True
        for part in rule:
            if part in grammar:
                subtree, new_pos = parse(part, tokens, new_pos)
                if subtree is None:
                    matched = False
                    break
            else:
                if new_pos < len(tokens) and tokens[new_pos] == part:
                    new_pos += 1
                else:
                    matched = False
                    break
        if matched:
            return (symbol, rule), new_pos
    return None, pos

tokens = ["the", "cat", "chased", "the", "dog"]
result, pos = parse("S", tokens)
print(f"Parseable: {result is not None and pos == len(tokens)}")
```

### Semantics

Semantics studies meaning: what words mean (lexical semantics), how words combine to form phrase and sentence meaning (compositional semantics), and how language relates to the world (reference and truth conditions).

**Lexical semantics:** Words have multiple **senses** — "bank" can mean a financial institution, a river edge, or to tilt an aircraft.

**Semantic relations:**

| Relation | Definition | Example |
|----------|------------|---------|
| Synonymy | Same or similar meaning | "big" / "large" |
| Antonymy | Opposite meaning | "hot" / "cold" |
| Hyponymy | Specific to general | "dog" IS-A "mammal" |
| Meronymy | Part to whole | "wheel" PART-OF "car" |

**Compositional semantics** uses lambda calculus to model how meanings combine:

```
Every cat sleeps:
  every(cat, sleeps)
  = forall x: cat(x) -> sleeps(x)
```

**Semantic roles** describe the relationship of noun phrases to verbs:

| Role | Example |
|------|---------|
| Agent | "The cat" chased the mouse |
| Patient | The cat chased "the mouse" |
| Instrument | "A key" opened the door |
| Location | She slept "in the garden" |

**Computing relevance:** Search engines use query understanding and vector embeddings for semantic ranking. Knowledge graphs represent entities and relations as semantic networks. Compilers perform type checking and scope resolution as semantic analysis. Word embeddings (skip-gram, GloVe) capture distributional semantics - "a word is characterized by the company it keeps."

```python
# Semantic analogy: king - man + woman ≈ queen
import numpy as np

embeddings = {
    "king":   np.array([0.9, 0.7, 0.3]),
    "queen":  np.array([0.8, 0.7, 0.1]),
    "man":    np.array([0.7, 0.2, 0.5]),
    "woman":  np.array([0.6, 0.2, 0.3]),
}

analogy = embeddings["king"] - embeddings["man"] + embeddings["woman"]
for word, vec in embeddings.items():
    sim = np.dot(analogy, vec) / (np.linalg.norm(analogy) * np.linalg.norm(vec))
    print(f"{word:10} {sim:.3f}")
```

### Pragmatics

Pragmatics studies how context influences meaning — what speakers mean beyond the literal content of their words. While semantics deals with "what is said," pragmatics deals with "what is meant."

**Context types:**

| Context type | Definition | Example |
|--------------|------------|---------|
| Physical | Time, place, setting | "Here" means the speaker's location |
| Social | Roles, relationships | Formal vs informal register |
| Linguistic | Surrounding discourse | "It" refers to earlier mention |
| Epistemic | Shared knowledge | "The king" = known monarch |

**Deixis** refers to words requiring context to resolve — "I", "you", "here", "now", "this", "that".

**Implicature:** What a speaker implies beyond literal meaning.

```
A: "Are you going to the party?"
B: "I have a deadline tomorrow."

Implicature: B cannot attend.
Grice's maxims: Quantity (right amount), Quality (truthful), Relation (relevant), Manner (clear).
```

**Speech acts:** Utterances perform actions.

| Type | Definition | Example |
|------|------------|---------|
| Assertive | States a fact | "The server is down." |
| Directive | Requests or commands | "Please restart the server." |
| Commissive | Promises or offers | "I will fix it by tomorrow." |
| Declarative | Changes reality | "You are fired." |

**Computing relevance:** Chatbots recognize user intent (directive: "Book a flight"), resolve deixis ("this one" refers to displayed option), and respect Gricean maxims. API signatures encode pragmatics — optional parameters signal obligatoriness, error messages are speech acts (warnings, directives, failures).

```python
intents = {
    "greeting": [r"\bhi\b", r"\bhello\b"],
    "book":     [r"\bbook\b", r"\breserve\b", r"\bflight\b"],
    "cancel":   [r"\bcancel\b", r"\bundo\b"],
}

def classify_intent(text):
    text = text.lower()
    for intent, patterns in intents.items():
        for p in patterns:
            import re
            if re.search(p, text):
                return intent
    return "unknown"

for u in ["Hi!", "Book a flight to Tokyo", "Cancel my reservation"]:
    print(f"{u:30} -> {classify_intent(u)}")
```

### Discourse

Discourse analysis studies language beyond the sentence level — how sentences combine into coherent texts, conversations, and narratives.

**Cohesion vs coherence:** Cohesion is surface-level connections (pronouns, conjunctions, lexical repetition); coherence is underlying logical connectedness.

**Cohesive devices:**

```
Reference:   "John came in. He sat down." (anaphora)
Ellipsis:    "The CI passed. The staging failed." (ellipsis of "CI")
Conjunction: "First build. Then test. Finally deploy."
Lexical:     "The server crashed. We rebooted the machine." (synonyms)
```

**Discourse coherence relations:**

| Relation | Definition | Example |
|----------|------------|---------|
| Elaboration | Details about prior content | "The system has three tiers. The frontend handles routing." |
| Explanation | Cause or reason | "The query failed because the index was corrupted." |
| Contrast | Highlights difference | "Python is dynamic. Rust is static." |
| Sequence | Temporal ordering | "Parse. Validate. Execute." |

**Rhetorical Structure Theory (RST)** represents discourse as a tree of rhetorical relations connecting text spans. Nuclei are central spans; satellites provide supporting material.

**Computing relevance:** Documentation uses discourse structure (tutorials use sequence, references use elaboration, troubleshooting uses explanation). Summarization selects nucleus spans. Machine translation maintains coherence across sentences. Good commit messages follow discourse structure: title (summary), body (elaboration/explanation), footer (related issues).

```python
discourse_segments = {
    "tutorial":     [r"\bhow to\b", r"\bguide\b", r"\bstep\b", r"\bfirst\b"],
    "reference":    [r"\breturns\b", r"\bparameter\b", r"\btype\b"],
    "explanation":  [r"\bbecause\b", r"\btherefore\b", r"\bsince\b"],
    "troubleshoot": [r"\berror\b", r"\bissue\b", r"\bfix\b"],
}

def classify(text):
    import re
    text = text.lower()
    for seg_type, patterns in discourse_segments.items():
        for p in patterns:
            if re.search(p, text):
                return seg_type
    return "other"
```

### Interactions between Levels

Every utterance simultaneously realizes phenomena at every level, with constraints cascading across the hierarchy.

**Bottom-up:** Phonological form determines morphological alternations (plural /s/ vs /z/ vs /ɪz/ depends on final phoneme). Morphological form determines syntactic category (past-tense verb serves as clause head). Syntactic structure determines semantic interpretation.

**Top-down:** Pragmatic context disambiguates syntactic parses ("I saw the man with a telescope" resolved by real-world knowledge). Discourse context determines morphological form (pronoun agreement with antecedents across sentences).

```
Multi-level analysis of "The cats were being chased by dogs."

Phonetics:        [ðə kæts wɜr biɪŋ tʃeɪst baɪ dɑgz]
Phonology:        Stress on "cats", "chas-", "dogs"
Morphology:       cat+PL, be+PAST+PL, chase+PAST, dog+PL
Syntax:           S -> NP(Det+N+PL) VP(Aux+V+VP) PP(P+N+PL)
Semantics:        PAST(be(chase(dogs, cats))) — passive inversion
Pragmatics:       Discourse-old referents ("the cats", "the dogs")
Discourse:        Assumes prior mention of cats and dogs
```

**Interface phenomena:**

| Interface | Phenomenon | Computing example |
|-----------|------------|-------------------|
| Phonology-Morphology | Allomorphy | TTS selects correct plural allophone |
| Morphology-Syntax | Subject-verb agreement | Grammar checker verifies number agreement |
| Syntax-Semantics | Quantifier scope | "Every cat chased a mouse" has two readings |
| Semantics-Pragmatics | Implicature | Chatbot infers unstated intent |

### Levels of Analysis in Programming Languages

Programming languages undergo multi-level analysis parallel to natural language:

| Level | Natural language | Programming language |
|-------|-----------------|---------------------|
| Lexical | Phonemes, morphemes | Tokens, lexemes |
| Syntax | Phrase structure | AST, parse tree |
| Semantics | Meaning, truth conditions | Type checking, evaluation |
| Pragmatics | Speaker intent | API design, idiomatic usage |
| Discourse | Multi-sentence coherence | Module structure, code organization |

**Compiler pipeline as multi-level analysis:**

```
Source code: "int x = 42;"
  Lexical (morphology): [int] [x] [=] [42] [;]
  Syntax:               Declaration(Type=int, Name=x, Value=42)
  Semantics:            Type check: 42 matches int; Scope: x declared
  Code gen (pragmatics): x -> register R1, 42 -> mov instruction
```

**Idioms as pragmatics:** Programming idioms convey intent beyond literal syntax. Python's EAFP (Easier to Ask Forgiveness than Permission) signals "expect key to exist, handle rare miss" — a pragmatic layer above the literal try/except semantics.

**Module structure as discourse:** Well-organized codebases follow discourse principles — README (introduction), implementation (body/narrative), tests (evaluation).

```
Package discourse structure:
  README.md           (introduction)
  src/main.py         (main narrative)
  src/utils.py        (elaboration)
  tests/              (evaluation)
```

### Relevance to Computing Subfields

| Subfield | Primary levels | Why |
|----------|---------------|-----|
| Speech recognition | Phonetics, phonology, LM | Acoustic features -> phonemes -> words |
| Text-to-speech | Phonology, morphology | Text -> phonemes -> prosody -> audio |
| Search & IR | Morphology, semantics | Stemming, query understanding, semantic ranking |
| Machine translation | All levels | Source analysis -> transfer -> target generation |
| Chatbots / Conversational AI | Semantics, pragmatics, discourse | Intent, dialogue state, response generation |
| Compilers | Syntax, semantics | Parsing, type checking, code generation |
| Code analysis / linters | Syntax, semantics, pragmatics | AST analysis, data flow, idiom detection |
| Documentation | Discourse, pragmatics | Structure, audience, coherence |
| Grammar checkers | Morphology, syntax | Agreement, parse validation |
| Summarization | Semantics, discourse | Content selection, coherence |
| Information extraction | Semantics, pragmatics | Entity recognition, relation extraction |
| Question answering | Syntax, semantics, discourse | Parse question, retrieve answer, maintain coherence |
| Code generation (LLM) | All levels | Token prediction -> syntax -> semantics -> intent |
| API design | Pragmatics, discourse | Affordances, error messages, consistency |

**NLP pipeline (full stack):**

```
Raw text -> Tokenization (morphology) -> POS tagging (morphology/syntax)
  -> Parsing (syntax) -> NER (semantics) -> SRL (semantics)
  -> Coreference resolution (discourse) -> Application
```

**Chatbot pipeline:**

```
User input -> Intent classification (pragmatics) -> Entity extraction (semantics)
  -> Dialog state tracking (discourse) -> Response generation (syntax+pragmatics)
```

## Study Cases

### Case 1: Debugging a Speech Recognition Failure

A voice assistant transcribes "Set a timer for fifteen minutes" as "fifty minutes."

| Level | Issue | Explanation |
|-------|-------|-------------|
| Phonetics | Acoustic similarity | [fɪfˈtin] vs [fɪfˈti] differ only in final nasal |
| Phonology | Reduction | "fifteen" reduces in fast speech, losing final /n/ |
| Lexical | Confusable pair | "fifteen" / "fifty" are near-homophones |

**Solution:** Add prosodic features — "fifteen" has stress on second syllable, "fifty" on first. Use duration and pitch contours to distinguish them.

### Case 2: Search Engine for Turkish

Search cannot find "okullarimizdan" (from our schools) when user queries "okul" (school).

| Level | Issue | Solution |
|-------|-------|----------|
| Morphology | Agglutinative inflection | Deploy stemmer to reduce to root |
| Semantics | Shared meaning | Stemming unifies surface forms |

```python
from snowballstemmer import TurkishStemmer
stemmer = TurkishStemmer()
query = "okul"
for w in ["okullarimizdan", "okulda", "okulun"]:
    if stemmer.stemWord(w) == stemmer.stemWord(query):
        print(f"Match: {w}")
```

### Case 3: Ambiguous Chatbot Instruction

Chatbot receives: "Remind me to call John when I get home."

| Level | Analysis |
|-------|----------|
| Syntax | "call John" (VP) + "when I get home" (adverbial clause) |
| Semantics | "call" = telephone |
| Pragmatics | "remind me" = create notification |
| Discourse | "John" must refer to a known contact |

**Intent representation:**
```python
intent = {
    "action": "create_reminder",
    "trigger": {"type": "geofence", "location": "home"},
    "task": "call John",
}
```

### Case 4: Subject-Verb Agreement Check

Grammar checker flags: "The list of items are on the table."

```
Syntax: NP head is "list" (sg), verb is "are" (pl).
The PP "of items" is a modifier — verb must agree with head, not modifier.
Error: singular head "list" with plural verb "are".
Fix: "The list of items is on the table."
```

## Examples

### Example 1: Multi-Level Analysis of a Single Sentence

Sentence: "Could you please open the door?"

| Level | Analysis |
|-------|----------|
| Phonetics | [kʊd] [ju] [pliz] [oʊpən] [ðə] [dɔr] |
| Morphology | could (modal), you (pronoun), please (adv), open (verb root), the (det), door (noun) |
| Syntax | Interrogative with subject-aux inversion |
| Semantics | Request for door-opening action; "the door" = specific known door |
| Pragmatics | Indirect directive — not asking about ability, but making a polite request |
| Discourse | Assumes door is mutually known; follows politeness norms |

### Example 2: Levels in a Code Review Comment

Comment: "Should we extract this into a helper function?"

| Level | Analysis |
|-------|----------|
| Morphology | extract (verb), helper (adj), function (noun) |
| Syntax | Interrogative: Aux(should) + NP(we) + VP(extract this into a helper function) |
| Semantics | Literal meaning: question about code refactoring |
| Pragmatics | Indirect directive — suggesting a change, not asking about ability |
| Discourse | "This" is anaphoric — refers to the code block under review |

### Example 3: Ambiguity at Multiple Levels

"Time flies like an arrow; fruit flies like a banana."

```
First clause:  Time(N) flies(V) like(P) an(Det) arrow(N)  — time passes
Second clause: Fruit(N) flies(N) like(P) a(Det) banana(N) — a fly likes bananas
```

The humor arises from lexical ambiguity ("flies" = V or N), syntactic ambiguity ("fruit flies" = N+N compound or N+V phrase), and semantic shift under parallel syntactic structure.

### Example 4: Discourse Structure in a README

```
Title:   "DevBook — Markdown Learning Library"    (introduction)
Intro:   "DevBook is a collection..."             (identification)
Purpose: "It covers topics..."                     (elaboration via anaphora)
Usage:   "To get started, browse..."               (sequence)
```

### Example 5: Phonological Rule in Code

English plural allomorphy expressed through phonological features:

```python
def plural_allomorph(word):
    sibilants = {"s", "z", "ʃ", "ʒ", "tʃ", "dʒ"}
    last_phoneme = phonemes(word)[-1]
    if last_phoneme in sibilants:
        return word + "ez"     # /ɪz/
    elif last_phoneme in {"p", "t", "k", "f", "θ"}:
        return word + "s"      # /s/
    else:
        return word + "z"      # /z/
```

## Glossary

| Term | Definition |
|------|------------|
| Adjacency pair | Two-turn conversational sequence (Q-A, greeting-greeting) |
| Affix | Bound morpheme attached to a root or stem |
| Agent | Semantic role of the entity performing an action |
| Agglutinative | Language type with many clearly segmented morphemes per word |
| Allomorph | Variant form of a morpheme conditioned by phonological context |
| Allophone | Predictable phonetic variant of a phoneme |
| Anaphora | Reference to an earlier element in discourse |
| Antonymy | Semantic relation of opposite meaning |
| Circumfix | Affix that surrounds the root |
| Coherence | Conceptual connectedness of a text |
| Cohesion | Surface-level linguistic connections between sentences |
| Compositional semantics | Principle that meaning of whole is built from meanings of parts |
| Constituency | Grouping of words into nested phrases |
| Deixis | Words whose reference depends on context (here, now, I) |
| Dependency grammar | Grammar model based on binary head-dependent relations |
| Derivation | Morphological process creating new words |
| Discourse | Language beyond the sentence level |
| Felicity condition | Requirement for a speech act to succeed |
| Free morpheme | Morpheme that can stand alone as a word |
| Fusional | Language type where affix encodes multiple grammatical meanings |
| Grice's maxims | Pragmatic principles: quantity, quality, relation, manner |
| Hyponymy | Semantic relation of specific to general |
| Implicature | Meaning implied beyond literal content |
| Infix | Affix inserted inside the root |
| Inflection | Grammatical modification without changing word class |
| IPA | International Phonetic Alphabet |
| Isolating | Language type with ~one morpheme per word |
| Lemma | Dictionary form of a word |
| Meronymy | Semantic relation of part to whole |
| Minimal pair | Two words distinguished by a single phoneme |
| Morpheme | Smallest unit carrying meaning or grammatical function |
| Morphology | Study of word structure and formation |
| Phoneme | Smallest sound unit distinguishing meaning |
| Phonetics | Study of physical speech sounds |
| Phonology | Study of abstract sound systems |
| Phonotactics | Permissible sound sequences in a language |
| Phrase structure rule | Rule describing how phrases are built from constituents |
| Polysynthetic | Language type where one word = full sentence |
| Pragmatics | Study of context-dependent meaning |
| Prosody | Rhythm, stress, and intonation of speech |
| RST | Rhetorical Structure Theory — discourse coherence framework |
| Semantic role | Thematic relation of a noun phrase to a verb |
| Semantics | Study of meaning in language |
| Sense | One distinct meaning of a word |
| Speech act | Utterance that performs an action |
| Synonymy | Semantic relation of same or similar meaning |
| Syntax | Study of sentence structure |
| Tokenization | Splitting text into processing units |

## Quick References

- [Jurafsky & Martin: Speech and Language Processing](https://web.stanford.edu/~jurafsky/slp3/) — comprehensive NLP textbook
- [Chomsky: Syntactic Structures (1957)](https://doi.org/10.1515/9783112316009) — foundational generative syntax
- [Grice: Logic and Conversation (1975)](https://doi.org/10.1163/9789004368811_003) — pragmatic maxims and implicature
- [Halliday & Hasan: Cohesion in English (1976)](https://www.routledge.com/Cohesion-in-English/Halliday-Hasan/p/book/9780582550414) — foundational discourse analysis
- [Mann & Thompson: Rhetorical Structure Theory (1988)](https://doi.org/10.1515/text.1.1988.8.3.243) — discourse coherence relations
- [Searle: Speech Acts (1969)](https://www.cambridge.org/core/books/speech-acts/91C45B7211A1BDB6645DAD68B17343C9) — foundational speech act theory
- [Porter: An Algorithm for Suffix Stripping (1980)](https://www.cs.toronto.edu/~frank/csc2501/Readings/R2_Porter/Porter-1980.pdf) — original stemming algorithm
- [Koskenniemi: Two-Level Morphology (1983)](https://www.aclweb.org/anthology/C84-1002/) — finite-state morphology
- [Devlin et al.: BERT (2019)](https://arxiv.org/abs/1810.04805) — contextualized representations
- [Aho, Lam, Sethi, Ullman: Compilers: Principles, Techniques, and Tools](https://www.pearson.com/en-us/subject-catalog/p/compilers-principles-techniques-and-tools/P200000003267) — compiler design
- [Grosz & Sidner: Discourse Structure (1986)](https://doi.org/10.1162/coli.1986.12.3.175) — computational discourse model

## Next Steps

- [Morphology & Tokenization](morphology-tokenization.md) — word structure, subword algorithms, and computational applications
- [Parsing: Natural & Programming Languages](parsing.md) — algorithms for syntactic analysis in both domains
- [Semantics: Meaning in Language & Code](semantics.md) — lexical semantics, compositional semantics, and type systems
