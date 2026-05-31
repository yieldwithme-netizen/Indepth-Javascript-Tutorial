# Loops

## Definition

Loops **repeat code** until a condition is met.

## Types

```javascript
// for loop
for (let i = 0; i < 5; i++) {
    console.log(i);
}

// while loop
let i = 0;
while (i < 5) {
    console.log(i);
    i++;
}

// do-while loop
let i = 0;
do {
    console.log(i);
    i++;
} while (i < 5);

// for...of
for (const item of [1, 2, 3]) {
    console.log(item);
}

// for...in
for (const key in { a: 1, b: 2 }) {
    console.log(key);
}
```

## Quick Revision

- for: known iterations
- while: unknown iterations
- do-while: at least once
- for...of: iterate values
- for...in: iterate keys

---

## Related Topics

- [[What-is-ForLoop]] - [[What-is-ForLoop|For loop]]
- [[What-is-WhileLoop]] - [[What-is-WhileLoop|While loop]]
- [[What-is-DoWhile]] - [[What-is-DoWhile|Do-while]]
- [[Loops]] - [[Loops|Loops]]
