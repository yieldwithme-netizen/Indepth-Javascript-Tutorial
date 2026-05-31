# Promises in JavaScript

## Definition

A Promise is an object representing the eventual completion or failure of an asynchronous operation. Promises provide a cleaner alternative to callbacks for handling async code, avoiding "callback hell" and enabling better error handling.

---

## Promise States

A Promise is in one of three states:

1. **Pending**: Initial state, operation not completed
2. **Fulfilled**: Operation completed successfully
3. **Rejected**: Operation failed

```javascript
const promise = new Promise((resolve, reject) => {
  // Do async work
  const success = true;

  if (success) {
    resolve("Done!"); // Fulfilled
  } else {
    reject(new Error("Failed!")); // Rejected
  }
});

console.log(promise); // Promise { <fulfilled>: "Done!" }
```

---

## Creating Promises

### Basic Promise

```javascript
const delay = (ms) => new Promise(resolve => setTimeout(resolve, ms));

delay(1000).then(() => console.log("1 second later"));
```

### Promisifying Callbacks

```javascript
// Callback style
function readFile(path, callback) {
  fs.readFile(path, "utf8", callback);
}

// Promisified version
function readFileAsync(path) {
  return new Promise((resolve, reject) => {
    fs.readFile(path, "utf8", (err, data) => {
      if (err) reject(err);
      else resolve(data);
    });
  });
}

// Using built-in promisify
import { promisify } from "util";
const readFileAsync = promisify(fs.readFile);
```

### Promise.all() for Parallel Operations

```javascript
const fetchUser = (id) => fetch(`/api/users/${id}`).then(r => r.json());
const fetchPosts = (id) => fetch(`/api/posts?userId=${id}`).then(r => r.json());

// Both run in parallel
const [user, posts] = await Promise.all([
  fetchUser(1),
  fetchPosts(1)
]);

console.log(user);
console.log(posts);
```

### Promise.allSettled() - Wait for All

```javascript
const promises = [
  Promise.resolve("Success 1"),
  Promise.reject("Error"),
  Promise.resolve("Success 2")
];

const results = await Promise.allSettled(promises);
console.log(results);
// [
//   { status: "fulfilled", value: "Success 1" },
//   { status: "rejected", reason: "Error" },
//   { status: "fulfilled", value: "Success 2" }
// ]

// Check for failures
const failures = results.filter(r => r.status === "rejected");
```

### Promise.race() - First to Settle

```javascript
const slow = new Promise(resolve => setTimeout(() => resolve("slow"), 3000));
const fast = new Promise(resolve => setTimeout(() => resolve("fast"), 1000));

const result = await Promise.race([slow, fast]);
console.log(result); // "fast"
```

### Promise.any() - First to Succeed

```javascript
const promises = [
  Promise.reject("Error 1"),
  Promise.reject("Error 2"),
  Promise.resolve("Success")
];

const result = await Promise.any(promises);
console.log(result); // "Success"

// If all fail
try {
  await Promise.any([
    Promise.reject("Error 1"),
    Promise.reject("Error 2")
  ]);
} catch (err) {
  console.log(err); // AggregateError
}
```

---

## Async/Await

### Basic Usage

```javascript
async function fetchUser(id) {
  const response = await fetch(`/api/users/${id}`);

  if (!response.ok) {
    throw new Error(`HTTP error! status: ${response.status}`);
  }

  return response.json();
}

// Usage
try {
  const user = await fetchUser(1);
  console.log(user);
} catch (err) {
  console.error("Failed to fetch user:", err);
}
```

### Sequential vs Parallel

```javascript
// Sequential (slower)
async function sequential() {
  const user = await fetchUser(1);
  const posts = await fetchPosts(user.id);
  const comments = await fetchComments(posts[0].id);
  return { user, posts, comments };
}

// Parallel (faster)
async function parallel() {
  const [user, posts] = await Promise.all([
    fetchUser(1),
    fetchPosts(1)
  ]);
  const comments = await fetchComments(posts[0].id);
  return { user, posts, comments };
}
```

### Error Handling Patterns

```javascript
// Try-catch (cleanest)
async function getData() {
  try {
    const data = await fetch("/api/data");
    return await data.json();
  } catch (err) {
    console.error("Fetch failed:", err);
    return null;
  }
}

// .catch() on promise
const data = await fetch("/api/data")
  .then(r => r.json())
  .catch(err => {
    console.error(err);
    return null;
  });

// Error boundary pattern
async function withErrorBoundary(fn, fallback) {
  try {
    return await fn();
  } catch (err) {
    console.error(err);
    return fallback;
  }
}

const data = await withErrorBoundary(
  () => fetch("/api/data").then(r => r.json()),
  { items: [] }
);
```

