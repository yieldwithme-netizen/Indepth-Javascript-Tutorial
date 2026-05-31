# What is a Function [[Parameter]]?

## Definition

[[Parameter|Parameters]] are **[[Variable|variables]] listed in [[Function]] definition** that receive values ([[Argument|arguments]]) when the [[Function]] is called.

## Basic [[Parameter|Parameters]]

```javascript
function greet(name, age) {
    return `Hello, ${name}! You are ${age}.`;
}

greet("John", 30); // "Hello, John! You are 30."
```

## Default [[Parameter|Parameters]]

```javascript
function greet(name = "Guest", age = 0) {
    return `Hello, ${name}! You are ${age}.`;
}

greet();           // "Hello, Guest! You are 0."
greet("John");     // "Hello, John! You are 0."
greet("John", 30); // "Hello, John! You are 30."
```

## Rest [[Parameter|Parameters]]

```javascript
function sum(...numbers) {
    return numbers.reduce((total, num) => total + num, 0);
}

sum(1, 2, 3);    // 6
sum(1, 2, 3, 4); // 10
```

## Destructuring [[Parameter|Parameters]]

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

- [[Parameter|Parameters]] = [[Variable|variables]] in [[Function]] definition
- [[Argument|Arguments]] = values passed when calling
- Default [[Parameter|parameters]]: `param = value`
- Rest [[Parameter|parameters]]: `...param` ([[Array]] of args)
- Destructuring: extract values from [[Object|objects]]/[[Array|arrays]]

---

## Related Topics

- [[What-is-Function]] - Functions
- [[Use-Parameters]] - Using parameters
- [[What-is-RestParam]] - Rest parameters
- [[Set-Defaults]] - Default parameters
- [[Declare-Function]] - Declaring functions