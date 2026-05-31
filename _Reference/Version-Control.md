# Version Control

## Definition
Version control is a system that records changes to files over time, allowing you to recall specific versions later. Git is the most widely used version control system in JavaScript development.

## Git Basics

### Repository Setup
```bash
# Initialize a new repository
git init

# Clone an existing repository
git clone https://github.com/user/repo.git

# Clone into specific directory
git clone https://github.com/user/repo.git my-project
```

### Basic Workflow
```bash
# Check status
git status

# Add files to staging
git add filename.js
git add .  # Add all files

# Commit changes
git commit -m "Add new feature"

# View commit history
git log
git log --oneline
```

## Git Commands

### Branching
```bash
# Create new branch
git branch feature-name

# Switch to branch
git checkout feature-name
git switch feature-name

# Create and switch
git checkout -b feature-name

# List branches
git branch
git branch -a  # Include remote

# Delete branch
git branch -d feature-name
```

### Merging
```bash
# Switch to target branch
git checkout main

# Merge feature branch
git merge feature-name

# Merge with commit
git merge --no-ff feature-name

# Abort merge
git merge --abort
```

### Remote Operations
```bash
# Add remote
git remote add origin https://github.com/user/repo.git

# Push changes
git push origin main
git push -u origin main  # Set upstream

# Pull changes
git pull origin main

# Fetch without merge
git fetch origin
```

### Undoing Changes
```bash
# Unstage file
git reset HEAD filename.js

# Undo last commit (keep changes)
git reset --soft HEAD~1

# Undo last commit (discard changes)
git reset --hard HEAD~1

# Revert commit (creates new commit)
git revert commit-hash
```

## .gitignore

```gitignore
# Dependencies
node_modules/
package-lock.json

# Build output
dist/
build/

# Environment variables
.env
.env.local

# IDE files
.vscode/
.idea/
*.swp

# OS files
.DS_Store
Thumbs.db

# Logs
*.log
npm-debug.log*
```

## Git Workflow Patterns

### Feature Branch Workflow
```bash
# Start new feature
git checkout main
git pull
git checkout -b feature/new-feature

# Work on feature
git add .
git commit -m "Add feature"

# Push feature branch
git push -u origin feature/new-feature

# Create pull request on GitHub
# After review, merge to main
```

### Git Flow
```bash
# Main branches
main      # Production code
develop   # Development integration

# Supporting branches
feature/* # New features
release/* # Release preparation
hotfix/*  # Critical fixes
```

## Common Git Operations

```bash
# View differences
git diff
git diff --staged
git diff main..feature

# Stash changes
git stash
git stash list
git stash pop
git stash drop

# Tag releases
git tag v1.0.0
git push origin v1.0.0

# Cherry-pick commits
git cherry-pick commit-hash

# Interactive rebase
git rebase -i HEAD~3
```

## GitHub/GitLab Concepts

```yaml
# Pull Request / Merge Request
- Title: Brief description
- Description: Detailed changes
- Reviewers: Team members
- Labels: bug, feature, documentation
- Assignee: Responsible person
```

## Common Use Cases
- Source code management
- Team collaboration
- Feature development
- Bug tracking
- Code review
- Release management

## Common Mistakes

| Mistake | Solution |
|---------|----------|
| Committing node_modules | Use .gitignore |
| Unclear commit messages | Write descriptive messages |
| Working on main branch | Use feature branches |
| Not pulling before push | Always pull first |
| Force pushing | Avoid unless necessary |

## Quick Revision Summary
- Git tracks file changes over time
- Use branches for isolated development
- Commit often with clear messages
- Pull before pushing to avoid conflicts
- Use .gitignore to exclude files
- Pull requests enable code review
- Never commit secrets or credentials

## Related Topics
- [[GitHub]]
- [[GitLab]]
- [[Pull-Requests]]
- [[Branching-Strategy]]
- [[Merge-Conflicts]]
- [[npm]]
- [[package-json]]
