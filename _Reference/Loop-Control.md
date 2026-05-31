# Loop Control

## Definition

Loop control uses **break and continue** to manage loop execution.

## break

```javascript
for (let i = 0; i < 10; i++) {
    if (i === 5) break;
    console.log(i); // 0, 1, 2, 3, 4
}
```

## continue

```javascript
for (let i = 0; i < 10; i++) {
    if (i % 2 === 0) continue;
    console.log(i); // 1, 3, 5, 7, 9
}
```

## Quick Revision

- `break` exits loop
- `continue` skips iteration
- Labels for nested loops

---

## Related Topics

- [[What-is-BreakContinue]] - [[What-is-BreakContinue|Break/continue]]
- [[Use-BreakContinue]] - [[Use-BreakContinue|Using break/continue]]
- [[Loop-Control]] - [[Loop-Control|Loop control]]
- [[What-is-ForLoop]] - [[What-is-ForLoop|For loop]]
