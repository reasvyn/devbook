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
- [Study Cases](#study-cases)
- [Examples](#examples)
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

**Sparse data problem:** "the dog" may have zero count even though "the cat" appears. Solution: smoothing.

| Technique | Idea |
|-----------|------|
| Add-1 (Laplace) | Add 1 to all counts |
| Kneser-Ney | Best performer, uses context diversity |
| Interpolation | Mix higher and lower order n-grams |

**Limitations:** Fixed context window, no generalization, no meaning, domain-sensitive.

### Neural Language Models

Neural LMs learn distributed representations (embeddings) for words.

**Word embeddings:** Dense vectors where similar words have similar vectors.

```
vec("king") - vec("man") + vec("woman") ~= vec("queen")
```

**Recurrent Neural Networks (RNNs):** Process sequences with a hidden state.

```
h_t = tanh(W_h * h_{t-1} + W_x * x_t + b)
y_t = softmax(W_y * h_t + c)
```

**LSTM (Long Short-Term Memory):** Solves vanishing gradients with gating mechanisms (input, forget, output gates). Captures dependencies up to ~200 tokens.

**Perplexity:** Measures model uncertainty. Lower = better. N-gram: ~200-1000. GPT-3: ~10-20.

### Transformers and Attention

The Transformer (Vaswani et al., 2017) uses only attention, enabling parallel computation.

**Self-attention:** Each token attends to every other token.

```
Attention(Q, K, V) = softmax(Q * K^T / sqrt(d_k)) * V
```

"sat" attends most to "cat" (subject-verb) and other tokens.

**Multi-head attention:** Multiple attention mechanisms in parallel. Heads specialize: subject-verb agreement, preposition attachment, coreference.

**Scaling laws:** Performance improves predictably with parameters, data, and compute (Kaplan et al., 2020).

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

**Attention head analysis** reveals specialization: some heads track subject-verb agreement, others handle anaphora, others detect negation scope.

### Chomsky vs. Statistical Approaches

**Chomsky's position:** Language is a generative rule system. Statistical patterns are epiphenomenal. Poverty of the stimulus means children learn from limited data, implying innate Universal Grammar.

**Statistical response:** Language is fundamentally statistical. Neural LMs learn rule-like behavior from distributional patterns. The poverty of the stimulus argument is overstated — rich patterns exist in child-directed speech.

**Current synthesis:**

```
Linguistic theory provides vocabulary for describing model behavior.
Statistical learning provides mechanisms that work at scale.
```

### LLMs and Syntax

**What LLMs get right:** Subject-verb agreement across intervening material, long-distance dependencies, negative polarity items.

```
"The keys to the cabinet are on the table." — High probability (correct)
"*The keys to the cabinet is on the table." — Low probability (correct)
```

**What LLMs get wrong:** Deep center-embedding, syntactic illusions. Performance degrades with distance.

**Syntax benchmarks:** BLiMP (~80-90% accuracy), SyntaxGym (~75-85%).

### LLMs and Semantics

**What LLMs know:** Word sense disambiguation (contextual embeddings distinguish "bank"), semantic similarity, entailment recognition.

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

### Tokenization and Morphology

**Tokenization methods:**

| Method | Example: "unbreakable" |
|--------|----------------------|
| Word-level | ["unbreakable"] |
| BPE | ["un", "break", "able"] |
| SentencePiece | ["un", "break", "able"] |

**Byte-Pair Encoding (BPE):** Iteratively merges most frequent char pairs. Morphologically rich languages get more tokens per word.

```
English: 1 word ~= 1.2 tokens
Turkish: 1 word ~= 3-5 tokens
Chinese: 1 word ~= 2-3 characters
```

**Tokenization artifacts:** Spelling sensitivity ("don't" vs "dont"), false subword compositionality ("butterfly" = "butter" + "fly").

### Linguistic Evaluation Benchmarks

| Benchmark | Task | Skill tested |
|-----------|------|-------------|
| GLUE/SuperGLUE | Multiple NLU tasks | General understanding |
| BLiMP | 67 grammatical contrasts | Syntax |
| SQuAD | Reading comprehension | QA + inference |
| MMLU | 57 subjects | Knowledge + reasoning |
| TruthfulQA | Factuality | Truthfulness |

**Benchmark limitations:** Data contamination (test data in pretraining), metric hacking, English bias, saturation over time.

### Limitations of Current Models

**Linguistic:** Poor systematic generalization, reliance on linear patterns over hierarchical structure, no grounding, low robustness to perturbations.

**Cognitive:** Limited memory (even with long context), inconsistency across rephrasings, catastrophic forgetting, confabulation.

**Engineering:** High cost (GPT-4 training ~$100M), latency, size (hundreds of GB), environmental impact.

### Future Directions

**Neuro-symbolic approaches:** Neural LMs + symbolic grammar validation, combining distributional learning with formal guarantees.

**Mechanistic interpretability:** Understanding how transformer circuits implement linguistic computations, enabling targeted editing.

**Multimodal models:** Vision-language (GPT-4V), speech-language (Whisper), embodied language (robots following instructions).

**Low-resource and multilingual:** Reducing English dominance, better tokenization for non-English languages.

**Controlled generation:** Moving beyond next-token prediction to goal-directed generation with control over sentiment, formality, factuality.

## Study Cases

### Case 1: Evaluating BERT's Syntactic Knowledge

**Method:** Minimal pairs testing subject-verb agreement.

```python
pairs = [
    ("The keys to the cabinet are lost.", "The keys to the cabinet is lost."),
    ("The key to the cabinets is lost.", "The key to the cabinets are lost."),
]
```

**Results:** 99% correct with no attractor, 95% with 1 attractor, 80% with 2 attractors. BERT knows agreement but degrades with distance.

### Case 2: Tokenization and Language Bias

A multilingual app using BPE: Turkish gets 4.2 tokens/word vs English 1.2 tokens/word. Same 4096-token context handles ~3400 English words but ~975 Turkish words.

**Mitigation:** Language-specific tokenizers, increased context for non-English, training in target language.

### Case 3: Prompt Engineering with Grice's Maxims

**Before:** `"Write code for this."` (violates all maxims).

**After:**

```
Write a Python function called validate_email that:
1. Takes a string parameter 'email'
2. Returns True if valid, False if not
3. Uses standard regex pattern
4. Does not make network calls
Use type hints. Include a docstring.
```

Following Grice gives more reliable, focused output.

## Examples

### Example 1: N-Gram Model Implementation

```python
from collections import defaultdict
import math

class BigramModel:
    def __init__(self):
        self.counts = defaultdict(lambda: defaultdict(int))
        self.context_counts = defaultdict(int)
        self.vocab = set()

    def train(self, sentences):
        for s in sentences:
            tokens = ["<s>"] + s.split() + ["</s>"]
            for i in range(1, len(tokens)):
                self.counts[tokens[i-1]][tokens[i]] += 1
                self.context_counts[tokens[i-1]] += 1
                self.vocab.add(tokens[i])

    def probability(self, word, context):
        if self.context_counts[context] == 0:
            return 1.0 / len(self.vocab)
        count = self.counts[context][word]
        return max(count, 0.01) / self.context_counts[context]

    def perplexity(self, sentences):
        log_prob = 0.0
        tokens_total = 0
        for s in sentences:
            tokens = ["<s>"] + s.split() + ["</s>"]
            for i in range(1, len(tokens)):
                log_prob += math.log2(self.probability(tokens[i], tokens[i-1]))
                tokens_total += 1
        return 2 ** (-log_prob / tokens_total)
```

### Example 2: Self-Attention Step-by-Step

Input: "I love programming" with d=4.

Positions: compute Q, K, V for each token. Score("love", "I") = 0.6, score("love", "love") = 0.8, score("love", "programming") = 0.3. Softmax: [0.35, 0.47, 0.18]. Output: weighted sum of values = contextual representation of "love".

### Example 3: Probing for Parts of Speech

```python
from transformers import AutoModel, AutoTokenizer

model = AutoModel.from_pretrained("bert-base-uncased")
tokenizer = AutoTokenizer.from_pretrained("bert-base-uncased")

text = "The cat sat on the mat."
inputs = tokenizer(text, return_tensors="pt")
outputs = model(**inputs)  # last_hidden_state: representations

# Train linear probe on labeled data
# probe = Linear(768, 45) trained for 10 epochs on Penn Treebank
```

### Example 4: Perplexity Comparison

| Model | Perplexity |
|-------|------------|
| Uniform (50k vocab) | 50,000 |
| Unigram | 962 |
| Bigram (Kneser-Ney) | 139 |
| LSTM (2 layers) | 78 |
| GPT-3 (zero-shot) | 20 |

### Example 5: Minimal Pair Testing for Agreement

```python
def test_agreement(model, tokenizer):
    pairs = [
        ("The cat runs.", "The cat run."),
        ("The cats run.", "The cats runs."),
    ]
    correct = sum(
        score_sequence(model, tokenizer, g) >
        score_sequence(model, tokenizer, b)
        for g, b in pairs
    )
    print(f"Accuracy: {correct}/{len(pairs)}")
```

## Glossary

| Term | Definition |
|------|------------|
| Attention | Mechanism weighting importance of different input positions |
| BLiMP | Benchmark of Linguistic Minimal Pairs |
| BPE | Byte-Pair Encoding — subword tokenization algorithm |
| Embedding | Dense vector representation of a token |
| Fine-tuning | Adapting a pretrained model to a task |
| Grounding | Connecting language to real-world experience |
| Hallucination | Fluent but factually incorrect output |
| Language model | Probability distribution over token sequences |
| LSTM | RNN with gating for long-range dependencies |
| Markov assumption | Next word depends only on previous n words |
| N-gram | Contiguous sequence of n items |
| Perplexity | Measure of model uncertainty (lower is better) |
| Probing | Classifier on representations to test knowledge |
| RNN | Recurrent Neural Network for sequences |
| Scaling law | Relationship between size, data, and performance |
| Self-attention | Each token attends to all tokens in the sequence |
| Smoothing | Handling zero counts in n-gram models |
| Stochastic parrot | Critique: LMs mimic without understanding |
| Token | Unit of text processed by an LM |
| Tokenization | Converting text into tokens |
| Transformer | Architecture based on self-attention |

## Quick References

- [Jurafsky & Martin: Speech and Language Processing (LM chapters)](https://web.stanford.edu/~jurafsky/slp3/)
- [Vaswani et al.: Attention Is All You Need (2017)](https://arxiv.org/abs/1706.03762)
- [Kaplan et al.: Scaling Laws for Neural Language Models (2020)](https://arxiv.org/abs/2001.08361)
- [Bender et al.: On the Dangers of Stochastic Parrots (2021)](https://dl.acm.org/doi/10.1145/3442188.3445922)
- [Chomsky: Syntactic Structures (1957)](https://www.degruyter.com/document/doi/10.1515/9783112316009/html)
- [Hugging Face Course](https://huggingface.co/learn/nlp-course)
- [The Annotated Transformer](http://nlp.seas.harvard.edu/2018/04/03/attention.html)
- [Goldberg: Neural Network Methods for NLP](https://www.morganclaypool.com/doi/10.2200/S00762ED1V01Y201703HLT037)

## Next Steps

- [Parsing](parsing.md) — syntactic analysis algorithms and their relationship to language models
