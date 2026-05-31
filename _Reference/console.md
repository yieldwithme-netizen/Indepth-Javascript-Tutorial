# Console

## Definition

The `console` object provides access to the browser's developer console. It's an essential debugging tool that allows developers to output information, track program execution, measure performance, and inspect data. The console is available in all modern browsers and Node.js environments.

## Code Examples

### Basic Output Methods

```javascript
// Log a message
console.log("Hello, World!");

// Log multiple values
console.log("Name:", "Alice", "Age:", 25);

// Log objects
const user = { name: "Bob", age: 30 };
console.log(user);
```

### Console Methods

```javascript
// console.log - General output
console.log("This is a log message");

// console.error - Error messages (usually red)
console.error("Something went wrong!");

// console.warn - Warning messages (usually yellow)
console.warn("This is a warning");

// console.info - Informational messages
console.info("This is info");

// console.debug - Debug messages
console.debug("Debug information");
```

### Formatting Output

```javascript
// String substitution
console.log("Hello, %s!", "Alice");          // "Hello, Alice!"
console.log("Number: %d", 42);               // "Number: 42"
console.log("Float: %f", 3.14);              // "Float: 3.14"
console.log("Object: %o", { key: "value" }); // "Object: { key: 'value' }"
console.log("CSS: %cStyled", "color: blue; font-weight: bold");

// Template literals
const name = "Charlie";
console.log(`Hello, ${name}!`);
```

### Grouping Output

```javascript
// Group messages
console.group("User Details");
console.log("Name: Alice");
console.log("Age: 25");
console.log("City: Seattle");
console.groupEnd();

// Collapsed groups
console.groupCollapsed("Collapsed Section");
console.log("Hidden by default");
console.log("Click to expand");
console.groupEnd();
```

### Tables and Data

```javascript
// Display as table
const users = [
  { name: "Alice", age: 25, city: "Seattle" },
  { name: "Bob", age: 30, city: "Portland" },
  { name: "Charlie", age: 35, city: "Denver" }
];
console.table(users);

// Display object as table
const user = { name: "Dave", age: 28, city: "Austin" };
console.table(user);
```

### Timing Operations

```javascript
// Start a timer
console.time("Loop");

// Perform some operation
let sum = 0;
for (let i = 0; i < 1000000; i++) {
  sum += i;
}

// End the timer
console.timeEnd("Loop"); // "Loop: 12.345ms"

// Time a specific operation
console.time("Array creation");
const arr = Array.from({ length: 1000 }, (_, i) => i);
console.timeEnd("Array creation");
```

### Counting

```javascript
// Count occurrences
console.count("apple");
console.count("banana");
console.count("apple");
console.count("banana");
console.count("apple");
// Output:
// apple: 1
// banana: 1
// apple: 2
// banana: 2
// apple: 3

// Reset counter
console.countReset("apple");
console.count("apple"); // apple: 1
```

### Assertions

```javascript
// Assert a condition
const age = 25;
console.assert(age >= 18, "User must be 18 or older"); // No output (true)

const score = 15;
console.assert(score >= 20, "Score must be 20 or higher"); // Assertion failed: Score must be 20 or higher
```

### Stack Trace

```javascript
function first() {
  second();
}

function second() {
  third();
}

function third() {
  console.trace("Trace from third function");
}

first();
// Trace from third function
//     at third (script.js:12)
//     at second (script.js:8)
//     at first (script.js:4)
//     at script.js:16
```

### Clearing Console

```javascript
// Clear the console
console.clear();
```

### Node.js Specific

```javascript
// In Node.js, console works similarly
const fs = require('fs');

// Log to file
const stream = fs.createWriteStream('log.txt');
const originalLog = console.log;
console.log = (...args) => {
  originalLog(...args);
  stream.write(args.join(' ') + '\n');
};

console.log("This goes to console and file");
```

### Custom Console Methods

```javascript
// Create custom logging functions
function createLogger(prefix) {
  return {
    log: (...args) => console.log(`[${prefix}]`, ...args),
    error: (...args) => console.error(`[${prefix}]`, ...args),
    warn: (...args) => console.warn(`[${prefix}]`, ...args)
  };
}

const logger = createLogger("APP");
logger.log("Application started");
logger.error("Database connection failed");
logger.warn("Low memory warning");
```

## Common Use Cases

1. **Debugging** - Output variable values during development
2. **Error tracking** - Log errors and warnings
3. **Performance measurement** - Time code execution
4. **Data inspection** - Display arrays and objects
5. **Development logging** - Track application flow

## Common Mistakes

1. **Leaving console.log in production** - Remove or use a logging library
2. **Using console for user-facing messages** - Use DOM or UI instead
3. **Not using appropriate methods** - Use `error` for errors, `warn` for warnings
4. **Logging sensitive data** - Never log passwords, tokens, or PII

```javascript
// WRONG: Logging sensitive data
console.log("Password:", userPassword);

// RIGHT: Logging non-sensitive data
console.log("User logged in:", user.username);
```

## Quick Revision Summary

- `console.log()` for general output
- `console.error()` and `console.warn()` for specific message types
- Use `%s`, `%d`, `%o` for formatted output
- `console.table()` displays data in table format
- `console.time()` and `console.timeEnd()` measure performance
- `console.assert()` validates conditions
- Remove console statements before production deployment

## Related Topics

- [[Debugging]]
- [[Error-Handling]]
- [[Browser-DevTools]]
- [[Node.js]]
- [[Performance-Measurement]]
- [[Logging]]
