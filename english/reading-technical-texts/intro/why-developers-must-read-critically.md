# Why Developers Must Read Critically

## Description

Software developers consume an enormous volume of written technical content — documentation, RFCs, academic papers, blog posts, specifications, and code. Critical reading is the skill of extracting accurate, actionable knowledge from these sources while identifying gaps, biases, and errors. This document explains why critical reading is a core engineering competency and how to develop it.

## Prerequisites

- [Why English Matters for Developers](../../intro/why-english-matters.md)

## Table of Contents

- [The Scale of Technical Reading](#the-scale-of-technical-reading)
- [Critical Reading vs. Passive Reading](#critical-reading-vs-passive-reading)
- [Types of Technical Literature](#types-of-technical-literature)
- [Reading with Purpose](#reading-with-purpose)
- [The Four-Pass Reading Method](#the-four-pass-reading-method)
- [Evaluating Technical Claims](#evaluating-technical-claims)
- [Reading Academic Papers](#reading-academic-papers)
- [Reading RFCs & Standards](#reading-rfcs--standards)
- [Reading Specifications](#reading-specifications)
- [Reading Code as Text](#reading-code-as-text)
- [Synthesis: Connecting Multiple Sources](#synthesis-connecting-multiple-sources)
- [Knowledge Management Systems](#knowledge-management-systems)
- [Common Reading Traps](#common-reading-traps)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### The Scale of Technical Reading

A professional developer reads more than they write. Estimates suggest:

- **100–200 emails per week**
- **50–200 Slack messages per day**
- **5–20 pull requests per week** (reading diffs and comments)
- **3–10 documentation pages per day**
- **1–5 technical articles or blog posts per week**
- **0–2 academic papers or RFCs per month**
- **Thousands of lines of code per day** (reading existing code)

**The cost of bad reading:**

| Bad reading habit | Cost |
|---|---|
| Skimming important details | Missed configuration, production incidents |
| Not verifying sources | Building on incorrect foundations |
| Confirmation bias | Choosing wrong technology because it confirms existing beliefs |
| Reading without context | Misunderstanding intent, making wrong assumptions |
| Not synthesizing | Repeating same mistakes, rediscovering known solutions |

### Critical Reading vs. Passive Reading

| Passive reading | Critical reading |
|---|---|
| Accepts text at face value | Questions claims and evidence |
| Reads linearly | Skips, scans, and jumps strategically |
| Does not take notes | Annotates and summarizes |
| Does not verify sources | Checks citations and references |
| Moves on after reading | Synthesizes with existing knowledge |
| Remember "what" was said | Understands "why" and "how" |
| One pass | Multiple passes with different goals |

**The critical reader's toolkit:**

1. **Question authorship.** Who wrote this? What biases might they have? What's their incentive?
2. **Question evidence.** Is the claim supported? How was the data collected? Is the sample representative?
3. **Question logic.** Do the conclusions follow from the premises? Are there hidden assumptions?
4. **Question context.** When was this written? What has changed since? What was the author's intended audience?
5. **Synthesize.** How does this connect to what I already know? Does it confirm or challenge my understanding?

### Types of Technical Literature

**Primary sources:**

| Type | Purpose | Authority | Example |
|---|---|---|---|
| Academic paper | Original research | Peer review | "MapReduce: Simplified Data Processing on Large Clusters" |
| RFC | Internet standard | IETF process | "RFC 793: Transmission Control Protocol" |
| Specification | Formal definition | Standards body | "ECMAScript Language Specification" |
| Patent | Legal protection | Legal review | "System and method for distributed data storage" |

**Secondary sources:**

| Type | Purpose | Authority | Example |
|---|---|---|---|
| Textbook | Systematic education | Editorial review | "Database System Concepts" |
| Blog post | Practical experience | None/reputation | "How Airbnb Migrated to Microservices" |
| Tutorial | Hands-on learning | None/reputation | "Django Tutorial" |
| Documentation | Reference | Product team | "Kubernetes Documentation" |
| Conference talk | Knowledge sharing | Program committee | "Keynote at KubeCon" |

**Tertiary sources:**

| Type | Purpose | Authority | Example |
|---|---|---|---|
| Stack Overflow | Problem solving | Community votes | "How to join two tables in SQL" |
| Wikipedia | Overview | Community editing | "Consensus (computer science)" |
| Technical newsletter | Curation | Curator's reputation | "Hacker Newsletter" |
| Social media | Discussion | None | Twitter thread on tech |

**Evaluating source reliability:**

| Criterion | High reliability | Low reliability |
|---|---|---|
| Review process | Peer review, standards body | No review, self-published |
| Author expertise | Subject matter expert, cited in field | Unknown, no relevant background |
| Evidence | Data, reproducible results | Anecdotes, opinions |
| Recency | Published within 1-2 years | Outdated, no update |
| Publisher | Academic journal, standards body | Personal blog, unknown platform |

### Reading with Purpose

Before reading any technical text, define your purpose:

**Three reading modes:**

| Mode | Goal | Speed | Attention |
|---|---|---|---|
| Search | Find specific information | Fast | Narrow |
| Understand | Comprehend the material | Moderate | Broad |
| Evaluate | Assess validity and relevance | Slow | Critical |

**Reading goals:**

| Goal | What to look for | When to stop |
|---|---|---|
| Learn a new concept | Definitions, examples, analogies | When you can explain it to someone else |
| Solve a specific problem | Step-by-step instructions, code | When your problem is solved |
| Evaluate a tool | Use cases, comparisons, limitations | When you can make a decision |
| Stay current | Key takeaways, trends | After understanding the main idea |
| Deep research | Citations, data, methodology | After thorough analysis |

**The reading triage:**

Not everything needs to be read thoroughly. Triage your reading:

1. Does this directly affect my current work? → Read thoroughly
2. Is this relevant to my role or domain? → Read strategically (abstract, conclusion, examples)
3. Is this background or general knowledge? → Skim or save for later
4. Is this irrelevant? → Skip

### The Four-Pass Reading Method

Adapted from the method for reading academic papers, this works for any technical text.

**Pass 1: The overview (5–10 minutes)**

- Read the title, abstract, and headings
- Look at diagrams, tables, and code examples
- Read the introduction and conclusion
- Goal: Decide whether to read further

**Pass 2: The comprehension (30–60 minutes)**

- Read the entire document sequentially
- Highlight unfamiliar terms and unclear sections
- Note questions and objections
- Mark key claims and evidence
- Goal: Understand what the author is saying

**Pass 3: The evaluation (20–30 minutes)**

- Re-read highlighted sections
- Check citations and references
- Evaluate claims against existing knowledge
- Identify assumptions and limitations
- Goal: Decide what to accept, question, or reject

**Pass 4: The synthesis (15–30 minutes)**

- Write a one-paragraph summary in your own words
- Note how this connects to other knowledge
- Identify open questions and action items
- Apply insights to your work
- Goal: Integrate the knowledge into your thinking

### Evaluating Technical Claims

**The evidence hierarchy:**

| Strength | Type | Example |
|---|---|---|
| Strongest | Empirical data with controls | A/B test with 10M users |
| | Peer-reviewed research | Published paper in reputable venue |
| | Reproducible benchmarks | Open-source benchmark with known methodology |
| | Production metrics | Before/after data from a real system |
| | Case studies | Detailed analysis of a real implementation |
| | Expert testimony | Subject matter expert with credentials |
| | Industry surveys | Large-scale surveys of practitioners |
| | Anecdotal experience | "In my experience..." |
| Weakest | Opinion | "I think..." |

**Questions to ask about evidence:**

1. **Is the evidence appropriate?** Don't use benchmarks to make security claims. Don't use anecdotes to make statistical claims.
2. **Is the evidence sufficient?** One data point is not a trend. One success story is not a methodology.
3. **Is the evidence current?** Technology evolves quickly. A benchmark from 2015 may not apply in 2025.
4. **Is the evidence relevant?** Claims about a system with 1M users may not apply to a system with 1K users.
5. **Is the evidence complete?** Did they include failure cases? Error bars? Negative results?

**Cognitive biases in technical reading:**

| Bias | Description | Mitigation |
|---|---|---|
| Confirmation bias | Accepting claims that confirm existing beliefs | Actively seek disconfirming evidence |
| Authority bias | Trusting claims because of the source's reputation | Evaluate the evidence, not the author |
| Recency bias | Giving more weight to recent information | Check historical context |
| Survivorship bias | Only seeing successful examples | Look for failures and negative results |
| Consistency bias | Expecting all evidence to be consistent | Acknowledge contradictory evidence |

### Reading Academic Papers

Academic papers have a standard structure that, once understood, makes reading more efficient:

**The IMRaD structure:**

| Section | Purpose | What to look for |
|---|---|---|
| Title | Succinctly describes the work | Does it match my interest? |
| Abstract | Summary of the entire paper | Key findings, methodology, contribution |
| Introduction | Context and problem statement | Gap they're filling, why it matters |
| Related Work | Prior research | How this builds on (or differs from) existing work |
| Method | How they did it | Is the approach valid? Reproducible? |
| Results | What they found | Data, metrics, statistical significance |
| Discussion | Interpretation of results | Limitations, implications |
| Conclusion | Summary and future work | Main takeaways |

**Reading a paper efficiently:**

1. **Abstract + conclusion first.** Decide if the paper is worth reading.
2. **Look at figures and tables.** They often communicate the key results more efficiently than text.
3. **Read the introduction.** Understand the problem and contribution.
4. **Read the discussion.** Understand limitations and implications.
5. **Read the method.** Only if you need to evaluate or reproduce the work.

**The paper-to-code pipeline:**

When reading a systems paper that describes a new technique:

1. Understand the problem and claimed improvement
2. Identify the key algorithmic or architectural contribution
3. Map the paper's terminology to your domain
4. Consider: can this be implemented in my context?
5. Prototype the core idea to validate the claims

**Where to find papers:**

| Resource | Focus | Access |
|---|---|---|
| arXiv.org | Computer science preprints | Free |
| ACM Digital Library | CS conference and journal papers | Subscription |
| IEEE Xplore | Engineering papers | Subscription |
| Google Scholar | Interdisciplinary search | Free |
| Papers With Code | Papers with code repositories | Free |
| Semantic Scholar | AI-powered paper discovery | Free |

### Reading RFCs & Standards

RFCs (Requests for Comments) define internet standards. Reading them efficiently requires understanding their structure.

**RFC structure:**

| Section | Purpose |
|---|---|
| Status | Standard, Experimental, Historic, Informational |
| Abstract | Brief summary of the specification |
| Introduction | Context and motivation |
| Terminology | Definitions of key terms |
| Protocol specification | Formal description of the protocol |
| Security considerations | Security implications |
| IANA considerations | Registry updates |
| References | Related standards |

**RFC maturity levels:**

| Level | Meaning | Example |
|---|---|---|
| Proposed Standard | Stable, widely reviewed | RFC 791 (IP) |
| Internet Standard | Proven in production | RFC 793 (TCP) |
| Experimental | Not yet proven | Various |
| Informational | Not a standard, just information | RFC 1149 (IP over Avian Carriers) |
| Historic | Superseded or obsolete | RFC 760 (superseded by 791) |
| Best Current Practice | Recommended practices | BCP 14 (RFC 2119 key words) |

**Key RFC conventions:**

- **RFC 2119 key words:** MUST, MUST NOT, REQUIRED, SHALL, SHALL NOT, SHOULD, SHOULD NOT, RECOMMENDED, MAY, OPTIONAL
  - MUST = required by the standard
  - SHOULD = recommended but not required
  - MAY = optional
- These words define the normative requirements of the standard
- Misreading them can lead to non-compliant implementations

**Reading an RFC efficiently:**

1. Read the abstract and introduction for context
2. Scan the terminology section for unfamiliar terms
3. Read the protocol specification section carefully
4. Read the security considerations (often overlooked but critical)
5. Check references for related standards

### Reading Specifications

Software specifications (language specs, API specs, protocol specs) are dense, precise documents designed for reference, not linear reading.

**Types of specifications:**

| Type | Example | Reading approach |
|---|---|---|
| Language specification | ECMAScript spec | Reference only; read when confused about behavior |
| API specification | OpenAPI spec | Machine-readable; tools generate documentation |
| Protocol specification | HTTP/1.1 spec | Read relevant sections when implementing |
| Data format specification | JSON spec | Read once; it's short and stable |
| Interface specification | USB spec | Read relevant sections for implementation |

**The spec reading strategy:**

1. Do not read a specification cover-to-cover (unless it's very short)
2. Use the specification to answer specific questions
3. Look for normative language (MUST, SHOULD, MAY) to understand requirements
4. Check examples for intended behavior
5. Look for notes and edge cases
6. Compare with implementations (bugs may be in either the spec or the implementation)

**Specification vs. implementation:**

A specification says what should happen. An implementation says what does happen. When they differ:

- The implementation may be wrong (doesn't follow the spec)
- The spec may be wrong (describes an impossible or undesirable behavior)
- The spec may be ambiguous (missed an edge case)
- The implementation may have discovered an edge case not covered by the spec

When reading: note discrepancies between spec and implementation. They are important.

### Reading Code as Text

Code is a form of technical communication. Reading code is a critical reading skill.

**Code reading goals:**

| Goal | Approach |
|---|---|
| Understand what a function does | Read the function body, tests, and callers |
| Find the source of a bug | Binary search through commit history, inspect diffs |
| Learn a new codebase | Start with the entry point, follow the flow |
| Review a change | Read the diff with context (not just the changed lines) |
| Assess code quality | Read for consistency, naming, test coverage |

**The code reading technique:**

1. **Read the signature first.** Inputs, output, side effects, errors
2. **Read the tests.** Tests document expected behavior better than comments
3. **Read the function body.** Follow the control flow
4. **Read the callers.** Understand how the function is used
5. **Read the history.** Git blame shows when and why the code changed

**Reading diffs effectively:**

- Understand the context (what file, what function, what change)
- Look for what changed, not just what the new code does
- Check for edge cases the diff might miss
- Verify tests cover the change
- Consider the diff as a narrative: what story does it tell?

### Synthesis: Connecting Multiple Sources

Synthesis is the most valuable reading skill — connecting information across multiple sources to form a coherent understanding.

**The synthesis process:**

1. **Collect.** Gather relevant sources (papers, blog posts, documentation, code)
2. **Extract.** Extract key claims, evidence, and conclusions from each
3. **Compare.** Identify agreements, contradictions, and gaps
4. **Connect.** Map relationships between sources (this builds on that, this contradicts that)
5. **Integrate.** Form a unified understanding that accounts for all sources
6. **Apply.** Use the synthesized knowledge to make decisions or take action

**Synthesis tools:**

| Tool | Use case |
|---|---|
| Zettelkasten (note-taking system) | Linking atomic notes across sources |
| Obsidian | Graph-based knowledge management |
| Roam Research | Bidirectional linking of ideas |
| Notion | Structured knowledge base |
| Mind maps | Visual organization of relationships |
| Comparison tables | Structured comparison of multiple sources |

**Synthesis example:**

You read:
- Blog post A: "Microservices improve deployment frequency"
- Paper B: "Microservices increase operational complexity"
- Case study C: "Company X failed after adopting microservices"
- Case study D: "Company Y succeeded after adopting microservices"

Synthesis:
- Microservices trade operational complexity for deployment flexibility
- Suitability depends on organizational maturity and team size
- Companies with >50 engineers tend to benefit; <20 tend to struggle
- Requires investment in observability, CI/CD, and DevOps culture

### Knowledge Management Systems

Reading is wasted if you cannot retrieve what you learned. A personal knowledge management (PKM) system solves this.

**The atomic note principle:**

Each note captures one idea, linked to other notes:

```
Note: CAP Theorem

A distributed data store can only provide two of three guarantees:
- Consistency (every read gets the most recent write)
- Availability (every request gets a non-error response)
- Partition tolerance (system continues despite network failures)

Related notes:
- [[Eventual Consistency]]
- [[Distributed Database Tradeoffs]]
- [[PACELC Theorem]]
Source: Brewer's 2000 PODC keynote, Gilbert & Lynch 2002 proof
```

**Reading capture workflow:**

1. **While reading:** Highlight, annotate, mark questions
2. **After reading:** Write a one-paragraph summary in your own words
3. **Extract key insights:** Create atomic notes for each important concept
4. **Link to existing notes:** Connect to previous knowledge
5. **Tag for retrieval:** Use consistent tags (e.g., #databases, #distributed-systems)
6. **Schedule review:** Review notes periodically (spaced repetition)

**The Feynman technique for verification:**

If you cannot explain a concept in simple terms, you do not understand it well enough:

1. Write the concept at the top of a blank page
2. Explain it as if teaching it to someone with no background
3. Identify gaps — places where your explanation is unclear or incomplete
4. Go back to the source to fill gaps
5. Repeat until the explanation is clear and simple

### Common Reading Traps

**1. The authority trap:**

"Google does it this way, so it must be right."

Google's solutions are optimized for Google's scale and team. They may not apply to your context.

**2. The recency trap:**

"This blog post from last week describes the new hot tool."

New is not better. Evaluate based on evidence, not novelty.

**3. The "it worked for them" trap:**

"We should use MongoDB because Company X succeeded with it."

Company X's success with MongoDB was influenced by their specific data model, query patterns, team expertise, and operational maturity. These factors may not match your context.

**4. The single source trap:**

Learning about a topic from one source (one book, one tutorial, one blog post). Every source has biases and blind spots. Read multiple perspectives.

**5. The false consensus trap:**

"Everyone knows that microservices are the right architecture."

Perceived consensus is often just the loudest voices. Seek out dissenting opinions.

**6. The reading-completion trap:**

"I must read every word of this paper."

No. Read strategically. Skip sections that are not relevant to your goal.

**7. The highlight-only trap:**

Reading with a highlighter but never reviewing highlights. Highlights are not understanding. Review and synthesize what you read.

**8. The note-taking trap:**

Taking extensive notes but never reviewing them. The act of note-taking helps comprehension, but the value compounds with review.

## Study Cases

### Case 1: Misreading a Configuration Guide

A developer read "The timeout is configurable via the TIMEOUT environment variable" and set `TIMEOUT=5000` expecting 5000 milliseconds.

The actual unit: seconds. The system timed out after 5000 seconds (83 minutes). Production impact: none (it was staging), but the feature was assumed broken for hours.

What went wrong: The documentation did not specify units. The developer assumed milliseconds (common in most systems). Critical reading would have flagged the missing unit information.

**The fix:** Critical readers ask about units, defaults, and edge cases. When in doubt, verify against tests or source code.

### Case 2: The Paper That Changed an Architecture

A team was designing a distributed queue system. They read the Amazon SQS documentation and the DynamoDB paper.

**From the SQS docs:** At-least-once delivery, exactly-once processing requires idempotent consumers.

**From the DynamoDB paper:** Eventual consistency, quorum-based reads for strong consistency.

The team synthesized:
- The queue might deliver duplicate messages (SQS behavior)
- Idempotency is the consumer's responsibility
- They needed a deduplication layer with strong consistency guarantees

This synthesis led to an architectural decision: use a database with strong consistency (not DynamoDB's eventual consistency) for the deduplication store. Without the synthesis of multiple sources, they might have used DynamoDB eventual consistency and seen duplicate processing.

### Case 3: The RFC That Saved a Protocol Implementation

A team implementing a WebSocket client used a blog post as their primary reference. The implementation worked for basic cases but failed on edge cases (large frames, fragmented messages, extension negotiation).

After reading RFC 6455 (WebSocket Protocol), they discovered:
- The blog post omitted mandatory frame masking
- Frame fragmentation had specific rules about control frames
- Extension negotiation required specific header fields

The cost of not reading the RFC: weeks of rework, edge case bugs in production, and security concerns.

**Lesson:** For protocol implementations, always read the specification (RFC). Secondary sources may omit critical details.

### Case 4: Misinterpreting Benchmarks

A team chose Database A over Database B based on a published benchmark showing Database A had 3x higher throughput.

Months into production, they discovered:
- The benchmark tested a workload (100% reads) that didn't match their workload (50% writes)
- Database A's performance degraded significantly under write-heavy workloads
- Database B would have been a better choice for their mixed workload

**What went wrong:** They read the benchmark result but not the benchmark methodology. They assumed the benchmark was relevant to their use case without verifying.

**Critical reading approach:** When evaluating benchmarks:
1. What workload was tested? (read/write mix, data size, concurrency)
2. What hardware was used? (How does it compare to ours?)
3. What version was tested? (Performance changes significantly between versions)
4. Who ran the benchmark? (Vendor benchmarks may be biased)
5. Was the methodology published? (Can we reproduce it?)

## Examples

### Example 1: Triage in practice

Engineer needs to understand message queue options for a new service.

Reading triage:
1. **Directly relevant (read thoroughly):** SQS and RabbitMQ documentation — these are the options being evaluated
2. **Relevant domain (read strategically):** Papers on distributed messaging (Kafka paper, Pulsar paper) — background knowledge
3. **Background (skim):** General articles on message queuing patterns — already familiar
4. **Irrelevant (skip):** Articles on event sourcing in frontend applications

### Example 2: Applying the four-pass method

Reading "The Log: What Every Software Engineer Should Know" (LinkedIn paper on Kafka):

**Pass 1 (5 min):** Title + abstract → it's about the log as a universal abstraction for distributed systems. Diagrams show producers, consumers, and a replicated log. Conclusion emphasizes simplicity and performance. Worth reading.

**Pass 2 (45 min):** Sequentially read. Key concept: the log is an append-only sequence of records. Consumer offsets track position. Log compaction for retention. Partitioning for parallelism.

**Pass 3 (20 min):** Re-read the compaction section — it was unclear. Check references: cites Chandy-Lamport, Spanner, and Bigtable papers. Identify assumption: log-based designs work well for event-sourced systems but less well for stateful systems needing random access.

**Pass 4 (15 min):** Summary: "A log-based architecture provides durability, ordering, and replication for distributed systems. Kafka implements this with partitioned logs and consumer offsets. The trade-off is that random access is expensive." Action item: apply log compaction pattern to the audit trail service.

### Example 3: Evidence evaluation

Claim: "GraphQL is always faster than REST for mobile clients."

Critical analysis:
- **Evidence?** None provided. Opinion, not data.
- **Appropriate?** "Always" is an absolute claim. Very few things are always true.
- **Sufficient?** No data. One counterexample would disprove.
- **Relevant?** Performance depends on query complexity, network conditions, data model.
- **Complete?** Ignores caching, batching, and compression benefits of REST.

Conclusion: Reject as unsupported. Seek actual benchmarks for the specific use case.

### Example 4: Reading a specification

OpenAPI spec excerpt for a user API:

```yaml
/users/{id}:
  get:
    summary: Get user by ID
    parameters:
      - name: id
        in: path
        required: true
        schema:
          type: string
    responses:
      '200':
        description: User object
      '404':
        description: User not found
```

Critical reading questions:
- What happens if the ID format is invalid? (Not specified — check error handling docs)
- Is the response format documented? (Not in this excerpt — check the response schema)
- Are there rate limits? (Not in the spec — check elsewhere)
- What authentication is required? (Not shown — check security section)

The spec is a contract, but it may be incomplete. The critical reader identifies what is not specified.

## Glossary

| Term | Definition |
|---|---|
| Abstract | Brief summary of a paper's content and contribution |
| Authority bias | Tendency to trust claims from perceived authorities without evaluating evidence |
| Confirmation bias | Tendency to accept information that confirms existing beliefs |
| Diataxis | Documentation framework: tutorials, how-to guides, reference, explanation |
| Feynman technique | Learning method: explain a concept in simple terms to identify gaps |
| IMRaD | Standard paper structure: Introduction, Method, Results, Discussion |
| Normative language | Terms (MUST, SHOULD, MAY) that define requirements in standards |
| PMF | Personal Knowledge Management — system for organizing personal knowledge |
| RFC | Request for Comments — internet standard document |
| Survivorship bias | Focusing on successful examples while ignoring failures |
| Synthesis | Connecting information from multiple sources to form a unified understanding |
| Zettelkasten | Note-taking system using atomic, linked notes |

## Quick References

- [How to Read a Paper (Keshav)](https://blizzard.cs.uwaterloo.ca/keshav/home/Papers/data/07/paper-reading.pdf) — the four-pass method
- [RFC 2119: Key Words for Requirements](https://www.rfc-editor.org/rfc/rfc2119) — normative language in standards
- [How to Read a Technical Specification](https://www.writethedocs.org/guide/) — Write the Docs guide
- [The Feynman Technique](https://fs.blog/feynman-technique/) — learning through explanation
- [Zettelkasten Method](https://zettelkasten.de/) — method for personal knowledge management
- [Google Scholar](https://scholar.google.com/) — academic paper search
- [Papers With Code](https://paperswithcode.com/) — papers with linked implementations
- [Semantic Scholar](https://www.semanticscholar.org/) — AI-powered paper discovery

## Next Steps

- [How to Read a Research Paper](../research-papers.md) — applying the four-pass method to academic papers
- [Reading RFCs & Internet Standards](../rfcs.md) — navigating protocol specifications
