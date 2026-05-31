# Default Parameters

Default parameters allow you to initialize named parameters with default values when no value or `undefined` is passed. They were introduced in ES6 (2015).

## Basic Syntax

```javascript
// Old way (pre-ES6)
function greet(name) {
  name = name || 'World';
  console.log(`Hello, ${name}!`);
}

// Modern way (ES6+)
function greet(name = 'World') {
  console.log(`Hello, ${name}!`);
}

greet('Alice'); // "Hello, Alice!"
greet();        // "Hello, World!"
greet(undefined); // "Hello, World!"
```

## Multiple Default Parameters

```javascript
function createUser(name = 'Anonymous', age = 0, role = 'user') {
  return { name, age, role };
}

createUser('Alice', 25, 'admin'); // { name: 'Alice', age: 25, role: 'admin' }
createUser('Bob', 30);            // { name: 'Bob', age: 30, role: 'user' }
createUser();                     // { name: 'Anonymous', age: 0, role: 'user' }
```

## Default Parameters with Expressions

```javascript
function getTimestamp(offset = 0) {
  return Date.now() + offset * 1000;
}

function multiply(a, b = a * 2) {
  return a * b;
}

multiply(5);    // 10 (b defaults to 10)
multiply(5, 3); // 15
```

## Default Parameters with Destructuring

```javascript
function processOptions({
  width = 100,
  height = 100,
  color = 'black',
  opacity = 1
} = {}) {
  return { width, height, color, opacity };
}

processOptions({ width: 200, color: 'red' });
// { width: 200, height: 100, color: 'red', opacity: 1 }

processOptions();
// { width: 100, height: 100, color: 'black', opacity: 1 }
```

## Practical Examples

### Configuration Object

```javascript
function createButton({
  text = 'Click me',
  color = 'blue',
  size = 'medium',
  disabled = false
} = {}) {
  const button = document.createElement('button');
  button.textContent = text;
  button.className = `btn btn-${color} btn-${size}`;
  button.disabled = disabled;
  return button;
}

createButton();
createButton({ text: 'Submit', color: 'green', size: 'large' });
```

### API Request Handler

```javascript
async function fetchData(url, options = {}) {
  const {
    method = 'GET',
    headers = { 'Content-Type': 'application/json' },
    body = null,
    timeout = 5000
  } = options;

  const controller = new AbortController();
  const timeoutId = setTimeout(() => controller.abort(), timeout);

  try {
    const response = await fetch(url, {
      method,
      headers,
      body: body ? JSON.stringify(body) : null,
      signal: controller.signal
    });
    clearTimeout(timeoutId);
    return response.json();
  } catch (error) {
    clearTimeout(timeoutId);
    throw error;
  }
}

// Usage
await fetchData('/api/users');
await fetchData('/api/users', { method: 'POST', body: { name: 'Alice' } });
```

### Array Operations

```javascript
function chunk(array, size = 10) {
  const chunks = [];
  for (let i = 0; i < array.length; i += size) {
    chunks.push(array.slice(i, i + size));
  }
  return chunks;
}

function flatten(arrays, depth = Infinity) {
  return arrays.reduce((acc, val) => 
    Array.isArray(val) && depth > 0
      ? acc.concat(flatten(val, depth - 1))
      : acc.concat(val)
  , []);
}
```

## Common Use Cases

- Configuration objects
- Optional function arguments
- API request options
- UI component props
- Utility functions

## Common Mistakes

1. **Confusing `null` with `undefined`** - Default parameters only apply to `undefined`
2. **Side effects in defaults** - Don't perform operations in parameter defaults
3. **Mutable defaults** - Avoid objects/arrays as defaults (they're shared)
4. **Overcomplicating** - Keep defaults simple
5. **Order matters** - Default parameters must come after required parameters

```javascript
// Correct: Required parameters before defaults
function example(required, optional = 'default') {}

// Wrong: Will cause syntax error
function example(optional = 'default', required) {}
```

## Advanced Patterns

### Default with Validation

```javascript
function divide(a, b = 1) {
  if (b === 0) {
    throw new Error('Division by zero');
  }
  return a / b;
}
```

### Lazy Evaluation

```javascript
function heavyComputation(value = Math.random()) {
  // Computation only happens if parameter is not provided
  return value * 2;
}
```

### Spread with Defaults

```javascript
function mergeOptions(...options) {
  const defaults = { theme: 'light', language: 'en' };
  return { ...defaults, ...options };
}
```

## Related Topics

- [[Functions]]
- [[Destructuring]]
- [[Spread-Operator]]
- [[Rest-Parameters]]
- [[ES6-Features]]

## Quick Revision

| Feature | Description |
|---------|-------------|
| Syntax | `param = defaultValue` |
| Triggered by | `undefined` values |
| Multiple | Supported |
| Expressions | Allowed as defaults |
| Destructuring | Works with objects/arrays |

Default parameters make functions more flexible and reduce the need for null checks and conditional logic.