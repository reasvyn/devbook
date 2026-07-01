# Evaluating Sources & Claims

## Description

The internet is full of technical content — blog posts, tutorials, documentation, benchmarks, and news. Not all of it is accurate, current, or relevant. This document teaches developers how to evaluate the credibility of sources, distinguish primary from secondary information, identify misinformation in technical writing, assess claims critically, and build a reliable information diet.

## Prerequisites

- [How to Read a Research Paper](research-papers.md) — evaluating sources builds on the critical reading skills developed for academic papers

## Table of Contents

- [Why Source Evaluation Matters](#why-source-evaluation-matters)
- [The Credibility Assessment Framework](#the-credibility-assessment-framework)
- [Primary vs. Secondary Sources](#primary-vs-secondary-sources)
- [Identifying Misinformation in Technical Writing](#identifying-misinformation-in-technical-writing)
- [Evaluating Claims in Blog Posts and News](#evaluating-claims-in-blog-posts-and-news)
- [Checking References and Citations](#checking-references-and-citations)
- [Understanding Study Limitations in Research Papers](#understanding-study-limitations-in-research-papers)
- [Reproducing Results](#reproducing-results)
- [Using Official Documentation as Source of Truth](#using-official-documentation-as-source-of-truth)
- [Building a Reliable Information Diet](#building-a-reliable-information-diet)
- [Common Cognitive Biases in Technical Reading](#common-cognitive-biases-in-technical-reading)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Why Source Evaluation Matters

Developers build their knowledge and make technical decisions based on information from many sources. The cost of acting on incorrect information can be severe:

| Cost type | Example |
|---|---|
| Security | Following outdated TLS configuration advice leaves systems vulnerable |
| Performance | Adopting a tool based on inflated benchmark claims wastes engineering time |
| Reliability | Implementing a pattern that works in tests but fails in production causes incidents |
| Financial | Choosing an expensive vendor solution when an open-source alternative would suffice |
| Career | Promoting an incorrect approach damages credibility |

**The information quality spectrum:**

```text
High quality:      Official specs, peer-reviewed papers, official documentation,
                   verified benchmarks, authoritative references

Medium quality:    Well-researched blog posts, conference talks, books from
                   reputable publishers, curated tutorials

Low quality:       Unverified blog posts, forum answers, vendor marketing,
                   social media posts, unedited AI-generated content

Unreliable:        Outdated content, known misinformation, anonymous sources,
                   content farms, paid promotions disguised as articles
```

### The Credibility Assessment Framework

Use the ACCAB framework to assess any source:

**Authority — Who wrote it?**

| Question | What to check |
|---|---|
| Is the author identified? | Named author or anonymous? |
| What are the author's credentials? | Job title, experience, publication history, reputation in the field |
| Is the author affiliated with a biased organization? | Vendor employee, competitor, advocacy group |
| Has the author written other content on this topic? | Blog, papers, talks, books |
| Is the author cited by others? | Citation count, references from other trusted sources |

**Accuracy — Is it correct?**

| Question | What to check |
|---|---|
| Are claims supported by evidence? | Data, citations, code examples, benchmarks |
| Are the facts checkable? | Can you verify the claims independently? |
| Are there errors in the content? | Technical inaccuracies, outdated commands, wrong versions |
| Does the content match official documentation? | Consistency with primary sources |
| Have errors been acknowledged and corrected? | Errata, update history, comments section |

**Currency — Is it up to date?**

| Question | What to check |
|---|---|
| When was it published? | Date of publication or last update |
| Is the technology version current? | References to versions that are now obsolete |
| Have there been significant changes since publication? | New standards, major library updates, security patches |
| Is the content still maintained? | Comments closed, no updates, dead links |

**Bias — What is the perspective?**

| Question | What to check |
|---|---|
| Does the author have a commercial interest? | Vendor affiliation, sponsored content, affiliate links |
| Does the content acknowledge alternatives? | Mentions competing approaches, discusses trade-offs |
| Are limitations discussed? | Honest about weaknesses and edge cases |
| Is the language emotional or measured? | Hype words, superlatives, FUD (fear, uncertainty, doubt) |
| Who funded the work? | Research funding, corporate sponsorship, government grants |

**Basis — What is the foundation?**

| Question | What to check |
|---|---|
| Is this primary, secondary, or tertiary information? | Original research vs. summary vs. curated list |
| Are sources cited? | References to specific papers, docs, or data |
| Can the claims be reproduced? | Is the methodology described? Is the code available? |
| Is the reasoning logical? | Do conclusions follow from evidence? Are there gaps? |

**Applying the framework:**

```text
Source: "Why GraphQL is 10x faster than REST" (unknown author blog)

Authority:   Author not identified, no bio, no social media presence — LOW
Accuracy:    No benchmarks, no code, no verifiable data — LOW
Currency:    No date visible — UNKNOWN
Bias:        Title uses "10x", emotional language — HIGH BIAS (likely marketing)
Basis:       No citations, no references — LOW

Verdict:     Do not rely on this source. Find verified benchmarks instead.
```

### Primary vs. Secondary Sources

Understanding the source hierarchy helps you evaluate how far information is from its origin.

**Primary sources — original information:**

| Type | Examples | Authority level |
|---|---|---|
| Official specification | ECMAScript spec, HTTP RFC, OpenAPI spec | Highest |
| Peer-reviewed paper | Conference paper, journal article | High (after review) |
| Original data | Benchmark results, survey data, telemetry | High (if methodology is sound) |
| Official documentation | Language docs, framework docs | High (for that specific version) |
| Source code | Open-source repository | High (truth of what the code does) |
| Patent | Granted patent | Varies (legal validity, not technical accuracy) |

**Secondary sources — interpretation of primary sources:**

| Type | Examples | Authority level |
|---|---|---|
| Book | Technical book published by reputable press | Medium-High (depends on author and editorial review) |
| Conference talk | Recorded presentation | Medium (depends on speaker and venue) |
| Blog post (expert) | Post by recognized expert with citations | Medium |
| Tutorial | Step-by-step guide | Medium (may omit edge cases) |
| Documentation site | Third-party docs (not official) | Medium (verify against primary) |

**Tertiary sources — curation and summary:**

| Type | Examples | Authority level |
|---|---|---|
| Newsletter | Curated links with summaries | Low-Medium |
| Wikipedia | Community-edited encyclopedia | Low-Medium (check citations) |
| Stack Overflow | Community Q&A | Low (check votes, comments) |
| Social media | Tweets, LinkedIn posts | Low |
| AI-generated content | LLM responses, AI articles | Low (may fabricate) |

**The telephone game effect:**

Each level of indirection from the primary source introduces potential for error. A secondary source may over-simplify, omit edge cases, or misinterpret the original. A tertiary source adds more distortion.

```text
Primary:     RFC 9110 defines HTTP semantics. Section 9.3.1 defines GET as
             "transferring a current representation of the target resource."

Secondary:   Blog post says "GET requests retrieve a resource from the server."
             (omits that it transfers a representation, not the resource itself)

Tertiary:    Stack Overflow says "GET is used to get data from the server."
             (further simplified, loses precision)

Reality:     Many developers believe GET only retrieves data, missing that
             GET can be used for any safe, idempotent operation.
```

### Identifying Misinformation in Technical Writing

Technical misinformation takes many forms. Recognizing these patterns helps you avoid being misled.

**Patterns of technical misinformation:**

**1. Hype and superlatives:**

Words like "revolutionary", "game-changing", "blazingly fast", "the future of", "everyone is switching to" are marketing language, not technical analysis.

```text
Suspicious:  "Rust is going to replace C++ everywhere. Every major company
             is rewriting their systems in Rust. If you are not learning Rust
             now, you will be obsolete in 5 years."

Balanced:    "Rust offers memory safety without garbage collection, making it
             attractive for systems programming. Adoption is growing in areas
             like browser engines and operating systems. However, C++ remains
             dominant in game development and legacy systems."
```

**2. Fake precision:**

Claims with precise numbers that cannot be verified.

```text
Suspicious:  "Our framework improves developer productivity by 47.3%."

Critical:    How was productivity measured? What was the sample size? Who
             conducted the study? Is the methodology published?
```

**3. Cherry-picked comparisons:**

Comparing a new tool's best feature against an old tool's worst feature.

```text
Suspicious:  "Database X is 100x faster than PostgreSQL."

Critical:    What workload? What configuration? What version of PostgreSQL?
             Is the comparison against an optimized configuration of both?
```

**4. Appeal to authority without substance:**

Using a famous name or company to lend credibility without evidence.

```text
Suspicious:  "As used by Google, Facebook, and Netflix — the world's
             leading companies trust this approach."

Critical:    How do they use it? At what scale? For what purpose? Does their
             use case match mine?
```

**5. FUD (Fear, Uncertainty, Doubt):**

Using fear of negative outcomes to promote a specific solution.

```text
Suspicious:  "If you are not using containers, your deployments are insecure
             and unreliable. Containers are the only way to achieve production
             readiness."

Critical:    Containers address specific deployment challenges. Many
             successful systems run without containers. The claim that
             containers are the "only way" is false.
```

**6. Survivorship bias:**

Only presenting successful cases while ignoring failures.

```text
Suspicious:  "Every company that adopted microservices saw improved deployment
             frequency."

Critical:    What about companies that failed with microservices? What about
             companies that reverted to monoliths? A list of successes without
             failures is not a balanced evaluation.
```

**7. False dichotomy:**

Presenting only two options when more exist.

```text
Suspicious:  "You can either use Kubernetes or manage servers manually. There
             is no middle ground."

Critical:    What about managed container services (ECS, Cloud Run)? What about
             PaaS (Heroku, Render)? What about serverless? The dichotomy is
             false.
```

**8. Citation laundering:**

Citing a claim from a secondary source that itself is citing a weaker original.

```text
Blog A says: "Studies show that 90% of microservice adoptions fail."

Blog A cites Blog B, which cites Blog C, which cites a tweet that
references an unpublished internal report.

Trace back:  There is no peer-reviewed study. The claim originates from an
             anonymous anecdote. It has been "laundered" through multiple
             citations that make it look authoritative.
```

### Evaluating Claims in Blog Posts and News

Blog posts and technical news articles are common secondary sources. Evaluate them systematically.

**Blog post evaluation checklist:**

```text
[ ] Author identified with relevant credentials
[ ] Date of publication visible
[ ] Technology versions mentioned
[ ] Claims supported by evidence (code, data, benchmarks)
[ ] Trade-offs and limitations discussed
[ ] Alternatives acknowledged
[ ] Sources cited (links to docs, papers, other posts)
[ ] Comments or updates section (corrections, discussions)
[ ] No excessive hype language
[ ] Not sponsored content (or clearly labeled if it is)
```

**Questions for evaluating benchmarks in blog posts:**

1. Is the benchmark code available? Can you run it yourself?
2. What hardware was used? (CPU, RAM, disk, network)
3. What versions of the software were tested?
4. What workload was used? (Read/write ratio, data size, concurrency)
5. Was the system configured optimally? (Tuning, caching, connection pooling)
6. Were cold starts measured? (First request after startup)
7. Are results reported with variance? (Error bars, percentiles)
8. Were multiple runs performed? (Single run may be noise)
9. Is the benchmark published by the vendor? (Vendor benchmarks are marketing)
10. Does the benchmark match your use case? (Different workloads yield different results)

**Questions for evaluating technical news:**

1. Is this a primary report or a summary of another source?
2. Is the original source linked and accessible?
3. Is the reporting balanced or promotional?
4. Are quotes attributed to named individuals?
5. Has the story been corroborated by other sources?
6. Is the publication known for technical accuracy?

### Checking References and Citations

References are the backbone of credible technical writing. They allow readers to verify claims and explore the foundation of an argument.

**When you see a citation, check:**

1. **Does the citation support the claim?** Sometimes papers are cited for claims they do not actually make. The classic case: citing a paper that disproves a claim as if it supports it.

2. **Is the citation primary or secondary?** A citation to a blog post about a paper is weaker than a citation to the paper itself.

3. **Is the citation current?** Technology changes. A citation from 2010 about database performance may not apply in 2025.

4. **Is the citation authoritative?** A citation to an arXiv preprint is different from a citation to a peer-reviewed conference paper. Both can be valid, but they carry different weight.

5. **Is the citation context accurate?** Does the cited source actually say what the citing author claims it says?

**Citation chasing:**

When a claim matters, chase the citation chain back to the primary source:

```text
Blog post says:  "Studies show pair programming reduces bugs by 25% [1]"

Reference [1]:   Blog B: "The Benefits of Pair Programming"
                 This blog cites reference [2]

Reference [2]:   "Effects of Pair Programming on Developer Productivity"
                 Conference paper from 2003

Read paper:      The study had 30 participants, all students, over 2 weeks.
                 The 25% reduction was for one specific type of bug.
                 The paper explicitly warns against generalizing.
                 
Conclusion:      The blog post's claim is technically based on the paper
                 but overstates the result. The original study is narrow.
```

### Understanding Study Limitations in Research Papers

When a paper makes a claim, understand the limitations of the study.

**Common limitations to watch for:**

| Limitation | What it means | Example |
|---|---|---|
| Small sample size | Results may not generalize | Study of 10 developers writing code |
| Single environment | Results may not transfer | Benchmark on specific hardware |
| Short duration | Long-term effects unknown | 2-week study of a development practice |
| Student participants | May not reflect professional practice | CS students as proxies for developers |
| Toy problem | May not reflect real-world complexity | Reverse a linked list vs. build a payment system |
| Self-reported data | Subject to bias | Surveys vs. actual measurements |
| No control group | Cannot attribute causation | Before/after without a control |
| Confounding variables | Other factors may explain results | New tool adopted at same time as team restructuring |
| Publication bias | Positive results more likely to be published | Failed experiments not published |

**Reading the limitations section:**

Every good paper has a limitations section. This is where the authors are honest about what their study does not prove.

```text
Example limitations from a paper:

"Several limitations of this study should be acknowledged.
First, our experiments were conducted on a single cloud provider
(AWS) with a specific instance type. Results may vary on other
platforms. Second, our workload generator simulates read-heavy
patterns; write-heavy workloads may produce different results.
Third, we tested with datasets up to 100GB; larger datasets may
reveal scaling issues not captured in our experiments."
```

A developer reading this should consider: is my workload read-heavy? How large is my dataset? Am I on AWS or a different cloud? If the answer differs from the study conditions, the results may not apply.

### Reproducing Results

The gold standard for evaluating a claim is to reproduce the result yourself.

**Levels of reproduction:**

| Level | Effort | Confidence gained |
|---|---|---|
| Read the paper and understand the methodology | Low | Low |
| Write a summary and evaluate the reasoning | Low | Low-Medium |
| Re-run the authors' published code | Medium | Medium-High |
| Re-implement the approach from the paper description | High | High |
| Run on your own workload and environment | High | Highest |

**Why reproduction matters:**

Papers and blog posts contain errors. A study of CS papers found that many results cannot be reproduced due to missing code, incomplete methodology descriptions, or hidden assumptions.

**When reproduction is not practical:**

Not every claim can or should be reproduced. Prioritize:

- Claims that form the basis of important decisions
- Claims that contradict established knowledge
- Claims that seem too good to be true
- Claims from sources with low credibility

**How to reproduce efficiently:**

1. Check if the authors provide code (GitHub, Papers With Code)
2. Check if the benchmark datasets are available
3. Set up the environment as described in the methodology
4. Run and compare results
5. If results differ, investigate: environment differences? Software version differences? Misunderstanding of the approach?

### Using Official Documentation as Source of Truth

Official documentation is the closest thing to a primary source for software systems. However, even official docs have limitations.

**When official docs are the best source:**

- API reference documentation — what parameters, what return types, what errors
- Configuration reference — what options, what defaults, what formats
- Language specification — syntax, semantics, type rules
- Protocol specification — wire format, state machines

**When official docs are not sufficient:**

| Situation | Why | What to do |
|---|---|---|
| Docs are outdated | New features not documented, old behavior not updated | Check release notes, changelog, GitHub issues |
| Docs are incomplete | Edge cases, error states not documented | Test behavior, check source code |
| Docs describe intent, not reality | Implementation may differ | Test against actual behavior |
| Docs assume expertise | Terminology not explained | Find supplemental learning resources |

**Verifying documentation against code:**

The source code is the ultimate source of truth. When documentation and code disagree, the code is likely correct (or the documentation reflects a planned but not implemented feature).

```text
Documentation says: "The timeout parameter accepts milliseconds."

Code says:         // timeout is in seconds
                   setTimeout(handler, timeout * 1000);

Reality:           The documentation is wrong. The implementation uses seconds.
                   File a documentation bug.
```

### Building a Reliable Information Diet

A developer's information diet is the set of sources they regularly consume. Design it intentionally.

**Source quality tiers:**

| Tier | Types | How to use |
|---|---|---|
| Tier 1: Foundations | Official specs, primary research, authoritative references | Build core knowledge on these |
| Tier 2: Commentary | Expert blogs, conference talks, technical books | Learn from experts, but verify key claims against Tier 1 |
| Tier 3: News | Newsletters, social media, aggregators | Stay current, but do not make decisions based on these alone |
| Tier 4: Help | Stack Overflow, forums, chat | Solve specific problems, but verify solutions |

**Curating your sources:**

1. **Identify key domains** relevant to your work (databases, networking, frontend, etc.)
2. **For each domain, find Tier 1 sources.** Official documentation, canonical papers, authoritative references.
3. **Identify Tier 2 experts.** Follow their blogs, talks, and social media.
4. **Set up Tier 3 feeds.** Newsletters, RSS feeds, Twitter/LinkedIn lists.
5. **Use Tier 4 for problem-solving only.** Do not build foundational knowledge from Q&A sites.

**Example information diet for a backend developer:**

```text
Tier 1 (foundations):
- HTTP: RFCs 9110-9114
- Databases: PostgreSQL documentation
- Distributed systems: "Designing Data-Intensive Applications" (book)
- Cloud: AWS/Azure/GCP official documentation

Tier 2 (commentary):
- Blogs: Dan Luu, Julia Evans, Marc Brooker
- Conferences: KubeCon, AWS re:Invent, Strange Loop talks

Tier 3 (news):
- Newsletters: TLDR, Backend Banter, The New Stack
- Social media: curated Twitter lists, LinkedIn topics

Tier 4 (help):
- Stack Overflow, GitHub Discussions, Discord communities
```

**Periodic source audit:**

Every quarter, review your information diet:

- Are all sources still current?
- Are any sources no longer reliable?
- Are there new domains you need to add?
- Are you spending too much time on low-quality sources?
- Are you relying too heavily on any single source?

### Common Cognitive Biases in Technical Reading

**Confirmation bias:** Seeking and accepting information that confirms existing beliefs while ignoring contradictory evidence.

Mitigation: Actively look for content that challenges your current views. When evaluating a new tool, read critical reviews and failure stories, not just success stories.

**Authority bias:** Trusting a claim because it comes from a recognized authority (company, person, institution).

Mitigation: Evaluate the evidence, not the source. Google's recommendation may be wrong for your context. A famous engineer's opinion is still just an opinion.

**Recency bias:** Giving more weight to recent information because it is recent.

Mitigation: Check the date but also check whether older information has been superseded. Some foundational knowledge does not change.

**Availability bias:** Judging the likelihood of events based on how easily examples come to mind.

Mitigation: Just because you read about three microservices failures this week does not mean microservices are failing everywhere. Base decisions on systematic data, not anecdotal examples.

**Sunk cost bias:** Continuing to believe a source or approach because you have invested time in it.

Mitigation: Be willing to change your mind when new evidence emerges. Prior investment does not justify continued belief.

**Dunning-Kruger effect:** Overestimating your knowledge of a topic, leading to poor source evaluation.

Mitigation: The more you learn, the more you realize what you do not know. Humility in evaluation is a sign of expertise.

## Glossary

| Term | Definition |
|---|---|
| ACCAB | Credibility assessment framework: Authority, Accuracy, Currency, Bias, Basis |
| Availability bias | Overestimating the likelihood of events that come to mind easily |
| Cherry-picking | Selecting only data that supports a claim while ignoring contradictory data |
| Citation chasing | Tracing the chain of references back to the original source |
| Citation laundering | Citing a claim through intermediaries that conceal the weak original source |
| Confirmation bias | Favoring information that confirms existing beliefs |
| Credibility | Trustworthiness and expertise of a source |
| FUD | Fear, Uncertainty, Doubt — tactic of spreading negative information |
| Hallucination | AI-generated content that is factually incorrect but presented with confidence |
| Misinformation | False or misleading information regardless of intent |
| Primary source | Original information with direct knowledge (spec, paper, code) |
| Secondary source | Interpretation or summary of primary sources |
| Survivorship bias | Focusing on successful cases while ignoring failures |
| Tertiary source | Curation and summary of secondary sources |
| Tier 1/2/3/4 | Information source quality levels: foundations, commentary, news, help |

## Quick References

- [CRAAP Test](https://library.csuchico.edu/sites/default/files/craap-test.pdf) — currency, relevance, authority, accuracy, purpose (library science framework)
- [Google's Search Quality Rater Guidelines](https://static.googleusercontent.com/media/guidelines.raterhub.com/en//searchqualityevaluatorguidelines.pdf) — how Google evaluates content quality
- [Retraction Watch](https://retractionwatch.com/) — database of retracted scientific papers
- [Papers With Code](https://paperswithcode.com/) — papers with available code implementations
- [Vendor Benchmark Skepticism Guide (Dan Luu)](https://danluu.com/benchmarking/) — classic post on benchmark evaluation
- [How to Lie with Statistics (Huff)](https://en.wikipedia.org/wiki/How_to_Lie_with_Statistics) — classic book on statistical manipulation
- [xkcd: Significant](https://xkcd.com/882/) — comic about statistical significance

## Next Steps

- [Note-Taking & Knowledge Management](note-taking.md) — organizing and retaining the knowledge you gain from evaluated sources
- [Reading RFCs & Internet Standards](rfcs.md) — applying source evaluation to protocol specifications
