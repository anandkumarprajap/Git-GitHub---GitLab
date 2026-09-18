# Git Branching, Merge, Rebase, Squash & Pull Request

## 1. What is Git?

**Git** is a version-control system used to track changes in source code.

It helps developers:
- Track code changes
- Create branches
- Work with multiple developers
- Go back to previous versions
- Merge different changes
- Collaborate using GitHub

---

# 2. Important Git Terms

| Term | Meaning |
|---|---|
| Repository | Project managed by Git |
| Commit | Saved snapshot of changes |
| Commit ID | Unique hash of a commit |
| Branch | Pointer to a line of development |
| HEAD | Current location in Git history |
| Remote | GitHub repository |
| origin | Default name of remote GitHub repository |
| Merge | Combines two branch histories |
| Rebase | Moves/replays commits onto another base |
| Squash | Combines multiple commits into one |
| Pull Request | Request to merge changes through GitHub |

---

# 3. Basic Git Workflow

```text
Working Directory
       |
       v
   git add
       |
       v
   Staging Area
       |
       v
  git commit
       |
       v
 Local Repository
       |
       v
   git push
       |
       v
     GitHub
```

---

# 4. Clone Repository

Clone a GitHub repository to your local machine.

```bash
git clone git@github.com:USERNAME/REPOSITORY.git
```

Example:

```bash
git clone git@github.com:anandkumarprajap/devboard.git
```

Go inside the repository:

```bash
cd devboard
```

---

# 5. Check Remote Repository

```bash
git remote -v
```

Example output:

```text
origin  git@github.com:anandkumarprajap/devboard.git (fetch)
origin  git@github.com:anandkumarprajap/devboard.git (push)
```

---

# 6. Check Branches

Show local branches:

```bash
git branch
```

Show local + remote branches:

```bash
git branch -a
```

Example:

```text
* master
  devops
  test
  remotes/origin/master
  remotes/origin/devops
  remotes/origin/test
```

`*` means the current branch.

---

# 7. Create a New Branch

```bash
git checkout -b test
```

Modern command:

```bash
git switch -c test
```

This creates the branch and switches to it.

### Important

A new branch is **not an empty folder**.

If you create:

```bash
git checkout -b test
```

while you are on `master`, the new `test` branch starts from the current `master` commit.

```text
master
   |
   v
  C1
   |
   +------> test
```

Therefore, both branches initially contain the same files.

---

# 8. Switch Branch

Old command:

```bash
git checkout master
```

Modern command:

```bash
git switch master
```

Switch to devops:

```bash
git switch devops
```

Switch to test:

```bash
git switch test
```

---

# 9. Commit

A commit is a **saved snapshot of changes**.

Basic workflow:

```bash
git status
git add .
git commit -m "Added new file"
```

Push:

```bash
git push origin test
```

---

# 10. Commit ID

Every commit gets a unique ID/hash.

Example:

```text
029ee6d
814fa3e
12ed3dd
5bfa3b8
4d2721f
ec91bfe
fa680dc
b39af7e
1cd38ea
```

Example:

```bash
git log --oneline
```

Output:

```text
1cd38ea added file3.txt
b39af7e added file2.txt
fa680dc added file1.txt
```

The first value is the **commit ID**.

```text
1cd38ea  ---> Commit ID
added file3.txt ---> Commit message
```

A full commit hash can be much longer, while Git commonly displays a short form.

---

# 11. Check Commit History

Normal:

```bash
git log
```

Short format:

```bash
git log --oneline
```

Best format for branch visualization:

```bash
git log --oneline --graph --decorate --all
```

Example:

```text
* 1cd38ea added file3.txt
* b39af7e added file2.txt
* fa680dc added file1.txt
* bd54875 Added new folder
* ec91bfe Added a new master file
* ...
```

---

# 12. HEAD

`HEAD` means the commit/branch you are currently working on.

Example:

```text
HEAD
 |
 v
1cd38ea
```

`HEAD~1` means one commit before HEAD.

```bash
git show HEAD~1
```

`HEAD~2`:

```bash
git show HEAD~2
```

---

