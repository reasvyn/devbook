# Morphology & Tokenization

## Description

Morphology studies the internal structure of words — how prefixes, suffixes, and roots combine to create meaning. Tokenization is the practical task of splitting text into meaningful units. For developers working with search engines, machine translation, or LLMs, understanding morphology explains why some languages need 5x more tokens than English, and why stemming and subword tokenization are critical for cross-lingual applications.

## Prerequisites

- [Formal Languages & Grammars](formal-languages.md) — finite automata, string operations, and the formal foundations of tokenization

## Table of Contents

- [What Is Morphology?](#what-is-morphology)
- [Morphemes: Free and Bound](#morphemes-free-and-bound)
- [Inflectional vs. Derivational Morphology](#inflectional-vs-derivational-morphology)
- [Morphological Typology](#morphological-typology)
- [Stemming and Lemmatization](#stemming-and-lemmatization)
- [Finite-State Morphology](#finite-state-morphology)
- [Finite-State Transducers](#finite-state-transducers)
- [Tokenization Fundamentals](#tokenization-fundamentals)
- [Tokenization Challenges across Languages](#tokenization-challenges-across-languages)
- [Subword Tokenization: BPE](#subword-tokenization-bpe)
- [Subword Tokenization: WordPiece](#subword-tokenization-wordpiece)
- [Subword Tokenization: Unigram and SentencePiece](#subword-tokenization-unigram-and-sentencepiece)
- [Tokenization in LLMs](#tokenization-in-llms)
- [Tokenization Pitfalls](#tokenization-pitfalls)
- [Morphological Analysis for Search](#morphological-analysis-for-search)
- [Morphological Analysis for Machine Translation](#morphological-analysis-for-machine-translation)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### What Is Morphology?

Morphology studies the structure of words. Morphemes are the smallest meaning-bearing units.

```
Word:    "unbreakable"
Morphemes: un-  +  break  +  -able
Meaning:  "not"   "break"   "able to be"
```

**Why developers should care:**

| Application | Morphology relevance |
|-------------|----------------------|
| Search | Match "running" to "run" via stemming |
| Translation | Handle Turkish agglutination: `evlerinizden` = "from your houses" |
| LLMs | Subword tokenizers learn morphological patterns implicitly |
| Spell check | Generate valid inflected forms |
| Text-to-speech | Determine pronunciation from morphological structure |

### Morphemes: Free and Bound

**Free morphemes** stand alone as words: `cat`, `run`, `happy`, `book`.

**Bound morphemes** must attach to another morpheme: `un-`, `-ed`, `-s`, `-ing`, `-able`.

| Position | Name | Example |
|----------|------|---------|
| Before root | Prefix | un-do, re-make |
| After root | Suffix | help-ful, walk-ed |
| Inside root | Infix | (English: "fanbloodytastic") |
| Around root | Circumfix | German ge-sag-t |

### Inflectional vs. Derivational Morphology

**Inflection** modifies a word for grammatical features without changing its core meaning:

| Category | Example |
|----------|---------|
| Plural | cat -> cats |
| Past tense | walk -> walked |
| Present 3sg | run -> runs |
| Comparative | tall -> taller |

**Derivation** creates new words, often changing part of speech:

```
create (verb)  -> creation (noun)     [-ion: verb -> noun]
                  creative (adj)       [-ive: verb -> adj]
                  creatively (adv)     [-ly: adj -> adv]
```

| Property | Inflectional | Derivational |
|----------|--------------|--------------|
| Changes meaning | Grammatical only | Lexical change |
| Changes POS | No (usually) | Often |
| Productive | Fully | Semi-productive |
| Order | After derivation | Before inflection |

**Morphological hierarchy of "unwalkability":**

```
                 un-walk-abil-ity
                 |      |     |   |
            (deriv) (root)(deriv)(deriv)
              un-   walk  -abil  -ity
              not   walk   able   state
```

### Morphological Typology

| Type | Description | Example |
|------|-------------|---------|
| Isolating | One morpheme per word | Mandarin Chinese |
| Agglutinative | Many morphemes, clear boundaries | Turkish, Finnish |
| Fusional | Morphemes fuse multiple meanings | Russian, Arabic |
| Polysynthetic | Word = entire sentence | Inuktitut |

**Agglutinative (Turkish):**

```
ev-ler-imiz-den
house-PL-our-from
"from our houses"
```

**Implication for tokenization:** Turkish "uygarlastiramadiklarimizdanmissinizcasina" = "as if you are one of those we could not civilize" — a single word requiring ~20 English words.

### Stemming and Lemmatization

**Stemming** is a heuristic that chops affixes, producing a non-word stem.

```python
from nltk.stem import PorterStemmer

stemmer = PorterStemmer()
words = ["running", "runner", "runs", "easily"]
stems = [stemmer.stem(w) for w in words]
# ["run", "runner", "run", "easili"]
```

Porter stemmer rules step 1a: `SSES -> SS` (caresses -> caress), `IES -> I` (ponies -> poni), `S ->` (cats -> cat).

**Lemmatization** returns the dictionary form (lemma) using vocabulary and POS:

```python
import spacy

nlp = spacy.load("en_core_web_sm")
doc = nlp("She ran better yesterday")
for token in doc:
    print(f"{token.text:10} -> {token.lemma_:10}")
```

```
She        -> she
ran        -> run
better     -> well
yesterday  -> yesterday
```

| Property | Stemming | Lemmatization |
|----------|----------|---------------|
| Output | Stem (may not be word) | Lemma (real word) |
| Speed | Fast (rule-based) | Slower (needs lexicon) |
| "better" | "better" | "well" |
| "ran" | "ran" | "run" |
| Use case | Search (high recall) | Text analysis (precision) |

### Finite-State Morphology

Two-level morphology (Koskenniemi, 1983) maps between lexical (dictionary) form and surface form using finite-state rules.

```
Lexical:   cat + PL
Surface:   cats

Lexical:   walk + PAST
Surface:   walked
```

**Finite-state lexicon for English verbs:**

```
Regular verb entries:
  walk:  walk +V
  talk:  talk +V

Rules:
  +V +PAST  ->  "ed"
  +V +3SG   ->  "s"
  +V +ING   ->  "ing"
```

Irregular forms are listed explicitly: `goose+PL -> geese`, `child+PL -> children`.

### Finite-State Transducers

An FST reads a lexical form and produces a surface form — like an FSA with output on transitions.

**FST for English plural:**

```
States: q0 (initial), q2 (final)
Transitions:
  q0 --c:c--> q0  --a:a--> q0  --t:t--> q0  --+PL:s--> q2
```

**Composition of morphological FSTs:**

```
Lexicon FST  o  Morphotactic FST  o  Phonological FST  =>  Surface

cat+PL -> lexicon: cat+PL -> morphotactic: cat+s -> phonology: cats
```

**Practical libraries:**

```python
import pynini

past_regular = pynini.concat(pynini.accep("walk"), pynini.accep("ed"))
print(past_regular.string("walk+PAST"))  # "walked"
```

### Tokenization Fundamentals

Tokenization splits text into tokens — the units processed by NLP systems.

| Level | Example: "Don't panic!" | Units |
|-------|------------------------|-------|
| Character | D, o, n, ', t, _, p, a, n, i, c, ! | 12 |
| Word | ["Do", "n't", "panic", "!"] | 4 |
| Subword | ["Don", "'t", "pan", "ic", "!"] | 5 |

```python
from nltk.tokenize import word_tokenize
tokens = word_tokenize("Don't panic! He's here.")
# ["Do", "n't", "panic", "!", "He", "'s", "here", "."]
```

### Tokenization Challenges across Languages

**Chinese and Japanese — no spaces:**

```
Chinese: 今天天气真好  -> ["今天"] ["天气"] ["真好"]
Japanese: 私は学生です -> ["私"] ["は"] ["学生"] ["です"]
```

**Maximum matching for Chinese:**

```python
def forward_max_match(text, vocab, max_len=4):
    tokens = []
    i = 0
    while i < len(text):
        for j in range(min(max_len, len(text) - i), 0, -1):
            if text[i:i+j] in vocab:
                tokens.append(text[i:i+j])
                i += j
                break
        else:
            tokens.append(text[i])
            i += 1
    return tokens

text = "今天天气真好"
vocab = {"今天", "天气", "真", "真好", "好"}
# Result: ["今天", "天气", "真好"]
```

**German compounds:**

```
Lebensversicherungsgesellschaft
  "life insurance company"
  Leben + s + Versicherung + s + Gesellschaft
```

**Korean — morphological segmentation required due to spacing inconsistency:**

```
나는학교에간다  ->  나 + 는 + 학교 + 에 + 가 + ㄴ다
"I go to school"
```

### Subword Tokenization: BPE

Byte-Pair Encoding (Sennrich et al., 2016) iteratively merges the most frequent character pairs.

**Algorithm:**

```
1. Initialize vocabulary with all characters
2. Count adjacent character pair frequencies
3. Merge the most frequent pair into a new symbol
4. Repeat until vocabulary reaches target size

Training: "low low low low lower lowest"

Initial chars: {l, o, w, e, r, s, t, _}
Pairs: lo(6), ow(6), we(1), er(2), es(1), st(1)

Merge 1: lo -> lo
Merge 2: ow -> low
Merge 3: er -> er
Merge 4: low+er -> lower
Merge 5: low+est -> lowest
```

```python
from tokenizers import Tokenizer, models, trainers

tokenizer = Tokenizer(models.BPE())
trainer = trainers.BpeTrainer(vocab_size=1000)
tokenizer.train_from_iterator(
    ["low low low lower lowest"],
    trainer=trainer
)
```

### Subword Tokenization: WordPiece

WordPiece (used in BERT) merges pairs by likelihood increase, not raw frequency:

```
score(x, y) = P(xy) / (P(x) * P(y))

If xy occurs together more often than expected by chance, merge it.
```

```python
from transformers import BertTokenizer

tokenizer = BertTokenizer.from_pretrained("bert-base-uncased")
tokens = tokenizer.tokenize("unbreakable")
# ["un", "##brea", "##kable"]  ("##" = continuation)
```

| Aspect | BPE | WordPiece |
|--------|-----|-----------|
| Merge criterion | Frequency | Likelihood ratio |
| Prefix | None | "##" for word-internal |
| Typical size | 16k-50k | 30k |
| Used by | GPT, RoBERTa | BERT, DistilBERT |

### Subword Tokenization: Unigram and SentencePiece

**Unigram (Kudo, 2018):** Starts with a large vocabulary, iteratively removes tokens that least reduce corpus likelihood. Encoding uses Viterbi to find the most probable segmentation:

```
argmax_{seg} sum_{t in seg} log P(t)
```

**SentencePiece (Kudo & Richardson, 2018):** Language-agnostic — no pre-tokenization. Treats input as raw bytes, applies BPE or Unigram.

```python
import sentencepiece as spm

spm.SentencePieceTrainer.train(
    input="corpus.txt",
    model_prefix="m",
    vocab_size=8000,
    model_type="bpe"
)

sp = spm.SentencePieceProcessor(model_file="m.model")
pieces = sp.encode("Hello, world!", out_type=str)
# ['▁Hello', ',', '▁world', '!']  (▁ marks space)
```

| Property | SentencePiece |
|----------|---------------|
| Pre-tokenization | None (raw bytes) |
| Normalization | NFKC Unicode |
| Reversible | Spaces encoded as tokens |
| Byte fallback | Unknown chars -> UTF-8 bytes |
| Used by | T5, mBART, LLaMA 3 |

### Tokenization in LLMs

**GPT-2/3/4:** BPE on bytes, ~100k vocabulary.

```python
from transformers import GPT2Tokenizer

tokenizer = GPT2Tokenizer.from_pretrained("gpt2")
tokenizer.tokenize("Hello, world!")
# ["Hello", ",", " world", "!"]
```

**LLaMA 3:** SentencePiece BPE, 128k vocabulary, digit splitting.

**Case sensitivity matters:**

```
Word          Token IDs (GPT-4)
"hello"       [15339]
"Hello"       [22691]
" HELLO"      [14930]
```

First letter case changes the token ID entirely, affecting probability calculations.

**Multilingual token ratio:**

| Language | Tokens per word (GPT-4 style) |
|----------|-------------------------------|
| English | ~1.3 |
| Turkish | ~3.5 |
| Japanese | ~2.5 characters per token |
| Chinese | ~2.0 characters per token |

A 4096-token context holds ~3150 English words but only ~1170 Turkish words.

### Tokenization Pitfalls

**1. False compositionality:**

```python
tokenizer.tokenize("butterfly")  # ["butter", "fly"] — not semantically "butter" + "fly"
tokenizer.tokenize("elsewhere")  # ["else", "where"]
```

**2. Number tokenization:**

GPT-2 treats `42` as `["4", "2"]` but `142` as `["1", "42"]` — inconsistent digit grouping causes arithmetic failures.

**3. Spelling sensitivity:**

```python
tokenizer.tokenize("don't")  # ["don", "'", "t"]
tokenizer.tokenize("dont")   # ["dont"] — model may not know "dont"
```

**4. Whitespace artifacts:**

```python
tokenizer.tokenize("  hello")  # ["  ", "hello"]
tokenizer.tokenize("hello ")  # ["hello", " "]
```

**5. Special tokens and prompt injection:**

LLM tokenizers reserve special tokens (`<|begin_of_text|>`, `<|system|>`, `<|user|>`). Crafted inputs using these token IDs can confuse role assignment.

### Morphological Analysis for Search

Search engines use stemming or lemmatization to match queries despite surface differences.

**Elasticsearch stemmer configuration:**

```json
{
  "settings": {
    "analysis": {
      "filter": {
        "english_stemmer": { "type": "stemmer", "language": "english" }
      },
      "analyzer": {
        "stemmed_analyzer": {
          "tokenizer": "standard",
          "filter": ["lowercase", "english_stemmer"]
        }
      }
    }
  }
}
```

**German decompounding** splits compounds before indexing:

```
"Donaudampfschifffahrtsgesellschaft"
  -> ["Donau", "dampf", "schifffahrt", "gesellschaft"]
```

**Search for morphologically rich languages:**

| Language | Approach |
|----------|----------|
| Turkish | Stem before indexing, store root forms |
| Arabic | Root-based stemming (triconsonantal roots) |
| Chinese | Segment words, index phrases |
| Japanese | MeCab morphological analyzer |

### Morphological Analysis for Machine Translation

**Statistical MT:** Word-by-word translation fails due to morphological agreement.

```
English: "The cat chased the mice."
German:  "Die Katze jagte die Mäuse."

cat -> Katze (fem, nom, sg)
chased -> jagte (past, 3sg)
mice -> Mause (fem, acc, pl)
```

**Neural MT:** Subword tokenization handles most morphology implicitly:

```
BPE: "The cat chase-d the mice."
     ["The", "cat", "chase", "d", "the", "mice"]
```

Transformers learn cross-lingual morphological patterns from parallel data. A Turkish translator learns that verbs go at the end and person is marked on the verb.

**Factored models** incorporate lemmas, POS tags, and morphological features as additional inputs, helpful for low-resource languages.

## Study Cases

### Case 1: Building a Search Stemmer for Turkish

Problem: Turkish inflections prevent search matching.

```python
from snowballstemmer import TurkishStemmer

stemmer = TurkishStemmer()
tokens = ["evlerinizden", "okullarimizda", "kitaplarimizi"]

for t in tokens:
    print(f"{t:20} -> {stemmer.stemWord(t)}")
```

Output:
```
evlerinizden         -> ev
okullarimizda        -> okul
kitaplarimizi        -> kitap
```

All roots found correctly. "ev" over-stems "evler" (houses) but this is acceptable for search recall.

### Case 2: BPE for a Multilingual Application

An app needs to support English, Turkish, and Japanese with one tokenizer.

```python
from tokenizers import Tokenizer, models, trainers

tokenizer = Tokenizer(models.BPE())
trainer = trainers.BpeTrainer(vocab_size=32000)
tokenizer.train(["english.txt", "turkish.txt", "japanese.txt"], trainer)
```

Results: ~60% vocab to English, 25% Turkish, 15% Japanese. Turkish averages 3.2 tokens/word, Japanese 2.5 chars/token, English 1.3 tokens/word.

### Case 3: Tokenizer Bias Analysis

A developer compares token counts for equivalent sentences in different languages:

```python
sentences = {
    "en": "I want to learn about this topic in detail.",
    "tr": "Bu konu hakkinda detayli bilgi edinmek istiyorum.",
    "ja": "Kono topikku nitsuite kuwashiku manabitai.",
}

for lang, text in sentences.items():
    tokens = len(tokenizer.encode(text))
    print(f"{lang}: {tokens} tokens")
```

Output:
```
en: 11 tokens
tr: 18 tokens (64% more)
ja: 22 tokens (100% more)
```

Solution: language-specific tokenization or larger context allocation for non-English.

## Examples

### Example 1: Porter Stemmer Step-by-Step

Word: "connecting"

```
Step 1a: No S/SSES/IES/SS rule -> "connecting"
Step 1b: Ends with "ing", vowel before -> "connect"
Step 2:  No applicable rules -> "connect"
```

### Example 2: FST for English Plural

```
Lexicon:  cat+PL -> cat+s
          box+PL -> box+es (sibilant rule)
Phonology: cat+s -> cats
           box+es -> boxes
```

### Example 3: BPE Merge Sequence

```
Training: "aa aa ab"
Initial vocab: a, b, _
Pairs: aa(4), a_(2), _a(1), ab(1)
Merge 1: aa -> X
Data becomes: X X ab
```

### Example 4: Unigram Segmentation Probability

```
Corpus: "ab"(5x), "a"(3x), "b"(2x)
Tokens: {a:0.3, b:0.2, ab:0.5}

"aba":
  a+b+a = 0.018
  ab+a  = 0.15  (higher — Viterbi picks this)
```

### Example 5: SentencePiece Training

```python
spm.SentencePieceTrainer.train(
    input="data.txt",
    model_prefix="sp",
    vocab_size=8000,
    model_type="unigram",
)

sp = spm.SentencePieceProcessor(model_file="sp.model")
print(sp.encode("Hello world", out_type=str))
```

## Glossary

| Term | Definition |
|------|------------|
| Affix | A bound morpheme attached to a root |
| Agglutinative | Language with many morphemes, clear boundaries |
| BPE | Byte-Pair Encoding — subword tokenization by merging frequent char pairs |
| Bound morpheme | Morpheme that cannot stand alone |
| Derivation | Creating new words from existing ones |
| Free morpheme | Morpheme that can stand alone as a word |
| FST | Finite-State Transducer — maps between lexical and surface forms |
| Inflection | Grammatical modification of a word |
| Isolating | Language with ~one morpheme per word |
| Lemma | Dictionary form of a word |
| Lemmatization | Finding the lemma using vocabulary and POS |
| Maximum matching | Dictionary-based Chinese segmentation by longest match |
| Morpheme | Smallest meaning-bearing unit |
| Morphology | Study of word structure |
| SentencePiece | Language-agnostic subword tokenizer |
| Stem | Base unit after affix removal |
| Stemming | Heuristic removal of affixes |
| Subword tokenization | Splitting into units between characters and words |
| Tokenization | Splitting text into processing units |
| Unigram | Probabilistic subword tokenization |
| WordPiece | Subword tokenization by likelihood ratio |

## Quick References

- [Jurafsky & Martin: Speech and Language Processing (Tokenization chapter)](https://web.stanford.edu/~jurafsky/slp3/)
- [Koskenniemi: Two-Level Morphology (1983)](https://www.aclweb.org/anthology/C84-1002/)
- [Sennrich et al.: Neural Machine Translation of Rare Words with Subword Units (2016)](https://arxiv.org/abs/1508.07909)
- [Kudo: Subword Regularization (2018)](https://arxiv.org/abs/1804.10959)
- [Kudo & Richardson: SentencePiece (2018)](https://arxiv.org/abs/1808.06226)
- [Porter: An Algorithm for Suffix Stripping (1980)](https://www.cs.toronto.edu/~frank/csc2501/Readings/R2_Porter/Porter-1980.pdf)
- [Hugging Face Tokenizers Library](https://huggingface.co/docs/tokenizers/)
- [Pynini: Finite-State Grammar Compilation](https://pynini.readthedocs.io/)

## Next Steps

- [Parsing](parsing.md) — how token sequences are analyzed into syntactic structures
- [Language Models](language-models.md) — how subword tokenization and morphology feed into language model architectures
