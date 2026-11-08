# Git fundimentals

This is a beginner guide on how to work with git and github

This guide assumes that you understand the following

- you can do basic terminal navigation (`cd`, `ls`, `mkdir`)
- You understand how the *command Line* works
- You have install a code editor (*vs code* as an example)

## what problem does Git solve

Imagine working on a project without version control:

- you copy the whole folder multiple time with titles like `my-app-final`, `my-app-final-v2`, `my-app-final-v3-for-real`
- You accidentally overwrite working code with a broken experiment
- You can’t easily see *what* changed or *when*
- Sharing changes with others is done by zipping folders or emailing files

Git solves this issue by being a **Version contol sytem**

- It tracks changes over time
- It lets you move backward and forward through history
- It lets multiple people work on the same code without stepping on each other

### Git vs Github

it is very common to confuse these two:

- **Git**: is a tool that lives on your computer it tracks versions of your project/(file) in a **repository** (or *repo*)
- **Github**: is a website that hosts Git repositories on the cloud. It adds colaberation features like pull request, issues, and code review

you can use **Git** without **Github**(locally only), but you cannot use Github without git

### core vocabulary

You’ll see these terms a lot:

- **Repository (repo)**: the project that Git is tracking. A folder on your machine that has a hidden `.git` folder inside.
- **commit**: a snapshot of your project at a moment in time, with a message.
- **History**: the ordered list of commits that shows how your project evolved.
- **Local vs remote**:
  - **local**: on your computer
  - **Remote**: hosted somwhere else (e.g. github)

Keep these in mind as we build a mental model of how get works

## How Git thinks: the core mental model

When Git confuses people, it’s usually because they don’t have a mental model.

Git thinks in **three main areas**:

1. **Working directory**
2. **Staging area**
3. **Repository (history)**

### Working directory

This is just your project folder:

- open files in your editor
- you can add, delete or modify your code
- nothing here is permanent from git's perspective

### Staging area ("the waiting room")

The staging area is where you tell Git:
> “These are the exact changes I want in my next snapshot.”

you send info to the staging area with the following commands:

```bash
git add file.py # adding a single file by name

git add . # this adds all new or edited files automaticlly
```

these commands take the changes from the working Directory and sends it to the staging area

you can think of the sataging area as:

- A checklist of changes you’re about to commit
- A way to group related edits into a single meaningful snapshot

### Repository (history of commits)

when you run:

```bash
git commit -m "Describe the change"
```

**Git**:

- takes whatever is in the staging area
- saves it as a commit in the Repository
- records
  - exactly which files have changed
  - The content of those changes
  - who made the changes
  - the message that you sent with the commit

From then on, that commit is part of your project’s history.

### Commits are snapshots

It’s useful to imagine each commit as a snapshot of your entire project at a moment in time.

This makes it easy to:

- Go back to how the project looked yesterday
- Compare “before” and “after” for a bug fix
- Understand the story of the codebase over time (and the changes to the code base)

## installing Git

Before we can use Git, we need to make sure it’s installed.

### check if git is already installed

open your terminal and run:

```bash
git --version
```

if you see a response like:

```bash
git version 2.45.0
```

then you  already have git installed on your computer

if you see an error response then you need to install git on to your machine

### Installing Git on macOS

You have two common options:

1. Xcode Command Line Tools (simple)
2. Homebrew (recommended if you already use Homebrew)

Option 1: Xcode Command Line Tools

```bash
xcode-select --install
```

follow the prompts. This installs Git and other developer tools

Option 2: Homebrew

If you use Homebrew:

``` bash
brew install git
```

### Installing Git on Linux

Use your distribution’s package manager:
This is also how you install git with WSL in the WSL terminal

```bash
# Debian/Ubuntu
sudo apt update
sudo apt install git

# Fedora
sudo dnf install git

# Arch Linux
sudo pacman -S git
```

### Install Git on Windows

Install **Git for Windows**:

- Go to [https://gitforwindows.org/](https://gitforwindows.org/)
- Download the installer
- Accept the defaults unless you have a specific reason not to

This gives you:

- Git
- A terminal called **Git Bash**, which is a good shell for beginners

### Basic Git configuration

Once Git is installed, run these commands to tell Git who you are.

Replace the placeholders with your actual name and email:

```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```

You only need to do this **once per machine**.

Git will now attach this identity to your future commits.
