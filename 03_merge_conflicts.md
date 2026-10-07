# 3. Merge Conflicts

A conflict is not a failure. It is git saying: two people changed the same lines, and I am
not willing to guess which one you meant.

Most people meet their first conflict under pressure, on someone else's repository. You are
going to meet yours deliberately, on your own, with nothing at stake.

## Objectives

At the end of this exercise you should be able to:

- Cause a merge conflict on purpose.
- Read the conflict markers git writes into the file.
- Resolve a conflict and complete the merge.
- Abandon a merge and get back to where you started.

## The task, part 1: cause one

Keep working in `recipes`.

1. Make sure you are on `main` and everything is committed. `git status` should be quiet.
2. Create a branch `metric-units` and switch to it.
3. In `pancakes.md`, change the ingredient quantities to metric, and commit.
4. Switch back to `main`.
5. In `pancakes.md`, change the **same lines** to imperial quantities, and commit.
6. Run `git log --oneline --graph --all`. Two branches have edited the same lines.
7. Merge: `git merge metric-units`. Read the error carefully. Which file is in conflict?
8. Run `git status`. Git tells you exactly what state you are in and what it expects next.

## The task, part 2: read it

9. Open `pancakes.md`. You will see something like this:

```
<<<<<<< HEAD
- 1 cup flour
=======
- 125 g flour
>>>>>>> metric-units
```

10. Work out which side is which. `HEAD` is the branch you are **on**. The name at the bottom
    is the branch you are **merging in**.
11. Nothing here is magic: those markers are plain text that git wrote into your file. Your
    job is to edit the file until it says what you want, markers removed.

## The task, part 3: resolve it

12. Edit the file so it holds the version you want. You may keep one side, the other, or
    write something new. **Delete all three marker lines.**
13. Run `git status`. Git still considers the file unresolved.
14. Stage the file. That is how you tell git the conflict is settled.
15. Run `git status` again, then commit. Git will offer a merge commit message; accept it.
16. Run `git log --oneline --graph --all`. Find the merge commit and its two parents.

## The task, part 4: escape one

Sometimes you start a merge and realise it is the wrong moment.

17. Create another conflict the same way as before: from `main`, create a branch
    `metric-sugar`, change one ingredient line in `pancakes.md` and commit. Switch back to `main`,
    change the same line differently and commit. Then run `git merge metric-sugar`.
18. When the conflict appears, run `git merge --abort`.
19. Run `git status` and open the file. Confirm you are exactly back where you started.

## Check your understanding

1. In the markers above, which quantities came from the branch you were standing on?

   <details>
   <summary>Show answer</summary>

   The ones above `=======`, under `<<<<<<< HEAD`. `HEAD` always means the branch you are currently on, which in that example was `main` holding the imperial quantities. The name at the bottom, after `>>>>>>>`, is the branch being merged in.

   </details>

2. Why does git ask you to `git add` a file you have just fixed by hand?

   <details>
   <summary>Show answer</summary>

   Staging is how you tell git the conflict is settled. Git cannot check whether your edit is correct, so it waits for you to say so explicitly, and `git add` is that signal. Until you stage the file, `git status` keeps listing it under "Unmerged paths" and will not let you complete the merge.

   </details>

3. You resolved a conflict but left a `=======` line in the file. Would git stop you
   committing? What would be wrong with the result?

   <details>
   <summary>Show answer</summary>

   Git would let you commit it. Git does not parse your file or check for leftover markers, it only tracks whether you staged the file. The result would be a committed file containing `=======` and both versions of the text, which usually breaks code and always looks careless in a pull request. Read the file after resolving, before you stage it.

   </details>

4. When is `git merge --abort` the right choice, and when is it not available?

   <details>
   <summary>Show answer</summary>

   `git merge --abort` is right when you have started a merge, hit conflicts, and would rather deal with it later or from a different starting point. It returns the working directory to exactly its pre-merge state. It is only available while the merge is in progress: once you have committed the merge, undo it with `git revert -m 1 <merge-hash>` instead. A merge commit has two parents, and `-m 1` tells git to keep the first one, the branch you merged into, as the version to go back to. A plain `git revert` on a merge commit fails because git cannot tell which parent you mean.

   </details>

## Useful commands

- `git merge <branch>` start a merge
- `git status` during a conflict, it lists unmerged paths and what to do next
- `git add <file>` mark a conflict resolved
- `git merge --abort` undo the merge attempt and return to the previous state
- `git log --oneline --graph --all` see both parents of a merge commit
