# Effective Study Techniques

## Description

Dunlosky et al. (2013) evaluated ten learning techniques and found that most popular study strategies — rereading, highlighting, summarization — are among the least effective. The techniques with the strongest evidence base — practice testing and distributed practice — are also the ones students use least. This document introduces the evidence-based hierarchy of study techniques, explains the Feynman technique and Pomodoro method, and provides practical guidance for developers seeking to optimize their learning efficiency.

The disconnect between what students actually do and what the evidence recommends is striking. Rereading and highlighting dominate study behavior despite being rated low-utility, while practice testing and distributed practice — rated high-utility — are underutilized. This document bridges the gap by presenting each technique's evidence base, mechanism, and practical implementation for developers.

## Prerequisites

- [Memory and Forgetting — Basic](memory-and-forgetting-basic.md) — how memory encodes, stores, and retrieves information
- [Metacognitive Strategies — Basic](metacognitive-strategies-basic.md) — self-regulation as the overarching learning skill

The connection between these prerequisites and the current material is direct: memory research provides the theoretical foundation for why spaced practice and retrieval testing work, while metacognitive strategies provide the framework for selecting, monitoring, and evaluating the techniques described here. Without the metacognitive foundation, a developer may know about these techniques but be unable to implement them effectively — a common failure mode where knowledge does not translate into practice.

## Table of Contents

