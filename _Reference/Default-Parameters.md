# Default Parameters

## Definition

**Default parameters** allow named function arguments to be initialized with default values if no value or `undefined` is passed. Introduced in ES6, they eliminate the need for manual default value assignments inside functions.

## Basic Syntax

```javascript
function greet(name = "Guest") {
  return `Hello, ${name}!`;
}

greet("Alice"); // "Hello, Alice!"
greet();        // "Hello, Guest!"
greet(undefined); // "Hello, Guest!"
greet(null);      // "Hello, null!" (null is NOT undefined)
```

## Default Values

### Simple Defaults

```javascript
function createUser(name, age = 18, role = "user") {
  return { name, age, role };
}

createUser("Alice");        // { name: "Alice", age: 18, role: "user" }
createUser("Bob", 25);      // { name: "Bob", age: 25, role: "user" }
createUser("Charlie", 30, "admin"); // { name: "Charlie", age: 30, role: "admin" }
```

### Expressions as Defaults

```javascript
function getDiscount(price, discount = price * 0.1) {
  return price - discount;
}

getDiscount(100);     // 90 (10% discount applied)
getDiscount(100, 20); // 80 (custom discount)

// Function calls as defaults
function formatDate(date = new Date()) {
  return date.toLocaleDateString();
}

formatDate(); // Today's date
```

### Destructuring with Defaults

```javascript
// Object destructuring defaults
function createUser({
  name = "Anonymous",
  age = 0,
  email = null,
} = {}) {
  return { name, age, email };
}

createUser({ name: "Alice" }); // { name: "Alice", age: 0, email: null }
createUser();                  // { name: "Anonymous", age: 0, email: null }

// Array destructuring defaults
function getCoordinates([x = 0, y = 0] = []) {
  return { x, y };
}

getCoordinates([5, 10]); // { x: 5, y: 10 }
getCoordinates([5]);     // { x: 5, y: 0 }
getCoordinates();        // { x: 0, y: 0 }
```

## Practical Examples

### API Configuration

```javascript
function fetchUsers({
  baseUrl = "https://api.example.com",
  endpoint = "/users",
  method = "GET",
  headers = {},
  timeout = 5000,
} = {}) {
  return fetch(`${baseUrl}${endpoint}`, {
    method,
    headers: {
      "Content-Type": "application/json",
      ...headers,
    },
    signal: AbortSignal.timeout(timeout),
  });
}

// Use with any combination of options
fetchUsers({ endpoint: "/users/1", method: "DELETE" });
```

### Event Handler

```javascript
function handleClick({
  message = "No message",
  type = "info",
  duration = 3000,
  onClose = () => {},
} = {}) {
  const toast = showToast(message, { type, duration });
  toast.on("close", onClose);
}
```

### Math Utilities

```javascript
function clamp(value, min = 0, max = 100) {
  return Math.min(Math.max(value, min), max);
}

clamp(150);      // 100
clamp(-10);      // 0
clamp(50, 10, 90); // 50

function lerp(start, end, t = 0.5) {
  return start + (end - start) * t;
}

lerp(0, 100);     // 50
lerp(0, 100, 0.3); // 30
```

### Factory Functions

```javascript
function createLogger({
  prefix = "[LOG]",
  level = "info",
  timestamp = true,
} = {}) {
  return (message) => {
    const time = timestamp ? new Date().toISOString() : "";
    const log = `${prefix} ${time} [${level}] ${message}`;
    console.log(log);
  };
}

const logger = createLogger({ prefix: "[APP]" });
logger("Server started"); // "[APP] 2024-01-15... [info] Server started"
```

### React Component Props

```javascript
function Button({
  children = "Click me",
  variant = "primary",
  size = "medium",
  disabled = false,
  onClick = () => {},
}) {
  return (
    <button
      className={`btn btn-${variant} btn-${size}`}
      disabled={disabled}
      onClick={onClick}
    >
      {children}
    </button>
  );
}

// Usage - only pass what you need
<Button variant="danger" onClick={handleDelete} />
```

## Comparison with Old Patterns

```javascript
// Old way (ES5)
function greet(name) {
  name = name || "Guest";
  return `Hello, ${name}!`;
}

// Problem: falsy values are overwritten
greet(""); // "Hello, Guest!" (wrong!)
greet(0);  // "Hello, Guest!" (wrong!)

// New way (ES6 default parameters)
function greet(name = "Guest") {
  return `Hello, ${name}!`;
}

greet(""); // "Hello, !" (correct!)
greet(0);  // "Hello, 0!" (correct!)
```

## Default Parameter Order

```javascript
// Defaults can reference earlier parameters
function createUser(name, role = `${name}-role`) {
  return { name, role };
}

createUser("Alice"); // { name: "Alice", role: "Alice-role" }

// But not later parameters (TDZ applies)
// function fn(a = b, b = 1) {} // ReferenceError
```

## Common Use Cases

- **Configuration objects** with sensible defaults
- **API functions** with default endpoints, headers, timeouts
- **UI components** with default props
- **Utility functions** with default values
- **Optional function parameters** without manual checks

## Common Mistakes

| Mistake | Fix |
|---------|-----|
| Using `\|\|` for defaults (loses falsy values) | Use default parameters |
| Forgetting `null` passes the default | `null` is not `undefined` |
| Overriding all defaults unnecessarily | Only pass non-default values |
| Circular default references | Don't reference later parameters |

## Quick Revision

- Default parameters initialize arguments when `undefined` or omitted
- `null` does **not** trigger defaults (only `undefined`)
- Can use expressions, function calls, and earlier parameters as defaults
- Destructuring parameters can have defaults too
- Pass `{}` as default for optional object parameters
- Cleaner than `\|\|` operator (preserves falsy values like `0`, `""`)
- Parameter order matters: defaults can reference earlier params only

## Related Topics

- [[Destructuring]]
- [[Spread-Operator]]
- [[Rest-Parameters]]
- [[Functions]]
- [[ES6-Features]]
- [[Optional-Chaining]]
