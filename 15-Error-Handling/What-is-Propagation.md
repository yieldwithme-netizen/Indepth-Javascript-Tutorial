# What is Error Propagation

## Definition

Error propagation is the process by which an error travels up the call stack from where it occurred to where it is caught. In JavaScript, if an error is not caught in the current function, it automatically propagates to the calling function, and continues up until it's caught or crashes the program.

---

## How Propagation Works

```javascript
function thirdFunction() {
  throw new Error("Error in thirdFunction");
}

function secondFunction() {
  thirdFunction(); // Error propagates up
}

function firstFunction() {
  secondFunction(); // Error propagates up again
}

try {
  firstFunction(); // Caught here
} catch (error) {
  console.log(error.message); // "Error in thirdFunction"
}
```

The error travels: `thirdFunction` → `secondFunction` → `firstFunction` → `catch` block

---

## Visualizing the Stack

```
Call Stack:
┌─────────────────┐
│ catch block     │ ← Error caught here
├─────────────────┤
│ firstFunction   │
├─────────────────┤
│ secondFunction  │
├─────────────────┤
│ thirdFunction   │ ← Error thrown here
└─────────────────┘
```

---

## Propagation in Nested Calls

```javascript
function fetchData() {
  return parseData(rawResponse);
}

function parseData(response) {
  return JSON.parse(response);
}

function processUser() {
  try {
    const data = fetchData();
    return data.name;
  } catch (error) {
    console.log("Caught in processUser:", error.message);
    // Decide: handle or re-throw
    throw error; // Re-throw if can't handle
  }
}

function app() {
  try {
    processUser();
  } catch (error) {
    console.log("Caught in app:", error.message);
  }
}

app();
```

---

## Explicit Re-throwing

You can catch an error, add context, and re-throw:

```javascript
async function loadUserProfile(userId) {
  try {
    const response = await fetch(`/api/users/${userId}`);
    return await response.json();
  } catch (error) {
    // Add context and re-throw
    throw new Error(`Failed to load user ${userId}: ${error.message}`);
  }
}
```

### Preserving Stack Trace

```javascript
async function processData(data) {
  try {
    return await transform(data);
  } catch (error) {
    // Create new error but preserve original
    const wrapped = new Error("Processing failed");
    wrapped.originalError = error;
    wrapped.stack = `${wrapped.stack}\n\nOriginal error:\n${error.stack}`;
    throw wrapped;
  }
}
```

---

## Propagation with Promises

Errors in promises propagate differently:

```javascript
// Synchronous - propagates up call stack
function syncExample() {
  throw new Error("Sync error");
}

// Async - rejected promise, not thrown
async function asyncExample() {
  throw new Error("Async error");
}

// Promise chain - propagates to next catch
fetchData()
  .then(process)
  .catch(handleError); // Catches errors from both
```

### Promise Rejection Propagation

```javascript
function getData() {
  return fetch("/api/data")
    .then(response => response.json())
    .then(data => {
      if (!data.valid) {
        throw new Error("Invalid data");
      }
      return data;
    });
}

// Error propagates to the catch
getData()
  .then(display)
  .catch(handleError);
```

---

## Uncaught Errors

If no catch handler exists, errors propagate to the global scope:

```javascript
// Browser - triggers window.onerror
function throwUncaught() {
  throw new Error("Uncaught error");
}

// Node.js - triggers process.on('uncaughtException')
process.on("uncaughtException", (error) => {
  console.error("Uncaught exception:", error);
  process.exit(1);
});

// Unhandled promise rejection
process.on("unhandledRejection", (reason, promise) => {
  console.error("Unhandled rejection:", reason);
});
```

---

## Common Use Cases

### Layered Architecture

```javascript
// Data layer - throws specific errors
class UserRepository {
  async findById(id) {
    const user = await db.query("SELECT * FROM users WHERE id = ?", [id]);
    if (!user) {
      throw new NotFoundError("User", id);
    }
    return user;
  }
}

// Service layer - catches and wraps
class UserService {
  constructor(userRepo) {
    this.userRepo = userRepo;
  }

  async getProfile(userId) {
    try {
      return await this.userRepo.findById(userId);
    } catch (error) {
      if (error instanceof NotFoundError) {
        throw new ServiceError("User not found", { cause: error });
      }
      throw error; // Re-throw unexpected errors
    }
  }
}

// Controller layer - catches and responds
class UserController {
  async getProfile(req, res) {
    try {
      const user = await userService.getProfile(req.params.id);
      res.json(user);
    } catch (error) {
      if (error instanceof ServiceError) {
        res.status(404).json({ error: error.message });
      } else {
        res.status(500).json({ error: "Internal server error" });
      }
    }
  }
}
```

---

## Common Mistakes

**Mistake 1: Catching too early**

```javascript
// Bad - catches before processing
function processItems(items) {
  try {
    for (const item of items) {
      try {
        processItem(item);
      } catch (e) {
        console.log("Item failed:", e);
        // Should we continue? Break? Re-throw?
      }
    }
  } catch (e) {
    // This never runs - inner catch swallows errors
  }
}

// Better - decide at appropriate level
function processItems(items) {
  const errors = [];
  for (const item of items) {
    try {
      processItem(item);
    } catch (e) {
      errors.push({ item, error: e });
    }
  }
  if (errors.length > 0) {
    throw new BatchError(errors);
  }
}
```

**Mistake 2: Silently swallowing errors**

```javascript
// Bad - error disappears
try {
  riskyOperation();
} catch (e) {
  // Nothing here
}

// Good - at minimum, log it
try {
  riskyOperation();
} catch (e) {
  console.error("Operation failed:", e);
}
```

**Mistake 3: Not considering async propagation**

```javascript
// Bad - unhandled rejection
async function getData() {
  const response = await fetch(url);
  return response.json();
}
getData(); // If this rejects, nothing catches it

// Good - always handle async errors
getData().catch(error => console.error(error));

// Or await in async context
async function main() {
  try {
    const data = await getData();
  } catch (error) {
    console.error(error);
  }
}
```

---

## Related Topics

- [[What-is-TryCatch]]
- [[Use-TryCatch]]
- [[Throw-Errors]]
- [[Handle-AsyncErrors]]
- [[What-is-ErrorTypes]]

---

## Quick Revision

- Errors propagate up the call stack until caught
- Use try-catch at appropriate layer, not everywhere
- Re-throw with context for better debugging
- Async errors propagate through promise chains
- Uncaught errors crash the program or trigger global handlers
- Consider error propagation in layered architectures
- Always handle async promise rejections
