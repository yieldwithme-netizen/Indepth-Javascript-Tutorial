# What is a [[for]] [[loop]]?

## Definition

A [[for]] [[loop]] **repeats code** a specific number of times.

## Basic Syntax

```javascript
for (initialization; condition; increment) {
    // code to repeat
}
```

## How It Works

```
1. Run initialization (once)
2. Check condition
3. If true, run code
4. Run increment
5. Go to step 2
```

## Examples

```javascript
// Basic loop
for (let i = 0; i < 5; i++) {
    console.log(i); // 0, 1, 2, 3, 4
}

// Counting down
for (let i = 5; i > 0; i--) {
    console.log(i); // 5, 4, 3, 2, 1
}

// Loop through array
const fruits = ["apple", "banana", "orange"];
for (let i = 0; i < fruits.length; i++) {
    console.log(fruits[i]);
}

// Skip by 2
for (let i = 0; i < 10; i += 2) {
    console.log(i); // 0, 2, 4, 6, 8
}
```

## Common Patterns

```javascript
// Sum numbers
let sum = 0;
for (let i = 1; i <= 10; i++) {
    sum += i;
}
console.log(sum); // 55

// Find largest
const nums = [5, 2, 9, 1, 7];
let max = nums[0];
for (let i = 1; i < nums.length; i++) {
    if (nums[i] > max) {
        max = nums[i];
    }
}
console.log(max); // 9
```

## Common Mistakes

```javascript
// ❌ Wrong: i++ after code (infinite loop)
for (let i = 0; i < 5; ) {
    console.log(i);
    // forgot i++!
}

// ❌ Wrong: Off-by-one error
for (let i = 0; i <= 5; i++) { // runs 6 times (0,1,2,3,4,5)
    console.log(i);
}

// ✅ Right: Use < for exact count
for (let i = 0; i < 5; i++) { // runs 5 times (0,1,2,3,4)
    console.log(i);
}
```

## Quick Revision

- [[for]] loop: `for (init; condition; increment)`
- Use when you know the number of [[iteration|iterations]]
- `let i = 0` starts at 0
- `i < length` for [[array|array]] (0 to length-1)
- Always increment to avoid infinite loops

---

## Related Topics

- [[What-is-WhileLoop]] - [[while]] loops
- [[What-is-DoWhile]] - [[do]]-[[while]] loops
- [[Write-ForLoop]] - Writing [[for]] loops
- [[Use-ForIn]] - [[for]]...[[in]] loop
- [[Use-ForOf]] - [[for]]...[[of]] loop
