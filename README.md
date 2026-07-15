# DevBook

**Markdown-based learning resources for developers.**

DevBook is a living library of technical knowledge for developers, by developers. Every concept is explained in plain Markdown — from the mathematical foundations that power machine learning, to the networking protocols that move data across the internet, to the programming patterns that shape production-grade software.

No dependencies. No build step. No `npm install`. Open any `.md` file in your editor or browser and start reading.

## Who Is This For

- **Working developers** brushing up on fundamentals or exploring unfamiliar territory.
- **Students and career-switchers** building a structured foundation in computer science and software engineering.
- **AI agents** tasked with generating, reviewing, or maintaining technical content.
- **Anyone** who prefers plain-text, distraction-free learning over video courses and interactive platforms.

## What's Inside

21 subjects, each containing one or more modules with focused content files. Every document follows a mandatory 9-section format designed for clarity and retention.

### Foundational Knowledge

| Subject | Directory | Scope |
|---------|-----------|-------|
| **Mathematics** | `mathematics/` | Linear algebra, calculus, probability & statistics, discrete math, number theory |
| **Computer Science** | `computer-science/` | Data structures, algorithms, OS concepts, compilers, computation theory |
| **Physics** | `physics/` | Newtonian mechanics, electromagnetism — intuition for simulation, rendering, game dev |
| **Hardware** | `hardware/` | Embedded systems, electronics, IoT, firmware, computer architecture |

### Building Software

| Subject | Directory | Scope |
|---------|-----------|-------|
| **Programming** | `programming/` | Language paradigms, OOP, FP, design patterns, testing, debugging, CI/CD |
| **Software Engineering** | `software/` | Methodology, testing, project management, code review, team practices |
| **Systems Design** | `systems-design/` | Scalability, consistency models, caching, CDNs, message queues, microservices |
| **UI/UX Design** | `ui-ux/` | Interface design, accessibility, interaction patterns, design systems |

### Infrastructure & Data

| Subject | Directory | Scope |
|---------|-----------|-------|
| **Networks** | `networks/` | OSI & TCP/IP, HTTP/HTTPS, DNS, TLS, routing, load balancing, REST, gRPC |
| **Security** | `security/` | OWASP Top 10, authentication, authorization, encryption, OAuth2, JWT, CSP |
| **Data & Databases** | `data-databases/` | SQL, NoSQL, data modeling, indexing, storage engines, data pipelines |
| **Cloud & DevOps** | `cloud-devops/` | Infrastructure, CI/CD, containerization, orchestration, monitoring |
| **AI / Machine Learning** | `ai-ml/` | Neural networks, gradient descent, transformers, NLP, computer vision, MLOps |

### Communication & Expression

| Subject | Directory | Scope |
|---------|-----------|-------|
| **English** | `english/` | Technical writing, documentation style, grammar, clear communication |

### Business & Career

| Subject | Directory | Scope |
|---------|-----------|-------|
| **Careers** | `careers/` | In-depth explorations of software professions — roles, responsibilities, progression |
| **Business** | `business/` | Entrepreneurship, corporate finance, investment, business models, strategy |
| **Governance** | `governance/` | Law, policy, ethics, regulation, compliance for digital technology |

### Personal & Social

| Subject | Directory | Scope |
|---------|-----------|-------|
| **Level Up** | `level-up/` | Existential recovery, resilience, habits, meaning, and purpose |
| **Philosophy** | `philosophy/` | Existentialism, stoicism, ethics, meaning, and the good life |
| **Psychology** | `psychology/` | Positive psychology, behavioral psychology, cognitive psychology |
| **Social** | `social/` | Psychology, communication, human relations, teamwork, cognitive biases |

## Quick Start

```bash
git clone https://github.com/reasvyn/devbook.git
cd devbook
# Open any .md file in your editor or browser — that's it
```

### Navigation

- **Browse by subject** — enter any top-level directory and open its `index.md`.
- **Follow a learning path** — each module `index.md` is a numbered learning path with phases and optional branches.
- **Search** — `grep -r "topic" devbook/` to find what you need across all subjects.

