# What is a [[Function]]?

## Definition

A [[Function]] is a **reusable block of code** that performs a specific task.

## Creating [[Function|Functions]]

### [[Function]] Declaration

```javascript
function greet(name) {
    return `Hello, ${name}!`;
}

greet("John"); // "Hello, John!"
```

### [[Function]] [[Expression]]

```javascript
const greet = function(name) {
    return `Hello, ${name}!`;
};
```

### [[Arrow]] [[Function]] (ES6)

```javascript
const greet = (name) => `Hello, ${name}!`;
```

## [[Function]] Parts

```javascript
// Parameters vs Arguments
function add(a, b) {  // a, b are PARAMETERS
    return a + b;
}
add(5, 3);  // 5, 3 are ARGUMENTS
```

## [[Function]] hoisting

```javascript
// Function declarations are hoisted
greet(); // Works! "Hello"

function greet() {
    console.log("Hello");
}

// Function expressions are NOT hoisted
sayHi(); // Error!

const sayHi = function() {
    console.log("Hi");
};
```

## Quick Revision

- [[Function]] = reusable code block
- Declaration: `function name() {}`
- Expression: `const name = function() {}`
- [[Arrow]]: `const name = () => {}`
- [[Parameter|Parameters]] = defined, [[Argument|Arguments]] = passed

---

## Related Topics

- [[Declare-Function]] - Declaring functions
- [[What-is-Parameter]] - Parameters
- [[What-is-Return]] - Return values
- [[What-is-Arrow]] - Arrow functions
- [[What-is-Scope]] - Scope