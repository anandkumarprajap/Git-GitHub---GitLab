# Git & GitHub Advanced Branching — Beginner Notes

## 1. What I Practiced

In this lab, I practiced:

- Clone a GitHub repository using SSH
- `git fetch --all`
- `git checkout`
- `git branch`
- `git pull`
- `git remote -v`
- `git log`
- `git log --oneline`
- Merge branches
- Rebase branches
- Squash commits
- Push branches to GitHub
- Create Pull Requests
- Merge a Pull Request
- Sync local branches with GitHub
- Understand `master`, `devops`, and `test` branches

---

# 2. Basic Git Architecture

```text
                  GitHub Remote Repository
                  ┌──────────────────────┐
                  │       origin         │
                  │                      │
                  │  master              │
                  │  devops              │
                  │  test                │
                  └──────────┬───────────┘
                             │
                    git clone / fetch
                             │
                             ▼
                  ┌──────────────────────┐
                  │    Local Repository  │
                  │                      │
                  │  master              │
                  │  devops              │
                  │  test                │
                  └──────────┬───────────┘
                             │
                         checkout
                             │
                             ▼
                     Working Directory
                     ┌────────────────┐
                     │ Files           │
                     │ README.md       │
                     │ test.py         │
                     │ etc.            │
                     └────────────────┘
```

### Important

`origin` is normally the name of the GitHub remote repository.

Check it:

```bash
git remote -v
```

Example:

```text
origin  git@github.com:USERNAME/devboard.git (fetch)
origin  git@github.com:USERNAME/devboard.git (push)
```

---

# 3. Clone Repository

Clone a repository from GitHub:

```bash
git clone git@github.com:USERNAME/devboard.git
```

Example:

```bash
git clone git@github.com:anandkumarprajap/devboard.git
```

Then:

```bash
cd devboard
```

Check files:

```bash
ls
```

---

# 4. GitHub SSH Authentication

When cloning for the first time, you may see:

```text
The authenticity of host 'github.com' can't be established.
Are you sure you want to continue connecting?
```

If you have verified the GitHub host fingerprint, entering:

```text
yes
```

adds GitHub to:

```text
~/.ssh/known_hosts
```

After that, Git can use your SSH key for GitHub authentication.

---

# 5. Check Current Branch

```bash
git branch
```

Example:

```text
* master
  devops
  test
```

The `*` means the currently active branch.

---

# 6. Create a New Branch

Create and switch to a branch:

```bash
git checkout -b test
```

Modern Git alternative:

```bash
git switch -c test
```

Check:

```bash
git branch
```

Output:

```text
  devops
  master
* test
```

---

# 7. Switch Between Branches

Switch to `master`:

```bash
git checkout master
```

Switch to `devops`:

```bash
git checkout devops
```

Switch to `test`:

```bash
git checkout test
```

Modern command:

```bash
git switch master
```

### Important

Do NOT use:

```bash
cd checkout devops
```

`cd` is for changing directories.

`git checkout` is for changing Git branches/files.

Correct:

```bash
git checkout devops
```

---

# 8. Understand Branches

Imagine the repository starts like this:

```text
A
│
B
│
C
├──────── master
│
└──────── devops
```

The branches point to different commits.

After adding commits:

```text
A
│
B
│
C
├──────── master
│
D
│
E
└──────── devops
```

Now `devops` contains commits that `master` does not have.

---

# 9. git fetch

Command:

```bash
git fetch
```

or:

```bash
git fetch --all
```

### What does fetch do?

It downloads information about new commits and branches from the remote repository.

```text
GitHub
   │
   │ git fetch
   ▼
Local Repository
   │
   └── Updates origin/* references
```

### Important

`git fetch` does **not normally change your current working files**.

Example:

```bash
git fetch --all
```

Then check:

```bash
git branch -a
```

---

# 10. git pull

Command:

```bash
git pull origin devops
```

This basically means:

```text
git fetch
      +
git merge
```

Diagram:

