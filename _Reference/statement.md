# Statements

## Definition
Statements are instructions that perform actions in JavaScript. They are the building blocks of JavaScript programs, controlling program flow, declaring variables, and performing operations.

## Types of Statements

### 1. Declaration Statements
```javascript
// Variable declarations
var name = "John";
let age = 25;
const PI = 3.14159;

// Function declarations
function greet() {
  return "Hello!";
}

// Class declarations
class Person {
  constructor(name) {
    this.name = name;
  }
}
```

### 2. Expression Statements
```javascript
// Assignment
x = 10;
counter++;

// Function calls
console.log("Hello");
document.write("Content");

// Method calls
array.push(item);
string.toUpperCase();
```

### 3. Control Flow Statements
```javascript
// If-else statement
if (age >= 18) {
  console.log("Adult");
} else {
  console.log("Minor");
}

// Switch statement
switch (day) {
  case "Monday":
    console.log("Start of week");
    break;
  case "Friday":
    console.log("Almost weekend");
    break;
  default:
    console.log("Regular day");
}
```

### 4. Loop Statements
```javascript
// For loop
for (let i = 0; i < 10; i++) {
  console.log(i);
}

// While loop
while (condition) {
  doSomething();
}

// Do-while loop
do {
  doSomething();
} while (condition);

// For...in (objects)
for (let key in object) {
  console.log(key);
}

// For...of (iterables)
for (let item of array) {
  console.log(item);
}
```

### 5. Jump Statements
```javascript
// Break
for (let i = 0; i < 10; i++) {
  if (i === 5) break;
}

// Continue
for (let i = 0; i < 10; i++) {
  if (i === 5) continue;
}

// Return
function add(a, b) {
  return a + b;
}

// throw
function divide(a, b) {
  if (b === 0) throw new Error("Division by zero");
  return a / b;
}
```

## Common Use Cases
- Program flow control
- Variable and function declarations
- Iterating over data
- Error handling
- Conditional logic

## Common Mistakes

| Mistake | Solution |
|---------|----------|
| Missing semicolons | Use consistent semicolon style |
| Unclosed blocks | Always close braces/brackets |
| Unreachable code after return | Remove dead code |
| Missing break in switch | Add break or use comments |

## Quick Revision Summary
- Statements are instructions that perform actions
- Major types: declaration, expression, control flow, loop, jump
- Always use proper syntax and structure
- Semicolons help prevent ambiguity
- Block statements use curly braces `{}`

## Related Topics
- [[Expressions]]
- [[Operators]]
- [[Control-Flow]]
- [[Loops]]
- [[Functions]]
- [[Scope]]
