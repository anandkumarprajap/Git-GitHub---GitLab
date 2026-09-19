![Image 1](1.png)
![Image 2](2.png)
![Image 3](3.png)
![Image 4](4.png)
![Image 5](5.png)
![Image 6](6.png)
![Image 7](7.png)
![Image 8](8.png)
![Image 9](9.png)
![Image 10](10.png)
![Image 11](11.png)

# Git Undoing, Revert & Blame

These commands are used to inspect changes and undo or move Git history.

---

# 1. `git blame`

## Definition

`git blame` shows **who last changed each line of a file**.

It displays:

* Commit ID
* Author
* Date
* Line number
* Line content

## Why Use?

Use `git blame` when you want to:

* Find who changed a particular line.
* Find which commit introduced a change.
* Investigate or debug a change.

## Command

```bash
git blame <file-name>
```

## Example

```bash
git blame keys.txt
```

Example output:

```text
c9c9faa (Ubuntu 2026-07-12 ...) 1 AWS_SECRET_ACCESS_KEY = ...
```

## Result

Git tells you that commit `c9c9faa` was responsible for the current version of that line.

---

# 2. `git revert`

## Definition

`git revert` **undoes the changes made by a specific commit by creating a new commit**.

The original commit remains in Git history.

## Why Use?

Use `git revert` when:

* A commit has already been pushed to GitHub.
* You want to safely undo a commit.
* You do not want to rewrite existing Git history.

## Command

```bash
git revert <commit-id>
```

## Example

```bash
git revert c9c9faa
```

If `c9c9faa` added `keys.txt`, Git creates a new commit that removes it.

Example:

```text
c9c9faa  added keys
     ↓
1f7fd44  Revert "added keys"
```

## Result

The original commit stays in history, but its changes are reversed.

```bash
git log --oneline
```

Example:

```text
1f7fd44 Revert "added keys"
c9c9faa added keys
```

### Important

**Revert does NOT delete the original commit.**

---

# 3. `git reset`

## Definition

`git reset` moves the current branch's `HEAD` to another commit.

Depending on the option, it can also:

* Keep changes staged.
* Unstage changes.
* Delete local changes.

## Why Use?

Use `git reset` when:

* You want to move the branch back to an earlier commit.
* You want to combine or remove local commits.
* You want to change the staging state.
* You are working on local/unpublished history.

## Basic Command

```bash
git reset <commit-id>
```

## Example

```bash
git reset 230f211
```

This moves `HEAD` to commit `230f211`.

---

# 4. `git reset 230f211` — Mixed Reset

## Definition

`git reset <commit>` without an option performs a **mixed reset** by default.

It moves `HEAD` and resets the staging area, but keeps the file changes in the working directory.

## Why Use?

Use it when:

* You want to remove commits.
* You want to keep the changes.
* You want the changes to become unstaged so you can modify or select what to commit.

## Command

```bash
git reset 230f211
```

Same as:

```bash
git reset 230f211 --mixed
```

## Example

Before:

```text
A → B → C → D
          ↑
         HEAD
```

Run:

```bash
git reset 230f211
```

After:

```text
A → B
    ↑
   HEAD
```

The changes from the later commits remain in your working directory.

## Result

```bash
git status
```

May show:

```text
Changes not staged for commit:
    modified: test.py
```

---

# 5. `git reset 230f211 --soft`

## Definition

A soft reset moves `HEAD` to the specified commit but **keeps the changes staged**.

## Why Use?

Use it when:

* You want to remove one or more commits.
* You want to combine commits.
* You want to create a new commit from the existing changes.
* You want to edit the commit history while keeping changes ready to commit.

## Command

```bash
git reset 230f211 --soft
```

## Example

Before:

```text
A → B → C → D
          ↑
         HEAD
```

Run:

```bash
git reset 230f211 --soft
```

After:

```text
A → B
    ↑
   HEAD
```

The changes from `C` and `D` remain **staged**.

## Result

```bash
git status
```

Example:

```text
Changes to be committed:
    modified: test.py
```

You can then create a new commit:

```bash
git commit -m "Combined changes"
```

---

# 6. `git reset 230f211 --mixed`

## Definition

A mixed reset moves `HEAD` and resets the staging area, while keeping the changes in the working directory.

It is the **default reset mode**.

## Why Use?

Use it when:

* You want to remove commits.
* You want to keep the file changes.
* You want to review or modify the changes before committing again.

## Command

```bash
git reset 230f211 --mixed
```

