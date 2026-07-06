# SSH Key, Clone, Clone vs Fork (Git & GitHub Notes)
![15.png](15.png)

![16.png](16.png)

![17.png](17.png)

## Objective

Learn how to:

- Generate an SSH key
- Add the SSH key to GitHub
- Clone a GitHub repository
- Understand the difference between Clone and Fork
- Know when to use Clone and Fork

---

# 1. What is an SSH Key?

SSH (Secure Shell) is a secure authentication method that allows your computer to communicate with GitHub without entering your username and password every time.

GitHub identifies your computer using a pair of cryptographic keys.

- Private Key → Stored on your computer (Never share it)
- Public Key → Added to your GitHub account

---

# 2. Generate an SSH Key

Check whether SSH is installed:

```bash
ssh
```

Generate a new SSH key:

```bash
ssh-keygen
```

Press **Enter** to accept the default location.

Example:

```
C:\Users\david\.ssh\id_ed25519
```

If a key already exists:

```
Overwrite (y/n)?
```

Choose:

```
y
```

(Optional) Enter a passphrase or press Enter to skip.

---

# 3. View Generated Keys

Move to the SSH directory:

```bash
cd ~/.ssh
```

Windows PowerShell:

```powershell
cd $HOME\.ssh
```

List files:

```bash
ls
```

Output:

```
id_ed25519
id_ed25519.pub
```

- id_ed25519 → Private Key
- id_ed25519.pub → Public Key

---

# 4. Display the Public Key

```bash
cat id_ed25519.pub
```

Copy the entire output.

Example:

```
ssh-ed25519 AAAAC3Nza... david@LAPTOP
```

---

# 5. Add SSH Key to GitHub

1. Login to GitHub.
2. Go to **Settings**.
3. Open **SSH and GPG keys**.
4. Click **New SSH key**.
5. Enter a title.
6. Paste the copied public key.
7. Click **Add SSH key**.

---

# 6. Test the SSH Connection

```bash
ssh -T git@github.com
```

Expected output:

```
Hi username! You've successfully authenticated.
```

---

# 7. Clone a Repository

Clone using SSH:

```bash
git clone git@github.com:username/repository.git
```

Example:

```bash
git clone git@github.com:anandkumarprajap/Git-GitHub---GitLab.git
```

Move into the repository:

```bash
cd Git-GitHub---GitLab
```

Check files:

```bash
ls
```

---

# What is Clone?

Clone creates a complete copy of an existing GitHub repository on your local computer.

It downloads:

- Files
- Branches
- Commit history
- Git configuration

---

# Clone Uses

- Download an existing project
- Contribute to a repository
- Work locally
- Create new branches
- Push changes back to GitHub

---

# What is Fork?

A Fork creates your own copy of someone else's repository on your GitHub account.

The forked repository is independent of the original repository.

---

# Fork Uses

- Contribute to open-source projects
- Experiment without affecting the original project
- Maintain your own version
- Submit Pull Requests

---

# Clone vs Fork

| Clone | Fork |
|-------|------|
| Local copy | GitHub copy |
| Stored on your computer | Stored in your GitHub account |
| Uses `git clone` | Uses the GitHub **Fork** button |
| Can push if you have permission | You always have permission on your fork |
| Best for repositories you own or collaborate on | Best for contributing to open-source projects |

---

# Workflow

## Clone Workflow

```
GitHub Repository
        │
        ▼
git clone
        │
        ▼
Local Computer
        │
        ▼
Edit → Commit → Push
```

## Fork Workflow

```
Original Repository
        │
        ▼
Fork
        │
        ▼
Your GitHub Repository
        │
        ▼
git clone
        │
        ▼
Local Computer
        │
        ▼
Commit → Push
        │
        ▼
Create Pull Request
```

---

# Important Commands

```bash
ssh-keygen
```

```bash
ssh -T git@github.com
```

```bash
cat ~/.ssh/id_ed25519.pub
```

```bash
git clone <repository-url>
```

```bash
git remote -v
```

```bash
git status
```

---

# Observation

- Generated an SSH key pair.
- Added the public key to GitHub.
- Authenticated GitHub using SSH.
- Cloned a repository successfully.
- Learned the difference between Clone and Fork.
- Understood when to use Clone and Fork in real-world development.
......................................................................................................................................................
......................................................................................................................................................
# Git & GitHub Hands-on Notes

## Objective
Learn how to:
- Create shell scripts
- Initialize a Git repository
- Track files using Git
- Generate SSH keys
- Connect GitHub with SSH
- Push code from Local → GitHub
- Pull changes from GitHub → Local

---

# 1. Create a Shell Script

```bash
mkdir shell-scripts
cd shell-scripts

vim hello.txt
rm hello.txt

vim hello.sh
```

### hello.sh

```bash
#!/bin/bash

read -p "Enter a name: " name
echo "hello.dosto, $name"
```

Check the file:

```bash
cat hello.sh
```

Give execute permission:

```bash
chmod 746 hello.sh
```

Check permissions:

```bash
ls -l
```

Run the script:

```bash
./hello.sh
```

Example Output

```
Enter a name:
Anand

hello.dosto, Anand
```

---

## Screenshot

![Creating shell script](1.png)
![File permissions](2.png)