# 13. Git Fetch

Download information about remote branches without changing your current working files.

```bash
git fetch
```

Fetch all remotes:

```bash
git fetch --all
```

### Important

Correct:

```bash
git fetch --all
```

Incorrect:

```bash
git fetch -all
```

`fetch` updates remote-tracking information.

It does **not automatically merge** those changes into your current branch.

---

# 14. Git Pull

`git pull` gets remote changes and integrates them into your current branch.

```bash
git pull origin master
```

Conceptually:

```text
git pull
   =
git fetch
   +
git merge
```

You can also use rebase:

```bash
git pull --rebase origin master
```

---

# 15. Git Push

Upload local commits to GitHub.

```bash
git push origin master
```

For devops:

```bash
git push origin devops
```

For test:

```bash
git push origin test
```

---

# 16. MERGE

## Definition

**Merge combines the history of one branch with another branch.**

Example:

```text
master
   A---B---C
        \
         D---E   devops
```

Merge `devops` into `master`:

```bash
git switch master
git merge devops
```

Result:

```text
A---B---C---------M
     \           /
      D---E-----/
```

`M` is the merge commit when Git creates a merge commit.

---

# 17. Master Merge Devops

Scenario:

```text
master:
A---B---C

devops:
A---B---C---D---E
```

Run:

```bash
git switch master
git merge devops
```

Now the changes from `devops` are included in `master`.

Push:

```bash
git push origin master
```

---

# 18. Devops Merge Master

If `master` has new changes:

```text
master:
A---B---C---M

devops:
A---B---C---D---E
```

Switch to devops:

```bash
git switch devops
```

Merge master:

```bash
git merge master
```

Possible result:

```text
A---B---C---M---------X
     \       \       /
      D---E---\-----/
```

The merge commit `X` combines the histories.

In your practice, you created a merge commit similar to:

```text
230f211 Merge branch 'master' into devops
```

---

# 19. REBASE

## Definition

**Rebase moves/replays your branch commits on top of another branch.**

Example before rebase:

```text
master:
A---B---C

test:
A---B---D---E
```

Run:

```bash
git switch test
git rebase master
```

After rebase:

```text
A---B---C---D'---E'
```

The test commits are replayed on top of the latest master.

---

# 20. Why Commit IDs Change During Rebase

A rebase creates new commits.

Therefore:

```text
Original:

D = old commit ID
E = old commit ID

After rebase:

D' = new commit ID
E' = new commit ID
```

Even if the code changes are the same, the commit IDs can change because the commit's parent/history changes.

### Example from practice

You had a commit like:

```text
33504ad
Added new folder
```

After rebase, it became a new commit ID:

```text
bd54875
Added new folder
```

This is normal behavior.

---

# 21. Rebase Command

```bash
git switch test
git rebase master
```

If conflicts occur:

```bash
git status
```

Fix the files.

Then:

```bash
git add .
git rebase --continue
```

Cancel the rebase:

```bash
git rebase --abort
```

---

# 22. MERGE vs REBASE

### Merge

```text
A---B---C---------M
     \           /
      D---E-----/
```

Preserves branch history.

### Rebase

```text
A---B---C---D'---E'
```

Creates a cleaner linear history.

### Simple difference

```text
MERGE
= Combine histories

REBASE
= Replay commits on a new base
```

---

# 23. SQUASH

## Definition

**Squash combines multiple commits into one commit.**

Suppose `test` has:

```text
fa680dc  added file1.txt
b39af7e  added file2.txt
1cd38ea  added file3.txt
```

Three commits:

```text
fa680dc
   |
b39af7e
   |
1cd38ea
```

After squash:

```text
ONE COMMIT
   |
combined changes from
file1 + file2 + file3
```

---

# 24. Git Squash Merge

From `master`:

```bash
git switch master
```

Make sure master is updated:

```bash
git pull origin master
```

Squash the test branch:

```bash
git merge --squash test
```

Check:

```bash
git status
```

Create one commit:

```bash
git commit -m "Squashed test changes"
```

Push:

```bash
git push origin master
```

### Important

This command:

