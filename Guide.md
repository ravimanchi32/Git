# 🚀 Complete Git & GitHub Learning Guide (Beginner to Advanced)

## 📌 Overview

This guide helps you learn:

- Git basics
- GitHub workflow
- Branching & collaboration
- Pull Requests
- Merge conflicts
- Real-world DevOps workflow
- Best practices used in companies

---

# 🧠 What is Git?

Git is a **version control system**.

It helps you:
- Track code changes
- Collaborate with teams
- Roll back changes
- Maintain history

---

# 🧠 What is GitHub?

GitHub is a **cloud platform for Git repositories**.

Git = tool  
GitHub = hosting platform

---

# 🛠 Step 1: Install Git

## Windows
Download:
[Git Official Website](https://git-scm.com/downloads?utm_source=chatgpt.com)

## Linux

```bash
sudo apt update
sudo apt install git
```

## Mac

```bash
brew install git
```

---

# 🔍 Step 2: Verify Installation

```bash
git --version
```

Example:

```bash
git version 2.44.0
```

---

# ⚙️ Step 3: Configure Git

Set username:

```bash
git config --global user.name "Your Name"
```

Set email:

```bash
git config --global user.email "your@email.com"
```

Check config:

```bash
git config --list
```

---

# 📂 Step 4: Create Your First Repository

Create folder:

```bash
mkdir my-project
cd my-project
```

Initialize Git:

```bash
git init
```

---

# 📄 Step 5: Create First File

```bash
touch app.py
```

Add content:

```python
print("Hello Git")
```

---

# 🔍 Step 6: Check Git Status

```bash
git status
```

Shows:
- Modified files
- Untracked files
- Staged files

---

# ➕ Step 7: Add Files to Staging Area

Add single file:

```bash
git add app.py
```

Add all files:

```bash
git add .
```

---

# 💾 Step 8: Commit Changes

```bash
git commit -m "Initial commit"
```

A commit is a snapshot of your code.

---

# 📜 Step 9: View Commit History

```bash
git log
```

Compact view:

```bash
git log --oneline
```

---

# 🔁 Step 10: Modify Files & Track Changes

Edit file:

```python
print("Git is awesome")
```

Check difference:

```bash
git diff
```

---

# 🌿 Step 11: Understanding Branches

Branches help developers work independently.

Check current branch:

```bash
git branch
```

Create new branch:

```bash
git branch feature-login
```

Switch branch:

```bash
git checkout feature-login
```

Or:

```bash
git switch feature-login
```

---

# 🔀 Step 12: Merge Branches

Go back to main:

```bash
git checkout main
```

Merge branch:

```bash
git merge feature-login
```

---

# ⚠️ Step 13: Merge Conflicts

Happens when same lines are modified in multiple branches.

Fix:
- Open file
- Remove conflict markers
- Commit again

---

# 🌐 Step 14: Create GitHub Repository

Go to:
[GitHub](https://github.com?utm_source=chatgpt.com)

Create:
- New Repository

---

# 🔗 Step 15: Connect Local Repo to GitHub

```bash
git remote add origin https://github.com/username/repo.git
```

Verify:

```bash
git remote -v
```

---

# ⬆️ Step 16: Push Code to GitHub

```bash
git push -u origin main
```

---

# ⬇️ Step 17: Clone Repository

```bash
git clone https://github.com/username/repo.git
```

---

# 🔄 Step 18: Pull Latest Changes

```bash
git pull origin main
```

---

# 🚀 Step 19: Real Team Workflow

Typical workflow:

```text
Create Branch
      ↓
Write Code
      ↓
Commit Changes
      ↓
Push Branch
      ↓
Create Pull Request
      ↓
Code Review
      ↓
Merge to Main
```

---

# 🔍 Step 20: Pull Requests (PR)

Pull Request is used for:
- Code review
- Collaboration
- CI/CD validation

---

# 🧪 Step 21: Git Ignore

Create `.gitignore`

Example:

```gitignore
node_modules/
.env
*.log
```

---

# 🔐 Step 22: SSH Authentication (Recommended)

Generate SSH key:

```bash
ssh-keygen -t rsa -b 4096
```

Add key to GitHub.

Test:

```bash
ssh -T git@github.com
```

---

# 🔄 Step 23: Undo Changes

Undo unstaged changes:

```bash
git checkout -- file.txt
```

Undo staged changes:

```bash
git reset HEAD file.txt
```

---

# ⏪ Step 24: Revert Commit

```bash
git revert <commit-id>
```

---

# 🧹 Step 25: Remove File from Git

```bash
git rm file.txt
```

---

# 📦 Step 26: Stash Changes

Temporarily save changes:

```bash
git stash
```

Restore:

```bash
git stash pop
```

---

# 🏷 Step 27: Git Tags

Create tag:

```bash
git tag v1.0
```

Push tags:

```bash
git push origin --tags
```

---

# 🔍 Step 28: Inspect Remote Branches

```bash
git branch -r
```

---

# 🚀 Step 29: Cherry Pick Commit

Apply specific commit:

```bash
git cherry-pick <commit-id>
```

---

# ⚡ Step 30: Rebase vs Merge

## Merge
- Preserves history

## Rebase
- Cleaner linear history

Example:

```bash
git rebase main
```

---

# 🧠 Important Git Concepts

| Concept | Meaning |
|---|---|
| Repository | Project storage |
| Commit | Snapshot |
| Branch | Parallel work |
| Merge | Combine branches |
| Remote | GitHub repository |
| Pull Request | Code review request |

---

# 🚀 Real-World GitHub Workflow

## Feature Development

```text
main branch
   ↓
Create feature branch
   ↓
Develop feature
   ↓
Push branch
   ↓
Create PR
   ↓
Review
   ↓
Merge
```

---

# 🔥 Common Git Commands Cheat Sheet

| Command | Purpose |
|---|---|
| git init | Initialize repo |
| git status | Check changes |
| git add . | Stage files |
| git commit | Save snapshot |
| git push | Upload changes |
| git pull | Download changes |
| git clone | Copy repo |
| git branch | Manage branches |
| git merge | Merge branch |
| git log | View history |

---

# ⚠️ Common Mistakes Beginners Make

- Committing secrets/passwords
- Working directly on main branch
- Forgetting to pull latest code
- Large commits instead of small commits

---

# 🔐 GitHub Best Practices

- Use meaningful commit messages
- Create feature branches
- Use Pull Requests
- Protect main branch
- Enable CI/CD checks

---

# 📚 Recommended Learning Path

## Beginner
- Git basics
- Commit/push/pull
- Branching

## Intermediate
- PR workflow
- Merge conflicts
- Stash/rebase

## Advanced
- CI/CD integration
- GitHub Actions
- Monorepos
- Advanced Git debugging

---

# 🚀 Next Topics to Learn

- GitHub Actions
- Docker
- Kubernetes
- Terraform
- CI/CD pipelines

---

# 📌 Conclusion

Git and GitHub are foundational skills for:
- Developers
- DevOps Engineers
- Cloud Engineers
- SREs

Mastering Git improves:
- Collaboration
- Deployment workflow
- Code management
- Production stability

---

# ⭐ Support

- Star ⭐ the repo
- Share with others
- Follow for more DevOps content

---
