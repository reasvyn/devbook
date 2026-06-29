# How to Read a Research Paper

## Description

Academic papers are a primary source of new knowledge in computer science — from new algorithms and system designs to empirical studies of developer practices. This document teaches developers the three-pass method for reading papers efficiently, understanding paper structure, evaluating methodology, and deciding what to implement versus what to merely understand.

## Prerequisites

None — this is a foundational skill for technical reading.

## Table of Contents

- [Why Developers Read Papers](#why-developers-read-papers)
- [The Three-Pass Method](#the-three-pass-method)
- [Understanding Paper Structure](#understanding-paper-structure)
- [Identifying Key Contributions](#identifying-key-contributions)
- [Evaluating Methodology](#evaluating-methodology)
- [Reading for Implementation vs. Understanding](#reading-for-implementation-vs-understanding)
- [Tools for Paper Management](#tools-for-paper-management)
- [Reading Different Paper Types](#reading-different-paper-types)
- [The Paper-to-Code Pipeline](#the-paper-to-code-pipeline)
- [Common Pitfalls When Reading Papers](#common-pitfalls-when-reading-papers)
- [Building a Reading Workflow](#building-a-reading-workflow)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Why Developers Read Papers

Academic papers are the foundation of most technology we use daily. The consensus algorithm in etcd, the log-structured merge trees in LevelDB, the Paxos and Raft protocols, the MapReduce paradigm, and the transformer architecture behind modern LLMs all originated as academic papers. Reading papers directly gives you access to the original reasoning, assumptions, and trade-offs that informed these systems.

**Benefits of reading papers:**

| Benefit | Explanation |
|---|---|
| Deep understanding | Papers explain not just what a system does, but why it was designed that way |
| Early access | Papers often precede production-ready implementations by years |
| Critical perspective | Papers include limitations and open problems that marketing materials omit |
| Foundational knowledge | Understanding the original paper helps you evaluate derived implementations |
| Research literacy | Skills transfer to evaluating claims in blogs, documentation, and vendor materials |

**What papers are not:**

Papers are not tutorials, not reference documentation, and not production-ready blueprints. They describe research — often prototyped in controlled environments. Reading papers with the wrong expectations leads to frustration or, worse, incorrect implementation decisions.

### The Three-Pass Method

The three-pass method, formalized by S. Keshav for reading academic papers, optimizes for time-to-understanding. You do not read every paper the same way. You triage first, then dig deeper only as needed.

**Pass 1: The Survey (5-10 minutes)**

The goal is to decide whether the paper is worth your time.

Steps:

1. Read the title, abstract, and keywords
2. Scan the introduction — specifically the last paragraph that states contributions
3. Read the section headings to understand the structure
4. Look at every figure, table, and diagram
5. Read the conclusion

After Pass 1, you should be able to answer:

- What problem does this paper address?
- What is the claimed contribution?
- Is the paper relevant to my work?
- Should I invest more time?

If the answer to the last question is no, stop. Archive the paper and move on. You have read it well enough.

**Pass 2: The Read (30-90 minutes)**

The goal is to understand the paper's content in detail.

Steps:

1. Read the entire paper sequentially
2. Skip details you do not understand on first pass — mark them
3. Note unfamiliar terminology, notation, or references
4. Write marginal annotations: questions, objections, connections
5. Pay special attention to the method and results sections
6. After reading, write a one-paragraph summary in your own words

**Pass 3: The Review (15-30 minutes)**

The goal is to evaluate and synthesize the paper.

Steps:

1. Re-read sections marked as unclear
2. Check cited references for context
3. Evaluate the methodology critically
4. Assess whether the claims are supported by the evidence
5. Consider how the work applies (or does not apply) to your context
6. Write review notes: strengths, weaknesses, open questions, implementation ideas

**When to stop after each pass:**

| Pass | Stop if |
|---|---|
| Pass 1 | Not relevant, too preliminary, superseded by later work |
| Pass 2 | Understanding is sufficient for your purpose, paper is clear but not groundbreaking |
| Pass 3 | You need to implement the approach, build on the work, or evaluate it thoroughly |

Not every paper deserves all three passes. A paper you cite as background might only get Pass 1. A paper whose technique you plan to implement gets all three passes and possibly a fourth (reproduction).

### Understanding Paper Structure

Computer science papers generally follow the IMRaD structure with variations.

**Title and Abstract:**

The title is a concise claim or description. The abstract is a 150-250 word summary covering the problem, approach, key results, and implications. Read the abstract critically — it is an advertisement for the paper, not an objective summary.

```text
Example abstract structure:

"Distributed consensus is fundamental to replicated state machines.
Existing protocols like Paxos are difficult to understand and implement
correctly. [PROBLEM]

We present Raft, a consensus protocol designed for understandability.
Raft decomposes consensus into leader election, log replication, and
safety, reducing the state space compared to Paxos. [APPROACH]

Our evaluation shows that students understand Raft significantly faster
than Paxos, and that Raft performs comparably to Paxos in throughput
and latency. [RESULTS]

These results suggest that protocol understandability can be improved
without sacrificing performance, making consensus more accessible for
practical systems." [IMPLICATIONS]
```

**Introduction:**

The introduction sets context, states the problem, summarizes prior work's limitations, and lists contributions. The last paragraph of the introduction is the most important — it lists what the paper claims to contribute.

```text
The contributions of this paper are:
1. A new consensus protocol that separates leader election, log
   replication, and safety into distinct phases
2. A mechanism for cluster membership changes using a joint consensus
   approach
3. An evaluation showing comparable performance to Paxos with
   significantly improved understandability
```

**Related Work:**

This section positions the paper within existing research. Read it to understand what the authors consider the state of the art. Pay attention to how the authors critique prior work — those critiques reveal the gap the paper aims to fill.

**Method / Approach:**

This section describes what the authors did. It may include formal definitions, algorithms, system architecture, experimental setup, or mathematical proofs. This is the most important section for evaluating the paper's validity.

For systems papers, look for:

- System architecture diagrams
- Key algorithms (often presented as pseudocode)
- Implementation details (language, libraries, configurations)
- Experimental setup (hardware, software versions, dataset characteristics)

For empirical papers, look for:

- Research questions and hypotheses
- Study design (controlled experiment, case study, survey)
- Participant characteristics (number, background, selection criteria)
- Data collection methods (measurements, interviews, logs)
- Statistical methods used

**Results:**

This section presents what the authors found. Look at figures and tables first — they often communicate results more clearly than text. Pay attention to:

- Error bars and confidence intervals
- Statistical significance (p-values, effect sizes)
- Comparison to baselines
- Ablation studies (which parts contribute to the result)
- Failure cases and negative results

**Discussion:**

The authors interpret their results, acknowledge limitations, and suggest future work. The limitations section is often the most honest part of a paper. Read it to understand the boundaries of the claimed contributions.

**Conclusion:**

A summary of the paper's contributions and a look forward. Often overlaps with the abstract but may include more nuance about limitations.

**References:**

The bibliography reveals which communities the paper engages with, what prior work it builds on, and which alternative approaches it acknowledges. A paper with few or narrow references may be disconnected from related work.

### Identifying Key Contributions

Every paper claims to contribute something new. The contribution might be:

| Contribution type | Example | What to evaluate |
|---|---|---|
| New algorithm | Paxos, Raft, MapReduce | Correctness, complexity, trade-offs vs. existing |
| New system | Google File System, Dynamo | Architecture decisions, performance characteristics |
| Empirical finding | "Microservices increase deployment frequency" | Methodology, statistical validity, generalizability |
| New metric or benchmark | TPC-C, YCSB, SPEC | Relevance, reproducibility, coverage |
| New formalism or model | CAP theorem, CALM theorem | Soundness, practical implications |
| New tool or framework | TensorFlow, PyTorch | Usability, adoption, performance |

**Evaluating a claimed contribution:**

1. **Is it truly new?** Check the related work section. Sometimes papers rebrand existing techniques.
2. **Is it significant?** A 1% improvement on a solved problem is less significant than a 10x improvement that opens new possibilities.
3. **Is it correct?** Check for logical errors, incorrect proofs, or flawed experimental design.
4. **Is it useful?** Does the contribution apply to real problems or only to artificial scenarios?
5. **Is it reproducible?** Would you be able to build on this work with the information provided?

**The contribution signal-to-noise ratio:**

Some papers make bold claims in the abstract but deliver modest results. Others understate significant findings. Reading critically means separating what the paper actually shows from what the authors claim it shows.

### Evaluating Methodology

Methodology is the most commonly overlooked section by developers reading papers. Developers tend to focus on the system design or algorithm while skipping how the claims were validated.

**Common methodological issues:**

| Issue | What to look for | Why it matters |
|---|---|---|
| No baseline comparison | Paper claims improvement but does not compare to existing work | Cannot assess significance |
| Weak baseline | Compares against an unoptimized or straw-man baseline | Inflates apparent improvement |
| Small workload | Benchmarks use small or unrealistic data | Results may not scale |
| Cherry-picked results | Best-case scenario presented as typical | Misleading performance expectations |
| Missing error bars | Single run, no variance reported | Performance may be noisy |
| No ablation study | Cannot tell which component contributed to improvement | Hard to understand or reproduce |
| Confounding factors | Not controlling for environment, implementation quality | Results may be coincidental |
| Statistical issues | Small sample, p-hacking, multiple comparisons | Results may not be statistically significant |

**Questions to ask about methodology:**

1. Is the experimental setup described in enough detail to reproduce?
2. Are the workloads representative of real-world usage?
3. Are the metrics appropriate for the claimed contribution?
4. Would the results hold if the experiment were run in a different environment?
5. Are negative results or failures reported?
6. Are the statistical methods appropriate and correctly applied?

### Reading for Implementation vs. Understanding

Not all papers need to be read for implementation. Recognizing the depth required saves time and effort.

**Reading for understanding:**

You read to know the concept exists, understand the intuition, and recognize when to apply it.

Example: Reading the CAP theorem paper to understand the trade-off between consistency and availability in distributed systems.

What you need:

- The core insight (CAP trade-off)
- The practical implications (when to choose CP vs. AP)
- The limitations (PACELC extension, real-world nuance)

What you can skip:

- Formal definitions and proofs
- Full related work section
- Detailed experimental setup

**Reading for implementation:**

You read to build something based on the paper's approach.

Example: Reading the Raft paper to implement a consensus library.

What you need:

- Precise algorithm specification
- All edge cases and failure scenarios
- Implementation details (timeouts, batching, pipelining)
- Correctness arguments and proofs

What you cannot skip:

- Any detail in the algorithm description
- Safety and liveness properties
- Cluster membership change protocol
- Log compaction mechanism

**The implementation gap:**

Even detailed papers leave implementation details unspecified. You will encounter decisions the paper did not anticipate. The implementation gap includes:

- Choosing specific timeout values
- Handling network partitions
- Optimizing for specific hardware
- Managing concurrency and locking
- Log storage format and I/O patterns

When reading for implementation, keep a running list of "implementation decisions I will need to make."

### Tools for Paper Management

Managing a growing paper library requires systematic tooling. Without it, papers become a pile of PDFs that you cannot navigate or retrieve.

**Paper discovery tools:**

| Tool | Description | Best for |
|---|---|---|
| arXiv | Free preprint repository for CS, math, physics | Staying current, pre-publication access |
| Google Scholar | Search across multiple publishers | Finding papers and tracking citations |
| Semantic Scholar | AI-enhanced search with citation graphs | Finding related work and influential papers |
| Papers With Code | Papers linked to code implementations | Implementation-focused reading |
| DBLP | CS bibliography database | Author and venue tracking |
| ACM Digital Library | Full-text access to ACM publications | Conference proceedings |
| IEEE Xplore | Full-text access to IEEE publications | Engineering-focused papers |

**Reference management tools:**

| Tool | Platform | Key features |
|---|---|---|
| Zotero | Cross-platform, open source | Browser integration, tag-based organization, group libraries, BibTeX export |
| Mendeley | Cross-platform, free tier | PDF annotation, social features, recommendation engine |
| Paperpile | Web-based, subscription | Google Scholar integration, Google Docs support |
| JabRef | Cross-platform, open source | BibTeX-native, lightweight, LaTeX-focused |
| EndNote | Cross-platform, paid | Maximum citation format support, institutional licensing |

**Zotero workflow:**

```text
1. Install Zotero and browser connector
2. Browse to paper page on arXiv, ACM DL, or IEEE Xplore
3. Click browser connector — Zotero imports metadata and PDF
4. Organize into collections (e.g., "distributed-systems", "ml-systems")
5. Tag with topics, status ("to-read", "read", "implementing")
6. Annotate PDFs using Zotero's built-in PDF viewer
7. Export citations as BibTeX for LaTeX papers
8. Sync to cloud for access across devices
```

**Reading workflow in Zotero:**

```text
1. Inbox collection — all new papers go here
2. Triage — apply Pass 1, move to To Read or Discard
3. Read — apply Pass 2, annotate PDF
4. Reviewed — move to Reviewed collection, write notes
5. Reference — add to project-specific collection
6. Extract — create atomic notes in knowledge management system
```

### Reading Different Paper Types

Not all CS papers follow the same structure or evaluation norms.

**Systems papers (OSDI, SOSP, NSDI, ATC):**

Systems papers describe the design and implementation of a computer system. They follow a design-implementation-evaluation structure.

Reading focus:

- What is the system architecture?
- What are the key design decisions and trade-offs?
- How is the system implemented?
- What workloads test which aspects?
- What are the performance characteristics?

**Theory papers (STOC, FOCS, PODC, SODA):**

Theory papers present formal results: theorems, proofs, complexity bounds.

Reading focus:

- What is the formal problem definition?
- What is the claimed result (upper bound, lower bound, impossibility)?
- Is the proof correct?
- What are the assumptions?
- Does the result have practical implications?

**Empirical papers (ICSE, CHI, PLDI, SIGMETRICS):**

Empirical papers study how people build software or how systems behave in practice.

Reading focus:

- What research questions are asked?
- What is the study methodology?
- Is the sample representative?
- Are the conclusions supported by the data?
- Could there be alternative explanations for the results?

**Position papers (workshops, early-stage venues):**

Position papers argue for a particular approach or perspective, often without experimental validation.

Reading focus:

- What is the argument?
- Is the reasoning sound?
- What evidence (if any) supports the claims?
- Is the position worth exploring further?

**Survey papers:**

Survey papers summarize and organize existing research on a topic.

Reading focus:

- What is the classification or taxonomy?
- Which areas have been well-studied?
- Which areas need more work?
- What are the open problems?

### The Paper-to-Code Pipeline

When a paper describes a technique you want to implement, follow this pipeline:

**Step 1: Understand the problem.**

Read the introduction and related work until you can articulate the problem in your own words. If you cannot state the problem clearly, you cannot implement the solution correctly.

**Step 2: Formalize the approach.**

Extract the algorithm or architecture from the paper. Represent it as:

- Pseudocode with explicit data structures
- A state machine with defined states and transitions
- An architecture diagram with component responsibilities and interfaces

**Step 3: Map to your domain.**

The paper uses specific terminology and assumptions. Map those to your system:

```text
Paper term           Your term
------------------   -----------------
"node"               "server instance"
"leader"             "primary replica"
"log entry"          "WAL record"
"quorum"             "majority of voters"
"epoch"              "leader term"
```

**Step 4: Identify unspecified details.**

List decisions the paper leaves open:

```text
- Heartbeat interval: 50ms? 500ms? Adaptive?
- Election timeout: 150-300ms? Randomized range?
- Batch size: Fixed? Adaptive to load?
- Log compaction: Every N entries? Every M bytes?
- Snapshot format: Protobuf? FlatBuffers? JSON?
- Network transport: TCP? QUIC? In-memory?
- Storage engine: RocksDB? BoltDB? Custom?
```

**Step 5: Prototype the core.**

Implement the minimal viable version of the key contribution. Do not optimize. Do not handle all edge cases. Validate that the core idea works in your environment.

**Step 6: Validate against the paper.**

Does your implementation behave as the paper describes? If not, check:

- Misunderstanding of the algorithm
- Incorrect parameterization
- Environment differences
- Paper omitted a critical detail

**Step 7: Productionize.**

Add error handling, monitoring, performance optimization, and testing. Each addition should preserve the core properties proven in the paper.

### Common Pitfalls When Reading Papers

**Confusing simulation with reality:**

Papers often evaluate in simulation or controlled environments. A 10x improvement in simulation may become a 2x improvement (or a regression) in production. The paper's evaluation is a best-case scenario.

**Ignoring assumptions:**

Every paper makes assumptions: network reliability, clock synchronization, homogeneous hardware, specific workload characteristics. If your environment violates these assumptions, the paper's claims may not hold.

**Overindexing on results:**

A compelling result does not guarantee correct methodology. Conversely, modest results may obscure a genuinely important idea. Evaluate the contribution, not just the numbers.

**Relying on the abstract alone:**

The abstract is a summary, not a substitute for reading the paper. Many developers cite papers based only on the abstract, missing crucial limitations and caveats.

**Not checking the date:**

Computer science evolves quickly. A paper from 2000 may be foundational but outdated in implementation details. A paper from last year may have been superseded.

**Reading in isolation:**

A single paper is one voice in a conversation. Read related papers, check follow-up work, and look for replication studies.

### Building a Reading Workflow

An effective reading workflow is a pipeline, not a one-time event.

**Weekly workflow:**

```text
Monday:    Scan new papers from arXiv alerts and conference proceedings
           Apply Pass 1, file into To Read or Archive

Tuesday:   Read one paper from the To Read queue (Pass 2)
           Annotate and write summary

Wednesday: Review one paper (Pass 3) from the Read queue
           Write review notes, extract to knowledge base

Thursday:  Read one paper from the To Read queue (Pass 2)

Friday:    Implement or prototype one technique from a reviewed paper
           Clean up reading queue, update notes
```

**Monthly workflow:**

- Review all papers read that month
- Identify themes and connections
- Archive papers no longer relevant
- Update knowledge management system with cross-links

**Annual workflow:**

- Review the year's reading against learning goals
- Identify topics that need deeper investigation
- Prune the paper library — delete or archive papers that no longer matter

## Study Cases

### Case 1: Reading the MapReduce Paper for Understanding

A developer hears about MapReduce as a data processing paradigm. They need to understand the concept but are not implementing it.

Pass 1 (5 minutes):

- Title: "MapReduce: Simplified Data Processing on Large Clusters"
- Abstract: Describes a programming model and implementation for processing large datasets on a cluster
- Headings: Introduction, Programming Model, Implementation, Refinements, Performance, Experience, Related Work, Conclusions
- Figure 1: The MapReduce execution overview — key diagram
- Conclusion: MapReduce simplifies large-scale data processing

Decision: Worth reading. The concept is foundational.

Pass 2 (30 minutes):

- Map phase: process key-value pairs to produce intermediate key-value pairs
- Reduce phase: merge all intermediate values for the same key
- The system handles partitioning, scheduling, fault tolerance, and coordination automatically
- Key insight: restricting the programming model enables automatic parallelization

Summary: "MapReduce is a programming model that lets you write map and reduce functions while the framework handles distribution, fault tolerance, and parallelism across a cluster."

Result: Sufficient understanding for architectural knowledge. No Pass 3 needed.

### Case 2: Reading the Raft Paper for Implementation

A team decides to build a consensus-based replicated state machine. They choose Raft.

Pass 1 (10 minutes):

- Abstract: Raft is designed for understandability
- Figures show leader election and log replication
- The protocol decomposes into sub-problems
- Evaluation shows comparable performance to Paxos

Decision: Must read in full. Implementation depends on complete understanding.

Pass 2 (90 minutes):

- Detailed reading of all sections
- Leader election: terms, timeouts, votes
- Log replication: entries, commit index, match/next indices
- Safety: election restriction, committing entries from previous terms
- Cluster membership: joint consensus, configuration changes
- Log compaction: snapshots, install snapshot RPC

Pass 3 (30 minutes):

- Methodology evaluation: The understandability evaluation (student survey) is weak but the protocol correctness argument is compelling
- Questions: How to handle disk I/O during log writes? What about clock skew?
- Implementation notes: Timeout randomization, batch commit, pipeline replication

Outcome: Team produces a working Raft implementation with a clear mapping from paper section to code module.

### Case 3: A Flawed Evaluation in Paper Reading

A developer reads a paper claiming a new caching algorithm is 5x faster than LRU.

Pass 1:

- Title sounds promising
- Abstract claims significant improvement
- Decision: Read further

Pass 2:

- Methodology section reveals that the evaluation used a synthetic workload with high temporal locality (repeating sequential access patterns)
- The baseline LRU implementation was a naive linked-list version, not an optimized hash map + doubly linked list
- Only one workload was tested
- No error bars reported — single run

Pass 3 evaluation:

- The workload is unrealistic for most production systems
- The baseline is weak (optimized LRU would be faster)
- Single run means no variance data — the improvement could be noise
- The claim "5x faster" is specific to this one workload

Decision: Do not adopt. The paper's evaluation does not support the claimed general improvement.

## Examples

### Example 1: Abstract Analysis

Abstract: "We present a novel approach to distributed transaction processing using optimistic concurrency control. Our system achieves 2x throughput improvement over two-phase locking in geo-distributed environments. We evaluate using the TPC-C benchmark on a clusters of 16 nodes across 3 regions."

Critical analysis:

- "Novel" is unsubstantiated — check related work
- "2x throughput improvement" — 2x compared to what baseline? Unoptimized 2PL?
- "Geo-distributed" — what is the latency between regions? 10ms? 100ms?
- "16 nodes, 3 regions" — is this representative of the target deployment?
- "TPC-C" — standard benchmark, good. Which warehouse count? Default 10 warehouses?

### Example 2: Figure Reading

A paper's Figure 3 shows a line chart with throughput on Y-axis and number of clients on X-axis. Three lines: their system, the baseline, and an idealized upper bound.

What to look for:

- Are the axes labeled with units? (Throughput: ops/sec? KB/sec?)
- Where does their system diverge from the baseline? (At what client count?)
- Does their system approach the upper bound? (How close? At what cost?)
- Is the Y-axis truncated? (Does it exaggerate differences?)
- Are error bars shown? (If not, one run only.)
- What happens at the rightmost X values? (Does their system saturate or degrade gracefully?)

### Example 3: Citation Checking

The paper says: "Prior work has shown that NoSQL databases outperform SQL databases for web workloads [12]."

When you check reference [12]:

- Is it a peer-reviewed paper or a vendor white paper?
- What workloads were tested?
- Is the comparison fair (same hardware, optimized configurations)?
- Is the reference recent or decade-old?

If [12] is a MongoDB white paper comparing MongoDB 2.0 against MySQL 5.0 with default settings on a write-heavy workload, the claim is compromised. The citation is being used to support a broader claim than the original study supports.

### Example 4: Terminology Mapping

Paper: "Our system uses a coordinator to manage transaction ordering. Each transaction is assigned a timestamp from a monotonically increasing sequence. Transactions commit only if all participants agree."

Mapping to implementation terms:

```text
Paper term              Implementation concept
coordinator             Transaction manager service
timestamp               Monotonic logical clock (HYTRX timestamp)
monotonically increasing Global sequence counter (e.g., ZooKeeper sequential ZNode)
participants            Database shards involved in the transaction
commit                  Two-phase commit (prepare + commit phases)
agree                   All participants acknowledge prepare phase
```

## Glossary

| Term | Definition |
|---|---|
| Ablation study | Experiment that removes components to measure their contribution |
| Abstract | Brief summary of a paper's problem, approach, results, and implications |
| Baseline | The existing approach that a new technique is compared against |
| Citation graph | Network of papers connected by citations, showing influence and relationships |
| Contribution | The novel aspect a paper claims to add to existing knowledge |
| Empirical paper | Paper based on observation or experiment rather than theory alone |
| Error bar | Visual indicator of variability or uncertainty in data |
| IMRaD | Standard paper structure: Introduction, Method, Results, and Discussion |
| Methodology | The systematic approach used to conduct the research |
| Pass | One complete reading of a paper with a specific goal |
| Position paper | Paper that argues a perspective without full experimental validation |
| Preprint | Version of a paper published before peer review |
| Related work | Section describing prior research the paper builds on or differs from |
| Reproducibility | Ability to obtain the same results using the described methodology |
| Survey paper | Paper that summarizes and organizes existing research on a topic |
| Systems paper | Paper describing the design, implementation, and evaluation of a computer system |
| Three-pass method | Reading strategy: survey, read, review |
| Theory paper | Paper presenting formal results with proofs |

## Quick References

- [How to Read a Paper (Keshav)](https://blizzard.cs.uwaterloo.ca/keshav/home/Papers/data/07/paper-reading.pdf) — the canonical three-pass method
- [Papers We Love](https://paperswelove.org/) — community for reading and discussing CS papers
- [The Morning Paper](https://blog.acolyer.org/) — daily summaries of notable CS papers
- [arXiv cs updates](https://arxiv.org/list/cs/recent) — daily CS paper updates
- [Zotero](https://www.zotero.org/) — open-source reference management
- [Mendeley](https://www.mendeley.com/) — reference manager with social features
- [Semantic Scholar](https://www.semanticscholar.org/) — AI-powered academic search
- [Papers With Code](https://paperswithcode.com/) — papers with linked implementations
- [Google Scholar](https://scholar.google.com/) — broad academic paper search

## Next Steps

- [Reading RFCs & Internet Standards](rfcs.md) — applying critical reading to protocol specifications
- [Evaluating Sources & Claims](evaluating-sources.md) — techniques for assessing credibility in technical content