```bash
git merge --squash test
```

prepares/stages the combined changes.

It does **not automatically create the final commit**.

You normally follow it with:

```bash
git commit -m "..."
```

---

# 25. Your Test Branch Example

Your test branch contained:

```text
bd54875  Added new folder
     |
fa680dc  added file1.txt
     |
b39af7e  added file2.txt
     |
1cd38ea  added file3.txt
```

The three file commits were:

```text
fa680dc
b39af7e
1cd38ea
```

They represent:

```text
file1.txt
file2.txt
file3.txt
```

When squash merged, the changes could be represented by one commit on the target branch.

In your GitHub workflow, you had a PR merge commit such as:

```text
ca4030b
Merge pull request #2 from anandkumarprajap/test
```

---

# 26. MERGE vs SQUASH

Normal merge:

```text
master
A---B---------M
     \       /
      C---D
```

The individual commits remain visible.

Squash:

```text
master
A---B---S
```

Where `S` contains the combined changes from:

```text
C + D
```

### Simple difference

```text
MERGE
= Keep individual commits

SQUASH
= Combine multiple commits into one
```

---

# 27. PULL REQUEST

## Definition

A **Pull Request (PR)** is a request to merge changes from one branch into another branch on GitHub.

Example:

```text
test
 |
 | Pull Request
 v
master
```

A developer can:

1. Create a branch
2. Make changes
3. Commit changes
4. Push branch to GitHub
5. Open Pull Request
6. Review changes
7. Approve
8. Merge

---

# 28. Pull Request Workflow

```text
Developer
    |
    v
Create branch
    |
    v
Make changes
    |
    v
git add .
    |
    v
git commit
    |
    v
git push
    |
    v
GitHub
    |
    v
Pull Request
    |
    v
Code Review
    |
    v
Merge / Squash Merge
    |
    v
master
```

---

# 29. Create a Pull Request

Create branch:

```bash
git switch -c test
```

Make changes:

```bash
vim file1.txt
```

Add:

```bash
git add .
```

Commit:

```bash
git commit -m "added file1.txt"
```

Push:

```bash
git push -u origin test
```

Then open GitHub and create:

```text
test  --->  master
```

---

# 30. GitHub Pull Request Options

GitHub commonly provides merge methods such as:

### Create a merge commit

```text
A---B---------M
     \       /
      C---D
```

### Squash and merge

```text
A---B---S
```

Multiple PR commits become one commit on the target branch.

### Rebase and merge

```text
A---B---C---D
```

The commits are applied linearly without creating a normal merge commit.

---

# 31. Three Branch Workflow

Your practice used:

```text
master
devops
test
```

Conceptually:

```text
                    ┌──────────────┐
                    │    master    │
                    └──────┬───────┘
                           │
             ┌─────────────┴─────────────┐
             │                           │
             v                           v
        ┌─────────┐                 ┌─────────┐
        │ devops  │                 │  test   │
        └─────────┘                 └─────────┘
```

---

# 32. Example Three-Branch History

### Master

```text
029ee6d  Initial commit
   |
814fa3e  added nginx script
   |
12ed3dd  Delete hello.html
   |
5bfa3b8  This is a test file
   |
ec91bfe  Added a new master file
```

### Devops

```text
029ee6d  Initial commit
   |
814fa3e  added nginx script
   |
12ed3dd  Delete hello.html
   |
5bfa3b8  This is a test file
   |
4d2721f  Added changes to test
   |
230f211  Merge branch 'master' into devops
```

### Test

```text
bd54875  Added new folder
   |
fa680dc  added file1.txt
   |
b39af7e  added file2.txt
   |
1cd38ea  added file3.txt
```

> Commit IDs above are the short IDs from your practice history; Git may show different IDs after operations such as rebase.

---

# 33. Complete Concept Diagram

