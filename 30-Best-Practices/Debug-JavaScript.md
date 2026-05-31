# How to Debug JavaScript

## Definition

Debugging is the process of **finding and fixing errors** in your code using systematic techniques and tools.

## Debugging Tools

| Tool | Purpose |
|------|---------|
| `console.log()` | Quick value inspection |
| `debugger` | Breakpoint pauses |
| Chrome DevTools | Full debugging suite |
| VS Code Debugger | IDE-integrated debugging |
| ESLint | Static code analysis |

## Console Methods

```javascript
// Basic logging
console.log("Normal output");
console.warn("Warning message");
console.error("Error message");

// Structured output
console.table([{ name: "John", age: 30 }, { name: "Jane", age: 25 }]);
console.dir(document.body);
console.dirxml(document.body);

// Timing
console.time("fetch");
await fetch("/api/data");
console.timeEnd("fetch"); // fetch: 120.5ms

// Grouping
console.group("User Data");
console.log("Name: John");
console.log("Age: 30");
console.groupEnd();

// Counting
console.count("button click");
console.countReset("button click");
```

## Using Debugger Keyword

```javascript
function calculateTotal(items) {
  let total = 0;
  for (const item of items) {
    debugger; // Execution pauses here
    total += item.price * item.quantity;
  }
  return total;
}
```

## Chrome DevTools Debugging

```javascript
// 1. Open DevTools (F12 or Ctrl+Shift+I)
// 2. Go to Sources tab
// 3. Set breakpoints by clicking line numbers
// 4. Use controls:
//    - Resume (F8): Continue execution
//    - Step Over (F10): Execute current line
//    - Step Into (F11): Enter function
//    - Step Out (Shift+F11): Exit function

// 5. Watch expressions
// In Watch panel, add expressions to monitor:
// total
// items.length
// user?.name
```

## Common Error Types and Fixes

```javascript
// TypeError: Cannot read properties of undefined
const user = {};
console.log(user.name); // undefined
console.log(user.name.length); // Error!

// Fix: Optional chaining
console.log(user.name?.length);

// ReferenceError: variable not defined
console.log(myVar); // Error!

// Fix: Declare variable
const myVar = "hello";

// SyntaxError: unexpected token
const obj = { name: "John" age: 30 }; // Missing comma

// Fix: Correct syntax
const obj = { name: "John", age: 30 };
```

## Error Handling Debugging

```javascript
// Try-catch with detailed error info
try {
  riskyOperation();
} catch (error) {
  console.error("Error message:", error.message);
  console.error("Error name:", error.name);
  console.error("Stack trace:", error.stack);
  console.error("Full error:", error);
}

// Async error debugging
async function fetchData() {
  try {
    const response = await fetch("/api/data");
    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }
    return await response.json();
  } catch (error) {
    console.error("Fetch failed:", error);
    throw error;
  }
}
```

## VS Code Debug Configuration

```json
// .vscode/launch.json
{
  "version": "0.2.0",
  "configurations": [
    {
      "type": "node",
      "request": "launch",
      "name": "Debug Node.js",
      "program": "${workspaceFolder}/app.js",
      "console": "integratedTerminal"
    },
    {
      "type": "chrome",
      "request": "launch",
      "name": "Debug Chrome",
      "url": "http://localhost:3000",
      "webRoot": "${workspaceFolder}"
    }
  ]
}
```

## Performance Debugging

```javascript
// Measure execution time
console.time("loop");
for (let i = 0; i < 1000000; i++) {
  // Operation
}
console.timeEnd("loop"); // loop: 15.2ms

// Memory usage
console.memory;
// { jsHeapSizeLimit: 2197815296, totalJSHeapSize: 76511040, usedJSHeapSize: 58904120 }

// Profile with Performance API
performance.mark("start");
await heavyOperation();
performance.mark("end");
performance.measure("operation", "start", "end");
console.log(performance.getEntriesByName("operation"));
```

## Common Mistakes

```javascript
// BAD: Using console.log in production
console.log("Debugging:", sensitiveData);

// GOOD: Remove or use a logger
if (process.env.NODE_ENV === "development") {
  console.log("Debugging:", sensitiveData);
}

// BAD: Not checking for null/undefined
function getUserName(user) {
  return user.name; // May throw if user is null
}

// GOOD: Defensive programming
function getUserName(user) {
  return user?.name ?? "Unknown";
}

// BAD: Swallowing errors
try {
  riskyCode();
} catch (e) {
  // Silent failure
}

// GOOD: Handle errors properly
try {
  riskyCode();
} catch (error) {
  console.error("Error:", error);
  showErrorToUser(error.message);
}
```

## Quick Revision

- Use `console.log()` for quick debugging
- Use `debugger` keyword for breakpoints
- Chrome DevTools provides full debugging capabilities
- Always check error messages and stack traces
- Use optional chaining to prevent null errors
- Remove debugging code before production
- Use VS Code debugger for IDE-integrated debugging

---

## Related Topics

- [[What-is-Debugging]] - Debugging overview
- [[What-is-Error-Handling]] - Error handling
- [[Use-Chrome-DevTools]] - Chrome DevTools
- [[What-is-CodeReview]] - Code review
- [[Write-Documentation]] - Documentation