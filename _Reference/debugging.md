# Debugging Techniques in JavaScript

## Definition

Debugging is the process of finding and fixing errors (bugs) in code. JavaScript provides several tools and techniques for debugging, including console methods, breakpoints, DevTools, and error handling patterns.

---

## Console Methods

### Basic Logging

```javascript
// console.log - basic output
const name = "Alice";
console.log("Name:", name);

// console.error - error messages (red)
console.error("Something went wrong");

// console.warn - warnings (yellow)
console.warn("Deprecated function used");

// console.info - informational
console.info("Server started on port 3000");
```

### Advanced Console Methods

```javascript
// console.table - display arrays/objects as table
const users = [
  { id: 1, name: "Alice", age: 25 },
  { id: 2, name: "Bob", age: 30 }
];
console.table(users);

// console.group - group related logs
console.group("User Operations");
console.log("Creating user...");
console.log("User created");
console.groupEnd();

// console.time - measure execution time
console.time("fetch");
await fetch("/api/data");
console.timeEnd("fetch"); // "fetch: 234.567ms"

// console.trace - print stack trace
function nested() {
  console.trace("Called from:");
}
function middle() { nested(); }
function outer() { middle(); }
outer();

// console.assert - conditional logging
const age = 15;
console.assert(age >= 18, "Must be 18+", { age });

// console.dir - detailed object view
console.dir(document.body, { depth: 2 });

// console.count - count occurrences
function processItem(item) {
  console.count("items processed");
  // ...
}

// console.clear - clear console
console.clear();
```

### Styled Console Output

```javascript
// Custom styles
console.log("%cBold Red Text", "color: red; font-weight: bold");
console.log("%cGreen background", "background: green; color: white");

// Multiple styles
console.log("%cWarning:%c Important message",
  "color: orange; font-weight: bold",
  "color: inherit"
);
```

---

## Browser DevTools

### Setting Breakpoints

```javascript
// Programmatic debugger statement
function problematicFunction() {
  const data = fetchData();
  debugger; // Execution pauses here
  process(data);
}

// Conditional debugger
function processArray(arr) {
  arr.forEach((item, index) => {
    if (index === 5) {
      debugger; // Pauses only on 6th element
    }
    processItem(item);
  });
}
```

### DevTools Features

1. **Sources Panel**: Set breakpoints, step through code
2. **Console**: Execute code, inspect variables
3. **Network**: Monitor API calls
4. **Performance**: Profile execution time
5. **Memory**: Find memory leaks

---

## Error Handling Patterns

### Try-Catch

```javascript
// Basic try-catch
try {
  const data = JSON.parse(invalidJSON);
} catch (err) {
  console.error("Parse error:", err.message);
}

// With finally
try {
  const result = riskyOperation();
  return result;
} catch (err) {
  console.error(err);
  return null;
} finally {
  cleanup(); // Always runs
}
```

### Custom Error Classes

```javascript
class AppError extends Error {
  constructor(message, code, statusCode) {
    super(message);
    this.code = code;
    this.statusCode = statusCode;
    this.name = "AppError";
  }
}

class ValidationError extends AppError {
  constructor(field, message) {
    super(`Validation failed: ${field} - ${message}`, "VALIDATION_ERROR", 422);
    this.field = field;
  }
}

// Usage
try {
  if (!user.email) {
    throw new ValidationError("email", "Email is required");
  }
} catch (err) {
  if (err instanceof ValidationError) {
    console.error(`${err.field}: ${err.message}`);
  }
}
```

### Error Boundaries (React)

```javascript
class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false, error: null };
  }

  static getDerivedStateFromError(error) {
    return { hasError: true, error };
  }

  componentDidCatch(error, errorInfo) {
    console.error("Error caught:", error, errorInfo);
    // Send to error reporting service
  }

  render() {
    if (this.state.hasError) {
      return <h1>Something went wrong</h1>;
    }
    return this.props.children;
  }
}
```

---

## Common Use Cases

### Debugging Async Code

```javascript
// Add logging to async functions
async function fetchData(url) {
  console.log(`Fetching: ${url}`);
  const start = performance.now();

  try {
    const response = await fetch(url);
    console.log(`Response: ${response.status} ${response.statusText}`);

    if (!response.ok) {
      throw new Error(`HTTP ${response.status}`);
    }

    const data = await response.json();
    const duration = performance.now() - start;
    console.log(`Completed in ${duration.toFixed(2)}ms`);

    return data;
  } catch (err) {
    console.error(`Failed after ${performance.now() - start}ms:`, err);
    throw err;
  }
}
```