```text
GitHub
   │
   │ pull
   ▼
fetch changes
   │
   ▼
merge changes
   │
   ▼
Local branch updated
```

Example:

```bash
git checkout devops
git pull origin devops
```

If nothing changed:

```text
Already up to date.
```

---

# 11. fetch vs pull

| Command | Purpose |
|---|---|
| `git fetch` | Download remote changes |
| `git pull` | Fetch + integrate changes |
| `git push` | Upload local commits |
| `git clone` | Download repository for the first time |

Simple memory trick:

```text
FETCH = See remote changes

PULL = Bring remote changes into current branch

PUSH = Send local commits to GitHub

CLONE = Get repository initially
```

---

# 12. Check Repository Status

```bash
git status
```

Example:

```text
On branch devops

Your branch is up to date with 'origin/devops'.

nothing to commit, working tree clean
```

This tells you:

- Current branch
- Whether local branch is ahead/behind
- Modified files
- Staged files
- Untracked files

---

# 13. Check Remote Repository

```bash
git remote -v
```

Example:

```text
origin  git@github.com:anandkumarprajap/devboard.git (fetch)
origin  git@github.com:anandkumarprajap/devboard.git (push)
```

`origin` = remote repository name.

---

# 14. Git Log

Full log:

```bash
git log
```

Short log:

```bash
git log --oneline
```

Example:

```text
ca4030b Merge pull request
1cd38ea added file3.txt
b39af7e added file2.txt
fa680dc added file1.txt
ec91bfe Added new master file
70885a6 Merge pull request
5bfa3b8 This is a test file
```

### Commit structure

```text
ca4030b
   │
   └── Short commit ID

Merge pull request
   │
   └── Commit message
```

---

# 15. Merge

Merge means:

> Take changes from one branch and integrate them into another branch.

Example:

```bash
git checkout master
git merge devops
```

Diagram:

Before:

```text
A──B──C──────── master
       \
        D──E──F── devops
```

After:

```text
A──B──C──D──E──F── master
             │
             └── devops
```

If Git can simply move the branch pointer, it is called a:

```text
Fast-forward merge
```

---

# 16. Merge devops into master

Correct workflow:

```bash
git checkout master
git pull origin master
git merge devops
```

Then:

```bash
git push origin master
```

Diagram:

```text
             devops
                │
                ▼
A──B──C──D──E──F
                │
                ▼
              master
```

---

# 17. Merge Master into DevOps

Sometimes `master` has new changes and you want those changes in `devops`.

```bash
git checkout devops
git merge master
```

Diagram:

```text
master
   │
   ├──── New commit
   │
   ▼
A──B──C──M
     \
      D──E──F── devops

After merge:

A──B──C──M────────┐
     \             │
      D──E──F──────┴── Merge commit
```

This is useful when keeping a feature/development branch synchronized with the main branch.

---

# 18. Rebase

Rebase moves your branch commits on top of another branch.

Example:

```bash
git checkout test
git rebase master
```

Before:

```text
A──B──C──────── master
     \
      D──E────── test
```

After:

```text
A──B──C──D'──E'── test
             │
             └── master
```

The commits `D` and `E` are recreated as `D'` and `E'`.

### Simple meaning

```text
Rebase = Put my branch commits on top of the latest target branch.
```

---

# 19. Merge vs Rebase

### Merge

```text
A──B──C──────M── master
     \       /
      D──E──
```

Merge keeps the branch history and may create a merge commit.

### Rebase

```text
A──B──C──D'──E'── test
```

Rebase creates a more linear history.

| Merge | Rebase |
|---|---|
| Preserves branch history | Rewrites branch history |
| Can create merge commit | Usually creates linear history |
| Safer for shared branches | Be careful with shared branches |
| Easy to understand | Useful for clean history |

### Important rule

Avoid rebasing a branch that other people are already using unless your team has agreed on it.

---

# 20. Squash

Squashing combines multiple commits into one logical commit.

Suppose `test` has:

