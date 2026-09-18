# 🔐 GitHub Protected `master` Branch — Developer & Maintainer Workflow

This is a hands-on GitHub practice setup that simulates a **real company-style Git workflow** using two GitHub accounts.

The main goal is to make sure:

* 👨‍💻 Developer works on a feature branch
* 🔒 `master` is protected
* ❌ Developer cannot directly push to `master`
* 🔀 Developer creates a Pull Request
* 👨‍💼 Maintainer reviews the code
* ❌ If there is a problem → Request Changes
* 🛠️ Developer fixes the code and pushes again
* 🔄 Existing PR automatically updates
* ✅ Maintainer reviews and approves
* 🚀 Maintainer merges the PR into `master`

---

# 👥 GitHub Roles

| GitHub Account     | Role                  | Responsibility                                  |
| ------------------ | --------------------- | ----------------------------------------------- |
| `aadarshkumar1920` | Developer             | Create branch → Code → Commit → Push → PR → Fix |
| `anandkumarprajap` | Maintainer            | Review → Request Changes → Approve → Merge      |
| `master`           | Protected Main Branch | Receives reviewed and approved code             |

### Overall Architecture

```text
aadarshkumar1920
      │
      │ Push
      ▼
developer-feature
      │
      │ Pull Request
      ▼
master 🔒
      │
      │ Review
      ▼
anandkumarprajap
      │
      ├── ❌ Error
      │      │
      │      ▼
      │   Request Changes
      │      │
      │      ▼
      │   Developer Fixes
      │      │
      │      ▼
      │   Push Again
      │      │
      │      ▼
      │   Existing PR Updates
      │
      └── ✅ Correct
             │
             ▼
           Approve
             │
             ▼
           Merge
             │
             ▼
          master 🔒
```

---

# 1. 🔑 Give Developer Repository Access

Login to GitHub as:

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
Collaborators
    ↓
Add people
```

Add:

```text
aadarshkumar1920
```

Give the developer account a repository role that allows it to contribute code.

Do **not** give the developer administrator/owner-level access.

Then log in as:

```text
aadarshkumar1920
```

Accept the repository invitation.

---

# 2. 🔒 Protect the `master` Branch

Login as:

```text
anandkumarprajap
```

Go to:

```text
Repository
    ↓
Settings
    ↓
Branches / Branch protection rules
```

Create a branch protection rule.

### Branch name pattern

Enter:

```text
master
```

> Protect `master`, not `developer-feature`.

---

# 3. ⚙️ Recommended Branch Protection Settings

For this practice, use the following configuration.

## Require a pull request before merging

```text
☑ Require a pull request before merging
```

This creates:

```text
developer-feature
       │
       ▼
Pull Request
       │
       ▼
master
```

The developer must submit changes through a PR.

---

## Require approvals

Enable:

```text
☑ Require approvals
```

Set:

```text
Required approvals: 1
```

Workflow:

```text
Developer
    ↓
Pull Request
    ↓
1 Approval Required
    ↓
Merge
```

---

## Dismiss stale pull request approvals

Enable:

```text
☑ Dismiss stale pull request approvals when new commits are pushed
```

This is useful when the developer changes the code after review.

Example:

```text
Code v1
   ↓
Review
   ↓
Changes requested
   ↓
Developer fixes
   ↓
Code v2 pushed
   ↓
Review again
   ↓
Approve
```

---

## Require approval of the most recent reviewable push

If available, enable:

```text
☑ Require approval of the most recent reviewable push
```

This helps ensure that the latest code pushed to the PR is reviewed before merging.

---

## Require conversation resolution

Enable:

```text
☑ Require conversation resolution before merging
```

This means review conversations must be resolved before the PR can be merged.

Example:

```text
Reviewer:
Please fix app.txt

       ↓

Developer:
Fixes app.txt

       ↓

Reviewer:
Checks fix

       ↓

Conversation resolved
```

---

# 4. ❌ Settings to Leave OFF

For this basic Git/GitHub practice, leave these disabled:

```text
☐ Require review from Code Owners

☐ Require status checks to pass before merging

☐ Require signed commits

☐ Require linear history

☐ Require deployments to succeed before merging

☐ Lock branch
```

These features can be practiced later with advanced GitHub workflows and CI/CD.

---

# 5. 🛡️ Prevent Administrator Bypass

If available, enable:

```text
☑ Do not allow bypassing the above settings
```

This makes the branch protection rules apply more strictly, including to users who might otherwise have bypass permissions.

Desired result:

```text
master 🔒

Direct push              ❌
Force push               ❌
Delete branch            ❌
Merge without PR         ❌
Merge without approval   ❌

Approved PR              ✅
Maintainer merge         ✅
```

---

# 6. 🚫 Force Push and Branch Deletion

Leave these disabled:

```text
☐ Allow force pushes

