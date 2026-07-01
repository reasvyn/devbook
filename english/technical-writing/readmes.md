# README-Driven Development

## Description

README-driven development is the practice of writing the README before writing the code. The README becomes a specification, a design tool, and a marketing document all in one. This document covers how to write effective READMEs and use them as the foundation of your development process.

## Prerequisites

- [Conciseness](../grammar-and-style/conciseness.md) — READMEs demand concise, scannable writing

## Table of Contents

- [What Is README-Driven Development?](#what-is-readme-driven-development)
- [Writing the README Before the Code](#writing-the-readme-before-the-code)
- [The README as a Specification](#the-readme-as-a-specification)
- [Structure of a Good README](#structure-of-a-good-readme)
- [Title and Description](#title-and-description)
- [Installation Section](#installation-section)
- [Usage Section](#usage-section)
- [API Section](#api-section)
- [Contributing Section](#contributing-section)
- [License Section](#license-section)
- [GitHub README Conventions and Formatting](#github-readme-conventions-and-formatting)
- [README as Marketing](#readme-as-marketing)
- [Badge Culture](#badge-culture)
- [README Generators and Templates](#readme-generators-and-templates)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### What Is README-Driven Development?

README-driven development is a workflow where you write the README file for a project before you write any code. The README serves as a lightweight specification that forces you to clarify your project's purpose, API, and usage before implementation begins.

**The core idea:**

```text
1. Write the README first — define the API, usage, and design
2. Review the README with stakeholders — validate the design
3. Implement the code — make it match the README
4. Update the README only when the design changes
```

**Benefits of README-driven development:**

```text
Design clarity: Writing docs first forces you to think through the API design
before coding. Edge cases and inconsistencies surface early.

Better APIs: When you document the API before implementing it, you evaluate
it from the consumer's perspective. This leads to more intuitive interfaces.

Less rework: Design issues caught at the README stage cost nothing to fix.
Design issues caught after implementation require refactoring.

Natural specification: The README becomes the source of truth. Code is
written to match it, not the other way around.

Marketing alignment: The README is also your project's front page. Writing
it first ensures the messaging is clear from day one.
```

### Writing the README Before the Code

**The README-first workflow in practice:**

```text
Step 1: Define the problem
- What does this project do?
- Who is it for?
- What problem does it solve?

Step 2: Define the API
- What are the main functions, classes, or commands?
- What inputs do they take?
- What outputs do they produce?

Step 3: Write usage examples
- Show the simplest possible usage
- Show one or two common patterns

Step 4: Define the installation
- How does someone get started?
- What are the dependencies?

Step 5: Review and refine
- Share the README with potential users
- Incorporate feedback before writing code

Step 6: Implement and match
- Write code that fulfills the README
- Keep the README updated as designs evolve
```

**What to capture at each stage:**

```text
Before coding:
- Project name and tagline
- One-paragraph description
- Code example showing primary use case
- Installation command
- API signatures (function names, parameters, return types)

During implementation:
- Detailed usage examples
- Error handling documentation
- Performance considerations

After implementation (rarely changes):
- Contributing guidelines
- Changelog
- License
```

### The README as a Specification

A README written first is a specification document. It must be precise enough that a developer can implement from it.

**Specification-quality README excerpt:**

```text
# csv2json

Convert CSV files to JSON with schema validation.

## API

### parse(csvString, options?)

Parses a CSV string and returns an array of objects.

Parameters:
- csvString (string): The CSV content to parse. Must contain a header row.
- options (object, optional):
  - delimiter (string, default: ","): Field delimiter character
  - headers (boolean, default: true): Whether the first row is a header
  - validate (boolean, default: false): Validate against schema if provided

Returns: Array<Record<string, string>>

Throws:
- ParseError: If the CSV is malformed
- ValidationError: If data does not match the schema

Example:
import { parse } from "csv2json";
const result = parse("name,age\nAlice,30\nBob,25");
// [{ name: "Alice", age: "30" }, { name: "Bob", age: "25" }]
```

**What makes a README a good specification:**

```text
Precision: Every parameter, return value, and error is documented
Unambiguous: Examples show exact input and output
Testable: A developer can write tests directly from the README
Complete: All public API surfaces are covered
Minimal: Internal implementation details are not in the spec
```

### Structure of a Good README

A well-structured README follows a predictable pattern that readers can scan quickly.

**Standard README structure:**

```text
1. Title — Project name with a tagline
2. Description — What and why (1-2 paragraphs)
3. Badges — Status, build, coverage, version
4. Table of Contents — Navigation for long READMEs
5. Installation — How to install or set up
6. Usage — Quick start example
7. API — Detailed API documentation (or link to it)
8. Configuration — Environment variables, options
9. Contributing — How to contribute
10. License — Legal terms
11. Acknowledgments — Credits
```

**Long README vs. short README:**

```text
Short README (libraries, small tools):
- Title and badge
- One-liner description
- Installation command
- Usage example
- API summary
- License

Long README (frameworks, platforms, large projects):
- Title, badges, table of contents
- Multi-paragraph description with screenshots
- Installation with multiple methods
- Multiple usage examples (basic, advanced, error cases)
- Full API reference (or link to dedicated docs)
- Contributing guide
- Changelog
- License
```

### Title and Description

The title and description are the most-read parts of any README. They determine whether a reader continues or moves on.

**Title conventions:**

```text
# Project Name

Avoid:
- All caps names
- Unnecessary prefixes (lib-, js-, node-)
- Version numbers in the title
- Punctuation or exclamation marks

Good examples:
# csv2json
# React Router
# Docker Compose
```

**Description guidelines:**

```text
Bad:
csv2json is a tool for converting CSV files to JSON format.

Good:
csv2json converts CSV files to JSON with optional schema validation.
It handles large files via streaming, supports custom delimiters,
and produces type-safe output for use in data pipelines.

The description should answer:
1. What does it do? (one clear sentence)
2. Why is it different? (what problem does it solve better)
3. Who is it for? (target audience if relevant)
```

**Tagline format:**

```text
# csv2json

Convert CSV files to JSON with schema validation.

# React Router

Declarative routing for React applications.

Keep the tagline under 10 words. It appears in search results and package registries.
```

### Installation Section

The installation section must give every reader a clear path to running the project.

**Single install method:**

```text
## Installation

npm install csv2json
```

**Multiple install methods:**

```text
## Installation

### npm
npm install csv2json

### Yarn
yarn add csv2json

### From source
git clone https://github.com/user/csv2json.git
cd csv2json
npm install
npm run build
```

**System requirements:**

```text
## Requirements

- Node.js 18 or later
- npm 9 or later
```

### Usage Section

The usage section is where readers learn how the project actually works. It should show the most common use case first.

**Minimal usage example:**

```text
## Usage

Convert a CSV file to JSON:

csv2json input.csv > output.json
```

**Step-by-step usage:**

```text
## Usage

### Basic conversion

Convert a simple CSV file to JSON:

csv2json data.csv > data.json

### With schema validation

csv2json data.csv --schema schema.json > validated.json

### Streaming large files

csv2json large.csv --stream > output.json
```

**Usage examples with expected output:**

```text
csv2json sample.csv
# [
#   { "name": "Alice", "age": "30" },
#   { "name": "Bob", "age": "25" }
# ]
```

**Usage for libraries (code-based):**

```js
import { parse } from "csv2json";

const csv = `name,age
Alice,30
Bob,25`;

const result = parse(csv, { headers: true });
console.log(result);
// [
//   { name: "Alice", age: "30" },
//   { name: "Bob", age: "25" }
// ]
```

### API Section

For libraries and tools, the API section is the most important part of the README. It must be complete and precise.

**API documentation structure:**

```text
## API

### parse(csvString, options?)

Parses a CSV string and returns an array of objects.

#### Parameters

| Parameter   | Type   | Default | Description                     |
|-------------|--------|---------|---------------------------------|
| csvString   | string | —       | CSV content with header row     |
| options     | object | {}      | Parsing options (see below)     |

#### Options

| Option    | Type    | Default | Description                     |
|-----------|---------|---------|---------------------------------|
| delimiter | string  | ","     | Field delimiter character       |
| headers   | boolean | true    | Treat first row as header       |
| validate  | boolean | false   | Validate against provided schema|

#### Returns

`Array<Record<string, string>>` — Array of objects with header row values as keys.

#### Throws

| Error          | Condition                              |
|----------------|----------------------------------------|
| ParseError     | CSV is malformed (mismatched quotes)   |
| ValidationError| Validation fails and validate is true  |
```

### Contributing Section

The contributing section sets expectations for how others can participate in the project.

**Minimal contributing section:**

```text
## Contributing

Contributions are welcome. Please open an issue first to discuss
significant changes.

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Run tests (npm test)
5. Submit a pull request
```

**Comprehensive contributing section:**

```text
git clone https://github.com/user/csv2json.git
cd csv2json
npm install
npm test
npm run lint

PR process:
1. Ensure all tests pass
2. Add tests for new features
3. Update the README if the API changes
4. Submit the PR with a clear description
```

### License Section

The license section is often the most overlooked part of a README, but it carries legal weight.

**Standard license format:**

```text
## License

MIT (c) Jane Smith

See LICENSE for the full license text.
```

**License badge integration:**

```text
## License

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

This project is licensed under the MIT License.
```

**Common open-source licenses for developer tools:**

```text
MIT — Permissive. Use in commercial projects. No warranty.
Apache 2.0 — Permissive with patent protections.
GPL 3.0 — Copyleft. Derivative works must also be GPL.
BSD 3-Clause — Permissive. Similar to MIT.
```

### GitHub README Conventions and Formatting

GitHub renders Markdown with specific conventions that README authors should understand.

**GitHub-flavored Markdown features:**

```text
Syntax highlighting in code blocks:
```python
def hello():
    print("Hello")
```

Task lists:
- [x] Implement parser
- [ ] Add schema validation

Tables:
| Feature | Status |
|---------|--------|
| Parsing | Done   |

Alerts (GitHub 2024+):
> [!NOTE]
> Useful information for users.

> [!WARNING]
> Important cautionary information.

> [!TIP]
> Helpful advice for a better experience.
```

**README visual hierarchy:**

```text
Use headings to establish clear hierarchy:
# Project Name          — only one H1
## Description          — H2 for major sections
### Installation        — H3 for subsections

Keep the first screenful (above the fold) focused:
1. Project name and badges
2. One-sentence description
3. Installation command
4. Minimal usage example
```

### README as Marketing

For open-source projects, the README is the primary marketing page. It is what developers see when they discover your project on GitHub, npm, or other package registries.

**Marketing elements in a README:**

```text
Above the fold (must capture attention):
- Clear project name and tagline
- Badges showing project health
- One-sentence value proposition
- Screenshot or demo GIF (if applicable)
- Installation command

Below the fold (convert interest to adoption):
- Detailed usage examples
- Feature list
- Comparison with alternatives
```

**Screenshot and demo placement:**

```text
Place the primary screenshot or GIF immediately after the tagline.
Keep screenshots small. Animated GIFs under 5MB.
```

**Feature comparison table:**

```text
Comparison tables must be accurate and fair. A misleading comparison
damages credibility when readers discover the truth.
```

### Badge Culture

Badges show project metadata. Place them in a single row below the title. Keep to 5-7 maximum. Only include useful badges — broken badges damage credibility.

### README Generators and Templates

Several tools can help scaffold a README from templates or prompts.

**Common README generators:**

```text
readme-md-generator: npm install -g readme-md-generator. Interactive CLI.
Standard Readme: npm install -g standard-readme. Follows standard-readme spec.
```

**When to use a generator:**

```text
Use a generator for common patterns (library, CLI, app). Write from scratch
for projects with unique documentation or marketing needs.
```

## API

### parse(data, options)
### parseFile(path, options)
### parseStream(stream, options)
```

During review, a team member notices that all three functions have similar signatures. The team realizes that a single `parse` function using streams or string input would be simpler.

```text
// Revised README after review
## API

### parse(input, options?)

Parses CSV from a string, file path, or ReadableStream.

Parameters:
- input (string | ReadableStream): CSV content or file path
- options (object, optional): Parsing options

Returns: Promise<Array<Record<string, string>>>
```

The team implements against the revised API. The README-first approach prevented shipping a redundant, confusing API. The fix cost zero implementation time because no code had been written yet.

### Case 2: Rewriting a Confusing README

```text
Before:
csv2json is a node module that can be used to convert csv files
to json format. It is written in typescript and is very fast.
It handles large files and has many configuration options.

Usage:
const csv2json = require("csv2json");
const result = csv2json("data.csv");
console.log(result);

After:
# csv2json

Convert CSV files to JSON with schema validation.

## Installation

npm install csv2json

## Usage

import { parse } from "csv2json";

const data = await parse("data.csv", { headers: true });
console.log(data);
// [{ name: "Alice", age: "30" }, ...]
```

Changes: Clear title and tagline, removed fluff, added ESM import, showed actual output.

### Case 3: Badge Overload Cleanup

```text
Before:
[![Build](...)] [![Coverage](...)] [![License](...)] [![Version](...)]
[![Downloads](...)] [![Stars](...)] [![Forks](...)] [![Issues](...)]
[![PRs](...)] [![Twitter](...)] [![Chat](...)] [![Dependencies](...)]

After:
[![Build](...)] [![Coverage](...)] [![License](...)] [![npm](...)]

Reduced from 12 badges to 4. Removed badges provided information that
was either obvious (stars, forks) or better viewed on the GitHub UI (issues).
```

## Usage

csv2json data.csv --output data.json

## License

MIT
```

### Example 2: Library README Template

```markdown
# prettier-plugin-sql

[![CI](CI)] [![npm](npm)]

A Prettier plugin for formatting SQL queries.

npm install prettier-plugin-sql --save-dev

```js
// prettier.config.js
module.exports = {
  plugins: ["prettier-plugin-sql"],
};
```

Before:
SELECT id, name FROM users

After:
SELECT id, name
FROM users
```

### Example 3: README-First Specification Fragment

```markdown
# semver-check

Compare semantic versions and check constraint satisfaction.

## Installation

npm install semver-check

## API

### satisfies(version, range)

Checks if a version satisfies a semver range.

Parameters:
- version (string): A semantic version string (e.g., "1.2.3")
- range (string): A semver range (e.g., "^1.0.0" or ">=2.0.0 <3.0.0")

Returns: boolean

Example:
satisfies("1.5.0", "^1.0.0") // true
satisfies("2.0.0", "^1.0.0") // false
```

### Example 4: README Table of Contents

```markdown
## Table of Contents

- [Installation](#installation)
- [Usage](#usage)
- [API](#api)
- [Configuration](#configuration)
- [Contributing](#contributing)
- [License](#license)
```

### Example 5: Badge Row Combinations

```markdown
# Project Name

[![Build Status](CI)]
[![codecov](COVERAGE)]
[![npm version](NPM)]
[![License: MIT](LICENSE)]
[![PRs Welcome](CONTRIBUTING)]
```

## Glossary

| Term | Definition |
|------|------------|
| README | A documentation file that introduces and explains a project |
| README-driven development | Writing the README before writing code, using it as a specification |
| Badge | A small image indicating project metadata (build status, version, license) |
| Shields.io | A service for generating custom badge images |
| Tagline | A short phrase summarizing the project, placed below the title |
| Above the fold | The content visible without scrolling on the repository page |
| Specification | A precise description of what a program does and how to use it |
| Value proposition | A statement of the benefit a project provides to its users |
| Social proof | Evidence that others use or trust a project |
| Table of contents | A navigational listing of section headings in the README |
| Code of conduct | A document establishing expectations for participant behavior |
| Changelog | A log of notable changes for each release of a project |

## Quick References

- [Make a README](https://www.makeareadme.com/) — guide to writing effective READMEs
- [Standard Readme](https://github.com/RichardLitt/standard-readme) — a README specification and generator
- [Shields.io](https://shields.io/) — badges for your README
- [Awesome README](https://github.com/matiassingers/awesome-readme) — curated list of great README examples
- [GitHub-flavored Markdown](https://docs.github.com/en/get-started/writing-on-github)
- [Readme Driven Development (Tom Preston-Werner)](https://tom.preston-werner.com/2010/08/23/readme-driven-development.html) — the original blog post

## Next Steps

- [API Docs](api-docs.md) — writing comprehensive API reference documentation
- [Tutorials](tutorials.md) — creating step-by-step guides for getting started
