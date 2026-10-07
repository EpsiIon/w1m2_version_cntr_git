# Version Control with Git and GitHub

A hands-on introduction to git and GitHub. You build a git repository, learn to read what git is telling you at every step, break things on
purpose so you know how to undo them, and finish by working with a partner through a real
pull request on GitHub.

## Learning Objectives

By the end of this repository, you should be able to:

- Configure git and create a repository, reading `git status`, `git diff` and `git log` as
  you go.
- Move a change from the working directory through the staging area into a commit, and
  locate it at each step.
- Create and merge branches, and read history as a graph.
- Cause a merge conflict, read the markers, and resolve it.
- Undo work safely, and keep secrets and generated files out of a repository.
- Clone from GitHub, push a branch, open a pull request, review a colleague's, and merge.

## Learning Path

The exercises build on each other and share one repository, so work through them in order.

| File / Folder                                                                         | Description                                                                                      |
| ------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------ |
| [**1 - Setup and Your First Repository**](01_setup_and_your_first_repository.md) | Configure git, create a repository, and move a change through the staging area into a commit.    |
| [**2 - Branching and Merging**](02_branching_and_merging.md)                     | What a branch actually is, fast-forward versus merge commits, and reading history as a graph.    |
| [**3 - Merge Conflicts**](03_merge_conflicts.md)                                 | Cause a conflict deliberately, read the markers, resolve it, and abandon a merge.                |
| [**4 - Undoing Things**](04_undoing_things.md)                                   | Restore, unstage, amend and revert, and which of them destroys work.                             |
| [**5 - Ignoring Files**](05_ignoring_files.md)                                   | Keep environments and secrets out of a repository, and why ignoring a tracked file does nothing. |
| [**6 - The GitHub Pair Workflow**](06_github_pair_workflow.md)                   | Clone, push, pull request, review, merge, and resolve a conflict between two people.             |

### Additional Folders and Files

| File / Folder              | Description                                             |
| -------------------------- | ------------------------------------------------------- |
| [**Assets**](assets/) | Diagrams of the history shapes used in the exercises. |

## Setup

You need git and a GitHub account.

> [!NOTE]
> Throughout these steps, text in angle brackets like `<repo-name>` is a
> **placeholder**. Replace it, including the `< >` brackets, with your own
> value. For example, `cd <repo-name>` becomes `cd my-version-control`.

### 1. Check git is installed

```bash
git --version
```

If that fails, install git before going further.

---

### 2. Tell git who you are

Exercise 1 walks through this, but if you have not done it on this machine yet:

```bash
git config --global user.name "Your Name"
git config --global user.email "your@email.com"
git config --global core.editor "code --wait"
```

The last line makes git open VS Code whenever it needs you to write a message, such as for a
merge commit. Close the message tab to hand control back to git.

---

### 3. Check GitHub recognises you

Exercise 6 needs this, and it is worth finding out early rather than at the point you need it:

```bash
ssh -T git@github.com
```

> [!IMPORTANT]
> You should see a greeting with your username. If you are asked for a password, GitHub is
> not recognising your key, and exercise 6 will not work until it does. Go back to the
> [Environment Setup Guide](https://neuefische.github.io/ds-installation/) and redo the SSH
> step for your operating system.

---

### 4. Create the Repository from the Template

Click **Use this template** on GitHub.

When creating the repository:

- Set yourself as the **Owner**
- Choose a repository name
- Disable **Include all branches**
- Click **Create repository**

> [!IMPORTANT]
> Everyone completes this step on their own.

---

### 5. Clone the Repository

Copy the SSH URL from the **Code** button on GitHub, then run:

```bash
git clone <copied-ssh-url>
```

The copied SSH URL will look like `git@github.com:<your-username>/<repo-name>.git`.

---

### 6. Open the Exercises

Move into the project folder and launch VS Code:

```bash
cd <repo-name>
code .
```

> [!WARNING]
> This repository holds the exercise instructions. Create the `recipes` folder for
> exercise 1 **outside** it (for example next to it, with `cd ..` first), so you do not end
> up with a git repository inside another one.

## How to use this repository

Each exercise is a numbered task list. After every step, **run `git status` and read it**,
even when the step does not say to. Learning to read that output is most of what this day is
for.

Every exercise ends with a **Check your understanding** section. Answer those out loud with
the person next to you **before** opening the toggle, then click **Show answer** to check.
Opening it first turns a self-check into reading, which is not the same thing. If you cannot
answer one, that is the part to go back over.

Exercise 6 is done **in pairs** and needs both of you at the same point, so wait for your
partner before starting it.

## References & Further Reading

- [**Pro Git**](https://git-scm.com/book/en/v2): the official book, free online. Chapters 2
  and 3 cover everything in this repository in more depth.
- [**git-katas**](https://github.com/eficode-academy/git-katas): a large set of deliberate practice exercises.
- [**Oh Shit, Git!?!**](https://ohshitgit.com/): short recipes for getting out of common
  messes.
- [**Learn Git Branching**](https://learngitbranching.js.org/): a visual sandbox for seeing
  what branch and merge commands do to a history graph.
- [**GitHub Docs: About pull requests**](https://docs.github.com/en/pull-requests): the reference for the workflow in exercise 6.
