# What is Fetch API

## Definition

The **Fetch API** is a modern, built-in browser (and Node.js 18+) interface for making HTTP requests. It replaces the older `XMLHttpRequest` and returns a [[Create-Promise|Promise]] that resolves to the `Response` to that request.

## Syntax

```javascript
fetch(url, options)
  .then((response) => response.json())
  .then((data) => console.log(data))
  .catch((error) => console.error(error));
```

## Code Example: Basic GET Request

```javascript
fetch("https://jsonplaceholder.typicode.com/posts/1")
  .then((response) => response.json())
  .then((data) => console.log(data))
  .catch((err) => console.error("Error:", err));
```

## Code Example: GET with Async/Await

```javascript
async function getPost() {
  try {
    const response = await fetch("https://jsonplaceholder.typicode.com/posts/1");

    if (!response.ok) {
      throw new Error(`HTTP error! Status: ${response.status}`);
    }

    const data = await response.json();
    console.log(data);
  } catch (error) {
    console.error("Failed to fetch post:", error);
  }
}

getPost();
```

## Code Example: POST Request

```javascript
async function createPost() {
  const response = await fetch("https://jsonplaceholder.typicode.com/posts", {
    method: "POST",
    headers: {
      "Content-Type": "application/json",
    },
    body: JSON.stringify({
      title: "Hello World",
      body: "This is a new post",
      userId: 1,
    }),
  });

  const data = await response.json();
  console.log("Created:", data);
}

createPost();
```

## Code Example: PUT, PATCH, DELETE

```javascript
// PUT — full update
await fetch("https://jsonplaceholder.typicode.com/posts/1", {
  method: "PUT",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({ title: "Updated", body: "New body", userId: 1 }),
});

// PATCH — partial update
await fetch("https://jsonplaceholder.typicode.com/posts/1", {
  method: "PATCH",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({ title: "Patched title" }),
});

// DELETE
await fetch("https://jsonplaceholder.typicode.com/posts/1", {
  method: "DELETE",
});
```

## Response Object

The `response` object contains metadata about the request:

```javascript
const response = await fetch("https://jsonplaceholder.typicode.com/posts/1");

console.log(response.ok);          // true (status 200-299)
console.log(response.status);      // 200
console.log(response.statusText);  // "OK"
console.log(response.headers);     // Headers object
console.log(response.url);         // final URL after redirects

// Body methods
const data = await response.json();    // parse as JSON
const text = await response.text();    // parse as text
const blob = await response.blob();    // parse as Blob
const formData = await response.formData(); // parse as FormData
```

## Code Example: Error Handling

```javascript
async function safeFetch(url) {
  try {
    const response = await fetch(url);

    if (!response.ok) {
      throw new Error(`HTTP ${response.status}: ${response.statusText}`);
    }

    const data = await response.json();
    return { data, error: null };
  } catch (error) {
    return { data: null, error: error.message };
  }
}

const result = await safeFetch("https://api.example.com/data");
if (result.error) {
  console.error("Request failed:", result.error);
} else {
  console.log(result.data);
}
```

## Code Example: Query Parameters

```javascript
const params = new URLSearchParams({
  page: 1,
  limit: 10,
  sort: "desc",
});

const response = await fetch(`https://api.example.com/posts?${params}`);
const data = await response.json();
```

## Common Use Cases

- **Fetching data from REST APIs**
- **Submitting forms** to a backend
- **Uploading/downloading** files
- **Interacting with third-party services**

## Common Mistakes

1. **Not checking `response.ok`** — fetch only rejects on network errors, not HTTP errors
```javascript
// Bad: 404 still resolves
const res = await fetch("/nonexistent");
const data = await res.json(); // might fail silently

// Good
const res = await fetch("/nonexistent");
if (!res.ok) throw new Error(`HTTP ${res.status}`);
```

2. **Forgetting to parse the response body**
```javascript
// Bad: response object, not the data
const res = await fetch("/api/data");
console.log(res); // Response object, not JSON

// Good
const res = await fetch("/api/data");
const data = await res.json();
console.log(data);
```

3. **Not handling CORS errors** — cross-origin requests may be blocked by the server

4. **Using fetch in older environments** — add a polyfill for IE11 or use `axios`

## Quick Revision Summary

- Fetch API returns a Promise that resolves to a Response object
- Always check `response.ok` before reading the body
- Parse with `.json()`, `.text()`, `.blob()`, etc.
- Use `method`, `headers`, and `body` in the options object
- Fetch does not reject on HTTP errors (4xx, 5xx) — only on network failures

## Related Topics

- [[Make-HTTP]]
- [[Use-AsyncAwait]]
- [[Create-Promise]]
- [[What-is-ThenCatch]]
