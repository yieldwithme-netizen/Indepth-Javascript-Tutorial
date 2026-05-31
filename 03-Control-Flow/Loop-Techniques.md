# Loop Techniques

## Definition

Loop techniques are **patterns** for efficient looping.

## Techniques

```javascript
// Cache length
for (let i = 0, len = arr.length; i < len; i++) { }

// Reverse iteration
for (let i = arr.length - 1; i >= 0; i--) { }

// Break early
for (const item of arr) {
    if (condition) break;
}

// Skip iterations
for (let i = 0; i < 10; i++) {
    if (i % 2 === 0) continue;
}
```

## Quick Revision

- Cache array length
- Reverse for efficiency
- Break for early exit
- Continue to skip iterations

---

## Related Topics

- [[What-is-ForLoop]] - [[What-is-ForLoop|For loop]]
- [[Loop-Control]] - [[Loop-Control|Loop control]]
- [[Loop-Techniques]] - [[Loop-Techniques|Loop techniques]]
- [[Use-BreakContinue]] - [[Use-BreakContinue|Break/continue]]