Or simply:

```bash
git reset 230f211
```

## Example

Before:

```text
A → B → C
        ↑
       HEAD
```

Run:

```bash
git reset 230f211 --mixed
```

After:

```text
A → B
    ↑
   HEAD
```

The changes from `C` are kept in the working directory but are **unstaged**.

## Result

```bash
git status
```

Example:

```text
Changes not staged for commit:
    modified: test.py
```

---

# 7. `git reset 230f211 --hard`

## Definition

A hard reset moves `HEAD`, resets the staging area, and resets the working directory to the selected commit.

**Local changes after that commit are discarded.**

## Why Use?

Use it when:

* You are sure you do not need the changes.
* You want your working directory to exactly match an older commit.
* You want to completely remove local changes.

## Command

```bash
git reset 230f211 --hard
```

## Example

Before:

```text
A → B → C → D
          ↑
         HEAD
```

Run:

```bash
git reset 230f211 --hard
```

After:

```text
A → B
    ↑
   HEAD
```

The files are also restored to the state of `230f211`.

## Result

```bash
git status
```

Output:

```text
On branch tod-fod
nothing to commit, working tree clean
```

### ⚠️ Important

`--hard` can discard uncommitted work.

Always check:

```bash
git status
```

before using it.

---

# 8. Difference Between Revert and Reset

| Command             | What happens to history?  | Changes                 | Best use                         |
| ------------------- | ------------------------- | ----------------------- | -------------------------------- |
| `git revert`        | Creates a new undo commit | Reversed                | Shared/pushed commits            |
| `git reset --soft`  | Moves HEAD                | Changes remain staged   | Rework commits                   |
| `git reset --mixed` | Moves HEAD                | Changes become unstaged | Rework files                     |
| `git reset --hard`  | Moves HEAD                | Changes discarded       | Completely discard local changes |

---

# 9. Soft vs Mixed vs Hard

The easiest way to remember:

```text
SOFT
HEAD moves
    ↓
Changes stay STAGED
```

```text
MIXED
HEAD moves
    ↓
Changes become UNSTAGED
```

```text
HARD
HEAD moves
    ↓
Changes are DISCARDED
```

---

# 10. Practical Example

Suppose your history is:

```text
029ee6d → 814fa3e → 12ed3dd → 5bfa3b8 → 230f211 → d66098a
                                                   ↑
                                                  HEAD
```

You want to go back to:

```text
230f211
```

### Keep changes staged

```bash
git reset 230f211 --soft
```

Result:

```text
HEAD → 230f211
Changes → STAGED
```

### Keep changes but unstage them

```bash
git reset 230f211 --mixed
```

Result:

```text
HEAD → 230f211
Changes → UNSTAGED
```

### Delete the changes

```bash
git reset 230f211 --hard
```

Result:

```text
HEAD → 230f211
Changes → DELETED
```

---

# 11. Important Commands

### Check current branch

```bash
git branch
```

### Check status

```bash
git status
```

### View commit history

```bash
git log --oneline
```

### See who changed each line

```bash
git blame keys.txt
```

### Safely undo a commit

```bash
git revert c9c9faa
```

### Mixed reset

```bash
git reset 230f211
```

### Soft reset

```bash
git reset 230f211 --soft
```

### Mixed reset explicitly

```bash
git reset 230f211 --mixed
```

### Hard reset

```bash
git reset 230f211 --hard
```

---

# 12. One-Line Interview Definitions

### `git blame`

> Shows who last modified each line of a file.

### `git revert`

> Creates a new commit that reverses the changes of an existing commit.

### `git reset`

> Moves the current branch HEAD to another commit.

### `git reset --soft`

> Moves HEAD while keeping changes staged.

### `git reset --mixed`

> Moves HEAD while keeping changes unstaged.

### `git reset --hard`

> Moves HEAD and discards changes in the staging area and working directory.

---

# 13. Easy Memory Trick

```text
BLAME  → WHO changed the line?

REVERT → UNDO using a NEW commit.

RESET  → MOVE HEAD.

SOFT   → Keep STAGED.

MIXED  → Keep but UNSTAGED.

HARD   → DELETE local changes.
```

## Golden Rule

```text
Already pushed/shared commit?
        ↓
   Prefer REVERT

Local commit/history?
        ↓
     RESET can be used
```

> ⚠️ Be especially careful with `git reset --hard` and `git push --force`, because they can remove or rewrite work that other people may depend on.

