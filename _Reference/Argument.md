# Arguments

Arguments in JavaScript refer to the values passed to functions when they are called. Understanding how arguments work is essential for writing effective functions.

## Definition

Arguments are the actual values supplied to a function when it is invoked. JavaScript provides multiple ways to handle arguments: named parameters, the `arguments` object, rest parameters, and default parameters.

## The `arguments` Object

```javascript
// The arguments object is an array-like object available in functions
function greet() {
    console.log(arguments); // Arguments object
    console.log(arguments.length);
    console.log(arguments[0]);
}

greet('Hello', 'World'); // Arguments: ['Hello', 'World']

// Converting arguments to array
function sum() {
    const args = Array.from(arguments);
    return args.reduce((total, num) => total + num, 0);
}

sum(1, 2, 3); // 6

// Using spread operator to convert
function logAll() {
    const args = [...arguments];
    args.forEach(arg => console.log(arg));
}
```

## Named Parameters

```javascript
// Explicitly named parameters (preferred approach)
function greet(name, greeting = 'Hello') {
    return `${greeting}, ${name}!`;
}

greet('Alice'); // "Hello, Alice!"
greet('Bob', 'Hi'); // "Hi, Bob!"

// Destructuring parameters
function createUser({ name, age, email }) {
    return { name, age, email };
}

createUser({ name: 'John', age: 30, email: 'john@example.com' });
```

## Default Parameters

```javascript
// Default parameter values
function multiply(a, b = 1) {
    return a * b;
}

multiply(5); // 5
multiply(5, 3); // 15

// Default parameters can reference other parameters
function createUser(name, role = 'user') {
    return { name, role };
}

// Default can be expressions
function greet(name, time = new Date().getHours()) {
    const period = time < 12 ? 'morning' : 'afternoon';
    return `Good ${period}, ${name}`;
}
```

## Rest Parameters

```javascript
// Collect remaining arguments into an array
function sum(...numbers) {
    return numbers.reduce((total, num) => total + num, 0);
}

sum(1, 2, 3, 4); // 10

// Rest parameters must be last
function log(first, ...rest) {
    console.log('First:', first);
    console.log('Rest:', rest);
}

log('a', 'b', 'c', 'd');
// First: a
// Rest: ['b', 'c', 'd']

// Combining with other parameters
function multiply(factor, ...numbers) {
    return numbers.map(num => num * factor);
}

multiply(2, 1, 2, 3); // [2, 4, 6]
```

## Spread Operator (Arguments)

```javascript
// Pass array elements as arguments
function sum(a, b, c) {
    return a + b + c;
}

const numbers = [1, 2, 3];
sum(...numbers); // 6

// Concatenating arrays
const arr1 = [1, 2];
const arr2 = [3, 4];
const combined = [...arr1, ...arr2]; // [1, 2, 3, 4]
```

## Common Use Cases

- Creating flexible functions with optional parameters
- Building utility functions that accept variable arguments
- Implementing function wrappers and decorators
- Creating middleware systems
- Building APIs with optional configuration

## Common Mistakes

1. **Using `arguments` with arrow functions** - Arrow functions don't have `arguments` object
2. **Rest parameters must be last** - Cannot have parameters after rest
3. **Not handling undefined** - Default parameters help avoid this
4. **Mutating arguments object** - `arguments` is array-like but not an array
5. **Forgetting arguments are by value** - Objects/arrays are passed by reference

## Related Topics

- [[Function Parameters]]
- [[Rest Parameters]]
- [[Spread Operator]]
- [[Default Parameters]]
- [[Destructuring]]

## Quick Revision

| Concept | Example |
|---------|---------|
| arguments object | `function f() { return arguments; }` |
| Default params | `function f(a = 10) {}` |
| Rest params | `function f(...args) {}` |
| Spread operator | `f(...array)` |
| Named params | `function f({ name, age }) {}` |
