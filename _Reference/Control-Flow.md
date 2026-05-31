# Control Flow

Control flow refers to the order in which individual statements, instructions, or function calls are executed in a JavaScript program. It determines the path your code takes based on conditions, loops, and branching logic.

## Key Concepts

### Conditional Statements

**if...else**
Executes different code blocks based on a condition:

```javascript
const temperature = 25;

if (temperature > 30) {
  console.log("It's hot outside!");
} else if (temperature > 20) {
  console.log("It's nice outside!");
} else {
  console.log("It's cold outside!");
}
```

**switch**
Selects one of many code blocks to execute based on a value:

```javascript
const day = "Monday";

switch (day) {
  case "Monday":
    console.log("Start of work week");
    break;
  case "Friday":
    console.log("Almost weekend!");
    break;
  case "Saturday":
  case "Sunday":
    console.log("Weekend!");
    break;
  default:
    console.log("Midweek day");
}
```

### Loops

**for loop**
Repeats code a specific number of times:

```javascript
for (let i = 0; i < 5; i++) {
  console.log(`Iteration ${i}`);
}
```

**while loop**
Continues while a condition is true:

```javascript
let count = 0;
while (count < 3) {
  console.log(count);
  count++;
}
```

**do...while loop**
Executes at least once, then continues while condition is true:

```javascript
let num = 0;
do {
  console.log(num);
  num++;
} while (num < 3);
```

**for...in**
Iterates over object properties:

```javascript
const person = { name: "Alice", age: 25 };

for (const key in person) {
  console.log(`${key}: ${person[key]}`);
}
```

**for...of**
Iterates over iterable objects (arrays, strings, etc.):

```javascript
const colors = ["red", "green", "blue"];

for (const color of colors) {
  console.log(color);
}
```

### Control Flow Statements

**break**
Exits a loop or switch statement early:

```javascript
for (let i = 0; i < 10; i++) {
  if (i === 5) break;
  console.log(i);
}
```

**continue**
Skips the rest of the current loop iteration and moves to the next:

```javascript
for (let i = 0; i < 10; i++) {
  if (i % 2 === 0) continue;
  console.log(i); // Only odd numbers
}
```

**Ternary Operator**
Shorthand for simple if...else:

```javascript
const age = 18;
const canVote = age >= 18 ? "Yes" : "No";
console.log(canVote); // "Yes"
```

## Common Use Cases

- Form validation before submission
- Game logic (player actions, scoring)
- Data filtering and processing
- UI state management
- API response handling

## Common Mistakes

1. **Forgetting `break` in switch statements** - Causes fall-through to next case
2. **Infinite loops** - Always ensure loop conditions eventually become false
3. **Using `=` instead of `==` or `===`** in conditions
4. **Not handling all cases** in switch statements
5. **Off-by-one errors** in loop boundaries

## Related Topics

- [[Conditional-Operators]]
- [[Loops]]
- [[Functions]]
- [[Error-Handling]]
- [[Iteration-Protocols]]

## Quick Revision

| Statement | Purpose |
|-----------|---------|
| `if...else` | Execute code based on condition |
| `switch` | Select from multiple cases |
| `for` | Loop a specific number of times |
| `while` | Loop while condition is true |
| `break` | Exit loop/switch early |
| `continue` | Skip to next iteration |
| Ternary | Shorthand if...else |

Control flow is fundamental to writing dynamic, responsive JavaScript programs that can make decisions and repeat actions efficiently.