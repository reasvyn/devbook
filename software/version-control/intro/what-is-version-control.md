# Introduction to Version Control

## Description

What version control is, why it is essential for every software project, how different systems compare, and how tools like Git store and manage your code history under the hood.

## Prerequisites

- Basic familiarity with files and directories on a computer

## Table of Contents

- [Why Version Control](#why-version-control)
- [Core Concepts](#core-concepts)
- [A Brief History](#a-brief-history)
- [Local vs Centralized vs Distributed](#local-vs-centralized-vs-distributed)
- [How Git Stores Data](#how-git-stores-data)
- [Common Operations](#common-operations)
- [Choosing a System](#choosing-a-system)
- [Version Control in the Development Workflow](#version-control-in-the-development-workflow)

## Content

### Why Version Control

Every developer has experienced the pain of losing work. A hard drive fails, a mistaken deletion removes the wrong file, or a change breaks functionality and there is no way to undo it. Version control solves all of these problems.

At its simplest, version control is a system that records changes to a file or set of files over time so that you can recall specific versions later. It acts as a time machine for your code.

The benefits go far beyond backup:

- **History** — every change is recorded with who made it, when, and why (via commit messages). You can see exactly how a file evolved over weeks, months, or years.
- **Collaboration** — multiple people can work on the same codebase simultaneously without stepping on each other's toes. Changes are merged together, and conflicts are resolved explicitly.
- **Experimentation** — branches let you try out ideas in isolation. If the experiment fails, delete the branch. If it succeeds, merge it back into the main line.
- **Recovery** — if a recent change introduces a bug, you can roll back to a known good state in seconds. If you discover that a bug existed for weeks, you can find exactly when it was introduced.
- **Accountability** — every line of code is attributed to an author and a commit message. This helps with code review, debugging, and understanding why a particular change was made.
- **Deployment traceability** — every deployed version can be traced back to a specific commit. This connects code changes to production behavior.

Without version control, teams resort to shared folders, ZIP files named `final-v3-really-final.zip`, and constant communication about who is editing what. These approaches do not scale beyond a single person and collapse entirely under the pressure of a production incident.

### Core Concepts

Every version control system shares a set of fundamental ideas:

**Repository (repo)** — a database of all changes and the current state of the project. In Git, it lives in a hidden directory called `.git` inside the project folder. The repository contains the entire history, not just the latest version.

**Commit** — a snapshot of the project at a specific point in time. Each commit has a unique identifier (a cryptographic hash) and a message describing what changed and why. A commit typically includes a pointer to its parent commit, forming a chain.

**Working tree** — the current state of files on your disk. This is where you make edits. The working tree is separate from the repository — changes must be explicitly committed to become part of the history.

**Staging area (index)** — an intermediate area where you prepare the next commit. You choose which changes to include, giving you fine-grained control over what goes into each snapshot. Not all VCS have this concept. Git's staging area is one of its distinguishing features.

**Revision / Version** — a specific state of the repository identified by a commit hash, tag, or revision number. In Git, a revision is always a full snapshot plus metadata.

**Branch** — a movable pointer to a specific commit. When you make a new commit on a branch, the pointer moves forward. Branches let you diverge from the main line of development and work in isolation.

**Merge** — the act of combining changes from two branches. A merge creates a new commit that has two parents, representing the integration of two lines of development.

**Remote** — a copy of the repository hosted on another machine, typically a server like GitHub, GitLab, or Bitbucket. Remotes enable collaboration across team members.

### A Brief History

Version control has evolved through three distinct generations, each solving the limitations of the previous one.

**1970s — SCCS (Source Code Control System)** was the first widely used VCS. It worked on individual files and could only be used by one person at a time. Files were locked while being edited, which made collaboration cumbersome.

**1980s — RCS (Revision Control System)** improved on SCCS by storing reverse diffs (how to go from the current version back to previous ones). It was still file-oriented and single-user.

**1990s — CVS (Concurrent Versions System)** introduced a client-server model where multiple developers could work on the same files simultaneously. Changes were merged rather than requiring exclusive locks. CVS became the standard for open-source development.

**2000s — Subversion (SVN)** addressed many of CVS's shortcomings: atomic commits, versioned directories, efficient handling of binary files. It became the dominant VCS in enterprises. However, SVN was still centralized — the server was a single point of failure and all operations required network access.

**2005 — Git** was created by Linus Torvalds for Linux kernel development. The existing tools (BitKeeper) had licensing issues, and no other VCS could handle the scale and complexity of the Linux kernel project. Git's design goals were: speed, distributed architecture, strong integrity guarantees, and support for non-linear development (thousands of branches). Git achieved all of these and rapidly became the dominant VCS.

Today, Git is used by over 90% of developers. Its ecosystem — GitHub, GitLab, Bitbucket, CI/CD integration, code review platforms — is unmatched.

### Local vs Centralized vs Distributed

Understanding the architectural differences helps explain why Git works the way it does.

**Local systems** (RCS, SCCS) store version history on the local filesystem. They use a simple database of patches. These systems work for a single developer but provide no collaboration mechanisms at all. If your hard drive fails, you lose everything.

| Aspect | Local |
|--------|-------|
| Collaboration | None |
| Single point of failure | The only copy is on your machine |
| Network required | No |
| Speed | Fast — all operations are local |
| Typical users | Single developer with basic needs |

**Centralized systems** (CVS, Subversion, Perforce) use a single server that holds the canonical repository. Developers check out a working copy, make changes, and commit back to the server.

| Aspect | Centralized |
|--------|-------------|
| Collaboration | All changes go through the central server |
| Single point of failure | Server outage blocks all operations |
| Network required | Yes — most operations need the server |
| Speed | Medium — network latency for commits and history |
| Typical users | Enterprise teams, legacy projects |

Centralized systems were a huge step forward for team collaboration, but they have fundamental weaknesses:

- If the server goes down, nobody can commit, view history, or create branches.
- If the server's disk fails without a recent backup, the entire history is lost.
- Branching is expensive — it typically involves copying files on the server.
- Most operations require network access, making them slow on poor connections.

**Distributed systems** (Git, Mercurial, Fossil) give every developer a full copy of the entire repository, including its complete history. There is no single source of truth — every clone is a full backup.

| Aspect | Distributed |
|--------|-------------|
| Collaboration | Push/pull between peers, typically via a shared remote |
| Single point of failure | None — every clone is a full backup |
| Network required | Only for push/pull with remotes |
| Speed | Fast — most operations are local (commit, diff, log, branch) |
| Typical users | Everyone — from individual developers to large enterprises |

Key advantages of distributed design:

- **Offline work** — you can commit, branch, view history, and diff without any network connection. This is essential for developers who travel, work on planes, or have unreliable internet.
- **Speed** — Git operations are measured in milliseconds because they touch only local files. Comparing this to SVN where `svn log` might take seconds querying a remote server.
- **Redundancy** — every clone is a complete backup of the entire project history. If the central server is destroyed, any developer's clone can restore it.
- **Flexible workflows** — you can push and pull between any repositories, not just a central server. This enables code review workflows, maintainer models, and integration strategies that are impossible in centralized systems.

### How Git Stores Data

Git is fundamentally different from most VCS in how it stores data. Most systems (SVN, Perforce) store *changes* (deltas) between file versions. Git stores *snapshots*.

When you commit in Git:

1. Git calculates a checksum (SHA-1 hash) of every file's contents.
2. Git stores each file as a **blob** (binary large object) in the object database.
3. Git creates a **tree** object that maps filenames to blobs (like a directory listing).
4. Git creates a **commit** object that points to the tree and records metadata (author, message, parent commits).

```
Commit abc123
├── Tree (root directory)
│   ├── blob "README.md" → content hash
│   ├── tree "src/"
│   │   ├── blob "main.py" → content hash
│   │   └── blob "utils.py" → content hash
│   └── blob "package.json" → content hash
├── Author: Alice <alice@example.com>
├── Message: "Add main application logic"
└── Parent: def456
```

Git's object types:

| Object | Purpose | Example |
|--------|---------|---------|
| Blob | Stores file content | The bytes of `main.py` |
| Tree | Maps names to hashes (like a directory) | Lists what files are in `src/` |
| Commit | A snapshot pointing to a tree with metadata | "Add login feature" |
| Tag | A named pointer to a specific commit | `v1.0.0` |
| Ref | A named pointer to a commit (branch, HEAD) | `main`, `feature-x` |

Because content is addressed by its hash, Git automatically detects duplicate content. If a file does not change between commits, Git reuses the same blob. This makes Git storage efficient despite storing full snapshots.

**Integrity:** Every object in Git is checksummed. The hash is computed from the object's contents. If a file becomes corrupted on disk, Git detects it because the hash will not match. This makes Git incredibly resilient to data corruption.

**Packfiles:** Over time, Git optimizes storage by creating packfiles. A packfile stores multiple objects in a single file, compressed using delta encoding (storing differences between similar objects). Packfiles are created automatically by `git gc` (garbage collection). This is why a Git repository with years of history can be smaller than a single checkout of the latest version in some other VCS.

### Common Operations

Regardless of the system, most developers use the same workflow day to day:

1. **Pull** the latest changes from the shared repository to stay up to date.
2. **Create a branch** for the feature or fix being worked on.
3. **Make changes** to files in the working tree — add, modify, delete.
4. **Stage** the changes that should be part of the next commit. This lets you choose which changes belong together.
5. **Commit** the staged changes with a descriptive message explaining what and why.
6. **Push** the branch to the shared repository so others can see it.
7. **Open a pull request** (or merge request) to propose merging the changes into the main branch.
8. **Review** and address feedback from teammates.
9. **Merge** once the changes are approved.

Steps 1-6 happen on the command line. Steps 7-9 happen on a platform like GitHub, GitLab, or Bitbucket.

### Choosing a System

For new projects starting today, Git is the default choice. Its ecosystem is unmatched — hosting platforms, CI/CD integration, code review tools, and a massive community. Git has become the lingua franca of version control.

The main contenders today:

| System | Type | Use Case |
|--------|------|----------|
| Git | Distributed | Default choice for all new projects |
| Mercurial | Distributed | A few large projects (Mozilla, OpenJDK) |
| Subversion (SVN) | Centralized | Legacy enterprise, some embedded projects |
| Perforce | Centralized | Large game development, binary-heavy projects |
| Fossil | Distributed | SQLite project, built-in bug tracker and wiki |

Subversion still exists in some legacy enterprise environments, and Mercurial is used by a few large projects, but these are exceptions. Learning Git is the single highest-leverage investment a developer can make in their tooling.

### Version Control in the Development Workflow

Version control is not an afterthought — it is central to how modern software is built. It connects to every part of the development lifecycle:

**Code review** — platforms like GitHub use pull requests to make code review a first-class activity. Every change is discussed, approved, and tracked before reaching the main branch.

**CI/CD** — continuous integration pipelines are triggered by commits and pull requests. Tests run automatically on every push. If a commit breaks the build, the team knows immediately.

**Deployment** — releases are tagged commits. Rollbacks are reverts. Every environment (dev, staging, production) runs a known commit with a known state.

**Debugging** — `git bisect` performs a binary search through history to find the exact commit that introduced a bug. Combined with automated tests, this can pinpoint the offending change in seconds.

**Experimentation** — branches let teams try radical ideas without risk. If an experiment works, it is merged. If not, the branch is deleted and nobody remembers the failed attempt.

**Documentation** — the commit log is the most accurate documentation of why the code evolved the way it did. A well-maintained log answers questions that no other documentation covers.

## Study Cases

**Case 1: Recovering from a production incident.**

A deployment introduced a critical bug. The team needs to roll back immediately. With Git, they run `git revert <commit>` to create a new commit that undoes the change, then deploy. The rollback is tracked in history, and the revert commit itself is deployed — not a mysterious manual fix.

**Case 2: Finding when a bug was introduced.**

A customer reports that a feature has been broken for weeks. Using `git bisect`, the developer marks the current commit as "bad" and a known-good commit from a month ago as "good". Git checks out commits in the middle, and the developer tests each one. After a few steps, Git identifies the exact commit that introduced the bug.

**Case 3: A corrupted repository.**

A developer's hard drive fails. They replace the drive, clone the repository from the remote, and are back to work in minutes. Every teammate's clone is a full backup — no data is lost.

## Examples

**Example: Tracking a single developer's work.**

Before VCS: saving files with names like `index-v2.html`, `index-v3.html`, `index-FINAL.html`.

With Git:
```bash
git init
echo "<h1>Hello</h1>" > index.html
git add index.html
git commit -m "Add initial HTML file"
# Later...
echo "<h1>Hello World</h1>" > index.html
git commit -am "Update heading text"
# View history
git log --oneline
# Output:
# abc1234 Update heading text
# def5678 Add initial HTML file
```

**Example: Two developers collaborating.**

Alice and Bob both clone the same repository. Alice adds a feature, commits, and pushes. Bob does the same with a different feature. Both changes are merged via a pull request:

```bash
# Alice
git switch -c feature-login
# ... make changes ...
git add -A && git commit -m "Add login form"
git push -u origin feature-login
# Open PR, get approved, merge to main

# Bob
git switch -c feature-export
# ... make changes ...
git add -A && git commit -m "Add CSV export"
git push -u origin feature-export
# Open PR, get approved, merge to main
```

Alice and Bob never step on each other's toes, even though they are working in the same repository at the same time.

## Study Cases

**Case 1: The lost laptop.**

A developer's laptop is stolen. They had uncommitted work on three features. With Git, the committed work is safe on the remote — they clone the repository on a new machine and lose nothing. Only the uncommitted changes are gone, which is a few hours of work instead of weeks. The lesson: commit early and often, push regularly.

**Case 2: The accidental deletion.**

A developer accidentally runs `rm -rf src/` in the wrong directory. Since the project is a Git repository, they restore everything with:

```bash
git checkout -- src/
```

The files are back in seconds. No backup restoration, no panic. Every Git operation is reversible because the history is never lost.

**Case 3: The broken deployment.**

A new release causes a spike in error rates. The team identifies the bad commit and rolls back:

```bash
git revert abc1234
git push
```

The deployment is rolled forward to a new commit that undoes the bad changes. The rollback is tracked in history for the post-mortem. The team can later re-apply the changes after fixing the issue.

**Case 4: Finding a regression across 2000 commits.**

A feature that worked two months ago is now broken. Instead of manually checking every commit:

```bash
git bisect start
git bisect bad                 # current commit is bad
git bisect good abc123         # commit from two months ago is good
# Git checks out the midpoint. Test. Mark good or bad.
# After ~11 steps (log2(2000)), Git identifies the exact commit.
git bisect reset
```

The bug was introduced by a seemingly unrelated change to a configuration file. Without `git bisect`, finding this would have taken hours or days.

**Case 5: Restoring a deleted branch.**

A developer accidentally deletes a feature branch that had not been merged or pushed:

```bash
git branch -D important-feature
```

Recovery:

```bash
git reflog                      # find the commit hash of the branch tip
git branch important-feature abc1234  # recreate the branch at that commit
```

The reflog records every movement of HEAD for 90 days by default. It is the safety net for branch accidents.

## Examples

**Example: Git data model visualized.**

A simple repository with two commits:

```
$ git init
$ echo "hello" > file.txt
$ git add file.txt
$ git commit -m "Initial commit"

# Git stores:
#   blob:  ce013625030ba8dba906f756967f9e9ca3944648  (content "hello\n")
#   tree:  fd3a3b4d1a9c8e7b6d5f4c3b2a1f0e9d8c7b6a5  (file.txt → blob)
#   commit: abc1234...  (tree, author, message, no parent)
```

After the second commit:

```
$ echo "world" >> file.txt
$ git add file.txt
$ git commit -m "Add world"

# Git reuses the unchanged blob for the new tree
#   The second tree contains a new blob for the updated file.txt
#   The second commit points to the new tree and has the first commit as parent
```

The two blobs exist independently. Git never overwrites objects — it only adds new ones. This is why Git never loses data. Even deleting a file just adds a new tree without that file; the old tree and blobs are still in the object database.

**Example: Why Git is fast.**

All of these operations are local (no network):

| Operation | Local or Remote | Approximate time |
|-----------|----------------|-----------------|
| `git status` | Local | 10ms |
| `git add` | Local | 5ms |
| `git commit` | Local | 20ms |
| `git log` | Local | 15ms |
| `git diff` | Local | 20ms |
| `git branch` | Local | 5ms |
| `git push` | Remote (network) | 500ms-5s |
| `git pull` | Remote (network) | 500ms-5s |

The only operations that require network are `push`, `pull`, and `fetch`. Everything else is instantaneous.

## Glossary

| Term | Definition |
|------|------------|
| Repository | A database containing the full history and current state of a project |
| Commit | A snapshot of changes with a unique hash and message |
| Working tree | The current state of files on disk |
| Staging area | An intermediate area to prepare the next commit |
| Branch | A movable pointer to a specific commit, enabling parallel development |
| Merge | Combining changes from two branches |
| Remote | A copy of the repository hosted on another server |
| Clone | A local copy of a repository |
| Pull | Fetching and integrating changes from a remote |
| Push | Sending local commits to a remote |
| Hash | A SHA-1 checksum that uniquely identifies an object |
| Blob | A Git object storing file content |
| Tree | A Git object mapping names to hashes (directory listing) |
| Ref | A named pointer to a commit (branch name, tag name, HEAD) |
| HEAD | A ref pointing to the current commit or branch |
| Rebase | Replaying commits from one base onto another |
| Cherry-pick | Applying a single commit from one branch to another |
| Bisect | A tool that binary searches history to find when a bug was introduced |
| Reflog | A log of all HEAD movements, useful for recovery |

## Quick References

- [Pro Git Book](https://git-scm.com/book) — the official and comprehensive Git reference, available free online
- [Git SCM Documentation](https://git-scm.com/docs) — command reference for every Git subcommand
- [GitHub Git Tutorials](https://github.com/git-guides) — beginner-friendly guides from GitHub
- [Atlassian Git Tutorials](https://www.atlassian.com/git/tutorials) — visual guides covering Git concepts and workflows

## Next Steps

- [Git Basics](git-basics.md) — get started with installation, configuration, and the core workflow
- [Branching & Merging](branching-and-merging.md) — learn to work with branches and combine work
