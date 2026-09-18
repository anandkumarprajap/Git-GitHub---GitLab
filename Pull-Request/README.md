# 🚀 GitHub Pull Request — Developer & Maintainer Workflow

A practical GitHub exercise to simulate a **real-world development team workflow** using two GitHub accounts:

* **`anandkumarprajap`** → Repository Owner / Maintainer
* **`aadarshkumar1920`** → Developer

The goal is to practice:

```text
Developer → Branch → Code → Commit → Push
          → Pull Request → Code Review
          → Changes Requested → Fix → Review
          → Approve → Merge → Delete Branch
```

---

## 👥 Team Roles

| GitHub Account     | Role        | Responsibility                                     |
| ------------------ | ----------- | -------------------------------------------------- |
| `anandkumarprajap` | Maintainer  | Review, approve and merge PR                       |
| `aadarshkumar1920` | Developer   | Create branch, develop, commit, push and create PR |
| `master`           | Main Branch | Receives reviewed and approved code                |

### Workflow

```text
aadarshkumar1920
       │
       │ Pull Request
       ▼
anandkumarprajap
       │
       │ Review / Approve
       ▼
     master
```

> **Important:** The developer account must have access to the repository. Adding someone as a PR reviewer is different from giving them repository permission to push code.

---

# 1. 🔐 Give Developer Repository Access

Login as:

```text
anandkumarprajap
```

Open your repository:

```text
github-pull-request
```

Go to:

```text
Settings
    ↓
Collaborators / Collaborators and teams
    ↓
Add people
```

Add:

```text
aadarshkumar1920
```

Send the invitation.

Now login as:

```text
aadarshkumar1920
```

Accept the repository invitation.

The second account can now work as the **Developer**.

---

# 2. 📥 Clone the Repository

Switch to:

```text
aadarshkumar1920
```

Clone the repository:

```bash
git clone git@github.com:anandkumarprajap/github-pull-request.git
```

Enter the repository:

```bash
cd github-pull-request
```

Check the branch:

```bash
git branch
```

Expected:

```text
* master
```

Get the latest code:

```bash
git pull origin master
```

---

# 3. 🌿 Create Developer Branch

Developers should not normally make changes directly on `master`.

Create a feature branch:

```bash
git checkout -b developer-feature
```

Check:

```bash
git branch
```

Expected:

```text
* developer-feature
  master
```

Now development happens on:

```text
developer-feature
```

---

# 4. 💻 Developer Writes Code

Create an example application file:

```bash
echo "This code has a bug" > app.txt
```

Check the working tree:

```bash
git status
```

Expected:

```text
Untracked files:

    app.txt
```

Check the file:

```bash
cat app.txt
```

Output:

```text
This code has a bug
```

---

# 5. 📦 Developer Commits Code

Add the file:

```bash
git add app.txt
```

Commit:

```bash
git commit -m "add application code"
```

Check commit history:

```bash
git log --oneline
```

Example:

```text
b36abf6 add application code
672ca72 add txt good
11fbaed add babu html
81df01f add hello.txt
```

---

# 6. 🚀 Push Developer Branch

Push the branch to GitHub:

```bash
git push -u origin developer-feature
```

GitHub now contains:

```text
master
developer-feature
```

---

# 7. 🔀 Create Pull Request

Login to GitHub as:

```text
aadarshkumar1920
```

Open:

```text
anandkumarprajap/github-pull-request
```

Click:

```text
Compare & pull request
```

Make sure the branches are:

```text
base repository:
anandkumarprajap/github-pull-request

base:
master

compare:
developer-feature
```

PR structure:

```text
aadarshkumar1920

wants to merge changes into

master

from

developer-feature
```

Example title:

```text
Add application code
```

Example description:

```text
Added application code for testing.

Please review the changes.
```

Click:

```text
Create pull request
```

---

# 8. 👨‍💻 Maintainer Opens Pull Request

Switch to:

```text
anandkumarprajap
```

Open the Pull Request.

You should see:

```text
aadarshkumar1920 wants to merge

developer-feature → master
```

This simulates a real development team workflow.

---

# 9. 🔎 Maintainer Reviews Code

Open:

```text
Files changed
```

You will see:

```diff
+ This code has a bug
```

The Maintainer now reviews the developer's changes.

### General Comment

Example:

```text
Please fix the issue in app.txt before merging.
```

### Line Comment

Click the `+` beside the changed line.

Comment:

```text
Please fix this line. The current implementation is incorrect.
```

Then:

```text
Start a review
        ↓
Finish your review
        ↓
Request changes
        ↓
Submit review
```

GitHub will show:

```text
Changes requested
```

---

# 10. 🔄 Developer Receives Review

Switch back to:

```text
aadarshkumar1920
```

Open the Pull Request.

The developer sees:

```text
Changes requested
```

And the review comment:

```text
Please fix this line.
```

The developer must now fix the code.

---

# 11. 🛠️ Developer Fixes Code

On the developer machine:

```bash
git checkout developer-feature
```

Fix the file:

```bash
echo "This code is fixed" > app.txt
```

Check the file:

```bash
cat app.txt
```

Output:

```text
This code is fixed
```

Check Git status:

```bash
git status
```

Expected:

```text
modified: app.txt
```

---

# 12. 📝 Commit the Fix

Stage the modified file:

```bash
git add app.txt
```

Commit the fix:

```bash
git commit -m "fix application code"
```

Check history:

```bash
git log --oneline
```

Example:

```text
7a12345 fix application code
b36abf6 add application code
672ca72 add txt good
11fbaed add babu html
81df01f add hello.txt
```

---

# 13. 🚀 Push the Fix

Push the updated branch:

```bash
git push origin developer-feature
```

### ⚠️ Important

