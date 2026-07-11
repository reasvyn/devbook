# Tutorials & Getting Started Guides

## Description

Tutorials are the first experience many developers have with your product. A good tutorial teaches by doing — it guides the reader through a complete, working example while explaining the concepts along the way. This document covers how to write tutorials that educate without frustrating.

## Prerequisites

- [READMEs](readmes.md) — tutorials often follow the README-first development pattern

## Table of Contents

- [What Makes a Good Tutorial](#what-makes-a-good-tutorial)
- [Tutorial Structure](#tutorial-structure)
- [Prerequisites Section](#prerequisites-section)
- [Objectives Section](#objectives-section)
- [Step-by-Step Instructions](#step-by-step-instructions)
- [Setting Up Reproducible Environments](#setting-up-reproducible-environments)
- [Avoiding Tutorial Hell](#avoiding-tutorial-hell)
- [Teaching Concepts vs. Teaching Tool Usage](#teaching-concepts-vs-teaching-tool-usage)
- [Verifying Outcomes at Each Step](#verifying-outcomes-at-each-step)
- [Handling Errors in Tutorials](#handling-errors-in-tutorials)
- [Screenshot and Diagram Use](#screenshot-and-diagram-use)
- [Writing for Different Experience Levels](#writing-for-different-experience-levels)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### What Makes a Good Tutorial

A good tutorial is a guided learning experience. It differs from a how-to guide in an important way: a how-to guide solves a specific problem, while a tutorial builds understanding.

**Attributes of a good tutorial:**

```text
Self-contained: The reader can complete it with only the listed prerequisites
Verifiable: Each step produces a visible result the reader can confirm
Scaffolded: Concepts are introduced one at a time, building on what came before
Complete: The reader ends with a working result — a deployed app, a completed analysis
Minimal: Only essential steps are included. Optional tangents are marked as such

Attributes of a bad tutorial:
- Assumes knowledge the reader does not have
- Skips steps ("install the dependencies" without saying which)
- Does not explain why things work
- Leaves the reader with an incomplete or broken result
- Gets interrupted by version-specific details
```

**The tutorial writer's checklist:**

```text
Before: Can a beginner complete it in one sitting? Tested from scratch? Dependencies documented?
During: Each step explains what and why? Verification points included? Code copy-pasteable?
After: Tested on clean machine? Time estimate accurate? External links valid?
```

### Tutorial Structure

Every tutorial should follow a predictable structure that sets expectations and guides the reader through the learning process.

**Standard tutorial format:**

```text
# Tutorial Title

Description: One paragraph explaining what this tutorial covers and why it matters.

## Prerequisites

What the reader needs before starting — tools, accounts, prior knowledge.

## Objectives

What the reader will achieve by completing this tutorial.

## Step 1: [Action-oriented title]

Instructions with code blocks and verification.

## Step 2: [Action-oriented title]

...

## Summary

Restate what was accomplished and suggest next steps.

## Next Steps

Links to related tutorials or how-to guides.
```

**Time estimates:**

```text
Include an estimated completion time in the title or description:

"Build Your First REST API (30 minutes)"

Common formats:
- (15 minutes)
- (30 minutes)
- (1 hour)
- Two-part: "Part 1: Setup (20 minutes)" and "Part 2: Implementation (30 minutes)"
```

**The inverted pyramid for tutorial steps:**

```text
Each step should follow this pattern:

1. State the goal: "Now we will create the database schema."
2. Explain briefly: "This schema defines the structure of our data."
3. Show the action: Provide the SQL or code to execute.
4. Verify the result: "You should see the tables 'users' and 'orders' listed."
5. Explain what happened: "The CREATE TABLE statement created two tables..."

This pattern moves from concrete (do this) to abstract (why it matters),
which keeps readers engaged while building understanding.
```

### Prerequisites Section

The prerequisites section is a contract between the tutorial and the reader. If the reader meets the prerequisites, the tutorial should work. If not, the tutorial should tell them where to get what they need.

**Prerequisite types:**

```text
Required tools:
- Node.js 18 or later
- Docker Desktop
- A code editor (VS Code recommended)

Required accounts:
- A GitHub account
- An AWS account (free tier is sufficient)
- A Stripe test account

Required knowledge:
- Basic JavaScript (variables, functions, async/await)
- Familiarity with the command line
- Understanding of REST API concepts

Recommended (not required):
- Experience with React (helpful but not essential)
- Familiarity with TypeScript
```

**Prerequisite formatting:**

```text
## Prerequisites

Before starting this tutorial, make sure you have:

- [Node.js](https://nodejs.org) 18 or later installed
  (Check with: `node --version`)
- [Docker Desktop](https://www.docker.com/products/docker-desktop/) installed
  (Check with: `docker --version`)
- A [GitHub account](https://github.com/join)
  (Sign up if you do not have one)

You should also be familiar with:
- Basic command-line operations
- JavaScript syntax (arrow functions, promises)
```

**Prerequisite verification commands:**

```text
Always include commands to verify prerequisites:

```bash
node --version
# Should output: v18.x.x or later

docker --version
# Should output: Docker version 24.x.x or later

git --version
# Should output: git version 2.x.x or later
```
```

### Objectives Section

The objectives section tells the reader exactly what they will accomplish. It sets expectations and provides a sense of progress as each objective is completed.

**Writing good objectives:**

```text
Good objectives are:
- Concrete: "Create a Node.js server that responds to HTTP requests"
- Verifiable: "Deploy the application to a production URL"
- Measurable: "Write five unit tests that all pass"

Bad objectives are:
- Vague: "Learn about web development"
- Unverifiable: "Understand how servers work"
- Too broad: "Become an expert in Node.js"

Example objectives section:

## Objectives

By the end of this tutorial, you will have:

1. Created a Node.js project with Express
2. Defined three API endpoints (GET, POST, DELETE)
3. Connected to a PostgreSQL database
4. Deployed the application to Railway
5. Verified all endpoints return correct responses
```

**Objective checkboxes in the summary:**

```text
## Summary

Congratulations! You have completed the tutorial. You should now have:

- [x] A Node.js project with Express installed
- [x] API endpoints for GET, POST, and DELETE operations
- [x] A PostgreSQL database connection
- [x] A deployed application on Railway
```

### Step-by-Step Instructions

The core of any tutorial is the step-by-step instructions. Each step should be actionable, verifiable, and appropriately sized.

**Step size principles:**

```text
Each step should take 2-5 minutes to complete. If a step takes longer,
break it into substeps. If a step takes less than 30 seconds, consider
combining it with another step.

Step 1: Install dependencies (2 minutes)
  npm install express pg dotenv

Step 2: Create the database schema (5 minutes)
  Write SQL file, run migration

Step 3: Implement the server (5 minutes)
  Write the Express server code

Step 4: Test the endpoints (3 minutes)
  Start the server, make requests with curl
```

**Code blocks in tutorials:**

```text
Every code block must be:
1. Complete enough to understand alone
2. Minimal enough to focus on the concept
3. Preceded by explanation of what it does
4. Followed by an explanation of how it works

// Example of good code block presentation:

Create a file called `server.js` and add the following code:

```javascript
const express = require("express");
const app = express();
const PORT = process.env.PORT || 3000;

app.get("/", (req, res) => {
  res.json({ message: "Hello, World!" });
});

app.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`);
});
```

This code creates an Express server that listens on port 3000
and responds with JSON when you visit the root URL.

```

**Instructions style guide for tutorials:**

```text
Do:
- Start each instruction with a verb: "Create", "Run", "Open", "Add"
- Use numbered steps for sequential actions
- Use bullet lists for items that can be done in any order
- Put commands in code blocks with the prompt omitted or consistent
- Show the expected output after each command

Don't:
- Use passive voice ("the file should be created")
- Assume the reader knows how to do something without instruction
- Use vague language ("set up the configuration appropriately")
- Skip error scenarios without mentioning them
```

### Setting Up Reproducible Environments

A tutorial that does not work on the reader's machine is worse than no tutorial at all. Reproducible environments are essential.

**Environment reproducibility techniques:**

```text
Pre-configured starter repos:
- Provide a GitHub template repository with all dependencies configured
- Reader clones the repo and starts from a known state
- Example: https://github.com/example/tutorial-starter

Docker-based environments:
- Provide a Dockerfile and docker-compose.yml
- Reader runs one command to start the environment
- Eliminates "it works on my machine" problems

Dev containers:
- .devcontainer configuration for VS Code
- Reader opens in container with everything pre-installed
- Works on any OS without installation conflicts

Cloud-based environments:
- Gitpod, GitHub Codespaces, or Replit
- Reader starts a pre-configured environment in the browser
- Zero local setup required
```

**Describing environment setup in the tutorial:**

```text
Option 1: Clone the starter template (recommended)

```bash
git clone https://github.com/example/tutorial-starter.git
cd tutorial-starter
npm install
```

Option 2: Start from scratch

If you prefer to build from scratch, create a new directory and
initialize the project:

```bash
mkdir my-project
cd my-project
npm init -y
npm install express pg dotenv
```

Option 3: Use the Docker environment

```bash
docker compose up -d
docker compose exec app bash
```
```

**Version pinning for reproducibility:**

```text
Always pin dependency versions in tutorials:

// Good — pinned versions
npm install express@4.18.2 pg@8.11.3 dotenv@16.3.1

// Bad — may break when new versions are released
npm install express pg dotenv

// For frameworks:
// Good — specify the version
npx create-react-app@5.0.1 my-app

// Good — specify language version in .nvmrc or .tool-versions
echo "18" > .nvmrc
```

### Avoiding Tutorial Hell

Tutorial hell is the state where a learner follows tutorial after tutorial without being able to build anything independently.

**How to design tutorials that prevent tutorial hell:**

```text
1. Include deliberate practice exercises: "Now modify the code to return the user's name."
2. Vary the context: Do not use the same example project in every tutorial.
3. Explain the "why" behind each step: Never just say "run this command."
4. Include debugging exercises: Give readers broken code and ask them to fix it.
5. End with an open-ended challenge: "Try adding a search endpoint that filters by name."

Progressive independence:
Tutorial 1 (high guidance): Exact code. Goal: success.
Tutorial 2 (medium guidance): "Create a function that does X." Goal: confidence.
Tutorial 3 (low guidance): "Build a feature with these requirements." Goal: independence.
```

### Teaching Concepts vs. Teaching Tool Usage

A great tutorial teaches both concepts and tool usage, but it prioritizes them differently depending on the audience.

**Concept-first tutorials:**

```text
Best for: Teaching fundamental programming concepts
Goal: Understanding that transfers to any tool

Structure:
1. Explain the concept (e.g., "what is an API?")
2. Show how the tool implements the concept
3. Let the reader apply the concept in a different context

Example — teaching REST APIs:
1. Explain what REST is (resources, methods, statelessness)
2. Show how Express implements REST endpoints
3. Challenge: Create a new endpoint without instructions
```

**Tool-first tutorials:**

```text
Best for: Getting productive quickly
Goal: Accomplishing a specific task immediately

Structure:
1. Show the tool command or API
2. Explain what it does (briefly)
3. Let the reader use it
4. Expand on the concept later

Example — teaching Docker:
1. Run `docker run nginx` to start a web server
2. Explain that Docker runs isolated containers
3. Show how to map ports
4. Later: explain what a container image is
```

**Hybrid approach for developer tutorials:**

```text
For developer audiences, use the tool-first approach with concept callouts:

1. Do the thing (tool usage)
   "Run npm install express to add Express to your project."

2. Brief concept callout
   "npm install downloads the Express package from npm registry
    and adds it to your package.json dependencies."

3. Use the tool
   "Now import Express in your server.js file..."

4. Deeper concept callout (optional, marked as such)
   "Optional deep dive: What happens when npm install runs?"

This approach gets readers building immediately while still teaching concepts.
Mark optional deep dives so readers can skip them or return later.
```

### Verifying Outcomes at Each Step

Every step in a tutorial should have a verification point. The reader should know they did the step correctly before moving on.

**Types of verification:**

```text
Visual verification — something the reader can see:
  "You should see 'Server running on port 3000' in the terminal."

Output verification — specific output from a command:
  ```bash
  curl http://localhost:3000
  # Expected output: {"message": "Hello, World!"}
  ```

File verification — the reader checks a file exists:
  "Open your project folder. You should see a 'node_modules' directory."

State verification — a specific condition is met:
  "Open http://localhost:3000 in your browser. You should see Hello World."

Error absence — no error messages:
  "The command should complete without errors."
```

**Verification formatting:**

```text
After running the migration command:

```bash
npx knex migrate:latest
```

You should see output similar to:
```
Batch 1 run: 1 migrations
```

If you see an error about a missing database, make sure you created
the database in the previous step before running the migration.
```

**Checkpoint sections:**

```text
For longer tutorials, add checkpoint sections every few steps:

### Checkpoint: What you should have so far

By this point, your project should have:
- [ ] A package.json file with express and pg as dependencies
- [ ] A server.js file that starts on port 3000
- [ ] A database.js file with the PostgreSQL connection
- [ ] A running server that responds to GET /

If anything is missing, review the steps above before continuing.
```

### Handling Errors in Tutorials

Readers will make mistakes and encounter errors. Good tutorial design anticipates common failure points and provides recovery paths.

**Common error anticipation:**

```text
For each step, ask:
- What could go wrong here?
- What is the most common mistake?
- What dependency might be missing?

Then document the recovery path:

"Error: 'command not found: node' — Node.js is not installed."
"Error: 'listen EADDRINUSE :::3000' — Port 3000 is in use. Use a different port."
```

**Common errors to anticipate:**

```text
EACCES: Permission error. Run with sudo or configure npm prefix.
Cannot find module: Dependencies not installed. Run npm install.
Relation does not exist: Migration not run. Run npx knex migrate:latest.
```

**Error placement:** Put common errors inline after the step where they occur.
Put setup errors in a "Troubleshooting" section after setup.
Put a link to full troubleshooting at the bottom of the tutorial.

### Screenshot and Diagram Use

Use screenshots for UI-heavy steps and visual verification. Avoid screenshots for code (use code blocks) or terminal output (use text).

**Screenshot best practices:** Crop tightly, add annotations, use consistent styling, compress under 200KB each, provide alt text.

**When to use diagrams:** Architecture overviews, data flow, sequence of operations, comparisons.

**Tools:** Mermaid (Markdown-native), Excalidraw (hand-drawn style), Diagrams.net, PlantUML.

### Writing for Different Experience Levels

**Level indicators:**

```text
Beginner: No prior knowledge. Every concept explained. 15-30 min.
Intermediate: Basic knowledge assumed. Setup summarized. 30-60 min.
Advanced: Significant prior knowledge. Focus on patterns. 45-90 min.
```

**Progressive disclosure within a tutorial:**

```text
Use callouts to provide extra depth for interested readers without
overwhelming beginners:

Step 3: Create the database schema

Run the following migration:
```sql
CREATE TABLE users (
  id SERIAL PRIMARY KEY,
  email VARCHAR(255) UNIQUE NOT NULL
);
```

> **For experienced readers:** The SERIAL type is a PostgreSQL-specific
> auto-incrementing integer. If you are using a different database,
> you may need to use AUTOINCREMENT (SQLite) or AUTO_INCREMENT (MySQL).

This gives beginners what they need while acknowledging that advanced
readers might need to adapt.
```

## Prerequisites

1. Node.js 18 or later
   ```bash
   node --version
   # Must show v18.x.x or later
   ```

2. npm 9 or later
   ```bash
   npm --version
   # Must show 9.x.x or later
   ```

3. A code editor (VS Code recommended)

4. Basic JavaScript knowledge
   You should understand arrow functions, async/await, and objects.
```

### Example 3: Progressive Disclosure

```text
Run the migration command:

```bash
npx knex migrate:latest
```

This creates the database tables defined in your migration files.

> **Note for SQLite users:** If you are using SQLite instead of
> PostgreSQL, you may need to install the better-sqlite3 driver:
> `npm install better-sqlite3` and update your knexfile to use
> `client: "better-sqlite3"`.
```

### Example 4: Error Recovery

```text
If you see: Error: listen EADDRINUSE :::3000

Port 3000 is already in use by another process. To fix this:

1. Find the process using the port:
   ```bash
   lsof -i :3000  # macOS/Linux
   netstat -ano | findstr :3000  # Windows
   ```

2. Stop the process:
   ```bash
   kill <PID>  # macOS/Linux
   taskkill /PID <PID> /F  # Windows
   ```

3. Alternatively, use a different port by setting the PORT environment
   variable: `PORT=3001 node server.js`
```

### Example 5: Tutorial Summary with Checkboxes

```text
## Summary

Congratulations! You have built and deployed your first API.

What you accomplished:
- [x] Created a Node.js project with Express
- [x] Defined GET and POST endpoints
- [x] Connected to a PostgreSQL database
- [x] Deployed to production

Key concepts learned:
- Routing: Mapping URLs to handler functions
- Middleware: Processing requests before they reach handlers
- Environment variables: Configuring your app without code changes
```

## Glossary

| Term | Definition |
|------|------------|
| Tutorial | A guided learning experience that builds understanding through step-by-step instruction |
| Tutorial hell | A state where a learner completes tutorials but cannot build independently |
| Verification point | A specific condition the reader can check to confirm a step was completed correctly |
| Progressive disclosure | Revealing additional depth only to readers who want it |
| Scaffolding | Providing high initial guidance and gradually removing it as the learner gains confidence |
| Reproducible environment | A development setup that produces the same results regardless of the host machine |
| Version pinning | Specifying exact dependency versions to prevent breakage from updates |
| Starter template | A pre-configured project repository that serves as the starting point for a tutorial |
| Inverted pyramid | Presenting the most important information first, then providing supporting detail |
| Checkpoint | A point in a tutorial where the reader verifies their progress before continuing |
| Error recovery | Instructions for diagnosing and fixing common problems in a tutorial |
| Deliberate practice | Focused exercises designed to improve specific skills |

## Quick References

- [The Readme Project: Tutorials](https://readme.com/documentation) — tutorial best practices from ReadMe
- [Diataxis: Tutorials](https://diataxis.fr/tutorials/) — the tutorial mode in the Diataxis framework
- [GNOME Tutorials Guidelines](https://developer.gnome.org/documentation/guidelines/tutorials.html) — detailed tutorial writing guidelines
- [Google Technical Writing: Tutorials](https://developers.google.com/tech-writing/tutorials) — Google's guidance on writing tutorials
- [DigitalOcean Tutorials Guidelines](https://www.digitalocean.com/community/tutorials/digitalocean-s-write-for-donations-guidelines) — community tutorial standards

## Next Steps

- [API Docs](api-docs.md) — writing reference documentation for API endpoints
- [Diataxis Framework](diataxis.md) — understanding how tutorials fit into the broader documentation landscape
