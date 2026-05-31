# Rest Parameters Specification

## Definition

Rest parameters allow a function to accept an indefinite number of arguments as an array. They provide a cleaner alternative to the `arguments` object and enable more flexible function signatures.

## Syntax

```javascript
function functionName(...restParam) {
  // restParam is an array
}
```

## Code Examples

### Basic Rest Parameters

```javascript
function sum(...numbers) {
  return numbers.reduce((total, num) => total + num, 0);
}

console.log(sum(1, 2, 3));          // 6
console.log(sum(1, 2, 3, 4, 5));   // 15
console.log(sum());                 // 0
```

### Rest Parameters with Regular Parameters

```javascript
function greet(greeting, ...names) {
  return names.map(name => `${greeting}, ${name}!`);
}

console.log(greet("Hello", "Alice", "Bob"));
// ["Hello, Alice!", "Hello, Bob!"]
```

### Destructuring with Rest Parameters

```javascript
function processUser({ name, email, ...rest }) {
  console.log(`Name: ${name}`);
  console.log(`Email: ${email}`);
  console.log(`Other data:`, rest);
}

processUser({
  name: "John",
  email: "john@example.com",
  age: 30,
  role: "admin"
});
// Name: John
// Email: john@example.com
// Other data: { age: 30, role: "admin" }
```

### Spread vs Rest

```javascript
// SPREAD - expands an array into individual elements
const numbers = [1, 2, 3];
console.log(Math.max(...numbers)); // 3

// REST - collects individual elements into an array
function collect(...args) {
  return args;
}
console.log(collect(1, 2, 3)); // [1, 2, 3]
```

### Arrow Functions with Rest

```javascript
const multiply = (...numbers) => {
  return numbers.reduce((product, num) => product * num, 1);
};

console.log(multiply(2, 3, 4)); // 24
```

### Real-World Examples

```javascript
// Logging utility
function log(level, ...messages) {
  const timestamp = new Date().toISOString();
  console.log(`[${timestamp}] [${level}]`, ...messages);
}

log("INFO", "Server started", "Port:", 3000);

// Array manipulation
function removeItems(array, ...indices) {
  return array.filter((_, index) => !indices.includes(index));
}

console.log(removeItems([10, 20, 30, 40, 50], 1, 3));
// [10, 30, 50]

// Function composition
function pipe(...functions) {
  return (value) => {
    return functions.reduce((result, fn) => fn(result), value);
  };
}

const transform = pipe(
  x => x + 1,
  x => x * 2,
  x => x - 3
);

console.log(transform(5)); // ((5+1)*2)-3 = 9
```

### Rest Parameters vs Arguments Object

```javascript
// Old way - arguments object (array-like, no array methods)
function oldSum() {
  let total = 0;
  for (let i = 0; i < arguments.length; i++) {
    total += arguments[i];
  }
  return total;
}

// Modern way - rest parameters (true array)
function newSum(...numbers) {
  return numbers.reduce((total, num) => total + num, 0);
}

// Arrow functions don't have arguments object
const arrowSum = (...numbers) => numbers.reduce((a, b) => a + b, 0);
```

## Common Use Cases

- Variadic functions (functions accepting any number of arguments)
- Collecting remaining arguments
- Implementing flexible APIs
- Forwarding arguments to other functions
- Function composition and piping

## Common Mistakes

1. **Rest parameter must be last**: It must be the final parameter

```javascript
// Wrong
function wrong(...args, last) {}

// Correct
function correct(first, ...rest) {}
```

2. **Only one rest parameter per function**: You can't have multiple rest parameters

```javascript
// Wrong
function wrong(...args1, ...args2) {}
```

3. **Rest parameters vs spread confusion**: Remember - rest collects, spread expands

```javascript
// Rest - in function definition
function collect(...items) {}

// Spread - in function call
const arr = [1, 2, 3];
collect(...arr);
```

4. **Empty rest parameter**: Returns empty array, not undefined

```javascript
function test(...args) {
  console.log(args); // [] if no arguments
}
```

## Related Topics

- [[Spread-Operator]] - Spread syntax
- [[Default-Parameters]] - Default parameter values
- [[Destructuring]] - Destructuring assignment
- [[Arrow-Functions]] - Arrow function syntax
- [[Function-Parameters]] - Parameter handling
- [[Arguments-Object]] - Legacy arguments object

## Quick Revision Summary

| Feature | Description |
|---------|-------------|
| Syntax | `function fn(...param) {}` |
| Type | Always an array |
| Position | Must be last parameter |
| Count | Only one rest parameter allowed |
| Empty | Returns empty array `[]` |
| Difference from spread | Rest collects, spread expands |
| Replaces | `arguments` object |
