# How to Use Async/Await

## Definition

`async/await` is syntactic sugar built on top of [[Create-Promise|Promises]]. It lets you write asynchronous code that **looks and behaves like synchronous code**, making it easier to read and maintain.

- `async` — marks a function as asynchronous (always returns a promise)
- `await` — pauses execution inside an async function until a promise resolves

## Syntax

```javascript
async function myFunction() {
  const result = await someAsyncOperation();
  return result;
}
```

## Code Example: Basic Usage

```javascript
function fetchUser() {
  return new Promise((resolve) => {
    setTimeout(() => resolve({ id: 1, name: "Alice" }), 1000);
  });
}

async function displayUser() {
  console.log("Loading...");
  const user = await fetchUser();
  console.log("User:", user.name);
  console.log("Done");
}

displayUser();

// Output:
// Loading...
// (1 second pause)
// User: Alice
// Done
```

## Code Example: Sequential Async Operations

```javascript
function getUser() {
  return new Promise((resolve) => setTimeout(() => resolve({ id: 1 }), 500));
}

function getPosts(userId) {
  return new Promise((resolve) =>
    setTimeout(() => resolve([{ id: 1 }, { id: 2 }]), 500)
  );
}

function getComments(postId) {
  return new Promise((resolve) =>
    setTimeout(() => resolve(["Great!", "Thanks"]), 500)
  );
}

async function loadContent() {
  const user = await getUser();
  console.log("User:", user.id);

  const posts = await getPosts(user.id);
  console.log("Posts:", posts.length);

  const comments = await getComments(posts[0].id);
  console.log("Comments:", comments);
}

loadContent();
```

## Code Example: Error Handling with try/catch

```javascript
async function fetchData() {
  try {
    const response = await fetch("https://jsonplaceholder.typicode.com/todos/1");
    const data = await response.json();
    console.log(data);
  } catch (error) {
    console.error("Failed to fetch:", error.message);
  }
}

fetchData();
```

## Code Example: Parallel Operations

```javascript
async function loadDashboard() {
  // Run all three in parallel (much faster than sequential)
  const [user, posts, notifications] = await Promise.all([
    fetchUser(),
    fetchPosts(),
    fetchNotifications(),
  ]);

  console.log(user, posts, notifications);
}
```

## Code Example: Async Arrow Functions

```javascript
const getData = async () => {
  const response = await fetch("https://api.example.com/data");
  const data = await response.json();
  return data;
};

getData().then((data) => console.log(data));
```

## Code Example: Async Methods in Classes

```javascript
class ApiService {
  constructor(baseUrl) {
    this.baseUrl = baseUrl;
  }

  async getUser(id) {
    const res = await fetch(`${this.baseUrl}/users/${id}`);
    if (!res.ok) throw new Error("User not found");
    return res.json();
  }

  async getPosts(userId) {
    const res = await fetch(`${this.baseUrl}/users/${userId}/posts`);
    return res.json();
  }
}

const api = new ApiService("https://api.example.com");
const user = await api.getUser(1);
```

## Common Mistakes

1. **Using `await` outside an `async` function**
```javascript
// Bad (module-level top-level await works, but not in regular functions)
// const data = await fetchData();

// Good
async function init() {
  const data = await fetchData();
}
init();
```

2. **Forgetting to handle errors**
```javascript
// Bad: unhandled rejection
async function risky() {
  const data = await fetch("https://bad-url.example.com");
  return data.json();
}

// Good: try/catch
async function safe() {
  try {
    const data = await fetch("https://bad-url.example.com");
    return await data.json();
  } catch (err) {
    console.error(err);
    return null;
  }
}
```

3. **Unnecessary sequential awaits** — use `Promise.all` for independent operations
```javascript
// Slow: sequential
async function load() {
  const user = await fetchUser();
  const posts = await fetchPosts();
}

// Fast: parallel
async function load() {
  const [user, posts] = await Promise.all([fetchUser(), fetchPosts()]);
}
```

4. **Forgetting that async functions always return promises**
```javascript
async function getValue() {
  return 42;
}

getValue().then((val) => console.log(val)); // 42
```

## Quick Revision Summary

- `async` functions always return a Promise
- `await` pauses until a Promise resolves — does not block the thread
- Use `try/catch` for error handling in async functions
- Use `Promise.all` for parallel independent operations
- Async/await replaces complex `.then()` chains with linear code

## Related Topics

- [[Create-Promise]]
- [[What-is-ThenCatch]]
- [[What-is-Fetch]]
- [[Make-HTTP]]
- [[What-is-PromiseAll]]
- [[What-is-PromiseRace]]