```text
A──B──C──D──E
       │
       ├── added file1
       ├── added file2
       └── added file3
```

Instead of keeping three separate commits:

```text
A──B──C──D──E
```

Squash can produce:

```text
A──B──C──S
```

Where:

```text
S = Added required files
```

---

# 21. Squash Merge

Example:

```bash
git checkout master
git merge --squash test
```

This prepares the changes from `test` without creating a normal merge commit.

Then commit:

```bash
git commit -m "Add test files"
```

Then:

```bash
git push origin master
```

### Important

Correct syntax:

```bash
git merge --squash test
```

Not:

```bash
git merge -squash test
```

---

# 22. What Does `git merge --squash` Do?

Suppose:

```text
master:

A──B

test:

A──B──C──D──E
```

Run:

```bash
git checkout master
git merge --squash test
```

Git prepares the combined changes:

```text
A──B──────── master
     \
      C──D──E── test
```

The files from `C + D + E` are placed into the working tree/staging area.

Then:

```bash
git commit -m "Add test changes"
```

Result:

```text
A──B──S──── master
     \
      C──D──E── test
```

`S` contains the combined changes.

---

# 23. Pull Request Workflow

A common professional workflow is:

```text
Developer
   │
   ▼
Create feature branch
   │
   ▼
Make changes
   │
   ▼
git add
   │
   ▼
git commit
   │
   ▼
git push
   │
   ▼
GitHub
   │
   ▼
Pull Request
   │
   ▼
Code Review
   │
   ▼
Approve
   │
   ▼
Merge into master
```

---

# 24. Create a Pull Request from CLI

Suppose you are on:

```text
test
```

Push it:

```bash
git push -u origin test
```

GitHub may provide a Pull Request URL.

Then open GitHub and create:

```text
test → master
```

Pull Request.

---

# 25. Pull Request Diagram

```text
             GitHub
        ┌────────────────┐
        │                │
        │    master      │
        │       ▲        │
        │       │        │
        │       │ Merge  │
        │       │        │
        │    Pull Request│
        │       ▲        │
        │       │        │
        │      test      │
        │                │
        └────────────────┘
```

The Pull Request is a request:

> "Please review my changes and merge them into the target branch."

---

# 26. Pull Request Between Two Branches

Example:

```text
Source branch:
test

Target branch:
master
```

PR:

```text
test  ───────────────►  master
       Pull Request
```

After merge:

```text
master
   ▲
   │
   └── test changes
```

---

# 27. Pull Request Between Different Accounts

This is common in open-source projects.

Example:

```text
Developer Account
       │
       │ Fork
       ▼
Developer's Repository
       │
       │ changes
       ▼
feature branch
       │
       │ Pull Request
       ▼
Original Repository
       │
       ▼
    maintainer
       │
       ▼
Review + Merge
```

Example:

```text
Account A
┌──────────────────┐
│ original repo    │
│ master           │
└────────▲─────────┘
         │
      Pull Request
         │
         │
┌────────┴─────────┐
│ Account B        │
│ fork             │
│ devops/test      │
└──────────────────┘
```

The owner/maintainer of the original repository reviews the Pull Request and decides whether to merge it.

---

# 28. GitHub Pull Request Merge Options

GitHub commonly provides options such as:

### Create a merge commit

```text
Feature commits
       \
        Merge Commit
             │
             ▼
           master
```

### Squash and merge

```text
C1
C2
C3
 │
 ▼
ONE COMMIT
 │
 ▼
master
```

### Rebase and merge

```text
C1──C2──C3
 │
 ▼
master history
```

The exact available options can depend on repository settings.

---

# 29. Important Difference: Merge vs Pull Request

These are related but not the same.

### Git Merge

A local Git operation:

```bash
git merge devops
```

### Pull Request

A GitHub collaboration workflow:

```text
Branch → Pull Request → Review → Merge
```

A Pull Request can eventually result in a merge, squash merge, or rebase merge depending on the repository settings and chosen method.

---

# 30. Your Lab Branch Structure

