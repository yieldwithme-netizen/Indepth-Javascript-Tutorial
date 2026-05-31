# Switch Statement

## Definition

A `switch` statement is a control flow mechanism that evaluates an expression and matches its value against multiple `case` clauses. It executes the code block associated with the matching case, providing an alternative to multiple `if-else` statements when comparing a single value against many possibilities.

## Syntax

```javascript
switch (expression) {
  case value1:
    // code block
    break;
  case value2:
    // code block
    break;
  default:
    // code block
    break;
}
```

## Code Examples

### Basic Switch Statement

```javascript
const day = "Monday";

switch (day) {
  case "Monday":
    console.log("Start of work week");
    break;
  case "Friday":
    console.log("TGIF!");
    break;
  case "Saturday":
  case "Sunday":
    console.log("Weekend!");
    break;
  default:
    console.log("Midweek day");
    break;
}
// Output: "Start of work week"
```

### Switch with Numbers

```javascript
const grade = 85;

switch (true) {
  case grade >= 90:
    console.log("A");
    break;
  case grade >= 80:
    console.log("B");
    break;
  case grade >= 70:
    console.log("C");
    break;
  case grade >= 60:
    console.log("D");
    break;
  default:
    console.log("F");
    break;
}
// Output: "B"
```

### Fall-Through Behavior

```javascript
const month = 2;
let daysInMonth;

switch (month) {
  case 2:
    daysInMonth = 28; // No break, falls through
  case 4:
  case 6:
  case 9:
  case 11:
    daysInMonth = daysInMonth || 30;
    break;
  default:
    daysInMonth = 31;
    break;
}

console.log(`Month ${month} has ${daysInMonth} days`);
// Output: "Month 2 has 28 days"
```

### Switch with String Comparison

```javascript
const command = "start";

switch (command.toLowerCase()) {
  case "start":
    console.log("Starting application...");
    break;
  case "stop":
    console.log("Stopping application...");
    break;
  case "pause":
    console.log("Pausing application...");
    break;
  case "restart":
    console.log("Restarting application...");
    break;
  default:
    console.log("Unknown command");
    break;
}
```

### Switch in Function

```javascript
function getSeason(month) {
  switch (month) {
    case 12:
    case 1:
    case 2:
      return "Winter";
    case 3:
    case 4:
    case 5:
      return "Spring";
    case 6:
    case 7:
    case 8:
      return "Summer";
    case 9:
    case 10:
    case 11:
      return "Fall";
    default:
      return "Unknown";
  }
}

console.log(getSeason(7)); // "Summer"
console.log(getSeason(11)); // "Fall"
```

### Switch with Expressions

```javascript
const x = 10;
const y = 5;

switch (true) {
  case x > y:
    console.log("x is greater");
    break;
  case x < y:
    console.log("y is greater");
    break;
  default:
    console.log("x equals y");
    break;
}
// Output: "x is greater"
```

### Practical Example: Calculator

```javascript
function calculate(a, b, operator) {
  switch (operator) {
    case "+":
      return a + b;
    case "-":
      return a - b;
    case "*":
      return a * b;
    case "/":
      if (b === 0) {
        throw new Error("Division by zero");
      }
      return a / b;
    case "%":
      return a % b;
    case "**":
      return a ** b;
    default:
      throw new Error(`Unknown operator: ${operator}`);
  }
}

console.log(calculate(10, 5, "+")); // 15
console.log(calculate(10, 5, "*")); // 50
console.log(calculate(10, 3, "%")); // 1
```

### Switch vs If-Else

```javascript
// Using if-else
const color = "red";
if (color === "red") {
  console.log("Stop");
} else if (color === "yellow") {
  console.log("Caution");
} else if (color === "green") {
  console.log("Go");
} else {
  console.log("Invalid color");
}

// Using switch (cleaner for single value comparison)
switch (color) {
  case "red":
    console.log("Stop");
    break;
  case "yellow":
    console.log("Caution");
    break;
  case "green":
    console.log("Go");
    break;
  default:
    console.log("Invalid color");
    break;
}
```

### Object Lookup Pattern (Alternative to Switch)

```javascript
// Switch approach
function getHttpStatus(statusCode) {
  switch (statusCode) {
    case 200:
      return "OK";
    case 301:
      return "Moved Permanently";
    case 404:
      return "Not Found";
    case 500:
      return "Internal Server Error";
    default:
      return "Unknown";
  }
}

// Object lookup approach (often cleaner)
const httpStatuses = {
  200: "OK",
  301: "Moved Permanently",
  404: "Not Found",
  500: "Internal Server Error"
};

function getHttpStatus(statusCode) {
  return httpStatuses[statusCode] || "Unknown";
}
```

## Common Use Cases

1. **Menu systems** - Handle user selections
2. **State machines** - Manage application states
3. **Command processing** - Execute different actions based on input
4. **Day/month handling** - Convert numbers to names
5. **Type checking** - Handle different data types

## Common Mistakes

1. **Forgetting `break`** - Causes unintended fall-through
2. **Using `===` implicitly** - Switch uses strict comparison
3. **Not including `default`** - Always handle unexpected values
4. **Using for complex conditions** - If-else is better for multiple conditions

```javascript
// WRONG: Missing break
switch (color) {
  case "red":
    console.log("Stop");
    // Falls through to next case!
  case "yellow":
    console.log("Caution");
    break;
}

// RIGHT: Always include break
switch (color) {
  case "red":
    console.log("Stop");
    break;
  case "yellow":
    console.log("Caution");
    break;
}
```

## Quick Revision Summary

- Switch evaluates an expression and matches against case values
- Always use `break` to prevent fall-through
- `default` handles unmatched values
- Uses strict comparison (`===`)
- Best for comparing single value against many options
- Consider object lookup for simple mappings

## Related Topics

- [[If-Else-Statement]]
- [[Comparison-Operators]]
- [[Control-Flow]]
- [[Ternary-Operator]]
- [[Loops]]
- [[Functions]]
