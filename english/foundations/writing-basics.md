# Writing Basics

## Description

Writing is how developers communicate — commit messages describe what changed, comments explain why, documentation tells others how to use code, and issues report what went wrong. This document teaches the mechanics of English writing from the ground up: constructing sentences, using punctuation correctly, building paragraphs, and writing the short technical texts that developers produce daily.

## Prerequisites

- [Reading from Scratch](reading-from-scratch.md) — ability to decode and read English text
- [Vocabulary for Beginners](vocabulary-for-beginners.md) — familiarity with core technical vocabulary

## Table of Contents

- [Why Writing Matters for Developers](#why-writing-matters-for-developers)
- [Sentence Structure](#sentence-structure)
- [Parts of Speech](#parts-of-speech)
- [Building Longer Sentences](#building-longer-sentences)
- [Common Sentence Mistakes](#common-sentence-mistakes)
- [Paragraphs](#paragraphs)
- [Technical Writing Conventions](#technical-writing-conventions)
- [Commit Messages](#commit-messages)
- [Writing Issue Reports](#writing-issue-reports)
- [Writing Code Comments](#writing-code-comments)
- [Practice Exercises](#practice-exercises)
- [Learning Tips](#learning-tips)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Why Writing Matters for Developers

Every developer writes daily:

| What you write | Why it matters |
|---------------|---------------|
| Commit messages | Your future self and teammates need to understand what changed and why |
| Code comments | Explain non-obvious decisions for whoever reads the code next |
| Documentation | Tell others how to use your code |
| Issue reports | Describe bugs clearly so they can be fixed |
| Pull request descriptions | Explain what your changes accomplish |
| Chat messages | Discuss technical decisions with your team |
| Emails | Communicate with colleagues, clients, and communities |

Poor writing creates confusion. A commit message that says "fixed stuff" tells no one what was fixed. A bug report that says "it is broken" gives no one enough information to fix it. Clear writing is not a luxury — it is a professional necessity.

### Sentence Structure

Every complete English sentence has two essential parts:

```
┌─────────────────────────────────────────┐
│            SENTENCE                      │
│                                         │
│  ┌──────────────┐  ┌────────────────┐  │
│  │   SUBJECT     │  │    PREDICATE    │  │
│  │  (who/what)   │  │  (does what)   │  │
│  └──────────────┘  └────────────────┘  │
└─────────────────────────────────────────┘
```

The **subject** is who or what the sentence is about. The **predicate** tells what the subject does or is.

**Examples:**

| Subject | Predicate | Full sentence |
|---------|-----------|--------------|
| The function | returns a value | The function returns a value. |
| The error | occurred during compilation | The error occurred during compilation. |
| The user | clicked the button | The user clicked the button. |
| The server | is running on port 3000 | The server is running on port 3000. |
| We | need to fix the bug | We need to fix the bug. |

**Three sentence types:**

| Type | Purpose | Example |
|------|---------|---------|
| **Statement** | States a fact or opinion | "The code compiles without errors." |
| **Question** | Asks something | "Does the function handle null values?" |
| **Command** | Tells someone to do something | "Run the test suite before pushing." |

### Parts of Speech

English words have roles. Understanding these roles helps you construct correct sentences:

| Part of speech | Role | Examples |
|---------------|------|---------|
| **Noun** | A person, place, thing, or concept | `file`, `function`, `error`, `database`, `user` |
| **Verb** | An action or state | `run`, `return`, `is`, `execute`, `handle` |
| **Adjective** | Describes a noun | `fast`, `empty`, `recursive`, `optional`, `public` |
| **Adverb** | Describes a verb, adjective, or other adverb | `quickly`, `always`, `never`, `directly`, `correctly` |
| **Pronoun** | Replaces a noun | `it`, `they`, `this`, `which`, `who` |
| **Preposition** | Shows relationship (time, place, direction) | `in`, `on`, `at`, `to`, `from`, `with`, `by` |
| **Conjunction** | Connects words or clauses | `and`, `but`, `or`, `if`, `when`, `because` |
| **Article** | Introduces a noun | `a`, `an`, `the` |

**Example analysis:**

```
The fast function correctly handles empty input.
│    │     │         │         │       │    │
art adj  noun       adv       verb   adj   noun
│    │     │         │         │       │    │
subject─────────  predicate──────────────────────
```

### Building Longer Sentences

Simple sentences can be combined into more complex ones:

**Using "and" (adding information):**

```
Simple: The function returns a value.
Simple: The function logs a message.
Combined: The function returns a value and logs a message.
```

**Using "but" (contrasting):**

```
Simple: The code is fast.
Simple: It is hard to read.
Combined: The code is fast but hard to read.
```

**Using "because" (explaining why):**

```
Simple: The test failed.
Simple: The input was null.
Combined: The test failed because the input was null.
```

**Using "when" or "if" (conditions):**

```
Conditional: When the file is empty, the function returns early.
Conditional: If the user is not authenticated, redirect to login.
```

**Using "which" or "that" (adding details):**

```
Simple: The function handles errors.
Detailed: The function, which handles errors gracefully, is well-tested.
```

### Common Sentence Mistakes

| Mistake | Wrong | Correct |
|---------|-------|---------|
| Run-on sentence | The code is broken it needs fixing | The code is broken. It needs fixing. |
| Comma splice | The test failed, we need to fix it | The test failed. We need fixing it. |
| Fragment (no verb) | The error in the function | The error in the function was unexpected. |
| Fragment (no subject) | Returned null | The function returned null. |
| Wrong word form | The function work correctly | The function works correctly. |
| Missing article | Function returns value | The function returns a value. |

**How to check your sentences:**

1. Does it have a subject? (who/what is doing the action?)
2. Does it have a verb? (what action is happening?)
3. Does it make sense on its own? (is it a complete thought?)

If any answer is "no," the sentence needs revision.

### Paragraphs

A **paragraph** is a group of related sentences about one topic. In technical writing, paragraphs are typically short — 3 to 6 sentences.

**Structure of a technical paragraph:**

```
┌──────────────────────────────────────┐
│  Topic sentence                      │  ← states the main idea
│  (what this paragraph is about)      │
├──────────────────────────────────────┤
│  Supporting sentence 1               │  ← explains or gives details
│  Supporting sentence 2               │
│  Supporting sentence 3               │
├──────────────────────────────────────┤
│  Closing sentence (optional)         │  ← wraps up or transitions
└──────────────────────────────────────┘
```

**Example paragraph:**

```
The map() function transforms each element in an array. It takes a
callback function as an argument and applies that function to every
element. The results are collected into a new array, which is returned.
The original array is not modified.

[topic]        [supporting detail]     [supporting detail]
[new information]                       [closing]
```

**Paragraph rules for technical writing:**

- One idea per paragraph
- Start with the most important information
- Use simple sentences — readers scan, they do not read word-by-word
- Keep paragraphs short — long paragraphs discourage scanning

### Technical Writing Conventions

Technical writing follows specific conventions:

**Use present tense for facts:**

```
Wrong:  The function will return a value.
Right:  The function returns a value.
```

**Use imperative mood for instructions:**

```
Wrong:  You should run the test suite.
Right:  Run the test suite.
```

**Use active voice (subject does the action):**

```
Passive: The error is thrown by the function.
Active:  The function throws an error.
```

**Be specific:**

```
Vague:    The function is slow.
Specific: The function takes 3 seconds to process 10,000 records.
```

**Avoid unnecessary words:**

```
Wordy:    In order to install the package, you need to run the command.
Concise:  Run the command to install the package.
```

### Commit Messages

A commit message explains what a code change does. It has two parts: a short summary and an optional detailed description.

**Format:**

```
Short summary (50 characters or fewer, imperative mood, no period)

Optional detailed description: what changed and why.
Wrap lines at 72 characters. Use blank lines to separate paragraphs.
```

**Good commit messages:**

```
Fix null pointer when user is not logged in

The dashboard was crashing for unauthenticated users because
getUser() returned null without checking. Added a null check
before accessing user.name.

Closes #142
```

```
Add email validation to registration form

- Check that email contains @ and a valid domain
- Show error message if validation fails
- Disable submit button until email is valid
```

**Bad commit messages:**

```
fixed stuff          ← does not say what was fixed
update               ← does not say what was updated
WIP                  ← too vague, even for work in progress
asdfasdf             ← meaningless
```

**The golden rule of commit messages:** someone reading only the commit history should be able to understand what changed and why, without looking at the code.

### Writing Issue Reports

An issue report tells other developers about a bug or a feature request. A good issue report has:

**Bug report template:**

```
Title: Brief description of the bug

Steps to reproduce:
1. What did you do?
2. What did you click?
3. What command did you run?

Expected behavior: What should have happened?

Actual behavior: What actually happened?

Environment: Operating system, browser, version numbers
```

**Example:**

```
Title: Login page crashes when email field is empty

Steps to reproduce:
1. Go to /login
2. Leave the email field empty
3. Click "Sign In"

Expected behavior: An error message says "Email is required."

Actual behavior: The page shows a white screen with the error
"Cannot read property 'email' of undefined."

Environment: Chrome 120, Ubuntu 22.04, app version 2.1.0
```

### Writing Code Comments

Comments explain why code exists or what non-obvious parts do:

**Good comments explain "why," not "what":**

```python
# Bad: this adds 1 to x
x = x + 1

# Good: offset by 1 because array indices start at 0
x = x + 1
```

**Good comments explain complex logic:**

```python
# Binary search: repeatedly divide the search interval in half.
# Time complexity: O(log n) — much faster than scanning every element.
def binary_search(arr, target):
    ...
```

**Good comments mark TODO items:**

```python
# TODO: optimize this query — currently scans entire table
# FIXME: this breaks when input is None
# HACK: temporary workaround for upstream bug #4521
```

**Comment rules:**

- Do not comment obvious code
- Do not comment out code — delete it (version control remembers)
- Keep comments up to date — stale comments are worse than no comments
- Write comments in English

### Practice Exercises

**Exercise 1: Write a commit message.**

You changed the login function to check if the password is at least 8 characters long. Write a commit message.

Sample answer:

```
Require minimum 8 characters for password

Added password length validation to the login function.
Users with shorter passwords now see a clear error message
instead of a generic "invalid credentials" error.
```

**Exercise 2: Write a bug report.**

The application crashes when you upload a file larger than 10 MB. Write an issue report.

Sample answer:

```
Title: Application crashes on file upload > 10 MB

Steps to reproduce:
1. Go to /upload
2. Select a file larger than 10 MB
3. Click "Upload"

Expected behavior: An error message says "File too large. Maximum size is 10 MB."

Actual behavior: The application crashes with "OutOfMemoryError: Java heap space."

Environment: Firefox 121, Windows 11, app version 3.0.2
```

**Exercise 3: Rewrite a bad sentence.**

Bad: "The function is not working properly and it needs to be fixed because it is broken."

Rewrite:

```
The function throws a TypeError when the input is null.
Add a null check before processing the input.
```

## Learning Tips

- **Write every day.** A commit message, a code comment, a note — daily writing builds the habit.
- **Read your writing aloud.** If it sounds awkward when spoken, it is awkward on screen. Revise.
- **Imitate good examples.** When you read a well-written commit message or documentation, copy its structure. Imitation is the fastest way to learn style.
- **Keep sentences short.** If a sentence has more than 25 words, split it. Short sentences are easier to read on screens.
- **Edit ruthlessly.** Write your first draft, then remove every word that is not necessary. The result will be clearer.
- **Use tools.** Grammar checkers (like Grammarly or LanguageTool) catch mechanical errors. They do not replace clear thinking, but they help.

## Glossary

| Term | Definition |
|------|------------|
| Adjective | A word that describes a noun (fast, empty, optional) |
| Adverb | A word that describes a verb, adjective, or other adverb (quickly, always) |
| Article | A word that introduces a noun (a, an, the) |
| Clause | A group of words with a subject and verb |
| Commit message | A description of what a code change does |
| Conjunction | A word that connects words or clauses (and, but, or, if) |
| Imperative mood | A verb form used for commands ("Run the script") |
| Noun | A word that names a person, place, thing, or concept |
| Paragraph | A group of related sentences about one topic |
| Predicate | The part of a sentence that tells what the subject does |
| Preposition | A word showing relationship (in, on, at, to, from) |
| Pronoun | A word that replaces a noun (it, they, this) |
| Subject | The part of a sentence that tells who or what the sentence is about |
| Verb | A word that expresses an action or state (run, return, is) |

## Quick References

- [Google Technical Writing Courses](https://developers.google.com/tech-writing) — free courses on clear technical writing
- [Plain English Campaign](https://www.plainenglish.co.uk/) — guides on writing clear, simple English
- [Conventional Commits](https://www.conventionalcommits.org/) — a specification for commit messages

## Next Steps

- [Reading Comprehension](reading-comprehension.md) — understand technical paragraphs and instructions
- [Grammar & Style](../grammar-and-style/index.md) — refine your English grammar and writing style
- [Technical Writing](../technical-writing/index.md) — write documentation, READMEs, and developer guides
