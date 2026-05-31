# Async/Await

## Definition
`async/await` is syntactic sugar built on top of Promises that makes asynchronous code easier to read and write. It allows you to write asynchronous code that looks synchronous.

## Syntax
```javascript
async function functionName() {
  const result = await asynchronousOperation();
  return result;
}
```

## Code Examples

### Basic Async Function
```javascript
async function fetchData() {
  return 'Hello, World!';
}

// Returns a Promise
fetchData().then(data => console.log(data)); // "Hello, World!"
```

### Await with Promises
```javascript
async function getUser() {
  const response = await fetch('https://api.example.com/user');
  const user = await response.json();
  return user;
}
```

### Error Handling with Try/Catch
```javascript
async function fetchData() {
  try {
    const response = await fetch('https://api.example.com/data');
    const data = await response.json();
    return data;
  } catch (error) {
    console.error('Error:', error);
    throw error;
  }
}
```

### Parallel Execution
```javascript
async function fetchAll() {
  const [users, posts] = await Promise.all([
    fetch('/api/users').then(r => r.json()),
    fetch('/api/posts').then(r => r.json())
  ]);
  return { users, posts };
}
```

### Async Arrow Functions
```javascript
const greet = async (name) => {
  const message = `Hello, ${name}!`;
  return message;
};

// Async IIFE
(async () => {
  const result = await greet('Alice');
  console.log(result);
})();
```

### Sequential vs Parallel
```javascript
// Sequential (slower)
async function sequential() {
  const a = await fetch('/api/a');
  const b = await fetch('/api/b');
  return [a, b];
}

// Parallel (faster)
async function parallel() {
  const [a, b] = await Promise.all([
    fetch('/api/a'),
    fetch('/api/b')
  ]);
  return [a, b];
}
```

### Async Methods in Classes
```javascript
class DataLoader {
  async load(url) {
    const response = await fetch(url);
    return response.json();
  }

  async loadAll(urls) {
    return Promise.all(urls.map(url => this.load(url)));
  }
}
```

### Await in Loops
```javascript
async function processItems(items) {
  const results = [];
  for (const item of items) {
    const result = await processItem(item);
    results.push(result);
  }
  return results;
}

// Parallel alternative
async function processAllItems(items) {
  return Promise.all(items.map(item => processItem(item)));
}
```

## Common Use Cases
- API calls
- File operations
- Database queries
- Any Promise-based operation

## Common Mistakes
- **Missing `async` keyword**: Functions using `await` must be `async`
- **Unnecessary `await`**: Don't `await` non-Promise values
- **Not handling errors**: Always use `try/catch` or `.catch()`
- **Sequential when parallel is possible**: Use `Promise.all()` for independent operations

## Related Topics
- [[Promise]]
- [[Callbacks]]
- [[Event-Loop]]
- [[AJAX]]
- [[Fetch-API]]

## Quick Revision
- `async` declares an asynchronous function
- `await` pauses execution until Promise resolves
- Returns Promises automatically
- Use `try/catch` for error handling
- Use `Promise.all()` for parallel execution