Your practice effectively had:

```text
                         GitHub
                           │
             ┌─────────────┼─────────────┐
             │             │             │
          master         devops         test
             │             │             │
             ▼             ▼             ▼
          Main code     DevOps work    Test work
```

Local:

```text
~/github-advanced/devboard
             │
             ├── master
             ├── devops
             └── test
```

Remote:

```text
origin/master
origin/devops
origin/test
```

---

# 31. Local Branch vs Remote Branch

This is very important.

```text
Local:

master
devops
test


Remote-tracking:

origin/master
origin/devops
origin/test
```

Example:

```text
master
   │
   └──────── origin/master
```

They can temporarily point to different commits.

Check:

```bash
git branch
```

Check remote branches:

```bash
git branch -r
```

Check both:

```bash
git branch -a
```

---

# 32. Why "Your Branch Is Ahead" Appears

Example:

```text
Your branch is ahead of 'origin/devops' by 1 commit.
```

Meaning:

```text
Local devops:

A──B──C
      ▲
      local devops


GitHub:

A──B
    ▲
    origin/devops
```

Your local branch has one commit that GitHub doesn't have.

Send it:

```bash
git push origin devops
```

After push:

```text
A──B──C
      ▲
      ├── local devops
      └── origin/devops
```

---

# 33. Why "Your Branch Is Behind" Appears

Example:

```text
Your branch is behind 'origin/master' by 2 commits.
```

Meaning GitHub has commits that your local branch does not have.

Update:

```bash
git pull origin master
```

or:

```bash
git fetch origin
git merge origin/master
```

---

# 34. Your `test` Branch Example

You created:

```bash
git checkout -b test
```

Then created files:

```text
file1.txt
file2.txt
file3.txt
```

Commits looked approximately like:

```text
file1 commit
     │
     ▼
file2 commit
     │
     ▼
file3 commit
```

Then pushed:

```bash
git push -u origin test
```

GitHub then had:

```text
origin/test
```

You could create:

```text
test → master
```

Pull Request.

---

# 35. Why You Saw Changes After Pull

Suppose GitHub had:

```text
master:
A──B──C
```

Your local machine:

```text
master:
A──B
```

Then:

```bash
git pull origin master
```

Git downloads `C` and updates your local branch:

```text
A──B──C
     ▲
     master
```

---

# 36. `git pull --rebase`

Command:

```bash
git pull origin master --rebase
```

This is useful when you want to update your local branch while keeping your local commits on top of the latest remote commits.

Example:

Before:

```text
Remote:
A──B──C

Local:
A──B──D
```

With rebase:

```text
A──B──C──D'
```

instead of creating a merge commit.

---

# 37. Common Command Sequence

## Clone

```bash
git clone git@github.com:USERNAME/REPO.git
cd REPO
```

## Check branches

```bash
git branch
```

## Fetch everything

```bash
git fetch --all
```

## Switch branch

```bash
git checkout devops
```

## Pull branch

```bash
git pull origin devops
```

## Check status

```bash
git status
```

## Check history

```bash
git log --oneline
```

## Check remote

```bash
git remote -v
```

---

# 38. Developer Branch Workflow

```text
                    master
                       │
                       │
                       ▼
              Create feature branch
                       │
                       ▼
                    devops
                       │
              ┌────────┴────────┐
              │                 │
           modify             test
              │                 │
              ▼                 ▼
           git add          git status
              │
              ▼
          git commit
              │
              ▼
       git push origin devops
              │
              ▼
         GitHub PR
              │
              ▼
        Code Review
              │
              ▼
       Merge / Squash
              │
              ▼
            master
```

---

# 39. Recommended Real-World Workflow

For a team project:

```bash
git checkout master
git pull origin master

git checkout -b feature/login
```

Make changes.

Then:

```bash
git status
git add .
git commit -m "Add login feature"
git push -u origin feature/login
```

Open GitHub:

```text
feature/login
      │
      ▼
Pull Request
      │
      ▼
master
```

After review:

