# Learning Pyramid and Active Learning — Intermediate

## Description

This document provides a deeper analysis of the learning pyramid myth, examining the provenance research in detail, the psychological mechanisms that sustain citation cascades in education, and the genuine evidence base for multimedia learning principles and generative learning strategies. The goal is to equip developers with the ability to distinguish evidence-based claims from unsupported assertions when evaluating learning advice.

## Prerequisites

- [Learning Pyramid and Active Learning — Basic](learning-pyramid-and-active-learning-basic.md) — Dale's Cone, the myth, and the general principle of active learning
- [Theories of Learning — Basic](theories-of-learning-basic.md) — foundational learning frameworks

## Table of Contents

- [The Provenance Analysis in Detail](#the-provenance-analysis-in-detail)
- [Citation Cascade: How Myths Survive in Academia](#citation-cascade-how-myths-survive-in-academia)
- [Mayer's Multimedia Learning Principles in Depth](#mayers-multimedia-learning-principles-in-depth)
- [Noetel et al. (2022): The Meta-Meta-Analysis](#noetel-et-al-2022-the-meta-meta-analysis)
- [The Generative Learning Framework](#the-generative-learning-framework)
- [Designing Multimodal Learning Experiences](#designing-multimodal-learning-experiences)

## The Provenance Analysis in Detail

### Masters (2013): Tracing the Numbers

Ken Masters published "Edgar Dale's Pyramid of Learning in Medical Education" in *Medical Teacher* (2013), conducting the most thorough provenance analysis of the learning pyramid. His investigation traced the claim through decades of citations:

**1946** — Dale publishes *Audio-Visual Methods in Teaching* with the Cone of Experience. The cone contains 11 levels. No percentages are attached to any level. Dale explicitly warns against overinterpreting the model.

**1954, 1960, 1969** — Dale publishes revised editions of his book. The cone remains unchanged in its essential structure. No percentages appear in any edition.

**Approximately 1970** — An unknown source superimposes retention percentages onto a simplified version of Dale's cone. The specific numbers (10/20/30/50/70/90%) appear for the first time.

**1970s-2000s** — The pyramid with percentages circulates in training materials, textbooks, and educational presentations. It is increasingly attributed to Dale and/or the National Training Laboratories (NTL).

**2013** — Masters publishes his analysis, concluding: "The Pyramid is rubbish, the statistics are rubbish, and they do not come from Edgar Dale."

### The NTL Attribution

The National Training Laboratories in Bethel, Maine, is frequently cited as the source of the retention percentages. When researchers attempted to verify this attribution:

- The NTL was unable to locate or produce any original research supporting the percentages.
- The NTL acknowledged that Dale produced "a similar pyramid with slightly different numbers" — but Dale's actual cone contains no numbers.
- No NTL publication from the relevant period contains the data that would support the claimed percentages.

### Subramony et al. (2014)

Subramony, Molenda, Betrus, and Thalheimer confirmed Masters's findings through independent analysis. They traced the superimposition of numbers onto Dale's cone to an unknown source around 1970 and documented the absence of any supporting empirical research.

## Citation Cascade: How Myths Survive in Academia

The learning pyramid is a case study in citation cascade — a phenomenon where a claim gains perceived credibility through repetition rather than evidence. The mechanism operates through several stages:

### Stage 1: Original Claim

A claim is made in a specific context with appropriate qualifications and limitations. Dale's cone was a descriptive model of experience types, explicitly presented as non-hierarchical and non-quantitative.

### Stage 2: Simplification and Decontextualization

The claim is simplified and stripped of its original qualifications. Dale's 11-level cone becomes a 6-level pyramid. The cautionary notes are removed. Qualifiers like "approximately" and "in some contexts" disappear.

### Stage 3: Attribution to Authority

The simplified claim is attributed to a respected source. The pyramid is attributed to Dale (a legitimate educational researcher) or NTL (a legitimate institution). This attribution provides credibility without verification.

### Stage 4: Textbook Inclusion

The claim appears in textbooks. Textbook authors often rely on other textbooks rather than primary sources. Once the pyramid appears in one textbook, subsequent authors cite it as established knowledge.

### Stage 5: Self-Reinforcing Citation

Each citation reinforces the perception that the claim is well-established. The volume of citations creates an illusion of independent corroboration — but all citations trace back to the same unsubstantiated source.

### Stage 6: Resistance to Correction

Even after debunking (Masters, 2013; Subramony et al., 2014), the pyramid persists because:

- It appears in established textbooks that are not updated frequently.
- It is convenient for advocates of active learning (it supports their position).
- It is visually compelling and easy to remember.
- The correction is less interesting than the original claim.

### The 43-Article Problem

Masters documented that the learning pyramid had been cited in at least 43 peer-reviewed medical education articles. In each case, the citation was presented as established fact, not as an empirical claim requiring verification. This illustrates a systemic failure in academic citation practices.

## Mayer's Multimedia Learning Principles in Depth

Richard Mayer's multimedia learning principles represent what the evidence actually supports about learning modalities — as opposed to the fabricated percentages of the learning pyramid.

### The Twelve Principles (Mayer, 2009, 2021)

| # | Principle | Statement | Evidence |
|---|-----------|-----------|----------|
| 1 | Multimedia | People learn better from words and pictures together | Strong |
| 2 | Spatial contiguity | Place text near corresponding graphics | Strong |
| 3 | Temporal contiguity | Present narration and animation simultaneously | Strong |
| 4 | Coherence | Exclude extraneous words, pictures, and sounds | Strong |
| 5 | Modality | Use narration with graphics rather than text with graphics | Strong |
| 6 | Redundancy | Do not add on-screen text to narration and graphics | Strong |
| 7 | Signaling | Highlight essential material | Moderate |
| 8 | Segmenting | Present in learner-paced segments | Moderate |
| 9 | Pre-training | Teach key concepts before the main lesson | Moderate |
| 10 | Personalization | Use conversational rather than formal style | Moderate |
| 11 | Voice | Use human voice rather than machine voice | Moderate |
| 12 | Image | No evidence that adding images of the speaker helps | Weak |

### What These Principles Replace

The multimedia principles replace the learning pyramid's false quantitative claims with genuine, evidence-based qualitative principles. They do not tell you what percentage of information is retained through different modalities. They tell you that combining words and pictures works better than words alone, and that specific design choices (contiguity, coherence, modality) improve learning outcomes.

### Application to Developer Documentation

| Principle | Documentation Application |
|-----------|--------------------------|
| Multimedia | Combine text explanations with code examples and diagrams |
| Spatial contiguity | Place parameter descriptions directly in code examples |
| Coherence | Remove decorative elements, unnecessary screenshots |
| Modality | Use voiceover in video tutorials rather than text overlays |
| Segmenting | Break long tutorials into short, focused sections |
| Pre-training | Define key terms before using them in procedures |

## Noetel et al. (2022): The Meta-Meta-Analysis

Noetel and colleagues conducted a comprehensive meta-meta-analysis published in *Review of Educational Research*, synthesizing evidence from hundreds of meta-analyses on learning techniques. Key findings:

### Techniques With Strong Evidence

- **Multimedia learning** — combining visual and verbal information consistently improves outcomes.
- **Spacing** — distributed practice outperforms massed practice across all conditions tested.
- **Retrieval practice** — testing produces better long-term retention than restudying.
- **Interleaving** — mixing problem types improves discrimination and transfer.

### Techniques With Moderate Evidence

- **Elaborative interrogation** — asking "why?" improves learning in many but not all contexts.
- **Self-explanation** — explaining to yourself during study improves outcomes.
- **Concrete examples** — connecting abstract concepts to specific instances aids understanding.

### Techniques With Weak or No Evidence

- **Learning styles matching** — no support for the claim that matching instruction to style improves outcomes.
- **Unelaborated practice testing without feedback** — testing is most effective when feedback is provided.
- **Summarization** — effective when done well, but quality varies enormously.

## The Generative Learning Framework

Fiorella and Mayer (2015) proposed the generative learning framework — a unifying account of why active learning strategies work. The framework identifies five strategies that require learners to actively generate knowledge:

### The Five Strategies

| Strategy | What It Involves | Mechanism |
|----------|------------------|-----------|
| **Summarizing** | Creating a brief representation of the material | Selecting and organizing essential information |
| **Drawing** | Creating a visual representation (diagram, map, chart) | Integrating verbal and visual information |
| **Explaining** | Generating explanations for how or why something works | Elaborative processing and self-explanation |
| **Mapping** | Creating a visual representation of relationships | Organizing information spatially |
| **Self-testing** | Attempting to retrieve information from memory | Retrieval practice and metacognitive feedback |

### The Common Mechanism

All five strategies share a common mechanism: they require the learner to *generate* a representation of the material rather than passively receiving one. Generation forces selection (what to include), organization (how to structure), and integration (how it connects to existing knowledge). These cognitive operations produce deeper encoding than passive reception.

## Designing Multimodal Learning Experiences

### For Self-Study

A multimodal study session for learning a new technology might include:

1. **Read** the documentation (textual processing).
2. **Watch** a conference talk on the topic (visual + auditory processing).
3. **Build** a small project (generative learning through drawing/building).
4. **Explain** the concept to a colleague or rubber duck (generative learning through explaining).
5. **Test yourself** on key concepts (retrieval practice).
6. **Create** a diagram of the system architecture (generative learning through mapping).

Each modality creates different encoding opportunities. The combination is more effective than any single modality.

### For Team Learning

- **Tech talks** combine presentation (verbal + visual) with Q&A (retrieval + elaboration).
- **Code reviews** require both analytical processing (evaluating code) and generative processing (articulating reasoning).
- **Pair programming** combines visual code analysis, verbal discussion, and generative problem-solving.

### For Documentation Design

Effective technical documentation should:

- Combine text with diagrams (multimedia principle).
- Place explanatory text near the code it describes (spatial contiguity).
- Remove unnecessary decorative elements (coherence).
- Use voiceover in video tutorials rather than text overlays (modality).
- Break long tutorials into focused sections (segmenting).

## Learning Tips

- When you encounter a learning claim with specific percentages, investigate the source. If the source cannot be traced to primary research, treat the claim with skepticism.
- The most important practical principle from multimedia learning research is the spatial contiguity principle: place explanations near the things they explain. This single change typically produces the largest improvement in learning outcomes.
- Build multimodal study habits deliberately. If your current study routine relies primarily on one modality (reading, watching, or building), add complementary modalities.

## Glossary

| Term | Definition |
|------|------------|
| Citation cascade | The phenomenon where a claim gains perceived credibility through repeated citation rather than evidence |
| Provenance analysis | Tracing a claim back to its original source to verify its basis |
| Generative learning | Active strategies that require learners to generate representations of material rather than passively receiving them |
| Multimedia principle | People learn better from words and pictures together than from words alone |
| Spatial contiguity | Placing explanatory text near the corresponding visual element |
| Coherence | Eliminating extraneous material from instructional presentations |

## Quick References

- Masters, K. (2013). "Edgar Dale's Pyramid of Learning in Medical Education." *Medical Teacher*, 35(10), e1566-e1573
- Noetel, M. et al. (2022). "Learning about Learning: A Meta-Meta-Analysis." *Review of Educational Research*
- Fiorella, L. & Mayer, R. E. (2015). *Learning as a Generative Activity*. Cambridge University Press
- Mayer, R. E. (2021). *Multimedia Learning* (3rd ed.). Cambridge University Press

## Next Steps

- [Effective Study Techniques — Intermediate](effective-study-techniques-intermediate.md) — implementing multimedia and generative learning principles through specific study strategies
- [Cognitive Load Theory — Intermediate](cognitive-load-theory-intermediate.md) — how cognitive architecture constrains multimedia design
- [Metacognitive Strategies — Intermediate](metacognitive-strategies-intermediate.md) — monitoring the effectiveness of multimodal approaches
