# Git Basics

## Description

A practical guide to installing Git, configuring it for the first time, and mastering the daily workflow of staging, committing, and inspecting changes.

## Prerequisites

- [Introduction to Version Control](intro/what-is-version-control.md) — understand what version control is and why it matters

## Table of Contents

- [Installation](#installation)
- [First-Time Configuration](#first-time-configuration)
- [Creating a Repository](#creating-a-repository)
- [The Core Workflow](#the-core-workflow)
- [Inspection Tools](#inspection-tools)
- [Ignoring Files](#ignoring-files)
- [Undoing Changes](#undoing-changes)
- [Working with Remotes](#working-with-remotes)
- [Tags](#tags)
- [Basic Workflow Example](#basic-workflow-example)
- [Common Pitfalls](#common-pitfalls)
- [Best Practices](#best-practices)

## Content

### Installation

**Linux (Debian/Ubuntu):**

```bash
sudo apt update
sudo apt install git
git --version
```

**Linux (Fedora/RHEL):**

```bash
sudo dnf install git
git --version
```

**macOS:**

The easiest way is via Homebrew:

```bash
brew install git
```

Alternatively, the Xcode Command Line Tools include Git:

```bash
xcode-select --install
```

**Windows:**

Download the installer from [git-scm.com](https://git-scm.com) and run it. The default options are sensible for most users. Make sure "Git Bash" is selected — it provides a Unix-like terminal on Windows.

After installation, verify it works:

```bash
git --version
```

On Windows, use "Git Bash" (installed with Git) instead of the default Command Prompt for a better experience.

### First-Time Configuration

Git needs to know who you are because every commit is stamped with an author. Set your name and email globally — these will be attached to every commit you make:

```bash
git config --global user.name "Your Full Name"
git config --global user.email "your.email@example.com"
```

These settings are stored in `~/.gitconfig`. You can verify them:

```bash
git config --global --list
```

Other useful global settings:

```bash
# Set your preferred editor (instead of the default vim/nano)
git config --global core.editor "code --wait"    # VS Code
git config --global core.editor "nano"            # Nano

# Set the default branch name for new repositories
git config --global init.defaultBranch main

# Enable color output for all Git commands
git config --global color.ui auto

# Configure line-ending handling
git config --global core.autocrlf input          # macOS/Linux
git config --global core.autocrlf true            # Windows
```

You can override global settings per-repository by omitting `--global` and running the command inside a repo, which writes to `.git/config` instead of `~/.gitconfig`.

### Creating a Repository

There are two ways to get a Git repository:

**Start from scratch with `git init`:**

```bash
mkdir my-project
cd my-project
git init
```

This creates a hidden `.git` directory. The project is now a Git repository, but it contains no commits yet.

**Clone an existing repository with `git clone`:**

```bash
git clone https://github.com/user/repository.git
cd repository
```

This downloads the entire project history and creates a working copy. Cloning is how you start working on an existing project.

### The Core Workflow

Git's basic workflow is a three-step cycle:

1. **Modify** files in your working tree.
2. **Stage** the changes you want to include in the next commit.
3. **Commit** the staged changes, creating a snapshot in the repository.

**Checking the current state — `git status`:**

This is the most frequently used command. It shows which files are modified, which are staged, and which are untracked:

```bash
git status
```

A clean status means nothing has changed since the last commit.

**Staging changes — `git add`:**

Staging tells Git which changes to include in the next commit:

```bash
# Stage a single file
git add index.html

# Stage all changes in the current directory
git add .

# Stage all changes in the entire repository
git add -A

# Stage interactively — choose which hunks to stage
git add -p
```

The `-p` (patch) flag is particularly useful. It lets you stage parts of a file, keeping related changes together in the same commit while leaving unrelated edits unstaged.

**Committing — `git commit`:**

Once changes are staged, commit them:

```bash
git commit -m "Add user registration form"
```

The commit message should be short (under 50 characters) and describe *what* changed and *why*, not *how*. For longer messages, omit `-m` and Git opens your editor:

```bash
git commit
```

Write a short summary on the first line, leave a blank line, then add a detailed description.

**A complete cycle:**

```bash
echo "Hello" > README.md
git status                    # README.md is untracked
git add README.md
git status                    # README.md is staged
git commit -m "Add README"
git status                    # clean — nothing to commit
```

### Inspection Tools

**Viewing commit history — `git log`:**

```bash
git log                       # full history
git log --oneline             # compact, one line per commit
git log --graph               # show branch structure
git log --oneline --graph --all  # everything, compact
```

Press `q` to exit the log viewer.

**Viewing changes — `git diff`:**

```bash
git diff                      # unstaged changes
git diff --staged             # staged changes (what will be committed)
git diff HEAD                 # all changes since the last commit
git diff abc123..def456       # changes between two commits
```

**Viewing a specific commit — `git show`:**

```bash
git show abc123               # show the changes in commit abc123
git show HEAD                 # show the latest commit
```

### Ignoring Files

Not every file belongs in version control. Compiled binaries, dependencies, environment files, and operating system artifacts should be excluded.

Create a `.gitignore` file in the root of your repository:

```gitignore
# Compiled output
*.class
*.o
*.pyc
dist/
build/

# Dependencies
node_modules/
vendor/
.bundle/

# Environment
.env
.env.local
*.log

# IDE and editor files
.vscode/
.idea/
*.swp
*.swo

# OS files
.DS_Store
Thumbs.db
```

Each pattern specifies files or directories to ignore. The `.gitignore` itself should be committed so the whole team benefits.

You can also use `.gitignore` patterns:

```gitignore
# Ignore all .log files
*.log

# But keep this specific log
!important.log

# Ignore a directory
build/

# Ignore files in any nested config directory
**/config/secrets.*
```

### Undoing Changes

Git provides several ways to undo work, depending on where the change is.

**Unstage a file (keep changes):**

```bash
git restore --staged file.txt
```

**Discard unstaged changes (permanent):**

```bash
git restore file.txt
```

**Amend the last commit (change message or add missing files):**

```bash
git commit --amend -m "Better commit message"
```

If you forgot to stage a file:

```bash
git add forgotten-file.txt
git commit --amend --no-edit
```

Only amend commits that have not been pushed to a shared branch.

**Reset to a previous commit (use with extreme caution):**

```bash
# Soft reset — move HEAD back but keep changes staged
git reset --soft HEAD~1

# Mixed reset — move HEAD back and unstage (default)
git reset HEAD~1

# Hard reset — discard everything (irreversible without reflog)
git reset --hard HEAD~1
```

`--hard` discards uncommitted changes permanently. Use `git reflog` to recover if needed.

**Revert a commit safely (creates a new commit that undoes the changes):**

```bash
git revert abc123
```

Unlike `reset`, `revert` is safe to use on shared branches because it does not rewrite history.

### Working with Remotes

A remote is a Git repository hosted on another server — typically GitHub, GitLab, or Bitbucket.

**View remotes:**

```bash
git remote -v
```

**Add a remote:**

```bash
git remote add origin https://github.com/user/repository.git
```

**Push commits:**

```bash
git push origin main          # push main branch to origin
git push -u origin feature    # set upstream tracking for a new branch
```

The `-u` flag sets the upstream, so future pushes can be just `git push`.

**Pull changes:**

```bash
git pull                      # fetch and merge changes from upstream
git pull --rebase             # fetch and rebase instead of merge
```

**Fetch without merging:**

```bash
git fetch                     # download changes but do not merge
git log --oneline origin/main # inspect the remote branch
git merge origin/main         # merge manually
```

### Tags

Tags mark specific points in history, typically releases:

```bash
git tag v1.0.0                # create a lightweight tag
git tag -a v1.0.0 -m "Release version 1.0.0"  # annotated tag
git tag                       # list all tags
git push origin v1.0.0        # push a tag
git push --tags               # push all tags
```

### Basic Workflow Example

Here is a complete walkthrough from scratch:

```bash
# Create and initialize a project
mkdir hello-world
cd hello-world
git init

# Create a file
echo "# Hello World" > README.md
git status                    # README.md is untracked

# Stage and commit
git add README.md
git commit -m "Initial commit"

# Create a remote on GitHub, then connect
git remote add origin https://github.com/yourname/hello-world.git
git push -u origin main

# Make more changes
echo "print('hello')" > main.py
git add main.py
git commit -m "Add hello script"

# Push to GitHub
git push
```

### Common Pitfalls

**Committing large files** — Git handles text files well but struggles with large binaries. Use `.gitignore` for build artifacts and consider Git LFS for large assets.

**Vague commit messages** — "fix stuff" or "update" are not helpful. Write messages that explain the motivation: "Fix login crash when email contains trailing spaces".

**Committing secrets** — API keys, passwords, and tokens committed to Git are exposed forever, even if deleted later. Use `.gitignore` or environment variables.

**Forgetting to pull before pushing** — if the remote has new changes, your push will be rejected. Pull first, resolve any conflicts, then push.

**Using `--hard` without thinking** — `git reset --hard` discards uncommitted work. Always check `git status` first.

### Best Practices

- **Commit early and often** — small, focused commits are easier to review and debug. Aim for one logical change per commit.
- **Write meaningful commit messages** — the first line is a summary (50 chars max), followed by a blank line and optional body explaining the reasoning.
- **Keep the main branch stable** — never commit directly to main. Use branches for all changes.
- **Pull before starting work** — start each session with `git pull` to avoid divergence.
- **Use `.gitignore` from day one** — it is harder to clean up ignored files after they have been tracked.
- **Review your changes before committing** — use `git diff` to make sure you are only committing what you intend.
- **Never force push shared branches** — `git push --force` rewrites history and breaks other developers' clones. Use `--force-with-lease` if absolutely necessary.

## Glossary

| Term | Definition |
|------|------------|
| Repository | The `.git` directory containing full project history |
| Commit | A snapshot of staged changes with a unique hash |
| Staging area | The index where changes are prepared before committing |
| Working tree | Files on disk that you are editing |
| HEAD | A pointer to the current commit (the tip of the current branch) |
| Remote | A URL pointing to another copy of the repository |
| Origin | The default name for the primary remote repository |
| Clone | A local copy of a remote repository |
| Push | Sending commits to a remote repository |
| Pull | Fetching and merging changes from a remote |
| Fetch | Downloading changes without merging |
| Hash | A SHA-1 checksum that uniquely identifies a commit |
| Ref | A named pointer to a commit (branch, tag, HEAD) |
| Reflog | A local log of all HEAD movements, useful for recovery |

## Quick References

- [Pro Git Book — Getting Started](https://git-scm.com/book/en/v2/Getting-Started-About-Version-Control) — the official Git book, chapters 1 and 2 cover everything above
- [Git SCM Reference](https://git-scm.com/docs) — complete command documentation
- [Atlassian Git Tutorials](https://www.atlassian.com/git/tutorials) — visual guides to Git concepts
- [GitHub Git Cheat Sheet](https://education.github.com/git-cheat-sheet-education.pdf) — a downloadable PDF reference

## Next Steps

- [Branching & Merging](branching-and-merging.md) — learn to work with branches and combine work from multiple developers
