# What is Git?

## Definition

Git is a **version control system** for tracking changes in code.

## Basic Commands

```bash
# Initialize
git init

# Clone
git clone https://github.com/user/repo.git

# Stage changes
git add file.js
git add .

# Commit
git commit -m "Add feature"

# Push
git push origin main

# Pull
git pull origin main

# Status
git status

# Log
git log --oneline
```

## Branches

```bash
# Create branch
git branch feature

# Switch branch
git checkout feature

# Create and switch
git checkout -b feature

# Merge
git checkout main
git merge feature

# Delete branch
git branch -d feature
```

## Workflow

```bash
# 1. Create branch
git checkout -b feature

# 2. Make changes
# 3. Stage and commit
git add .
git commit -m "Add feature"

# 4. Push branch
git push origin feature

# 5. Create Pull Request
# 6. Merge after review
```

## Quick Revision

- Git = version control
- Commands: init, add, commit, push, pull
- Branches for features
- Merge to combine changes
- Use for: collaboration, history

---

## Related Topics

- [[What-is-Git]] - Git overview
- [[Use-Git]] - Using Git
- [[What-is-CodeReview]] - Code review
- [[What-is-CICD]] - CI/CD
- [[Setup-CICD]] - Setting up CI/CD
