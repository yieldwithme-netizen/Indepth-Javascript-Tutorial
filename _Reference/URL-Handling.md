# URL Handling

## Definition

URL handling in JavaScript involves parsing, constructing, modifying, and validating URLs. The `URL` and `URLSearchParams` APIs (built into modern browsers and Node.js) provide a standardized way to work with web addresses without manual string manipulation.

## Code Examples

### Parsing a URL

```javascript
const url = new URL(
  "https://example.com:8080/path/page?name=Alice&age=30#section1"
);

console.log(url.protocol);  // "https:"
console.log(url.hostname);  // "example.com"
console.log(url.port);      // "8080"
console.log(url.pathname);  // "/path/page"
console.log(url.search);    // "?name=Alice&age=30"
console.log(url.hash);      // "#section1"
console.log(url.origin);    // "https://example.com:8080"
```

### Building a URL

```javascript
const base = new URL("https://api.example.com");
const endpoint = new URL("/users", base);

endpoint.searchParams.set("page", "1");
endpoint.searchParams.set("limit", "10");
endpoint.searchParams.append("sort", "name");

console.log(endpoint.toString());
// "https://api.example.com/users?page=1&limit=10&sort=name"
```

### URLSearchParams

```javascript
const params = new URLSearchParams("name=Alice&age=30&city=NYC");

// Get values
console.log(params.get("name"));  // "Alice"
console.log(params.get("age"));   // "30"

// Set and append
params.set("age", "31");
params.append("hobby", "reading");

// Check existence
console.log(params.has("name"));  // true

// Delete
params.delete("city");

// Iterate
for (const [key, value] of params) {
  console.log(`${key}: ${value}`);
}

// Convert to string
console.log(params.toString());
// "name=Alice&age=31&hobby=reading"
```

### Working with Query Strings

```javascript
// Parse query string to object
function parseQueryString(queryString) {
  const params = new URLSearchParams(queryString);
  const result = {};
  for (const [key, value] of params) {
    result[key] = value;
  }
  return result;
}

// Build query string from object
function buildQueryString(obj) {
  const params = new URLSearchParams(obj);
  return params.toString();
}

console.log(parseQueryString("name=Alice&age=30"));
// { name: "Alice", age: "30" }

console.log(buildQueryString({ name: "Bob", age: 25 }));
// "name=Bob&age=25"
```

### Encoding and Decoding

```javascript
// Encode special characters
const encoded = encodeURIComponent("hello world & foo=bar");
console.log(encoded); // "hello%20world%20%26%20foo%3Dbar"

// Decode
const decoded = decodeURIComponent(encoded);
console.log(decoded); // "hello world & foo=bar"

// Encode URI (less aggressive)
const uri = encodeURI("https://example.com/path with spaces");
console.log(uri); // "https://example.com/path%20with%20spaces"

// Decode URI
console.log(decodeURI(uri)); // "https://example.com/path with spaces"
```

### Dynamic URL Construction

```javascript
function buildAPIUrl(base, endpoint, params = {}) {
  const url = new URL(endpoint, base);

  Object.entries(params).forEach(([key, value]) => {
    if (value !== undefined && value !== null) {
      url.searchParams.set(key, String(value));
    }
  });

  return url.toString();
}

const apiUrl = buildAPIUrl("https://api.example.com", "/users", {
  page: 1,
  limit: 10,
  search: "john doe",
});

console.log(apiUrl);
// "https://api.example.com/users?page=1&limit=10&search=john%20doe"
```

## Common Use Cases

- **API requests** — Construct API URLs with query parameters
- **Form handling** — Extract form data from URL parameters
- **Routing** — Parse URLs for client-side routing
- **Analytics** — Track URL parameters
- **Redirects** — Build redirect URLs dynamically
- **Deep linking** — Share specific app states via URLs

## Common Mistakes

```javascript
// Mistake 1: Manually concatenating query strings
// Bad:
const badUrl = baseUrl + "?name=" + name + "&age=" + age;
// If name has special chars, this breaks

// Good:
const url = new URL("/path", baseUrl);
url.searchParams.set("name", name);

// Mistake 2: Not encoding user input
// Bad: XSS vulnerability
const userInput = '<script>alert("xss")</script>';
const bad = `https://example.com/search?q=${userInput}`;

// Good: encode user input
const safe = new URL("https://example.com/search");
safe.searchParams.set("q", userInput);

// Mistake 3: Confusing encodeURIComponent vs encodeURI
// encodeURIComponent: encodes everything except A-Z a-z 0-9 - _ . ! ~ * ' ( )
// encodeURI: preserves common URI characters like / ? : @ & = +

// Mistake 4: Not handling missing params
const searchParams = new URLSearchParams(window.location.search);
// const page = searchParams.get("page"); // Could be null
const page = parseInt(searchParams.get("page"), 10) || 1; // Safe
```

## Related Topics

- [[Fetch-API]]
- [[DOM-Manipulation]]
- [[Event-Handling]]
- [[Node-JS]]
- [[Async-Await]]
- [[String-Methods]]

## Quick Revision

| API | Purpose |
|-----|---------|
| `URL` | Parse and construct full URLs |
| `URLSearchParams` | Work with query parameters |
| `encodeURIComponent` | Encode string for URL parameters |
| `decodeURIComponent` | Decode URL-encoded strings |
| `encodeURI` | Encode full URI |
| `decodeURI` | Decode full URI |

| Method | Description |
|--------|-------------|
| `new URL()` | Parse or construct a URL |
| `searchParams.get()` | Get a parameter value |
| `searchParams.set()` | Set or update a parameter |
| `searchParams.append()` | Add a parameter |
| `searchParams.delete()` | Remove a parameter |
| `toString()` | Get full URL string |