```text
                         MASTER
                           |
                           v
                  029ee6d Initial
                           |
                  814fa3e nginx
                           |
                  12ed3dd delete hello
                           |
                  5bfa3b8 test file
                           |
                  ec91bfe master file
                           |
                           |
              ┌────────────┴─────────────┐
              |                          |
              v                          v

           DEVOPS                      TEST
              |                          |
         4d2721f                    bd54875
      changes to test              new folder
              |                          |
         230f211                    fa680dc
       merge master                 file1.txt
                                         |
                                    b39af7e
                                    file2.txt
                                         |
                                    1cd38ea
                                    file3.txt
```

---

# 34. Four Important Git Operations

## MERGE

```text
Two histories
     ↓
Combine
     ↓
Possible merge commit
```

Command:

```bash
git merge branch-name
```

---

## REBASE

```text
Old branch
     ↓
Move/replay commits
     ↓
New base
```

Command:

```bash
git rebase master
```

---

## SQUASH

```text
Commit 1
Commit 2
Commit 3
   ↓
 ONE COMMIT
```

Command:

```bash
git merge --squash test
git commit -m "Combined test changes"
```

---

## PULL REQUEST

```text
Branch
   ↓
Push to GitHub
   ↓
Pull Request
   ↓
Review
   ↓
Merge / Squash
```

---

# 35. Useful Commands Cheat Sheet

```bash
# Check status
git status

# Check branches
git branch

# Check all branches
git branch -a

# Create branch
git checkout -b test

# Switch branch
git checkout master

# Modern switch
git switch master

# Add changes
git add .

# Commit
git commit -m "message"

# View commits
git log

# Short commit history
git log --oneline

# Graph history
git log --oneline --graph --decorate --all

# View remote
git remote -v

# Download remote information
git fetch

# Download all remote information
git fetch --all

# Pull
git pull origin master

# Pull using rebase
git pull --rebase origin master

# Push
git push origin master

# Merge
git merge devops

# Rebase
git rebase master

# Abort rebase
git rebase --abort

# Squash
git merge --squash test

# Create squash commit
git commit -m "Squashed test changes"
```

---

# 36. Easy Way to Remember

```text
COMMIT
  ↓
Save a snapshot

BRANCH
  ↓
Separate line of development

MERGE
  ↓
Combine branches

REBASE
  ↓
Move/replay commits onto another base

SQUASH
  ↓
Combine multiple commits into one

PULL REQUEST
  ↓
Ask GitHub/team to review and merge changes
```

---

# 37. Recommended Visualization Command

Whenever you are confused about branches and commits, run:

```bash
git log --oneline --graph --decorate --all
```

This is one of the most useful commands for understanding:

```text
Branch
Commit ID
HEAD
Merge
Rebase
Divergence
```

Example:

```text
*   ca4030b Merge pull request
|\
| * 1cd38ea added file3.txt
| * b39af7e added file2.txt
| * fa680dc added file1.txt
|/
* ec91bfe Added a new master file
|\
| * 4d2721f Added changes to test
|/
* 5bfa3b8 This is a test file
```

---

# 38. Final Summary

```text
Git
 |
 +-- Commit
 |     └── Unique Commit ID
 |
 +-- Branch
 |     ├── master
 |     ├── devops
 |     └── test
 |
 +-- Merge
 |     └── Combine histories
 |
 +-- Rebase
 |     └── Replay commits on new base
 |
 +-- Squash
 |     └── Multiple commits → One commit
 |
 +-- Pull Request
       └── GitHub review → Merge
```

### Golden Rule

```text
MERGE  = Combine
REBASE = Replay on new base
SQUASH = Combine commits
PR     = Request to merge
COMMIT = Save changes
BRANCH = Separate development line
```




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
![Image 12](12.png)
![Image 13](13.png)
![Image 14](14.png)
![Image 15](15.png)
![Image 16](16.png)
![Image 17](17.png)
![Image 18](18.png)
![Image 19](19.png)
![Image 20](20.png)
![Image 21](21.png)
![Image 22](22.png)
![Image 23](23.png)
![Image 24](24.png)
![Image 25](25.png)
![Image 26](26.png)
![Image 27](27.png)
![Image 28](28.png)
![Image 29](29.png)
![Image 30](30.png)
![Image 31](31.png)
![Image 32](32.png)
![Image 33](33.png)
![Image 34](34.png)


