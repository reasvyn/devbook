# Writing for International Audiences

## Description

Technical documentation reaches a global audience. Non-native English speakers, readers from different cultures, and translation workflows all impose constraints on your writing. This document covers strategies for creating documentation that works worldwide.

## Prerequisites

- [Sentence Structure for Technical Writing](sentence-structure.md) — writing for international audiences requires mastery of clear, simple sentences
- [Conciseness & Eliminating Jargon](conciseness.md) — wordiness and jargon create disproportionate difficulties for global readers

## Table of Contents

- [The Challenge of Global Technical Documentation](#the-challenge-of-global-technical-documentation)
- [Simplified English](#simplified-english)
- [Avoiding Idioms and Metaphors](#avoiding-idioms-and-metaphors)
- [Cultural Considerations](#cultural-considerations)
- [Date, Time, and Number Formats](#date-time-and-number-formats)
- [Units of Measurement](#units-of-measurement)
- [Translation Readiness](#translation-readiness)
- [Localization vs. Internationalization](#localization-vs-internationalization)
- [Writing for Non-Native English Speakers](#writing-for-non-native-english-speakers)
- [Testing with International Readers](#testing-with-international-readers)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### The Challenge of Global Technical Documentation

English is the dominant language of technology, but most technical writers produce documentation for a global audience. The reader in Tokyo, Berlin, or Sao Paulo may have a different level of English proficiency, cultural background, and technical context than the writer.

**Key challenges include:**

**Language proficiency variation.** Readers range from fluent English speakers to those who read English as a second language with limited proficiency. A document readable at grade 8 level may be challenging for someone who learned English through technical materials only.

**Cultural differences.** Directness, formality, humor, and visual metaphors vary across cultures. What seems friendly in one culture may seem rude or confusing in another.

**Translation overhead.** Documentation not designed for translation requires more time, money, and effort to localize. Poorly structured source text produces poor translations even with professional translators.

**Infrastructure differences.** Date formats, number formats, time zones, character encoding, and text direction differ across regions. Documentation that assumes one convention confuses readers from other regions.

**Context gaps.** Examples relying on region-specific knowledge — local sports, national holidays, popular culture — are meaningless to international readers.

**Right-to-left languages.** Arabic, Hebrew, Persian, and Urdu read right-to-left. Documentation must consider RTL layout and text flow.

**Character sets.** Latin, Cyrillic, CJK, Devanagari, and Arabic scripts have different character sets. Use Unicode (UTF-8) encoding and test font rendering across scripts.

### Simplified English

Simplified English is a controlled language designed to improve readability and translation accuracy. It restricts vocabulary, sentence structure, and grammar to a defined subset.

**Core principles:**

```text
1. Use only approved vocabulary.
2. Use each word in only one meaning.
3. Use the same term consistently throughout.
4. Keep sentences short (20 words or fewer).
5. Use one idea per sentence.
6. Use the active voice.
7. Use articles ("the", "a", "an") consistently.
8. Avoid verb clusters (more than two verbs together).
9. Avoid noun clusters (more than three nouns together).
10. Use consistent punctuation rules.
```

**Word usage rules.** Each word has a single approved meaning:

```text
"Follow" means to go after, not to understand.
"Check" means to verify, not to stop or limit.
"Run" means to operate, not to move quickly.
"Right" is used only for direction, not correctness.
"Light" is used only for illumination, not weight.
```

Multiple words for the same concept are restricted to one:

```text
Use "start", not "begin", "commence", or "initiate".
Use "end", not "terminate", "cease", or "conclude".
Use "use", not "utilize", "employ", or "leverage".
```

**Sentence structure rules:**

```text
Write one instruction per sentence:
- "Open the file. Edit the configuration. Save the changes."
- Not: "Open the file, edit the configuration, and then save the changes."

Use the same sentence pattern for similar actions:
- "To [goal], [action]."
- "To start the server, run start.sh."
- "To stop the server, run stop.sh."
```

**Vocabulary restrictions.** A Simplified English dictionary limits vocabulary to approximately 1,000 words plus approved technical terms. This is more restrictive than most projects need, but the principle of controlled vocabulary applies at any scale.

**Benefits:**

```text
- 20-30% reduction in translation costs
- 40-50% improvement in translation accuracy
- Faster reading times for non-native speakers
- Fewer ambiguities requiring translator decisions
- Consistent terminology across all translated versions
```

### Avoiding Idioms and Metaphors

Idioms and metaphors are culturally specific. They are difficult to translate and confusing for non-native speakers.

**Common idioms to avoid:**

```text
Idiom: "Hit the ground running" -> Better: "Start working immediately"
Idiom: "Think outside the box" -> Better: "Consider new approaches"
Idiom: "Put the cart before the horse" -> Better: "Do things in the wrong order"
Idiom: "The ball is in your court" -> Better: "It is your turn to act"
Idiom: "Bite the bullet" -> Better: "Accept the difficult task"
Idiom: "Piece of cake" -> Better: "Easy" or "simple"
Idiom: "Cut corners" -> Better: "Skip steps" or "take shortcuts"
Idiom: "Once in a blue moon" -> Better: "Rarely"
Idiom: "The bottom line" -> Better: "The key point" or "the result"
Idiom: "Take it with a grain of salt" -> Better: "Be skeptical"
Idiom: "Pass with flying colors" -> Better: "Succeed completely"
```

**Metaphors to avoid:**

```text
"The system has a heartbeat."
Assumes cultural familiarity with personification.
Better: "The system sends a status signal every 30 seconds."

"Kill the process."
Violent metaphor. Better: "Stop the process" or "end the process."
```

**The idiom test.** Ask: "Would this phrase make sense if translated literally into another language?" If no, use a literal, universal alternative.

**Acceptable technical metaphors.** Some metaphors are standard domain vocabulary:

```text
"Tree" in data structures
"Parent" and "child" in hierarchies
"Socket" in networking
"Thread" in concurrency
```

These are acceptable because they are part of the domain vocabulary. Define them in a glossary for non-native readers.

**Sports and game metaphors.** These are particularly problematic:

```text
Avoid: "Slam dunk", "Touch base", "Level playing field", "Hail Mary"
Replace with: "Certain success", "Contact", "Fair conditions", "Last resort"
```

### Cultural Considerations

Cultural differences affect how documentation is received and understood.

**Directness and formality:**

```text
US / Northern Europe: Direct communication is valued. "You" is standard.
Informal tone is acceptable. Instructions are imperative ("Do this").

Japan / Korea: Indirect communication is preferred. Formality signals
respect. "Please do this" is preferred over "Do this."

Germany / Switzerland: Direct, precise communication. Informality can seem
unprofessional. Use formal equivalents in translation.
```

**Color associations.** Colors carry different meanings across cultures:

```text
Red: Danger/stop (US/Europe), luck/prosperity (China), mourning (South Africa)
Green: Success/go (US/Europe), infidelity (China), fertility (Middle East)
White: Purity (US/Europe), mourning (East Asia)
Yellow: Caution (US/Europe), imperial power (China), mourning (Latin America)

Do not rely on color alone to convey meaning. Use text labels and icons.
```

**Symbols and icons:**

```text
Thumbs up: Positive in US/Europe, offensive in Middle East.
OK gesture: Positive in US, offensive in Brazil and Turkey.
Checkmark: Means "correct" in US/Europe, can mean "error" in Japan.
```

**Numbers and symbols:**

```text
13: Unlucky in US/Europe.
4: Unlucky in China, Japan, Korea (sounds like "death").
```

**Naming and addressing:**

```text
Family name comes first in China, Japan, Korea, Hungary.
Use full names or roles, not first names alone.
Avoid "Mr./Mrs./Ms." unless the preference is known.
Avoid gendered address forms.
```

**Humor and wordplay:**

```text
Humor does not translate. Puns rely on specific language features.
Irony and sarcasm are culturally specific.
Avoid jokes, pop culture references, and current events in documentation.
```

### Date, Time, and Number Formats

Date and time formats vary significantly across regions. Using ISO standards avoids confusion.

**Date formats by region:**

```text
YYYY-MM-DD: ISO 8601, some Asian countries. Example: 2025-01-15
MM/DD/YYYY: United States. Example: 01/15/2025
DD/MM/YYYY: Europe, Latin America, Australia. Example: 15/01/2025
DD.MM.YYYY: Germany, Russia. Example: 15.01.2025
```

**Best practices for dates:**

```text
1. Use ISO 8601 (YYYY-MM-DD) in technical contexts — it is unambiguous.
2. Write out months in prose: "15 January 2025" (international format).
3. Specify the time zone for specific times.
4. Use UTC for server-side timestamps.
5. Avoid relative dates: "today", "tomorrow", "next week".
```

**Time formats:**

```text
12-hour clock (AM/PM): US, Canada, Australia, India
24-hour clock: Most of Europe, military, technical contexts
Best practice: Use 24-hour clock in technical documentation.
```

**Number formats:**

```text
Decimal separator: Period (.) in US/UK/Asia, comma (,) in Europe.
Thousands separator: Comma in US/UK, period in Europe, space in some regions.
Best practice: Use SI format (spaces for thousands, period for decimal).
```

**Currency formats:**

```text
Use ISO currency codes (USD, EUR, JPY) alongside or instead of symbols.
Specify clearly: "USD 1,234.56" not "$1,234.56".
```

**Time zones:**

```text
Use UTC offsets: UTC+5:30, UTC-8:00
Use IANA names: America/New_York, Asia/Tokyo
Avoid abbreviations like EST, PST — they are ambiguous.
```

### Units of Measurement

Different regions use different measurement systems.

**Measurement systems:**

```text
Metric (SI): Most countries worldwide. Meters, kilograms, Celsius.
Imperial/US Customary: United States, limited UK use. Feet, pounds, Fahrenheit.
Best practice: Provide metric values, add imperial in parentheses.
```

**Storage and data.** All countries use bytes, kilobytes, megabytes. Clarify binary vs. decimal: 1 KB = 1,000 bytes, 1 KiB = 1,024 bytes.

**Network speed.** Mbps and Gbps are universal. Specify megabit vs. megabyte: Mbps vs. MB/s.

**Temperature.** Data centers: use Celsius. Consumer devices: add Fahrenheit for US readers.

**Common conversion errors:**

```text
"Billion": US = 10^9, UK (traditional) = 10^12. Use scientific notation.
"Ton": US ton = 2,000 lb, UK ton = 2,240 lb, metric ton = 1,000 kg.
Use "metric ton" or convert to kilograms.
```

### Translation Readiness

Translation readiness means writing source text so it can be translated accurately and efficiently.

**Terminology consistency.** The single most important factor for translation quality:

```text
Use one term per concept. Do not call it "server configuration" in one place
and "server setup" in another.
Use one term per UI element. If the button says "Save", call it "Save"
everywhere.
Do not use synonyms for variation. Technical writing values consistency.
Create a terminology glossary for translators.
```

**Short sentences.** Short sentences are easier to translate:

```text
Hard to translate:
"After you configure the database server, which must be running on a
separate machine, you can start the application by running the deploy
script, but make sure you have configured the environment variables first."

Easy to translate:
"Configure the database server. The database server must run on a separate
machine. Then, configure the environment variables. Finally, run the deploy
script to start the application."
```

**Sentence length guidelines:** 15-20 words maximum. One idea per sentence. Avoid parenthetical asides.

**Controlled vocabulary.** Restrict words to a defined set:

```text
Authorized: "start" (meaning: make a process begin)
Prohibited: "launch", "boot", "fire up", "initiate", "spin up"

Authorized: "stop" (meaning: make a process end)
Prohibited: "kill", "terminate", "halt", "shut down", "abort"

Authorized: "change" (meaning: modify a value)
Prohibited: "alter", "modify", "adjust", "tweak"
```

**Writing for translation memory tools.** Translation memory (TM) stores previously translated segments and reuses them:

```text
TM-friendly writing:
- Write complete sentences.
- Do not split sentences with line breaks.
- Do not embed variables in the middle of sentences.
- Use consistent capitalization and punctuation.
- Keep placeholders separate from text.

Good: "The log file is at /var/log/{product}.log."
Bad: "The log file is at /var/log/app.log." (translator changes path)
```

**Variables and placeholders.** Use clear, descriptive placeholder names:

```text
Use: {product_name}, {version}, {date}
Not: {x}, {var1}, {val}
```

**Text length considerations.** Translated text often expands:

```text
English to German: +20-30%
English to Russian: +15-25%
English to Spanish: +15-20%
English to Japanese: -10-20% (characters)
English to Chinese: -20-30% (characters)

Do not hard-code text lengths. Use flexible containers in UI layouts.
```

**Strings to externalize.** Every user-facing text string should be externalized from code:

```text
Error messages, button labels, menu items, tooltips, status messages,
confirmation dialogs, help text, documentation text.
```

### Localization vs. Internationalization

These terms describe different activities:

**Internationalization (i18n).** Designing content and systems so they can be adapted to different languages without engineering changes:

```text
- Use Unicode (UTF-8) encoding
- Externalize all user-facing strings
- Design layouts that accommodate text expansion
- Support right-to-left text flow
- Use locale-agnostic date/time/number formats
- Separate content from code
- Document terminology and writing conventions
```

**Localization (l10n).** Adapting content for a specific language and region:

```text
- Translate text strings
- Adapt date, time, number, and currency formats
- Convert units of measurement
- Replace culturally inappropriate images or icons
- Adjust colors and symbols for cultural appropriateness
- Test with native speakers
```

**The relationship.** Internationalization enables localization. Without it, each locale requires code changes, string lengths break layouts, and localization is slow and expensive. With it, localization is purely content work.

**Internationalization checklist:**

```text
- [ ] UTF-8 encoding used throughout
- [ ] No text embedded in images
- [ ] All text strings externalized
- [ ] Date/time formats are locale-aware or use ISO
- [ ] Number formats are locale-aware
- [ ] Terminology is consistent
- [ ] Sentences are short (under 20 words)
- [ ] No idioms, metaphors, or cultural references
- [ ] Variables and placeholders are clearly marked
- [ ] Units of measurement are specified or dual-provided
- [ ] Colors are not the sole carrier of meaning
- [ ] Directional language is avoided
- [ ] Link text is descriptive
- [ ] Alt text is provided for all images
```

### Writing for Non-Native English Speakers

Most international readers are non-native English speakers. Writing for them requires specific techniques.

**Vocabulary choices:**

```text
Use common English words over rare ones.
Avoid regional vocabulary: "apartment" not "flat", "truck" not "lorry".
Avoid sophisticated synonyms: "stop" not "terminate", "end" not "cease".
```

**Sentence structure.** Put the main subject and verb early:

```text
Accessible: "If the server fails to start, check the log file."
Difficult: "Should the server fail to start, which can happen due to port
conflicts or configuration errors, checking the log file may provide
diagnostic information."
```

**Verb tense and mood:**

```text
Use present tense for most statements.
Use imperative mood for instructions.
Avoid future tense when present tense works.
Use "can" for possibility, "must" for requirement.
```

**Articles and prepositions.** Be consistent:

```text
Always use articles with countable singular nouns: "Open the file."
Use consistent prepositions: "click" (not "click on"), "log in to" (not "log into").
```

**False friends.** Words that look similar in two languages but have different meanings:

```text
"Actually" -> looks like "actualmente" (Spanish, means "currently")
Better: "in fact" or "really"

"Sensible" -> looks like "sensible" (French/Spanish, means "sensitive")
Better: "practical" or "reasonable"

"Eventually" -> looks like "eventualmente" (Spanish, means "possibly")
Better: "finally" or "in the end"
```

**High-frequency words.** Non-native speakers know the most common 2,000-3,000 English words. Favor them:

```text
Verbs: use, make, take, get, give, put, set, turn, start, stop, run
Nouns: file, system, user, name, value, time, type, list, number
Adjectives: new, old, good, bad, high, low, full, empty, open, closed
Prepositions: in, on, at, to, for, with, from, by, as, of
```

**Common grammar challenges:**

```text
Phrasal verbs — avoid or explain:
"Set up" -> "configure" or "create"
"Shut down" -> "stop" or "power off"
"Go through" -> "review" or "check"

Conditional structures — simplify:
Complex: "Had the server been configured correctly, the error would not
have occurred."
Simple: "If you configure the server correctly, the error does not occur."

Negation — use direct:
Difficult: "The server is not incapable of handling the load."
Clear: "The server can handle the load."
```

### Testing with International Readers

Testing is essential to produce documentation that works globally.

**Types of international testing:**

```text
Readability testing:
- Measure reading grade level (target grade 6-8).
- Use Hemingway Editor or Flesch-Kincaid Grade Level.

Translation quality testing:
- Have professional translators review translated content.
- Back-translate key documents and compare meaning.
- Check for untranslated strings.
- Verify variable handling in translated text.

Usability testing with non-native speakers:
- Recruit participants whose first language is not English.
- Ask them to complete tasks using your documentation.
- Measure task completion time and error rates.
- Interview about confusing terms or structures.

Cultural review:
- Have a local reviewer check cultural appropriateness.
- Verify images, colors, symbols, and examples.
- Confirm measurements and formats match local conventions.
- Review legal disclaimers for local compliance.
```

**Questions for international usability testing:**

```text
1. Did you understand every sentence on the first read?
2. Were there any words you needed to look up?
3. Were the examples relevant to your context?
4. Did you find any cultural references confusing?
5. Were the date/time/number formats clear?
6. Did the instructions match workflows in your region?
7. Would you use different terms in your locale?
```

**Continuous testing approach:**

```text
- Include readability checks in CI pipeline.
- Run Vale with internationalization rules.
- Test translation round-trip (translate, translate back, compare).
- Conduct quarterly reviews with international contributors.
- Maintain a feedback channel for global readers.
```

**Tools for international testing:**

```text
Readability: Hemingway Editor, Readable, Yoast Readability
Locale testing: ICU Message Format testers, pseudo-localization, RTL testers
Translation verification: TM consistency checks, glossary compliance tools
```

## Study Cases

### Case 1: Rewriting for Translation Readiness

```text
Original:
"When you're done tinkering with your config, just give the green button a
poke and the system will take it from there — it'll spin up your containers
lickety-split."

Problems:
- "tinkering" is informal, hard to translate
- "give the green button a poke" is an idiom
- "take it from there" is conversational
- "spin up" is a phrasal verb
- "lickety-split" is an uncommon idiom

Internationalized version:
"After you finish editing the configuration, click the **Deploy** button.
The system builds and starts the containers. The deployment usually takes
less than one minute."

Changes: All idioms and informal language replaced with literal, precise
language. Each sentence expresses one idea.
```

### Case 2: Date Format Error in API Documentation

```text
Original:
"The endpoint accepts a date parameter in MM/DD/YYYY format.
Example: 03/05/2025 means March 5, 2025."

Problem: A developer in Germany reads 03/05/2025 as May 3, 2025. The
MM/DD/YYYY format causes data errors.

Revision:
"The endpoint accepts a date parameter in YYYY-MM-DD format.
Example: 2025-03-05 means March 5, 2025."

Impact: ISO 8601 eliminates ambiguity worldwide. The example explicitly
states the meaning in prose.
```

### Case 3: Removing Cultural References

```text
Original:
"This tool is like an MVP in baseball — it's the most valuable player on
your team. Just like a pinch hitter, it comes through when needed."

Problems: Baseball is not a global sport. "MVP" and "pinch hitter" are
baseball-specific. The analogy adds no technical information.

Revision:
"This tool handles the most complex part of your deployment pipeline. It
reduces deployment time by automating dependency management."

Changes: Remove cultural analogy entirely. Replace with direct, factual
description. State the benefit without metaphor.
```

### Case 4: Translation Cost Reduction

```text
Scenario: A company with products in 20 languages reviews their
documentation for translation readiness.

Issue 1: Inconsistent terminology
Original uses "start", "launch", "initiate", and "run" interchangeably.
Translation memory cannot match across variants.
Estimated annual cost: $12,000 in extra translation.
Fix: Standardize to "run" for executing processes.

Issue 2: Long sentences
Average sentence length: 28 words. Longest: 67 words.
Complex sentences require per-sentence complexity pricing.
Estimated annual cost: $8,000 in surcharges.
Fix: Restructure to 18-word average. Flat-rate pricing now applies.

Issue 3: Untranslatable text in images
45 screenshots with embedded English text.
Each requires image edit per locale.
Estimated annual cost: $15,000.
Fix: Remove text from images. Use callouts with numbered labels.

Total annual savings: $35,000.
```

## Examples

### Example 1: Idiom Replacement

```text
Original: "To get the ball rolling, install the dependencies."
Better:   "To start, install the dependencies."

Original: "Once you have the hang of it, the process is a piece of cake."
Better:   "After you learn the process, it becomes easy."
```

### Example 2: Date Format Standardization

```text
Ambiguous: "The update was released on 06/07/2025."
Clear:     "The update was released on 2025-07-06."

Ambiguous: "The sale ends 1/2/25."
Clear:     "The sale ends 2025-02-01 (February 1, 2025)."
```

### Example 3: Cultural Symbol Awareness

```text
Original: "Checkmark icon: Project created successfully"
Issue: In Japan, checkmarks can indicate errors in some contexts.
Better:  "Project created successfully" (text only)
```

### Example 4: Translation-Ready Instructions

```text
Original:
"When you've got your database all set up, which should take about 10
minutes, you'll want to configure the connection string by navigating to
the Settings section of the control panel, and if you run into any issues,
check out the troubleshooting guide."

Translation-ready:
"Set up the database. This takes about 10 minutes. Then, configure the
connection string. Go to **Settings** in the control panel. If you have
problems, see the troubleshooting guide."
```

### Example 5: Measurement Conversion

```text
US-only:  "Operating temperature: 32-95 degrees Fahrenheit."
Global:   "Operating temperature: 0-35 degrees Celsius."

US-only:  "The rack is 42U tall and 24 inches deep."
Global:   "The rack is 42U tall and 610 mm (24 in) deep."
```

### Example 6: Phrasal Verb Simplification

```text
Original: "Power up the device and plug in the cable."
Better:   "Turn on the device and connect the cable."

Original: "Set up the environment before you carry out the test."
Better:   "Configure the environment before you run the test."
```

### Example 7: Readability Grade Levels

```text
Grade 14: "The implementation of a comprehensive caching strategy
necessitates careful consideration of the eviction policy."

Grade 8:  "Choose an eviction policy for your cache. The eviction policy
determines how the system removes entries when the cache is full."

Grade 6:  "Choose how your cache removes old data. The cache removes data
when it runs out of space. The eviction policy controls this process."
```

### Example 8: Terminology Consistency Table

```text
| Concept | Approved Term | Prohibited Terms |
|---------|---------------|------------------|
| Execute a program | run | execute, launch, invoke |
| End a program | close | kill, terminate, destroy |
| Enter the system | sign in | login, authenticate |
| Modify a setting | change | adjust, tweak, alter |
| Install software | install | set up, deploy |
| Remove software | uninstall | delete, deactivate |
| Verify correctness | check | validate, verify, confirm |
```

## Glossary

| Term | Definition |
|------|------------|
| Controlled vocabulary | A restricted set of approved terms for consistent usage |
| Cultural reference | An allusion to a specific culture's customs or symbols |
| False friend | A word that looks similar in two languages but has a different meaning |
| i18n | Abbreviation for internationalization (18 letters between i and n) |
| Idiom | A phrase whose meaning is not predictable from its individual words |
| Internationalization | Designing content to be easily adaptable to different languages |
| l10n | Abbreviation for localization (10 letters between l and n) |
| Locale | A specific combination of language and region (e.g., en-US, pt-BR) |
| Localization | Adapting content for a specific language and region |
| Metaphor | A figure of speech that describes something as something else |
| Phrasal verb | A verb combined with a preposition ("set up", "shut down") |
| Pseudo-localization | Artificially generating translated text to test layouts |
| Right-to-left (RTL) | Text that reads right to left (Arabic, Hebrew, Persian) |
| Simplified English | A controlled language with restricted vocabulary and grammar |
| Translation memory | A database of previously translated segments for reuse |
| Unicode | A character encoding standard supporting all writing systems |
| UTF-8 | A variable-width Unicode encoding, standard for the web |

## Quick References

- [W3C Internationalization](https://www.w3.org/International/) — guidelines for global web content
- [Unicode Consortium](https://unicode.org/) — character encoding standards
- [ISO 8601 Date Format](https://www.iso.org/iso-8601-date-and-time-format.html) — international date format standard
- [Microsoft Style Guide: Global Communications](https://learn.microsoft.com/en-us/style-guide/global-communications/) — Microsoft's guidance for international audiences
- [Google Developer Documentation Style Guide: Internationalization](https://developers.google.com/style/internationalization) — Google's guidance
- [Simplified English Standard (ASD-STE100)](https://asd-ste100.org/) — aerospace standard for Simplified English
- [Hemingway Editor](https://hemingwayapp.com/) — readability analysis tool
- [Common Locale Data Repository (CLDR)](https://cldr.unicode.org/) — locale data for formatting
- [ICU Message Format](https://unicode-org.github.io/icu/) — internationalization library

## Next Steps

- [Technical Style Guides: Microsoft, Google, Apple](style-guides.md) — how major style guides handle internationalization