```text
Merge / Squash and merge
```

Then locally:

```bash
git checkout master
git pull origin master
```

---

# 40. Important Commands Cheat Sheet

| Command | Meaning |
|---|---|
| `git clone URL` | Clone repository |
| `git branch` | List local branches |
| `git branch -a` | List local + remote branches |
| `git checkout branch` | Switch branch |
| `git checkout -b branch` | Create + switch branch |
| `git switch branch` | Switch branch |
| `git fetch` | Download remote information |
| `git fetch --all` | Fetch all remotes |
| `git pull` | Fetch + integrate changes |
| `git push` | Upload commits |
| `git status` | Check working tree |
| `git remote -v` | Show remote URLs |
| `git log` | Full commit history |
| `git log --oneline` | Short commit history |
| `git merge branch` | Merge branch |
| `git merge --squash branch` | Combine branch changes |
| `git rebase master` | Rebase current branch on master |

---

# 41. Common Mistakes From the Lab

### Mistake 1

Wrong:

```bash
git fetch -all
```

Correct:

```bash
git fetch --all
```

---

### Mistake 2

Wrong:

```bash
cd checkout devops
```

Correct:

```bash
git checkout devops
```

---

### Mistake 3

Wrong:

```bash
git merge -squash test
```

Correct:

```bash
git merge --squash test
```

---

### Mistake 4

Wrong:

```bash
git log oneline
```

Correct:

```bash
git log --oneline
```

---

### Mistake 5

Wrong:

```bash
git pull origin master -rebase
```

Correct:

```bash
git pull origin master --rebase
```

---

### Mistake 6

Commit command should normally be:

```bash
git commit -m "Added new file"
```

`-m` means commit message.

---

# 42. Clean Professional Workflow

```text
             GitHub
                │
                │ clone
                ▼
        Local Repository
                │
                ▼
          master branch
                │
        git checkout -b
                │
                ▼
        feature/devops branch
                │
          Make changes
                │
          git add .
                │
          git commit
                │
          git push -u
                │
                ▼
             GitHub
                │
                ▼
         Pull Request
                │
         Code Review
                │
          ┌─────┴─────┐
          │           │
        Merge       Squash
          │           │
          └─────┬─────┘
                ▼
             master
```

---

# 43. Easy Interview Explanation

### What is Git?

Git is a distributed version control system used to track code changes and collaborate with other developers.

### What is a branch?

A branch is an independent line of development.

### What is `git fetch`?

`git fetch` downloads information about changes from the remote repository without directly integrating those changes into the current branch.

### What is `git pull`?

`git pull` fetches remote changes and integrates them into the current branch.

### What is merge?

Merge combines changes from one branch into another branch.

### What is rebase?

Rebase moves or reapplies your branch commits on top of another branch to create a more linear history.

### What is squash?

Squash combines multiple commits into a single commit.

### What is a Pull Request?

A Pull Request is a GitHub workflow used to propose changes from one branch/repository to another for review before merging.

---

# 44. Final Mental Model

Remember this:

```text
CLONE
  │
  ▼
REPOSITORY
  │
  ├── master
  ├── devops
  └── test
       │
       ▼
    CHANGE
       │
       ▼
    git add
       │
       ▼
    git commit
       │
       ▼
    git push
       │
       ▼
     GitHub
       │
       ▼
 Pull Request
       │
       ▼
 Review
       │
       ▼
Merge / Squash
       │
       ▼
    master
```

## Golden Rules

1. **fetch** = download remote information
2. **pull** = fetch + integrate
3. **push** = upload local commits
4. **merge** = combine branch histories
5. **rebase** = replay commits on a new base
6. **squash** = combine multiple commits into one
7. **Pull Request** = request review and integration through GitHub
8. Always check `git status` before important Git operations.
9. Before starting new work, update your base branch.
10. Avoid rebasing shared/public branches unless the team agrees.
11. Use meaningful commit messages.
12. Never commit passwords, API keys, SSH private keys, or other secrets.
