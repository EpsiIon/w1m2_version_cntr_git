# 5. Ignoring Files

Not everything in a project folder belongs in the repository. Virtual environments, data
files, editor settings and anything holding a password all stay out.

This is short, and it matters more than its length suggests: the most common way people leak
a credential is committing it by accident.

## Objectives

At the end of this exercise you should be able to:

- Keep files out of a repository with `.gitignore`.
- Explain why `.gitignore` itself is committed rather than ignored.
- Untrack a file that `.gitignore` cannot reach because it is already committed.

## The task

Keep working in `recipes`.

1. Create a folder `.venv` with a file in it, and a file `secrets.env` containing
   `API_KEY=do-not-commit-me`.
2. Run `git status`. Both appear as untracked. Committing everything now would be a mistake.
3. Create a file called `.gitignore` and add:

```
.venv/
*.env
```

4. Run `git status`. The two are gone from the listing. What is listed instead?
5. Commit `.gitignore`. **This file belongs in the repository**, because everyone working on
   the project should ignore the same things.

### The trap

6. Create a file `notes.txt`, then stage and commit it.
7. Now add `notes.txt` to `.gitignore` and commit that.
8. Edit `notes.txt` and run `git status`. **Git still tracks it.** Why?
9. Stop tracking it while keeping it on disk: `git rm --cached notes.txt`, then commit.
10. Edit it again and run `git status`. Now it is ignored.

> [!CAUTION]
> `.gitignore` decides what git will **start** tracking. It has no effect on what it already
> tracks. If a secret is already committed, ignoring it does not remove it from the history,
> and the credential must be treated as compromised and rotated.

## Check your understanding

1. Why is `.gitignore` committed rather than ignored?

   <details>
   <summary>Show answer</summary>

   Because everyone working on the project should ignore the same things. If `.gitignore` were ignored, each person would have to recreate it, and sooner or later somebody would commit their virtual environment or a credentials file. Committing it makes the rule part of the project rather than part of one person's setup.

   </details>

2. You added a data file to `.gitignore` but it still shows up in `git status`. What happened?

   <details>
   <summary>Show answer</summary>

   The file was already tracked before you added it to `.gitignore`. `.gitignore` only controls what git will **start** tracking, so it has no effect on files already in the repository. Stop tracking it with `git rm --cached <file>`, which keeps the file on disk, then commit that change.

   </details>

3. A password was committed last week and you have now added the file to `.gitignore`. Is the
   password safe?

   <details>
   <summary>Show answer</summary>

   No. The password is in the repository's history, and history is exactly what git is designed to preserve and distribute. Anyone with a clone, including one made before you changed anything, still has it. Treat the credential as compromised and rotate it. Removing it from history afterwards is possible but does not undo the exposure.

   </details>

4. Name three things that should usually be ignored in a Python project.

   <details>
   <summary>Show answer</summary>

   Three common ones:

   - The virtual environment, `.venv/`, which is large and machine-specific.
   - Anything holding secrets, such as `.env`.
   - Generated files, such as `__pycache__/` and `.ipynb_checkpoints/`.

   Data files are often ignored too, depending on size and whether they can be downloaded again.

   </details>

## Useful commands

- `.gitignore` one pattern per line, `#` starts a comment
- `folder/` ignore a directory, `*.ext` ignore by extension
- `git rm --cached <file>` stop tracking a file, keep it on disk
- `git status --ignored` show what is being ignored
