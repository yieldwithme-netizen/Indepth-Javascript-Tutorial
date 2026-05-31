# What is Partial Application

Partial application is the process of fixing a subset of a function's arguments, producing a new function that takes the remaining arguments.

## Definition

Unlike currying (which returns a chain of single-argument functions), partial application fixes **one or more** arguments and returns a function that accepts the rest.

```javascript
// Partial application
function add(a, b, c) {
  return a + b + c;
}

const add5 = add.bind(null, 5);  // Fixes first argument
add5(10, 20); // 35
```

## Partial Application vs Currying

```javascript
// Currying: f(a, b, c) -> f(a)(b)(c)
const curry = a => b => c => a + b + c;

// Partial Application: f(a, b, c) -> f(_, b, c)
const partial = (a, b, c) => a + b + c;
const add10 = partial.bind(null, 10);
add10(20, 30); // 60
```

## Using .bind() for Partial Application

```javascript
function greet(greeting, name) {
  return `${greeting}, ${name}!`;
}

const sayHello = greet.bind(null, "Hello");
const sayHi = greet.bind(null, "Hi");

sayHello("Alice"); // "Hello, Alice!"
sayHi("Bob");      // "Hi, Bob!"
```

## Manual Partial Application

```javascript
function partial(fn, ...presetArgs) {
  return function(...laterArgs) {
    return fn(...presetArgs, ...laterArgs);
  };
}

function multiply(a, b, c) {
  return a * b * c;
}

const doubleAndTriple = partial(multiply, 2, 3);
doubleAndTriple(5); // 30 (2 * 3 * 5)
```

## Practical Examples

### Event Handler
```javascript
function handleEvent(eventName, element, event) {
  console.log(`${eventName} on ${element}`, event);
}

const handleClick = partial(handleEvent, "click", "button");
handleClick({ target: "btn1" });
```

### API Request Builder
```javascript
function fetchData(method, url, data) {
  return fetch(url, { method, body: JSON.stringify(data) });
}

const post = partial(fetchData, "POST");
post("/api/users", { name: "Alice" });
```

### Math Utilities
```javascript
const log = (base, value) => Math.log(value) / Math.log(base);

const log2 = partial(log, 2);
const log10 = partial(log, 10);

log2(8);   // 3
log10(100); // 2
```

## Common Mistakes

- Confusing partial application with currying
- Overusing `.bind()` when arrow functions are clearer
- Partially applying arguments in the wrong order

## Quick Revision

- Partial application fixes some arguments, returns function for the rest
- Use `.bind(null, fixedArg)` for quick partial application
- More flexible than currying when you want to fix multiple arguments at once
- Great for creating specialized functions from general ones

## Related Topics

- [[What-is-Currying]]
- [[Curry-Functions]]
- [[What-is-Composition]]
- [[Function-Scope-and-Closures]]
