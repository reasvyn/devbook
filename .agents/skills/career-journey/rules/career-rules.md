# Career Journey — Career-Specific Rules

This file contains rules specific to the `careers/` subject. General content rules apply from [CONTENT-RULES.md](../../../../CONTENT-RULES.md).

## Core Principles

- **Profession, not tutorial.** `careers/` explores what a profession *is* — its history, responsibilities, specializations, progression, industry context, and culture. It does NOT teach the skills needed to enter the profession.
- **Reference, don't teach.** Career documents link to appropriate subjects (`programming/`, `computer-science/`, `networks/`, etc.) for actual skill content. The career file itself discusses the profession.
- **Learning path as narrative.** Each career must include a learning path section that provides high-level narrative guidance and references existing subjects. It is a roadmap, not a curriculum.

## Directory Structure

```
careers/
├── index.md
├── intro/
│   ├── index.md
│   └── {short-description}.md
└── {career}/
    ├── index.md
    ├── intro/
    │   ├── index.md
    │   └── {short-description}.md
    └── {short-description}.md
```

## Content Rules

- Career modules are subjects of study about professions, not learning paths for individuals.
- Each career module has an `index.md`, an `intro/` with background content, and content files covering different aspects of the profession.
- Content files follow the 9-section mandatory format.
- Unlike other subjects, careers are not organized by experience level — they are standalone profession profiles.

## Cross-Reference Principles

- Core technical skills → `computer-science/`, `programming/`, `mathematics/`
- Infrastructure knowledge → `networks/`, `cloud-devops/`, `systems-design/`
- Professional practices → `software/`, `security/`, `data-databases/`
- Human factors → `english/`, `social/`, `psychology/`, `business/`
- Foundational science → `physics/`, `hardware/`