### Debugging React Components

```javascript
// React DevTools Profiler
import { Profiler } from "react";

function onRenderCallback(
  id,
  phase,
  actualDuration,
  baseDuration,
  startTime,
  commitTime
) {
  console.log(`${id} ${phase}:`, {
    actualDuration,
    baseDuration,
    startTime,
    commitTime
  });
}

function App() {
  return (
    <Profiler id="App" onRender={onRenderCallback}>
      <MyComponent />
    </Profiler>
  );
}
```

### Debugging Node.js

```javascript
// Node.js inspect mode
// node --inspect app.js
// node --inspect-brk app.js (pause on first line)

// Programmatic inspect
const util = require("util");

function debugObject(obj) {
  console.log(util.inspect(obj, {
    showHidden: true,
    depth: null,
    colors: true
  }));
}

// Chrome DevTools for Node.js
// node --inspect app.js
// Open chrome://inspect in Chrome
```

### Performance Profiling

```javascript
// Measure function execution
function measure(fn, name = fn.name) {
  return function(...args) {
    console.time(name);
    const result = fn.apply(this, args);
    console.timeEnd(name);
    return result;
  };
}

const heavyComputation = measure((n) => {
  let sum = 0;
  for (let i = 0; i < n; i++) sum += i;
  return sum;
});

heavyComputation(1000000); // "heavyComputation: 5.234ms"

// Performance API
function profile(name) {
  performance.mark(`${name}-start`);

  return {
    end() {
      performance.mark(`${name}-end`);
      performance.measure(name, `${name}-start`, `${name}-end`);
      const measure = performance.getEntriesByName(name)[0];
      console.log(`${name}: ${measure.duration.toFixed(2)}ms`);
      performance.clearMarks(`${name}-start`);
      performance.clearMarks(`${name}-end`);
      performance.clearMeasures(name);
    }
  };
}

const p = profile("data processing");
await processData();
p.end(); // "data processing: 123.456ms"
```

---

## Common Mistakes

### Mistake 1: console.log for Production

```javascript
// Wrong: leaving console.log in production
function processUser(user) {
  console.log("Processing:", user); // Should not be in production!
  // ...
}

// Correct: use a logging library
import winston from "winston";

const logger = winston.createLogger({
  level: process.env.LOG_LEVEL || "info"
});

function processUser(user) {
  logger.info("Processing user", { userId: user.id });
  // ...
}
```

### Mistake 2: Swallowing Errors

```javascript
// Wrong: catch without handling
try {
  riskyOperation();
} catch (err) {
  // Error disappears!
}

// Correct: at least log it
try {
  riskyOperation();
} catch (err) {
  console.error("Operation failed:", err);
  // Or rethrow: throw err;
}
```

### Mistake 3: Not Using debugger

```javascript
// Wrong: using alert for debugging
function calculate(x) {
  alert("x is: " + x); // Disrupts user!
  return x * 2;
}

// Correct: use debugger or console.log
function calculate(x) {
  console.log("x is:", x);
  return x * 2;
}
```

### Mistake 4: Debugging Production Issues Without Logs

```javascript
// Wrong: no context in errors
async function fetchData(url) {
  try {
    return await fetch(url);
  } catch (err) {
    throw new Error("Failed to fetch"); // Loses original error!
  }
}

// Correct: preserve error context
async function fetchData(url) {
  try {
    return await fetch(url);
  } catch (err) {
    throw new Error(`Failed to fetch ${url}: ${err.message}`, {
      cause: err
    });
  }
}
```

---

## Quick Revision Summary

| Tool/Technique | Use Case |
|----------------|----------|
| `console.log` | Basic debugging |
| `console.table` | Array/object inspection |
| `console.time` | Performance measurement |
| `debugger` | Breakpoints in code |
| Try-catch | Error handling |
| Custom errors | Domain-specific errors |
| DevTools Sources | Step-through debugging |
| DevTools Network | API monitoring |
| DevTools Memory | Memory leak detection |
| Performance API | Profiling |

---

## Related Topics

- [[Promise]] - Debugging async/await
- [[this]] - Understanding `this` context
- [[API-Design]] - API error handling
- [[loop]] - Debugging iteration issues
- [[Object]] - Object inspection techniques
