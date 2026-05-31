# Function Parameters

## Definition

Parameters are **variables in function definition** that receive values.

## Basic Syntax

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

greet(); // "Hello, Guest! You are 0."
```

## Quick Revision

- Parameters: variables in definition
- Arguments: values passed
- Default parameters: `param = value`
- Rest parameters: `...param`

---

## Related Topics

- [[What-is-Parameter]] - [[What-is-Parameter|Parameters]]
- [[Function-Parameters]] - [[Function-Parameters|Function parameters]]
- [[Function Parameters]] - [[Function Parameters|Function parameters]]
- [[Default-Parameters]] - [[Default-Parameters|Default parameters]]
