# How to Use Labels

## Breaking Nested Loops

```javascript
// Without label (only breaks inner loop)
for (let i = 0; i < 5; i++) {
    for (let j = 0; j < 5; j++) {
        if (j === 2) break; // only breaks inner
        console.log(`${i},${j}`);
    }
}

// With label (breaks both loops)
outer: for (let i = 0; i < 5; i++) {
    for (let j = 0; j < 5; j++) {
        if (j === 2) break outer; // breaks outer
        console.log(`${i},${j}`);
    }
}
```

## Continuing Outer Loop

```javascript
// Skip to next iteration of outer loop
outer: for (let i = 0; i < 3; i++) {
    for (let j = 0; j < 3; j++) {
        if (j === 1) continue outer;
        console.log(`${i},${j}`);
    }
}
// Output:
// 0,0
// 1,0
// 2,0
```

## When to Use

```javascript
// ✅ Use labels when:
// - Breaking out of nested loops
// - Continuing outer loop
// - Complex loop control

// ❌ Avoid when:
// - Simple loops
// - Hard to understand code
```

## Quick Revision

- Labels name loops for control
- `break labelName` exits the labeled loop
- `continue labelName` continues to next iteration of labeled loop
- Use for nested loop control
- Avoid overusing (hard to read)

---

## Related Topics

- [[What-is-Label]] - [[What-is-Label|Labels]] overview
- [[Use-BreakContinue]] - [[Use-BreakContinue|Break/continue]]
- [[Write-ForLoop]] - [[Write-ForLoop|For loops]]
- [[Write-WhileLoop]] - [[Write-WhileLoop|While loops]]
