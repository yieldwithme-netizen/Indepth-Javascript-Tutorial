# Rest Parameters

## Definition

Rest parameters allow a function to accept an indefinite number of arguments as an array. Instead of using the `arguments` object, rest parameters provide a cleaner, more predictable way to work with variable-length argument lists. The rest parameter must be the last parameter in the function signature and is prefixed with `...`.

## Syntax

```javascript
function functionName(...restParam) {
  // use restParam as an array
}
```

## Code Examples

### Basic Usage

```javascript
function sum(...numbers) {
  return numbers.reduce((total, num) => total + num, 0);
}

console.log(sum(1, 2, 3));        // 6
console.log(sum(1, 2, 3, 4, 5)); // 15
console.log(sum());               // 0
```

### Rest Parameters with Regular Parameters

```javascript
function greet(greeting, ...names) {
  return names.map(name => `${greeting}, ${name}!`);
}

console.log(greet("Hello", "Alice", "Bob", "Charlie"));
// ["Hello, Alice!", "Hello, Bob!", "Hello, Charlie!"]
```

### Rest Parameters in Arrow Functions

```javascript
const multiply = (...numbers) => {
  return numbers.reduce((product, num) => product * num, 1);
};

console.log(multiply(2, 3, 4));   // 24
console.log(multiply(5, 10));     // 50
```

### Object Destructuring with Rest Parameters

```javascript
function createUser(name, age, ...hobbies) {
  return {
    name,
    age,
    hobbies
  };
}

const user = createUser("John", 25, "reading", "gaming", "hiking");
console.log(user);
// { name: "John", age: 25, hobbies: ["reading", "gaming", "hiking"] }
```

### Spread vs Rest

```javascript
// Spread operator: expands an array into individual elements
const numbers = [1, 2, 3];
const expanded = [...numbers, 4, 5]; // [1, 2, 3, 4, 5]

// Rest parameter: collects individual elements into an array
function collect(...items) {
  return items;
}
const collected = collect(1, 2, 3); // [1, 2, 3]
```

### Practical Example: Flexible Logging

```javascript
function log(level, ...messages) {
  const timestamp = new Date().toISOString();
  console.log(`[${timestamp}] [${level}]`, ...messages);
}

log("INFO", "Server started on port", 3000);
log("ERROR", "Failed to connect:", "timeout exceeded");
```

## Common Use Cases

1. **Variadic functions** - Functions that accept any number of arguments
2. **Rest parameters in destructuring** - Extracting remaining properties
3. **Event handlers** - Processing variable numbers of parameters
4. **Mathematical operations** - Functions like sum, average, min, max
5. **Function forwarding** - Passing arguments to other functions

## Common Mistakes

1. **Rest parameter must be last** - It cannot be followed by other parameters
2. **Only one rest parameter allowed** - You cannot have multiple rest parameters
3. **Confusing spread with rest** - Spread expands, rest collects
4. **Not an array-like object** - Rest parameters create a true array, not an arguments object

```javascript
// WRONG: Rest parameter not last
function wrong(a, ...rest, b) {} // SyntaxError

// RIGHT: Rest parameter last
function correct(a, b, ...rest) {} // Works
```

## Quick Revision Summary

- Rest parameters collect unlimited arguments into a true array
- Use `...paramName` syntax as the last parameter
- Returns a real array with all array methods available
- More predictable than the `arguments` object
- Useful for variadic functions and function forwarding

## Related Topics

- [[Spread-Operator]]
- [[Arrow-Functions]]
- [[Destructuring-Assignment]]
- [[Functions]]
- [[ES6-Features]]
- [[Array-Methods]]