---

## Common Use Cases

### Debouncing with Promises

```javascript
function debounce(fn, delay) {
  let timeoutId;

  return function(...args) {
    clearTimeout(timeoutId);

    return new Promise(resolve => {
      timeoutId = setTimeout(() => {
        resolve(fn.apply(this, args));
      }, delay);
    });
  };
}

const debouncedSearch = debounce(async (query) => {
  const results = await fetch(`/api/search?q=${query}`);
  return results.json();
}, 300);
```

### Rate Limiting

```javascript
class RateLimiter {
  constructor(limit, interval) {
    this.limit = limit;
    this.interval = interval;
    this.queue = [];
    this.running = 0;
  }

  async add(fn) {
    while (this.running >= this.limit) {
      await new Promise(resolve => setTimeout(resolve, 100));
    }

    this.running++;
    try {
      return await fn();
    } finally {
      this.running--;
      setTimeout(() => {
        if (this.queue.length > 0) {
          const next = this.queue.shift();
          this.add(next);
        }
      }, this.interval);
    }
  }
}
```

### Retry Logic

```javascript
async function withRetry(fn, maxRetries = 3, delay = 1000) {
  for (let i = 0; i < maxRetries; i++) {
    try {
      return await fn();
    } catch (err) {
      if (i === maxRetries - 1) throw err;
      console.log(`Attempt ${i + 1} failed, retrying in ${delay}ms...`);
      await new Promise(resolve => setTimeout(resolve, delay));
      delay *= 2; // Exponential backoff
    }
  }
}

// Usage
const data = await withRetry(() => fetch("/api/data").then(r => r.json()));
```

### Promise Pool (Concurrency Control)

```javascript
async function promisePool(tasks, concurrency) {
  const results = [];
  const executing = new Set();

  for (const [i, task] of tasks.entries()) {
    const promise = task().then(result => {
      executing.delete(promise);
      results[i] = result;
    });

    executing.add(promise);

    if (executing.size >= concurrency) {
      await Promise.race(executing);
    }
  }

  await Promise.all(executing);
  return results;
}
```

---

## Common Mistakes

### Mistake 1: Not Returning Promises

```javascript
// Wrong: fire and forget
async function process() {
  fetch("/api/data"); // Not awaited!
}

// Correct: return or await
async function process() {
  await fetch("/api/data");
}
```

### Mistake 2: Swallowing Errors

```javascript
// Wrong: errors disappear
async function getData() {
  try {
    return await fetch("/api/data");
  } catch (err) {
    // Error is swallowed!
  }
}

// Correct: rethrow or handle
async function getData() {
  try {
    return await fetch("/api/data");
  } catch (err) {
    console.error(err);
    throw err; // Rethrow
    // Or return fallback: return null;
  }
}
```

### Mistake 3: Sequential When Parallel Is Better

```javascript
// Wrong: waits for each to complete
const user = await fetchUser(1);
const posts = await fetchPosts(1);
const comments = await fetchComments(1);

// Correct: runs in parallel
const [user, posts, comments] = await Promise.all([
  fetchUser(1),
  fetchPosts(1),
  fetchComments(1)
]);
```

### Mistake 4: Creating Promises Incorrectly

```javascript
// Wrong: antipattern (promise constructor antipattern)
const getData = () => new Promise((resolve, reject) => {
  fetch("/api/data")
    .then(r => r.json())
    .then(resolve)
    .catch(reject);
});

// Correct: fetch already returns a promise
const getData = () => fetch("/api/data").then(r => r.json());
```

### Mistake 5: Forgetting await

```javascript
// Wrong: missing await
async function test() {
  const promise = fetch("/api/data");
  console.log(promise); // Promise object, not data!
}

// Correct
async function test() {
  const response = await fetch("/api/data");
  const data = await response.json();
  console.log(data);
}
```

---

## Quick Revision Summary

| Method | Description | Use When |
|--------|-------------|----------|
| `Promise.all()` | Wait for all | All must succeed |
| `Promise.allSettled()` | Wait for all | Need all results |
| `Promise.race()` | First to settle | Timeout pattern |
| `Promise.any()` | First to succeed | Fallback sources |
| `async/await` | Sequential async | Clean async code |
| `.then().catch()` | Promise chaining | Simple transformations |

---

## Related Topics

- [[this]] - `this` in promise callbacks
- [[Array]] - Array methods with async operations
- [[debugging]] - Debugging async code
- [[loop]] - Async iteration patterns
- [[API-Design]] - Async API handlers
