# Note-Taking & Knowledge Management

## Description

Reading technical content is wasted if you cannot retrieve and apply what you learned. This document covers active reading techniques, note-taking methods (Zettelkasten, Cornell, outline, mind maps), tools for managing technical knowledge, building a personal knowledge graph, and incorporating spaced repetition to retain what you learn over time.

## Prerequisites

- [Evaluating Sources & Claims](evaluating-sources.md) — evaluate content before taking notes on it
- [How to Read a Research Paper](research-papers.md) — the three-pass method integrates with note-taking workflows

## Table of Contents

- [Active Reading vs. Passive Reading](#active-reading-vs-passive-reading)
- [The Note-Taking Pipeline](#the-note-taking-pipeline)
- [Note-Taking Methods](#note-taking-methods)
- [Tools for Technical Knowledge Management](#tools-for-technical-knowledge-management)
- [Organizing Technical Knowledge](#organizing-technical-knowledge)
- [Code Snippets in Notes](#code-snippets-in-notes)
- [Linking Notes and Building a Knowledge Graph](#linking-notes-and-building-a-knowledge-graph)
- [Reviewing and Spaced Repetition](#reviewing-and-spaced-repetition)
- [Separating Consumption from Creation](#separating-consumption-from-creation)
- [Note-Taking for Different Content Types](#note-taking-for-different-content-types)
- [Developing a Personal Knowledge Management Workflow](#developing-a-personal-knowledge-management-workflow)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Active Reading vs. Passive Reading

Most developers read passively — they consume text without engaging with it. Active reading transforms the reading process by incorporating note-taking, questioning, and synthesis.

| Passive reading | Active reading |
|---|---|
| Read linearly from start to finish | Read with a purpose, skip and jump |
| No notes or annotations | Take notes, highlight, question |
| Accept claims at face value | Evaluate claims, check references |
| Move on without review | Review and synthesize |
| Forgetting rate: 80% within a week | Retention rate: 60-80% with review |
| Output: nothing | Output: notes, summaries, action items |

**The active reading cycle:**

```text
Preview -> Read -> Annotate -> Summarize -> Connect -> Review
   |         |         |           |            |         |
   v         v         v           v            v         v
Skim the   Read in   Mark key   Write a 1-3  Link to   Revisit
structure  detail    passages,  sentence    existing  notes
and goals            ask        summary in  notes.    periodically.
                     questions  your own    Identify
                     in the     words       gaps and
                     margins                questions
```

**Active reading techniques:**

**Marginalia:** Write questions, objections, and connections in the margins of the document or on sticky notes for physical books.

```text
Text: "The CAP theorem states that a distributed system cannot provide
       consistency, availability, and partition tolerance simultaneously."

Marginalia: "What about PACELC? Is this still accurate for modern systems?"
            "Connection: HBase chose consistency, Cassandra chose availability."
```

**The QEC method (Question-Evidence-Conclusion):**

For each section or document, identify:

- What question is the author trying to answer?
- What evidence do they present?
- What conclusion do they draw?

**The Feynman technique:**

After reading, write an explanation of the concept as if teaching it to someone with no background. Where your explanation is unclear, you have identified a gap in your understanding.

### The Note-Taking Pipeline

An effective note-taking system is a pipeline, not a single step.

**Stage 1: Capture (while reading)**

Capture raw notes during reading. These are not organized or structured.

```text
Raw notes for "The Log: What Every Software Engineer Should Know":

- The log (append-only sequence) is a universal abstraction for distributed systems
- Kafka uses partitioned logs for parallelism
- Consumer groups track their offset independently
- Log compaction retains latest value per key -> useful for cache rebuilds
- Used in: data integration, stream processing, CDC (change data capture)
- Q: How does log compaction handle deletes? (tombstone records)
- Connection to: event sourcing pattern, CQRS
```

**Stage 2: Process (after reading)**

Within 24 hours of reading, process raw notes into structured notes:

1. Clarify unclear points
2. Remove duplicate information
3. Add context and connections
4. Categorize and tag

**Stage 3: Organize (weekly)**

Weekly review of processed notes:

1. Move permanent notes into the knowledge base
2. Create links between related notes
3. Identify topics that need deeper research
4. Archive or discard notes that are no longer relevant

**Stage 4: Review (monthly)**

Monthly review of the knowledge base:

1. Browse by tags and connections
2. Identify knowledge gaps
3. Update outdated information
4. Remove or archive obsolete notes

**Stage 5: Create (ongoing)**

Use the knowledge base as input for creation — writing blog posts, designing systems, making decisions.

### Note-Taking Methods

Different methods suit different purposes. Choose based on your needs and preferences.

**Zettelkasten (Slip Box):**

The Zettelkasten method, developed by sociologist Niklas Luhmann, creates atomic, interconnected notes.

Principles:

1. **Atomicity:** Each note captures one idea
2. **Autonomy:** Each note is self-contained and understandable on its own
3. **Connection:** Each note links to related notes
4. **Association:** Notes are connected by context, not just hierarchy

Zettelkasten note structure:

```markdown
# CAP Theorem

A distributed data store can provide only two of three guarantees:
- Consistency (every read receives the most recent write)
- Availability (every request receives a non-error response)
- Partition tolerance (system continues despite network failures)

## Notes
- Implication: when building distributed systems, you must choose
  which trade-off to make.
- The theorem assumes network partitions WILL happen.

## Links
- [[PACELC Theorem]] — extends CAP with latency vs. consistency trade-off
- [[Eventual Consistency]] — chose AP over CP
- [[Consensus Protocols]] — chose CP over AP

## Source
[Brewer's Keynote at PODC 2000]
[Gilbert & Lynch, "Brewer's Conjecture...", 2002]
```

**Cornell Method:**

The Cornell method divides each page into three sections:

```text
+--------------------------------------------------+
| Cue Column          | Note-Taking Column          |
| (Keywords,          | (Main notes from reading)  |
|  Questions,         |                             |
|  Prompts)           |                             |
|                     |                             |
| What is CAP?        | Brewer's theorem says you   |
|                     | can have at most 2 of: CP,  |
|                     | CA, AP.                     |
|                     |                             |
| What is PACELC?     | If partition -> choose C/A  |
|                     | Else -> choose L/C          |
|                     |                             |
+--------------------------------------------------+
| Summary (Bottom)                                  |
| CAP is the fundamental trade-off in distributed   |
| systems. PACELC extends it for normal operation.  |
+--------------------------------------------------+
```

Best for: Lecture notes, book notes, structured learning.

**Outline Method:**

Use hierarchical indentation to capture structure.

```text
1. Distributed Systems Fundamentals
   1.1 CAP Theorem
       - Consistency: all nodes see same data
       - Availability: every request gets response
       - Partition: network may fail
       - Choose 2 of 3
   1.2 PACELC Extension
       - If partition: C vs A
       - Else: L vs C
```

Best for: Well-structured content, technical specifications, documentation.

**Mind Maps:**

Visual representation with the central concept at the center and related ideas branching outward.

```
                          CAP Theorem
                             |
          +------------------+------------------+
          |                  |                  |
     Consistency       Availability      Partition Tolerance
          |                  |
     All reads       Every request
     return same     gets a response
     value           (non-error)
```

Best for: Brainstorming, visual learners, complex relationships.

**Progressive Summarization (Tiago Forte):**

Layer progressively denser summaries on top of notes:

```text
Layer 1: Original note (captured text)
Layer 2: Bold passages (most important parts)
Layer 3: Highlighted passages (key insight)
Layer 4: Executive summary (one paragraph)
Layer 5: Remix (original insight, new connections)
```

### Tools for Technical Knowledge Management

| Tool | Model | Strengths | Weaknesses |
|---|---|---|---|
| Obsidian | Local Markdown files | Fast, offline, extensible, graph view, free | Sync requires paid service |
| Notion | Cloud database | Rich formatting, databases, collaboration | Slow, vendor lock-in |
| Logseq | Local Markdown/Org files | Open source, local-first, outliner | Less polished UI |
| Roam Research | Outliner | Bidirectional linking, block-level references | Paid, vendor lock-in |
| Bear | Apple-only Markdown | Beautiful, fast | Apple only |
| VS Code + Foam | Code editor | Developer-native, git integration | No mobile, requires setup |

**Choosing a tool:**

| If you want... | Consider |
|---|---|
| Local files, no lock-in | Obsidian, Logseq, plain Markdown |
| Rich queries | Notion |
| Version control | Markdown in a git repo |
| Code editor integration | VS Code + Foam, Emacs Org-mode |

**Obsidian workflow for developers:**

```text
Vault structure:
technical-notes/
├── concepts/            # Atomic concept notes
│   ├── cap-theorem.md
│   └── raft-consensus.md
├── papers/              # Paper summaries
├── code/                # Code snippets and patterns
├── projects/            # Project-specific notes
└── daily/               # Daily notes (optional)
```

### Organizing Technical Knowledge

**Folder-based organization:**

Organize notes into a folder hierarchy that mirrors your knowledge domains.

```text
databases/
├── index.md
├── relational/
│   ├── postgresql/
│   │   ├── indexing.md
│   │   ├── vacuum.md
│   │   └── replication.md
│   └── transactions/
│       ├── acid.md
│       └── isolation-levels.md
└── nosql/
    ├── document-stores.md
    └── key-value-stores.md
```

**Tag-based organization:**

Tags cut across hierarchies and allow flexible retrieval.

```text
#databases #postgresql #performance  — for PostgreSQL indexing notes
#distributed-systems #consensus      — for Raft and Paxos notes
#security #authentication #tokens    — for JWT notes
```

**Hybrid organization:**

Most effective systems combine folders and tags:

```text
Folder: databases/postgresql/
Tags:   #performance, #indexing, #tuning
```

**The PARA method (Tiago Forte):**

Organize notes into four top-level categories:

| Category | Purpose | Example |
|---|---|---|
| Projects | Active outcomes with deadlines | "Build payment service", "Write blog post on CAP" |
| Areas | Ongoing responsibilities | "Databases", "Security", "Career" |
| Resources | Topics of interest | "Distributed systems", "Go programming" |
| Archives | Inactive items from above categories | Completed projects, old resources |

This method ensures that notes serve active work, not just passive accumulation.

### Code Snippets in Notes

Code snippets are a crucial part of technical note-taking. They represent concrete examples of concepts.

**Code note template:**

```markdown
# Pattern: Circuit Breaker

## Description
Prevents cascading failures by stopping calls to a failing service.

## Implementation (Go)

```go
type CircuitBreaker struct {
    failureCount int
    threshold    int
    state        State
}

func (cb *CircuitBreaker) Call(fn func() error) error {
    if cb.state == Open {
        return ErrCircuitOpen
    }
    err := fn()
    if err != nil {
        cb.failureCount++
        if cb.failureCount >= cb.threshold {
            cb.state = Open
        }
    }
    return err
}
```

## Usage
- Wrap external service calls
- Set threshold based on expected error rate
- Add half-open state for recovery

## Source
[Release It! by Michael Nygard]
```

**What to capture in code notes:**

| Element | Purpose |
|---|---|
| Problem | What does this code solve? |
| Dependencies | What libraries or frameworks are needed? |
| Setup | How to install and configure |
| Core code | The essential implementation |
| Edge cases | Error handling, boundary conditions |
| Usage example | How to call the code |
| Performance | Complexity, memory, trade-offs |

**Linking code to concepts:**

A code note should link back to the concept it implements:

```markdown
## Links
- [[Raft Consensus Algorithm]] — the consensus protocol this implements
- [[Leader Election]] — how the cluster selects a leader
- [[Log Replication]] — how entries are replicated across nodes
```

### Linking Notes and Building a Knowledge Graph

The power of a note-taking system comes from the connections between notes, not the notes themselves.

**Types of links:**

| Link type | Meaning | Example |
|---|---|---|
| Supports | Note A provides evidence for Note B | [[CAP Theorem]] supports [[Why NoSQL Exists]] |
| Contradicts | Note A challenges Note B | [[Optimistic Locking]] contradicts [[Pessimistic Locking]] |
| Extends | Note A is a specific case of Note B | [[Raft]] extends [[Consensus Protocols]] |
| Prerequisite | Note A must be understood before Note B | [[TCP]] is a prerequisite for [[HTTP]] |
| Example | Note A is an example of Note B | [[PostgreSQL MVCC]] is an example of [[MVCC]] |

**Bidirectional linking:**

In systems like Obsidian, Logseq, and Roam, links are bidirectional — each note shows both what it links to and what links to it.

```text
CAP Theorem
  - Links to: Eventual Consistency, PACELC Theorem, Quorum
  - Linked from: Distributed Systems Index, NoSQL Databases, Brewer's Paper Note
```

Bidirectional links make it easy to navigate from a note to its context.

**The knowledge graph:**

A collection of linked notes forms a knowledge graph. The graph reveals:

- **Clusters:** groups of strongly connected notes (a topic area)
- **Bridges:** notes that connect different clusters (interdisciplinary connections)
- **Orphans:** notes with no connections (gaps in your knowledge structure)
- **Hubs:** highly connected notes (central concepts in your understanding)

**Building the graph:**

1. Create atomic notes for each concept
2. Link each new note to at least two existing notes
3. When a note has no connections, it is an orphan — either connect it or discard it
4. Review the graph periodically to identify gaps

### Reviewing and Spaced Repetition

Knowledge decays without review. Spaced repetition schedules reviews at increasing intervals to maximize retention.

**The forgetting curve:**

```text
Retention
100% |*
  90% |  *                 Review 1 (1 day)
  80% |    * 
  70% |      *             Review 2 (3 days)
  60% |        *    
  50% |          *         Review 3 (7 days)
  40% |            *
  30% |              *     Review 4 (14 days)
  20% |                *
  10% |                  *
  ---+--------------------------- Time
     |  1d  3d  7d  14d  30d
```

Without review, you forget 80% of what you read within a week.

**Implementation strategies for spaced repetition:**

**Option 1: Use a dedicated tool (Anki)**

Create flashcards from your notes and review daily.

```markdown
Q: What does the CAP theorem state?
A: A distributed system can provide at most 2 of 3:
   Consistency, Availability, Partition Tolerance.
```

**Option 2: Manual review schedule**

1. Day 1: Read and take notes
2. Day 2: Review notes, write summary
3. Day 7: Review summary, check understanding
4. Day 30: Review again, connect to new knowledge
5. Day 90: Final review, archive or escalate

**Option 3: Random review**

Set a timer for 10 minutes each day. Open a random note from your knowledge base and review it.

**Reviewing effectively:**

- Do not just re-read. Recall from memory first, then check.
- Explain the concept out loud or in writing before looking at your notes.
- Identify what you have forgotten and focus on those gaps.
- Update notes with new connections and insights.

### Separating Consumption from Creation

One of the biggest traps in knowledge management is spending all your time consuming and never creating.

**The consumption trap:**

```text
Day 1:   Read 5 blog posts, save 20 highlights
Day 2:   Read 3 papers, add 15 notes
Day 3:   Read documentation, capture snippets
...
Day 30:  Have 500 notes but have not written or built anything
```

**The solution: output ratio**

Aim for at least 1 hour of creation for every 3 hours of consumption.

| Consumption | Creation |
|---|---|
| Reading a paper | Writing a summary |
| Watching a talk | Taking structured notes |
| Reading documentation | Building a small example |
| Reading code | Writing tests |

**Creation types:**

| Creation type | Effort | Value |
|---|---|---|
| Summary note | Low | Synthesize source into own words |
| Code example | Medium | Apply concept to concrete problem |
| Blog post | High | Explain concept to others |
| Talk or workshop | High | Deep understanding from teaching |
| Open-source contribution | High | Apply knowledge to real project |

**The connection note as creation:**

Even a single well-written connection note — linking two concepts — is a creative act. You have identified a relationship that did not exist in your notes before.

### Note-Taking for Different Content Types

**For papers and long-form content:**

1. Capture the citation (author, title, year, venue)
2. Write a one-paragraph summary
3. Extract the key contributions
4. Note the methodology and limitations
5. Identify connections to other knowledge
6. Rate: skip, skim, read, or reference

```markdown
## Paper: "MapReduce: Simplified Data Processing on Large Clusters"

**Citation:** Dean, J., & Ghemawat, S. (2004). OSDI.

**Summary:** MapReduce is a programming model that processes large
datasets by splitting work across a cluster. Users write Map and
Reduce functions; the framework handles distribution and fault
tolerance.

**Key Contributions:**
- Simple programming model hides complexity of distributed computing
- Automatic fault tolerance for commodity hardware
- Near-linear scalability

**Methodology:** Implemented on Google's cluster, evaluated with
word count, grep, and sort workloads up to 1TB.

**Limitations:** Not suitable for iterative algorithms, not optimized
for real-time processing.

**Connections:**
- Related to [[Functional Programming]] map/reduce concept
- Foundation for [[Hadoop MapReduce]] and [[Spark]]
- Contrast with [[Dryad]] (DAG-based approach)
```

**For RFCs and specifications:**

1. Record the RFC number, status, and date
2. Note the protocol or concept defined
3. Extract normative requirements (MUST, SHOULD, MAY)
4. Highlight security considerations

**For code and patterns:**

1. Describe the problem the pattern solves
2. Include a minimal code example
3. Note the trade-offs and alternatives

**For tutorials and how-to guides:**

1. Note the steps and the reasoning behind each step
2. Annotate with what could go wrong
3. Note the version of tools used

### Developing a Personal Knowledge Management Workflow

A PKM workflow is personal. What works for one developer may not work for another. Build your own by starting simple and iterating.

**The minimal viable PKM (start here):**

```text
Tool:     Plain Markdown files
Structure: One folder per topic, one file per concept
Linking:  File references (e.g., [cap-theorem.md])
Review:   Weekly 15-minute review of new notes
```

**The PKM maturity model:**

| Level | Description | Tools | Effort |
|---|---|---|---|
| 1. Capture | Save links and bookmarks | Bookmarks, read-later apps | Low |
| 2. File | Save files organized by topic | Folder-based Markdown | Low |
| 3. Note | Write atomic notes in own words | Obsidian, Notion | Medium |
| 4. Connect | Link notes bidirectionally | Obsidian, Logseq, Roam | Medium |
| 5. Graph | Navigate via knowledge graph | Obsidian graph view | Medium |
| 6. Review | Schedule spaced repetition | Anki + Obsidian | High |
| 7. Create | Produce output from knowledge | Blog, code, talks | High |

Move up the maturity model only when the current level is a consistent habit.

**The daily note habit:**

```text
Morning (5 min):  Review yesterday's notes. Set reading goal for today.
During day:       Capture ideas and quotes as they come.
Evening (10 min): Process daily captures. Write one atomic note.
Weekly (30 min):  Review new notes. Create links. File in permanent structure.
Monthly (1 hour): Browse knowledge graph. Identify gaps. Plan next month's reading.
```

**Habit tips:**

- Capture what is surprising, useful, or connects to existing knowledge — not everything.
- Process notes within 24 hours or they become stale.
- Every note should have at least one link. If a note has no links after 30 days, connect it or delete it.
- Your note-taking system is for retrieval, not storage.

## Structure
1. Prepare phase: coordinator asks all participants to prepare
2. Commit phase: if all prepared, coordinator asks all to commit

## Properties
- Atomic: all participants commit or all abort
- Blocking: if coordinator fails during commit, participants block
- Used in: distributed databases, XA transactions

## Limitations
- Blocking nature makes it unsuitable for long-running transactions
- Single point of failure (coordinator)
- Performance overhead of two rounds of messaging

## Links
- [[SAGA Pattern]] — alternative for long-running transactions
- [[CAP Theorem]] — 2PC sacrifices availability for consistency
- [[Distributed Transactions]] — broader context
```

### Example 2: Connection Note

A connection note links two concepts that might not traditionally go together:

```markdown
# Connection: CAP Theorem and Transaction Isolation Levels

- Serializable isolation provides strong consistency within a node
- CAP's consistency is about cross-node consistency
- Choosing isolation levels + replication strategy determines your
  position in the CAP trade-off space
```

### Example 3: Anki Flashcard from Notes

From a CAP theorem note:

```markdown
Q: What is the CAP theorem?
A: A distributed system can provide at most two of:
   Consistency, Availability, and Partition Tolerance.

Q: What is the PACELC extension?
A: If Partition exists, choose C vs A.
   Else (normal operation), choose Latency vs Consistency.
```

## Glossary

| Term | Definition |
|---|---|
| Active reading | Reading with intentional engagement, questioning, and note-taking |
| Atomic note | A note capturing exactly one idea, self-contained and linkable |
| Bidirectional link | A connection visible from both the source and target note |
| Cornell method | Note-taking system with cue, note, and summary sections |
| Feynman technique | Explaining a concept in simple terms to verify understanding |
| Forgetting curve | The exponential decay of memory over time without review |
| Knowledge graph | Network of interconnected notes showing relationships |
| Marginalia | Annotations written in the margins of a document |
| PARA | Organization method: Projects, Areas, Resources, Archives |
| Passive reading | Reading without active engagement or note-taking |
| PKM | Personal Knowledge Management — system for organizing personal knowledge |
| Progressive summarization | Layering denser summaries on top of source material |
| QEC method | Question-Evidence-Conclusion — identifying the structure of an argument |
| Spaced repetition | Reviewing information at increasing intervals for retention |
| Zettelkasten | Note-taking method using atomic, linked notes (German for "slip box") |

## Quick References

- [Zettelkasten Method](https://zettelkasten.de/) — detailed explanation of the method
- [How to Take Smart Notes (Ahrens)](https://www.amazon.com/How-Take-Smart-Notes-Nonfiction/dp/3982438802) — book on Zettelkasten
- [Obsidian](https://obsidian.md/) — Markdown-based knowledge management
- [Logseq](https://logseq.com/) — open-source, local-first knowledge management
- [Anki](https://apps.ankiweb.net/) — spaced repetition flashcard software
- [Andy Matuschak's Working Notes](https://notes.andymatuschak.org/) — example of a digital knowledge garden
- [Forte Labs: Building a Second Brain](https://fortelabs.com/) — PARA method and progressive summarization
- [The Archive](https://zettelkasten.de/the-archive/) — macOS Zettelkasten tool
- [Dendron](https://www.dendron.so/) — hierarchical note-taking for VS Code

## Next Steps

- [Documentation Architecture](../technical-writing/doc-architecture.md) — structuring documentation for teams, extending personal knowledge management to team-scale documentation
