# Refactoring

## Definition

Refactoring **improves code structure** without changing behavior.

## Techniques

```javascript
// Extract function
function processUser(user) {
    validateUser(user);
    saveUser(user);
    sendEmail(user);
}

// Rename
const userAge = 30; // instead of x

// Remove duplication
function add(a, b) { return a + b; }
```

## Quick Revision

- Improve code without changing behavior
- Extract functions
- Rename variables
- Remove duplication
- Use tests to verify

---

## Related Topics

- [[What-is-CleanCode]] - [[What-is-CleanCode|Clean code]]
- [[Refactoring]] - [[Refactoring|Refactoring]]
- [[What-is-DRY]] - [[What-is-DRY|DRY]]
- [[What-is-CodeReview]] - [[What-is-CodeReview|Code review]]
