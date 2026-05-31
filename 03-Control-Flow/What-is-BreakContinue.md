# What is [[break]] and [[continue]]?

## Definition

- `break` **exits** the [[loop]] immediately
- `continue` **skips** to the next [[iteration]]

## [[break]]

```javascript
// Exit loop when condition met
for (let i = 0; i < 10; i++) {
    if (i === 5) {
        break; // exits when i is 5
    }
    console.log(i); // 0, 1, 2, 3, 4
}

// Find first match
const nums = [1, 3, 5, 8, 9, 12];
for (const num of nums) {
    if (num > 6) {
        console.log(`First number > 6: ${num}`); // 8
        break;
    }
}

// Break out of while loop
while (true) {
    const input = prompt("Enter 'quit' to exit:");
    if (input === "quit") {
        break;
    }
    console.log(`You entered: ${input}`);
}
```

## [[continue]]

```javascript
// Skip even numbers
for (let i = 0; i < 10; i++) {
    if (i % 2 === 0) {
        continue; // skip even numbers
    }
    console.log(i); // 1, 3, 5, 7, 9
}

// Skip specific value
const fruits = ["apple", "banana", "orange", "grape"];
for (const fruit of fruits) {
    if (fruit === "banana") {
        continue; // skip banana
    }
    console.log(fruit); // apple, orange, grape
}

// Process only valid data
const data = [1, -1, 2, -2, 3, -3];
for (const item of data) {
    if (item < 0) {
        continue; // skip negatives
    }
    console.log(`Processing: ${item}`); // 1, 2, 3
}
```

## [[label|Labels]] with [[break]]/[[continue]]

```javascript
// Break nested loops
outer: for (let i = 0; i < 5; i++) {
    for (let j = 0; j < 5; j++) {
        if (i === 2 && j === 2) {
            break outer; // exits both loops
        }
        console.log(`${i},${j}`);
    }
}

// Continue outer loop
outer: for (let i = 0; i < 3; i++) {
    for (let j = 0; j < 3; j++) {
        if (j === 1) {
            continue outer; // goes to next i
        }
        console.log(`${i},${j}`);
    }
}
```

## Quick Revision

- `break` exits the [[loop]] entirely
- `continue` skips to next [[iteration]]
- [[label|Labels]] control nested loops
- Use `break` for early exit
- Use `continue` to skip unwanted iterations

---

## Related Topics

- [[What-is-ForLoop]] - [[for]] loops
- [[What-is-WhileLoop]] - [[while]] loops
- [[Use-BreakContinue]] - Using [[break]]/[[continue]]
- [[What-is-Label]] - [[label|Label]] statements
