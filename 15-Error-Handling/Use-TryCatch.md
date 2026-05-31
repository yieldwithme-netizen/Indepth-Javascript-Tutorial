# How to Use Try-Catch-Finally

## Definition

`try-catch-finally` is JavaScript's complete error handling mechanism. The `finally` block runs regardless of whether an error occurred, making it ideal for cleanup operations like closing files, releasing resources, or resetting state.

---

## Syntax

```javascript
try {
  // Code that might throw an error
} catch (error) {
  // Code that runs if an error occurs
} finally {
  // Code that ALWAYS runs
}
```

---

## The Three Blocks

### try Block

Contains code that might throw an error. If any statement throws, execution jumps to catch.

```javascript
try {
  console.log("Starting...");
  const result = riskyOperation();
  console.log("Result:", result);
} catch (error) {
  console.log("Something went wrong");
}
```

### catch Block

Handles the error. Runs only if an error is thrown in try.

```javascript
try {
  undefined.name;
} catch (error) {
  // Error object is automatically available
  console.log(error.message);
  console.log(error.stack);
}
```

### finally Block

Runs ALWAYS - whether try succeeds, catch handles an error, or an error propagates.

```javascript
try {
  console.log("try");
  riskyOperation();
} catch (error) {
  console.log("catch");
} finally {
  console.log("finally"); // Always runs
}

// Output (if error occurs):
// try
// catch
// finally

// Output (if no error):
// try
// finally
```

---

## Complete Examples

### File Operation Simulation

```javascript
let file = null;

function readFile(filename) {
  try {
    file = openFile(filename); // Hypothetical function
    const data = file.read();
    return data;
  } catch (error) {
    console.error(`Failed to read ${filename}:`, error.message);
    return null;
  } finally {
    if (file) {
      file.close(); // Always close the file
      console.log("File closed");
    }
  }
}
```

### Database Connection

```javascript
async function fetchData(query) {
  let connection = null;
  try {
    connection = await db.connect();
    const result = await connection.query(query);
    return result;
  } catch (error) {
    console.error("Query failed:", error.message);
    throw error;
  } finally {
    if (connection) {
      connection.release(); // Always release connection
    }
  }
}
```

### Transaction Rollback

```javascript
async function transferMoney(from, to, amount) {
  const transaction = await db.beginTransaction();
  try {
    await transaction.debit(from, amount);
    await transaction.credit(to, amount);
    await transaction.commit();
  } catch (error) {
    await transaction.rollback(); // Undo on error
    throw error;
  } finally {
    transaction.release();
  }
}
```

---

## Finally Block Behavior

The finally block runs in specific scenarios:

```javascript
// Scenario 1: No error
try {
  return "success";
} finally {
  console.log("finally runs"); // Logs
}
// Returns "success"

// Scenario 2: Error caught
try {
  throw new Error("oops");
} catch (e) {
  return "caught";
} finally {
  console.log("finally runs"); // Logs
}
// Returns "caught"

// Scenario 3: Error not caught
try {
  throw new Error("oops");
} finally {
  console.log("finally runs"); // Logs
  console.log("error propagates");
}
// Error propagates after finally
```

**Warning**: Return values in finally override try/catch returns:

```javascript
function example() {
  try {
    return "from try";
  } finally {
    return "from finally"; // Overrides try's return
  }
}

example(); // "from finally"
```

---

## Common Use Cases

### Resource Cleanup

```javascript
function processData(data) {
  const resource = acquireResource();
  try {
    return resource.process(data);
  } finally {
    resource.release();
  }
}
```

### State Reset

```javascript
class DataLoader {
  constructor() {
    this.loading = false;
  }

  async load(url) {
    this.loading = true;
    try {
      const response = await fetch(url);
      return await response.json();
    } catch (error) {
      console.error("Load failed:", error);
      throw error;
    } finally {
      this.loading = false; // Always reset
    }
  }
}
```

### Timer Cleanup

```javascript
function withTimeout(promise, ms) {
  const controller = new AbortController();
  const timeout = setTimeout(() => controller.abort(), ms);

  return promise
    .then(result => {
      clearTimeout(timeout);
      return result;
    })
    .catch(error => {
      clearTimeout(timeout);
      throw error;
    });
}
```

---

## Common Mistakes

**Mistake 1: Forgetting finally for cleanup**

```javascript
// Bad - resource leak if error occurs
async function getData() {
  const connection = await db.connect();
  const result = await connection.query("SELECT * FROM users");
  connection.close();
  return result;
}

// Good - always cleanup
async function getData() {
  const connection = await db.connect();
  try {
    return await connection.query("SELECT * FROM users");
  } finally {
    connection.close();
  }
}
```

**Mistake 2: Missing catch or finally**

```javascript
// Syntax error - must have catch or finally
try {
  riskyOperation();
} finally {
  cleanup();
}

// This is valid but rarely useful
```

**Mistake 3: Swallowing errors in finally**

```javascript
// Bad - hides original error
try {
  riskyOperation();
} catch (e) {
  throw new Error("Wrapped");
} finally {
  throw new Error("Cleanup failed"); // Overrides original
}
```

---

## Related Topics

- [[What-is-TryCatch]]
- [[What-is-ErrorTypes]]
- [[Throw-Errors]]
- [[Handle-AsyncErrors]]
- [[What-is-Propagation]]

---

## Quick Revision

- `try`: Contains code that might throw
- `catch`: Handles errors if they occur
- `finally`: Always runs for cleanup
- finally runs even with return statements
- Return in finally overrides try/catch returns
- Use finally for resource cleanup (files, connections, timers)
- Always pair try with either catch or finally (or both)
