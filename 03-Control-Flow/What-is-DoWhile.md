# What is a [[do]]-[[while]] [[loop]]?

## Definition

A [[do]]-[[while]] [[loop]] is like a [[while]] [[loop]], but it **runs the code first**, then checks the [[condition]].

## Basic Syntax

```javascript
do {
    // code to repeat
} while (condition);
```

## How It Differs from [[while]]

```javascript
// While: check FIRST, then run
while (false) {
    console.log("Never runs!");
}

// Do-while: run FIRST, then check
do {
    console.log("Runs once!");
} while (false);
```

## Examples

```javascript
// Menu system
let choice;
do {
    choice = prompt("1. Play\n2. Settings\n3. Quit");
    switch (choice) {
        case "1": console.log("Playing..."); break;
        case "2": console.log("Settings..."); break;
        case "3": console.log("Quitting..."); break;
    }
} while (choice !== "3");

// Input validation
let age;
do {
    age = parseInt(prompt("Enter age (1-120):"));
} while (isNaN(age) || age < 1 || age > 120);

// Run at least once
let num = 100;
do {
    console.log(num); // Runs once even though condition is false
} while (num < 10);
```

## When to Use

```javascript
// Use do-when when:
// 1. Code must run at least once
// 2. User input validation
// 3. Menu systems
// 4. Game loops

// Use while when:
// 1. Might not need to run at all
// 2. Simple condition checks
```

## Quick Revision

- [[do]]-[[while]]: runs code THEN checks [[condition]]
- Guarantees at least one execution
- Use for input validation and menus
- Similar to [[while]], but different order
- Don't forget the semicolon after `while`

---

## Related Topics

- [[What-is-WhileLoop]] - [[while]] loops
- [[What-is-ForLoop]] - [[for]] loops
- [[Write-DoWhile]] - Writing [[do]]-[[while]]
- [[Write-WhileLoop]] - Writing [[while]] loops
