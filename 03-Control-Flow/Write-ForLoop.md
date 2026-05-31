# How to Write For Loops

## Basic Syntax

```javascript
for (initialization; condition; increment) {
    // code to repeat
}
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

## Quick Revision

- For loop: `for (init; condition; increment)`
- Use when you know the number of iterations
- `let i = 0` starts at 0
- `i < length` for arrays (0 to length-1)
- Always increment to avoid infinite loops

---

## Related Topics

- [[What-is-ForLoop]] - [[What-is-ForLoop|For loop]] overview
- [[Write-WhileLoop]] - [[Write-WhileLoop|While loops]]
- [[Write-DoWhile]] - [[Write-DoWhile|Do-while loops]]
- [[Use-ForIn]] - [[Use-ForIn|For...in]]
- [[Use-ForOf]] - [[Use-ForOf|For...of]]
