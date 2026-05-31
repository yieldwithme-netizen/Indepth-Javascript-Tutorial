# How to Throw Errors

## Definition

Throwing errors in JavaScript allows you to explicitly signal that something went wrong. You use the `throw` statement to create and raise an error, which can then be caught by a `try-catch` block or propagate up the call stack.

---

## Basic Syntax

```javascript
throw new Error("Something went wrong");
throw "String error"; // Works but not recommended
throw 42; // Works but not recommended
throw null; // Works but not recommended
```

---

## Throwing Error Objects

### Throwing Built-in Errors

```javascript
// TypeError - wrong data type
function processValue(value) {
  if (typeof value !== "number") {
    throw new TypeError("Expected a number");
  }
  return value * 2;
}

processValue(5);   // 10
processValue("5"); // TypeError: Expected a number
```

```javascript
// ReferenceError - undeclared variable
function checkVariable() {
  if (typeof undeclaredVar === "undefined") {
    throw new ReferenceError("Variable is not defined");
  }
}
```

```javascript
// RangeError - value out of range
function setAge(age) {
  if (age < 0 || age > 150) {
    throw new RangeError("Age must be between 0 and 150");
  }
  return age;
}

setAge(25);  // 25
setAge(-5);  // RangeError: Age must be between 0 and 150
```

### Throwing Custom Messages

```javascript
function divide(a, b) {
  if (b === 0) {
    throw new Error("Division by zero");
  }
  return a / b;
}

divide(10, 0); // Error: Division by zero
```

### Throwing Custom Error Classes

```javascript
class ValidationError extends Error {
  constructor(field, message) {
    super(message);
    this.name = "ValidationError";
    this.field = field;
  }
}

function validateEmail(email) {
  if (!email.includes("@")) {
    throw new ValidationError("email", "Invalid email format");
  }
}
```

---

## Common Use Cases

### Input Validation

```javascript
function createUser(name, age, email) {
  if (!name || typeof name !== "string") {
    throw new TypeError("Name must be a non-empty string");
  }

  if (typeof age !== "number" || age < 0) {
    throw new RangeError("Age must be a positive number");
  }

  if (!email || !email.includes("@")) {
    throw new TypeError("Valid email required");
  }

  return { name, age, email };
}
```

### API Response Validation

```javascript
async function fetchUser(id) {
  if (typeof id !== "number") {
    throw new TypeError("User ID must be a number");
  }

  const response = await fetch(`/api/users/${id}`);

  if (!response.ok) {
    throw new Error(`HTTP error! status: ${response.status}`);
  }

  const data = await response.json();

  if (!data.name) {
    throw new Error("Invalid user data: missing name");
  }

  return data;
}
```

### State Validation

```javascript
class ShoppingCart {
  constructor() {
    this.items = [];
    this.checkedOut = false;
  }

  addItem(item) {
    if (this.checkedOut) {
      throw new Error("Cannot modify cart after checkout");
    }
    this.items.push(item);
  }

  checkout() {
    if (this.items.length === 0) {
      throw new Error("Cart is empty");
    }
    this.checkedOut = true;
  }
}
```

---

## Throwing Non-Error Values

While you can throw any value, it's strongly discouraged:

```javascript
// Bad - throws string
throw "Something went wrong";

// Bad - throws number
throw 404;

// Bad - throws null
throw null;

// Good - always throw Error objects
throw new Error("Something went wrong");
```

**Why?** Error objects have useful properties like `stack`, `name`, and `message`. Non-Error throws lose this debugging information.

---

## Re-throwing Errors

Sometimes you want to catch an error, add context, and re-throw:

```javascript
async function fetchUserData(userId) {
  try {
    const response = await fetch(`/api/users/${userId}`);
    return await response.json();
  } catch (error) {
    // Add context and re-throw
    throw new Error(`Failed to fetch user ${userId}: ${error.message}`);
  }
}
```

### Preserving Original Error

```javascript
async function processData(data) {
  try {
    return await transform(data);
  } catch (error) {
    // Preserve original error stack
    const wrappedError = new Error("Processing failed");
    wrappedError.originalError = error;
    throw wrappedError;
  }
}
```

---

## Common Mistakes

**Mistake 1: Throwing string literals**

```javascript
// Bad
throw "Invalid input";

// Good
throw new TypeError("Invalid input");
```

**Mistake 2: Not providing descriptive messages**

```javascript
// Bad
throw new Error("Error");

// Good
throw new Error("User email already exists: " + email);
```

**Mistake 3: Throwing in async without try-catch**

```javascript
// Bad - error may be unhandled
async function fetchData() {
  const response = await fetch(url);
  if (!response.ok) {
    throw new Error("Failed"); // Caller must catch
  }
}

// Better - document or handle
async function fetchData() {
  try {
    const response = await fetch(url);
    if (!response.ok) {
      throw new Error(`HTTP ${response.status}`);
    }
    return await response.json();
  } catch (error) {
    console.error("fetchData failed:", error);
    throw error; // Still throw, but log first
  }
}
```

**Mistake 4: Catching and not re-throwing**

```javascript
// Bad - swallows error silently
try {
  riskyOperation();
} catch (e) {
  // Error disappears
}

// Better - at least log it
try {
  riskyOperation();
} catch (e) {
  console.error("Operation failed:", e);
  throw e; // Re-throw if can't handle
}
```

---

## Related Topics

- [[What-is-TryCatch]]
- [[Use-TryCatch]]
- [[What-is-ErrorTypes]]
- [[What-is-CustomError]]
- [[Create-CustomError]]
- [[What-is-Propagation]]

---

## Quick Revision

- Use `throw` to explicitly signal errors
- Always throw `Error` objects, not primitives
- Provide descriptive error messages
- Throw specific error types (TypeError, RangeError, etc.)
- Re-throw errors when adding context
- Use custom error classes for domain-specific errors
- Never silently swallow caught errors
