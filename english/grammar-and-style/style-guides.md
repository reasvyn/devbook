# Technical Style Guides: Microsoft, Google, Apple

## Description

Technical style guides define the rules and conventions for writing clear, consistent documentation. Understanding the major guides — Microsoft, Google, and Apple — helps you choose the right standards for your project and produce documentation that meets industry expectations.

## Prerequisites

- [Sentence Structure for Technical Writing](sentence-structure.md) — understanding how sentences work is essential before applying style guide rules
- [Punctuation for Clarity](punctuation.md) — style guides specify punctuation conventions for different contexts

## Table of Contents

- [What Is a Technical Style Guide?](#what-is-a-technical-style-guide)
- [Why Style Guides Matter](#why-style-guides-matter)
- [The Microsoft Style Guide](#the-microsoft-style-guide)
- [The Google Developer Documentation Style Guide](#the-google-developer-documentation-style-guide)
- [The Apple Style Guide](#the-apple-style-guide)
- [Comparing the Three Guides](#comparing-the-three-guides)
- [Choosing a Style Guide for Your Project](#choosing-a-style-guide-for-your-project)
- [Maintaining Consistency Within a Project](#maintaining-consistency-within-a-project)
- [Creating a Custom Style Guide](#creating-a-custom-style-guide)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### What Is a Technical Style Guide?

A technical style guide is a set of standards for writing and formatting documentation. It covers voice and tone, terminology, grammar, punctuation, code formatting, UI element references, accessibility, and internationalization.

Unlike general writing style guides such as Chicago Manual of Style or AP Stylebook, technical style guides address the specific needs of software documentation. They answer questions about capitalizing button names, formatting error messages, when to use bold versus code font, and how to write inclusive documentation.

### Why Style Guides Matter

**Consistency.** When every document follows the same rules, readers know what to expect. Consistency reduces cognitive load and makes documentation easier to scan.

**Professionalism.** Following a recognized style guide signals that your documentation meets industry standards and builds trust with readers.

**Accessibility.** Style guides codify accessibility best practices — writing for screen readers, using plain language, avoiding ableist language, and providing text alternatives.

**Translation readiness.** Style guides specify rules that make documentation easier and cheaper to translate. Short sentences, consistent terminology, and clear pronoun usage improve localization outcomes.

**Onboarding efficiency.** New writers on a project can produce on-brand documentation from day one when a style guide exists. It eliminates repeated editorial feedback on the same issues.

### The Microsoft Style Guide

The Microsoft Style Guide is one of the most comprehensive technical style guides available. It is freely accessible online and covers a broad range of topics.

**Terminology standards.** Microsoft maintains a terminology database specifying approved and prohibited terms:

```text
Use "set up" (verb) or "setup" (noun) — not "configure" when simpler.
Use "sign in" instead of "log in" or "login".
Use "device" instead of "machine" or "computer" when the platform is unknown.
```

**Voice and tone.** The guide emphasizes a friendly, professional voice:

```text
Use "you" to address the reader directly.
Use "we" only when referring to Microsoft or the development team.
Avoid "please" — it can sound passive-aggressive in instructions.
Write in present tense whenever possible.
Use contractions — they feel natural and friendly.
```

**Accessibility requirements.** Microsoft has strong accessibility guidelines:

```text
Write alt text for every image.
Avoid directional language ("left", "right", "above", "below").
Use sentence case for headings and UI labels.
Write in plain language — target a reading level of grade 8 or below.
Ensure color is not the only means of conveying information.
```

**Formatting conventions.** Microsoft specifies how to format different elements:

```text
UI elements: Bold. "Select **Save**."
Code elements: Code font. "Enter `npm install`."
Keyboard shortcuts: Bold with plus signs. "Press **Ctrl+S**."
Error messages: Regular text in quotes. "If you see 'Access denied'..."
File names: Italic. "Open the *web.config* file."
```

**Prohibited terms.** The guide maintains a list of terms to avoid:

```text
Avoid: "click on" (just "click"), "double-click" (prefer "open")
Avoid: "end user" (just "user")
Avoid: "execute" (prefer "run"), "leverage" (prefer "use"), "utilize" (prefer "use")
Avoid: "in order to" (prefer "to")
```

**Procedures and instructions.** The guide specifies step-by-step instruction format:

```text
Start each step with a verb.
Use numbered lists for sequential steps.
Use bullet lists for non-sequential items.
Limit procedures to seven steps or fewer.
Do not include keyboard shortcuts as the primary method.
```

**Additional rules:**

```text
Use "select" instead of "choose" for UI interactions.
Use "clear" instead of "deselect" or "uncheck".
Avoid Latin abbreviations ("e.g.", "i.e.", "etc.") — use English equivalents.
Use the serial (Oxford) comma.
```

### The Google Developer Documentation Style Guide

The Google Developer Documentation Style Guide focuses on developer-facing documentation — API references, tutorials, guides, and code samples.

**Tone and voice.** Google emphasizes a neutral, authoritative voice:

```text
Be conversational but concise.
Use "you" for the reader.
Avoid "we" except when referring to the Google team.
Avoid jokes, cultural references, and metaphors.
Prefer imperative mood for instructions.
Use present tense for most statements.
```

**Code examples.** Code examples are central to Google's guide:

```text
Every code example must be complete enough to understand but minimal enough to focus on the concept.
Use real, meaningful example values — not "foo" or "bar".
Prefer one code example per concept.
Show both the code and its output when instructive.
Make code examples copy-pasteable whenever possible.
```

**UI element references.** When documentation refers to a user interface:

```text
Refer to UI elements by their visible label, not by their type.
Capitalize the exact label text as it appears.
Put the label in bold.
Use "click" for buttons, "select" for menu items, "enter" for text fields.
```

**Accessibility and inclusivity.** Google's guide includes accessibility requirements:

```text
Use descriptive link text — not "click here" or "read more".
Write alt text for all images.
Use inclusive language — avoid "master/slave", "whitelist/blacklist".
Provide text transcripts for videos.
Write for a reading level of grade 9 or below.
```

**Punctuation and formatting:**

```text
Use sentence case for headings and titles.
Use serial comma (Oxford comma).
Use code font for: code, file names, command-line entries, URLs, key names.
Use bold for: UI elements, important terms on first use.
Use italics for emphasis only — sparingly.
```

**Documentation structure.** Google recommends organizing developer documentation into specific sections:

```text
Overview — what the feature does and why to use it.
Quickstart — a minimal working example.
Concepts — in-depth explanation of how the feature works.
How-to guides — task-oriented procedures.
Tutorials — end-to-end walkthroughs.
Reference — API documentation.
Best practices — recommendations for production use.
```

**Bad vs. Good examples.** The Google Style Guide uses side-by-side comparisons:

```text
Bad: "If you want to create a new project, you can go ahead and click the Create button."
Good: "To create a project, click **Create**."

Bad: "The library is pretty fast and should be used whenever possible."
Good: "The library processes requests in under 10ms. Use it for high-throughput applications."
```

### The Apple Style Guide

The Apple Style Guide is the standard for documentation about Apple platforms — iOS, macOS, watchOS, and tvOS.

**UI element naming.** Apple places great emphasis on precise UI element naming:

```text
Always use Apple's registered term for UI elements, not generic descriptions.
Example: Use "Touch ID", not "fingerprint reader".
Example: Use "Control Center", not "settings panel".
Do not create compound names not used by Apple.
```

**Naming conventions for Apple technologies:**

```text
Use "macOS" (not "OS X" or "Mac OS").
Use "iOS" (not "iPhone OS").
Use "SwiftUI" (one word, no space), "UIKit" (not "UI Kit").
Use "Xcode" (not "XCode" or "xcode").
```

**Keyboard key names.** Apple specifies exact formatting for keyboard references:

```text
Capitalize key names: "Command", "Shift", "Option", "Control".
Use the symbol name in running text: "Press Command-C to copy."
For menus: "Choose Edit > Copy."
Use hyphens between key names: "Command-Shift-C".
```

**Internationalization.** Apple has strong internationalization guidance:

```text
Do not embed text in images — it cannot be translated.
Keep text strings in code separate from UI layout.
Use Auto Layout to accommodate text expansion from translation.
Design for right-to-left languages from the start.
Avoid cultural references that do not translate.
Use system APIs for date, time, number, and currency formatting.
```

**Voice and tone:**

```text
Use a friendly but professional tone.
Address the reader as "you".
Avoid marketing language in technical documentation.
Be precise about versions — "iOS 17" not "the latest iOS".
Write in present tense and use active voice.
```

**Terminology usage:**

```text
Use "app" not "application" (unless referring to the Application folder).
Use "tap" for iOS/iPadOS interactions, "click" for macOS.
Use "gesture" for touch interactions (swipe, pinch, rotate).
Use "notification" (not "alert" for system notifications).
```

**Formatting conventions:**

```text
UI labels: Bold.
Code: Mono-spaced font.
Menu paths: Hyphens between menu levels: "Choose File > Open".
File names: Regular text, capitalized as the file system shows.
```

**Inclusive language:**

```text
Use "parent" instead of "mother/father".
Use "sibling" instead of "brother/sister".
Use "they" as a singular pronoun.
Use "people" instead of "users" when referring to people generally.
```

**Specific prohibitions:**

```text
Do not use "iDevice" or "iProduct" — these are not Apple terms.
Do not use "Mac" as an adjective ("Mac computer" is acceptable).
Do not abbreviate Apple product names.
Do not hyphenate "touchscreen" or "touchpad".
```

### Comparing the Three Guides

| Topic | Microsoft | Google | Apple |
|-------|-----------|--------|-------|
| Availability | Free online | Free online | Free PDF download |
| Audience | General software users | Developers | Apple platform developers |
| Voice | Friendly, conversational | Neutral, authoritative | Friendly, professional |
| UI elements | Bold label | Bold label | Bold label |
| Code font | Inline code, code blocks | Inline code, code blocks | Code font |
| Keyboard shortcuts | Bold with plus | Depends on context | Hyphen-separated |
| Oxford comma | Yes | Yes | Yes |
| Contractions | Encouraged | Allowed | Allowed |
| Reading level target | Grade 8 | Grade 9 | Not specified |
| Accessibility | Strong emphasis | Strong emphasis | Present |
| Internationalization | Detailed | Detailed | Strong emphasis |

### Choosing a Style Guide for Your Project

**Match your platform.** If your documentation targets Apple platforms, use the Apple Style Guide. For a general developer tool, consider Google or Microsoft. For enterprise IT admin documentation, Microsoft is a strong choice.

**Consider your audience.** Developer-facing documentation benefits from Google's concise, code-focused approach. End-user documentation benefits from Microsoft's conversational voice. Platform-specific documentation for Apple should follow Apple's conventions.

**Hybrid approach.** Many projects adopt elements from multiple guides. A project might use Microsoft's accessibility guidelines, Google's code example rules, and Apple's terminology for Apple-platform features.

### Maintaining Consistency Within a Project

**Document your choices.** Create a project-level style sheet that records which conventions your team follows:

```text
Project Style Sheet: DevBook Documentation
- Base guide: Google Developer Documentation Style Guide
- UI element formatting: Microsoft style (bold labels)
- Keyboard shortcuts: Apple style (hyphen-separated)
- Accessibility: WCAG 2.1 AA minimum
- Oxford comma: Yes
- Contractions: Yes
- Reading level target: Grade 8
```

**Automate where possible.** Use linting tools to enforce style rules automatically:

```text
- vale: Open-source linter for prose. Supports Microsoft, Google, and custom styles.
- write-good: Tool for identifying weasel words and complex phrases.
- alex: Catches insensitive language.
- proselint: Checks for common writing issues.
```

**Review consistently.** Use a style guide checklist during documentation reviews:

```text
Style Review Checklist:
- [ ] Follows the chosen style guide
- [ ] No prohibited terms
- [ ] UI elements formatted correctly
- [ ] Code examples follow conventions
- [ ] Headings use correct capitalization
- [ ] Punctuation matches style guide rules
- [ ] Links use descriptive text
- [ ] Images have alt text
- [ ] Tone is consistent throughout
```

**Update regularly.** Style guides evolve. Microsoft and Google update their guides multiple times per year. Revisit your project's style decisions at least once per quarter.

### Creating a Custom Style Guide

When existing guides do not fully address your needs, create a custom guide using an existing guide as a foundation.

**1. Choose a base guide.** Select the existing guide that most closely matches your needs.

**2. Identify gaps.** List topics the base guide does not cover — your product's specific terminology, brand voice requirements, code language conventions, or screenshot standards.

**3. Resolve conflicts.** When two reference guides disagree, document which rule your project follows:

```text
Conflicting Rule: Keyboard shortcut format
- Microsoft: Ctrl+S
- Apple: Command-S
- DevBook Decision: Ctrl+S (cross-platform audience)
```

**4. Write rules with examples.** Each rule should include a clear statement, a correct example, and an incorrect example:

```text
Rule: Use sentence case for headings.
Correct: Installing the package
Incorrect: Installing The Package
```

**5. Define your terminology.** Create a project-specific glossary with approved terms and prohibited terms:

```text
Approved: "repo" (not "repository"), "config" (not "configuration")
Prohibited: "just", "simply", "easy", "obviously"
```

**6. Establish a review process.** Define how the style guide is maintained, who approves changes, and how often it is reviewed.

**7. Enforce with tooling.** Configure automated linters to catch violations at scale:

```text
.vale.ini:
StylesPath = .vale/styles
MinAlertLevel = suggestion
[*.md]
BasedOnStyles = Google, DevBook
```

## Study Cases

### Case 1: Applying the Microsoft Style Guide

An installation guide uses inconsistent terms and passive voice. The team adopts the Microsoft Style Guide.

```text
Before:
The installation of the application should be performed by the user. The user
can then login with their credentials. To execute the setup, the user should
double-click on the setup.exe file.

After:
Install the application. Sign in with your credentials. To run setup,
double-click setup.exe.

Changes: Active voice, "login" -> "sign in", "execute" -> "run",
"double-click on" -> "double-click", removed "It is recommended that".
```

### Case 2: Applying the Google Style Guide

An API tutorial uses vague examples and conversational filler.

```text
Before:
If you want to get started with our API, you can go ahead and make a GET
request to the /users endpoint. Pretty neat, right?

After:
To get started, send a GET request to the `/users` endpoint.

Changes: "If you want to" -> "To", removed "you can go ahead and",
removed "Pretty neat, right?" (no jokes), `/users` in code font.
```

### Case 3: Applying the Apple Style Guide

A guide describes a macOS feature with incorrect Apple terminology.

```text
Before:
Go to System Preferences and click on the Security icon.

After:
Go to **System Settings** and select **Security**.

Changes: "System Preferences" -> "System Settings" (macOS naming),
"click on" -> "click", "Security icon" -> "Security" as bold label.
```

## Examples

### Example 1: Microsoft Style Conversion

```text
Before:
The user can login by clicking on the Sign In button. Afterwards, the user
should navigate to Settings in order to configure their profile.

After:
Sign in by selecting **Sign In**. Then, go to **Settings** to configure
your profile.
```

### Example 2: Google Style Conversion

```text
Before:
Our API is pretty awesome. Basically, make a GET call to get user data.
It's that easy!

After:
To retrieve user data, send a GET request to `/users`.
```

### Example 3: Apple Style Conversion

```text
Before:
Go to the Finder menu and select Preferences. Press Cmd+W to close.

After:
In the Finder menu, select **Settings**. Press Command-W to close.
```

### Example 4: Style Sheet Entry

```text
Rule: Platform-specific keyboard shortcuts
- Windows/Linux: Ctrl+S, Ctrl+Shift+P
- macOS: Command-S, Command-Shift-P
- Cross-platform: Show both on first use.
```

### Example 5: Terminology Table

```text
| Term | Status | Alternative |
|------|--------|-------------|
| leverage | Prohibited | use |
| utilize | Prohibited | use |
| execute | Prohibited | run |
| login | Prohibited | sign in |
| setup (noun) | Approved | - |
| set up (verb) | Approved | - |
```

## Glossary

| Term | Definition |
|------|------------|
| Style guide | A set of standards for writing and formatting content |
| Voice | The consistent personality and tone of writing |
| Oxford comma | A comma before the final item in a list ("A, B, and C") |
| Sentence case | Capitalizing only the first word and proper nouns in a heading |
| Accessibility | Designing content usable by people with disabilities |
| Localization | Adapting content for a specific language or region |
| Internationalization | Designing content to be easily adaptable to different languages |
| Linter | A tool that automatically checks writing against defined rules |
| Controlled vocabulary | A restricted set of approved terms for consistent usage |
| Imperative mood | Giving direct commands ("Run the command") |
| WCAG | Web Content Accessibility Guidelines |
| Plain language | Writing that is clear, concise, and easy to understand |
| Alt text | Text alternative for images used by screen readers |

## Quick References

- [Microsoft Style Guide](https://learn.microsoft.com/en-us/style-guide/welcome/) — full guide available online
- [Google Developer Documentation Style Guide](https://developers.google.com/style) — guide for developer-facing content
- [Apple Style Guide](https://support.apple.com/guide/applestyleguide/) — official Apple guide
- [Vale](https://vale.sh/) — open-source prose linter supporting multiple style guides
- [write-good](https://github.com/btford/write-good) — tool for identifying weasel words
- [alex](https://alexjs.com/) — tool for catching insensitive language
- [WCAG 2.1 Guidelines](https://www.w3.org/TR/WCAG21/) — Web Content Accessibility Guidelines

## Next Steps

- [Conciseness & Eliminating Jargon](conciseness.md) — applying style guide principles to write shorter, clearer content
- [Writing for International Audiences](international-audiences.md) — making documentation that works globally
