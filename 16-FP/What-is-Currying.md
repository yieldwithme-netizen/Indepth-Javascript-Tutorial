# What is Currying

Currying is the technique of transforming a function with multiple arguments into a **sequence of functions**, each taking a single argument.

## Definition

Instead of `f(a, b, c)`, currying gives you `f(a)(b)(c)` — each call returns a new function until all arguments are provided.

```javascript
// Normal function
function add(a, b) {
  return a + b;
}
add(2, 3); // 5

// Curried version
function curriedAdd(a) {
  return function(b) {
    return a + b;
  };
}
curriedAdd(2)(3); // 5
```

## How It Works

```javascript
// Curried function
function multiply(a) {
  return function(b) {
    return a * b;
  };
}

const double = multiply(2);   // Returns a function
const triple = multiply(3);   // Returns a different function

double(5);  // 10
triple(5);  // 15
```

## Arrow Function Syntax

```javascript
const add = a => b => a + b;

add(10)(20); // 30

// More examples
const greet = greeting => name => `${greeting}, ${name}!`;
greet("Hello")("Alice"); // "Hello, Alice!"
```

## Why Currying is Useful

### Function Specialization
```javascript
const log = level => message => console.log(`[${level}] ${message}`);

const error = log("ERROR");
const info = log("INFO");

error("Something broke"); // [ERROR] Something broke
info("System started");   // [INFO] System started
```

### Callback Factories
```javascript
const fetchFrom = url => callback => {
  fetch(url).then(res => res.json()).then(callback);
};

const fetchUsers = fetchFrom("/api/users");
fetchUsers(data => console.log(data));
```

### Partial Application (preview)
```javascript
const add = a => b => a + b;
const add10 = add(10);
[1, 2, 3].map(add10); // [11, 12, 13]
```

## Common Mistakes

- Confusing currying with partial application
- Forgetting that curried functions need all arguments to produce a result
- Over-currying making code harder to read

## Quick Revision

- Currying transforms `f(a, b, c)` into `f(a)(b)(c)`
- Each call returns a new function expecting the next argument
- Useful for creating specialized functions from general ones
- Works naturally with closures

## Related Topics

- [[Curry-Functions]]
- [[What-is-Partial]]
- [[What-is-Composition]]
- [[What-is-Immutability]]
- [[Closures-and-Scope]]
