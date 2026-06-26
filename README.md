# DevBook

**Markdown-based learning resources for developers.**

DevBook is a living library of technical knowledge for developers, by developers. Every concept is explained in plain Markdown — from the mathematical foundations that power machine learning, to the networking protocols that move data across the internet, to the programming patterns that shape production-grade software.

Whether you're brushing up on fundamentals or diving into a new area, DevBook gives you the signal without the noise: concise, practical, and ready to open in your editor.

## Topics

| Topic | Scope |
|-------|-------|
| **Computer Science** | Data structures, algorithms, OS concepts, compilers, computation theory |
| **Programming** | Language paradigms, OOP, FP, design patterns, testing, debugging, CI/CD |
| **Mathematics** | Linear algebra, calculus, probability & statistics, discrete math, number theory |
| **Networks** | OSI & TCP/IP, HTTP/HTTPS, DNS, TLS, routing, load balancing, REST, gRPC |
| **Systems Design** | Scalability, consistency models, caching, CDNs, message queues, microservices |
| **Security** | OWASP Top 10, authentication, authorization, encryption, OAuth2, JWT, CSP |
| **Data & Databases** | SQL, NoSQL, data modeling, indexing, storage engines, data pipelines |
| **AI / ML** | Neural networks, gradient descent, transformers, NLP, computer vision, MLOps |
| **Cloud & DevOps** | Infrastructure, CI/CD, containerization, orchestration, monitoring |
| **UI/UX Design** | Interface design, accessibility, interaction patterns, design systems |
| **Software Engineering** | Methodology, testing, project management, code review, team practices |
| **Physics** | Newtonian mechanics, electromagnetism — intuition for simulation, rendering, game dev |
| **Hardware** | Embedded systems, electronics, IoT, firmware, computer architecture |
| **Governance** | Law, policy, ethics, regulation, compliance for digital tech |
| **English** | Technical writing, documentation style, grammar, clear communication |
| **Business** | Entrepreneurship, corporate finance, investment, business models, strategy |
| **Social** | Psychology, communication, human relations, teamwork, cognitive biases |

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
{subject}/
├── index.md
├── intro/                         ← subject-level intro (optional)
└── {module}/
    ├── index.md
    ├── intro/                     ← module-level intro (required)
    └── {submodule}/               ← optional
        ├── index.md
        ├── intro/                 ← submodule-level intro (required)
        └── {short-description}.md
```

- **`index.md`** — every subject, module, and submodule **must** have an `index.md` that lists and links to its children.
- **`intro/`** — required for every module and submodule. Contains background, principles, history, and philosophy.
- Content files sit flat under the module or submodule. `intro/` is the only subdirectory allowed at these levels.

Every document follows a mandatory format:

1. **Title** — document title
2. **Description** — brief overview and why it matters
3. **Prerequisites** — what you should know before reading
4. **Table of Contents** — section navigation
5. **Content / Material** — the core material
6. **Study Cases** (Optional) — real-world walkthroughs
7. **Examples** (Optional) — additional practical examples
8. **Glossary** — key terms and definitions
9. **Quick References** (Optional) — verified external links to journals, books, blog posts, websites
10. **Next Steps** — where to go from here

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
