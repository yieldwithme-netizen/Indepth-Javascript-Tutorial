# What is Try-Catch

## Definition

`try-catch` is a JavaScript error handling mechanism that allows you to "try" running code that might throw an error and "catch" that error if it occurs, preventing the program from crashing.

It provides a way to handle runtime errors gracefully by defining a fallback behavior when something goes wrong.

---

## Basic Syntax

```javascript
try {
  // Code that might throw an error
  riskyOperation();
} catch (error) {
  // Code to handle the error
  console.error("An error occurred:", error.message);
}
```

---

## How It Works

1. The `try` block executes normally
2. If an error occurs anywhere in the `try` block:
   - Execution immediately jumps to the `catch` block
   - The remaining code in `try` is skipped
   - The error object is passed to `catch`
3. If no error occurs, the `catch` block is skipped entirely

```javascript
try {
  console.log("Step 1");
  const result = undefined.name; // TypeError
  console.log("Step 2"); // This never runs
} catch (error) {
  console.log("Error caught:", error.message);
}

// Output:
// Step 1
// Error caught: Cannot read properties of undefined (reading 'name')
```

---

## The Error Object

When an error is caught, the error object contains useful information:

```javascript
try {
  JSON.parse("invalid JSON");
} catch (error) {
  console.log(error.name);    // "SyntaxError"
  console.log(error.message); // "Unexpected token i in JSON at position 0"
  console.log(error.stack);   // Stack trace with line numbers
  console.log(error instanceof SyntaxError); // true
}
```

---

## Common Use Cases

### Parsing JSON

```javascript
function safeJsonParse(jsonString) {
  try {
    return JSON.parse(jsonString);
  } catch (error) {
    console.error("Invalid JSON:", error.message);
    return null;
  }
}

safeJsonParse('{"valid": true}'); // { valid: true }
safeJsonParse('not json');        // null
```

### Accessing Properties Safely

```javascript
function getNestedValue(obj, path) {
  try {
    return path.split('.').reduce((current, key) => current[key], obj);
  } catch (error) {
    return undefined;
  }
}

const user = { profile: { name: "Alice" } };
getNestedValue(user, "profile.name"); // "Alice"
getNestedValue(user, "profile.age");  // undefined
```

### DOM Manipulation

```javascript
function hideElement(selector) {
  try {
    const element = document.querySelector(selector);
    element.style.display = "none";
  } catch (error) {
    console.warn("Could not hide element:", error.message);
  }
}

hideElement("#myElement"); // Works if element exists
hideElement("#nonexistent"); // Logs warning
```

---

## Common Mistakes

**Mistake 1: Empty catch block**

```javascript
// Bad - silently swallows errors
try {
  riskyOperation();
} catch (e) {
  // Error disappears without a trace
}

// Good - at least log the error
try {
  riskyOperation();
} catch (e) {
  console.error("Error:", e.message);
}
```

**Mistake 2: Catching errors you can't handle**

```javascript
// Bad - catching everything
try {
  doSomething();
} catch (e) {
  // What do you do with memory errors, etc?
}

// Better - be specific about what can go wrong
try {
  const data = JSON.parse(input);
  process(data);
} catch (e) {
  if (e instanceof SyntaxError) {
    console.error("Invalid input format");
  } else {
    throw e; // Re-throw unexpected errors
  }
}
```

**Mistake 3: Using try-catch for flow control**

```javascript
// Bad - using exceptions for normal logic
try {
  const value = obj.prop;
} catch (e) {
  const value = defaultValue;
}

// Good - use optional chaining or nullish checks
const value = obj?.prop ?? defaultValue;
```

---

## Related Topics

- [[Use-TryCatch]]
- [[What-is-ErrorTypes]]
- [[Throw-Errors]]
- [[Handle-AsyncErrors]]
- [[What-is-Propagation]]

---

## Quick Revision

- `try-catch` handles runtime errors gracefully
- Code in `try` block runs; if error occurs, `catch` block runs
- Error object contains `name`, `message`, and `stack`
- Catch only errors you can handle; re-throw others
- Don't use try-catch for normal flow control
- Always log or handle caught errors appropriately
