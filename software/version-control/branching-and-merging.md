# Branching & Merging

## Description

How branches enable parallel development, how to merge work back together, how to resolve conflicts when they arise, and the most common branching strategies used by teams.

## Prerequisites

- [Git Basics](git-basics.md) — comfortable with add, commit, push, pull

## Table of Contents

- [What is a Branch](#what-is-a-branch)
- [Creating and Switching Branches](#creating-and-switching-branches)
- [Merging](#merging)
- [Merge Conflicts](#merge-conflicts)
- [Rebasing](#rebasing)
- [Cherry-Picking](#cherry-picking)
- [Branching Strategies](#branching-strategies)
- [Tags vs Branches](#tags-vs-branches)
- [Common Workflows](#common-workflows)
- [Best Practices](#best-practices)

## Content

### What is a Branch

A branch is a movable pointer to a specific commit. When you create a new branch, Git creates a new pointer. As you make commits on that branch, the pointer moves forward while the original branch stays where it was.

Branches make it possible to work on multiple features, bug fixes, or experiments simultaneously without interfering with each other. Each branch is an isolated environment. Changes on one branch do not affect another until you explicitly merge them.

The default branch is called `main` (or `master` on older repositories). When you first clone a repository, you are on `main`.

### Creating and Switching Branches

**Create a new branch:**

```bash
git branch feature-login
```

This creates a branch named `feature-login` pointing at the current commit. The branch exists but you are still on `main`.

**Switch to a branch:**

```bash
git switch feature-login

# Older Git versions use:
git checkout feature-login
```

**Create and switch in one command:**

```bash
git switch -c feature-login

# Older syntax:
git checkout -b feature-login
```

**List branches:**

```bash
git branch                     # local branches, * marks current
git branch -r                  # remote branches
git branch -a                  # all branches (local + remote)
```

**Delete a branch:**

```bash
git branch -d feature-login    # safe delete (must be merged)
git branch -D feature-login    # force delete (discard changes)
```

**Rename a branch:**

```bash
git branch -m old-name new-name
```

### Merging

Merging combines the work from two branches into one. The most common scenario is merging a feature branch back into `main`.

**Basic merge:**

```bash
git switch main
git merge feature-login
```

This takes the commits from `feature-login` and integrates them into `main`.

**Fast-forward merge:**

If the target branch (`main`) has not moved since the feature branch was created, Git simply moves the pointer forward. This creates a linear history with no merge commit:

```
main:   A --- B
                \
feature-login:   C --- D

After git merge feature-login:

main:   A --- B --- C --- D
```

Git performs a fast-forward by default when possible. To force a merge commit even when a fast-forward is possible:

```bash
git merge --no-ff feature-login
```

**Three-way merge:**

If both branches have diverged (both have new commits), Git performs a three-way merge using the two branch tips and their common ancestor. This creates a merge commit with two parents:

```
main:   A --- B --- E
              \
feature-login: C --- D

After git merge feature-login:

main:   A --- B --- E --- F (merge commit)
              \         /
feature-login: C --- D
```

The merge commit `F` has two parents: `E` (the previous tip of `main`) and `D` (the tip of `feature-login`).

**Viewing merged branches:**

```bash
git branch --merged           # branches that have been merged
git branch --no-merged        # branches that have not been merged
```

### Merge Conflicts

A conflict occurs when two branches modify the same part of the same file in different ways. Git cannot automatically decide which change to keep and asks you to resolve it manually.

**When a conflict happens:**

Git pauses the merge and marks the conflicted files. Running `git status` shows them under "both modified".

**Conflict markers in the file:**

```
<<<<<<< HEAD
This is the version from main.
=======
This is the version from feature-login.
>>>>>>> feature-login
```

- `<<<<<<< HEAD` — the start of the conflict on the current branch
- `=======` — the divider between the two versions
- `>>>>>>> feature-login` — the end of the conflict, showing the incoming branch

**Resolving a conflict:**

1. Open the conflicted file in your editor.
2. Decide which version to keep, or edit to create a combined version.
3. Remove the conflict markers (`<<<<<<<`, `=======`, `>>>>>>>`).
4. Save the file.
5. Stage the resolved file: `git add file.txt`
6. Continue the merge: `git commit`

Or abort the merge entirely:

```bash
git merge --abort
```

**Conflict resolution strategies:**

| Strategy | When to use |
|----------|-------------|
| Keep ours | The current branch's version is always correct |
| Keep theirs | The incoming branch's version is always correct |
| Edit manually | Neither version is entirely right — combine them |
| Use a merge tool | Run `git mergetool` to use a visual diff tool |

**Preventing conflicts:**

- Pull frequently to stay up to date with `main`.
- Keep branches short-lived (days, not weeks).
- Communicate with your team about who is modifying which files.
- Rebase instead of merge to maintain a linear history (see below).

### Rebasing

Rebasing is an alternative to merging. Instead of creating a merge commit, it rewrites the feature branch's history so it appears to have started from the latest `main`:

```bash
git switch feature-login
git rebase main
```

Before rebase:

```
main:   A --- B --- E
              \
feature-login: C --- D
```

After rebase:

```
main:   A --- B --- E
                    \
feature-login:       C' --- D'
```

Commits `C` and `D` are replayed on top of `E`, creating new commits `C'` and `D'` with new hashes. The result is a perfectly linear history.

**Interactive rebase:**

Use `-i` to edit, squash, reorder, or drop commits:

```bash
git rebase -i HEAD~3          # rebase the last 3 commits
```

This opens an editor showing the commits. Change `pick` to:

| Command | Effect |
|---------|--------|
| `pick` | Keep the commit as is |
| `reword` | Change the commit message |
| `squash` | Combine with the previous commit |
| `fixup` | Combine, discarding the message |
| `drop` | Remove the commit |

**When to use merge vs rebase:**

| Scenario | Use |
|----------|-----|
| Merging a feature branch into main | Merge (preserves context that work happened on a branch) |
| Updating a feature branch with latest main | Rebase (keeps history linear, avoids merge commits) |
| Shared branch (pushed, other people use it) | Merge. Never rebase shared branches. |
| Cleaning up local commits before pushing | Interactive rebase |

**Golden rule of rebasing:** Never rebase commits that have been pushed to a shared branch. Rebasing rewrites history — other developers' clones will be out of sync and they will have to do a force pull to recover.

### Cherry-Picking

Cherry-picking applies a specific commit from one branch onto another:

```bash
git switch main
git cherry-pick abc123
```

This takes the changes introduced by commit `abc123` and creates a new commit on `main` with the same changes (but a new hash).

Use cherry-pick when you need a specific fix from one branch but do not want the entire branch. For example, a hotfix committed on a feature branch can be cherry-picked to `main` without merging the incomplete feature.

### Branching Strategies

**GitHub Flow — simple and widely used:**

```
main  : A --- B --- C --- D --- E
                \         /
feature-x:       X1 - X2
```

- `main` is always deployable.
- Create a branch for every feature or fix.
- Open a pull request for discussion.
- Merge to `main` and deploy.

Best for: teams that deploy frequently, continuous delivery shops.

**Git Flow — more structured:**

```
main     : v1.0 --- v1.1 --- v1.2
develop  :  ---------- dev ---------
                \         /
feature-x:       X1 - X2
                  \
release-1.1:      R1 - R2
                  \
hotfix-1.0.1:    H1
```

- `main` stores official release history.
- `develop` is the integration branch for features.
- Feature branches branch off `develop`.
- Release branches prepare a release.
- Hotfix branches fix production issues and merge to both `main` and `develop`.

Best for: scheduled releases, projects with formal release cycles.

**Trunk-Based Development:**

```
main  : A --- B --- X1 --- C --- X2 --- D
```

- All developers work on short-lived branches (hours, not days).
- Branches are merged to `main` multiple times per day.
- Feature flags hide incomplete features.

Best for: CI/CD, organizations with strong automated testing, teams that need to avoid long-lived branches.

### Tags vs Branches

Tags are static pointers to a specific commit, while branches move as new commits are made:

```bash
git tag v1.0.0                # tag the current commit
git tag v1.0.0 abc123         # tag a specific commit
git push origin v1.0.0        # push a tag
```

Use tags to mark releases. Never make commits on a tag — if you need to patch a release, create a branch from the tag, fix it, and tag the fix as `v1.0.1`.

### Common Workflows

**Starting a new feature:**

```bash
git switch main
git pull
git switch -c feature-xyz
# make changes...
git add .
git commit -m "Implement feature xyz"
git push -u origin feature-xyz
```

**Updating your feature branch with latest main:**

```bash
git switch feature-xyz
git rebase main
# resolve any conflicts
git push --force-with-lease
```

**Finishing a feature (merge to main):**

```bash
git switch main
git pull
git merge --no-ff feature-xyz
git push
git branch -d feature-xyz
```

**Hotfix on a release:**

```bash
git switch main
git pull
git switch -c hotfix-security
# fix the vulnerability...
git add .
git commit -m "Fix XSS vulnerability in login form"
git push -u origin hotfix-security
# open PR, get approval, merge
git switch main
git merge hotfix-security
git tag v1.0.1
git push origin v1.0.1
```

### Best Practices

- **Keep branches short-lived** — the longer a branch lives, the harder it is to merge. Aim for a few days at most.
- **Pull and rebase daily** — start each work session by rebasing your feature branch on the latest main.
- **Write descriptive branch names** — use `type/description` or `type/issue-number-description`: `fix/login-crash`, `feat/user-dashboard`, `chore/update-deps`.
- **Delete branches after merging** — stale branches clutter the remote. GitHub and GitLab offer auto-deletion after merge.
- **Never force push shared branches** — use `--force-with-lease` instead of `--force` for your own branches.
- **Prefer merge for public history** — merge commits preserve the context that work happened in parallel. Use rebase for local cleanup.
- **Resolve conflicts carefully** — always understand both sides of a conflict before resolving. Test after resolving.

## Study Cases

**Case 1: A merge conflict in a configuration file.**

You and a teammate both edited `config.json`. Your changes are on `main`, theirs are on `feature-deploy`:

```json
<<<<<<< HEAD
  "port": 8080,
=======
  "port": 3000,
>>>>>>> feature-deploy
```

The correct port for this environment is 3000. You keep theirs and remove the markers:

```json
  "port": 3000,
```

Stage, commit, and verify the application works.

**Case 2: Accidental commit on main.**

You committed directly to `main` instead of a feature branch:

```bash
# Create the branch at the current commit
git branch feature-oops
# Move main back one commit
git reset --hard HEAD~1
# Switch to the feature branch
git switch feature-oops
```

**Case 3: Recovering from a bad rebase.**

You rebased a feature branch and the result is broken. Find the original state:

```bash
git reflog                    # shows all HEAD movements
git switch -c feature-restore abc123  # create a branch at the pre-rebase commit
```

## Glossary

| Term | Definition |
|------|------------|
| Branch | A movable pointer to a commit that enables parallel development |
| HEAD | The current commit you are viewing, typically the tip of the current branch |
| Merge | Combining changes from two branches |
| Fast-forward merge | A merge where the target branch has not diverged; Git simply moves the pointer |
| Three-way merge | A merge using two branch tips and their common ancestor, producing a merge commit |
| Merge conflict | A situation where two branches modified the same part of a file differently |
| Rebase | Replaying commits from one branch onto another to create a linear history |
| Cherry-pick | Applying a single commit from one branch onto another |
| Merge commit | A commit with two parents, created during a three-way merge |
| Force push | Overwriting a remote branch with local history (destructive) |
| Reflog | A log of all movements of HEAD, useful for recovery after errors |
| Upstream | The remote branch that corresponds to a local branch |

## Quick References

- [Pro Git Book — Branching](https://git-scm.com/book/en/v2/Git-Branching-Branches-in-a-Nutshell) — the definitive guide to Git branching
- [Atlassian — Merge vs Rebase](https://www.atlassian.com/git/tutorials/merging-vs-rebasing) — practical breakdown of when to use each
- [GitHub Flow](https://docs.github.com/en/get-started/quickstart/github-flow) — the simple branching strategy used by GitHub
- [Learn Git Branching](https://learngitbranching.js.org/) — an interactive visual tutorial for Git branching

## Next Steps

- [Remote Collaboration](remote-collaboration.md) — pull requests, code review, and working with distributed teams
