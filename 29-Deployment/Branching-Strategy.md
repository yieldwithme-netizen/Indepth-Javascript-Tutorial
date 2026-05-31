# Branching Strategy

## Definition

Branching strategy defines **how branches are used** in Git workflow.

## Common Strategies

```bash
# Git Flow
main (production)
  └── develop (integration)
       ├── feature/login
       ├── feature/signup
       └── hotfix/bug-123

# GitHub Flow
main (production)
  └── feature-branch → PR → merge
```

## Quick Revision

- Git Flow: develop, feature, release, hotfix
- GitHub Flow: simple, branch + PR
- Feature branches for new work
- Main always deployable

---

## Related Topics

- [[What-is-Git]] - [[What-is-Git|Git]]
- [[Use-Git]] - [[Use-Git|Using Git]]
- [[Branching-Strategy]] - [[Branching-Strategy|Branching strategy]]
- [[Git-Workflow]] - [[Git-Workflow|Git workflow]]
