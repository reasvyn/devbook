# What Is Computational Linguistics?

## Description

Computational linguistics (CL) is the scientific study of language using computational methods. It sits at the intersection of linguistics and computer science, applying algorithms and models to understand how language works. For developers, CL explains why NLP tools work the way they do and how programming languages draw from linguistic theory.

## Prerequisites

- [The Linguistics Behind Code & Language](../../intro/linguistics-and-code.md) — foundational connections between linguistics and programming
- [Why English Matters for Developers](../../intro/why-english-matters.md)

## Table of Contents

- [Defining Computational Linguistics](#defining-computational-linguistics)
- [CL vs. NLP vs. AI](#cl-vs-nlp-vs-ai)
- [A Brief History of Computational Linguistics](#a-brief-history-of-computational-linguistics)
- [The Levels of Linguistic Analysis](#the-levels-of-linguistic-analysis)
- [Formal Grammars & Parsing](#formal-grammars--parsing)
- [Statistical & Corpus Linguistics](#statistical--corpus-linguistics)
- [Machine Learning in Computational Linguistics](#machine-learning-in-computational-linguistics)
- [Key CL Applications](#key-cl-applications)
- [CL for Developers: Why It Matters](#cl-for-developers-why-it-matters)
- [The CL Toolbox](#the-cl-toolbox)
- [Evaluating CL Systems](#evaluating-cl-systems)
- [Ethics in Computational Linguistics](#ethics-in-computational-linguistics)
- [Current Research Directions](#current-research-directions)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Defining Computational Linguistics

Computational linguistics asks: How can we model language with computers? What algorithms capture the structure and meaning of human language?

**The two goals of CL:**

| Goal | Question | Example |
|---|---|---|
| **Scientific** | What does computation reveal about human language? | Modeling how children acquire grammar |
| **Engineering** | How can we build useful language technologies? | Building a machine translation system |

**What makes language hard for computers?**

- **Ambiguity at every level:** Phonemes, words, syntax, semantics, pragmatics
- **Discrete, combinatorial structure:** Infinite sentences from finite means
- **Context-dependence:** Meaning depends on world knowledge, not just words
- **Variation:** Dialects, registers, styles, individual differences
- **Sparsity:** Most possible sentences have never been seen before

**The fundamental challenge:**

> "Every time I fire a linguist, the performance of the speech recognizer goes up." — attributed to Fred Jelinek, IBM speech recognition researcher (1988)

This quote illustrates the tension between rule-based and statistical approaches that has defined CL. Today, the field uses both: linguistic structure informs neural architectures; neural methods reveal patterns that challenge linguistic theories.

### CL vs. NLP vs. AI

| Field | Focus | Goal |
|---|---|---|
| **Computational Linguistics** | Understanding language through computation | Scientific understanding of language |
| **Natural Language Processing** | Building systems that process language | Engineering useful applications |
| **AI / Machine Learning** | Intelligent systems that learn | General problem-solving |

**Overlap:**

- CL and NLP heavily overlap in methods (both use machine learning, both work with text)
- CL tends to be more theory-driven (linguistic questions inform the methods)
- NLP tends to be more application-driven (product requirements inform the methods)
- Both increasingly use the same tools (transformers, attention, embeddings)

**The spectrum:**

```
Pure CL                          Applied NLP
│                                    │
Linguistic theory → Corpus methods → Feature engineering → Neural models → Products
```

A researcher studying whether transformers learn hierarchical structure is doing CL. An engineer building a chatbot for customer support is doing NLP. Both might use the same tool (BERT) but ask different questions.

### A Brief History of Computational Linguistics

**1950s–1960s: The symbolic era**

- **1950:** Turing proposes the Imitation Game (Turing Test)
- **1954:** Georgetown-IBM experiment: 60 Russian sentences translated to English using 250 words and 6 grammar rules
- **1956:** Chomsky introduces generative grammar, influencing both linguistics and CS
- **1957:** Chomsky's Syntactic Structures — formal grammars for natural language
- **1960s:** ELIZA (Weizenbaum) — simple pattern matching simulated conversation. SHRDLU (Winograd) — natural language interaction in a blocks world

**1970s–1980s: The era of knowledge**

- **1970s:** Augmented Transition Networks (ATNs) — procedural parsing. Conceptual Dependency (Schank) — meaning representation
- **1980s:** Lexical-Functional Grammar (LFG), Head-Driven Phrase Structure Grammar (HPSG) — unification-based frameworks
- **1980s:** Marcus's Parsing — deterministic parsing with lookahead
- **1980s:** Machine-readable dictionaries (LDOCE, WordNet)
- **1980s:** Hidden Markov Models for speech recognition

**1990s: The statistical revolution**

- **1990:** Chunking and shallow parsing replace full parsing for applications
- **1993:** IBM Model 1-5 for machine translation — word alignment, expectation maximization
- **1996:** Brill tagger — transformation-based learning for part-of-speech tagging
- **1999:** Support Vector Machines for text classification
- **2000s:** Maximum Entropy models, Conditional Random Fields

**2010s: The neural era**

- **2013:** Word2Vec (Mikolov) — distributed word embeddings
- **2014:** Sequence-to-sequence models (Sutskever) — neural machine translation
- **2015:** Attention mechanism (Bahdanau) — focus on relevant parts of input
- **2017:** Transformers (Vaswani) — attention-only architecture
- **2018:** BERT (Devlin) — pre-trained contextual representations
- **2020s:** GPT-3, ChatGPT, LLMs — large-scale language models

**Key insight from this history:**

Every major advance in CL came from a combination of:
- Better linguistic understanding (theory)
- More data (corpora)
- More compute (Moore's law)
- Better algorithms (from symbolic → statistical → neural)

### The Levels of Linguistic Analysis

CL approaches language at multiple, interacting levels:

**Phonetics & Phonology (sound):**

CL applications at this level:
- **Speech recognition:** Converting audio to text (ASR)
- **Text-to-speech:** Converting text to audio (TTS)
- **Pronunciation modeling:** For speech recognition and synthesis
- **Phoneme alignment:** Aligning audio with text transcripts

Key techniques:
- Acoustic models (mapping audio features to phonemes)
- Pronunciation dictionaries (CMU Pronouncing Dictionary)
- Prosody modeling (stress, rhythm, intonation)

**Morphology (word structure):**

CL applications:
- **Stemming:** Reducing words to stem forms ("running" → "run")
- **Lemmatization:** Reducing words to dictionary forms ("better" → "good")
- **Morphological analysis:** Breaking words into morphemes ("unlockable" → "un" + "lock" + "able")
- **Morphological generation:** Producing word forms from a lemma

Key techniques:
- Finite-state transducers (FSTs) for morphological analysis
- Paradigm-based learning for morphologically rich languages
- Subword tokenization (BPE, WordPiece, SentencePiece)

**Syntax (sentence structure):**

CL applications:
- **Parsing:** Producing tree structures for sentences
- **Part-of-speech tagging:** Labeling words with grammatical categories
- **Chunking:** Grouping words into phrases (NP, VP)
- **Constituency parsing:** Producing phrase structure trees
- **Dependency parsing:** Producing dependency relations

Key techniques:
- Context-free grammars (CFGs) + parsing algorithms (CKY, Earley)
- Probabilistic CFGs (PCFGs)
- Transition-based and graph-based dependency parsing
- Neural approaches (biLSTM + CRF, Transformer encoders)

**Semantics (meaning):**

CL applications:
- **Word sense disambiguation:** Choosing the correct meaning of an ambiguous word
- **Semantic role labeling:** Identifying who did what to whom
- **Named entity recognition:** Identifying people, places, organizations
- **Relation extraction:** Identifying relationships between entities
- **Semantic parsing:** Converting text to formal meaning representations

Key techniques:
- Distributional semantics (word2vec, GloVe, BERT embeddings)
- Frame semantics (FrameNet)
- Abstract Meaning Representation (AMR)
- Natural Language Inference (NLI)

**Pragmatics & Discourse (context):**

CL applications:
- **Coreference resolution:** Identifying when two mentions refer to the same entity
- **Discourse parsing:** Identifying discourse relations (contrast, elaboration, explanation)
- **Anaphora resolution:** Resolving pronouns to their antecedents
- **Dialog state tracking:** Tracking context in conversational systems

Key techniques:
- Mention-pair models for coreference
- Rhetorical Structure Theory (RST) for discourse
- Transformer models with long-range attention

### Formal Grammars & Parsing

Formal grammars are the bridge between linguistic theory and computational implementation.

**Context-Free Grammars (CFGs):**

A CFG consists of:
- Non-terminals: syntactic categories (S, NP, VP, N, V, etc.)
- Terminals: words
- Productions: rewrite rules (S → NP VP)

Example:
```
S   → NP VP
NP  → Det N | N
VP  → V NP | V
Det → "the" | "a"
N   → "cat" | "dog" | "mouse"
V   → "chased" | "ate"
```

**Parsing algorithms:**

| Algorithm | Type | Complexity | Characteristics |
|---|---|---|---|
| CKY | Bottom-up | O(n^3) | Requires CNF grammar |
| Earley | Top-down | O(n^3) | Works with any CFG |
| Chart parsing | Dynamic programming | O(n^3) | General framework |
| Shift-reduce | Greedy | O(n) | Fast, approximate |
| Transition-based | Greedy/beam | O(n) | Popular for dependency parsing |

**Probabilistic CFGs:**

PCFGs assign probabilities to productions, allowing the parser to choose the most likely parse:

```
NP → Det N   (0.8)
NP → N       (0.2)
```

**Dependency parsing:**

Representing sentence structure as dependency relations between words:

```
         root
          │
        chased
       /      \
     cat      mouse
      │          │
     The        the
```

Advantages over constituency parsing:
- Closer to semantic relations
- Works well across languages
- Efficient parsing algorithms (transition-based)

**Universal Dependencies:**

A framework for cross-linguistically consistent dependency annotation:
- 17 universal part-of-speech tags
- 37 universal dependency relations
- Annotated treebanks for 100+ languages

### Statistical & Corpus Linguistics

Corpus linguistics studies language through large collections of real texts (corpora). Most modern CL is corpus-based.

**Key corpora:**

| Corpus | Language | Size | Content |
|---|---|---|---|
| Brown Corpus | English | 1M words | 500 texts, 15 genres |
| British National Corpus | English | 100M words | Written + spoken |
| COCA (Corpus of Contemporary American English) | English | 1B words | Balanced across genres |
| Gigaword | English | 4B words | News text |
| Common Crawl | Multilingual | Billions of pages | Web text |

**N-gram models:**

Probabilistic models of word sequences:

```
P(w_n | w_1, w_2, ..., w_{n-1})
```

- Unigram: P(the) = 0.07
- Bigram: P(cat | the) = 0.002
- Trigram: P(chased | the cat) = 0.001

**Smoothing:**

Handling unseen n-grams:
- Add-1 (Laplace) smoothing
- Good-Turing estimation
- Kneser-Ney smoothing
- Interpolation

**Collocations:**

Statistical identification of multi-word expressions:
- Pointwise mutual information (PMI)
- Chi-square test
- t-test

### Machine Learning in Computational Linguistics

CL has adopted every major ML paradigm:

**Supervised learning:**

| Task | Input | Output | Models |
|---|---|---|---|
| POS tagging | Word + context | Tag | CRF, HMM, biLSTM |
| NER | Sentence | Entity spans | CRF, Transformers |
| Parsing | Sentence | Parse tree | Transition-based, GNN |
| Text classification | Document | Class | SVM, CNN, BERT |
| Machine translation | Source text | Target text | Seq2seq, Transformers |

**Unsupervised learning:**

| Task | Input | Output | Models |
|---|---|---|---|
| Word embeddings | Corpus | Word vectors | word2vec, GloVe |
| Topic modeling | Documents | Topic distributions | LDA |
| Clustering | Documents | Clusters | k-means, HDBSCAN |
| Grammar induction | Sentences | Grammar | Expectation maximization |

**Sequence labeling with CRFs:**

Conditional Random Fields (CRFs) are the standard for structured prediction:

```
P(y | x) ∝ exp(∑ w_i * f_i(x, y))
```

Features for POS tagging:
- Current word identity
- Previous/next word
- Current word suffix (e.g., -ing, -ed)
- Current word prefix
- Capitalization
- Previous/next tag

**Neural approaches:**

The neural revolution in CL brought:

- **Word embeddings:** Dense vector representations capturing distributional similarity
- **Contextual embeddings:** ELMo, BERT — representations that depend on context
- **Sequence models:** RNNs, LSTMs, GRUs for modeling sequences
- **Attention:** Focusing computation on relevant parts of input
- **Transformers:** Self-attention for long-range dependencies
- **Pre-training + fine-tuning:** BERT, GPT — large models adapted to specific tasks

### Key CL Applications

**Machine Translation (MT):**

| Era | Approach | Example system |
|---|---|---|
| 1950s-80s | Rule-based | SYSTRAN |
| 1990s-2010s | Statistical (SMT) | Moses, Google Translate (initial) |
| 2010s-present | Neural (NMT) | Google NMT, DeepL, GPT |

NMT uses encoder-decoder architecture with attention:

```
Source: "The cat sat on the mat"
Encoder: [contextual representations]
Attention: [align source and target positions]
Decoder: "Le chat s'est assis sur le tapis"
Target: "Le chat s'est assis sur le tapis"
```

**Question Answering (QA):**

| Type | Example | Approach |
|---|---|---|
| Extractive | "What is the capital of France?" → "Paris" | Span prediction from a passage |
| Abstractive | "Explain reinforcement learning" | Generated answer |
| Open-domain | Questions over a large corpus | Retrieval + reading comprehension |
| Closed-book | Questions answered from model knowledge | LLM knowledge |

**Summarization:**

| Type | Approach | Example |
|---|---|---|
| Extractive | Select important sentences | MEAD |
| Abstractive | Generate novel summary | BART, PEGASUS |

**Information Extraction (IE):**

```
Input: "Apple acquired Beats Electronics for $3B in 2014."
Output:
  [Acquisition]
    Acquirer: Apple
    Target: Beats Electronics
    Amount: $3,000,000,000
    Date: 2014
```

**Sentiment Analysis:**

| Level | Example | Task |
|---|---|---|
| Document | Movie review → positive/negative | Document-level sentiment |
| Sentence | "The camera is great but the battery is terrible." | Aspect-based sentiment |
| Aspect | Battery: negative; Camera: positive | Fine-grained analysis |

**Dialog Systems:**

| Type | Characteristics | Example |
|---|---|---|
| Task-oriented | Goal: complete a task | Customer service bot |
| Social | Goal: engaging interaction | Replika |
| Question-answering | Goal: provide information | Technical support bot |

### CL for Developers: Why It Matters

**Foundations of code intelligence:**

Many developer tools use CL techniques:

| Tool | CL technique |
|---|---|
| Autocomplete | Language models (n-gram → neural) |
| Spell-check | Edit distance, n-gram probabilities |
| Grammar check | POS tagging, dependency parsing |
| Code search | Embeddings, semantic similarity |
| Documentation generation | Summarization, template filling |
| Bug detection | Pattern matching, statistical anomalies |

**Understanding the limits of LLMs:**

CL provides the framework to understand what LLMs can and cannot do:

- They capture distributional patterns but may lack compositional understanding
- They perform well on in-distribution but fail on out-of-distribution examples
- They can generate fluent text but may not understand meaning (the "stochastic parrot" debate)

**Better prompt engineering:**

CL concepts inform prompt design:

- **Constituency:** What structure does the prompt imply?
- **Semantic role:** What role (agent, patient, instrument) should the model take?
- **Pragmatics:** What context is the model assuming?
- **Coreference:** Is the reference clear?

**Better regular expressions:**

CL formalisms (FSAs, FSTs) are the theory behind regular expressions:

```
Regular expression: ^[A-Z][a-z]*\s[A-Z][a-z]*$

Finite state automaton:
States: {q0, q1, q2, q3, q4}
Transitions:
  q0 --[A-Z]--> q1   (first capital letter)
  q1 --[a-z]--> q1   (lowercase letters)
  q1 --[ ]--> q2     (space)
  q2 --[A-Z]--> q3   (second capital letter)
  q3 --[a-z]--> q3   (lowercase letters)
  q3 --EOF--> q4     (accept)
```

### The CL Toolbox

**Python libraries:**

| Library | Purpose | Key features |
|---|---|---|
| NLTK | Education, research | Corpora, tokenizers, taggers, parsers |
| spaCy | Production NLP | Fast, pipeline, named entities, dependencies |
| Stanza | Research NLP | Neural pipeline, 100+ languages |
| Hugging Face Transformers | SOTA models | BERT, GPT, T5, thousands of models |
| Flair | Sequence labeling | BERT-based, character-level |
| AllenNLP | Research | Configurable, reproducible experiments |
| CoreNLP (Java) | Comprehensive | Syntax, sentiment, coreference |

**Key CL datasets:**

| Dataset | Task | Size |
|---|---|---|
| Penn Treebank | Parsing, POS | 1M words |
| SQuAD | Reading comprehension | 100k questions |
| GLUE / SuperGLUE | General NLU | Multiple tasks |
| CoNLL | NER, parsing | 1M+ tokens |
| Common Sense QA | Reasoning | 12k questions |
| MultiNLI | Natural language inference | 433k pairs |
| WikiText | Language modeling | 100M+ tokens |

**CL tasks and dataset sizes:**

| Task | Benchmark | Training size | SOTA (2024) |
|---|---|---|---|
| Text classification | GLUE (MNLI) | 393k | 92.7% (DeBERTa) |
| Question answering | SQuAD 2.0 | 130k | 95.0% F1 (LLM) |
| NER | CoNLL 2003 | 14k sentences | 95.2% F1 (T5) |
| Parsing | PTB | 40k sentences | 97.5% UAS (Transformer) |
| Translation | WMT 2014 En-De | 4.5M pairs | 36.5 BLEU (Transformer) |

### Evaluating CL Systems

**Intrinsic evaluation:**

Measuring performance on the specific task:

| Task | Metric | What it measures |
|---|---|---|
| Classification | Accuracy, F1, Precision, Recall | Correctness |
| Parsing | UAS, LAS (Unlabeled/Labeled Attachment Score) | Structure correctness |
| Translation | BLEU, METEOR, chrF | Output quality vs. reference |
| Summarization | ROUGE-1, ROUGE-2, ROUGE-L | Content overlap with reference |
| Language model | Perplexity | Predictive accuracy |
| Generation | BLEU, METEOR, human evaluation | Quality, fluency, adequacy |

**Extrinsic evaluation:**

Measuring performance on the downstream task:

- Does the improved POS tagger make the parser better?
- Does the better parser improve the relation extraction system?
- Does the relation extraction system improve the knowledge base?

**Human evaluation:**

For many CL tasks, automated metrics are imperfect:

- BLEU correlates poorly with human judgment for some languages
- ROUGE misses semantic equivalence ("the car" vs "the vehicle")
- Human evaluation is expensive but sometimes necessary

**Evaluation pitfalls:**

| Pitfall | Example | Fix |
|---|---|---|
| Test set leakage | Training on test data | Hold out test set, don't look at it |
| Metric hacking | Optimizing BLEU without improving quality | Human evaluation |
| Inappropriate metric | Using accuracy on imbalanced data | F1, precision-recall curves |
| Single-split evaluation | Lucky train/test split | Cross-validation, multiple seeds |
| Ignoring variance | Reporting one run | Report mean + std over multiple runs |

### Ethics in Computational Linguistics

**Bias in language models:**

| Source | Example | Impact |
|---|---|---|
| Training data bias | Overrepresentation of certain demographics | Models perform worse for underrepresented groups |
| Labeling bias | Annotator demographics influence labels | Systematic errors for certain varieties of language |
| Task framing | Binary gender classification | Erasure of non-binary identities |
| Use context | Deployed without safety checks | Harassment, misinformation |

**Language preservation vs. extraction:**

CL tools can help preserve endangered languages — or extract knowledge from communities without benefit. Researchers should:
- Work with language communities
- Ensure benefits flow back to the community
- Respect cultural protocols around language use

**Privacy:**

- Text corpora may contain personal information
- Language models memorize training data (can leak passwords, names)
- De-anonymization is increasingly possible with language models

**Responsible CL practice:**

1. Document the dataset: source, demographics, collection method, limitations
2. Evaluate for bias: stratified evaluation across demographic groups
3. Consider dual use: could this technology be used for harm?
4. Involve stakeholders: include affected communities in design
5. Be transparent: publish model cards and data statements

### Current Research Directions

**Large Language Models (LLMs):**

- Scaling laws: performance improves predictably with compute, data, parameters
- Emergent abilities: capabilities that appear at certain scales
- In-context learning: learning from examples in the prompt
- Instruction following: models fine-tuned to follow directions
- Retrieval-augmented generation (RAG): combining LLMs with external knowledge

**Multimodal language models:**

- GPT-4V, CLIP, DALL-E — models that understand images and text
- Speech-text models — Whisper, Gato
- Video-language models — understanding video with language

**Understanding neural networks for language:**

- Probing: what do language models know about syntax, semantics, world knowledge?
- Mechanistic interpretability: how do specific circuits in transformers implement linguistic operations?
- Causal analysis: what changes in model behavior when components are ablated?

**Low-resource languages:**

- Transfer learning: adapting models from high-resource to low-resource languages
- Multilingual models: mBERT, XLM-R covering 100+ languages
- Zero-shot learning: applying models to languages not seen in training

**Human-in-the-loop CL:**

- Active learning: models ask for labels on uncertain examples
- Interactive learning: models learn from corrections during use
- Human evaluation: integrating human judgment into evaluation

## Glossary

| Term | Definition |
|---|---|
| Attention | Mechanism allowing a model to focus on relevant parts of the input |
| BLEU | Metric for machine translation based on n-gram overlap with reference |
| Constituency | Phrase structure analysis of sentences |
| Corpus (pl. corpora) | Large collection of texts for linguistic analysis |
| Dependency | Grammatical relationship between a head word and its dependents |
| Embedding | Dense vector representation of a word, phrase, or sentence |
| N-gram | Contiguous sequence of n items from a text |
| NER | Named Entity Recognition — identifying entities (people, places, etc.) |
| Parse tree | Tree structure representing the grammatical analysis of a sentence |
| ROUGE | Metric for summarization based on n-gram overlap with reference |
| Transformer | Neural architecture based entirely on attention mechanisms |

## Quick References

- [Jurafsky & Martin: Speech and Language Processing](https://web.stanford.edu/~jurafsky/slp3/) — comprehensive CL textbook (free online)
- [NLTK Book](https://www.nltk.org/book/) — practical introduction to CL with Python
- [Hugging Face Course](https://huggingface.co/learn/nlp-course) — practical NLP with transformers

## Next Steps

- [Formal Languages & Grammars](../formal-languages.md) — the mathematical foundations of language
- [Parsing: Natural & Programming Languages](../parsing.md) — algorithms for syntactic analysis
