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
- [Branching Strategies](#branching-strategies)
- [Practical Tips](#practical-tips)
- [Version Control in the Development Workflow](#version-control-in-the-development-workflow)
- [Learning Tips](#learning-tips)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

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

1. **Pull** the latest changes from the shared repository to stay up to date. Prefer `git pull --rebase` to avoid unnecessary merge commits.
2. **Create a branch** for the feature or fix being worked on. Name it descriptively: `feature/add-login`, `fix/null-pointer-crash`.
3. **Make changes** to files in the working tree — add, modify, delete.
4. **Stage** the changes that should be part of the next commit. Use `git add <file>` for specific files or `git add -p` for interactive staging of individual hunks.
5. **Commit** the staged changes with a descriptive message. Use the imperative mood: "Add login form validation" not "Added login form validation".
6. **Push** the branch to the shared repository so others can see it. Use `git push -u origin branch-name` on the first push to set the upstream tracking.
7. **Open a pull request** (or merge request) to propose merging the changes into the main branch.
8. **Review** and address feedback from teammates. Make additional commits and push them to update the PR.
9. **Merge** once the changes are approved. Options include creating a merge commit, squashing commits, or rebasing before merging.

Steps 1-6 happen on the command line. Steps 7-9 happen on a platform like GitHub, GitLab, or Bitbucket.

**Stashing:** When you need to switch context without committing unfinished work, use `git stash` to temporarily save changes:

```bash
git stash push -m "WIP: login form validation"
git stash list
git stash pop   # restore and remove from stash
git stash drop  # remove without restoring
git stash apply # restore but keep in stash
```

**Interactive rebase:** Clean up commit history before merging with `git rebase -i`. This lets you squash, reorder, rename, and drop commits:

```bash
git rebase -i HEAD~5
```

This opens an editor where you can change `pick` to `squash`, `reword`, `edit`, `drop`, or `fixup` for each commit. This is invaluable for polishing history before sharing.

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

### Branching Strategies

How you use branches determines your team's collaboration model. Three strategies dominate modern development.

**Git Flow** uses two permanent branches (`main` and `develop`) plus supporting branches for features, releases, and hotfixes. Features branch off `develop`, releases are prepared on a `release` branch, and hotfixes branch directly from `main`.

```
main ────●──────────●──────────●
          \        / \        /
develop ──●──●──●──●──●──●──●──●
              |     |        |
          feature  release  hotfix
```

Git Flow works well for projects with scheduled releases and maintenance branches. It is overkill for continuous delivery — the overhead of managing multiple branch types slows teams that deploy daily.

**GitHub Flow** is simpler: everything branches from `main`, and every change is proposed via a pull request. There are no `develop` or `release` branches. Once a PR is reviewed and passes CI, it merges directly to `main` and is deployed.

```
main ──●──●──●──●──●──●──●──●──●
        |     |        |
      feature fix    feature
```

GitHub Flow suits continuous deployment where `main` is always deployable. It minimizes bureaucracy but requires disciplined CI/CD and short-lived branches.

**Trunk-Based Development** takes simplicity further. Developers work on short-lived branches (hours, not days) and merge directly to `main` multiple times per day. Feature flags gate incomplete functionality. There are no long-running feature branches.

```
main ──●──●──●──●──●──●──●──●──●
        |  |  |  |  |  |  |  |  |
      (tiny branches, merged within hours)
```

Trunk-based development reduces merge conflicts, accelerates feedback, and enables continuous deployment. It requires strong automated testing and feature flag infrastructure.

**GitLab Flow** adds environment branches (`staging`, `production`) between feature branches and deployment. Changes flow through environments in a controlled pipeline.

Choose the strategy that matches your release cadence:
- **Scheduled releases:** Git Flow
- **Continuous deployment:** GitHub Flow
- **High-velocity CI/CD:** Trunk-Based Development
- **Multi-environment pipelines:** GitLab Flow

### Practical Tips

**Write good commit messages.** Follow the conventional format: a short subject line (50 chars or less), a blank line, then a body explaining what and why. The subject line should complete the sentence "This commit will...":

```
feat(auth): add OAuth2 login provider

Implement Google and GitHub OAuth2 authentication.
The existing session-based auth remains unchanged.

Closes #142
```

**Use .gitignore from the start.** Ignore build artifacts (`node_modules/`, `dist/`, `build/`), dependency directories, environment files (`.env`, `*.local`), and OS metadata (`.DS_Store`, `Thumbs.db`). Use gitignore.io or GitHub's template repository to generate a comprehensive file.

**Rebase before merging public branches.** Keep the commit history linear by rebasing feature branches onto the latest `main` before merging. Avoid merge commits in feature branches:

```bash
git checkout feature-branch
git rebase main
```

**Prefer merge commits for integrating features.** When merging a feature branch with multiple commits into `main`, use `--no-ff` to force a merge commit that preserves the branch context:

```bash
git checkout main
git merge --no-ff feature-branch
```

**Commit early, commit often.** Small, focused commits are easier to review, revert, and bisect. A commit should represent one logical change. Avoid mixing formatting changes with logic changes in the same commit.

**Use `git bisect` to find bugs.** When a regression appears, bisect automates searching through history to find the exact commit that introduced the bug:

```bash
git bisect start
git bisect bad HEAD
git bisect good v1.0.0
# Git checks out the midpoint. Test it, then mark good or bad.
git bisect good   # or: git bisect bad
# Repeat until the first bad commit is found.
git bisect reset
```

Combine `git bisect run` with a script for fully automated bisecting:

```bash
git bisect start HEAD v1.0.0
git bisect run npm test
```

**Protect sensitive data.** Never commit API keys, passwords, or certificates. Add sensitive files to `.gitignore` before the first commit. If you commit secrets accidentally, rotate them immediately — removing them from the file does not remove them from history. Use tools like `git-secrets` or `talisman` to prevent leaks.

**Set up Git hooks.** Automate quality checks with client-side hooks. Run linters, formatters, and type checkers before allowing a commit:

```bash
# .git/hooks/pre-commit
npm run lint && npm run typecheck
```

Use `husky` for Node.js projects to manage hooks via `package.json`:

```json
{
  "husky": {
    "hooks": {
      "pre-commit": "lint-staged"
    }
  }
}
```

**Use `git worktree` for parallel work.** When you need to work on two branches simultaneously without stashing, create a linked working tree:

```bash
git worktree add ../project-hotfix hotfix
```

**Master the reflog.** Git's reflog records every movement of HEAD. If you lose a commit after a destructive rebase or reset, the reflog is your safety net:

```bash
git reflog
git reset --hard HEAD@{2}
```

**Leverage `git blame` with care.** `git blame` shows who last modified each line. Use it to understand why a change was made, not to assign blame.

### Version Control in the Development Workflow

Version control is not an afterthought — it is central to how modern software is built. It connects to every part of the development lifecycle:

**Code review** — platforms like GitHub use pull requests to make code review a first-class activity. Every change is discussed, approved, and tracked before reaching the main branch.

**CI/CD** — continuous integration pipelines are triggered by commits and pull requests. Tests run automatically on every push. If a commit breaks the build, the team knows immediately.

**Deployment** — releases are tagged commits. Rollbacks are reverts. Every environment (dev, staging, production) runs a known commit with a known state.

**Debugging** — `git bisect` performs a binary search through history to find the exact commit that introduced a bug. Combined with automated tests, this can pinpoint the offending change in seconds.

**Experimentation** — branches let teams try radical ideas without risk. If an experiment works, it is merged. If not, the branch is deleted and nobody remembers the failed attempt.

**Documentation** — the commit log is the most accurate documentation of why the code evolved the way it did. A well-maintained log answers questions that no other documentation covers.

## Learning Tips

**Visualize your history.** Run `git log --graph --oneline --all` to see the branch structure. This mental model of commits as a directed acyclic graph is essential for understanding rebase, merge, and cherry-pick.

**Practice recovery in a throwaway repo.** Create a test repository and practice undoing commits with `git reset --soft`, `git reset --mixed`, and `git reset --hard`. Recover lost commits with `git reflog`. Resolve merge conflicts intentionally to build confidence.

**Pair bisect with automated tests.** The real power of `git bisect` is `bisect run`. Write a script that exits 0 for good and non-0 for bad, then let Git automate the search across hundreds of commits.

**Use a GUI alongside the CLI.** Tools like GitKraken, SourceTree, or GitLens in VS Code provide visual diffs and history exploration. Use them to build intuition, then transition to CLI for speed.

**Read the Pro Git book.** The official Pro Git book is comprehensive, free, and well-written. Read chapters 1-3 for fundamentals, then chapters on branching and rebasing.

**Practice daily.** Use version control for every project, even personal ones. The habit of committing, branching, and reviewing your own history builds fluency faster than any tutorial.

**Learn one command per day.** Git has over 150 subcommands. Focus on the 20 that matter most: `init`, `clone`, `add`, `commit`, `push`, `pull`, `fetch`, `branch`, `checkout`, `merge`, `rebase`, `log`, `diff`, `status`, `stash`, `reset`, `revert`, `bisect`, `reflog`, `cherry-pick`.

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
| Detached HEAD | A state where HEAD points directly to a commit instead of a branch |
| Fast-forward merge | A merge where the target branch has not diverged, simply moving the pointer forward |
| Squash merge | Combining all commits from a branch into a single commit on the target |
| Fork | A server-side copy of a repository, typically used in open-source contributions |
| Upstream | The remote repository that a fork or local repo was originally cloned from |
| Submodule | A reference to another Git repository embedded within a repository |
| Worktree | A linked working directory for simultaneous work on multiple branches |
| Stash | Temporary storage for uncommitted changes |
| Hook | A script that runs automatically on Git events (pre-commit, post-commit) |
| Object database | The internal storage (.git/objects) containing blobs, trees, commits, and tags |
| Dangling commit | A commit not reachable from any branch or tag, subject to garbage collection |
| Diff | A representation of changes between two versions of files |
| Tracked file | A file that Git is already monitoring for changes |
| Untracked file | A file in the working tree that Git has not been told about |
| Ignored file | A file matched by `.gitignore` patterns that Git will not track |

## Quick References

- [Pro Git Book](https://git-scm.com/book) — the official and comprehensive Git reference, available free online
- [Git SCM Documentation](https://git-scm.com/docs) — command reference for every Git subcommand
- [GitHub Git Guides](https://github.com/git-guides) — beginner-friendly guides from GitHub
- [Atlassian Git Tutorials](https://www.atlassian.com/git/tutorials) — visual guides covering Git concepts and workflows
- [Learn Git Branching](https://learngitbranching.js.org/) — interactive visual tutorial for branching and merging
- [Conventional Commits](https://www.conventionalcommits.org/) — specification for structured commit messages
- [gitignore.io](https://www.toptal.com/developers/gitignore) — generate .gitignore files for projects and IDEs
- [Oh Shit, Git!?!](https://ohshitgit.com/) — practical recovery recipes for common Git mistakes
- [Flight Rules for Git](https://github.com/k88hudson/git-flight-rules) — guide for what to do when things go wrong

## Next Steps

- [Git Basics](git-basics.md) — get started with installation, configuration, and the core workflow
- [Branching & Merging](branching-and-merging.md) — learn to work with branches and combine work
