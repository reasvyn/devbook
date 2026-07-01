# Corpora & Data Annotation

## Description

A corpus is a large, structured collection of text used to train and evaluate language models. Annotation adds human-labeled information — parts of speech, named entities, syntactic structures — that teaches models to understand language. For developers building NLP systems, understanding corpora and annotation is essential for training data quality, model evaluation, and avoiding the biases that lead to production failures.

## Prerequisites

- [Formal Languages & Grammars](formal-languages.md) — the formal structure of language that annotation schemas describe

## Table of Contents

- [What Is a Corpus?](#what-is-a-corpus)
- [Corpus Types](#corpus-types)
- [Key Corpora](#key-corpora)
- [Corpus Size and Balance](#corpus-size-and-balance)
- [Annotation Schemas: POS Tagging](#annotation-schemas-pos-tagging)
- [Annotation Schemas: Named Entity Recognition](#annotation-schemas-named-entity-recognition)
- [Annotation Schemas: Dependency Parsing](#annotation-schemas-dependency-parsing)
- [Annotation Schemas: Discourse](#annotation-schemas-discourse)
- [Inter-Annotator Agreement](#inter-annotator-agreement)
- [Cohen's Kappa](#cohens-kappa)
- [Krippendorff's Alpha](#krippendorffs-alpha)
- [Annotation Tools](#annotation-tools)
- [Data Splits: Train / Dev / Test](#data-splits-train-dev-test)
- [Bias in Corpora](#bias-in-corpora)
- [Legal and Ethical Concerns](#legal-and-ethical-concerns)
- [Creating Your Own Dataset](#creating-your-own-dataset)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### What Is a Corpus?

A corpus (plural: corpora) is a structured collection of language data used for linguistic analysis or NLP model training.

```
Corpus = Collection of texts + Metadata + (Optional) Annotations
```

**Why corpora matter for developers:**

| Purpose | Why corpora are essential |
|---------|---------------------------|
| Language models | Training data determines model behavior |
| Spell checkers | Corpus frequency data guides corrections |
| Search engines | Corpus statistics inform ranking algorithms |
| Sentiment analysis | Labeled corpora train classifiers |
| Machine translation | Parallel corpora provide translation pairs |

### Corpus Types

| Type | Description | Example |
|------|-------------|---------|
| General | Broad representation | British National Corpus (100M words) |
| Specialized | Domain-specific | PubMed abstracts (biomedical) |
| Monitor | Continuously updated | COCA (1990-present) |
| Reference | Static, balanced snapshot | Brown Corpus (1961, 1M words) |
| Parallel | Texts in multiple languages | Europarl (21 EU languages) |
| Learner | Language learner errors | Cambridge Learner Corpus |

**Written vs. spoken:** Written corpora are larger and cheaper to collect. Spoken corpora include prosody and pauses but require expensive transcription.

### Key Corpora

**English corpora:**

| Corpus | Size | Content |
|--------|------|---------|
| BNC (British National Corpus) | 100M words | British English, 1990s |
| COCA | 1B+ words | American English, 1990-present |
| Brown Corpus | 1M words | American English, 1961 (pioneering) |
| OANC | 15M words | American English, open license |
| enTenTen20 | 36B words | Web-crawled English |

**Multilingual and parallel:**

| Corpus | Languages | Size |
|--------|-----------|------|
| Europarl | 21 EU languages | ~60M per lang |
| UN Parallel Corpus | 6 UN languages | ~300M per lang |
| OPUS | 100+ languages | Varies |

**Web-scale corpora used in LLM training:**

| Dataset | Used by | Size |
|---------|---------|------|
| C4 | T5 | 750 GB cleaned web text |
| The Pile | GPT-2/3 era | 825 GB (22 datasets) |
| RefinedWeb | Falcon | 5 TB filtered web |
| FineWeb-Edu | FineWeb-Edu | 15T tokens |

**Domain-specific:**

| Domain | Corpus | Size |
|--------|--------|------|
| Biomedical | PubMed Central | ~5M articles |
| Legal | PILE of Law | ~250 GB |
| Code | GitHub Code | ~50M repos |
| Social media | Reddit | ~3B comments |

### Corpus Size and Balance

**Size recommendations:**

| Task | Recommended size |
|------|------------------|
| Word frequency | 100M+ |
| POS tagging | 500k+ (labeled) |
| Language modeling | 1B+ tokens |
| Machine translation | 10M+ sentence pairs |
| LLM pre-training | 1T+ tokens |

**Balance principles:**

A balanced corpus represents the full range of language use. Brown Corpus defined 15 genre categories with proportional sampling.

```
Balanced proportions: 30% fiction, 25% news, 20% academic, 15% web, 10% spoken
```

**Zipf's law:**

In any corpus, rank-ordered word frequency follows a power law: the top ~135 words account for half of all tokens. This means rare words dominate the vocabulary even in large corpora.

### Annotation Schemas: POS Tagging

POS tagging labels each word with a grammatical category.

**Penn Treebank tagset (45 tags):**

| Tag | Category | Examples |
|-----|----------|----------|
| CC | Coord conjunction | and, but, or |
| DT | Determiner | the, a, this |
| JJ | Adjective | quick, lazy |
| NN | Noun, singular | cat, dog |
| VB | Verb, base form | run, eat |
| IN | Preposition | in, on, at |

```python
import spacy

nlp = spacy.load("en_core_web_sm")
doc = nlp("The quick brown fox jumps over the lazy dog.")

for token in doc:
    print(f"{token.text:6} {token.pos_:4} {token.tag_:4}")
```

```
The    DET  DT
quick  ADJ  JJ
fox    NOUN NN
jumps  VERB VBZ
```

**Universal Dependencies (UD):** Cross-lingual tagset with 17 tags (ADJ, ADP, ADV, NOUN, VERB, etc.), used across 100+ languages.

### Annotation Schemas: Named Entity Recognition

NER identifies named entities and classifies them.

**CoNLL-2003 types:** PER (person), ORG (organization), LOC (location), GPE (geo-political), MISC.

**BIO tagging scheme:**

```
Tokens:  Marie  Curie  discovered  radium  in  Paris  .
Labels:  B-PER  I-PER  O           O       O   B-LOC  O
```

B = Beginning, I = Inside, O = Outside.

```python
nlp = spacy.load("en_core_web_sm")
doc = nlp("Ada Lovelace worked at MIT in Cambridge.")

for ent in doc.ents:
    print(f"{ent.text:20} {ent.label_}")
```

```
Ada Lovelace          PERSON
MIT                   ORG
Cambridge             GPE
```

**Fine-grained (OntoNotes 5.0):** 18 types including PERSON, ORG, GPE, DATE, TIME, MONEY, PERCENT, QUANTITY.

### Annotation Schemas: Dependency Parsing

Dependency parsing labels binary grammatical relations between words.

**Universal Dependencies relations:**

| Relation | Description | Example |
|----------|-------------|---------|
| nsubj | Nominal subject | "fox" -> nsubj -> "jumps" |
| dobj | Direct object | "cat" -> dobj -> "chased" |
| amod | Adjectival modifier | "quick" -> amod -> "fox" |
| prep | Prepositional modifier | "over" -> prep -> "jumps" |
| det | Determiner | "the" -> det -> "dog" |

**Dependency tree for "The quick brown fox jumps over the lazy dog":**

```
jumps (ROOT)
  nsubj -> fox -> det -> The
                -> amod -> quick, brown
  prep  -> over -> pobj -> dog -> det -> the
                                -> amod -> lazy
```

**CoNLL-U format:**

```
1  The     the     DET   DT   4  det   _
2  quick   quick   ADJ   JJ   4  amod  _
3  brown   brown   ADJ   JJ   4  amod  _
4  fox     fox     NOUN  NN   5  nsubj _
5  jumps   jump    VERB  VBZ  0  root  _
6  over    over    ADP   IN   5  prep  _
7  the     the     DET   DT   9  det   _
8  lazy    lazy    ADJ   JJ   9  amod  _
9  dog     dog     NOUN  NN   6  pobj  _
10 .       .       PUNCT .    5  punct _
```

### Annotation Schemas: Discourse

Discourse annotation captures relationships between sentences.

**Penn Discourse Tree Bank (PDTB) relations:**

| Relation | Example |
|----------|---------|
| Contrast | "I like coffee, **but** tea is better." |
| Cause | "**Because** it rained, the match was canceled." |
| Temporal | "**After** eating, we left." |
| Expansion | "**Moreover**, the results were consistent." |

**Coreference annotation** links mentions of the same entity:

```
"[Ada]₁ was a mathematician. [She]₁ wrote the first algorithm.
[Her]₁ notes were published posthumously."
```

### Inter-Annotator Agreement

Multiple annotators label the same data to assess reliability.

**Percentage agreement:**

```
Annotator A: PER PER O   O   B-ORG O   O   B-GPE
Annotator B: PER PER O   B-ORG ORG  O   B-LOC GPE
                                    ^^^ discrepancies
Agreement: 6/8 = 75%
```

Problem: Chance agreement inflates this. If both always guess "O", agreement >90% with no real annotation.

### Cohen's Kappa

Cohen's kappa corrects for chance agreement between two annotators:

```
kappa = (P_o - P_e) / (1 - P_e)

P_o = observed agreement
P_e = expected agreement by chance
```

**Interpretation:**

| kappa | Agreement |
|-------|-----------|
| < 0.00 | Poor |
| 0.00-0.20 | Slight |
| 0.21-0.40 | Fair |
| 0.41-0.60 | Moderate |
| 0.61-0.80 | Substantial |
| 0.81-1.00 | Almost perfect |

**Example computation:**

Two annotators on 100 items, 3 categories {A, B, C}:

```
          B: A   B   C   Total
A: A      40   5   5   50
   B      10  20   5   35
   C       5   5   5   15
Total     55  30  15   100

P_o = (40+20+5)/100 = 0.65
P_e = (50/100*55/100) + (35/100*30/100) + (15/100*15/100)
    = 0.275 + 0.105 + 0.0225 = 0.4025
kappa = (0.65-0.4025)/(1-0.4025) = 0.414 (moderate)
```

**Limitations:** Sensitive to prevalence (rare categories get inflated kappa). Treats all disagreements equally.

### Krippendorff's Alpha

Generalizes to any number of annotators, any scale type, and handles missing data.

```
alpha = 1 - (D_o / D_e)

D_o = observed disagreement
D_e = expected disagreement by chance
```

| Scale | Metric | Example |
|-------|--------|---------|
| Nominal | Exact match | Entity type classification |
| Ordinal | Rank distance | Relevance scores 1-5 |
| Interval | Squared difference | Age estimation |

**When to use each:**

| Measure | Annotators | Scales | Missing data |
|---------|------------|--------|--------------|
| Cohen's kappa | 2 | Nominal | No |
| Fleiss' kappa | 2+ | Nominal | No |
| Krippendorff's alpha | 2+ | Any | Yes |

**Typical targets for NLP:**

| Task | Minimum | Good |
|------|---------|------|
| POS tagging | 0.95 | 0.97 |
| NER | 0.80 | 0.90 |
| Dependency parsing | 0.85 | 0.92 |
| Sentiment | 0.70 | 0.80 |
| Discourse | 0.65 | 0.75 |

### Annotation Tools

**BRAT (Brat Rapid Annotation Tool):** Web-based, supports NER, relations, events, coreference. Output: standoff format (.ann + .txt).

**Prodigy (by spaCy):** Active learning loop — model predicts, user corrects, model retrains on hardest cases first.

**Doccano:** Open-source web-based tool for text classification, sequence labeling, seq2seq. Exports JSONL, CoNLL, spaCy.

| Tool | Best for | Cost |
|------|----------|------|
| Label Studio | Multi-modal (text, image, audio) | Open source |
| Inception | Complex annotation (coreference, discourse) | Open source |
| Prodigy | Active learning, custom pipelines | Paid |
| Scale AI | Enterprise API for annotation | Paid |
| UBIA | Text classification, NER | Paid |

### Data Splits: Train / Dev / Test

| Split | Purpose | Typical size |
|-------|---------|--------------|
| Training | Model learns parameters | 80% |
| Dev | Hyperparameter tuning | 10% |
| Test | Final evaluation (used once) | 10% |

**Why three sets:** Training error always decreases. Dev prevents overfitting to train. Test prevents overfitting to dev.

**Stratified splitting** preserves class proportions:

```python
from sklearn.model_selection import train_test_split

X_train, X_temp, y_train, y_temp = train_test_split(
    X, y, test_size=0.2, stratify=y, random_state=42
)
X_dev, X_test, y_dev, y_test = train_test_split(
    X_temp, y_temp, test_size=0.5, stratify=y_temp, random_state=42
)
```

**Cross-validation for small data:** k-fold for <10k, leave-one-out for <100 examples.

**Contamination:** Test set should be used at most once before publication. Many published results are inflated by repeated test set access.

### Bias in Corpora

**Selection bias:** Common Crawl is ~60% English, mostly Western, formal register. Under-represents spoken language, dialects, non-Western cultures.

**Annotation bias:** Annotators bring personal perspectives. "The service was acceptable" could be positive (relaxed annotator) or neutral (demanding one).

**Historical bias:** Corpora encode societal stereotypes.

```python
# Measuring word embedding bias:
def bias_score(embeddings, target, attr1, attr2):
    return cosine(embeddings[target], embeddings[attr1]) - \
           cosine(embeddings[target], embeddings[attr2])

# "doctor" more associated with "man" than "woman"
# "nurse" more associated with "woman" than "man"
```

**Mitigation strategies:**

| Strategy | Example |
|----------|---------|
| Balanced sampling | Equal dialect representation |
| Debiasing algorithms | Hard debiasing (Bolukbasi et al.) |
| Counterfactual augmentation | Swap gendered terms |
| Post-hoc evaluation | Disparate impact analysis |

**The multilinguality curse:** Models trained on 80% English perform poorly on Swahili. Every added language dilutes capacity for others.

### Legal and Ethical Concerns

**Copyright and TDM:**

```
Training on copyrighted text:
  EU: Text and Data Mining exception
  US: Fair use (contested, ongoing litigation)
  Japan: Explicitly allows for AI training
  Pending: NYT v. OpenAI, Getty v. Stability AI
```

**PII redaction:**

```python
from presidio_analyzer import AnalyzerEngine

analyzer = AnalyzerEngine()
text = "Contact John Doe at 555-1234 or john@example.com."

results = analyzer.analyze(text=text, language="en")
for r in results:
    print(f"Found {r.entity_type} at {r.start}-{r.end}")
```

**Informed consent:** Public web data used for training raises questions. Users did not consent to AI training use. Even public data can contain private conversations.

**Data licensing:**

```
Open (CC-BY, CC0):  Freely usable, cite source
Research-only:       Cannot use commercially
Proprietary:         Licensed from vendors
```

### Creating Your Own Dataset

**Pipeline:**

```
1. Define task and annotation schema
2. Collect raw data (sampling strategy)
3. Write annotation guidelines (detailed, with examples)
4. Pilot annotation (10-100 items, measure agreement)
5. Refine guidelines based on disagreements
6. Full annotation (distribute to annotators)
7. Quality checks (spot-checks, agreement monitoring)
8. Adjudication (resolve disagreements)
9. Release / versioning
```

**Sample guideline excerpt (NER):**

```
PERSON: Named individual.
  Include: full names, first name, last name
  Exclude: brand mascots, deity names
  Edge: "President Biden" -> PERSON (not ORG)
        "the President" -> O (generic)
```

**Minimum viable annotation script:**

```python
import json

class Annotator:
    def __init__(self):
        self.annotations = []

    def add(self, text_id, spans, labels):
        self.annotations.append({
            "text_id": text_id,
            "spans": spans,
            "labels": labels
        })

    def export(self, path):
        with open(path, "w") as f:
            for ann in self.annotations:
                f.write(json.dumps(ann) + "\n")
```

## Glossary

| Term | Definition |
|------|------------|
| B-/I-/O- | BIO tagging scheme for entity labeling |
| Cohen's kappa | Chance-corrected agreement for two raters |
| CoNLL-U | Standard format for dependency-annotated corpora |
| Corpus | Structured collection of language data |
| Dev set | Development/validation set for tuning |
| Discourse | Units of language beyond a single sentence |
| Fleiss' kappa | Agreement for multiple raters (nominal) |
| Inter-annotator agreement | Consistency measure between annotators |
| Krippendorff's alpha | Agreement for any raters, any scale |
| NER | Named Entity Recognition |
| Parallel corpus | Aligned texts in multiple languages |
| PII | Personally Identifiable Information |
| POS tagging | Labeling words with parts of speech |
| Test set | Held-out data for final evaluation |
| Train set | Data for learning model parameters |
| Universal Dependencies | Cross-lingual dependency annotation framework |
| Zipf's law | Frequency inversely proportional to rank |

## Quick References

- [Jurafsky & Martin: Speech and Language Processing (Corpora)](https://web.stanford.edu/~jurafsky/slp3/)
- [BNC: British National Corpus](http://www.natcorp.ox.ac.uk/)
- [COCA: Corpus of Contemporary American English](https://www.english-corpora.org/coca/)
- [Universal Dependencies](https://universaldependencies.org/)
- [OntoNotes 5.0](https://catalog.ldc.upenn.edu/LDC2013T19)
- [Brat Rapid Annotation Tool](https://brat.nlplab.org/)
- [Doccano Open Source Annotation](https://github.com/doccano/doccano)
- [Label Studio](https://labelstud.io/)
- [Prodigy (spaCy)](https://prodi.gy/)
- [Cohen's Kappa (scikit-learn)](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.cohen_kappa_score.html)
- [Pile of Law](https://pile-of-law.org/)

## Next Steps

- [Evaluation Methods](evaluation-metrics.md) — how to measure system performance using annotated corpora
- [Language Models](language-models.md) — how language models are trained on large-scale corpora
