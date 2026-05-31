# For Loop in JavaScript

## Definition

The `for` loop is a control flow statement that allows code to be executed repeatedly based on a given condition. It's one of the most fundamental iteration constructs in JavaScript.

## Syntax

```javascript
for (initialization; condition; final-expression) {
  // code block to be executed
}
```

- **initialization**: Executed once before the loop starts (typically declares a counter variable)
- **condition**: Evaluated before each iteration; loop stops when false
- **final-expression**: Executed after each iteration (typically increments/decrements the counter)

## Code Examples

### Basic For Loop

```javascript
for (let i = 0; i < 5; i++) {
  console.log(i);
}
// Output: 0, 1, 2, 3, 4
```

### Looping Through an Array

```javascript
const fruits = ["apple", "banana", "cherry", "date"];

for (let i = 0; i < fruits.length; i++) {
  console.log(fruits[i]);
}
// Output: apple, banana, cherry, date
```

### Counting Down

```javascript
for (let i = 10; i > 0; i--) {
  console.log(i);
}
// Output: 10, 9, 8, ..., 1
```

### Nested For Loops

```javascript
for (let i = 1; i <= 3; i++) {
  for (let j = 1; j <= 3; j++) {
    console.log(`${i} x ${j} = ${i * j}`);
  }
}
```

### Using break and continue

```javascript
// break - exit the loop entirely
for (let i = 0; i < 10; i++) {
  if (i === 5) break;
  console.log(i);
}
// Output: 0, 1, 2, 3, 4

// continue - skip current iteration
for (let i = 0; i < 10; i++) {
  if (i % 2 === 0) continue;
  console.log(i);
}
// Output: 1, 3, 5, 7, 9
```

### Infinite Loop

```javascript
// Be cautious - use break to exit
for (;;) {
  console.log("This runs forever unless broken");
  break;
}
```

## Common Use Cases

- Iterating over arrays or array-like objects
- Repeating an action a specific number of times
- Processing collections of data
- Implementing counting mechanisms
- Creating patterns (nested loops)

## Common Mistakes

1. **Off-by-one errors**: Using `<` vs `<=` can cause missing or extra iterations
2. **Modifying the loop variable inside the loop**: Can cause unexpected behavior
3. **Using var instead of let**: `var` creates function-scoped variable that leaks
4. **Infinite loops**: Forgetting to update the counter or having an always-true condition

```javascript
// Bad practice - using var
for (var i = 0; i < 3; i++) {}
console.log(i); // 3 - leaks outside loop

// Good practice - using let
for (let i = 0; i < 3; i++) {}
console.log(i); // ReferenceError - i is block-scoped
```

## Related Topics

- [[While-Loop]] - Alternative loop syntax
- [[Do-While-Loop]] - Loop that always executes at least once
- [[For-Of-Loop]] - Iterating over iterable objects
- [[For-In-Loop]] - Iterating over object properties
- [[Array-Methods]] - Higher-order iteration methods (map, filter, reduce)
- [[Break-Continue]] - Loop control statements

## Quick Revision Summary

| Concept | Description |
|---------|-------------|
| Syntax | `for (init; condition; update) { body }` |
| Execution order | init → condition → body → update → condition... |
| Loop stops | When condition evaluates to false |
| `break` | Exits the loop immediately |
| `continue` | Skips to next iteration |
| Use `let` | Prefer `let` over `var` for block scoping |