☐ Allow deletions
```

This protects `master` from accidental destructive operations.

---

# 7. 👨‍💻 Developer Clones Repository

Switch to:

```text
aadarshkumar1920
```

Clone:

```bash
git clone git@github.com:anandkumarprajap/github-pull-request.git
```

Enter the repository:

```bash
cd github-pull-request
```

Check branches:

```bash
git branch
```

Expected:

```text
* master
```

Update local `master`:

```bash
git checkout master
git pull origin master
```

---

# 8. 🌿 Developer Creates Feature Branch

Do not develop directly on `master`.

Create:

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

# 9. 💻 Developer Writes Code

For practice, create a file containing an intentional error:

```bash
echo "This code has an ERROR" > app.txt
```

Check:

```bash
cat app.txt
```

Output:

```text
This code has an ERROR
```

Check Git status:

```bash
git status
```

---

# 10. 📦 Developer Commits Code

Stage the file:

```bash
git add app.txt
```

Commit:

```bash
git commit -m "add application code"
```

Check the commit:

```bash
git log --oneline
```

Example:

```text
b36abf6 add application code
```

---

# 11. 🚀 Developer Pushes Feature Branch

Push:

```bash
git push -u origin developer-feature
```

GitHub now contains:

```text
master
developer-feature
```

The developer has pushed to the feature branch, not `master`.

---

# 12. 🔀 Developer Creates Pull Request

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

Make sure:

```text
base: master
compare: developer-feature
```

Correct:

```text
developer-feature
       │
       ▼
      PR
       │
       ▼
     master
```

Incorrect:

```text
master
   │
   ▼
   PR
   │
   ▼
developer-feature
```

---

# 13. 📝 Pull Request Details

Example title:

```text
Add application code
```

Description:

```text
Added application code for testing.

Please review the changes.
```

Click:

```text
Create pull request
```

---

# 14. 👨‍💼 Maintainer Reviews PR

Switch to:

```text
anandkumarprajap
```

Open the Pull Request.

You should see:

```text
aadarshkumar1920

wants to merge

developer-feature → master
```

Go to:

```text
Files changed
```

You will see:

```diff
+ This code has an ERROR
```

---

# 15. ❌ Request Changes

The Maintainer finds the problem.

Click:

```text
Review changes
```

Select:

```text
Request changes
```

Example comment:

```text
Please fix the error in app.txt and push the corrected code.

After fixing, please request another review.
```

Submit the review.

GitHub will show:

```text
Changes requested
```

The PR should not be merged yet.

---

# 16. 🔄 Developer Receives Changes Requested

Switch to:

```text
aadarshkumar1920
```

Open the same Pull Request.

The developer sees:

```text
Changes requested
```

and the review comment:

```text
Please fix the error in app.txt.
```

Now the developer fixes the code.

---

# 17. 🛠️ Developer Fixes Code

Switch to the developer branch:

```bash
git checkout developer-feature
```

Fix the file:

```bash
echo "This code is fixed and ready for review" > app.txt
```

Check:

```bash
cat app.txt
```

Output:

```text
This code is fixed and ready for review
```

Check status:

```bash
git status
```

---

# 18. 📝 Developer Commits the Fix

Stage:

```bash
git add app.txt
```

Commit:

```bash
git commit -m "fix application code"
```

Check:

```bash
git log --oneline
```

Example:

```text
7a12345 fix application code
b36abf6 add application code
```

---

# 19. 🚀 Developer Pushes the Fix

Push:

```bash
git push origin developer-feature
```

### ⚠️ Important

Do **not** create another Pull Request.

The existing Pull Request automatically updates with the new commit.

```text
Existing PR
     │
     ▼
developer-feature
     │
     ├── b36abf6 add application code
     │
     └── 7a12345 fix application code
                         ↑
                      New commit
```

---

# 20. 🔄 Request Another Review

The developer can comment on the existing PR:

```text
The requested changes have been fixed.

Please review again.
```

The Maintainer can now review the updated code.

---

# 21. 🔎 Maintainer Reviews Again

Switch to:

```text
anandkumarprajap
```

Open the same PR.

Go to:

```text
Files changed
```

Compare the change:

```diff
- This code has an ERROR
+ This code is fixed and ready for review
```

The requested issue has been fixed.

---

# 22. ✅ Approve the Pull Request

Click:

```text
Review changes
```

Select:

```text
Approve
```

Example comment:

```text
The requested changes have been completed. Approved.
```

Submit the review.

Now the PR has the required approval.

---

# 23. 🚀 Merge into Protected `master`

As:

```text
anandkumarprajap
```

click:

```text
Merge pull request
```

Then:

```text
Confirm merge
```

The workflow is now:

```text
developer-feature
       │
       │ Pull Request
       ▼
     master 🔒
       │
       │ Approved
       ▼
      MERGE
