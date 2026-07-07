# Language Models & Linguistic Theory

## Description

Language models — from simple n-gram models to large transformers — are the most visible application of computational linguistics today. Understanding the linguistic theory behind these models reveals what they capture, what they miss, and how they relate to the Chomskyan and statistical traditions in linguistics. For developers, this knowledge is essential for working effectively with LLMs, prompt engineering, and building language-aware applications.

## Prerequisites

- [Formal Languages & Grammars](formal-languages.md) — grammars, the Chomsky hierarchy, and the formal structure of language
- [Semantics](semantics.md) — meaning in language and code, including lexical and compositional semantics

## Table of Contents

- [What Are Language Models?](#what-are-language-models)
- [N-Gram Language Models](#n-gram-language-models)
- [Neural Language Models](#neural-language-models)
- [Transformers and Attention](#transformers-and-attention)
- [Connection to Linguistic Theory](#connection-to-linguistic-theory)
- [Chomsky vs. Statistical Approaches](#chomsky-vs-statistical-approaches)
- [LLMs and Syntax](#llms-and-syntax)
- [LLMs and Semantics](#llms-and-semantics)
- [LLMs and Pragmatics](#llms-and-pragmatics)
- [Training Data and Linguistic Bias](#training-data-and-linguistic-bias)
- [Tokenization and Morphology](#tokenization-and-morphology)
- [Linguistic Evaluation Benchmarks](#linguistic-evaluation-benchmarks)
- [Limitations of Current Models](#limitations-of-current-models)
- [Future Directions](#future-directions)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### What Are Language Models?

A language model is a probability distribution over token sequences. Given previous tokens, it predicts the next token.

```
P("the cat sat on the mat") should be HIGH
P("sat cat the mat on the") should be LOW
```

**Applications:** Autocomplete, machine translation, speech recognition, spelling correction, text generation, code completion.

**Paradigms:**

| Paradigm | Approach | Example models |
|----------|----------|----------------|
| Statistical | Count-based probabilities | N-gram models |
| Neural | Learned representations | RNNs, Transformers |

### N-Gram Language Models

N-gram models estimate probability of a word given the previous n-1 words (Markov assumption).

```
P(wn | w1, ..., w_{n-1}) ~= P(wn | w_{n-1}, ..., w_{n-N+1})
```

**Types:** Unigram (no context), Bigram (previous word), Trigram (previous 2 words).

**Maximum likelihood estimation:** `P(wi | w_{i-1}) = count(w_{i-1}, wi) / count(w_{i-1})`

A practical Python implementation of a bigram model:

```python
from collections import defaultdict, Counter
import math

class BigramLM:
    def __init__(self):
        self.unigrams = Counter()
        self.bigrams = Counter()
        self.vocab_size = 0

    def train(self, sentences):
        for sentence in sentences:
            tokens = ["<s>"] + sentence.split() + ["</s>"]
            for i in range(len(tokens)):
                self.unigrams[tokens[i]] += 1
                if i > 0:
                    self.bigrams[(tokens[i-1], tokens[i])] += 1
        self.vocab_size = len(self.unigrams)

    def prob(self, w2, w1):
        return self.bigrams[(w1, w2)] / self.unigrams[w1]

    def prob_laplace(self, w2, w1):
        return (self.bigrams[(w1, w2)] + 1) / (self.unigrams[w1] + self.vocab_size)

    def sentence_prob(self, sentence):
        tokens = ["<s>"] + sentence.split() + ["</s>"]
        log_prob = 0.0
        for i in range(1, len(tokens)):
            p = self.prob_laplace(tokens[i], tokens[i-1])
            log_prob += math.log2(p)
        return log_prob

corpus = ["the cat sat on the mat",
          "the dog sat on the rug",
          "cats and dogs are pets"]
model = BigramLM()
model.train(corpus)
print(model.sentence_prob("the cat sat"))  # higher probability
print(model.sentence_prob("cat the sat"))  # lower probability
```

**Sparse data problem:** "the dog" may have zero count even though "the cat" appears. Solution: smoothing.

| Technique | Idea |
|-----------|------|
| Add-1 (Laplace) | Add 1 to all counts |
| Kneser-Ney | Best performer, uses context diversity |
| Interpolation | Mix higher and lower order n-grams |

**Limitations:** Fixed context window, no generalization, no meaning, domain-sensitive. A trigram model cannot capture agreement across a 10-word gap.

### Neural Language Models

Neural LMs learn distributed representations (embeddings) for words.

**Word embeddings:** Dense vectors where similar words have similar vectors.

```
vec("king") - vec("man") + vec("woman") ~= vec("queen")
```

This analogy property emerges because embeddings capture distributional semantics — words in similar contexts have similar representations.

**Recurrent Neural Networks (RNNs):** Process sequences with a hidden state.

```
h_t = tanh(W_h * h_{t-1} + W_x * x_t + b)
y_t = softmax(W_y * h_t + c)
```

The hidden state `h_t` theoretically captures all previous context, but in practice RNNs suffer from vanishing gradients — gradients shrink exponentially with distance, making it hard to learn long-range dependencies.

**LSTM (Long Short-Term Memory):** Solves vanishing gradients with gating mechanisms (input, forget, output gates). Captures dependencies up to ~200 tokens.

```
i_t = sigmoid(W_i * [h_{t-1}, x_t] + b_i)    # input gate
f_t = sigmoid(W_f * [h_{t-1}, x_t] + b_f)    # forget gate
o_t = sigmoid(W_o * [h_{t-1}, x_t] + b_o)    # output gate
c_t = f_t * c_{t-1} + i_t * tanh(W_c * [h_{t-1}, x_t] + b_c)
h_t = o_t * tanh(c_t)
```

**Perplexity:** Measures model uncertainty. Lower = better. N-gram: ~200-1000. GPT-3: ~10-20.

Perplexity is calculated as `PP(W) = P(w1, w2, ..., wN)^{-1/N}`. It represents the average branching factor — how many equally likely next tokens the model considers at each position.

### Transformers and Attention

The Transformer (Vaswani et al., 2017) uses only attention, enabling parallel computation.

**Self-attention:** Each token attends to every other token.

```
Attention(Q, K, V) = softmax(Q * K^T / sqrt(d_k)) * V
```

"sat" attends most to "cat" (subject-verb) and other tokens. The softmax produces attention weights that sum to 1, creating a weighted average of value vectors.

**Multi-head attention:** Multiple attention mechanisms in parallel. Heads specialize: subject-verb agreement, preposition attachment, coreference. A typical model uses 12-96 heads, each learning a different linguistic function.

**Positional encodings:** Since attention has no inherent notion of order, sinusoidal position encodings or learned position embeddings are added to input tokens.

**Scaling laws:** Performance improves predictably with parameters, data, and compute (Kaplan et al., 2020). Doubling parameters requires doubling training tokens for optimal allocation.

| Model | Parameters | Year |
|-------|-----------|------|
| BERT-base | 110M | 2018 |
| GPT-2 | 1.5B | 2019 |
| GPT-3 | 175B | 2020 |
| GPT-4 | ~1.8T (est.) | 2023 |

### Connection to Linguistic Theory

**Linguistic levels in LMs:**

| Level | What LMs capture |
|-------|------------------|
| Phonology | Subword patterns |
| Morphology | Word structure |
| Syntax | Agreement, constituency |
| Semantics | Word and sentence similarity |
| Pragmatics | Context, instruction following |

**Probing studies** train classifiers on LM representations to test linguistic knowledge. BERT layer 6 achieves ~95% for POS tagging. Middle layers of GPT-2 achieve ~85% for constituent boundaries.

**Attention head analysis** reveals specialization: some heads track subject-verb agreement, others handle anaphora, others detect negation scope. This mirrors how linguistic theory decomposes language into interacting modules.

### Chomsky vs. Statistical Approaches

**Chomsky's position:** Language is a generative rule system. Statistical patterns are epiphenomenal. Poverty of the stimulus means children learn from limited data, implying innate Universal Grammar.

**Statistical response:** Language is fundamentally statistical. Neural LMs learn rule-like behavior from distributional patterns. The poverty of the stimulus argument is overstated — rich patterns exist in child-directed speech.

**Current synthesis:**

```
Linguistic theory provides vocabulary for describing model behavior.
Statistical learning provides mechanisms that work at scale.
```

A key unresolved question: do neural LMs learn hierarchical syntactic rules, or do they approximate hierarchical behavior through shallow pattern matching? Evidence exists for both positions.

### LLMs and Syntax

**What LLMs get right:** Subject-verb agreement across intervening material, long-distance dependencies, negative polarity items.

```
"The keys to the cabinet are on the table." — High probability (correct)
"*The keys to the cabinet is on the table." — Low probability (correct)
```

**What LLMs get wrong:** Deep center-embedding, syntactic illusions. Performance degrades with distance.

Example of center-embedding failure:
```
"The rat the cat the dog chased ate the cheese." — LLMs assign low probability
```
Three levels of embedding overwhelm the model's linear processing.

**Syntax benchmarks:** BLiMP (~80-90% accuracy), SyntaxGym (~75-85%).

### LLMs and Semantics

**What LLMs know:** Word sense disambiguation (contextual embeddings distinguish "bank" as financial institution vs. river bank), semantic similarity, entailment recognition.

**What LLMs miss:** Compositional generalization (failing on novel combinations), grounding (no real-world experience), numeracy (fails on novel numbers), negation reasoning.

**The "Stochastic Parrot" debate (Bender et al., 2021):** LLMs produce fluent text by stitching training patterns without understanding. Counterargument: human language also relies heavily on pattern recognition; emergent reasoning abilities suggest more than surface matching.

### LLMs and Pragmatics

**Pragmatic abilities:** Recognizing implicature ("Can you open the window?" -> request), scalar implicature (some -> not all), adjusting register.

**Pragmatic failures:** Hallucination (confabulating facts), over-accommodation (agreeing with false premises), missing implicature (literal interpretation of requests).

**Grice's maxims in LLM output:**

| Maxim | Performance | Failure |
|-------|-------------|---------|
| Quantity | Often verbose | Provides too much detail |
| Quality | Often invents | Hallucination |
| Relation | Mostly relevant | Topic drift |
| Manner | Clear but verbose | Unnecessarily complex |

### Training Data and Linguistic Bias

**Common sources:** Common Crawl (~60%), Books (~15%), Wikipedia (~5%), Reddit (~10%).

**Linguistic biases:**

| Bias | Effect |
|------|--------|
| English dominance | English performance > Swahili |
| Formal register | Struggles with informal dialects (AAVE) |
| Western knowledge | Weak on non-Western culture |
| Language distribution | English ~80%, German ~5%, French ~4% |

The overrepresentation of English means that models learn English-specific patterns as if they were universal linguistic properties. Morphologically rich languages like Turkish or Finnish are poorly served by tokenizers optimized for English.

### Tokenization and Morphology

**Tokenization methods:**

| Method | Example: "unbreakable" |
|--------|----------------------|
| Word-level | ["unbreakable"] |
| BPE | ["un", "break", "able"] |
| SentencePiece | ["un", "break", "able"] |
| Unigram LM | ["un", "break", "able"] |

**Byte-Pair Encoding (BPE):** Iteratively merges most frequent character pairs. The merge operations are learned from training data, so the tokenizer reflects the most common subword units in the corpus.

```
English: 1 word ~= 1.2 tokens
Turkish: 1 word ~= 3-5 tokens
Chinese: 1 word ~= 2-3 characters
```

**Tokenization artifacts:**

- Spelling sensitivity: "don't" vs "dont" produce different token sequences, confusing the model
- False compositionality: "butterfly" tokenizes as "butter" + "fly", but neither subword relates to the meaning
- Number tokenization: "42" might be one token while "43" is two ("4" + "3"), making arithmetic unreliable
- Whitespace sensitivity: extra spaces change token boundaries and affect output quality

These artifacts show how tokenization is not linguistically neutral — it imposes a particular view of morphological structure.

### Linguistic Evaluation Benchmarks

| Benchmark | Task | Skill tested |
|-----------|------|-------------|
| GLUE/SuperGLUE | Multiple NLU tasks | General understanding |
| BLiMP | 67 grammatical contrasts | Syntax |
| SQuAD | Reading comprehension | QA + inference |
| MMLU | 57 subjects | Knowledge + reasoning |
| TruthfulQA | Factuality | Truthfulness |

BLiMP (Benchmark of Linguistic Minimal Pairs) presents pairs of sentences that differ only in grammaticality. The model must assign higher probability to the grammatical version. Examples include:
- Subject-verb agreement: "The dogs run" vs "*The dogs runs"
- Anaphor agreement: "John hurt himself" vs "*John hurt herself"
- NPI licensing: "She has ever been" vs "*She has ever gone" (ungrammatical without negation)

**Benchmark limitations:**

- **Data contamination:** Test data may appear in pretraining corpora, inflating scores
- **Metric hacking:** Models exploit surface patterns rather than demonstrating true understanding
- **English bias:** Most benchmarks test English, overstating cross-linguistic capability
- **Saturation:** Models quickly reach ceiling on older benchmarks (GLUE is now trivial for large models)
- **Construct validity:** A high benchmark score does not guarantee the model has learned the underlying linguistic principle

### Limitations of Current Models

**Linguistic:**

- Poor systematic generalization — failing to apply learned patterns to novel compositions. A model that understands "the dog chases the cat" may fail on "the cat chases the dog" in zero-shot settings.
- Reliance on linear patterns over hierarchical structure — models are sensitive to word order statistics rather than tree structure
- No grounding — language is learned from text alone, with no connection to perception, action, or physical experience
- Low robustness to perturbations — synonym substitution, minor reordering, or typos can cause large performance drops

**Cognitive:**

- Limited memory — even with 128K token context windows, models lose information in the middle (the "lost in the middle" problem)
- Inconsistency across rephrasings — answering correctly one way but incorrectly when the same question is rephrased
- Catastrophic forgetting — fine-tuning on new tasks degrades performance on previously learned tasks
- Confabulation — the model generates fluent but incorrect information without awareness of its falsehood

**Engineering:**

- High cost — GPT-4 training estimated at ~$100M, GPT-5 likely higher
- Latency — large models require seconds to generate even short responses
- Size — models weigh hundreds of gigabytes, requiring specialized hardware for inference
- Environmental impact — training a single large model can emit hundreds of tons of CO2

### Future Directions

**Neuro-symbolic approaches:** Neural LMs + symbolic grammar validation, combining distributional learning with formal guarantees.

**Mechanistic interpretability:** Understanding how transformer circuits implement linguistic computations, enabling targeted editing.

**Multimodal models:** Vision-language (GPT-4V), speech-language (Whisper), embodied language (robots following instructions).

**Low-resource and multilingual:** Reducing English dominance, better tokenization for non-English languages.

**Controlled generation:** Moving beyond next-token prediction to goal-directed generation with control over sentiment, formality, factuality.

## Learning Tips

- Build an n-gram model from scratch on a small corpus to understand the sparsity problem firsthand. Start with a children's book or small Wikipedia dump.
- Use probing classifiers on pretrained embeddings (e.g., BERT or GPT-2 via Hugging Face) to see what linguistic information each layer encodes. This makes abstract linguistic concepts tangible.
- Compare perplexity scores between n-gram and neural models on the same evaluation set to quantify the gap.
- Read Bender et al. (2021) alongside Chomsky's early work to understand the philosophical divide — both positions are worth taking seriously.
- Test an LLM on controlled linguistic phenomena (e.g., center-embedded sentences, garden-path sentences) to observe where it fails. This reveals the gap between surface fluency and deep syntactic understanding.
- When studying tokenization, inspect actual token IDs for different languages. Notice how token-per-word ratios differ and what that implies for model capacity allocation.

## Glossary

| Term | Definition |
|------|------------|
| Attention | Mechanism that weights the importance of different input positions when producing output |
| BLiMP | Benchmark of Linguistic Minimal Pairs — 67 syntactic contrasts for evaluating grammatical knowledge |
| BPE | Byte-Pair Encoding — subword tokenization algorithm that iteratively merges frequent character pairs |
| Bytecode | Intermediate representation in JIT-compiled languages (not directly relevant here — included for completeness) |
| Catastrophic forgetting | Loss of previously learned capabilities when a model is fine-tuned on new tasks |
| Center-embedding | A syntactic structure where a clause is embedded inside another clause of the same type |
| Compositional generalization | The ability to understand novel combinations of familiar linguistic elements |
| Context window | The maximum number of tokens a model can consider at once |
| Data contamination | Overlap between training data and test data that artificially inflates benchmark scores |
| Embedding | Dense vector representation of a token in a continuous space |
| Fine-tuning | Adapting a pretrained model to a specific task by continuing training on task data |
| Garden-path sentence | A grammatically correct sentence that leads the reader toward an incorrect parse |
| Grounding | Connecting language to real-world perceptual or physical experience |
| Hallucination | Fluent but factually incorrect or fabricated output from a language model |
| Implicature | Meaning that is implied rather than explicitly stated (e.g., "It's cold in here" = shut the window) |
| Language model | A probability distribution over sequences of tokens |
| LSTM | Long Short-Term Memory — RNN variant with gating mechanisms for long-range dependencies |
| Markov assumption | The assumption that the next token depends only on the n-1 previous tokens |
| Minimal pair | Two sentences that differ in only one linguistic feature (one grammatical, one not) |
| N-gram | A contiguous sequence of n items (words, characters, or subword tokens) |
| Negative polarity item | An expression that only appears in negative contexts (e.g., "any", "ever") |
| Perplexity | A measure of model uncertainty — lower values indicate better predictive performance |
| Probing | Training a classifier on a model's internal representations to test what linguistic information is encoded |
| RNN | Recurrent Neural Network — processes sequences by maintaining a hidden state across time steps |
| Scaling law | Empirical relationship showing that model performance improves predictably with size, data, and compute |
| Self-attention | Attention mechanism where each token in a sequence attends to every other token |
| Smoothing | Techniques for handling zero-count probability estimates in n-gram models |
| Stochastic parrot | A critical term describing LMs as systems that produce fluent text by stitching patterns without understanding |
| Token | The atomic unit of text that a language model processes (word, subword, or character) |
| Tokenization | The process of converting text into discrete tokens for model input |
| Transformer | Neural architecture based solely on self-attention mechanisms (Vaswani et al., 2017) |
| Universal Grammar | Chomsky's hypothesis that humans have innate, domain-specific linguistic knowledge |
| Vanishing gradient | The problem where gradients shrink exponentially through time steps, preventing RNNs from learning long-range dependencies |

## Quick References

- [Jurafsky & Martin: Speech and Language Processing](https://web.stanford.edu/~jurafsky/slp3/) — authoritative textbook covering n-gram models to neural approaches
- [Vaswani et al.: Attention Is All You Need (2017)](https://arxiv.org/abs/1706.03762) — the original Transformer paper
- [Kaplan et al.: Scaling Laws for Neural Language Models (2020)](https://arxiv.org/abs/2001.08361) — empirical study of how model performance scales
- [Bender et al.: On the Dangers of Stochastic Parrots (2021)](https://dl.acm.org/doi/10.1145/3442188.3445922) — influential critique of large language models
- [Chomsky: Syntactic Structures (1957)](https://www.degruyter.com/document/doi/10.1515/9783112316009/html) — foundational text in generative linguistics
- [Hugging Face NLP Course](https://huggingface.co/learn/nlp-course) — practical tutorials for working with transformer models
- [The Annotated Transformer](http://nlp.seas.harvard.edu/2018/04/03/attention.html) — line-by-line implementation of the Transformer in PyTorch
- [Goldberg: Neural Network Methods for NLP](https://www.morganclaypool.com/doi/10.2200/S00762ED1V01Y201703HLT037) — comprehensive survey of neural approaches to language
- [Warstadt et al.: BLiMP (2020)](https://arxiv.org/abs/1912.00582) — the Benchmark of Linguistic Minimal Pairs
- [Rogers et al.: A Primer in BERTology (2021)](https://arxiv.org/abs/2002.12327) — what we know about how BERT works linguistically

## Next Steps

- [Parsing](parsing.md) — syntactic analysis algorithms and their relationship to language models
- [Formal Languages & Grammars](formal-languages.md) — the Chomsky hierarchy and generative grammar foundations
