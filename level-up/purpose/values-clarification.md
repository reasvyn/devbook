# Values Clarification

## Description

The process of discovering and articulating your core values — the invisible architecture that determines whether your life feels aligned or hollow. This is not a personality quiz. It is a confrontation with the question: what actually matters to you, as opposed to what you have been told should matter? Values are the foundation of purpose, and without clarifying them, every mission you build will rest on unstable ground.

## Prerequisites

- [Finding Your Mission](finding-your-mission.md) — the directional search that values clarify and support
- [The Mechanism of Change](../fundamentals/the-mechanism-of-change.md) — the interdependent drivers of awareness, agency, and action, including the layers of self-awareness

## Table of Contents

- [What Values Actually Are](#what-values-actually-are)
- [What Values Are Not](#what-values-are-not)
- [Intrinsic vs. Extrinsic Values](#intrinsic-vs-extrinsic-values)
- [Why Values Clarification Matters](#why-values-clarification-matters)
- [The Values You Think You Should Have](#the-values-you-think-you-should-have)
- [The Clarification Process: Reflection](#the-clarification-process-reflection)
- [The Clarification Process: Experimentation](#the-clarification-process-experimentation)
- [The Clarification Process: Confirmation](#the-clarification-process-confirmation)
- [The Hierarchy of Values](#the-hierarchy-of-values)
- [Core Values vs. Satellite Values](#core-values-vs-satellite-values)
- [Values Conflict](#values-conflict)
- [How to Test Whether a Value Is Truly Yours](#how-to-test-whether-a-value-is-truly-yours)
- [The Developer's Values](#the-developers-values)
- [Living by Values in a World That Rewards Other Things](#living-by-values-in-a-world-that-rewards-other-things)
- [Values as a Decision-Making Framework](#values-as-a-decision-making-framework)
- [A Walkthrough: Clarifying Your Values](#a-walkthrough-clarifying-your-values)
- [When Your Values Change](#when-your-values-change)
- [The Courage to Live by Your Values](#the-courage-to-live-by-your-values)

## Content / Material

### What Values Actually Are

Values are not goals. They are not destinations you arrive at. They are directions you travel in. A goal is "get promoted to staff engineer." A value is "mastery of craft." Goals can be achieved. Values are practiced. You never finish valuing something. You live it, daily, in ways large and small.

A value is a deeply held principle that guides your behavior and decisions. It is the thing you care about when no one is watching. It is the filter through which you evaluate opportunities, relationships, and choices. When a decision aligns with your values, it feels right — even when it is hard. When a decision violates your values, it feels wrong — even when it is rewarded.

Values are not opinions. Opinions can be changed with arguments. Values are experiential. They come from lived experience, from the accumulated evidence of what has mattered to you across your life. You do not choose your values the way you choose a preference. You discover them the way you discover your own heartbeat — by paying attention to what has always been there.

```python
class Value:
    def __init__(self, name, definition, evidence):
        self.name = name
        self.definition = definition
        self.evidence = evidence  # lived experiences that proved this matters

    def test(self, decision):
        """Does this decision align with the value?"""
        return self.evaluate(decision) == "aligned"

    def evaluate(self, decision):
        """Values do not tell you what to do.
        They tell you whether a choice fits who you are."""
        for e in self.evidence:
            if self.violates(e, decision):
                return "violated"
        return "aligned"

    def violates(self, experience, decision):
        """A value is violated when a decision contradicts
        the principle proven by lived experience."""
        return not decision.supports(experience.principle)
```

The code above is a metaphor, but the logic is real. Values are evidence-based beliefs about what matters. The evidence comes from your life — from moments when something felt deeply right or deeply wrong. The values clarification process is the process of collecting and interpreting that evidence.

### What Values Are Not

Confusion about values is common. Clearing the confusion is the first step.

Values are not rules. A rule is "always tell the truth." A value is "honesty." Rules are rigid and context-dependent. Values are flexible and universal. Honesty sometimes means telling the truth directly. Sometimes it means knowing when silence is more honest than words. The value guides the interpretation. The rule is the interpretation.

Values are not desires. A desire is "I want to be rich." A value is "security." The desire is for a specific outcome. The value is for a state of being. You can satisfy the desire without satisfying the value — a person with millions in the bank may feel financially insecure because security is not about the number, it is about the feeling. And you can satisfy the value without satisfying the desire — a person with modest means may feel deeply secure because they have enough and they know it.

Values are not virtues. A virtue is a character trait — courage, patience, kindness. A value is a principle — autonomy, contribution, truth. Virtues are how you behave. Values are why you behave that way. You might act with courage (virtue) because you value justice. The virtue is the expression. The value is the motivation.

Values are not the same as interests. An interest is "I like programming." A value is "craftsmanship." The interest is about an activity. The value is about the quality of engagement. You might lose interest in programming but never lose your value of craftsmanship — it would express itself through whatever work you do.

The confusion between values and the other categories — goals, rules, desires, virtues, interests — is the most common barrier to clarification. When you mistake a goal for a value, you feel empty when you achieve the goal. When you mistake a rule for a value, you feel rigid and judgmental. When you mistake a desire for a value, you feel perpetually unsatisfied. When you mistake a virtue for a value, you feel moralistic rather than principled. When you mistake an interest for a value, you feel lost when the interest fades.

The resolution is to always ask: is this about a specific outcome (goal), a rigid behavior (rule), a wished-for result (desire), a character trait (virtue), or a temporary engagement (interest)? If the answer to all of these is no, and the principle persists across contexts and time, it is a value.

### Intrinsic vs. Extrinsic Values

The distinction between intrinsic and extrinsic values is one of the most important distinctions in values work.

Intrinsic values are rewarding in themselves. Autonomy is intrinsically valuable — the experience of being self-directed is satisfying regardless of what you are directing yourself toward. Creativity is intrinsically valuable — the act of making something new is rewarding in itself. Connection is intrinsically valuable — the experience of being known and accepted is satisfying regardless of what you do together.

Extrinsic values are rewarding because of what they produce. Money is extrinsically valuable — it is a tool, not an end. Status is extrinsically valuable — it is a signal, not a substance. Recognition is extrinsically valuable — it is feedback, not fulfillment.

The distinction is not absolute. Some values sit on the boundary. Mastery can be intrinsically rewarding (the joy of getting better) and extrinsically rewarding (the career advancement that skill produces). The question is not whether a value is purely intrinsic or purely extrinsic. The question is which dimension dominates.

The research on values and wellbeing is unambiguous. People who prioritize intrinsic values report higher life satisfaction, stronger relationships, and greater psychological resilience. People who prioritize extrinsic values report lower satisfaction, weaker relationships, and greater vulnerability to anxiety and depression.

The reason is structural. Intrinsic values are satisfied by the process. You can practice autonomy every day, regardless of your circumstances. Extrinsic values are satisfied by outcomes. You cannot control outcomes. The dependence on uncontrollable outcomes creates a fragile sense of wellbeing.

This does not mean extrinsic values are bad. Money matters. Status has practical utility. The point is not to eliminate extrinsic values but to recognize their limits and to ensure they serve your intrinsic values rather than replacing them.

The relationship between intrinsic and extrinsic values is like the relationship between the engine and the fuel gauge. The engine — intrinsic values — powers the car. The fuel gauge — extrinsic values — tells you how much fuel is left. The fuel gauge is useful. But if you stare at the fuel gauge instead of driving the car, you have confused the indicator for the purpose. The purpose is the driving. The gauge is information.

In practice, this means building a life where your extrinsic pursuits are in service of your intrinsic values. You earn money — an extrinsic pursuit — to fund the autonomy and creativity — intrinsic values — that make life meaningful. The money is a tool. The autonomy and creativity are the ends. When the tool becomes the end, the inversion produces emptiness. The emptiness is the signal that the hierarchy has flipped.

### Why Values Clarification Matters

Without clarified values, you are building on sand. You may build something impressive — a career, a reputation, a body of work — but if it is not aligned with what you actually care about, the achievement will feel hollow. The hollowness is the signal of misalignment.

Values clarification matters for purpose because values are the foundation that purpose rests on. A mission that violates your core values will not sustain you. You will abandon it, or worse, you will follow it and lose yourself. The mission must serve the values. The values are the deeper layer.

Values clarification also matters because it simplifies decision-making. When your values are clear, decisions become easier. You do not need to analyze every option from every angle. You ask one question: does this serve what I care about? The answer is usually immediate. The clarity saves energy that would otherwise be consumed by deliberation.

The third reason values clarification matters is that it protects you from external pressure. When you do not know what you value, you are vulnerable to the values of others. You adopt the priorities of your employer, your culture, your peer group. You spend years pursuing things that matter to them but not to you. The clarification is a defense against this drift.

### The Values You Think You Should Have

The most dangerous values are the ones you have adopted from others without examination.

You think you should value ambition because the industry rewards ambition. You think you should value productivity because the culture celebrates productivity. You think you should value prestige because the people around you pursue prestige. The "should" is the tell. Values that come with "should" are not values. They are obligations.

The obligations are seductive because they come with social approval. Pursuing the values your culture rewards earns you status, belonging, and praise. The approval feels like validation. It is not. It is payment for compliance. The compliance costs you something real — the alignment between your behavior and your actual values.

The difference between a genuine value and a "should" value shows up in the body. When you act on a genuine value, you feel grounded. There is a quiet satisfaction. When you act on a "should" value, you feel hollow. There is a performance quality — you are doing the right thing, but it does not feel like yours.

The exercise is to notice the "should" values. Write them down. Then ask: if no one would ever know whether I pursued this value, would I still pursue it? If the answer is no, the value is borrowed. If the answer is yes, it might be real.

Another diagnostic is to look at what you actually do with your discretionary time. Your calendar and your browser history reveal your real values more honestly than any self-assessment. You say you value learning, but you spend your free time on social media. You say you value health, but you eat poorly and skip exercise. The discrepancy between stated values and observed behavior is the gap that values clarification closes. The closing requires honesty. The honesty is the hardest part — not because you cannot see the gap, but because seeing it means admitting that you have been living by values you do not actually hold.

### The Clarification Process: Reflection

Values clarification is a three-phase process: reflection, experimentation, and confirmation. The phases build on each other. Reflection generates hypotheses. Experimentation tests them. Confirmation solidifies them.

Reflection is the inward-looking phase. You examine your life for evidence of what matters. The evidence is in the patterns — the recurring themes, the consistent reactions, the moments of alignment and misalignment.

The practice of reflective journaling is one of the most effective tools for this phase. You write about specific moments when you felt most alive, most fulfilled, most like yourself. You also write about moments when you felt deadened, compromised, or wrong. The contrast reveals your values.

The prompts for reflection are specific. When was the last time you felt deeply satisfied with your work, and what about it was satisfying? When was the last time you felt resentful about something you were asked to do, and what about it felt wrong? What would you do if money were irrelevant? What would you refuse to do, even if you were paid handsomely?

The answers to these questions are data. They are not conclusions. The data must be examined for patterns before it becomes insight. The examination is the actual work of reflection. Without it, the answers are just stories. With it, the answers become evidence of what you truly care about.

The patterns may take time to emerge. A single journal entry is a snapshot. Multiple entries across weeks or months reveal the trends. The trends are the signal. The individual entries are the noise. The patience to collect enough entries to see the trends is the discipline of the reflection phase.

### The Clarification Process: Experimentation

Reflection produces hypotheses about your values. Experimentation tests them.

A values hypothesis is a statement like: "I think I value autonomy." The hypothesis is based on reflection — you noticed that you feel most alive when you are self-directed. But reflection alone is not enough. You need to test the hypothesis in the real world.

The experiment is simple. You structure a situation that tests the value. For autonomy, you might take on a project with full ownership — no check-ins, no approvals, no oversight. You observe what happens. Do you thrive? Do you feel liberated? Or do you feel lost, anxious, and in need of guidance?

The experiment does not need to be dramatic. Small tests are often more revealing. You decline a meeting that wastes your time. You propose a new approach at work without asking permission. You take a day to work on whatever you want. The micro-experiments generate data without requiring a major life change.

The key to experimentation is honest observation. You are not looking for what you want to find. You are looking for what is actually there. The temptation is to interpret every result as confirmation of your hypothesis. Resist the temptation. Disconfirmation is just as valuable as confirmation. If you hypothesize that you value autonomy and the experiment reveals that you feel anxious without structure, the disconfirmation is a discovery. Maybe you value collaboration more than you realized.

### The Clarification Process: Confirmation

Confirmation is the phase where reflection and experimentation converge. You have reflected on your life for evidence. You have experimented with your hypotheses. Now you integrate the findings.

The integration is the hard part. The data from reflection and experimentation may conflict. You might value creativity (reflection) but discover that you perform best under constraint (experimentation). The apparent contradiction must be resolved before the values are confirmed.

The resolution requires nuance. Maybe you value creative constraint — the creativity of working within boundaries rather than the creativity of total freedom. The nuance matters. It transforms a vague value into a specific one.

Confirmation also involves testing the values against adversity. A value that you hold only when things are going well is a fair-weather value. A value that you hold when it costs you something — when it creates conflict, when it costs money, when it alienates people — that is a core value. The adversity test is the ultimate validator.

The output of confirmation is a short list of values — typically three to seven — that you can articulate clearly and defend confidently. The list is not permanent. It is your best understanding of your values as of now. It will evolve as you do.

### The Hierarchy of Values

Not all values are equal. Some are more fundamental than others. The hierarchy of values is the structure that organizes your values by importance.

At the top of the hierarchy are your core values — the non-negotiable principles that define who you are. These are the values you would sacrifice other things to uphold. If someone offered you a million dollars to violate your core value of honesty, you would refuse. The refusal is not rational. It is visceral. The core value is part of your identity.

Below the core values are supporting values — principles that complement and express the core values. If your core value is autonomy, your supporting values might include responsibility, self-direction, and privacy. The supporting values are important but not absolute. They can be negotiated in ways that core values cannot.

At the bottom are situational values — principles that apply in specific contexts. You might value efficiency at work but patience at home. The situational values are legitimate but contextual. They do not define you the way core values do.

The hierarchy is not fixed by age or personality. It is built through experience. You discover the hierarchy by observing which values you uphold under pressure and which you abandon. The values that survive pressure are core. The values that crumble are situational. The hierarchy is the empirical record of your priorities, not the aspirational record of what you think your priorities should be.

```python
class ValuesHierarchy:
    def __init__(self):
        self.core = []          # non-negotiable, define identity
        self.supporting = []    # complement and express core values
        self.situational = []   # context-dependent principles

    def rank(self, values):
        """Core values override all. Supporting override situational.
        When two values conflict, the higher rank prevails."""
        return sorted(values, key=lambda v: v.rank_level, reverse=True)

    def resolve_conflict(self, value_a, value_b):
        """When two values pull in different directions,
        the hierarchy provides the answer."""
        if value_a.rank_level > value_b.rank_level:
            return f"Follow {value_a.name} — it is more fundamental."
        elif value_b.rank_level > value_a.rank_level:
            return f"Follow {value_b.name} — it is more fundamental."
        else:
            return "Equal rank. Requires creative integration."
```

The hierarchy is not static. It shifts as you grow. A value that was supporting may become core. A value that was core may be revealed as situational. The hierarchy is a living document, not a stone tablet.

### Core Values vs. Satellite Values

The distinction between core and satellite values is practical. Core values are the ones you organize your life around. Satellite values are the ones that orbit the core — meaningful but not structurally essential.

A core value is a value that you would change your life to protect. If honesty is a core value and your job requires deception, you will leave the job. The value is more important than the context. A satellite value is a value that you honor when possible but do not restructure your life for. If aesthetics is a satellite value and your job is functional but ugly, you will be mildly annoyed but you will not quit.

The identification of core values requires the adversity test. Ask: what value would I uphold even if it cost me my job, my relationship, or my reputation? The values that survive this question are your core values. The values that crumble under the weight of real cost are satellite values.

The satellite values are not unimportant. They add flavor and texture to your life. They are the things that make your expression of the core values unique. Two people who both value honesty will express it differently because of their satellite values — one through direct communication, one through meticulous documentation.

### Values Conflict

The hardest moment in values work is when two core values collide.

You value autonomy and you value community. The open-source project needs a decisions made by committee, but you want to build it your way. The community requires compromise. The autonomy requires independence. Both values are core. Both are non-negotiable. And they are in direct conflict.

Values conflict is not a failure of clarification. It is a feature of having values that matter. If your values never conflicted, they would not be deep enough to matter. The conflict proves that you hold multiple principles seriously.

The resolution of values conflict is not about choosing one value over the other. It is about finding a creative expression that honors both. The creative resolution might look like: you maintain autonomy over your contributions while participating in collective decisions about direction. You find the boundary where autonomy and community coexist.

Sometimes the creative resolution is not possible. Sometimes the values genuinely cannot coexist in a given situation. The resolution then requires a hierarchy — which value is more fundamental in this context? The hierarchy provides the answer, but the cost of the choice must be acknowledged. Choosing one value over another is a loss, even when it is the right choice.

The developer who values both craftsmanship and shipping faces this conflict daily. Craftsmanship says: make it perfect. Shipping says: make it good enough. The resolution is context-dependent. For critical infrastructure, craftsmanship dominates. For a weekend prototype, shipping dominates. The resolution requires wisdom, not just values.

Values conflict also reveals something important about the depth of your values. A value that never conflicts with anything is probably not deep enough to matter. The depth of a value is measured by the intensity of the conflict it produces when it is challenged. The more painful the conflict, the more fundamental the value. The developer who feels genuine anguish when forced to ship untested code is revealing the depth of their craftsmanship value. The developer who feels mild annoyance is revealing a preference, not a principle.

The resolution of values conflict is a skill, not a talent. It improves with practice. The first time you face a genuine conflict, you will feel paralyzed. The second time, you will feel uncertain. The third time, you will have a method. The method is: name both values, identify the hierarchy, look for creative integration, and if integration is impossible, choose the higher value and grieve the loss. The grief is legitimate. You are sacrificing something that matters. The grief honors the sacrifice.

### How to Test Whether a Value Is Truly Yours

There are five tests for whether a value is genuinely yours or borrowed.

The first test is the independence test. Would you hold this value if no one else did? If the answer is yes, it is yours. If the answer is no — if you would abandon the value in the absence of social support — it is borrowed.

The second test is the sacrifice test. Would you give up something you want in order to honor this value? The sacrifice does not need to be dramatic. It might be giving up a project that violates the value, or a relationship that demands its violation. The willingness to sacrifice is proportional to the genuineness of the value.

The third test is the consistency test. Do you act on this value consistently, or only when it is convenient? A value that you practice only when it is easy is a preference, not a value. A value that you practice even when it is costly is a genuine value.

The fourth test is the emotional test. When you act in alignment with this value, do you feel grounded? When you violate it, do you feel wrong? The emotional response is a signal. It indicates that the value is connected to your identity, not just your beliefs.

The fifth test is the time test. Has this value been present across different phases of your life? A value that appeared last week and felt important might be a genuine discovery or a temporary infatuation. A value that has been present since childhood, surfacing in different forms across different contexts, is almost certainly core.

There is a sixth test that is less obvious but equally important: the envy test. The things you envy reveal the things you value. When you feel envy toward a colleague who received recognition for their craft, the envy reveals that you value craft recognition. When you feel envy toward a friend who quit their job to travel, the envy reveals that you value freedom. Envy is uncomfortable data, but it is data. The values hidden in your envy are often the ones you have not admitted to yourself.

The tests do not produce binary answers. They produce a probability. The more tests a value passes, the more likely it is genuine. A value that passes three tests is probably real. A value that passes all six is almost certainly core. The testing is not a one-time event. It is a practice. The values that survive the tests today should be retested in a year. The retesting is the maintenance of self-knowledge.

### The Developer's Values

Developers tend to hold a distinctive set of values. The values are shaped by the craft, the culture, and the nature of the work.

Craftsmanship is a common developer value. The desire to do work that is well-made, elegant, and durable. Craftsmanship is not about perfectionism — it is about care. The craftsman takes pride in the quality of their work, not because anyone is watching, but because the work reflects who they are.

Autonomy is another common developer value. The desire to control how you work, what you work on, and when you work on it. Autonomy is not laziness — it is self-direction. The autonomous developer is not avoiding accountability. They are choosing the form of accountability that fits their values.

Learning is a developer value that often goes unnamed. The desire to understand, to master new tools and concepts, to stay at the edge of what is possible. Learning is not just professional development — it is a way of engaging with the world. The developer who values learning is restless in the best sense — always reaching for the next understanding.

Contribution is the developer value that connects to legacy. The desire to build things that help others, that solve real problems, that make life better for the people who use what you create. Contribution is not altruism — it is the recognition that your skills have value beyond your paycheck.

These values are not universal among developers. They are common patterns, not rules. The developer who values social connection over autonomy is not wrong. The developer who values stability over mastery is not broken. The point is not to adopt the values of the developer culture. It is to discover your own and to recognize when they align with or diverge from the culture's defaults.

The danger of developer values is that they can become identity badges rather than guiding principles. You say you value craftsmanship, but you ship code without tests. You say you value learning, but you have not read a technical book in two years. The badge value is a performance. The lived value is a practice. The clarification process reveals the difference. It asks not what you say you value but what your behavior proves you value.

### Living by Values in a World That Rewards Other Things

The world does not always reward your values. It rewards productivity over quality, speed over depth, appearance over substance. Living by your values in this environment requires deliberate resistance.

The resistance is not dramatic. It is quiet and consistent. It is choosing to write good documentation when the deadline says skip it. It is choosing to mentor a junior developer when the performance review says focus on your own output. It is choosing to refactor the code when the product manager says ship the feature.

The resistance has costs. You will advance more slowly. You will be less visible. You will be passed over for opportunities that reward the values the world prioritizes. The cost is real and must be accepted.

But the resistance also has returns. The returns are internal. You sleep well. You respect yourself. Your work has integrity. The people who share your values recognize you and are drawn to you. The community of value-aligned people is smaller than the mainstream, but it is deeper.

The practice of living by your values is the practice of choosing integrity over optimization. You are not optimizing for the world's metrics. You are optimizing for alignment between your behavior and your principles. The optimization is a form of self-respect.

There is a specific experience that developers encounter when their values conflict with the organization's values. The organization values velocity — ship fast, move on, do not look back. The developer values craftsmanship — build it right, make it maintainable, care about the details. The conflict is daily. It is exhausting. And it is clarifying.

The clarification comes from recognizing that the conflict is not a problem to be solved. It is a signal to be interpreted. The persistent conflict between your values and your environment is evidence of misalignment. The misalignment may be tolerable for a season — you need the income, the experience, the relationships. But it is not sustainable indefinitely. The values that are consistently violated will eventually demand expression. The demand often comes as burnout, resentment, or a quiet quitting that precedes the actual departure.

The practice of living by your values in a misaligned environment requires a strategy. The strategy has three parts: pockets of alignment, boundary maintenance, and exit planning.

Pockets of alignment are the spaces within your environment where your values are honored. The open-source project you contribute to on weekends. The mentoring relationship you maintain outside of work. The personal project that expresses your craft. The pockets are not a substitute for full alignment. They are a survival mechanism. They keep your values alive while you navigate the misalignment.

Boundary maintenance is the practice of protecting your values from erosion. Erosion happens slowly. You violate a value once, and it feels wrong. You violate it again, and it feels less wrong. By the tenth violation, it feels normal. The normalcy is the danger. Boundary maintenance is the practice of noticing the erosion and correcting it before it becomes identity.

Exit planning is the long-term strategy for leaving the misaligned environment. The exit may be a new job, a career change, or a redefinition of your role within the organization. The exit is not impulsive. It is planned. The planning takes months or years. The planning is an act of self-respect — you are committing to a future where your values are honored.

### Values as a Decision-Making Framework

Values simplify decision-making by providing a filter. Every decision can be evaluated against your values. The evaluation is fast and reliable.

The process is simple. When faced with a decision, you ask: which option best serves my core values? The answer is usually clear. Not always easy — the best option may be the harder one — but clear.

The clarity comes from the hierarchy. When values conflict in a specific decision, the hierarchy provides the tiebreaker. If autonomy and security conflict in a job offer, and autonomy ranks higher, the choice is made. The emotional difficulty remains, but the intellectual decision is resolved.

Values-based decision-making is not the same as values-based reasoning. Reasoning is the attempt to justify a decision after the fact. Decision-making is the use of values to arrive at the choice before deliberation. The values come first. The analysis follows.

The developer's version of values-based decision-making looks like this: when evaluating a new project, you ask whether the project serves your values of craftsmanship, autonomy, and learning. If it does, you take it. If it does not, you decline — regardless of the compensation. The values filter is the primary filter. Everything else is secondary.

The values filter also works retrospectively. You can look back at your past decisions and understand why some felt right and others felt wrong. The decisions that aligned with your values feel right in memory. The decisions that violated your values feel wrong. The retrospective analysis is not about regret. It is about pattern recognition. The patterns confirm the values. The confirmation strengthens the filter. The strengthened filter improves future decisions.

### A Walkthrough: Clarifying Your Values

Here is a concrete walkthrough of the values clarification process. It follows the three phases: reflection, experimentation, confirmation.

**Reflection.** You set aside an afternoon. You write about five moments in your life when you felt most alive and aligned. You write about five moments when you felt most deadened and wrong. You read the accounts and look for patterns.

The alive moments share common themes: you were building something. You were teaching someone. You were working alone, deep in concentration. You were making something that helped people.

The deadened moments share common themes: you were in meetings that accomplished nothing. You were following processes that existed for political reasons. You were optimizing metrics that did not matter. You were managing people who did not want to be managed.

The patterns suggest values: craftsmanship (building), contribution (helping), autonomy (working alone), mastery (deep concentration). These are your hypotheses.

**Experimentation.** Over the next three months, you test each hypothesis. You take on a side project with full autonomy. You volunteer to mentor a junior developer. You attend a meeting-free week. You spend a week on deep technical work without interruptions.

After each experiment, you journal. The mentoring was rewarding but exhausting. The autonomous project was energizing but lonely. The deep technical work was flow-state but产生了 guilt about not being available. The meeting-free week was blissful but revealed that some meetings are necessary and valuable.

The data refines the hypotheses. Autonomy is real but needs boundaries. Craftsmanship is strong but requires solitude. Contribution is genuine but is more fulfilling when it is knowledge transfer, not management.

**Confirmation.** After three months of experimentation, you integrate the findings. Your core values are: craftsmanship, autonomy, and knowledge transfer. Your supporting values are: honesty, curiosity, and sustainability. Your situational values include: efficiency at work, patience with people.

You write the list down. You test it against adversity: would you leave a high-paying job that required you to abandon craftsmanship? Yes. Would you leave a high-paying job that required you to abandon efficiency? No. The hierarchy is confirmed.

The clarified values become your compass. When a new opportunity appears, you evaluate it against the list. When two opportunities compete, the list decides. The decision-making is simpler, faster, and more aligned with who you actually are.

The walkthrough is illustrative, not prescriptive. Your process will look different. The specific values will be different. The timeline will be different. The method — reflect, experiment, confirm — is the constant. The method works because it is grounded in evidence, not aspiration. You are not choosing values you wish you had. You are discovering values you already have. The discovery is the work. The work is worth doing.

One final note on the walkthrough: the process is not linear. The three phases overlap and循环. You reflect, experiment, discover something that sends you back to reflection, experiment again, and eventually confirm. The loop is not a sign of failure. It is a sign of thoroughness. The values you confirm after three loops are more reliable than the values you confirm after one.

### When Your Values Change

Values are not permanent. They evolve as you do. The values that defined you at twenty-five may not define you at forty.

The change is usually gradual. A value that was central becomes peripheral. A value that was peripheral becomes central. The shift is driven by experience — the things you live through reshape what matters to you.

The developer who valued autonomy above all else may discover, after years of isolated work, that collaboration matters more than they thought. The developer who valued craftsmanship may discover, after watching a less polished tool save lives, that impact matters more than elegance.

The change in values is not a betrayal of who you were. It is a development of who you are. The person you were at twenty-five was not wrong. They were incomplete. The new values do not invalidate the old ones. They supplement them.

The practice is to revisit your values periodically. Annually is sufficient. You ask: do these still hold? What has changed? What is new? The revision is not an admission of error. It is an act of honesty.

The most common value shifts happen around three life transitions. The first is the transition from individual contributor to leader. The developer who valued individual mastery discovers that their new role demands a different value — the development of others. The shift is disorienting. The value that defined you is no longer the value that serves you. The transition requires a re-ranking, not an abandonment.

The second transition is the encounter with mortality — your own or someone close to you. The encounter strips away the superficial values and reveals the core. The developer who valued prestige discovers that they actually value presence. The developer who valued accumulation discovers that they actually value contribution. The encounter with mortality is the most reliable values clarifier that exists.

The third transition is parenthood. The arrival of a child restructures the value hierarchy overnight. Autonomy, which was once core, may become supporting. Security, which was once peripheral, may become core. The restructuring is not a choice. It is a response to a new reality that demands new priorities.

The common thread across all three transitions is that the new values were always present but suppressed. The transition does not create the values. It surfaces them. The clarification process is the same as it always was: reflect, experiment, confirm. The context has changed. The method has not.

### The Courage to Live by Your Values

Values clarification without values practice is an intellectual exercise. The work is not complete until you live by what you have discovered.

Living by your values requires courage. It requires saying no to things that violate your values, even when saying no is costly. It requires saying yes to things that serve your values, even when saying yes is scary. It requires accepting the consequences of alignment.

The courage is not the absence of fear. It is the decision that alignment matters more than comfort. The aligned life is not always the easy life. It is the life that is yours.

The developer who values craftsmanship and refuses to ship buggy code will face pressure. The pressure is real. But the refusal is also real. The refusal is the expression of the value. The expression is the practice. The practice is the life.

The courage builds over time. Each act of alignment makes the next act easier. Each time you choose your values over convenience, you strengthen the muscle. The muscle is not physical. It is moral. The moral muscle is the capacity to do the right thing when the right thing is hard. The capacity grows with use. It atrophies with neglect. The values clarification process is the identification of what to use the muscle for. The living is the using.

## Learning Tips

- Start with the reflection phase before moving to experimentation. Premature experimentation without reflection produces random data without pattern recognition.
- Write your values down by hand. The physical act of writing creates a stronger memory trace than typing. The list becomes a ritual object — something you can hold and return to.
- Share your values with one trusted person. The act of articulating your values to another person makes them more real. It also creates accountability.
- Revisit your values every twelve months. Values evolve. The list you write today will not be the list you would write in five years. The revision is a sign of growth.
- Do not aim for perfection. Your first values list will be imprecise. That is fine. The imprecision will refine itself through experience.

## Glossary

| Term | Definition |
|------|------------|
| Core values | The non-negotiable principles that define your identity and guide your most important decisions |
| Extrinsic value | A value that is rewarding because of what it produces, rather than for its own sake |
| Intrinsic value | A value that is rewarding in itself, regardless of external outcomes |
| Satellite values | Values that are meaningful but not structurally essential — they orbit the core without defining it |
| Supporting values | Values that complement and express core values, providing nuance to how the core is lived |
| Values conflict | A situation where two core values pull in different directions, requiring creative resolution or hierarchical judgment |
| Values hierarchy | The structure that organizes values by importance, with core values at the top and situational values at the base |
| Values hypothesis | A tentative belief about what you value, generated through reflection and tested through experimentation |
| Should value | A value adopted from social pressure or cultural expectation rather than from genuine personal evidence |

## Quick References

- [Self-Determination Theory, Deci & Ryan](https://selfdeterminationtheory.org/) — the research framework on intrinsic vs. extrinsic motivation
- [The Values in Action (VIA) Classification](https://www.viacharacter.org/) — a research-based survey identifying character strengths and values
- [Motivational Interviewing, Miller & Rollnick](https://motivationalinterviewing.org/) — a clinical method for exploring and resolving ambivalence about values
- [Nonviolent Communication, Marshall Rosenberg](https://www.cnvc.org/) — a framework for connecting behavior to underlying needs and values
- [Let Your Life Speak, Parker Palmer](https://www.amazon.com/Let-Your-Life-Speak-Listening/dp/0470467783) — a gentle, honest exploration of vocation and values
- [Atomic Habits, James Clear](https://jamesclear.com/atomic-habits) — practical systems for aligning daily behavior with stated values

## Next Steps

- [The Power of Commitment](the-power-of-commitment.md) — turning clarified values into binding commitments that make purpose tangible
- [Legacy Thinking](legacy-thinking.md) — extending your values beyond your lifetime into the legacy you build
- [Compounding Wins](../habits/compounding-wins.md) — the systems that turn daily values-aligned actions into long-term transformation
