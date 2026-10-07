# 4. Undoing Things

Beginners fear git because they fear breaking something permanently. Almost nothing in git
is permanent once it is committed, and this exercise is where you find that out.

The important question is always: **which of the three places am I undoing?**

## Objectives

At the end of this exercise you should be able to:

- Throw away an edit you have not staged.
- Unstage something without losing the edit.
- Fix the message or contents of the commit you just made.
- Undo an old commit safely, without rewriting history.
- Choose the right undo command for a given situation.

## The task

Keep working in `recipes`.

### Discard an unstaged change

1. Edit `waffles.md` and make a change you do not want.
2. Run `git status`. Git names the command in its own output. Read it.
3. Run `git restore waffles.md`.
4. Open the file. Your change is gone, permanently. **This is the one undo in this exercise
   that cannot be recovered**, because the change was never committed.

### Unstage without losing work

5. Edit `waffles.md` again, this time with a change you want to keep.
6. Stage it, then decide it belongs in a separate commit.
7. Run `git restore --staged waffles.md`.
8. Run `git status`. The file is modified but no longer staged. Your edit survived.
9. Commit it properly.

### Fix the commit you just made

10. Commit a change with a deliberately poor message, such as `stuff`.
11. Run `git log --oneline` and note the hash.
12. Run `git commit --amend` and write a better message.
13. Run `git log --oneline` again. **The hash changed.** The old commit is gone and a new one
    stands in its place.
14. Make another small edit, stage it, and run `git commit --amend --no-edit` to fold it into
    that same commit.

> [!WARNING]
> Amend rewrites history. That is harmless on a commit only you have, and disruptive on one
> you have already pushed and someone else has pulled. The rule of thumb from here on: amend
> freely before pushing, think twice after.

### Undo an old commit safely

15. Run `git log --oneline` and find the commit that added `soup.md` in exercise 2. Pick
    that one: no later commit changed `soup.md`, so undoing it will not conflict.
16. Run `git revert <hash>`. Git creates a **new** commit that undoes that one, and opens
    the editor with a message for it. Accept it: save and close the tab.
17. Run `git log --oneline`. Is the original commit still there?
18. Explain the difference between `revert` and `amend` in one sentence.

## Check your understanding

1. You staged a file by mistake. Which command unstages it without losing the edit?

   <details>
   <summary>Show answer</summary>

   `git restore --staged <file>`. It moves the change back out of the staging area and leaves your edit in the working directory untouched. The similarly named `git restore <file>`, without `--staged`, does something very different and throws the edit away.

   </details>

2. Which of the commands in this exercise destroys work irrecoverably, and why is that one
   different?

   <details>
   <summary>Show answer</summary>

   `git restore <file>`, because the change it discards was never committed and never staged, so git has no copy of it anywhere. Everything else in the exercise operates on committed history, which git keeps: an amended commit is still recoverable through the reflog, and `revert` does not remove anything at all. The rule of thumb is that git protects what you have committed, and nothing else.

   </details>

3. Why is `revert` the safe choice for a commit that other people already have?

   <details>
   <summary>Show answer</summary>

   `revert` adds a new commit that undoes the changes, leaving the original commit in place. The history everyone already has stays valid, so nobody's clone diverges. Rewriting an already-shared commit, with `amend` or a reset, changes hashes that other people are building on, and they then have to reconcile two versions of the same history.

   </details>

4. You amended a commit and the hash changed. Why does a changed hash matter to a colleague?

   <details>
   <summary>Show answer</summary>

   A commit hash identifies the commit, so changing it creates a different commit rather than an edited one. If your colleague already pulled the original, their history and yours now disagree about what happened, and their next pull or push will report a divergence they have to resolve. This is why amending is safe before pushing and disruptive afterwards.

   </details>

## Useful commands

- `git restore <file>` discard unstaged changes to a file
- `git restore --staged <file>` unstage, keeping the change
- `git commit --amend` replace the previous commit, `--no-edit` keeps its message
- `git revert <hash>` add a new commit that undoes an old one
- `git log --oneline` find the hash you need
- `git reflog` list every commit `HEAD` has pointed at recently, including ones replaced by an amend
