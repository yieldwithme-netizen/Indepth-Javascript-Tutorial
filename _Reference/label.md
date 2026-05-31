# Labels in JavaScript

## Definition

Labels in JavaScript are identifiers followed by a colon (`label:`) that can be applied to statements. They are primarily used with `break` and `continue` statements to control the flow of nested loops. Labels provide a way to specify which loop to break out of or continue when working with multiple nested loops.

## Syntax

```javascript
labelName: statement
```

## Code Examples

### Basic Label with Break

```javascript
outerLoop: for (let i = 0; i < 5; i++) {
  for (let j = 0; j < 5; j++) {
    if (i === 2 && j === 3) {
      console.log(`Breaking at i=${i}, j=${j}`);
      break outerLoop;
    }
    console.log(`i=${i}, j=${j}`);
  }
}
```

### Label with Continue

```javascript
outerLoop: for (let i = 0; i < 3; i++) {
  for (let j = 0; j < 3; j++) {
    if (j === 1) {
      continue outerLoop;
    }
    console.log(`i=${i}, j=${j}`);
  }
}
// Output: i=0 j=0, i=1 j=0, i=2 j=0
```

### Breaking Nested While Loops

```javascript
let i = 0;
outer: while (i < 5) {
  let j = 0;
  while (j < 5) {
    if (i === 3 && j === 2) {
      break outer;
    }
    console.log(`i=${i}, j=${j}`);
    j++;
  }
  i++;
}
```

### Breaking Nested For-of Loops

```javascript
const matrix = [
  [1, 2, 3],
  [4, 5, 6],
  [7, 8, 9],
];

search: for (const row of matrix) {
  for (const cell of row) {
    if (cell === 5) {
      console.log("Found 5!");
      break search;
    }
  }
}
```

### Labels with Switch Statements

```javascript
// Labels can also be used with switch statements
loop: for (let i = 0; i < 3; i++) {
  switch (i) {
    case 0:
      console.log("Case 0");
      continue loop;
    case 1:
      console.log("Case 1");
      continue loop;
    case 2:
      console.log("Case 2");
      break loop;
  }
}
```

### Complex Nested Loop Control

```javascript
function findIn3D(array3D, target) {
  depth: for (let d = 0; d < array3D.length; d++) {
    row: for (let r = 0; r < array3D[d].length; r++) {
      col: for (let c = 0; c < array3D[d][r].length; c++) {
        if (array3D[d][r][c] === target) {
          console.log(`Found at [${d}][${r}][${c}]`);
          break depth;
        }
      }
    }
  }
}

const data3D = [
  [[1, 2], [3, 4]],
  [[5, 6], [7, 8]],
];

findIn3D(data3D, 5); // "Found at [1][0][0]"
```

## Common Use Cases

- **Nested loop control** — Break out of multiple nested loops simultaneously
- **Search algorithms** — Exit deep iterations when target is found
- **Matrix processing** — Control flow in multi-dimensional data
- **State machines** — Complex iteration patterns with early exit
- **Game loops** — Managing nested game logic cycles

## Common Mistakes

```javascript
// Mistake 1: Using labels in non-loop contexts (invalid)
// for (let i = 0; i < 5; i++) {
//   myLabel: console.log(i); // SyntaxError
// }

// Mistake 2: Referencing a label that doesn't exist
// break nonExistentLabel; // ReferenceError

// Mistake 3: Using labels unnecessarily
// Bad: labels for simple loops
// outer: for (let i = 0; i < 10; i++) {
//   break outer; // Works but unnecessary
// }

// Better: extract to a function for clarity
function findValue(arr, target) {
  for (let i = 0; i < arr.length; i++) {
    if (arr[i] === target) {
      return i;
    }
  }
  return -1;
}

// Mistake 4: Label names conflicting with variables
const loop = 10; // This shadows any label named 'loop'
```

## Related Topics

- [[For-Loop]]
- [[Loops]]
- [[Condition-Statements]]
- [[Function-Scope]]
- [[Control-Statements]]
- [[Switch-Statement]]

## Quick Revision

| Aspect | Description |
|--------|-------------|
| Syntax | `labelName: statement` |
| `break label` | Exits the labeled statement |
| `continue label` | Skips to next iteration of labeled loop |
| Works with | `for`, `while`, `do-while`, `switch` |
| Nesting | Most useful for deeply nested loops |
| Best Practice | Use sparingly; prefer function extraction |