- [The Dunlosky Review: A Hierarchy of Techniques](#the-dunlosky-review-a-hierarchy-of-techniques)
- [High-Utility Techniques](#high-utility-techniques)
- [Moderate-Utility Techniques](#moderate-utility-techniques)
- [Low-Utility Techniques](#low-utility-techniques)
- [The Feynman Technique](#the-feynman-technique)
- [The Pomodoro Technique](#the-pomodoro-technique)
- [Building an Effective Study System](#building-an-effective-study-system)

The techniques covered in this document span a wide range of effectiveness, from the highly effective (practice testing, distributed practice) to the surprisingly ineffective (rereading, highlighting). Understanding this hierarchy is the first step toward designing a study system that actually works — one built on evidence rather than habit.

## The Dunlosky Review: A Hierarchy of Techniques

### The Study

In 2013, John Dunlosky and colleagues published "Improving Students' Learning With Effective Learning Techniques" in *Psychological Science in the Public Interest*. The review evaluated ten learning techniques based on the breadth and quality of the evidence base, assigning each a utility rating of High, Moderate, or Low.

The review is the most comprehensive assessment of study techniques in the educational psychology literature. Its conclusions are clear: the techniques that work best are not the ones that feel most comfortable, and the techniques that feel most productive are often the least effective.

**Methodology:** The review team evaluated each technique across multiple dimensions: the number of laboratory and field studies, the range of populations and materials studied, the consistency of findings across studies, and the degree to which the research reflects authentic learning conditions. Each technique was then assigned a utility rating based on this comprehensive evaluation.

**Why this review matters for developers:** The review was conducted on students, but the underlying mechanisms — retrieval practice, spacing, interleaving — are general cognitive principles that apply to any learning context, including professional development. The specific implementation may differ (Anki for syntax, practice problems for algorithms, project building for frameworks), but the principles are universal.

### The Full Rating

| Technique | Utility Rating | Key Finding |
|-----------|---------------|-------------|
| Practice testing | **High** | Benefits all ages, materials, and contexts |
| Distributed practice | **High** | Overwhelming evidence across all variables |
| Elaborative interrogation | Moderate | Benefits generalize in some contexts; limited in others |
| Self-explanation | Moderate | Promising but implementation varies |
| Interleaved practice | Moderate | Dramatic effects in math; less evidence for comprehension |
| Summarization | Low | Inconsistent quality; requires extensive training |
| Highlighting | Low | No evidence of benefit over passive review |
| Keyword mnemonic | Low | Limited to imagery-friendly materials |
| Imagery for text | Low | Limited to imagery-friendly materials |
| Rereading | Low | Students' most common strategy; minimal benefit for retention |

## High-Utility Techniques

### Practice Testing

Practice testing — any form of self-testing (flashcards, practice exams, free recall, fill-in-the-blank) — is the single most effective study technique in the Dunlosky review.

**Why it works:**
- Strengthens retrieval pathways through active use.
- Provides metacognitive feedback (revealing what you know versus what you only recognize).
- Reduces test anxiety through familiarity with the retrieval process.
- Combines retrieval practice with the testing effect documented by Roediger and Karpicke (2006).

**The testing effect in detail:** Roediger and Karpicke (2006) demonstrated that students who studied a passage and then took a practice test remembered more after a one-week delay than students who studied the passage twice. The critical finding was that the benefit of testing increased over time: immediately after studying, the restudy group performed slightly better, but after a delay, the test group significantly outperformed. This demonstrates that retrieval practice strengthens long-term retention even when it feels less productive than re-reading.

**Implementation for developers:**
- Flashcards (Anki, physical cards) for facts, syntax, and concepts.
- Practice problems before checking solutions.
- Brain dumps: close documentation and write everything you remember.
- Teaching others (requires retrieval and articulation).
- After reading a section of a technical book, close it and write a summary from memory.
- Before starting a coding session, attempt to recall the API you will use without looking at documentation.

**Common mistakes with practice testing:**
- Testing with the source material open (this becomes recognition, not retrieval).
- Abandoning a flashcard that is difficult (difficult cards are the most valuable — they indicate the material that most needs strengthening).
- Testing only what you already know (comfort-focused testing provides false positive feedback).

### Distributed Practice

Distributed practice — spreading study sessions over time rather than cramming — is the second high-utility technique. The evidence is overwhelming: distributed practice robustly outperforms massed practice across all variables tested.

**Why it works:**
- Forgetting between sessions creates desirable difficulty that strengthens subsequent retrieval.
- Each review increases memory stability, extending the interval before the next review is needed.
- The spacing effect (Ebbinghaus, 1885) has been replicated for over a century.

**The spacing effect in detail:** Ebbinghaus (1885) first demonstrated that distributed practice produces more durable memories than massed practice using himself as the sole subject, memorizing lists of nonsense syllables. Over a century of subsequent research has confirmed and extended this finding: distributed practice improves retention of factual knowledge, conceptual understanding, procedural skills, and problem-solving ability across age groups, materials, and contexts.

The optimal spacing interval depends on the desired retention period and the difficulty of the material. Cepeda et al. (2008) conducted a large-scale analysis and found that the optimal interval for a single review was approximately 10-20% of the desired retention period. For material you want to remember for 30 days, the optimal first review interval is approximately 3-6 days.

**Implementation for developers:**
- Use a spaced repetition system (Anki, SuperMemo) for ongoing knowledge maintenance.
- Schedule regular review sessions rather than cramming before deadlines.
- Revisit a language or framework after increasing intervals (1 day, 3 days, 1 week, 1 month).
- For interview preparation, begin spaced practice at least 4-6 weeks before the interview.
- Maintain a "learning log" that tracks what you studied and when, enabling you to schedule reviews before forgetting occurs.

**The forgetting is the feature.** When you struggle to recall something during a spaced review, the effort strengthens the memory more than easy re-study would. The discomfort is a signal that learning is happening.

## Moderate-Utility Techniques

### Elaborative Interrogation

Asking "why is this true?" for each factual statement. Pressley et al. (1992) found that students using elaborative interrogation learned 72% more facts than those using rote strategies.

**Mechanism:** Generating an explanation creates deeper encoding than passive reading. Even imperfect explanations are superior to no explanation because the generation process forces engagement with the material's causal structure.

**Elaborative interrogation in detail:** When you ask "why?" about a fact, you generate an explanation that connects the new information to your existing knowledge. This connection creates additional retrieval pathways and makes the information more accessible in the future. The quality of the explanation matters less than the act of generating it — even incorrect explanations produce learning benefits because the generation process itself deepens encoding.

Dunlosky et al. (2013) rated elaborative interrogation as moderate-utility because its effectiveness depends on the learner's existing knowledge. For factual knowledge in well-structured domains (science, technology), it is highly effective. For material that requires understanding complex relationships or abstract principles, its effectiveness is more limited.

**Implementation for developers:**
- After reading a documentation statement, ask "Why is this API designed this way?"
- When learning a design pattern, ask "Why does this pattern solve the problem?"
- When studying a framework, ask "Why does the framework make this decision for me?"
- After learning a command, ask "Why does this flag produce that behavior?"
- When encountering a bug fix, ask "Why does this change resolve the issue?"
- Maintain a "why journal" where you record the explanations you generate for technical concepts.

### Self-Explanation

Explaining material to yourself during study. Chi et al. (1989) showed that students who self-explained learned more than those who did not.

**Mechanism:** Self-explanation forces the learner to identify gaps in understanding, articulate connections between concepts, and generate inferences that extend beyond the source material.

**Self-explanation in detail:** Chi et al. (1989) studied students learning physics by studying worked examples. Students who paused after each step and explained *why* that step was taken (self-explainers) significantly outperformed students who simply read through the examples. The self-explainers generated inferences, identified inconsistencies, and built mental models that transferred to new problems.

For developers, self-explanation is particularly valuable when studying code:

1. **Explain each line** — "This line destructures the props object to extract the `name` and `age` properties."
2. **Explain each decision** — "The author uses a Map here instead of an object because the keys are not strings."
3. **Explain the overall structure** — "This function handles three cases: empty input, single element, and multiple elements. The structure is a guard clause pattern."
4. **Explain connections** — "This utility function is similar to the one in the previous module, but it handles the edge case of null values."

Rittle-Johnson (2006) found that self-explanation was particularly effective when combined with comparison — comparing two different solutions to the same problem while explaining the differences between them.

### Interleaved Practice

Mixing different types of problems within a study session rather than blocking by type.

**Mechanism:** Discriminative contrast — switching between problem types forces the brain to identify which strategy applies to each problem, rather than executing the same procedure on autopilot.

**Interleaving in detail:** Rohrer and Taylor (2007) demonstrated that interleaved practice dramatically improved math test performance compared to blocked practice. Students who practiced math problems in mixed order scored 72% on a test, while students who practiced in blocked order scored only 38%. The reason: interleaved practice forces the learner to identify the problem type before selecting a strategy, which builds the discriminative skill needed on real tests where problem types are not labeled.

The effectiveness of interleaving depends on the discrimination required. For problems that look similar but require different strategies (e.g., different algorithm approaches), interleaving is highly effective. For problems that are clearly distinguishable, the benefit is smaller.

**Implementation for developers:**
- Alternate between different coding problem types (arrays, trees, graphs, dynamic programming).
- Mix vocabulary from different language categories.
- Alternate between different types of technical study (coding, system design, behavioral questions).
- When practicing debugging, alternate between different types of bugs (logic errors, runtime errors, performance issues).
- In Anki, mix cards from different topics rather than reviewing all cards from one topic at a time.

## Low-Utility Techniques

### Rereading

Rereading is the most common study strategy among students and one of the least effective. The problem is recognition fluency: rereading produces a sense of familiarity that is easily mistaken for genuine understanding. You recognize the material perfectly while being unable to retrieve it from memory.

**The distinction between recognition and recall** is critical. Recognition is identifying information when presented with it; recall is retrieving information without cues. Rereading strengthens recognition but does little for recall. Exams, interviews, and debugging sessions require recall, not recognition.

**Why rereading feels effective despite being ineffective:**

Rereading exploits a cognitive bias: the *mere exposure effect* (Zajonc, 1968). Repeated exposure to a stimulus increases familiarity and liking, creating a subjective sense of mastery. When you reread a documentation page for the third time, it feels deeply familiar — you can almost predict the next sentence. This familiarity is real, but it is recognition familiarity, not retrieval strength. The material feels known because you can recognize it when you see it, not because you can produce it from memory.

The practical implication: if you find yourself rereading material, stop and test yourself instead. Close the documentation and write what you remember. If you can produce the information from memory, you have learned it. If you cannot, the rereading was giving you false confidence.

### Highlighting

Highlighting produces no measurable benefit over passive review. The cognitive operations involved (reading, selecting a sentence, marking it) do not require deep processing. Highlighting creates an illusion of studying — the learner feels productive while performing a cognitively shallow activity.

**Why highlighting fails:**

The problem with highlighting is not that it is harmful — it is that it is insufficient. Highlighting identifies material for later processing, but if no subsequent processing occurs, the highlighting has accomplished nothing. The act of selecting a sentence and marking it does not require understanding, evaluation, or elaboration. It requires only pattern recognition (identifying "important" sentences) and motor coordination (drawing a line).

Dunlosky et al. (2013) rated highlighting as low-utility because no study has demonstrated that highlighting alone produces meaningful learning gains compared to passive review. The only context in which highlighting provides value is as a first-pass organizational tool that identifies material for subsequent active processing.

If you must highlight, use it only as a first pass to identify material for subsequent active processing (retrieval practice, elaborative interrogation, or self-explanation). Highlighting alone is insufficient.

### Summarization

Summarization can be effective when performed well, but producing high-quality summaries requires extensive training that most learners have not received. Poor summaries miss key points or reproduce the source text superficially. The technique's inconsistent effectiveness across learners and materials places it in the low-utility category.

**Why summarization is rated low despite seeming useful:**

The problem is not that summarization is inherently ineffective — it is that most learners produce poor summaries. A high-quality summary requires the ability to identify main ideas, distinguish them from supporting details, compress information without losing essential meaning, and organize ideas coherently. These are sophisticated cognitive skills that require training.

Dunlosky et al. (2013) noted that the effectiveness of summarization depends heavily on the learner's ability and the quality of the summary produced. Students who received training in summarization techniques showed improved outcomes, but the average student does not receive such training.

**When summarization can be effective:**
- When combined with the Feynman technique (explaining in your own words is a form of high-quality summarization).
- When used as a tool for identifying gaps in understanding (attempting to summarize reveals what you do not understand).
- When the summary is compared to the source material and revised (this adds a self-explanation component).

**When summarization is ineffective:**
- When the summary reproduces the source text's language without processing the meaning.
- When the summary includes everything (no discrimination between main ideas and details).
- When the summary is produced immediately after reading without delay or retrieval.

## The Feynman Technique

The Feynman technique, derived from Richard Feynman's teaching philosophy, is a practical implementation of several evidence-based principles: active recall, elaborative processing, and self-explanation.

### The Four Steps

1. **Choose a concept** — select a specific idea, technique, or system you are trying to learn.
2. **Explain it in simple language** — write or speak an explanation as if teaching a child or a non-technical colleague. Use no jargon. Use simple analogies.
3. **Identify gaps** — where did your explanation become vague, confused, or hand-wavy? These gaps reveal the boundaries of your understanding.
4. **Review and refine** — return to the source material to fill the gaps, then simplify the explanation further.

### Why It Works

The Feynman technique combines multiple evidence-based mechanisms:

- **Active recall** — generating an explanation from memory rather than reading it.
- **Elaborative processing** — creating simple analogies and connections deepens encoding.
- **Self-explanation** — articulating understanding forces identification of gaps.
- **Metacognitive feedback** — the breakdown points in your explanation reveal what you do not actually understand.

Chi et al. (1989) demonstrated that self-explanation produces superior learning outcomes. The Feynman technique is a structured form of self-explanation applied to conceptual understanding.

### Implementation for Developers

After reading documentation or a technical article:
1. Close the source material.
2. Write a short explanation of the concept in your own words (no copy-pasting).
3. Compare your explanation to the source. Where did you miss details? Where were you vague?
4. Revise and repeat until the explanation is complete and accurate.

This can be done in a notebook, a personal wiki, or even a blog post. The format matters less than the generative act of producing the explanation from memory.

**Extended examples of the Feynman technique applied to developer learning:**

*Example 1: Understanding closures in JavaScript*
1. Choose: "I want to understand JavaScript closures."
2. Explain: "A closure is when a function remembers the variables from where it was created, even after that outer function has finished running. So if you have a function inside a function, the inner function can still use the outer function's variables."
3. Identify gaps: "I said 'remembers' — but does the function actually store a copy? How does JavaScript handle this? What about garbage collection?"
4. Review and refine: Return to the documentation, learn about lexical scoping and the scope chain, then revise the explanation.

*Example 2: Understanding database indexing*
1. Choose: "I want to understand how database indexes work."
2. Explain: "An index is like a book's index — instead of reading the whole book to find a topic, you look it up in the index which tells you the page number. A database index stores a sorted list of values and pointers to the rows that contain those values."
3. Identify gaps: "What data structure is the index stored in? B-tree? Hash? When would you not want an index?"
4. Review and refine: Learn about B-trees, covering indexes, and index maintenance costs, then revise.

The Feynman technique is most effective when applied to concepts that feel understood but cannot be explained. If you cannot explain a concept simply, you do not understand it well enough — and the Feynman technique reveals this gap constructively.

## The Pomodoro Technique

### The Method

Francesco Cirillo developed the Pomodoro Technique in the late 1980s:

1. Choose a task.
2. Set a timer for 25 minutes (one "pomodoro").
3. Work on the task with full focus until the timer rings.
4. Take a 5-minute break.
5. After four pomodoros, take a longer break (15-30 minutes).

### What the Evidence Says

Biwer et al. (2022) reviewed approximately 30 published studies on the Pomodoro technique and related interval-based work methods:

- **Breaks reliably improve performance** versus unbroken work sessions exceeding 60-90 minutes.
- **The specific 25/5 ratio is not robustly superior** to other ratios (20/10, 45/15, 50/10). The benefits come from taking breaks and from having structure, not from the specific timing.
- **Effects are larger for cognitively demanding tasks** — precisely the kind of work that developers perform.

### Honest Assessment

The Pomodoro technique works because:
- **Breaks work** — they prevent cognitive fatigue and allow consolidation.
- **Structure works** — committing to a defined work period reduces procrastination and task-switching.
- **Time pressure works** — a finite interval creates urgency that sustains focus.

The specific parameters (25 minutes, 5 minutes) are one valid implementation among many. If a 45/15 rhythm works better for your tasks and attention span, use it. The principle (focused work + regular breaks) matters more than the specific timing.

### The Neuroscience of Breaks

Research on cognitive fatigue provides the theoretical foundation for the Pomodoro technique's break structure:

- **Attentional resource depletion** — sustained attention depletes a finite pool of cognitive resources. Breaks allow partial restoration of these resources. Ariga and Lleras (2011) demonstrated that brief diversions dramatically improved sustained attention performance.
- **Default mode network activation** — during rest, the brain's default mode network activates, consolidating recently processed information and generating spontaneous connections between ideas. This is the neural basis of the "shower insight" phenomenon.
- **Adenosine clearance** — prolonged cognitive effort increases adenosine levels in the brain, which promotes drowsiness and reduces cognitive efficiency. Breaks, especially those involving physical movement, facilitate adenosine clearance.
- **Ultradian rhythms** — the brain naturally cycles through periods of higher and lower alertness in approximately 90-minute cycles. Working with these cycles (rather than against them) produces more sustainable performance.

### Implementation for Developers

- Use Pomodoro for tasks that require sustained focus: coding, studying documentation, writing.
- Adjust the interval to match the task: 25 minutes for focused study, 45-50 minutes for deep coding sessions.
- The break is mandatory, not optional. Stand up, move, look at something distant. The break serves a cognitive function (consolidation, fatigue prevention), not merely a physical one.
- Do not use Pomodoro for tasks that are already engaging and flow-inducing. The technique is most valuable for tasks that require effortful focus.

**Customizing Pomodoro for different developer tasks:**

| Task Type | Recommended Interval | Rationale |
|-----------|---------------------|-----------|
| Code review | 25/5 | High cognitive switching; frequent context changes |
| Deep debugging | 45/15 or 50/10 | Requires sustained focus; breaking flow is costly |
| Documentation study | 25/5 | Passive activity; fatigue sets in quickly |
| Writing code | 45/15 or 50/10 | Flow state possible; shorter intervals interrupt productivity |
| Interview prep (mixed) | 25/5 | Variety of activities; interval aligns with switching |
| Learning a new tool | 25/5 | Novel material; high cognitive load; frequent breaks needed |

**Common Pomodoro mistakes:**
- Using the break to check email or social media (this is not rest — it is a different cognitive load).
- Skipping breaks during "productive" sessions (this defeats the consolidation function).
- Using Pomodoro for every activity, including those that naturally flow (forcing breaks during flow states reduces productivity).

## Building an Effective Study System

The most effective study system combines multiple high-utility techniques into a coherent workflow:

### For Learning New Technology

1. **Pre-study planning** — identify what you need to learn and why (metacognitive forethought).
2. **Worked examples** — study complete, correct code examples before writing code (cognitive load theory).
3. **Active recall** — close the documentation and write what you remember (retrieval practice).
4. **Elaborative interrogation** — ask why each concept works the way it does (elaborative processing).
5. **Spaced review** — revisit the material at increasing intervals (distributed practice).
6. **Building** — apply the knowledge in a real project (generative learning).
7. **Self-evaluation** — after the project, assess what you learned and what remains unclear (metacognitive reflection).

**Common learning path for a new technology:**

| Stage | Duration | Primary Activity | Key Technique |
|-------|----------|-----------------|---------------|
| Orientation | 1-2 hours | Overview reading, "hello world" | Pre-training, worked examples |
| Foundation | 1-3 days | Core concept study, small examples | Active recall, elaborative interrogation |
| Application | 3-7 days | Build a small project | Generative learning, project-based |
| Deepening | 1-2 weeks | Complex features, edge cases | Interleaving, Feynman technique |
| Consolidation | Ongoing | Anki, periodic review | Spaced repetition, distributed practice |

The stages are not strictly sequential — there is natural overlap and iteration. However, the general progression from orientation through application to consolidation mirrors the evidence-based learning path from novice to competent user.

### For Interview Preparation

1. **Interleaved practice** — mix problem types rather than blocking by category.
2. **Spaced flashcards** — use Anki for system design concepts, algorithmic techniques, and behavioral examples.
3. **Retrieval practice** — solve problems without looking at solutions; check only after attempting.
4. **Feynman technique** — explain your approach aloud as you solve (articulation + monitoring).
5. **Simulated conditions** — practice under time pressure to build the relevant performance skills.

### For Ongoing Knowledge Maintenance

1. **Daily Anki** — 10-15 minutes of spaced repetition for facts, syntax, and commands.
2. **Weekly review** — revisit recent learnings and connect them to existing knowledge.
3. **Monthly project** — build something small using recently learned technology.
4. **Teaching** — write a blog post, give a talk, or explain a concept to a colleague.

**The compounding effect of consistent practice:**

An effective study system produces results that compound over time. The developer who spends 30 minutes per day on deliberate practice (15 minutes of spaced repetition + 15 minutes of active recall or elaborative interrogation) covers approximately 182 hours per year. This is equivalent to more than four full work weeks of dedicated learning. Spread across 50 work weeks, it represents approximately 3.6 hours per week — a manageable commitment that produces substantial cumulative knowledge.

The key is consistency, not intensity. A developer who studies for 30 minutes every day will outperform a developer who studies for 4 hours once per week, even though the total time is identical. The spaced, distributed nature of daily practice produces stronger encoding than the massed, crammed nature of weekly marathons.

## Learning Tips

- The strategies that work best (retrieval practice, interleaving, spacing) feel harder during study. This is desirable difficulty — the discomfort is the mechanism of learning, not a sign that the strategy is failing.
- Start with one or two techniques and add others gradually. Attempting to implement every technique simultaneously creates extraneous cognitive load that undermines the very learning you are trying to improve.
- Track which combinations of techniques produce the best outcomes for your specific learning contexts. Metacognitive awareness is the meta-skill that optimizes everything else.
- The most common mistake developers make is treating study as a passive activity. Reading documentation, watching tutorials, and reviewing code are all potentially passive activities unless they involve active retrieval, elaboration, or generation. The question to ask is not "Did I spend time studying?" but "What cognitive operations did I perform during that time?"
- Technique effectiveness is not universal — it depends on the material, the learner, and the context. Use the Dunlosky ratings as starting guidelines, not as absolute rules. If summarization works well for you in a particular context, continue using it — the evidence describes average effects across populations, not guaranteed effects for individuals.
- When implementing a new technique, give it at least 2-4 weeks of consistent use before evaluating its effectiveness. Most techniques require time to produce visible results, and premature abandonment prevents the technique from reaching its full potential.

## Glossary

| Term | Definition |
|------|------------|
| Practice testing | Self-testing through flashcards, practice exams, or free recall — the highest-utility study technique |
| Distributed practice | Spacing study sessions over time rather than massing them together |
| Elaborative interrogation | Asking "why is this true?" to generate deeper processing |
| Self-explanation | Explaining material to yourself during study to identify gaps |
| Interleaved practice | Mixing different problem types within a study session |
| Rereading | Re-reading material — the most common but one of the least effective strategies |
| Feynman technique | A four-step process of choosing, explaining, identifying gaps, and refining understanding |
| Pomodoro technique | A time-management method using focused work intervals separated by breaks |
| Desirable difficulty | A learning condition that feels harder during practice but produces better long-term outcomes |
| Recognition fluency | The sense of familiarity produced by re-encountering material — often mistaken for genuine understanding |
| Mere exposure effect | Repeated exposure to a stimulus increases familiarity, creating a sense of mastery without genuine learning |
| Testing effect | The finding that retrieval practice produces stronger long-term retention than additional study |

## Quick References

- Dunlosky, J. et al. (2013). "Improving Students' Learning With Effective Learning Techniques." *Psychological Science in the Public Interest*, 14(1), 4-58 — the definitive review
- Dunlosky, J. (2013). "Strengthening the Student Toolbox." *American Educator*, Fall 2013 — accessible summary for practitioners
- Biwer, F. et al. (2022). "Fostering Effective Learning Strategies in Higher Education" — review of structured work methods
- Chi, M. T. H. et al. (1989). "Self-Explanations: How Students Study and Use Examples in Learning to Solve Problems" — self-explanation research
- Roediger, H. L. & Karpicke, J. D. (2006). "Test-Enhanced Learning." *Psychological Science*, 17(3), 249-255 — the testing effect
- Cepeda, N. J. et al. (2008). "Spacing Effects in Learning." *Psychological Science*, 19(11), 1095-1102 — optimal spacing intervals
- Rohrer, D. & Taylor, K. (2007). "The Shuffling of Mathematics Problems Improves Learning." *Instructional Science*, 35(6), 481-498 — interleaving evidence

## Next Steps

- [Effective Study Techniques — Intermediate](effective-study-techniques-intermediate.md) — desirable difficulty principle depth, combining techniques, and developer-specific workflows
- [Stages of Learning and Skill Acquisition — Basic](stages-of-learning-and-skill-acquisition-basic.md) — how skill develops from novice to expert
- [Cognitive Load Theory — Basic](cognitive-load-theory-basic.md) — how cognitive architecture constrains study technique effectiveness
