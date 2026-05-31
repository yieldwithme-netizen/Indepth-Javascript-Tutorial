# Git Branching

## Definition

Git branching creates **parallel versions** of code.

## Basic Commands

```bash
# Create branch
git branch feature

# Switch branch
git checkout feature

# Create and switch
git checkout -b feature

# List branches
git branch -a

# Delete branch
git branch -d feature

# Merge branch
git checkout main
git merge feature
```

## Quick Revision

- Branch = independent line of development
- `git branch` to create
- `git checkout` to switch
- `git merge` to combine
- Keep main branch clean

---

## Related Topics

- [[What-is-Git]] - [[What-is-Git|Git]]
- [[Use-Git]] - [[Use-Git|Using Git]]
- [[Git-Branching]] - [[Git-Branching|Git branching]]
- [[Branching-Strategy]] - [[Branching-Strategy|Branching strategy]]