## How Content Is Organized

### Directory Convention

```
{subject}/
├── index.md
├── intro/                         ← subject-level intro (required)
│   ├── index.md                   ← lists all intro files
│   └── {short-description}.md
└── {module}/
    ├── index.md
    ├── intro/                     ← module-level intro (required)
    │   ├── index.md               ← lists all intro files
    │   └── {short-description}.md
    ├── {short-description}.md     ← content files (flat)
    └── {submodule}/               ← optional
        ├── index.md
        └── {short-description}.md
```

### The 4-Level Index Chain

Every content file is reachable from the root through four levels of navigation:

```
Level 1  Master (root)     /index.md
                             └── [Subject](subject/index.md)
Level 2  Subject           subject/index.md
                             └── [Module](subject/module/index.md)
Level 3  Module            subject/module/index.md
                             ├── [Intro](subject/module/intro/index.md)
                             └── [Submodule](subject/module/submodule/index.md)  (optional)
Level 4  Submodule/Intro   subject/module/intro/index.md
                            subject/module/submodule/index.md
                             └── {short-description}.md
```

A content file is only considered published when it appears at all four levels. A missing link at any level means the content is orphaned.

### Careers Directory

The `careers/` directory follows the same convention. Each career is a module that explores a profession in depth — what the role entails, its responsibilities, specializations, career progression, and industry context. These documents discuss the *profession itself*, not what someone should learn to enter it.

## Document Format

Every content file follows a mandatory 9-section structure:

| # | Section | Required | Purpose |
|---|---------|----------|---------|
| 1 | **Title** | Yes | Document title |
| 2 | **Description** | Yes | Brief overview — what this covers and why a developer should care |
| 3 | **Prerequisites** | Yes | What the reader should know first, with links to related documents |
| 4 | **Table of Contents** | Yes | Section navigation with anchor links |
| 5 | **Content / Material** | Yes | Core material — explanations, code, diagrams, examples, walkthroughs |
| 6 | **Learning Tips** | Optional | Study advice, memory aids, practice strategies, common pitfalls |
| 7 | **Glossary** | Yes | Key terms and definitions introduced in the document |
| 8 | **Quick References** | Optional | Verified external links to journals, books, blog posts, websites |
| 9 | **Next Steps** | Yes | Where to go from here — related documents, practice exercises |

Sections marked "(Optional)" may be omitted if genuinely not needed. All content must be written in **English** using professional, technical, academic register. See [AGENTS.md](AGENTS.md) for the complete specification.

## Why Markdown?

| Reason | Why It Matters |
|--------|----------------|
| **Zero friction** | Open any `.md` file — no tooling, no setup, no login. |
| **Git-friendly** | Meaningful diffs, easy reviews, simple collaboration. |
| **Universal renderer** | GitHub, VS Code, Obsidian, Notion — every platform speaks Markdown. |
| **Focus on substance** | No styling rabbit holes; the content is the product. |
| **Extensible** | Embed LaTeX via `$$`, diagrams via Mermaid, or ASCII art. |

## For AI Agents

This repository includes [AGENTS.md](AGENTS.md) — a comprehensive guide for AI agents managing content. It covers:

- Core constraints and content rules
- Christian worldview integration approach
- Directory structure and indexing conventions
- Mandatory document format and content generation rules
- Workflows for creating and updating content
- Quality checklist

Any AI agent working with DevBook content should read AGENTS.md before making changes.

## Contributing

DevBook thrives on community input. If you'd like to add or improve a topic:

1. Fork the repo.
2. Create a branch (`git checkout -b topic/your-topic`).
3. Write your content in Markdown following the existing style.
4. Open a pull request.

See [CONTRIBUTING.md](CONTRIBUTING.md) for detailed guidelines on writing style, document format, directory structure, and the pull request process.

## License

[MIT](LICENSE) — free to use, share, and adapt.

---

*Most of the content in this repository is generated with assistance from AI.*
