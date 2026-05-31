# How to Use Git

## Definition

Git is a **distributed version control system** for tracking changes in code and enabling collaboration between developers.

## Essential Git Commands

```bash
# Initialize repository
git init

# Clone repository
git clone https://github.com/user/repo.git

# Check status
git status

# Add files to staging
git add filename.js
git add .  # Add all files

# Commit changes
git commit -m "Add new feature"

# Push to remote
git push origin main

# Pull from remote
git pull origin main
```

## Branch Management

```bash
# List branches
git branch
git branch -a  # Include remote branches

# Create branch
git branch feature/new-login

# Switch branch
git checkout feature/new-login
git switch feature/new-login  # Modern alternative

# Create and switch
git checkout -b feature/new-login

# Merge branch
git checkout main
git merge feature/new-login

# Delete branch
git branch -d feature/new-login
git branch -D feature/new-login  # Force delete
```

## Commit Best Practices

```bash
# Good commit messages
git commit -m "Add user authentication"
git commit -m "Fix login form validation"
git commit -m "Update README with installation steps"

# Bad commit messages
git commit -m "fix"
git commit -m "updates"
git commit -m "asdfgh"
```

## Stashing Changes

```bash
# Save changes temporarily
git stash

# List stashes
git stash list

# Apply stash
git stash apply
git stash pop  # Apply and remove

# Drop stash
git stash drop
```

## Undoing Changes

```bash
# Undo working directory changes
git checkout -- filename.js
git restore filename.js  # Modern alternative

# Unstage file
git reset HEAD filename.js
git restore --staged filename.js  # Modern

# Amend last commit
git commit --amend -m "Updated commit message"

# Revert commit (creates new commit)
git revert commit-hash

# Reset to specific commit
git reset --soft HEAD~1  # Keep changes staged
git reset --mixed HEAD~1 # Keep changes unstaged
git reset --hard HEAD~1  # Discard changes
```

## Viewing History

```bash
# View commit history
git log
git log --oneline  # Compact view
git log --graph    # Visual branch graph

# View specific file changes
git log -p filename.js

# See who changed what
git blame filename.js

# Compare branches
git diff main..feature
```

## Remote Repositories

```bash
# List remotes
git remote -v

# Add remote
git remote add origin https://github.com/user/repo.git

# Remove remote
git remote remove origin

# Fetch from remote
git fetch origin

# Pull with rebase
git pull --rebase origin main
```

## .gitignore

```bash
# .gitignore
node_modules/
.env
dist/
*.log
.DS_Store
.vscode/
coverage/
```

## Git Workflow

```bash
# 1. Create branch for feature
git checkout -b feature/add-login

# 2. Make changes and commit
git add .
git commit -m "Add login form"

# 3. Push branch
git push origin feature/add-login

# 4. Create Pull Request on GitHub

# 5. After review, merge to main
git checkout main
git merge feature/add-login

# 6. Clean up
git branch -d feature/add-login
git push origin --delete feature/add-login
```

## Common Mistakes

```bash
# BAD: Committing secrets
git add .env
git commit -m "Add config"

# GOOD: Always use .gitignore
echo ".env" >> .gitignore

# BAD: Committing node_modules
git add node_modules/
git commit -m "Add dependencies"

# GOOD: Install after clone
# Don't commit node_modules

# BAD: Force pushing to main
git push --force origin main

# GOOD: Use feature branches
git push origin feature/my-feature
```

## Quick Revision

- `git init` - Initialize repository
- `git add` - Stage changes
- `git commit` - Save changes
- `git push` - Upload to remote
- `git pull` - Download from remote
- `git branch` - Manage branches
- `git merge` - Combine branches
- Always write meaningful commit messages
- Never commit secrets or node_modules

---

## Related Topics

- [[What-is-Git]] - Git overview
- [[What-is-CICD]] - CI/CD pipelines
- [[What-is-GitActions]] - GitHub Actions
- [[What-is-CodeReview]] - Code review
- [[What-is-Deployment]] - Deployment