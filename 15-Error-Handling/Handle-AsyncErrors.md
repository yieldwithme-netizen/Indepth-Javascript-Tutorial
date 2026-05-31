# How to Handle Async Errors

## Definition

Async error handling deals with errors that occur in asynchronous operations like callbacks, promises, and async/await. These errors don't propagate through the normal call stack, requiring special handling techniques.

---

## The Problem

Async errors bypass try-catch:

```javascript
// This WON'T catch the error
try {
  setTimeout(() => {
    throw new Error("Async error");
  }, 1000);
} catch (error) {
  // Never runs!
  console.log("Caught:", error);
}
```

---

## Methods for Async Error Handling

### 1. Callbacks (Error-First Pattern)

The traditional Node.js approach:

```javascript
function fetchData(callback) {
  fs.readFile("data.json", (error, data) => {
    if (error) {
      return callback(error);
    }
    callback(null, JSON.parse(data));
  });
}

// Usage
fetchData((error, data) => {
  if (error) {
    console.error("Failed:", error.message);
    return;
  }
  console.log("Data:", data);
});
```

### 2. Promises with .catch()

```javascript
function fetchData() {
  return fetch("/api/data")
    .then(response => {
      if (!response.ok) {
        throw new Error(`HTTP ${response.status}`);
      }
      return response.json();
    })
    .catch(error => {
      console.error("Fetch failed:", error);
      throw error; // Re-throw or return fallback
    });
}

fetchData()
  .then(data => console.log(data))
  .catch(error => console.error("Final catch:", error));
```

### 3. Async/Await with Try-Catch

```javascript
async function fetchData() {
  try {
    const response = await fetch("/api/data");

    if (!response.ok) {
      throw new Error(`HTTP ${response.status}`);
    }

    const data = await response.json();
    return data;
  } catch (error) {
    console.error("Failed to fetch:", error.message);
    throw error;
  }
}

// Usage
async function main() {
  try {
    const data = await fetchData();
    console.log(data);
  } catch (error) {
    console.error("App error:", error);
  }
}
```

---

## Common Use Cases

### Fetch API Error Handling

```javascript
async function fetchUser(id) {
  try {
    const response = await fetch(`/api/users/${id}`);

    if (!response.ok) {
      const errorBody = await response.json().catch(() => ({}));
      throw new ApiError(response.status, errorBody.message);
    }

    return await response.json();
  } catch (error) {
    if (error instanceof ApiError) {
      throw error;
    }
    throw new NetworkError("Failed to connect to server", { cause: error });
  }
}
```

### Database Operations

```javascript
async function createUser(userData) {
  const connection = await pool.getConnection();
  try {
    await connection.beginTransaction();

    const result = await connection.query(
      "INSERT INTO users (name, email) VALUES (?, ?)",
      [userData.name, userData.email]
    );

    await connection.commit();
    return result.insertId;
  } catch (error) {
    await connection.rollback();
    throw new DatabaseError("Failed to create user", { cause: error });
  } finally {
    connection.release();
  }
}
```

### Parallel Async Operations

```javascript
async function loadDashboard() {
  try {
    const [users, posts, comments] = await Promise.all([
      fetchUsers(),
      fetchPosts(),
      fetchComments(),
    ]);

    return { users, posts, comments };
  } catch (error) {
    console.error("Dashboard load failed:", error);
    throw error;
  }
}

// Handle partial failures
async function loadWithFallbacks() {
  const results = await Promise.allSettled([
    fetchUsers(),
    fetchPosts(),
    fetchComments(),
  ]);

  const errors = results
    .map((r, i) => ({ status: r.status, index: i }))
    .filter(r => r.status === "rejected");

  if (errors.length > 0) {
    console.warn("Some operations failed:", errors);
  }

  return results
    .filter(r => r.status === "fulfilled")
    .map(r => r.value);
}
```

### Retry Logic

```javascript
async function fetchWithRetry(url, maxRetries = 3) {
  let lastError;

  for (let attempt = 1; attempt <= maxRetries; attempt++) {
    try {
      const response = await fetch(url);
      if (!response.ok) {
        throw new Error(`HTTP ${response.status}`);
      }
      return await response.json();
    } catch (error) {
      lastError = error;
      console.warn(`Attempt ${attempt} failed:`, error.message);

      if (attempt < maxRetries) {
        await new Promise(resolve =>
          setTimeout(resolve, Math.pow(2, attempt) * 1000)
        );
      }
    }
  }

  throw new Error(`Failed after ${maxRetries} attempts`, {
    cause: lastError,
  });
}
```

---

## Event Emitter Error Handling

```javascript
class DataLoader extends EventEmitter {
  async load(url) {
    try {
      const data = await fetch(url).then(r => r.json());
      this.emit("data", data);
    } catch (error) {
      this.emit("error", error);
    }
  }
}

const loader = new DataLoader();
loader.on("data", data => console.log("Loaded:", data));
loader.on("error", error => console.error("Failed:", error));
loader.load("/api/data");
```

---

## Common Mistakes

**Mistake 1: Unhandled promise rejections**

```javascript
// Bad - unhandled rejection
fetch("/api/data")
  .then(response => response.json())
  .then(data => console.log(data));
// If fetch fails, no catch handler

// Good - always add catch
fetch("/api/data")
  .then(response => response.json())
  .then(data => console.log(data))
  .catch(error => console.error(error));
```

**Mistake 2: Forgetting await**

```javascript
// Bad - async function returns Promise, not value
async function getData() {
  try {
    const response = fetch("/api/data"); // Missing await!
    const data = response.json(); // Missing await!
    return data;
  } catch (error) {
    // Won't catch the error
  }
}

// Good
async function getData() {
  try {
    const response = await fetch("/api/data");
    const data = await response.json();
    return data;
  } catch (error) {
    console.error(error);
  }
}
```

**Mistake 3: Mixing callbacks and promises**

```javascript
// Bad - callback won't be caught
async function saveData(callback) {
  try {
    await saveToDatabase(data);
    callback(null, success);
  } catch (error) {
    callback(error); // Callback error, not thrown
  }
}

// Good - use promises consistently
async function saveData() {
  try {
    await saveToDatabase(data);
    return success;
  } catch (error) {
    throw error;
  }
}
```

---

## Global Error Handlers

```javascript
// Browser
window.addEventListener("unhandledrejection", event => {
  console.error("Unhandled promise rejection:", event.reason);
  event.preventDefault();
});

window.addEventListener("error", event => {
  console.error("Global error:", event.error);
});

// Node.js
process.on("unhandledRejection", (reason, promise) => {
  console.error("Unhandled rejection:", reason);
});

process.on("uncaughtException", error => {
  console.error("Uncaught exception:", error);
  process.exit(1);
});
```

---

## Related Topics

- [[What-is-TryCatch]]
- [[Use-TryCatch]]
- [[What-is-Propagation]]
- [[What-is-ErrorTypes]]
- [[Throw-Errors]]

---

## Quick Revision

- Async errors don't propagate through try-catch
- Use `.catch()` for promise chains
- Use try-catch with async/await
- Always handle promise rejections
- Add await before async operations
- Use Promise.allSettled for partial failure handling
- Implement retry logic for transient failures
- Set up global handlers for unhandled rejections
