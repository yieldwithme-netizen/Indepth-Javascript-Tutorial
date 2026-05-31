# Function Parameters

## Definition

Parameters are **variables in function definition** that receive values when called.

## Basic Parameters

```javascript
function greet(name, age) {
    return `Hello, ${name}! You are ${age}.`;
}

greet("John", 30); // "Hello, John! You are 30."
```

## Default Parameters

```javascript
function greet(name = "Guest", age = 0) {
    return `Hello, ${name}! You are ${age}.`;
}

greet();           // "Hello, Guest! You are 0."
greet("John");     // "Hello, John! You are 0."
greet("John", 30); // "Hello, John! You are 30."
```

## Rest Parameters

```javascript
function sum(...numbers) {
    return numbers.reduce((total, num) => total + num, 0);
}

sum(1, 2, 3);    // 6
sum(1, 2, 3, 4); // 10
```

## Destructuring Parameters

```javascript
// Object destructuring
function greet({ name, age }) {
    return `Hello, ${name}! You are ${age}.`;
}

greet({ name: "John", age: 30 });

// Array destructuring
function first([first, ...rest]) {
    return first;
}

first([1, 2, 3]); // 1
```

## Quick Revision

- Parameters = variables in function definition
- Arguments = values passed when calling
- Default parameters: `param = value`
- Rest parameters: `...param` (array)
- Destructuring: extract from objects/arrays

---

## Related Topics

- [[What-is-Parameter]] - [[What-is-Parameter|Parameters]] overview
- [[Use-Parameters]] - [[Use-Parameters|Using parameters]]
- [[Default-Parameters]] - [[Default-Parameters|Default parameters]]
- [[What-is-RestParam]] - [[What-is-RestParam|Rest parameters]]
