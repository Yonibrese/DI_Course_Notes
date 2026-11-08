# Branches & Collaboration

Branches let you **experiment safely** without breaking your main code.

GitHub then gives you tools (like pull requests) to share and review those changes.

This guide focuses on a **simple, beginner‑friendly branching workflow**—no rebasing or advanced history rewriting.

## What is a branch?

So far, you can imagine your Git history like this:

```bash
commit A  →  commit B  →  commit C
```

A **branch** is just a **named pointer** to a specific commit.

- `main` is usually the default branch
- When you create a new branch, you’re saying:

> “Start a new line of development from this commit.”

Conceptually:

```bash
           feature-login
          /
commit A — B — C  ← main
```

Both branches share history up to a point, then diverge.

## Why use branches?

Branches are powerful because they let you:

- Keep `main` (or `master`) stable, especially after a project is live
- Experiment or build features in isolation
- Review changes before merging them into the main line of development

Common uses:

- **Feature branches**: `feature/signup-form`
- **Bugfix branches**: `fix/login-crash`
- **Experiment branches**: `spike/new-idea`

## Creating and switching branches

Start from your local repository’s `main` branch:

```bash
git status
```

Make sure you’re on `main` (it might be called `master`) and your working directory is clean (no uncommitted changes).

### Create and switch to a new branch

```bash
git checkout -b feature-notes-formatting
```

This does two things:

- Creates a new branch called `feature-notes-formatting`
- Checks it out (switches your working directory to that branch)

You can confirm:

```bash
git branch
```

The current branch will have a `*` next to it.

## Make commits on your feature branch

Now make some changes—edit files, run tests, etc. Then:

```bash
git status
git add .
git commit -m "Improve notes formatting and headings"
```

You’ve just added a commit **on your feature branch**, not on **main**.

Your history conceptually:

```bash
commit A — B — C      ← main
             \
              D      ← feature-notes-formatting
```

## Merging a branch back into main

Once you’re happy with your changes:

1. Switch back to main
2. Merge the feature branch into it

```bash
git checkout main
git merge feature-notes-formatting
```

If Git can combine the changes automatically, you’ll see a “fast‑forward” or “merge” message.
(If not Git with propmpt you on what you need to do to finish the merge)

Your history becomes:

```bash
commit A — B — C — D  ← main, feature-notes-formatting
```

At this point you can optionally delete the feature branch:

```bash
git branch -d feature-notes-formatting
```

>[!NOTE]
>optionally you can merge branches in github then `pull` them to your working directory (this might be how the work flow will be when you are colberating with a team)

## Pulling changes from GitHub

When you work with a remote (like GitHub), others may push commits to the same branch.

To **update your local branch** with the latest remote changes:

```bash
git pull origin main 
# 'origin' refers to the url of the remote repository
# 'main' is the branch name of the remote repository that you are pulling from in this case the `main` branch
```

**This is equvalant to**:

- `git fetch` (download new commits from the remote)
- `git merge` (merge them into your current branch)

**If:**

- No one else has changed the parts of the code you’re working on

Git will usually do a **fast‑forward**:

```bash
Local:   A — B
Remote:  A — B — C
```

After `git pull`, your local branch just walks forward to match the remote.

If both you and someone else changed the same lines, Git may ask you to resolve a **merge conflict**. That’s normal—take your time, read the conflict markers, and decide what the final version should be.

## GitHub collaboration basics

Once your branch is on GitHub, you can open a **pull request (PR)**.

The typical flow:

1. Create a branch locally

2. Commit your changes

3. Push the branch to GitHub:

    ```bash
    git push -u origin feature-notes-formatting
    ```

4. On GitHub, open a **pull request** from `feature-notes-formatting` into `main`

5. Teammates review your changes, leave comments, and request updates

6. Once approved, the PR is merged and your changes land on `main` (on the remote branch you need to `pull` them to your working directory)

Even when working solo, PRs can still be useful:

- They give you a clean diff of what’s about to change
- They create a documented discussion thread for major changes

## Forks vs collaborators (high level)

There are two common ways to contribute to a repository on GitHub:

- **Collaborator**:
  - You’re added directly to the repo with write access
  - You can create branches in the main repository
- **Fork**:
  - You create your own copy (“fork”) of the repository under your account
  - You make changes in your fork
  - You open a pull request from **your fork back** to the original repo

Forks are common in open source, where maintainers don’t want to give write access to everyone.
