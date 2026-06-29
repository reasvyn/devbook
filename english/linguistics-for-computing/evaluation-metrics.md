# Evaluation Methods for Language Systems

## Description

Evaluation is how we measure whether a language system works — and compare competing approaches. From classification metrics to human evaluation, the choice of metric shapes which research directions are pursued and which models are deployed. For developers building NLP systems, understanding evaluation tradeoffs prevents metric hacking, data leakage, and overclaiming results based on misleading numbers.

## Prerequisites

- [Formal Languages & Grammars](formal-languages.md) — the formal structure of language systems being evaluated
- [Corpora & Annotation](corpora-annotation.md) — how annotated test sets are created and validated

## Table of Contents

- [Intrinsic vs. Extrinsic Evaluation](#intrinsic-vs-extrinsic-evaluation)
- [Classification Metrics](#classification-metrics)
- [Accuracy and Its Pitfalls](#accuracy-and-its-pitfalls)
- [Precision, Recall, and F1](#precision-recall-and-f1)
- [Macro, Micro, and Weighted Averages](#macro-micro-and-weighted-averages)
- [Sequence Metrics: BLEU](#sequence-metrics-bleu)
- [Sequence Metrics: ROUGE](#sequence-metrics-rouge)
- [Sequence Metrics: METEOR and chrF](#sequence-metrics-meteor-and-chrf)
- [Sequence Metrics: WER and PER](#sequence-metrics-wer-and-per)
- [Parsing Metrics: UAS and LAS](#parsing-metrics-uas-and-las)
- [Language Model Evaluation: Perplexity](#language-model-evaluation-perplexity)
- [Language Model Evaluation: Downstream Tasks](#language-model-evaluation-downstream-tasks)
- [Human Evaluation](#human-evaluation)
- [Statistical Significance](#statistical-significance)
- [Bootstrap and Approximate Randomization](#bootstrap-and-approximate-randomization)
- [Evaluation Pitfalls](#evaluation-pitfalls)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Intrinsic vs. Extrinsic Evaluation

**Intrinsic evaluation** measures performance on the task itself using a dedicated test set:

```
Intrinsic: POS tagging accuracy on Penn Treebank
           Parsing UAS on Universal Dependencies
           BLEU score on WMT translation test set
```

**Extrinsic evaluation** measures impact on a downstream application:

```
Extrinsic: Does better POS tagging improve IE?
           Does lower perplexity improve chatbot ratings?
           Does higher BLEU improve user translation quality?
```

| Property | Intrinsic | Extrinsic |
|----------|-----------|-----------|
| Cost | Cheap, automated | Expensive, user studies |
| Reproducibility | High | Low |
| Diagnostic value | High | Low |
| Real-world relevance | Low | High |

**The gap:** A 0.1 BLEU improvement may not produce any noticeable difference. A 2-point perplexity drop may not improve chatbot ratings. Extrinsic validation is the final arbiter.

### Classification Metrics

Classification tasks include sentiment analysis, spam detection, intent classification.

**Confusion matrix:**

```
                Predicted
                Pos    Neg
Actual  Pos    TP     FN
        Neg    FP     TN
```

- **TP** (True Positive), **TN** (True Negative)
- **FP** (False Positive, Type I error)
- **FN** (False Negative, Type II error)

### Accuracy and Its Pitfalls

```
Accuracy = (TP + TN) / (TP + TN + FP + FN)
```

**The accuracy paradox:**

```python
# 1000 emails, 10 spam. Both classifiers:
acc1: always "legitimate"  -> (0+990)/1000 = 99%
acc2: detects 8/10 spam, 5 FP -> (8+985)/1000 = 99.3%
```

First classifier has zero spam detection. Accuracy fails for imbalanced data.

**When accuracy works:** Balanced classes (~50/50), equal error costs.

### Precision, Recall, and F1

```
Precision = TP / (TP + FP)   "Of predicted positives, how many are correct?"
Recall    = TP / (TP + FN)   "Of actual positives, how many were found?"
F1 = 2 * (P * R) / (P + R)  "Harmonic mean"
```

**Precision/recall tradeoff:**

```
Spam filter aggressive:  P=0.90, R=0.99, F1=0.943
Spam filter conservative: P=0.99, R=0.80, F1=0.886
Medical screening low threshold: P=0.05, R=0.99
```

**F-beta** weights recall more (beta > 1) or precision more:

```
F_beta = (1 + beta^2) * (P * R) / (beta^2 * P + R)
```

beta=2 weights recall 2x, beta=0.5 weights precision 2x.

### Macro, Micro, and Weighted Averages

For multi-class (e.g., NER with 6 classes):

| Average | Computation | When to use |
|---------|-------------|-------------|
| Micro | Global TP/FP/FN (equals accuracy) | Majority class priority |
| Macro | Mean per-class F1 | Rare classes matter most |
| Weighted | Macro weighted by class support | Balanced priority |

```python
from sklearn.metrics import classification_report

y_true = ["PER", "PER", "O", "O", "ORG", "O", "LOC"]
y_pred = ["PER", "O", "O", "O", "ORG", "ORG", "LOC"]

print(classification_report(y_true, y_pred))
```

```
              precision  recall  f1-score  support
       LOC       1.00    1.00     1.00       1
       ORG       1.00    1.00     1.00       1
       O         0.67    1.00     0.80       2
       PER       1.00    0.50     0.67       2
   micro avg     0.83    0.83     0.83       6
   macro avg     0.92    0.88     0.87       6
```

### Sequence Metrics: BLEU

BLEU (Papineni et al., 2002) measures n-gram overlap between candidate and reference translations.

```
BLEU = BP * exp(sum_{n=1}^{4} w_n * log(BLEU-n))

BP = exp(1 - ref_len / cand_len)  (brevity penalty, shorter cands penalized)
BLEU-n = clipped n-gram precision (each ref n-gram counted once)
```

**Example:**

```
Candidate: "the cat sat on the mat"
Reference: "the cat is on the mat"

Unigrams clipped: 5/7 = 0.71 (one "the" clipped)
Bigrams clipped:  2/6 = 0.33
BP = exp(1 - 6/7) = 0.868
BLEU = ~0.45
```

**Limitations:**

```
Reference: "the cat sat on the mat"
  A: "the cat sat on the mat"     BLEU=100
  B: "the feline rested on the rug" BLEU~30 (same meaning, different words)
  C: "mat the on sat cat the"     BLEU~60 (nonsense, same words)

BLEU ranks nonsense above a good paraphrase.
```

### Sequence Metrics: ROUGE

ROUGE (Lin, 2004) measures n-gram recall between reference and candidate. Used for summarization.

| Variant | Description |
|---------|-------------|
| ROUGE-1 | Unigram recall |
| ROUGE-2 | Bigram recall |
| ROUGE-L | Longest Common Subsequence |
| ROUGE-SU | Skip-bigram + unigram |

**Example:**

```
Reference: "The quick brown fox jumps over the lazy dog"
Candidate:  "The lazy dog was jumped over by the quick brown fox"

ROUGE-1: 8/10 = 0.80
ROUGE-2: 2/9 = 0.22
ROUGE-L: LCS=9, 9/10 = 0.90
```

**BLEU vs. ROUGE:**

| Aspect | BLEU | ROUGE |
|--------|------|-------|
| Focus | Precision | Recall |
| Target | Translation | Summarization |
| Brevity penalty | Yes | No (precision variant exists) |

### Sequence Metrics: METEOR and chrF

**METEOR:** Aligns using exact, stem, synonym, and paraphrase matches:

```
METEOR = F_mean * (1 - Penalty)
Penalty = 0.5 * (chunks / matched)^3
```

More chunks (discontiguous matches) increase penalty.

**chrF (Popovic, 2015):** Character n-gram F-score. Handles morphology better:

```python
import sacrebleu

candidate = "the cat sat on the mat"
reference = "the cat is on the mat"

bleu = sacrebleu.sentence_bleu(candidate, [reference])
chrf = sacrebleu.sentence_chrf(candidate, [reference])

print(f"BLEU: {bleu.score:.2f}, chrF: {chrf.score:.2f}")
```

| Metric | Level | Best for |
|--------|-------|----------|
| BLEU | Word n-grams | Translation |
| ROUGE | Word n-grams + LCS | Summarization |
| METEOR | Word + stem | Translation (best correlation) |
| chrF | Character n-grams | Morphologically rich |
| BLEURT | Neural embeddings | Any (uses learned model) |

### Sequence Metrics: WER and PER

**Word Error Rate** for speech recognition:

```
WER = (S + D + I) / N  (substitutions + deletions + insertions) / ref length

Ref: "the cat sat on the mat"
Hyp: "the cat on the mat"
       S=0, D=1, I=0, N=6
       WER = 1/6 = 0.167
```

**PER (Position-independent WER):** Ignores word order, used for translation.

### Parsing Metrics: UAS and LAS

**UAS** (Unlabeled Attachment Score): % of words with correct head.

**LAS** (Labeled Attachment Score): % of words with correct head and label.

```
Reference:  "fox" -> nsubj -> "jumps"
Predicted:  "fox" -> dobj  -> "jumps"

UAS: head "jumps" is correct -> pass
LAS: label "dobj" != "nsubj" -> fail
```

**Exact match:** Percentage of sentences with 100% correct dependencies.

**Typical scores on English UD:**

| Model | UAS | LAS |
|-------|-----|-----|
| spaCy en_core_web_lg | ~93 | ~91 |
| Biaffine parser | ~96 | ~94 |
| Transformer-based | ~97 | ~95 |

### Language Model Evaluation: Perplexity

Perplexity measures how well a model predicts held-out text:

```
PPL = 2^(-1/n * sum log2 P(wi | context))
```

Lower is better. Lower bound: 1. Upper bound: vocabulary size.

**Perplexity across model types (WikiText-2):**

| Model | PPL |
|-------|-----|
| Uniform (50k vocab) | 50,000 |
| Unigram | ~962 |
| Bigram (Kneser-Ney) | ~139 |
| LSTM (2 layers) | ~78 |
| GPT-2 (small) | ~35 |
| GPT-3 (175B) | ~12 |
| LLaMA 3 (70B) | ~6 |

**Limitations:**

```
Perplexity is domain-dependent:
  Wikipedia-trained model: PPL=20 on Wikipedia, PPL=200 on Twitter.

Perplexity does NOT measure:
  Factual accuracy, semantic coherence,
  instruction following, or safety.

A model that memorizes training data achieves low PPL
without generalization.
```

### Language Model Evaluation: Downstream Tasks

**Benchmark suites:**

| Benchmark | Skills | Risk |
|-----------|--------|------|
| GLUE/SuperGLUE | NLU | Data contamination |
| BLiMP | Syntax | Format overfitting |
| SQuAD | QA | Training/test overlap |
| MMLU | Knowledge | Benchmark saturation |
| HellaSwag | Commonsense | Metric hacking |
| TruthfulQA | Factuality | Single reference issue |

**Evaluation risks:**

```
Data contamination: Test data in training set.
  (BERT's SQuAD training overlapped with Wikipedia pre-training.)

Benchmark saturation: Human-level on SuperGLUE,
  but humans outperform on real-world tasks.

Metric hacking: Models optimize for score, not capability.
  Example: few-shot selection optimized for MMLU improves ~5%.
```

**Emergent approaches:** Log-probability on minimal pairs (BLiMP), LLM-as-judge (MT-Bench, AlpacaEval), behavioral testing (CheckList).

### Human Evaluation

**Adequacy and fluency scales (1-5):**

```
Adequacy: How much of the source meaning is preserved?
Fluency:  How natural does the output read?
```

**Likert (1-5):** "The response is helpful." [1]..[5]

**Pairwise comparison (A/B):**

```
Which is better?
  [ ] A much better  [ ] A better  [ ] Same  [ ] B better  [ ] B much better
```

| Method | Cost | Reliability |
|--------|------|-------------|
| Likert | Low | Moderate |
| Pairwise | Medium | High |
| Best-worst | Medium | Very high |

**Target:** Krippendorff's alpha > 0.67 (tentative), > 0.80 (reliable).

### Statistical Significance

Null hypothesis: both systems perform equally. p-value: probability of difference under null. Threshold: p < 0.05.

```
System A: BLEU=45.0
System B: BLEU=45.3

Difference: 0.3 BLEU. Is it significant?
Depends on test set size, variance, and error correlation.
```

### Bootstrap and Approximate Randomization

**Bootstrap resampling:**

```python
import numpy as np

def bootstrap_ci(scores_a, scores_b, n_iter=10000):
    diffs = []
    for _ in range(n_iter):
        idx = np.random.choice(len(scores_a), len(scores_a), replace=True)
        diffs.append(np.mean(scores_a[idx]) - np.mean(scores_b[idx]))
    return np.percentile(diffs, [2.5, 97.5])

# CI contains 0 -> not significant
```

**Approximate randomization:** Pool outputs, shuffle labels, compute difference on shuffled data. p-value = proportion of shuffled diffs >= observed.

**Sample sizes needed for BLEU significance:**

| Diff | Sentences needed |
|------|------------------|
| 1.0 | ~500 |
| 0.5 | ~2,000 |
| 0.1 | ~50,000 |

### Evaluation Pitfalls

**1. Data leakage:** Test data in training set. Common crawl dedup catches some but character-level overlap persists.

**2. Metric hacking:** BLEU optimization produces dull translations. MMLU few-shot selection optimization inflates scores ~5%.

**3. Multiple comparisons:** Evaluating 20 benchmarks at p<0.05 expects 1 false positive. Use Bonferroni: threshold = 0.05 / N.

**4. Test set reuse:** Repeated evaluation on same test set inflates performance. It becomes a second dev set.

**5. Metric disagreement:**

```
System A: BLEU=45.2, chrF=62.1
System B: BLEU=44.8, chrF=63.4

Report multiple metrics.
```

**6. Domain mismatch:** News-evaluated systems may fail on medical or legal text.

**7. Annotation quality:** Penn Treebank has ~3% POS errors — sets a ceiling on accuracy.

**8. Non-determinism:** GPU nondeterminism and random seeds affect results. Report mean across multiple runs:

```python
results = [run_system(seed=i) for i in range(5)]
mean = np.mean(results)
ci = 1.96 * np.std(results) / np.sqrt(5)
print(f"{mean:.2f} +/- {ci:.2f}")
```

## Study Cases

### Case 1: Choosing a Metric for Summarization

```
Reference: "The company reported a 15% increase in revenue
            for Q3, driven by strong sales in Asia."

Sys A: "Revenue increased 15% in Q3 due to strong Asia sales."
Sys B: "Company's revenue in Q3 was up 15% due to Asia sales."
```

| Metric | Sys A | Sys B |
|--------|-------|-------|
| ROUGE-1 | 0.78 | 0.76 |
| ROUGE-2 | 0.52 | 0.50 |
| ROUGE-L | 0.71 | 0.68 |

Sys A scores higher. Human eval confirms Sys A is more concise. ROUGE-1 captures the difference: "increased" matches "increase" vs "was up".

### Case 2: Significance Testing a BLEU Improvement

New system claims BLEU=35.2 vs baseline BLEU=34.8. Test set: 2000 sentences.

```python
observed_diff = 0.4
# Bootstrap CI: [-0.05, 0.85]
# CI contains 0 -> not significant at p<0.05
```

Need ~3000+ sentences for significance with a 0.4 BLEU difference.

### Case 3: Evaluating LLM for Customer Support

```
Intrinsic metrics (low validity):
  Perplexity: 5.2 vs 4.8 (no correlation with CSAT)
  MMLU: 85.3% vs 86.1% (not banking-specific)

Task-specific:
  Intent accuracy: 94% vs 95%
  Hallucination rate: 2.1% vs 1.8%

Extrinsic:
  CSAT: 4.2 vs 4.3 (not significant)
  Handle time: 4.1 vs 3.9 min (significant, p=0.03)
```

The metric that mattered was handle time, discovered only through extrinsic eval. Intrinsic metrics were misleading.

## Examples

### Example 1: Confusion Matrix

```python
from sklearn.metrics import confusion_matrix

y_true = ["spam", "ham", "ham", "spam", "ham"]
y_pred = ["spam", "spam", "ham", "ham", "ham"]
tn, fp, fn, tp = confusion_matrix(y_true, y_pred, labels=["ham", "spam"]).ravel()
print(f"TP={tp}, FP={fp}, FN={fn}, TN={tn}")
```

### Example 2: Precision, Recall, F1

```python
from sklearn.metrics import precision_score, recall_score, f1_score

y_true = [1, 0, 1, 1, 0, 1, 0, 0]
y_pred = [1, 0, 0, 1, 0, 1, 1, 0]
print(f"P={precision_score(y_true, y_pred):.3f}, "
      f"R={recall_score(y_true, y_pred):.3f}, "
      f"F1={f1_score(y_true, y_pred):.3f}")
```

### Example 3: BLEU with SacreBLEU

```python
import sacrebleu

refs = [["the cat is on the mat"]]
cands = ["the cat sat on the mat"]

bleu = sacrebleu.corpus_bleu(cands, refs)
chrf = sacrebleu.corpus_chrf(cands, refs)
print(f"BLEU={bleu.score:.1f}, chrF={chrf.score:.1f}")
```

### Example 4: Perplexity

```python
import math

def perplexity(log_probs):
    avg_log_prob = sum(log_probs) / len(log_probs)
    return math.exp(-avg_log_prob)

ppl = perplexity([-1.2, -0.8, -2.1, -1.5, -0.5, -1.0])
print(f"PPL={ppl:.2f}")
```

### Example 5: Macro vs Micro F1

```python
from sklearn.metrics import f1_score

y_true = ["PER", "PER", "O", "O", "ORG", "O", "LOC"]
y_pred = ["PER", "O", "O", "O", "ORG", "ORG", "LOC"]

print(f"Micro F1={f1_score(y_true, y_pred, average='micro'):.3f}")
print(f"Macro F1={f1_score(y_true, y_pred, average='macro'):.3f}")
```

### Example 6: Bootstrap CI

```python
import numpy as np

scores_a = np.random.normal(0.85, 0.1, 500)
scores_b = np.random.normal(0.87, 0.1, 500)

diffs = [np.mean(scores_b[np.random.choice(500, 500, replace=True)]) -
         np.mean(scores_a[np.random.choice(500, 500, replace=True)])
         for _ in range(10000)]

ci = np.percentile(diffs, [2.5, 97.5])
print(f"95% CI: [{ci[0]:.4f}, {ci[1]:.4f}]")
```

## Glossary

| Term | Definition |
|------|------------|
| Accuracy | (TP + TN) / (TP + TN + FP + FN) |
| BLEU | N-gram precision for MT evaluation |
| Bootstrap | Resampling method for confidence intervals |
| chrF | Character n-gram F-score |
| Confusion matrix | Table of TP, FP, FN, TN |
| Data leakage | Test data contaminating training |
| F1 | Harmonic mean of precision and recall |
| Extrinsic eval | Measuring downstream task impact |
| Intrinsic eval | Measuring on dedicated test set |
| LAS | Labeled Attachment Score (parsing) |
| Macro F1 | Unweighted mean F1 across classes |
| METEOR | Metric with stem/synonym matching |
| Micro F1 | Global TP/FP/FN across all classes |
| Perplexity | exp(average negative log-likelihood) |
| Precision | TP / (TP + FP) |
| Recall | TP / (TP + FN) |
| ROUGE | Recall-oriented metric for summarization |
| Statistical significance | Probability diff not due to chance |
| UAS | Unlabeled Attachment Score (parsing) |
| WER | Word Error Rate for speech recognition |

## Quick References

- [Jurafsky & Martin: Speech and Language Processing (Evaluation)](https://web.stanford.edu/~jurafsky/slp3/)
- [Papineni et al.: BLEU (2002)](https://www.aclweb.org/anthology/P02-1040/)
- [Lin: ROUGE (2004)](https://www.aclweb.org/anthology/W04-1013/)
- [Popovic: chrF (2015)](https://www.aclweb.org/anthology/W15-3049/)
- [Post: SacreBLEU (2018)](https://www.aclweb.org/anthology/W18-6319/)
- [Dozat & Manning: Deep Biaffine Parsing (2017)](https://arxiv.org/abs/1611.01734)
- [Ribeiro et al.: CheckList (2020)](https://aclanthology.org/2020.acl-main.442/)
- [sacrebleu Python Library](https://github.com/mjpost/sacrebleu)
- [scikit-learn Metrics](https://scikit-learn.org/stable/modules/model_evaluation.html)

## Next Steps

- [Language Models](language-models.md) — learning about the systems evaluated by these metrics
