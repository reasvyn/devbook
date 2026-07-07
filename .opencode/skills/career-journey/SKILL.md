---
name: career-journey
description: Use when the user mentions 'career', 'software engineer', 'profession', 'job', 'role', 'learning path', or references careers/ subject. Guides the creation and maintenance of career exploration content in DevBook.
---

# Skill: Career Journey

This skill governs all work in the `careers/` subject of DevBook.

## Core Principles

- **Profession, not tutorial.** `careers/` explores what a profession *is* — its history, responsibilities, specializations, progression, industry context, and culture. It does NOT teach the skills needed to enter the profession.
- **Reference, don't teach.** A career document about software engineers links to `programming/`, `computer-science/`, `networks/`, etc. for the actual skill content. The career file itself discusses the profession.
- **Always include a learning path** section that references existing subjects. The learning path is a high-level roadmap, not a curriculum.

## Directory Structure

```
careers/
├── index.md
├── intro/
│   ├── index.md
│   └── why-explore-careers.md
└── {career-name}/
    ├── index.md
    ├── intro/
    │   ├── index.md
    │   └── {intro-files}.md
    ├── what-is-{career}.md
    ├── responsibilities-and-daily-work.md
    ├── types-and-specializations.md
    ├── career-progression.md
    ├── building-foundations.md       ← learning path (references subjects)
    ├── first-steps-in-programming.md ← learning path
    └── ... (learning path continues)
```

## When Creating Content

1. Read the parent `index.md` and any existing career files first.
2. Research the profession thoroughly before writing.
3. Follow the 9-section mandatory format (400-800 lines).
4. In the learning path section, write narrative guidance and link to the appropriate subject (e.g., `[Computer Science](../../computer-science/index.md)`).
5. The "Prerequisites" section should reference the learning path or intro files.
6. "Next Steps" should link to other career modules or related professions.

## Existing Careers

- `software-engineer/`
