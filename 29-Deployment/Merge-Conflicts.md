# Merge Conflicts

## Definition

Merge conflicts occur when **competing changes** exist.

## Resolution

```bash
# Open conflicted file
<<<<<<< HEAD
Your changes
=======
Incoming changes
>>>>>>> branch-name

# Choose one, delete markers, then:
git add .
git commit -m "Resolve conflict"
```

## Quick Revision

- Conflicts = competing changes
- Edit file to resolve
- Markers: <<<<<<<, =======, >>>>>>>
- git add and commit when done

---

## Related Topics

- [[What-is-Git]] - [[What-is-Git|Git]]
- [[Merge-Conflicts]] - [[Merge-Conflicts|Merge conflicts]]
- [[Use-Git]] - [[Use-Git|Using Git]]
- [[Git-Workflow]] - [[Git-Workflow|Git workflow]]