```

The developer did not directly push the code into `master`.

The Maintainer merged the approved PR.

---

# 24. 🗑️ Delete Feature Branch

After the PR is merged, GitHub may display:

```text
Delete branch
```

You can delete the remote branch.

Command-line alternative:

```bash
git push origin --delete developer-feature
```

---

# 25. 🔄 Update Local `master`

As the repository owner:

```bash
git checkout master
```

Pull the latest changes:

```bash
git pull origin master
```

View history:

```bash
git log --oneline --graph --decorate --all
```

The merged developer changes should now be part of `master`.

---

# 26. 🧪 Test Protected `master`

This is an important part of the practice.

Switch to:

```text
aadarshkumar1920
```

Try to work directly on `master`:

```bash
git checkout master
```

Make a change:

```bash
echo "direct master change" >> app.txt
```

Stage it:

```bash
git add app.txt
```

Commit:

```bash
git commit -m "direct master change"
```

Now try:

```bash
git push origin master
```

Because `master` is protected, the push should be rejected according to the protection rules you configured.

Expected concept:

```text
remote: error
remote: protected branch
remote: changes must go through a pull request
```

The exact Git error message may differ depending on your repository configuration.

---

# 27. ✅ Test the Correct Developer Workflow

Now switch back:

```bash
git checkout developer-feature
```

Developer works on:

```text
developer-feature
```

Then:

```bash
git add .
git commit -m "developer changes"
git push origin developer-feature
```

This is the expected workflow.

```text
Developer
    │
    ▼
developer-feature
    │
    ▼
Pull Request
    │
    ▼
Maintainer Review
    │
    ├── ❌ Changes Requested
    │        │
    │        ▼
    │     Developer Fix
    │        │
    │        ▼
    │     Push Again
    │        │
    │        ▼
    │     Existing PR
    │
    └── ✅ Approved
             │
             ▼
          Merge
             │
             ▼
          master 🔒
```

---

# 🏢 Real Company-Style Git Workflow

The complete workflow to remember:

```text
                    MASTER 🔒
                        ▲
                        │
                      MERGE
                        │
                anandkumarprajap
                   Maintainer
                        │
                     APPROVE
                        │
                  Pull Request
                        │
                        │
               developer-feature
                        ▲
                        │
                aadarshkumar1920
                    Developer
                        │
                    Write Code
                        │
                      Commit
                        │
                       Push
```

---

# ❌ When Code Has an Error

```text
Developer
    │
    ▼
Create Feature Branch
    │
    ▼
Write Code
    │
    ▼
Commit
    │
    ▼
Push
    │
    ▼
Create PR
    │
    ▼
Maintainer Review
    │
    ▼
❌ Error Found
    │
    ▼
Request Changes
    │
    ▼
Developer Fixes Code
    │
    ▼
Commit
    │
    ▼
Push Same Branch
    │
    ▼
Existing PR Updates
    │
    ▼
Maintainer Reviews Again
    │
    ▼
✅ Approve
    │
    ▼
Merge
    │
    ▼
MASTER 🔒
```

---

# 🔐 Protected `master` Model

```text
                    master 🔒
                       │
          ┌────────────┴────────────┐
          │                         │
       Direct Push               Pull Request
          │                         │
          ❌                         ▼
                            Code Review
                                  │
                         ┌────────┴────────┐
                         │                 │
                       Error             Good
                         │                 │
                         ▼                 ▼
                  Request Changes       Approve
                         │                 │
                         ▼                 ▼
                  Developer Fixes       Merge
                         │                 │
                         └───────┬─────────┘
                                 ▼
                              master
```

---

# 📋 Final Roles

| Account / Branch   | Role             |             Push | Review | Merge |
| ------------------ | ---------------- | ---------------: | -----: | ----: |
| `aadarshkumar1920` | Developer        | ✅ Feature branch |      ❌ |     ❌ |
| `anandkumarprajap` | Maintainer       |                ✅ |      ✅ |     ✅ |
| `master`           | Protected branch |               🔒 |      — |     — |

---

# ⚠️ Important Distinction

## Reviewer vs Repository Access

Adding:

```text
aadarshkumar1920
```

as a **Pull Request reviewer** is different from giving that account repository permission to contribute code.

For this hands-on exercise:

```text
Repository Access
       +
Branch Protection
       +
Pull Request Review
```

are three separate concepts.

---

# 🎯 What You Should Memorize for DevOps Interviews

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
Code Review
       ↓
Changes Requested?
       │
   ┌───┴────┐
   │        │
  YES       NO
   │        │
   ▼        ▼
Fix Code   Approve
   │        │
Commit      │
   │        │
Push        │
   │        │
   └───┬────┘
       ▼
 Review Again
       ↓
    Approve
       ↓
 Merge
       ↓
 master 🔒
       ↓
Delete Feature Branch
```

## 🧠 Key Takeaways

1. **Never develop directly on `master` in a team workflow.**
2. **Use a feature branch for development.**
3. **Push the feature branch to GitHub.**
4. **Create a Pull Request targeting `master`.**
5. **Maintainer reviews the code.**
6. **If there is a problem, use `Request changes`.**
7. **Developer fixes the same branch.**
8. **Push the new commit to the same branch.**
9. **The existing PR automatically updates.**
10. **Maintainer reviews the new commit again.**
11. **Maintainer approves the PR.**
12. **Maintainer merges the PR into protected `master`.**
13. **Delete the feature branch after merging.**
14. **Protected `master` prevents unauthorized direct changes.**

> 🔑 **Core concept:** Developers contribute through Pull Requests, while the protected `master` branch receives changes only through the required review and merge process.
