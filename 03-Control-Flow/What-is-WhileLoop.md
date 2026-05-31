# What is a [[while]] [[loop]]?

## Definition

A [[while]] [[loop]] **repeats code** as long as a [[condition]] is `true`.

## Basic Syntax

```javascript
while (condition) {
    // code to repeat
    // must change condition to false!
}
```

## Examples

```javascript
// Basic while loop
let count = 0;
while (count < 5) {
    console.log(count); // 0, 1, 2, 3, 4
    count++;
}

// Ask until valid input
let input;
while (input !== "quit") {
    input = prompt("Enter 'quit' to exit:");
}

// Find first multiple of 7
let num = 1;
while (num % 7 !== 0) {
    num++;
}
console.log(num); // 7
```

## [[while]] vs [[for]]

```javascript
// Use for when: you know the number of iterations
for (let i = 0; i < 10; i++) { }

// Use while when: you don't know when it will end
while (condition) { }

// Example: user input validation
let age;
while (true) {
    age = parseInt(prompt("Enter age:"));
    if (!isNaN(age) && age > 0) {
        break; // valid input, exit loop
    }
    alert("Invalid! Try again.");
}
```

## Common Mistakes

```javascript
// ❌ Wrong: Infinite loop (condition never false)
while (true) {
    console.log("forever!");
    // No break or condition change!
}

// ❌ Wrong: Forgot to update variable
let x = 0;
while (x < 5) {
    console.log(x);
    // x++ missing!
}

// ✅ Right: Always update the variable
let x = 0;
while (x < 5) {
    console.log(x);
    x++;
}
```

## Quick Revision

- [[while]] loop: runs while [[condition]] is true
- Use when [[iteration]] count is unknown
- Always update [[variable]] to avoid infinite loops
- Use `break` to exit early
- More flexible than [[for]] loop

---

## Related Topics

- [[What-is-ForLoop]] - [[for]] loops
- [[What-is-DoWhile]] - [[do]]-[[while]] loops
- [[Write-WhileLoop]] - Writing [[while]] loops
- [[Use-BreakContinue]] - [[break]] and [[continue]]
