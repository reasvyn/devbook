# Conciseness & Eliminating Jargon

## Description

Conciseness is a cornerstone of good technical writing. Eliminating unnecessary words, filler phrases, and jargon makes documentation clearer, easier to translate, and more respectful of the reader's time. This document covers strategies for identifying wordiness and writing tighter prose.

## Prerequisites

- [Sentence Structure for Technical Writing](sentence-structure.md) — writing concise sentences requires understanding how sentences are built

## Table of Contents

- [Why Conciseness Matters in Technical Writing](#why-conciseness-matters-in-technical-writing)
- [Identifying Wordiness](#identifying-wordiness)
- [Eliminating Filler Phrases](#eliminating-filler-phrases)
- [Reducing Prepositional Phrases](#reducing-prepositional-phrases)
- [Avoiding Jargon and Buzzwords](#avoiding-jargon-and-buzzwords)
- [Replacing Complex Phrases with Simple Ones](#replacing-complex-phrases-with-simple-ones)
- [Using Strong Verbs](#using-strong-verbs)
- [Cutting Redundancy](#cutting-redundancy)
- [Eliminating Weasel Words](#eliminating-weasel-words)
- [Technical Terminology vs. Unnecessary Terminology](#technical-terminology-vs-unnecessary-terminology)
- [Tools for Checking Conciseness](#tools-for-checking-conciseness)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Why Conciseness Matters in Technical Writing

Technical readers are task-oriented. They want to find information, execute a task, or solve a problem as quickly as possible. Every unnecessary word is a barrier between the reader and their goal.

**Cognitive load.** Each unnecessary word consumes mental energy. A document with 20% filler effectively requires 20% more processing from the reader.

**Translation costs.** Translation is priced per word. Every unnecessary word increases localization cost. A document that is 20% wordier costs 20% more to translate.

**Scanability.** Technical readers scan before they read. Concise writing puts key information in predictable, easy-to-spot positions. Wordy writing buries the main point under preambles.

**Credibility.** Concise writing projects confidence. Writers who use many words to say little appear uncertain. Writers who make their point efficiently earn trust.

**Accessibility.** Readers with cognitive disabilities, non-native English speakers, and screen reader users all benefit from concise writing. Shorter sentences and simpler vocabulary make content more accessible.

**Mobile and embedded contexts.** Documentation displayed in terminals, IDE tooltips, error messages, or mobile screens has limited space. Concise writing ensures the full message fits the available display.

### Identifying Wordiness

Wordiness takes many forms. Recognizing them is the first step to eliminating them.

**Common patterns:**

```text
Unnecessary preambles:
Wordy: It is important to note that the server must be restarted.
Concise: Restart the server.

Redundant modifiers:
Wordy: The basic fundamentals of the protocol are easy to understand.
Concise: The fundamentals of the protocol are easy to understand.

Noun strings:
Wordy: The server-side database connection configuration process.
Concise: Configuring the database connection on the server.

Circumlocution (talking around the point):
Wordy: In the event that the connection fails, the system will perform
       an automatic attempt to reconnect.
Concise: If the connection fails, the system retries automatically.

Deadwood phrases:
Wordy: Due to the fact that the file is missing, the build fails.
Concise: The build fails because the file is missing.
```

**The wordiness test.** For each sentence, ask: "Can I remove words without losing meaning?" If yes, remove them.

### Eliminating Filler Phrases

Filler phrases add words without adding meaning. They are often habits carried over from academic writing or conversational speech.

```text
Filler -> Better
In order to -> To
In the event that -> If
Due to the fact that -> Because
In the process of -> During, while
With regard to -> About, for
On a regular basis -> Regularly
At this point in time -> Now
In the majority of cases -> Usually
In close proximity to -> Near
In the near future -> Soon
Is able to / Has the ability to -> Can
Is responsible for -> Handles, manages
In the case of -> For, in
With the exception of -> Except
Prior to -> Before
Subsequent to -> After
A majority of -> Most
A number of -> Several, many
There is / There are (sentence start) -> Rewrite with actual subject
```

**The "there is/are" test.** Sentences starting with "There is" or "There are" can almost always be rewritten:

```text
Wordy: There are three configuration options available to the user.
Concise: The user has three configuration options.

Wordy: There are several reasons why the build might fail.
Concise: The build might fail for several reasons.
```

**Nominalization detection.** Nominalization turns verbs into nouns, requiring extra words to express the same action:

```text
Wordy: The implementation of the new feature requires modification of the codebase.
Concise: Implementing the new feature requires modifying the codebase.

Wordy: The verification of the user's identity is performed by the system.
Concise: The system verifies the user's identity.
```

### Reducing Prepositional Phrases

Prepositional phrases often add unnecessary words. Replace them with one-word modifiers or restructure the sentence:

```text
Wordy: The configuration file for the application is located in the directory
       for the user's home folder.
Concise: The application configuration file is in the user's home directory.

Wordy: The error message in the log file at the top of the screen indicates
       a failure of the authentication process.
Concise: The error message at the top of the log indicates an authentication
         failure.
```

**Chain of prepositions.** Multiple consecutive prepositional phrases signal wordiness:

```text
Wordy: The report by the team on the performance of the database in production.
Concise: The team's report on database performance in production.

Wordy: The guide for the installation of the package on the server.
Concise: The guide for installing the package on the server.
```

**Possessive as replacement.** Use possessive form to eliminate prepositional phrases:

```text
Wordy: The settings of the application -> The application's settings
Wordy: The name of the file -> The file name
Wordy: The size of the database -> The database size
```

### Avoiding Jargon and Buzzwords

Jargon is specialized language appropriate for a specific audience. Buzzwords are trendy terms that lose precise meaning through overuse.

```text
Avoid -> Use
Leverage -> Use
Synergy -> (describe the actual interaction)
Paradigm -> Model, approach, method
Robust -> Reliable, fast, secure (be specific)
Scalable -> (show evidence or be specific)
Best-in-class -> (let the reader evaluate)
Game-changing -> (describe the actual improvement)
Next-generation -> (use version numbers or specifics)
Turnkey -> Ready to use
Zero-configuration -> (verify it is true)
Holistic -> Complete, comprehensive
Dynamic -> (describe the behavior)
Agile (as buzzword) -> Iterative, incremental
```

**When jargon is appropriate.** Jargon is acceptable when the audience knows the term and it is the most precise word available:

```text
Appropriate: The function memoizes the computed value.
Audience: JavaScript developers
"Memoize" is the precise term — alternatives like "cache" are less accurate.

Inappropriate: The application leverages a robust caching paradigm.
Audience: General users
Better: The application caches frequently accessed data.
```

**The test for jargon.** Ask: "Would an intelligent non-specialist understand this term without looking it up?" If no, define or replace it.

### Replacing Complex Phrases with Simple Ones

Building a mental list of common replacements helps you write concise prose:

```text
Complex -> Simple
Afford an opportunity -> Allow, let
At the present time -> Now
In the amount of -> For
In the event that -> If
On a daily basis -> Daily
On the grounds that -> Because
With reference to -> About
With the result that -> So
A sufficient number of -> Enough
During the course of -> During
For the purpose of -> For, to
In cases where -> When
In spite of the fact that -> Although
On the basis of -> By, from
Until such time as -> Until
With the exception of -> Except
```

**Technical replacements:**

```text
Complex -> Simple
Execute -> Run
Utilize -> Use
Terminate -> End, stop, close
Initialize -> Start, create, set up
Navigate to -> Go to, open
Authenticate -> Sign in (for general audiences)
Implement -> Add, build, create (when appropriate)
Deploy -> Release, publish, install (when appropriate)
Instantiate -> Create
Propagate -> Spread, pass, send
Facilitate -> Help, enable, support
```

### Using Strong Verbs

Weak verbs require supporting words to convey meaning. Strong verbs carry the meaning directly:

```text
Weak -> Strong
Make a decision -> Decide
Take action -> Act
Give assistance -> Help
Conduct an investigation -> Investigate
Perform an analysis -> Analyze
Have a requirement for -> Require
Make a modification -> Modify
Give consideration to -> Consider
Reach a conclusion -> Conclude
Make a comparison -> Compare
Have a discussion -> Discuss
Put into effect -> Implement
Carry out -> Do, run, perform
```

**Turning nouns back into verbs.** Restore the verb behind the nominalization:

```text
Weak: We made a recommendation that you use the updated library.
Strong: We recommended using the updated library.

Weak: The system performs the validation of user credentials.
Strong: The system validates user credentials.

Weak: The function handles the conversion of the input format.
Strong: The function converts the input format.
```

**Action verbs for technical writing:**

```text
Use: Run, start, stop, create, delete, update, read, write, send, receive
Use: Build, compile, deploy, test, verify, check, validate, parse
Use: Connect, disconnect, mount, unmount, load, save, export, import
Use: Filter, sort, search, find, replace, merge, split, join
Use: Install, uninstall, upgrade, downgrade, configure, register
```

### Cutting Redundancy

Redundancy repeats meaning unnecessarily:

```text
Redundant -> Better
Absolutely certain -> Certain
Actual fact -> Fact
Added bonus -> Bonus
Advance planning -> Planning
Alternative choice -> Choice
Basic fundamentals -> Fundamentals
Close proximity -> Proximity
Collaborate together -> Collaborate
Completely eliminate -> Eliminate
End result -> Result
Estimated at about -> Estimated
Final outcome -> Outcome
Free gift -> Gift
Join together -> Join
Major milestone -> Milestone
Mutual cooperation -> Cooperation
New innovation -> Innovation
Past history -> History
Personal opinion -> Opinion
Plan ahead -> Plan
Revert back -> Revert
Repeat again -> Repeat
Return back -> Return
Sum total -> Total
Unexpected surprise -> Surprise
Whether or not -> Whether (in most cases)
```

**Technical redundancies:**

```text
Connect together -> Connect
Download down -> Download
Upload up -> Upload
Merge together -> Merge
The exact same -> The same
Each and every -> Each or every
```

**The "ATM machine" syndrome.** Watch for acronyms followed by the expanded word:

```text
Redundant: PIN number -> PIN
Redundant: PDF format -> PDF
Redundant: CSS styles -> (acceptable, but be aware)
```

### Eliminating Weasel Words

Weasel words hedge certainty and make writing sound weak:

```text
Weasel word -> Impact
Basically -> Undermines authority. Delete or replace with specifics.
Essentially -> Similar to "basically". Usually unnecessary.
Virtually -> Creates artificial precision. Use "almost" or remove.
Quite -> Vague intensifier. Remove.
Somewhat -> Weakens statements. Remove or be specific.
Relatively -> Relative to what? Provide reference point.
Arguably -> Undermines the claim. State directly.
In general -> Often unnecessary. Replace with specific scope.
To a certain extent -> Meaningless qualifier. Remove entirely.
It is important to note that -> Filler. State the important thing directly.
```

**The weasel word removal test.** Read a sentence without the weasel word. If it still conveys the same meaning, remove it permanently:

```text
Before: The new framework is basically faster than the old one.
After:  The new framework is faster than the old one.

Before: It is important to note that the server must be restarted.
After:  Restart the server.
```

### Technical Terminology vs. Unnecessary Terminology

Not all technical terms are jargon. The distinction matters:

**Necessary technical terminology.** Terms that name specific technologies, protocols, or concepts:

```text
"TCP/IP" — no simpler alternative exists
"SQL injection" — precise security term
"Promise" — specific JavaScript meaning
"API endpoint" — precise technical reference
```

**Unnecessary terminology.** Terms that replace simple words with complex ones for no reason:

```text
"Leverage" instead of "use"
"Drill down" instead of "explore"
"Circle back" instead of "return" or "discuss later"
"Take offline" instead of "discuss separately"
"Ping" instead of "contact" or "message"
"Bandwidth" (for availability) instead of "time" or "capacity"
"Low-hanging fruit" instead of "easy tasks"
```

**Assessing terminology necessity.** Ask:

```text
1. Does the term have a simpler, equally precise alternative? If yes, use it.
2. Would removing or replacing the term confuse domain experts? If yes, keep it.
3. Is the term specific to one company or team? If yes, replace with standard.
4. Would a non-native speaker understand it? If no, define or replace it.
```

### Tools for Checking Conciseness

**Vale.** Open-source prose linter with built-in style guide support:

```text
Install: brew install vale
Usage: vale document.md
Checks: Passive voice, weasel words, wordy phrases, terminology
```

**write-good.** Node.js tool that catches common writing issues:

```text
Install: npm install -g write-good
Usage: write-good document.md
Checks: "There is" constructions, weasel words, passive, redundancies
```

**Hemingway Editor.** Highlights complex sentences, passive voice, and adverbs:

```text
Features: Color-coded highlighting, readability grade level, word count
```

**proselint.** Linter for prose with many categories:

```text
Install: pip install proselint
Usage: proselint document.md
Checks: Weasel words, redundant phrases, misused terms, hedging, hyperbole
```

**LanguageTool.** Open-source grammar and style checker:

```text
Features: Grammar, style improvements, readability analysis, editor plugins
```

**CI integration.** Integrate conciseness checks into your pipeline:

```text
name: Prose Lint
on: [pull_request]
jobs:
  vale:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: errata-ai/vale-action@v2
        with:
          styles: |
            https://github.com/errata-ai/Microsoft/releases/latest/download/Microsoft.zip
```

**Editor plugins.** Most editors support real-time style checking:

```text
VS Code: Vale extension, Code Spell Checker
Vim/Neovim: ale with Vale, proselint integration
```

## Study Cases

### Case 1: Rewriting a Wordy Error Message

```text
Original (62 words):
The system has encountered an unexpected error during the process of
attempting to establish a connection to the database server. It is
recommended that you verify that the database server is currently running
and that the connection parameters specified in the configuration file
are correctly configured.

Revision (25 words):
Failed to connect to the database. Make sure the database server is running
and the connection settings in the configuration file are correct.

Reduction: 60%
```

### Case 2: Simplifying a Documentation Introduction

```text
Original (73 words):
In order to get started with the utilization of our API, there are a few
preliminary steps that must first be completed by the user prior to the
commencement of any actual development work. First and foremost, you will
need to register for a developer account. Subsequent to the completion of
the registration process, you will be issued an API key which will be
utilized for the authentication of all requests.

Revision (24 words):
To start using the API, first register for a developer account. After
registering, you will receive an API key to authenticate all requests.

Reduction: 67%
```

### Case 3: Conciseness in a Configuration Guide

```text
Original (67 words):
The configuration of the application can be accomplished through the
modification of the config.yaml file. There are a number of different
configuration options that are available to the user. In the event that
you are unsure about the meaning of a particular configuration setting,
you can refer to the documentation for that specific setting in the
configuration reference guide located in the docs directory.

Revision (31 words):
Configure the application by editing the config.yaml file. The file offers
several settings. If you need help with a specific setting, see the
configuration reference in the docs directory.

Reduction: 54%
```

### Case 4: Jargon Removal

```text
Original (51 words):
Our solution leverages machine learning paradigms to deliver robust,
best-in-class analytics that empower your team to actionize data-driven
insights. The holistic platform seamlessly integrates with your existing
tech stack to synergize cross-functional data silos.

Revision (21 words):
Our application uses machine learning to analyze your data. It connects to
your existing tools and displays your key performance indicators.

Reduction: 59%
```

## Examples

### Example 1: Filler Phrase Removal

```text
Before:  In order to start the server, you need to run the start.sh script.
After:   To start the server, run the start.sh script.

Before:  Due to the fact that the API key is invalid, the request fails.
After:   The request fails because the API key is invalid.
```

### Example 2: Nominalization Fix

```text
Before:  We performed an analysis of the network traffic.
After:   We analyzed the network traffic.

Before:  The system provides the validation of user credentials.
After:   The system validates user credentials.
```

### Example 3: Redundancy Removal

```text
Before:  The end result of the migration process is a new database schema.
After:   The result of the migration is a new database schema.

Before:  Please revert back to the previous version of the file.
After:   Revert to the previous version of the file.
```

### Example 4: Weasel Word Removal

```text
Before:  It is important to note that the cache must be cleared.
After:   Clear the cache.

Before:  The build is basically complete, but there are a few remaining tasks.
After:   The build is complete except for a few remaining tasks.
```

### Example 5: Strong Verb Replacement

```text
Before:  Make a decision about which framework to use.
After:   Decide which framework to use.

Before:  The function performs a check of the input validity.
After:   The function checks the input validity.
```

### Example 6: Prepositional Phrase Reduction

```text
Before:  The configuration file for the database for production.
After:   The production database configuration file.

Before:  The button at the top of the page on the right-hand side.
After:   The button at the top right of the page.
```

### Example 7: Sentence-Level Rewrite

```text
Before:  There are three different types of authentication that are
         supported by the platform, and each one of them has its own
         unique set of configuration requirements.
After:   The platform supports three authentication types. Each type has
         its own configuration requirements.
```

### Example 8: Tool Output

```text
$ write-good draft.md
  = 2: "in order to" -> replace with "to"
  = 5: "there are" -> consider rewriting
  = 8: "utilize" -> replace with "use"
  = 12: "basically" -> is a weasel word
  = 15: "due to the fact that" -> replace with "because"
  = 18: passive voice -> consider active voice

Apply suggestions judiciously based on your audience and context.
```

## Glossary

| Term | Definition |
|------|------------|
| Buzzword | A trendy term that loses precise meaning through overuse |
| Circumlocution | Using many words to express an idea that could be stated briefly |
| Cognitive load | The mental effort required to process information |
| Controlled vocabulary | A restricted set of approved terms for consistent usage |
| Deadwood | Words that add no meaning and can be removed |
| Filler phrase | A phrase that adds length but not meaning |
| Hedge | A word or phrase that reduces the force of a statement |
| Jargon | Specialized language appropriate for a specific audience |
| Localization | Adapting content for a specific language or region |
| Nominalization | Turning a verb into a noun ("implement" to "implementation") |
| Prepositional phrase | A phrase that begins with a preposition and modifies another word |
| Redundancy | Unnecessary repetition of meaning |
| Scanability | The ease with which a reader can quickly find information |
| Strong verb | A verb that carries meaning directly without supporting words |
| Weasel word | A word that hedges certainty or weakens a statement |
| Wordiness | Using more words than necessary to convey meaning |

## Quick References

- [Hemingway Editor](https://hemingwayapp.com/) — tool for improving readability
- [Plain English Campaign](https://www.plainenglish.co.uk/) — guides for clear communication
- [Vale](https://vale.sh/) — open-source prose linter
- [write-good](https://github.com/btford/write-good) — tool for identifying weasel words
- [proselint](https://github.com/amperser/proselint) — linter for prose
- [LanguageTool](https://languagetool.org/) — open-source grammar and style checker
- [Microsoft Style Guide: Word Choice](https://learn.microsoft.com/en-us/style-guide/word-choice/) — Microsoft's guidance on word choice
- [Google Developer Documentation Style Guide: Word List](https://developers.google.com/style/word-list) — Google's approved terminology

## Next Steps

- [Technical Style Guides: Microsoft, Google, Apple](style-guides.md) — applying conciseness rules within a formal style guide framework
- [Writing for International Audiences](international-audiences.md) — how conciseness improves documentation for global readers
