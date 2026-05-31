# What is a [[label]] [[statement]]?

## Definition

A [[label]] is a **named reference** to a [[loop]] that lets you control nested loops with `break` and `continue`.

## Basic Syntax

```javascript
labelName: for (let i = 0; i < 5; i++) {
    // code
}
```

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
// - Simple loops (use break/continue without label)
// - Hard to understand code
```

## Quick Revision

- [[label|Labels]] name loops for control
- `break labelName` exits the labeled loop
- `continue labelName` continues to next [[iteration]] of labeled loop
- Use for nested loop control
- Avoid overusing (hard to read)

---

## Related Topics

- [[What-is-BreakContinue]] - [[break]] and [[continue]]
- [[Use-BreakContinue]] - Using [[break]]/[[continue]]
- [[Use-Label]] - Using [[label|labels]]
- [[What-is-ForLoop]] - [[for]] loops
