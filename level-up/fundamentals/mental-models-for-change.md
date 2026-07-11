# Mental Models for Change

## Description

Personal transformation is easier to navigate when you have frameworks that explain what is happening. This document introduces mental models — simplified representations of how change works — that help you understand your own patterns, predict the stages of transformation, and make deliberate choices about how to proceed. For a developer, these models are the APIs of the self.

## Prerequisites

- [The Mechanism of Change](the-mechanism-of-change.md) — the interdependent drivers of awareness, agency, and action, including the layers of self-awareness and radical honesty

## Table of Contents

- [What Mental Models Are and Why They Matter](#what-mental-models-are-and-why-they-matter)
- [The Johari Window: Known and Unknown Self](#the-johari-window-known-and-unknown-self)
- [The Stages of Change Model](#the-stages-of-change-model)
- [Karpman's Drama Triangle](#karpmans-drama-triangle)
- [First Principles Thinking](#first-principles-thinking)
- [Inversion: Working Backward from Where You Want to Be](#inversion-working-backward-from-where-you-want-to-be)
- [Stock and Flow Model of Identity](#stock-and-flow-model-of-identity)
- [Second-Order Thinking](#second-order-thinking)
- [How Developers Already Use These Models](#how-developers-already-use-these-models)
- [Choosing Which Models Serve You](#choosing-which-models-serve-you)

## Content / Material

### What Mental Models Are and Why They Matter

A mental model is a simplified representation of a complex system. It strips away noise to reveal the essential dynamics. In software, you use mental models every day: the request-response cycle, the event loop, the publish-subscribe pattern, the CAP theorem. None of these models capture the full complexity of the systems they describe. They do not need to. They need to be accurate enough about the important parts to guide your decisions.

Personal transformation is a complex system. Your behavior is driven by overlapping forces — biology, psychology, social environment, habits, beliefs, trauma, values, context. Trying to navigate this complexity without models is like trying to debug a distributed system by staring at the raw logs. You will drown in data and understand nothing.

Mental models do three things:

```python
class MentalModel:
    def __init__(self):
        self.name = ""
        self.description = ""

    def simplify(self, complexity):
        """Reduce infinite variables to the key drivers."""
        return extract_signal(complexity)

    def predict(self, situation):
        """Anticipate what happens next based on the pattern."""
        return anticipate(situation)

    def diagnose(self, outcome):
        """Explain why something happened by tracing it to a mechanism."""
        return explain(outcome)
```

The danger of mental models is the same as the danger of any abstraction: you can forget that the model is not reality. A developer who confuses the API documentation with the runtime behavior will ship buggy code. A person who confuses a mental model of themselves with their actual self will make plans that fail. Hold your models loosely. Use them as lenses, not as mirrors. When a model stops being useful — when reality no longer fits — discard it and find a better one.

The models in this document are not exhaustive. They are the ones most relevant to the early stages of personal transformation. Each one illuminates a different aspect of how change works. Together, they provide a multi-dimensional view of the process you are entering.

### The Johari Window: Known and Unknown Self

The Johari Window, developed by psychologists Joseph Luft and Harrington Ingham in 1955, is a model of self-knowledge and communication. It divides self-awareness into four quadrants based on what is known or unknown to you and to others.

```python
class JohariWindow:
    """
    A 2x2 matrix of self-knowledge.

    |                | Known to Self | Not Known to Self |
    |----------------|---------------|-------------------|
    | Known to Others| Open/Arena    | Blind Spot        |
    | Not Known to Others| Hidden/Façade| Unknown          |
    """
    def __init__(self):
        self.open = set()        # What you and others know about you
        self.blind = set()       # What others see but you do not
        self.hidden = set()      # What you know but others do not
        self.unknown = set()     # What neither you nor others know

    def expand_open(self, self_disclosure, feedback):
        """
        The Open quadrant grows through two mechanisms:
        1. Self-disclosure: sharing what is Hidden
        2. Feedback: learning what is Blind
        """
        self.hidden -= self_disclosure
        self.open |= self_disclosure
        self.blind -= feedback
        self.open |= feedback
```

**The Open quadrant** contains what both you and others know about you. Your visible skills, your obvious personality traits, the things you openly share. A large Open quadrant is associated with better relationships, higher performance, and less stress. People with a large Open quadrant do not waste energy maintaining a façade or being surprised by feedback they should have seen coming.

**The Blind quadrant** contains what others see but you do not. Your blind spots — the habits, reactions, and patterns that are obvious to everyone around you but invisible to you. Everyone has blind spots. The most dangerous people are those who believe they do not. Expanding your Open quadrant by reducing your Blind quadrant requires external feedback, which requires trust, which requires vulnerability.

**The Hidden quadrant** contains what you know but others do not. Your private fears, your insecurities, your true opinions, your past. Some Hidden information is appropriately private. But excessive hiding creates isolation and prevents genuine connection. The effort of maintaining a façade is exhausting, and the distance it creates prevents the intimacy that makes life meaningful.

**The Unknown quadrant** contains what neither you nor others know. Your unconscious patterns, your untapped potential, your repressed experiences. This quadrant is explored through new experiences, therapy, crisis, and confrontation with the unexpected. It shrinks as you live more fully and pay closer attention.

The Johari Window is directly connected to the self-awareness work in the Mechanism of Change document. Internal self-awareness expands the Open quadrant from your side. External self-awareness expands it from the other side. Radical honesty reduces the Hidden quadrant. The combination — honesty plus feedback — is the fastest way to grow the Open quadrant and reduce the others.

```python
# The Johari growth strategy
def grow_open_quadrant(window):
    """Two strategies, both required."""
    # Strategy 1: Ask for feedback (reduces Blind)
    feedback = ask_trusted_people("What do you see in me that I might not see?")
    window.blind -= feedback
    window.open |= feedback

    # Strategy 2: Self-disclose (reduces Hidden)
    honest_statements = journal_and_share_truths()
    window.hidden -= honest_statements
    window.open |= honest_statements

    # The Unknown quadrant shrinks indirectly —
    # through new experiences, crisis, and deep reflection
    return window
```

### The Stages of Change Model

The Transtheoretical Model of Change, developed by James Prochaska and Carlo DiClemente in the late 1970s, describes how people actually change. Not how they should change, not how self-help books say they change — how they empirically, observationally change. The model has five stages:

```python
class StagesOfChange:
    stages = [
        "precontemplation",  # Not aware a problem exists
        "contemplation",     # Aware, but not ready to act
        "preparation",       # Ready, planning to act soon
        "action",            # Actively changing behavior
        "maintenance",       # Sustaining the change over time
    ]

    def current_stage(self, person):
        """You are always in one of these stages for any given behavior."""
        pass

    def progress(self, current):
        """Movement is not always forward. Relapse is common and expected."""
        return f"{current} → next stage (or relapse to earlier stage)"
```

**Precontemplation.** In this stage, you do not see a problem. You are not in denial — you genuinely do not recognize the pattern or the behavior as problematic. Other people may see it. You do not. This stage is characterized by defensiveness when the topic is raised: "I don't have a problem with X." "That's just who I am." "Everyone does that."

The significance of this stage is that no amount of external intervention will produce change. Change requires internal readiness. The most effective approach from outside is planting seeds — sharing information, reflecting back observations, creating mild dissonance between the person's self-image and their behavior. But the person must arrive at awareness on their own timeline.

**Contemplation.** You see the problem. You are thinking about it, weighing the costs and benefits of change, considering what it would mean. This is the stage of ambivalence — you know you need to change, but you are not ready to act. The average person spends months to years in this stage for significant behavioral changes.

The danger of contemplation is analysis paralysis. You research every option, read every book, consider every perspective, and never commit to a direction. The remedy is to set a deadline for decision. Not a deadline for completion — a deadline for commitment. "By the end of this month, I will decide whether to pursue this change."

**Preparation.** You have decided to change and you are making plans. You are gathering resources, setting timelines, telling people about your intention. This stage feels productive — and it can become another form of delay. Planning can feel like doing without producing any actual change. The remedy is to plan minimally and act quickly.

**Action.** You are actively changing your behavior. This is the stage that gets all the attention in self-help content, but it is actually the shortest stage in most cases. The real work is in the stages before it (building readiness) and after it (maintaining the change). Action without preparation is impulsive. Action without maintenance is temporary.

**Maintenance.** You have changed and you are sustaining the change. This stage is characterized by relapse prevention, habit consolidation, and the gradual integration of the new behavior into your identity. Maintenance is where most people fail. The initial motivation of action fades, the old patterns exert their pull, and without ongoing effort, the change erodes.

```python
def stage_appropriate_intervention(stage):
    interventions = {
        "precontemplation": "Raise awareness. Share observations. Do not push.",
        "contemplation": "Explore ambivalence. List pros and cons. Set a decision deadline.",
        "preparation": "Make a specific, small plan. Tell one person. Start within 30 days.",
        "action": "Execute the plan. Track progress. Expect discomfort.",
        "maintenance": "Monitor for relapse triggers. Celebrate small wins. Seek support.",
    }
    return interventions.get(stage, "Unknown stage — reassess")
```

The critical insight of this model is that you cannot skip stages. People in precontemplation cannot jump to action. People in contemplation cannot jump to maintenance. Each stage has its own work, and doing the work of the wrong stage is wasted effort. The model also normalizes relapse — moving backward to an earlier stage is a normal part of change, not a failure.

### Karpman's Drama Triangle

Stephen Karpman's Drama Triangle, developed in 1968, maps three dysfunctional social roles that people cycle through in conflict situations: the Victim, the Persecutor, and the Rescuer. Understanding this triangle is essential because it describes the social dynamics that keep you stuck.

```python
class DramaTriangle:
    """
    Three roles, all dysfunctional, all interconnected.
    You rotate through them, sometimes within the same conversation.
    """
    roles = {
        "victim": {
            "core_belief": "Something is wrong, and I am powerless to fix it",
            "behavior": "Complaining, blaming, helplessness, avoiding responsibility",
            "catchphrase": "Why does this always happen to me?",
        },
        "persecutor": {
            "core_belief": "Something is wrong, and it is your fault",
            "behavior": "Blaming, criticizing, controlling, punishing",
            "catchphrase": "You should have known better",
        },
        "rescuer": {
            "core_belief": "Something is wrong, and I must fix it for you",
            "behavior": "Helping without being asked, enabling, overfunctioning",
            "catchphrase": "Let me handle that for you",
        },
    }

    def rotate(self, current_role):
        """The triangle is a cycle. Rescuers become Victims. Victims become Persecutors."""
        rotation = {
            "victim": "persecutor",     # "You never help me!" → attacking
            "persecutor": "victim",     # "I'm just trying to help!" → wounded
            "rescuer": "victim",        # "After everything I did for you!" → betrayed
        }
        return rotation.get(current_role)
```

**The Victim.** Not a victim of actual injustice — a role adopted in social dynamics. The Victim feels powerless, overwhelmed, and put-upon. The Victim's strategy is to avoid responsibility by framing themselves as unable to cope. "I can't handle this." "It's too much." "If only things were different." The Victim is not lying — they genuinely feel helpless. But the helplessness is a role, not a fact. It serves to attract Rescuers and deflect accountability.

**The Persecutor.** The Persecutor blames, criticizes, and controls. "You messed this up." "You should have done better." "This is all your fault." The Persecutor feels righteous — they are holding others accountable, setting standards, enforcing boundaries. But the Persecutor's aggression is often a displaced form of their own pain. Many Persecutors were once Victims who learned that attacking feels better than being attacked.

**The Rescuer.** The Rescuer helps — but in a way that enables rather than empowers. They swoop in before being asked. They take over when someone is struggling. They sacrifice their own needs to fix other people's problems. The Rescuer feels noble and selfless. But the Rescuer's help serves a purpose: it creates dependency, which confirms the Rescuer's value. A Rescuer who truly empowered others would become unnecessary, which threatens the Rescuer's identity.

The triangle is a system. All three roles are interdependent. The Victim needs the Rescuer. The Rescuer needs the Victim. The Persecutor gives both someone to blame. And the roles rotate — the Rescuer who is not appreciated becomes the Victim ("After everything I did for you!"), who then attacks the person they were helping, becoming the Persecutor.

**How to exit the triangle.** The exit is the Adult role — a concept from Eric Berne's Transactional Analysis that complements Karpman's model. The Adult role is characterized by:

- Taking responsibility without blame
- Offering help when asked, not when assumed
- Setting boundaries without punishment
- Recognizing your own agency instead of claiming helplessness

```python
class AdultRole:
    """
    The way out of the Drama Triangle.
    """
    def respond_to_conflict(self, situation):
        # Instead of Victim ("I can't handle this")
        # → "This is difficult. What can I do about it?"

        # Instead of Persecutor ("You failed")
        # → "This didn't work. What do we need to change?"

        # Instead of Rescuer ("Let me fix it")
        # → "Do you want help? What kind of help?"

        return "I see the situation clearly. I have agency. I choose my response."
```

In the context of personal transformation, the Drama Triangle explains why some relationships resist your change. When you stop playing the Victim, your Rescuers feel rejected. When you stop needing rescue, they feel unnecessary. When you start taking responsibility, your Persecutors lose their leverage. Changing your role disrupts the entire system, and the system will push back. Understanding this dynamic helps you anticipate resistance and hold your new role without reverting.

### First Principles Thinking

First Principles Thinking is the practice of breaking down complex problems into their most fundamental truths and reasoning upward from there. It is the opposite of reasoning by analogy — doing something because "that is how it has always been done" or because "that is what everyone else does."

In software, first principles thinking is the difference between a developer who copies Stack Overflow solutions and a developer who understands the underlying problem well enough to derive a solution from scratch. The first approach works until it does not. The second approach works everywhere.

Applied to personal transformation:

```python
class FirstPrinciplesThinking:
    def analyze(self, assumption):
        """
        Break an assumption down to its fundamental truths.
        """
        # Step 1: Identify the assumption
        # "I need to be productive every day to be valuable"

        # Step 2: Ask: what do I know to be absolutely true?
        fundamental_truths = [
            "I have a limited amount of energy each day",
            "Rest is biologically necessary",
            "Productivity is not the same as value",
            "My worth as a person is not contingent on output",
        ]

        # Step 3: Rebuild from the truths
        # "I have limited energy. Rest is necessary. My value is not output.
        #  Therefore, some days will be less productive, and that is not just
        #  acceptable — it is necessary for sustainability."

        return new_understanding
```

Most beliefs about how you should live, work, and relate are inherited — from culture, family, peers, or the internet. First principles thinking asks: "What do I actually know to be true, independent of what I have been told?" This is liberating and terrifying. It frees you from the tyranny of should. It also forces you to take responsibility for your own conclusions.

### Inversion: Working Backward from Where You Want to Be

Inversion is the practice of defining success first and then reasoning backward to determine what actions would produce it. Instead of asking "What should I do?" you ask "If I were already where I want to be, what would I be doing?"

In software architecture, this is equivalent to starting with the desired system behavior and designing backward to the implementation. You do not start with the tools you have and try to build something — you start with what you need and select the tools.

```python
def inversion_practice(current_state, desired_state):
    """
    Work backward from the desired state to identify necessary actions.
    """
    # Current: "I am a senior engineer who feels empty and disconnected"
    # Desired: "I am a person who feels connected to my work and my community"

    # Work backward from desired state:
    required_elements = [
        "I have meaningful relationships with colleagues",
        "My work connects to values I can articulate",
        "I spend time on things that matter to me, not just what matters to my employer",
        "I have a community outside of work",
    ]

    # For each element, ask: what daily action produces this?
    daily_actions = [
        "Have one genuine conversation per day (not about work)",
        "Spend 15 minutes weekly reflecting on whether my work aligns with my values",
        "Dedicate 3 hours per week to personally meaningful projects",
        "Attend one community event per month",
    ]

    return daily_actions
```

Inversion is powerful because it eliminates the noise. Instead of trying everything, you identify the minimum set of actions that would produce your desired state. This is especially valuable for developers who tend toward over-optimization — trying to do everything perfectly instead of doing the essential things adequately.

The negative form of inversion — "What would guarantee failure?" — is equally useful:

```python
def inversion_negative():
    """
    What would guarantee I stay stuck?
    """
    failure_guarantees = [
        "Never examine my patterns honestly",
        "Blame external circumstances for everything",
        "Avoid difficult conversations",
        "Optimize for comfort over growth",
        "Never ask for help",
        "Keep doing what I have always done",
    ]
    # Now: do the opposite of each one
```

### Stock and Flow Model of Identity

The Stock and Flow model, borrowed from systems dynamics, distinguishes between stocks (accumulated quantities) and flows (rates of change). Applied to identity:

```python
class IdentityStockAndFlow:
    """
    Stocks: Who you are (accumulated beliefs, habits, skills, relationships)
    Flows: What you do daily (actions, decisions, conversations)
    """
    def __init__(self):
        self.stocks = {
            "self_belief": 0.5,        # accumulated confidence
            "physical_health": 0.6,     # accumulated fitness
            "relationships": 0.4,       # accumulated connection
            "skills": 0.7,             # accumulated competence
            "meaning": 0.3,            # accumulated purpose
        }
        self.flows = {
            "daily_habits": [],         # actions that feed or drain stocks
            "decisions": [],            # choices that compound
            "conversations": [],        # interactions that build or erode
        }

    def update(self, time_period):
        """
        Stocks change slowly through accumulated flows.
        A single flow has tiny impact. Consistent flows compound.
        """
        for stock, value in self.stocks.items():
            daily_flow = self.calculate_flow(stock)
            # Small daily flow × 365 days = significant stock change
            self.stocks[stock] = value + daily_flow * time_period
```

The critical insight is that identity (stocks) changes slowly through daily action (flows). You cannot change who you are in a day. You can change what you do today, and if you do it consistently, who you are will follow. This is why habits matter — habits are the flows that feed the stocks. A single act of courage does not make you brave. A year of choosing courage over comfort does.

The stock-and-flow model also explains why dramatic interventions often fail. Quitting your job, moving to a new city, ending a relationship — these are large flow events that temporarily shift a stock. But without sustained daily flows to support the shift, the stock reverts. The new city feels exactly like the old city after three months because the daily patterns are the same.

### Second-Order Thinking

First-order thinking asks: "What is the immediate result of this action?" Second-order thinking asks: "And then what? And then what after that?"

```python
def second_order_thinking(action):
    """
    Trace the consequences of an action beyond the immediate result.
    """
    first_order = f"I start waking up at 5 AM to exercise"
    second_order = "I'm tired by 8 PM and less productive in evening meetings"
    third_order = "I start resenting the morning routine and quit after two weeks"
    fourth_order = "I conclude I'm 'not a morning person' and avoid the identity change"

    # The initial action seems good (first order).
    # But the cascading consequences reveal hidden costs.
    # Second-order thinking would have suggested a different approach:
    # start with 15 minutes of exercise at lunch instead.

    return [first_order, second_order, third_order, fourth_order]
```

Second-order thinking is essential for personal change because first-order thinking is what got you into your current situation. The immediate gratification of avoidance, the short-term relief of people-pleasing, the instant comfort of staying in bed — all of these are first-order solutions that produce second-order problems.

Developers use second-order thinking constantly in system design: "If we add this feature, it will require this infrastructure, which will increase latency, which will affect the user experience." The same thinking applied to personal life reveals why well-intentioned changes often fail — they were designed for the first order and blind to the second.

### How Developers Already Use These Models

The models in this document are not foreign to a developer's thinking. They are the same patterns you already apply to code, translated to the domain of personal life.

```python
# Developer models → Personal life models
model_translations = {
    "debugging": {
        "in_code": "Tracing an error to its root cause through systematic investigation",
        "in_life": "Tracing a behavioral pattern to its origin through self-awareness",
    },
    "refactoring": {
        "in_code": "Restructuring code to improve clarity without changing behavior",
        "in_life": "Restructuring habits to improve clarity without changing core values",
    },
    "technical_debt": {
        "in_code": "Accumulated shortcuts that slow future development",
        "in_life": "Accumulated avoidance that slows personal growth",
    },
    "post_mortem": {
        "in_code": "Blameless analysis of what went wrong after an incident",
        "in_life": "Blameless analysis of what went wrong after a personal setback",
    },
    "feature_flag": {
        "in_code": "Gradually rolling out a change with the ability to revert",
        "in_life": "Testing a behavioral change on a small scale before committing",
    },
    "dependency_management": {
        "in_code": "Tracking what your system depends on and updating regularly",
        "in_life": "Tracking what your well-being depends on and maintaining those foundations",
    },
    "monitoring": {
        "in_code": "Observing system metrics to detect problems early",
        "in_life": "Observing your own state to detect emotional problems early",
    },
}
```

This is not a metaphor — it is a literal translation. The cognitive tools are identical. The only difference is the domain. When you debug a production incident, you do not say "I am a bad person because the server crashed." You say "Something went wrong. Let me find out what." Apply the same professional detachment to yourself.

### Choosing Which Models Serve You

Not every model is useful for every person at every stage. The models presented here are tools, not doctrines. The practice is to try them, see which ones illuminate your situation, and set aside the ones that do not.

A practical approach:

```python
def select_models(your_situation):
    """
    Choose the models most relevant to where you are right now.
    """
    recommendations = {
        "cant_see_your_patterns": "Johari Window",
        "know_you_need_to_change_but_cant_start": "Stages of Change",
        "stuck_in_dysfunctional_relationships": "Drama Triangle",
        "lost_in_complexity_of_your_situation": "First Principles Thinking",
        "not_sure_what_to_change": "Inversion",
        "frustrated_that_change_is_slow": "Stock and Flow",
        "making_changes_that_backfire": "Second-Order Thinking",
    }

    # Start with the model that matches your current blocker.
    # Master it before adding another.
    # Models stack: once you see through the Johari Window,
    # you can add the Drama Triangle to understand your relationships,
    # then add Inversion to plan your path.
```

The goal is not to become a walking encyclopedia of mental models. It is to develop a small set of frameworks that you can deploy quickly when you encounter a situation that needs understanding. Two or three models, deeply understood and regularly applied, are worth more than twenty models that you can name but not use.

Hold all models loosely. They are maps, not territories. When the map does not match the territory, trust the territory. And remember that the most important model is the one you build yourself — a model of your own patterns, your own triggers, your own paths to change. The frameworks in this document are starting points for that personal model. They are not the destination.

## Learning Tips

**Pick one model per week.** Do not try to learn all seven models at once. Choose the one most relevant to your current situation, study it for a week, and apply it daily. After a week, evaluate whether it helped. If yes, keep using it. If not, try another.

**Apply models to small situations first.** Do not start by applying the Drama Triangle to your most painful relationship. Start with a minor disagreement at work. Do not start with Inversion to plan your entire life. Start with Inversion to plan your weekend. Build the skill with low stakes before applying it to high stakes.

**Write your observations in code metaphors.** If you are a developer, translating personal experiences into code concepts makes them more legible. "I am experiencing a stack overflow of obligations" is more useful than "I am overwhelmed." "This habit has become a memory leak — it silently drains my energy" is more actionable than "I am tired."

**Discuss models with someone.** Mental models become more useful when you articulate them to another person. Explain the Johari Window to a friend. Walk through the Stages of Change with your therapist. Use the Drama Triangle to analyze a movie scene. The act of explaining deepens your understanding.

**Revisit after a month.** The models that matter to you will reveal themselves over time. After a month of practice, you will notice which models you reach for instinctively. Those are your core models. The others are available if you need them, but your core models are the ones that have earned a permanent place in your thinking.

## Glossary

| Term | Definition |
|------|------------|
| Adult role | The functional alternative to the Drama Triangle: taking responsibility without blame, offering help when asked, recognizing your own agency |
| Blind quadrant | The Johari Window area containing what others see about you but you do not |
| Contemplation | The Stages of Change phase where you are aware of a problem but not ready to act |
| Drama Triangle | Karpman's model of three dysfunctional social roles (Victim, Persecutor, Rescuer) that cycle through conflict |
| First principles thinking | Breaking complex problems into fundamental truths and reasoning upward from there |
| Inversion | Working backward from a desired outcome to identify the actions that would produce it |
| Johari Window | Luft and Ingham's four-quadrant model of self-knowledge based on what is known or unknown to self and others |
| Maintenance | The Stages of Change phase where change is sustained over time through relapse prevention and habit consolidation |
| Mental model | A simplified representation of a complex system used to guide reasoning and decision-making |
| Open quadrant | The Johari Window area containing what both you and others know about you |
| Post-mortem | Blameless analysis of what went wrong, adapted from software practices to personal reflection |
| Precontemplation | The Stages of Change phase where you do not yet recognize a problem exists |
| Second-order thinking | Reasoning beyond immediate consequences to anticipate cascading effects |
| Stock and flow | A systems dynamics model distinguishing between accumulated quantities (stocks) and rates of change (flows) |
| Unknown quadrant | The Johari Window area containing what neither you nor others know about you |

## Quick References

- [Thinking in Systems, Donella Meadows](https://www.amazon.com/Thinking-Systems-Primer-Donella-Meadows/dp/1603581480) — the foundational text on systems thinking and feedback loops
- [The Art of Thinking Clearly, Rolf Dobelli](https://www.amazon.com/Art-Thinking-Clearly-Rolf-Dobelli/dp/0062236148) — accessible overview of cognitive biases and mental models
- [Poor Charlie's Almanack, Peter Kaufman](https://www.amazon.com/Poor-Charlies-Almanack-Charles-T-Munger/dp/1578645018) — Charlie Munger's collection of mental models for decision-making
- [The Drama Triangle, Stephen Karpman](https://www.karpmandramatriangle.com/) — original resource on the Drama Triangle and its resolution
- [The Transtheoretical Model, Prochaska & DiClemente](https://www.integrationinstitute.org/uploads/3/5/6/7/35673759/ttm_model.pdf) — the academic paper describing the stages of change
- [Insight, Tasha Eurich](https://www.amazon.com/Insight-Truth-About-Seeing-Yourself/dp/0525533869) — research on self-awareness and the Johari Window in practice
- [Super Thinking, Gabriel Weinberg & Lauren McCann](https://www.amazon.com/Super-Thinking-Big-Book-Mental/dp/0525533588) — comprehensive catalog of mental models for decision-making

## Next Steps

- [The Map of the Journey](the-map-of-the-journey.md) — a bird's-eye view of the path ahead, now legible through the models you have built
