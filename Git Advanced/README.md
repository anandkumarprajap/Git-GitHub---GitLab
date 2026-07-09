┌──────────────────────────────────────────────────────────────────────────────────────────────┐
│                    GIT HISTORY (YOUR REPOSITORY - DEVBOARD)                                  │
└──────────────────────────────────────────────────────────────────────────────────────────────┘


                                 LOCAL REPOSITORY HISTORY

                         MASTER BRANCH
                               │
                               ▼
(1) 029ee6d
┌─────────────────────────────┐
│ Initial Commit              │
└──────────────┬──────────────┘
               │
               │
               ├─────────────────────────────────────► DEVOPS BRANCH CREATED
               │                                       │
               │                                       ▼
               │                         (2) 814fa3e
               │                    ┌──────────────────────┐
               │                    │ Added nginx script   │
               │                    └──────────┬───────────┘
               │                               │
               │                               ▼
               │                         (3) 12ed3dd
               │                    ┌──────────────────────┐
               │                    │ Deleted hello.html~  │
               │                    └──────────┬───────────┘
               │                               │
               │                               ▼
               │                         (4) 5bfa3b8
               │                    ┌──────────────────────┐
               │                    │ Added test.py        │
               │                    └──────────┬───────────┘
               │                               │
               │                               ▼
               │                         (5) 4d2721f
               │                    ┌──────────────────────┐
               │                    │ Added changes test.py│
               │                    └──────────┬───────────┘
               │                               │
               │                               │
               │                               └─────────────┐
               │                                             │
               ▼                                             ▼
(6) Merge DevOps → Master (Fast Forward)
┌──────────────────────────────────────────────┐
│ Master now contains nginx + delete + test.py │
└──────────────────────┬───────────────────────┘
                       │
                       ▼
(7) 7e885a6
┌──────────────────────────────────────────────┐
│ GitHub Pull Request #1 merged                │
└──────────────────────┬───────────────────────┘
                       │
                       ▼
(8) ec91bfe
┌──────────────────────────────────────────────┐
│ Added new-master-file.txt                    │
└──────────────────────┬───────────────────────┘
                       │
                       ▼
(9) Rebase TEST on MASTER
┌──────────────────────────────────────────────┐
│ Test branch replayed on latest master        │
└──────────────────────┬───────────────────────┘
                       │
                       ▼
(10) GitHub Squash Merge
┌──────────────────────────────────────────────┐
│ PR #2 (test → master)                        │
│ file1 + file2 + file3 → ONE commit           │
└──────────────────────────────────────────────┘



──────────────────────────────────────────────────────────────────────────────────────────────

TEST BRANCH

                From commit 5bfa3b8

                        │
                        ▼
                bd5f875
         ┌────────────────────┐
         │ Added new-file.txt │
         └─────────┬──────────┘
                   │
                   ▼
                fa680dc
         ┌────────────────────┐
         │ Added file1.txt    │
         └─────────┬──────────┘
                   │
                   ▼
                b39af7e
         ┌────────────────────┐
         │ Added file2.txt    │
         └─────────┬──────────┘
                   │
                   ▼
                1cd38ea
         ┌────────────────────┐
         │ Added file3.txt    │
         └─────────┬──────────┘
                   │
                   ▼
          Push → Pull Request
                   │
                   ▼
          GitHub Squash Merge
                   │
                   ▼
      All commits become ONE commit on MASTER



──────────────────────────────────────────────────────────────────────────────────────────────

MERGE

master                     devops

A──B──C                     D──E
      \                    /
       └──── Merge ───────┘

                │
                ▼

A──B──C──────────────M

✔ History preserved
✔ Merge commit may be created



FAST-FORWARD MERGE

master

A──B

devops

A──B──C──D

git merge devops

Result

A──B──C──D

✔ No merge commit
✔ Pointer simply moves forward



REBASE

master

A──B──C

test

     D──E

git rebase master

Result

A──B──C──D'──E'

✔ Clean linear history
✔ Commit IDs change
✔ History rewritten



SQUASH MERGE

feature

A──B──C──D

          │
          ▼

master

X──────────S

S = One single commit

✔ Clean Git history
✔ Multiple commits become one
✔ Individual commit history is lost



COMMIT vs BRANCH

COMMIT
──────
A snapshot of your project at a point in time.

Project
   │
Commit-1
   │
Commit-2
   │
Commit-3

BRANCH
──────
A movable pointer to commits.

master ─────────► C3

devops ─────────► C5

test ───────────► C8



REAL LIFE ANALOGY

Commit  = Save button in a game 🎮

Branch  = Alternate storyline

Merge   = Combine two storylines

Rebase  = Rewrite storyline to make it look continuous

Squash  = Compress many saves into one final save

Clone   = Download complete repository

Fetch   = Check remote updates only

Pull    = Fetch + Merge/Rebase updates

Checkout = Switch branch

Checkout -b = Create new branch + Switch immediately
