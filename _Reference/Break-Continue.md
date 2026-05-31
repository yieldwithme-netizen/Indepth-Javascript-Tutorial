# Break/Continue

## Definition
`break` and `continue` are loop control statements. `break` exits the loop entirely, while `continue` skips the current iteration and moves to the next one.

## Syntax
```javascript
break;
continue;
```

## Code Examples

### Basic Break
```javascript
for (let i = 0; i < 10; i++) {
  if (i === 5) {
    break;
  }
  console.log(i);
}
// Output: 0, 1, 2, 3, 4
```

### Basic Continue
```javascript
for (let i = 0; i < 10; i++) {
  if (i % 2 === 0) {
    continue; // Skip even numbers
  }
  console.log(i);
}
// Output: 1, 3, 5, 7, 9
```

### Break in While Loop
```javascript
let count = 0;
while (count < 100) {
  if (count === 5) break;
  count++;
}
console.log(count); // 5
```

### Continue in While Loop
```javascript
let i = 0;
while (i < 10) {
  i++;
  if (i % 2 === 0) continue;
  console.log(i);
}
// Output: 1, 3, 5, 7, 9
```

### Labeled Break
```javascript
outer: for (let i = 0; i < 5; i++) {
  for (let j = 0; j < 5; j++) {
    if (j === 3) break outer;
    console.log(`i: ${i}, j: ${j}`);
  }
}
// Stops completely when j === 3
```

### Labeled Continue
```javascript
outer: for (let i = 0; i < 3; i++) {
  for (let j = 0; j < 3; j++) {
    if (j === 1) continue outer;
    console.log(`i: ${i}, j: ${j}`);
  }
}
// Output: i:0 j:0, i:1 j:0, i:2 j:0
```

### Break in Switch
```javascript
switch (value) {
  case 1:
    console.log('One');
    break;
  case 2:
    console.log('Two');
    break;
  default:
    console.log('Other');
}
```

### Find in Array
```javascript
const numbers = [1, 3, 5, 8, 9, 12];
let found;

for (const num of numbers) {
  if (num % 4 === 0) {
    found = num;
    break;
  }
}

console.log(found); // 8
```

### Skip Processing
```javascript
const items = ['a', null, 'b', undefined, 'c'];

for (const item of items) {
  if (item === null || item === undefined) {
    continue; // Skip invalid items
  }
  processItem(item);
}
```

### Nested Loops
```javascript
for (let i = 0; i < 5; i++) {
  for (let j = 0; j < 5; j++) {
    if (i + j === 6) {
      break; // Only breaks inner loop
    }
    console.log(`${i}, ${j}`);
  }
}
```

## Common Use Cases
- Early termination of loops
- Skipping invalid data
- Search operations
- Breaking nested loops with labels

## Common Mistakes
- **Break without loop**: `break` only works in loops/switch
- **Forgetting break in switch**: Causes fall-through
- **Label misuse**: Labels should be used sparingly
- **Infinite loops**: Ensure break condition is reachable

## Related Topics
- [[Loops]]
- [[For-Loop]]
- [[While-Loop]]
- [[Switch]]
- [[Loop-Control]]

## Quick Revision
- `break` exits the loop completely
- `continue` skips to next iteration
- Use labels for nested loop control
- `break` is required in switch cases
- Both work in `for`, `while`, `do-while` loops
