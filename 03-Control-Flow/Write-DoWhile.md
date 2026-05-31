# How to Write Do-While Loops

## Basic Syntax

```javascript
do {
    // code to repeat
} while (condition);
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

- Do-while: runs code THEN checks condition
- Guarantees at least one execution
- Use for input validation and menus
- Similar to while, but different order
- Don't forget the semicolon after `while`

---

## Related Topics

- [[What-is-DoWhile]] - [[What-is-DoWhile|Do-while]] overview
- [[Write-WhileLoop]] - [[Write-WhileLoop|While loops]]
- [[Write-ForLoop]] - [[Write-ForLoop|For loops]]
- [[Write-Switch]] - [[Write-Switch|Switch statements]]