- Creating shell-scripts directory
- Creating hello.sh
- Running the shell script
- File permissions
- Script execution

---

# 2. Version Control System (VCS)

## What is Git?

Git is a **Distributed Version Control System (DVCS)**.

It helps developers:

- Track file changes
- Restore previous versions
- Collaborate with teams
- Share projects

### Distributed Version Control

Examples

- GitHub
- GitLab
- Bitbucket

Each developer has a complete copy of the repository.

### Centralized Version Control

Example

- SVN (Subversion)

Only one central server stores the project.

---

# 3. Initialize Git Repository

Check repository status:

```bash
git status
```

Output:

```
fatal: not a git repository
```

Initialize Git:

```bash
git init
```

Configure Git:

```bash
git config --global user.name "anandkumarprjap"
git config --global user.email "anandkumar7738@gmail.com"
```

Check status:

```bash
git status
```

---

## Screenshot

![Git Init](3.png)

- git init
- Git configuration

---

# 4. Add and Commit Files

Add file:

```bash
git add hello.sh
```

Check status:

```bash
git status
```

Commit:

```bash
git commit -m "added shell script"
```

Check status again:

```bash
git status
```

---

## Screenshot

![Git Add](4.png)

- git add
- git status

![Git Commit](5.png)

- git commit

---

# 5. SSH Authentication

Generate SSH Key

```bash
ssh-keygen
```

Press **Enter** for all default options.

Check SSH directory:

```bash
cd ~/.ssh

ls
```

View Public Key

```bash
cat id_rsa.pub
```

Copy the complete key.

---

## Screenshot

- ssh-keygen
- id_rsa.pub

---

# 6. Add SSH Key to GitHub

1. Login to GitHub
2. Click Profile
3. Settings
4. SSH and GPG Keys
5. New SSH Key
6. Title

```
Playground Key
```

7. Key Type

```
Authentication Key
```

8. Paste the public key

9. Click **Add SSH Key**

10. Verify authentication.

---

## Screenshot

![SSH Key Generation](6.png)


- SSH key added successfully

---

# 7. Create GitHub Repository

![Public SSH Key](7.png)

![SSH Key Added](8.png)

Create Repository

```
shell-script-demo
```

Copy the SSH URL.

Example

```
git@github.com:anandkumarprajap/shell-script-demo.git
```

---

# 8. Connect Local Repository to GitHub

Add remote:

```bash
git remote add origin git@github.com:anandkumarprajap/shell-script-demo.git
```

Verify:

```bash
git remote -v
```

Check branch:

```bash
git branch
```

Output

```
master
```

---

## Screenshot

![Git Remote](9.png)

- git remote -v
- git branch

---

# 9. Push Local Repository to GitHub

Push code:

```bash
git push origin master
```

First time:

```
Are you sure you want to continue connecting?

yes
```

Git uploads all commits to GitHub.

---

## Screenshot

![Git Push](10.png)

- git push
- Push successful

---

# 10. Local → GitHub Workflow

```
Create File
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
GitHub Repository
```

---

# 11. SSH Workflow Diagram

```
+-------------------------+
|      Local Machine      |
|-------------------------|
| hello.sh                |
| Git Repository          |
| Private Key (id_rsa)    |
+-----------+-------------+
            |
            | SSH Authentication
            |
            ▼
+-------------------------+
|        GitHub           |
|-------------------------|
| Public Key              |
| Remote Repository       |
+-------------------------+
```

Private Key → Local Machine

Public Key → GitHub

SSH provides **password-less authentication**.

---

# 12. Pull Changes from GitHub

Run:

```bash
git pull origin master
```

New file downloaded:

```
variable.sh
```

Check files:

```bash
ls
```

Output

```
hello.sh
variable.sh
```

View contents:

```bash
cat variable.sh
```

Output

```
Name "Anand"

Location "Pune"
```

---

## Screenshot

![Repository Before Pull](11.png)

- Repository before pull

![Git Pull](12.png)

- git pull

![Variable Downloaded](13.png)

- variable.sh downloaded

![Variable Script](14.png)

- cat variable.sh

---

# 13. Git Workflow

```
Working Directory
        │
        ▼
git add
        │
        ▼
Staging Area
        │
        ▼
git commit
        │
        ▼
Local Repository
        │
        ▼
git push
        │
        ▼
GitHub Repository
        │
        ▼
git pull
        │
        ▼
Local Repository Updated
```

---

# Commands Used

```bash
mkdir shell-scripts
cd shell-scripts
vim hello.sh
chmod 746 hello.sh
./hello.sh
git init
git config --global user.name
git config --global user.email
git status
git add hello.sh
git commit -m "added shell script"
ssh-keygen
cat ~/.ssh/id_rsa.pub
git remote add origin <SSH_URL>
git remote -v
git branch
git push origin master
git pull origin master
```

---

# Conclusion

In this practical session, we learned:

- Linux file creation
- Shell scripting
- File permissions
- Git initialization
- Git staging
- Git commits
- SSH key generation
- GitHub SSH authentication
- Remote repository setup
- Push local repository to GitHub
- Pull changes from GitHub to local repository

This completes the basic Git and GitHub workflow from **Local → GitHub → Local**.
