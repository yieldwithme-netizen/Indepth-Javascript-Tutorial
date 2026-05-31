# How to Make HTTP Requests

## Definition

Making HTTP requests allows your JavaScript application to communicate with servers — fetching data, sending data, updating resources, and more. The primary tools are the [[What-is-Fetch|Fetch API]], `XMLHttpRequest` (legacy), and third-party libraries like `axios`.

## Methods Overview

| Method | Purpose | Body | Idempotent |
|--------|---------|------|------------|
| `GET` | Retrieve data | No | Yes |
| `POST` | Create data | Yes | No |
| `PUT` | Replace data | Yes | Yes |
| `PATCH` | Partial update | Yes | No |
| `DELETE` | Remove data | Optional | Yes |

## Code Example: GET Request

```javascript
async function getUsers() {
  const response = await fetch("https://jsonplaceholder.typicode.com/users");

  if (!response.ok) {
    throw new Error(`HTTP error: ${response.status}`);
  }

  const users = await response.json();
  return users;
}

getUsers().then((users) => users.forEach((u) => console.log(u.name)));
```

## Code Example: POST Request

```javascript
async function createUser(userData) {
  const response = await fetch("https://jsonplaceholder.typicode.com/users", {
    method: "POST",
    headers: {
      "Content-Type": "application/json",
      Authorization: "Bearer YOUR_TOKEN",
    },
    body: JSON.stringify(userData),
  });

  if (!response.ok) {
    throw new Error(`Failed to create user: ${response.status}`);
  }

  return response.json();
}

createUser({
  name: "Alice",
  email: "alice@example.com",
  role: "admin",
});
```

## Code Example: Reusable HTTP Client

```javascript
class HttpClient {
  constructor(baseURL, defaultHeaders = {}) {
    this.baseURL = baseURL;
    this.defaultHeaders = defaultHeaders;
  }

  async request(endpoint, options = {}) {
    const url = `${this.baseURL}${endpoint}`;
    const config = {
      headers: {
        "Content-Type": "application/json",
        ...this.defaultHeaders,
        ...options.headers,
      },
      ...options,
    };

    const response = await fetch(url, config);

    if (!response.ok) {
      const error = await response.json().catch(() => ({}));
      throw new Error(error.message || `HTTP ${response.status}`);
    }

    if (response.status === 204) return null;
    return response.json();
  }

  get(endpoint, options) {
    return this.request(endpoint, { ...options, method: "GET" });
  }

  post(endpoint, body, options) {
    return this.request(endpoint, {
      ...options,
      method: "POST",
      body: JSON.stringify(body),
    });
  }

  put(endpoint, body, options) {
    return this.request(endpoint, {
      ...options,
      method: "PUT",
      body: JSON.stringify(body),
    });
  }

  patch(endpoint, body, options) {
    return this.request(endpoint, {
      ...options,
      method: "PATCH",
      body: JSON.stringify(body),
    });
  }

  delete(endpoint, options) {
    return this.request(endpoint, { ...options, method: "DELETE" });
  }
}

// Usage
const api = new HttpClient("https://jsonplaceholder.typicode.com");

const users = await api.get("/users");
const post = await api.post("/posts", { title: "New", body: "Content", userId: 1 });
await api.delete("/posts/1");
```

## Code Example: Error Handling Wrapper

```javascript
async function httpGet(url) {
  try {
    const response = await fetch(url, {
      method: "GET",
      headers: { Accept: "application/json" },
    });

    if (!response.ok) {
      throw new Error(`HTTP ${response.status}: ${response.statusText}`);
    }

    return { data: await response.json(), error: null };
  } catch (error) {
    return { data: null, error: error.message };
  }
}

// Usage
const { data, error } = await httpGet("https://api.example.com/data");
if (error) {
  console.error("Request failed:", error);
} else {
  console.log("Success:", data);
}
```

## Code Example: Request with Timeout

```javascript
async function fetchWithTimeout(url, options = {}, timeoutMs = 5000) {
  const controller = new AbortController();
  const timeoutId = setTimeout(() => controller.abort(), timeoutMs);

  try {
    const response = await fetch(url, {
      ...options,
      signal: controller.signal,
    });

    clearTimeout(timeoutId);
    return response;
  } catch (error) {
    clearTimeout(timeoutId);
    if (error.name === "AbortError") {
      throw new Error(`Request timed out after ${timeoutMs}ms`);
    }
    throw error;
  }
}

const data = await fetchWithTimeout("https://api.example.com/data", {}, 3000);
```

## Code Example: Using Axios (Third-Party)

```javascript
import axios from "axios";

// Basic usage
const response = await axios.get("https://jsonplaceholder.typicode.com/users");
console.log(response.data);

// POST with data
const newUser = await axios.post("https://jsonplaceholder.typicode.com/users", {
  name: "Bob",
  email: "bob@example.com",
});

// Axios automatically parses JSON — no need for response.json()
```

## Common Headers

```javascript
const headers = {
  "Content-Type": "application/json",
  Authorization: "Bearer YOUR_TOKEN",
  Accept: "application/json",
  "Cache-Control": "no-cache",
};
```

## Common Mistakes

1. **Not checking response status**
```javascript
// Bad: 404 or 500 responses still resolve
const res = await fetch("/api/data");
const data = await res.json(); // might be an error page

// Good
const res = await fetch("/api/data");
if (!res.ok) throw new Error(`HTTP ${res.status}`);
const data = await res.json();
```

2. **Missing Content-Type header on POST**
```javascript
// Bad: server may not parse the body
fetch("/api/data", {
  method: "POST",
  body: JSON.stringify({ name: "Alice" }),
});

// Good
fetch("/api/data", {
  method: "POST",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({ name: "Alice" }),
});
```

3. **Not handling network errors** — always use `try/catch`
4. **Sending credentials in URLs** — use headers instead
5. **Forgetting CORS** — the server must allow cross-origin requests

## Quick Revision Summary

- Use GET to fetch, POST to create, PUT to replace, PATCH to update, DELETE to remove
- Always check `response.ok` and handle errors with `try/catch`
- Set `Content-Type: application/json` for JSON payloads
- Consider creating a reusable HTTP client for large projects
- Use `AbortController` for request timeouts
- For complex projects, consider libraries like `axios`

## Related Topics

- [[What-is-Fetch]]
- [[Use-AsyncAwait]]
- [[Create-Promise]]
- [[What-is-ThenCatch]]
- [[What-is-Async]]
