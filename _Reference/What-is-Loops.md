# JavaScript Loops

## Definition
Loops are control structures that repeat a block of code until a specified condition is met. They are essential for iterating over data structures and executing repetitive tasks.

## Types of Loops

### for loop
```javascript
for (let i = 0; i < 5; i++) {
  console.log(i); // 0, 1, 2, 3, 4
}
```

### while loop
```javascript
let count = 0;
while (count < 5) {
  console.log(count); // 0, 1, 2, 3, 4
  count++;
}
```

### do...while loop
```javascript
let num = 0;
do {
  console.log(num); // 0, 1, 2, 3, 4
  num++;
} while (num < 5);
```

### for...in loop (objects)
```javascript
const person = { name: 'John', age: 30, city: 'NYC' };
for (let key in person) {
  console.log(`${key}: ${person[key]}`);
}
```

### for...of loop (arrays, strings)
```javascript
const colors = ['red', 'green', 'blue'];
for (let color of colors) {
  console.log(color); // 'red', 'green', 'blue'
}
```

### forEach method
```javascript
const numbers = [1, 2, 3, 4, 5];
numbers.forEach((num, index) => {
  console.log(`${index}: ${num}`);
});
```

## Loop Control
```javascript
// break - exits the loop
for (let i = 0; i < 10; i++) {
  if (i === 5) break;
  console.log(i); // 0, 1, 2, 3, 4
}

// continue - skips current iteration
for (let i = 0; i < 10; i++) {
  if (i % 2 === 0) continue;
  console.log(i); // 1, 3, 5, 7, 9
}
```

## Common Use Cases
- Iterating over arrays and collections
- Repeating actions a specific number of times
- Processing data in objects
- Polling or waiting for conditions
- Building strings or aggregating values

## Common Mistakes
- Infinite loops (forgetting to update the condition)
- Off-by-one errors in loop bounds
- Modifying array while iterating
- Using `for...in` on arrays (use `for...of` instead)
- Variable scope issues with `var` in loops

## Quick Revision Summary
- `for` - when you know the number of iterations
- `while` - when you don't know when to stop
- `do...while` - execute at least once
- `for...in` - iterate over object properties
- `for...of` - iterate over array/string values
- Use `break` to exit and `continue` to skip

## Related Topics
- [[Arrays]]
- [[Objects]]
- [[Functions]]
- [[Array-Methods]]
- [[Iterators-and-Generators]]
