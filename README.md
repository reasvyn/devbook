# DevBook

**Markdown-based learning resources for developers.**

DevBook is a living library of technical knowledge for developers, by developers. Every concept is explained in plain Markdown — from the mathematical foundations that power machine learning, to the networking protocols that move data across the internet, to the programming patterns that shape production-grade software.

Whether you're brushing up on fundamentals or diving into a new area, DevBook gives you the signal without the noise: concise, practical, and ready to open in your editor.

## Topics

| Topic | Scope |
|-------|-------|
| **Mathematics** | Set theory, logic, linear algebra, calculus, probability & statistics, combinatorics, graph theory |
| **Physics** | Newtonian mechanics, electromagnetism, waves, thermodynamics — intuition for simulation, rendering, and game dev |
| **English** | Technical writing, documentation style, grammar for code comments, commit messages, and RFCs |
| **Computer Science** | Data structures, algorithms, time complexity, OS concepts, compilers, databases, architecture |
| **Computer Networks** | OSI & TCP/IP models, HTTP/HTTPS, DNS, TLS, routing, load balancing, REST, gRPC, WebSockets |
| **Programming** | Language paradigms, OOP, FP, design patterns, testing, debugging, profiling, CI/CD, refactoring |
| **Systems Design** | Scalability, consistency models, caching, CDNs, message queues, microservices, CAP theorem |
| **Security** | OWASP Top 10, authentication, authorization, encryption, OAuth2, JWT, CSP, SQL injection prevention |
| **AI / ML** | Neural networks, gradient descent, transformers, NLP, computer vision basics, MLOps fundamentals |

## Quick Start

```bash
git clone https://github.com/your-org/devbook.git
cd devbook
# That's it — open any .md file in your editor or browser
```

No dependencies. No build step. No `npm install`.

## Structure

### Directory Convention

```
{module}/
├── index.md
└── {submodule}/
    ├── index.md
    └── {level:intro|beginner|intermediate|advanced}/
        └── {short-description}.md
```

- **`index.md`** — every module and submodule **must** have an `index.md` that lists and links to its children.
- **Level** — each content file sits in a difficulty level directory: `intro`, `beginner`, `intermediate`, or `advanced`.
- **Short description** — a hyphenated slug describing the topic (e.g., `why-math.md`, `linear-algebra.md`).

### Index Tree

The `index.md` files form a tree that mirrors the directory structure:

```
devbook/
├── index.md                              # master index → links to module indexes
├── mathematics/
│   ├── index.md                          # → links to submodule indexes
│   ├── intro/
│   │   └── why-math.md
│   ├── beginner/
│   │   └── set-theory.md
│   └── linear-algebra/
│       ├── index.md                      # → links to level indexes
│       ├── intro/
│       │   └── vectors-and-matrices.md
│       ├── beginner/
│       │   └── matrix-operations.md
│       └── intermediate/
│           └── eigendecomposition.md
├── physics/
│   ├── index.md
│   └── ...
├── english/
│   ├── index.md
│   └── ...
├── computer-science/
│   ├── index.md
│   └── ...
├── networks/
│   ├── index.md
│   └── ...
├── programming/
│   ├── index.md
│   └── ...
├── systems-design/
│   ├── index.md
│   └── ...
├── security/
│   ├── index.md
│   └── ...
└── ai-ml/
    ├── index.md
    └── ...
```

Every document follows a mandatory format:

1. **Title** — document title
2. **Description** — brief overview and why it matters
3. **Prerequisites** — what you should know before reading
4. **Table of Contents** — section navigation
5. **Content / Material** — the core material
6. **Study Cases** (Optional) — real-world walkthroughs
7. **Examples** (Optional) — additional practical examples
8. **Glossary** — key terms and definitions
9. **Next Steps** — where to go from here

## Why Markdown?

| Reason | Why it matters |
|--------|----------------|
| **Zero friction** | Open any `.md` file — no tooling, no setup, no login. |
| **Git-friendly** | Meaningful diffs, easy reviews, simple collaboration. |
| **Universal renderer** | GitHub, VS Code, Obsidian, Notion — every platform speaks Markdown. |
| **Focus on substance** | No styling rabbit holes; the content is the product. |
| **Extensible** | Embed LaTeX via `$$`, diagrams via Mermaid, or ASCII art. |

## How to Use This

- **Browse by topic** — start with any directory that interests you.
- **Follow a path** — each directory's README suggests a learning order.
- **Search** — `grep -r "topic" devbook/` to find what you need fast.
- **Contribute** — found an error? Have a better explanation? Open a PR.

## Contributing

DevBook thrives on community input. If you'd like to add or improve a topic:

1. Fork the repo.
2. Create a branch (`git checkout -b topic/your-topic`).
3. Write your content in Markdown following the existing style.
4. Open a pull request.

See [CONTRIBUTING.md](CONTRIBUTING.md) for detailed guidelines.

## License

MIT — free to use, share, and adapt.

---

\* Most of the content in this repository is generated with assistance from AI.\*