**Do not create a second Pull Request.**

The existing Pull Request automatically receives the new commit.

```text
Pull Request

developer-feature
       │
       ├── b36abf6 add application code
       │
       └── 7a12345 fix application code
                         ↑
                      New fix
```

---

# 14. 🔍 Maintainer Reviews Again

Switch to:

```text
anandkumarprajap
```

Open the existing Pull Request.

Go to:

```text
Files changed
```

Check the updated code.

If the requested changes have been completed:

```text
Conversation
    ↓
Review changes
    ↓
Approve
```

Example review comment:

```text
The requested changes have been completed. Approved.
```

Submit the review.

The Pull Request is now approved and ready for merging, subject to any repository branch-protection requirements.

---

# 15. 🔀 Merge Pull Request

As:

```text
anandkumarprajap
```

Click:

```text
Merge pull request
```

Then:

```text
Confirm merge
```

The workflow becomes:

```text
developer-feature
       │
       │ Pull Request
       ▼
     master
       │
       ▼
    MERGED
```

GitHub will show that the Pull Request was successfully merged.

---

# 16. 🗑️ Delete Developer Branch

After merging, GitHub may show:

```text
Delete branch
```

Click it to delete the remote feature branch.

Or use the command line:

```bash
git push origin --delete developer-feature
```

---

# 17. 🔄 Update Local Master

Switch to the main branch:

```bash
git checkout master
```

Pull the latest changes:

```bash
git pull origin master
```

Check the history:

```bash
git log --oneline --graph --decorate --all
```

The developer's changes should now be part of `master`.

---

# 🔥 Complete Real-World GitHub Workflow

```text
                    GitHub Repository
                           │
                         master
                           │
                           ▼
                 Developer clones repo
                           │
                           ▼
               git checkout -b feature
                           │
                           ▼
                      Write code
                           │
                           ▼
                         Commit
                           │
                           ▼
                          Push
                           │
                           ▼
                  Create Pull Request
                           │
                           ▼
                   developer-feature
                           │
                           ▼
                    Code Review
                           │
                    ┌──────┴──────┐
                    │             │
                 Problem         Good
                    │             │
                    ▼             ▼
            Request Changes    Approve
                    │             │
                    ▼             │
             Developer fixes      │
                    │             │
                  Commit           │
                    │             │
                   Push            │
                    │             │
                    └──────┬───────┘
                           ▼
                    Review Again
                           │
                           ▼
                       Approve
                           │
                           ▼
                    Merge to master
                           │
                           ▼
                    Delete Branch
```

---

# 📌 Two-Account Practice Flow

### Developer

```text
aadarshkumar1920
```

Responsibilities:

```text
Clone Repository
      ↓
Create Feature Branch
      ↓
Write Code
      ↓
git add
      ↓
git commit
      ↓
git push
      ↓
Create Pull Request
      ↓
Receive Review
      ↓
Fix Code
      ↓
Commit Fix
      ↓
Push Fix
```

### Maintainer

```text
anandkumarprajap
```

Responsibilities:

```text
Open Pull Request
      ↓
Review Code
      ↓
Request Changes
      ↓
Developer Fixes Code
      ↓
Review Again
      ↓
Approve
      ↓
Merge
      ↓
Delete Feature Branch
```

---

# 🧠 Important Git Commands

### Check Branch

```bash
git branch
```

### Create Feature Branch

```bash
git checkout -b developer-feature
```

### Switch Branch

```bash
git checkout master
```

### Check Changes

```bash
git status
```

### Stage Changes

```bash
git add app.txt
```

### Commit

```bash
git commit -m "add application code"
```

### Push Branch

```bash
git push -u origin developer-feature
```

### Pull Latest Changes

```bash
git pull origin master
```

### Delete Remote Branch

```bash
git push origin --delete developer-feature
```

### View Commit History

```bash
git log --oneline
```

### View Graph

```bash
git log --oneline --graph --decorate --all
```

---

# ⚠️ Important Concepts

## 1. Reviewer ≠ Repository Access

Adding:

```text
aadarshkumar1920
```

as a **reviewer** does not automatically give that account permission to push code.

For this hands-on exercise, give the developer account repository access first.

---

## 2. Developer Should Work on a Feature Branch

Avoid:

```text
master → directly write code
```

Prefer:

```text
master
   │
   └── developer-feature
             │
             ├── Code
             ├── Commit
             └── Push
```

---

## 3. One Pull Request Can Receive Multiple Commits

Initial commit:

```text
b36abf6 add application code
```

Review:

```text
Changes requested
```

Developer fixes:

```text
7a12345 fix application code
```

Both commits remain associated with the same Pull Request:

```text
developer-feature
       │
       ├── b36abf6 add application code
       │
       └── 7a12345 fix application code
```

There is **no need to create another Pull Request** for the fix.

---

# 🎯 DevOps Interview Workflow

A typical GitHub team workflow can be summarized as:

```text
Clone
  ↓
Create Branch
  ↓
Develop
  ↓
Commit
  ↓
Push
  ↓
Pull Request
  ↓
Code Review
  ↓
Changes Requested?
  │
  ├── Yes → Fix → Commit → Push → Review Again
  │
  └── No  → Approve
               ↓
             Merge
               ↓
        Delete Feature Branch
               ↓
        Update Local Master
```

### Roles

```text
Developer
    │
    ├── Branch
    ├── Code
    ├── Commit
    ├── Push
    └── Pull Request
             │
             ▼
Maintainer
    │
    ├── Review
    ├── Request Changes
    ├── Approve
    └── Merge
             │
             ▼
          master
```

> **Key takeaway:** A Pull Request is not just a way to merge code. It provides a workflow for **collaboration, code review, feedback, approval, and controlled merging**.
